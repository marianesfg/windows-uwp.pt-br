---
author: laurenhughes
title: Pacotes de aplicativo do lote simples
description: Descreve como criar um lote simples de arquivos de pacote .appx do seu aplicativo com referências aos pacotes de aplicativo.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, empacotamento, configuração do pacote, lote simples
ms.localizationpriority: medium
ms.openlocfilehash: 63206619d75bedb92ad6c6d05c3188272c0760de
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5397939"
---
# <a name="flat-bundle-app-packages"></a>Pacotes de aplicativo do lote simples 

> [!IMPORTANT]
> Se você pretender enviar seu aplicativo para a Store, precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter aprovação para usar lotes simples.

Lotes simples são uma maneira melhor de arquivos do pacote do aplicativo. Um arquivo de lote de aplicativo usa uma estrutura de empacotamento de vários níveis na qual os arquivos de pacote do aplicativo precisam estar contidos dentro do lote do Windows típico, lotes simples removem essa necessidade referindo-se apenas os arquivos de pacote do aplicativo, permitindo que eles fiquem fora do lote de aplicativo. Como os arquivos de pacote do aplicativo não estão contidos no lote, eles podem ser processados paralelamente, que resulta em menor tempo, publicação mais rápida (porque cada arquivo de pacote do aplicativo pode ser processado ao mesmo tempo) de carregamento e, por fim, mais rápido desenvolvimento iterações.

![Diagrama de lote simples](images/bundle-combined.png)

Outra vantagem de lotes simples é a necessidade de criar menos pacotes. Como arquivos do pacote de aplicativo são apenas referenciados, duas versões do aplicativo podem referenciar o mesmo arquivo de pacote se o pacote não tiver mudado entre as duas versões. Assim, você precisa criar apenas os pacotes de aplicativo que foram alterados ao compilar os pacotes para a próxima versão do seu aplicativo.
Por padrão, os lotes simples farão referência arquivos do pacote de aplicativo na mesma pasta que o próprio. No entanto, essa referência pode ser alterada para outros caminhos (caminhos relativos, compartilhamentos de rede e locais http). Para fazer isso, você deve fornecer diretamente um **BundleManifest** durante a criação de lote simples. 

## <a name="how-to-create-a-flat-bundle"></a>Como criar um lote simples

Um lote simples pode ser criado usando a ferramenta MakeAppx.exe ou usando o layout de empacotamento para definir a estrutura do seu lote.

### <a name="using-makeappxexe"></a>Usando a MakeAppx.exe
Para criar um lote simples usando MakeAppx.exe, use o comando "MakeAppx.exe bundle" como de costume, mas com a opção /FB para gerar o arquivo de lote de aplicativo simples (que será extremamente pequeno, pois só faz referência os arquivos de pacote do aplicativo e não tem cargas reais ). 

Eis um exemplo da sintaxe do comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

Para obter mais informações sobre como usar MakeAppx.exe, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Usando o layout de empacotamento
Como alternativa, você pode criar um lote simples usando o layout de empacotamento. Para fazer isso, defina o atributo **FlatBundle** como **true** no elemento **PackageFamily** do manifesto do lote de aplicativo. Para saber mais sobre o layout de empacotamento, consulte [Criação do pacote com o layout de empacotamento](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Como implantar um lote simples 
Antes de um lote simples poder ser implantado, cada um dos pacotes de aplicativo (além do lote de aplicativo) precisará ser assinado com o mesmo certificado. Isso ocorre porque todos os arquivos de pacote de aplicativo (.appx/.msix) agora são arquivos independentes e não são mais contidos no arquivo de lote (.appxbundle/.msixbundle) do aplicativo. Depois que os pacotes forem assinados, use o [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) do PowerShell para apontar para o arquivo de lote de aplicativo e implantar o aplicativo (supondo que pacotes de aplicativo estão onde o lote de aplicativo espera). 