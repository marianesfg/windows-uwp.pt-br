---
author: normesta
Description: This article provides a deeper dive on how the Desktop Bridge works under the covers.
title: Nos bastidores da Ponte de Desktop
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: 4e6cd2b305a9d52a2239be46cc7f77650cdd6531
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623178"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>Nos bastidores do seu aplicativo da área de trabalho empacotado

Este artigo fornece Aprofunde-se sobre o que acontece com arquivos e entradas do registro quando você cria um pacote de aplicativo do Windows para seu aplicativo da área de trabalho.

Das metas principais de um pacote moderno é separar o estado do aplicativo do sistema o máximo possível, mantendo a compatibilidade com outros aplicativos. A ponte faz isso colocando o aplicativo dentro de um pacote da Plataforma Universal do Windows (UWP) e, em seguida, detectando e redirecionando algumas alterações feitas no sistema de arquivos e no Registro em tempo de execução.

Os pacotes criados para seu aplicativo da área de trabalho são aplicativos apenas de área de trabalho, totalmente confiáveis e não são virtualizados nem estão em área restrita. Isso permite que eles interajam com outros aplicativos da mesma maneira que aplicativos da área de trabalho clássicos.

## <a name="installation"></a>Instalação

Os pacotes de aplicativos são instalados em *C:\Program Files\WindowsApps\package_name*, com o executável intitulado *app_name.exe*. Cada pasta de pacote contém um manifesto (chamado AppxManifest.xml) que contém um namespace XML especial para apps empacotados. Dentro desse arquivo de manifesto está um elemento ```<EntryPoint>```, que faz referência ao aplicativo de confiança total. Quando esse aplicativo é iniciado, ele não é executado dentro de um contêiner de aplicativo, mas em vez disso, ele é executado como o usuário normalmente faria.

Depois da implantação, os arquivos do pacote serão marcados como somente leitura e totalmente bloqueados pelo sistema operacional. O Windows evitará a inicialização dos aplicativos se esses arquivos forem adulterados.

## <a name="file-system"></a>Sistema de arquivos

Para conter o estado do aplicativo, as alterações que o aplicativo faz em AppData são capturadas. Todas as gravações feitas na pasta AppData do usuário (por exemplo, *C:\Users\user_name\AppData*), inclusive criar, excluir e atualizar, são copiadas na gravação para um local privado por usuário e por aplicativo. Isso cria a ilusão de que o aplicativo empacotado está editando a AppData real quando está, na verdade, modificando uma cópia particular. Redirecionando gravações dessa maneira, o sistema pode acompanhar todas as modificações de arquivo feitas pelo aplicativo. Isso permite que o sistema Limpe esses arquivos quando o aplicativo é desinstalado, o que reduz o "rot" do sistema e fornecendo uma remoção de aplicativo a melhor experiência para o usuário.

Além de redirecionar AppData, pastas conhecidas do Windows (System32, arquivos de programas (x86) etc.) são mescladas dinamicamente a diretórios correspondentes no pacote do aplicativo. Cada pacote contém uma pasta chamada "VFS" na raiz. Todas as leituras de diretório ou arquivo no diretório VFS são mescladas em tempo de execução às respectivas contrapartes nativas. Por exemplo, um aplicativo pode conter *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* como parte do pacote do aplicativo, mas o arquivo apareceria seja instalado em *C:\Windows\System32\vc10.dll*.  Isso mantém a compatibilidade com aplicativos da área de trabalho, que podem esperar que arquivos estejam em locais sem pacote.

As gravações em arquivos/pastas no pacote de apps não são permitidas. As gravações em arquivos e pastas que não fazem parte do pacote são ignoradas pela ponte e são permitidas desde que o usuário tenha permissão.

### <a name="common-operations"></a>Operações comuns

Esta tabela de referência curta mostra operações comuns do sistema de arquivos e como a ponte as manipula.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar um arquivo ou uma pasta do Windows conhecida | Uma mescla dinâmica de *C:\Program Files\package_name\VFS\well_known_folder* à contraparte do sistema local. | A leitura de *C:\Windows\System32* retorna o conteúdo de *C:\Windows\System32* mais o conteúdo de *C:\Program Files\WindowsApps\package_name\VFS\SystemX86*.
Gravar em AppData | Copiar gravação por usuário, local de cada aplicativo. | AppData costuma ser *C:\Users\user_name\AppData*.  
Gravar no pacote | Não permitido. O pacote é somente leitura. | Gravações em *C:\Program Files\WindowsApps\package_name* não são permitidas.
Gravações fora do pacote | Permitido se o usuário tiver permissões. | Uma gravação em *C:\Windows\System32\foo.dll* será permitida se o pacote não contiver *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll* e o usuário tiver permissões.

### <a name="packaged-vfs-locations"></a>Locais dos pacotes VFS

A tabela a seguir mostra onde os arquivos fornecidos como parte do pacote são sobrepostos no sistema para o aplicativo. Seu aplicativo perceberá que esses arquivos estão em locais do sistema listado quando, na verdade, estão nos locais redirecionados dentro *C:\Program Files\WindowsApps\package_name\VFS*. Os locais de FOLDERID são das constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

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

Os pacotes de apps contêm um arquivo registry.dat, que funciona como o equivalente lógico de *HKLM\Software* no Registro real. Em tempo de execução, esse Registro virtual mescla o conteúdo desse hive ao hive do sistema nativo para oferecer uma visão singular de ambos. Por exemplo, se registry.dat contiver uma única chave "Foo", uma leitura de *HKLM\Software* em tempo de execução também conterá aparentemente "Foo" (além de todas as chaves do sistema nativo).

Somente as chaves em *HKLM\Software* fazem parte do pacote; as chaves em *HKCU* ou outras partes do Registro não fazem parte. Gravações em chaves ou valores no pacote não são permitidas. Gravações em chaves ou valores não parte do pacote são permitidas desde que o usuário tenha permissão.

Todas as gravações em HKCU são cópias em gravações para um local particular por usuário e por aplicativo. Tradicionalmente, os desinstaladores são conseguem limpar *HKEY_CURRENT_USER* porque os dados do Registro para usuários desconectados estão desmontados e não estão disponíveis.

Todas as gravações são mantidas durante a atualização de pacote e só são excluídas quando o aplicativo é totalmente removido.

### <a name="common-operations"></a>Operações comuns

Esta tabela de referência curta mostra operações comuns do Registro e como a ponte as manipula.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar *HKLM\Software* | Uma mesclagem dinâmica do hive do pacote à contraparte do sistema local. | Se registry.dat contiver uma única chave "Foo", em tempo de execução, uma leitura de *HKLM\Software* mostrará o conteúdo de *HKLM\Software* mais *HKLM\Software\Foo*.
Gravações em HKCU | Copiar gravação por usuário, local particular de cada aplicativo. | Igual a AppData para arquivos.
Grava no pacote. | Não permitido. O pacote é somente leitura. | As gravações em *HKLM\Software* não serão permitidas, se um par chave/valor correspondente existir no hive do pacote.
Gravações fora do pacote | Ignorado pela ponte. Permitido se o usuário tiver permissões. | As gravações em *HKLM\Software* são permitidas desde que um par chave/valor correspondente não exista no hive do pacote e o usuário tenha permissões de acesso corretas.

## <a name="uninstallation"></a>Desinstalação

Quando um pacote é desinstalado pelo usuário, todos os arquivos e pastas localizadas em *C:\Program Files\WindowsApps\package_name* são removidas, bem como todas as gravações redirecionadas para AppData ou do registro que foram capturadas durante o processo de empacotamento.

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
