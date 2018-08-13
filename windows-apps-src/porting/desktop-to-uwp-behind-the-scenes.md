---
author: normesta
Description: Este artigo se aprofunda no funcionamento da Ponte de Desktop nos bastidores.
title: Nos bastidores da Ponte de Desktop
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.openlocfilehash: 050499baaf383fc135d833ae1e4733c95f2b5fa1
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2017
ms.locfileid: "617351"
---
# <a name="behind-the-scenes-of-the-desktop-bridge"></a>Nos bastidores da Ponte de Desktop

Este artigo se aprofunda no funcionamento da Ponte de Desktop nos bastidores.

Uma das metas principais da Ponte de Desktop é separar o estado do aplicativo do estado do sistema o máximo possível, mantendo a compatibilidade com outros aplicativos. A ponte faz isso colocando o aplicativo dentro de um pacote da Plataforma Universal do Windows (UWP) e, em seguida, detectando e redirecionando algumas alterações feitas no sistema de arquivos e no Registro em tempo de execução.

Os pacotes criados para seu app da área de trabalho são aplicativos apenas de área de trabalho, totalmente confiáveis e que não são virtualizados nem estão em área restrita. Isso permite que eles interajam com outros aplicativos da mesma maneira que aplicativos da área de trabalho clássicos.

## <a name="installation"></a>Instalação

Os pacotes de aplicativos são instalados em *C:\Program Files\WindowsApps\package_name*, com o executável intitulado *app_name.exe*. Cada pasta de pacote contém um manifesto (chamado AppxManifest.xml) que contém um namespace XML especial para apps empacotados. Dentro desse arquivo de manifesto está um elemento ```<EntryPoint>```, que faz referência ao aplicativo de confiança total. Quando é iniciado, esse aplicativo não é executado dentro de um contêiner de aplicativo, e sim executado como o usuário normalmente faria.

Depois da implantação, os arquivos do pacote serão marcados como somente leitura e totalmente bloqueados pelo sistema operacional. O Windows evitará a inicialização dos aplicativos se esses arquivos forem adulterados.

## <a name="file-system"></a>Sistema de arquivos

Para conter o estado do aplicativo, a ponte tenta capturar as alterações que o aplicativo faz em AppData. Todas as gravações feitas na pasta AppData do usuário (por exemplo, *C:\Users\user_name\AppData*), inclusive criar, excluir e atualizar, são copiadas na gravação para um local privado por usuário e por aplicativo. Isso gera a ilusão de que o app empacotado está editando a AppData real quando está, na verdade, modificando uma cópia particular. Redirecionando gravações dessa maneira, o sistema pode acompanhar todas as modificações de arquivo feitas pelo aplicativo. Isso permitirá que o sistema limpe esses arquivos quando o aplicativo for desinstalado, o que reduz o "rot" do sistema e proporciona uma experiência de remoção de aplicativo melhor para o usuário.

Além de redirecionar AppData, a ponte também mescla dinamicamente pastas conhecidas do Windows (System32, Arquivos de Programas (x86) etc.) a diretórios correspondentes no pacote do aplicativo. Cada pacote contém uma pasta chamada "VFS" na raiz. Todas as leituras de diretório ou arquivo no diretório VFS são mescladas em tempo de execução às respectivas contrapartes nativas. Por exemplo, um aplicativo pode conter *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* como parte do pacote do aplicativo, mas o arquivo estaria aparentemente instalado em *C:\Windows\System32\vc10.dll*.  Isso mantém a compatibilidade com aplicativos da área de trabalho, que podem esperar que arquivos estejam em locais sem pacote.

As gravações em arquivos/pastas no pacote de apps não são permitidas. As gravações em arquivos e pastas que não fazem parte do pacote são ignoradas pela ponte e são permitidas desde que o usuário tenha permissão.

### <a name="common-operations"></a>Operações comuns

Esta tabela de referência curta mostra operações comuns do sistema de arquivos e como a ponte as manipula.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar um arquivo ou uma pasta do Windows conhecida | Uma mescla dinâmica de *C:\Program Files\package_name\VFS\well_known_folder* à contraparte do sistema local. | A leitura de *C:\Windows\System32* retorna o conteúdo de *C:\Windows\System32* mais o conteúdo de *C:\Program Files\WindowsApps\package_name\VFS\SystemX86*.
Gravar em AppData | Copiar gravação por usuário, local de cada aplicativo. | AppData costuma ser *C:\Users\user_name\AppData*.  
Gravar no pacote | Não permitido. O pacote é somente leitura. | Gravações em *C:\Program Files\WindowsApps\package_name* não são permitidas.
Gravações fora do pacote | Ignorado pela ponte. Permitido se o usuário tiver permissões. | Uma gravação em *C:\Windows\System32\foo.dll* será permitida se o pacote não contiver *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll* e o usuário tiver permissões.

### <a name="packaged-vfs-locations"></a>Locais dos pacotes VFS

A tabela a seguir mostra onde os arquivos fornecidos como parte do pacote são sobrepostos no sistema para o aplicativo. O aplicativo perceberá que esses arquivos estão em locais do sistema listado quando, na verdade, estão nos locais redirecionados dentro de *C:\Program Files\WindowsApps\package_name\VFS*. Os locais de FOLDERID são das constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

Local do sistema | Local redirecionado (em [PackageRoot]\VFS\) | Válido em arquiteturas
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | AppData comum | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Registro

A ponte manipula o Registro de maneira semelhante ao sistema de arquivos. Os pacotes de apps contêm um arquivo registry.dat, que funciona como o equivalente lógico de *HKLM\Software* no Registro real. Em tempo de execução, esse Registro virtual mescla o conteúdo desse hive ao hive do sistema nativo para oferecer uma visão singular de ambos. Por exemplo, se registry.dat contiver uma única chave "Foo", uma leitura de *HKLM\Software* em tempo de execução também conterá aparentemente "Foo" (além de todas as chaves do sistema nativo).

Somente as chaves em *HKLM\Software* fazem parte do pacote; as chaves em *HKCU* ou outras partes do Registro não fazem parte. Gravações em chaves ou valores no pacote não são permitidas. As gravações em chaves ou valores que não fazem parte do pacote são ignoradas pela ponte e permitidas desde que o usuário tenha permissão.

Todas as gravações em HKCU são cópias em gravações para um local particular por usuário e por aplicativo. Isso oferece os mesmos benefícios do manuseio do sistema de arquivos pela ponte em relação à limpeza de desinstalação. Tradicionalmente, os desinstaladores são conseguem limpar *HKEY_CURRENT_USER* porque os dados do Registro para usuários desconectados estão desmontados e não estão disponíveis.

Todas as gravações são mantidas durante a atualização do pacote e só são excluídas quando o aplicativo é totalmente removido.

### <a name="common-operations"></a>Operações comuns

Esta tabela de referência curta mostra operações comuns do Registro e como a ponte as manipula.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar *HKLM\Software* | Uma mesclagem dinâmica do hive do pacote à contraparte do sistema local. | Se registry.dat contiver uma única chave "Foo", em tempo de execução, uma leitura de *HKLM\Software* mostrará o conteúdo de *HKLM\Software* mais *HKLM\Software\Foo*.
Gravações em HKCU | Copiar gravação por usuário, local particular de cada aplicativo. | Igual a AppData para arquivos.
Grava no pacote. | Não permitido. O pacote é somente leitura. | As gravações em *HKLM\Software* não serão permitidas, se um par chave/valor correspondente existir no hive do pacote.
Gravações fora do pacote | Ignorado pela ponte. Permitido se o usuário tiver permissões. | As gravações em *HKLM\Software* são permitidas desde que um par chave/valor correspondente não exista no hive do pacote e o usuário tenha permissões de acesso corretas.

## <a name="uninstallation"></a>Desinstalação

Quando um pacote é desinstalado pelo usuário, todos os arquivos e pastas localizados em *C:\Program Files\WindowsApps\package_name* são removidos, bem como todas as gravações redirecionadas para AppData ou do Registro que foram capturadas pela ponte.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para perguntas específicas**

Nossa equipe monitora as [tags de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.
