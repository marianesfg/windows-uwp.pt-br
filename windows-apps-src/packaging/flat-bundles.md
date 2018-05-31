---
author: laurenhughes
title: Pacotes de aplicativo do lote simples
description: Descreve como criar um lote simples de arquivos de pacote .appx do seu aplicativo com referências aos pacotes de aplicativo.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, empacotamento, configuração do pacote, lote simples
ms.localizationpriority: medium
ms.openlocfilehash: 757f95a5f46bad6dbe650b4b552f3de486d84e1b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818201"
---
# <a name="flat-bundle-app-packages"></a>Pacotes de aplicativo do lote simples 

> [!IMPORTANT]
> Se você pretender enviar seu aplicativo para a Store, precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter aprovação para usar lotes simples.

Lotes simples são uma maneira melhor de agrupar os arquivos do pacote .appx do seu aplicativo. Um arquivo .appxbundle típico usa uma estrutura de empacotamento de vários níveis na qual os arquivos do pacote .appx precisam estar contidos dentro do lote. Os lotes simples removem essa necessidade referindo-se apenas aos arquivos do pacote .appx, permitindo que eles fiquem fora do lote de aplicativo. Como os arquivos do pacote .appx não estão mais contidos no lote, eles podem ser processados paralelamente, o que resulta em menor tempo de carregamento, publicação mais rápida (porque cada arquivo do pacote .appx pode ser processado ao mesmo tempo) e, em última análise, iterações de desenvolvimento mais rápidas.

![Diagrama de lote simples](images/bundle-combined.png)

Outra vantagem de lotes simples é a necessidade de criar menos pacotes. Como os arquivos do pacote .appx são apenas referenciados, duas versões do aplicativo poderão referenciar o mesmo arquivo de pacote se o pacote não tiver mudado entre as duas versões. Assim, você precisa criar apenas os pacotes de aplicativo que foram alterados ao compilar os pacotes para a próxima versão do seu aplicativo.
Por padrão, os lotes simples farão referência aos arquivos do pacote .appx na mesma pasta como a si mesmos. No entanto, essa referência pode ser alterada para outros caminhos (caminhos relativos, compartilhamentos de rede e locais http). Para fazer isso, você deve fornecer diretamente um **BundleManifest** durante a criação de lote simples. 

## <a name="how-to-create-a-flat-bundle"></a>Como criar um lote simples

Um lote simples pode ser criado usando a ferramenta MakeAppx.exe ou usando o layout de empacotamento para definir a estrutura do seu lote.

### <a name="using-makeappxexe"></a>Usando a MakeAppx.exe
Para criar um lote simples usando MakeAppx.exe, use o comando “MakeAppx.exe bundle” como de costume, mas com a opção /fb para gerar o arquivo .appxbundle simples (que será extremamente pequeno, pois só faz referência aos pacotes .appx e não tem cargas reais). 

Eis um exemplo da sintaxe do comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

Para obter mais informações sobre como usar MakeAppx.exe, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Usando o layout de empacotamento
Como alternativa, você pode criar um lote simples usando o layout de empacotamento. Para fazer isso, defina o atributo **FlatBundle** como **true** no elemento **PackageFamily** do manifesto do lote de aplicativo. Para saber mais sobre o layout de empacotamento, consulte [Criação do pacote com o layout de empacotamento](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Como implantar um lote simples 
Antes de um lote simples poder ser implantado, cada um dos pacotes de aplicativo (além do lote de aplicativo) precisará ser assinado com o mesmo certificado. Isso ocorre porque todos os arquivos do pacote do aplicativo (.appx) agora são arquivos independentes e não estão mais contidos no lote de aplicativo (.appxbundle). Depois que os pacotes forem assinados, use o [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) no PowerShell para apontar para o arquivo .appxbundle e implante o aplicativo (supondo que os pacotes de aplicativo estão onde o lote de aplicativo espera). 