---
Description: A tile is an app's representation on the Start menu. Every app has a tile. When you create a new Universal Windows Platform (UWP) app project in Microsoft Visual Studio, it includes a default tile that displays your app's name and logo.
title: Blocos
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e46d73c91f54b1bb74a70990a238f13ccd47645d
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971938"
---
# <a name="tiles-for-uwp-apps"></a>Blocos de aplicativos UWP

 

*Bloco* é a representação de um aplicativo no menu Iniciar. Todo aplicativo tem um bloco. Quando você cria um novo projeto de aplicativo da Plataforma Universal do Windows (UWP) no Microsoft Visual Studio, ele inclui um bloco padrão que exibe o nome e o logotipo do seu aplicativo.O Windows exibe esse bloco quando seu aplicativo é instalado pela primeira vez. Depois que seu aplicativo é instalado, você pode alterar o conteúdo do bloco por meio de notificações. Por exemplo, você pode alterar o bloco para comunicar novas informações ao usuário, como manchetes de jornais, ou o assunto da mensagem não lida mais recente.

## <a name="configure-the-default-tile"></a>Configurar o bloco padrão


Quando você cria um novo projeto no Visual Studio, ele cria um bloco padrão simples que exibe o nome e o logotipo do seu aplicativo.

Para editar seu bloco, clique duas vezes no arquivo **Package.appxmanifest** em seu projeto UWP principal para abrir o designer (ou clique com o botão direito do mouse no arquivo e selecione Exibir Código).

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Há alguns itens que você deve atualizar:

-   DisplayName: substitua esse valor pelo nome que você deseja exibir no bloco.
-   ShortName: como há espaço limitado para seu nome de exibição nos blocos, recomendamos que você especifique um ShortName também, para garantir que o nome do aplicativo não fique truncado.
-   Imagens de logotipo:

    Você deve substituir essas imagens por suas próprias. Você pode fornecer imagens para diferentes escalas visuais, mas não é necessário fornecer para todas elas. Para garantir que seu aplicativo tenha uma boa aparência em um vários dispositivos, recomendamos que você ofereça versões de escala em 100%, 200% e 400% de cada imagem. Consulte [Ativos de bloco e ícone](app-assets.md) para saber mais sobre a geração desses ativos.

    As imagens dimensionadas seguem esta convenção de nomenclatura:
    
    *&lt;nome da imagem&gt;*.scale-*&lt;fator de escala&gt;*.*&lt;extensão de arquivo de imagem&gt;* 

    Por exemplo: SplashScreen.scale-100.png

    Quando você se referir à imagem, faça referência e ela como *&lt;nome da imagem&gt;*.*&lt;extensão de arquivo de imagem&gt;* ("SplashScreen.png" neste exemplo). O sistema selecionará automaticamente a imagem dimensionada apropriada para o dispositivo nas imagens fornecidas por você.

-   Você não precisa, mas é altamente recomendável fornecer logotipos para tamanhos de bloco amplo e grande para que o usuário possa redimensionar o bloco do seu aplicativo para esses tamanhos. Para fornecer essas imagens adicionais, você cria um elemento **DefaultTile** e usa os atributos **Wide310x150Logo** e **Square310x310Logo** para especificar as imagens adicionais:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>Use notificações para personalizar seu bloco


Depois que seu aplicativo for instalado, você pode usar notificações para personalizar seu bloco. Você pode fazer isso na primeira vez que seu aplicativo for iniciado ou em resposta a um evento, como uma notificação por push.

Para saber como enviar notificações de blocos, consulte [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md).
