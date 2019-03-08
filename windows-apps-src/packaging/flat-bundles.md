---
title: Pacotes do lote simples de aplicativo
description: Descreve como criar um lote simples de pacote para empacotar seus arquivos de pacote .appx com referências aos pacotes de aplicativos.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, empacotamento, configuração do pacote, lote simples
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638481"
---
# <a name="flat-bundle-app-packages"></a>Pacotes do lote simples de aplicativo 

> [!IMPORTANT]
> Se você pretender enviar seu aplicativo para a Store, precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter aprovação para usar lotes simples.

Pacotes simples são uma maneira melhor de empacotamento de arquivos de pacote do aplicativo. Um Windows típico arquivo de pacote de aplicativo usa uma estrutura de empacotamento de vários níveis em que os arquivos de pacote do aplicativo precisam estar dentro do pacote, pacotes simples remover essa necessidade, fazendo referência a apenas os arquivos de pacote de aplicativo, permitindo que eles sejam fora do pacote de aplicativo. Uma vez que os arquivos de pacote de aplicativo não estão mais contidos no pacote, eles podem ser paralelos processado, que resulta em redução de carregamento de publicação de tempo, o mais rápida (já que cada arquivo de pacote do aplicativo pode ser processado ao mesmo tempo) e, por fim, mais rápido desenvolvimento iterações.

![Diagrama de lote simples](images/bundle-combined.png)

Outra vantagem de lotes simples é a necessidade de criar menos pacotes. Uma vez que os arquivos de pacote de aplicativo só são mencionados, duas versões do aplicativo podem referenciar o mesmo arquivo de pacote se o pacote não foi alterada entre as duas versões. Assim, você precisa criar apenas os pacotes de aplicativo que foram alterados ao compilar os pacotes para a próxima versão do seu aplicativo.
Por padrão, os pacotes simples fará referência a arquivos de pacote de aplicativo dentro da mesma pasta por si só. No entanto, essa referência pode ser alterada para outros caminhos (caminhos relativos, compartilhamentos de rede e locais http). Para fazer isso, você deve fornecer diretamente um **BundleManifest** durante a criação de lote simples. 

## <a name="how-to-create-a-flat-bundle"></a>Como criar um lote simples

Um lote simples pode ser criado usando a ferramenta MakeAppx.exe ou usando o layout de empacotamento para definir a estrutura do seu lote.

### <a name="using-makeappxexe"></a>Usando a MakeAppx.exe
Para criar um pacote simples usando MakeAppx.exe, use o comando "Pacote MakeAppx.exe" como de costume, mas com a opção /fb para gerar o arquivo de pacote de aplicativo simples (que será extremamente pequeno, pois ele só faz referência os arquivos de pacote do aplicativo e não contém qualquer cargas reais ). 

Eis um exemplo da sintaxe do comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

Para obter mais informações sobre como usar MakeAppx.exe, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Usando o layout de empacotamento
Como alternativa, você pode criar um lote simples usando o layout de empacotamento. Para fazer isso, defina o atributo **FlatBundle** como **true** no elemento **PackageFamily** do manifesto do lote de aplicativo. Para saber mais sobre o layout de empacotamento, consulte [Criação do pacote com o layout de empacotamento](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Como implantar um lote simples 
Antes de um lote simples poder ser implantado, cada um dos pacotes de aplicativo (além do lote de aplicativo) precisará ser assinado com o mesmo certificado. Isso ocorre porque todos os arquivos de pacote de aplicativo (.appx/.msix) agora são arquivos independentes e não estão contidos dentro do arquivo de pacote (.appxbundle/.msixbundle) de aplicativo mais. Depois que os pacotes são assinados, use o [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) no PowerShell para apontar para o arquivo de pacote do aplicativo e implantar o aplicativo (supondo que os pacotes de aplicativo são onde o pacote de aplicativo espera que eles sejam). 