---
author: mijacobs
Description: "Bloco é a representação de um aplicativo no menu Iniciar. Todo aplicativo tem um bloco. Quando você cria um novo projeto de aplicativo da Plataforma Universal do Windows (UWP) no Microsoft Visual Studio, ele inclui um bloco padrão que exibe o nome e o logotipo do seu aplicativo."
title: Blocos
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.sourcegitcommit: d3fe62d4de00c42079d62d105acdbb21e296ba5f
ms.openlocfilehash: a9f5d25dfd359364fa8e16666b03c7c105a867dd

---

# Blocos de aplicativos UWP





*Bloco* é a representação de um aplicativo no menu Iniciar. Todo aplicativo tem um bloco. Quando você cria um novo projeto de aplicativo da Plataforma Universal do Windows (UWP) no Microsoft Visual Studio, ele inclui um bloco padrão que exibe o nome e o logotipo do seu aplicativo. O Windows exibe esse bloco quando seu aplicativo é instalado pela primeira vez. Depois que seu aplicativo é instalado, você pode alterar o conteúdo do bloco por meio de notificações. Por exemplo, você pode alterar o bloco para comunicar novas informações ao usuário, como manchetes de jornais, ou o assunto da mensagem não lida mais recente.

## <span id="Configure_the_default_tile"></span><span id="configure_the_default_tile"></span><span id="CONFIGURE_THE_DEFAULT_TILE"></span>Configurar o bloco padrão


Quando você cria um novo projeto no Visual Studio, ele cria um bloco padrão simples que exibe o nome e o logotipo do seu aplicativo.

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
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

    Você deve substituir essas imagens por suas próprias. Você pode fornecer imagens para diferentes escalas visuais, mas não é necessário fornecer para todas elas. Para garantir que seu aplicativo tenha uma boa aparência em um vários dispositivos, recomendamos que você ofereça versões de escala em 100%, 200% e 400% de cada imagem.

    As imagens dimensionadas seguem esta convenção de nomenclatura: 
    
    *
              &lt;nome da imagem&gt;*.scale -*&lt;fator de escala&gt;*. *&lt;extensão de arquivo de imagem&gt;*  
    
    Por exemplo: SmallLogo.scale-100.png

    Quando você se referir à imagem, faça referência e ela como *&lt;nome da imagem&gt;*.*&lt;extensão de arquivo de imagem&gt;* ("SmallLogo.png" neste exemplo). O sistema selecionará automaticamente a imagem dimensionada apropriada para o dispositivo nas imagens fornecidas por você.

-   Você não precisa, mas é altamente recomendável fornecer logotipos para tamanhos de bloco amplo e grande para que o usuário possa redimensionar o bloco do seu aplicativo para esses tamanhos. Para fornecer essas imagens adicionais, você cria um elemento `DefaultTile` e usa os atributos `Wide310x150Logo` e `Square310x310Logo` para especificar as imagens adicionais:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <span id="Use_notifications_to_customize_your_tile"></span><span id="use_notifications_to_customize_your_tile"></span><span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"></span>Use notificações para personalizar seu bloco


Depois que seu aplicativo for instalado, você pode usar notificações para personalizar seu bloco. Você pode fazer isso na primeira vez que seu aplicativo for iniciado ou em resposta a algum evento, como uma notificação por push.

1.  Crie uma carga XML (na forma de um [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)) que descreve o bloco.

    -   O Windows 10 apresenta um novo esquema de bloco adaptável que você pode usar. Para obter instruções, consulte [Blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md). Para o esquema, consulte o artigo [Esquema de blocos adaptáveis](tiles-and-notifications-adaptive-tiles-schema.md). 

    -   Você pode usar os modelos de bloco do Windows 8.1 para definir seu bloco. Para saber mais, consulte [Criando blocos e notificações (Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260).

2.  Crie um objeto de notificação de bloco e passe para ele o [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) que você criou. Existem vários tipos de objetos de notificação:
    -   Um objeto [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) para atualizar imediatamente o bloco.
    -   Um objeto [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) para atualizar o bloco em algum momento no futuro.

3.  Use [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623) para criar um objeto [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628).
4.  Chame o método [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) e passe para ele o objeto de notificação de bloco que você criou na etapa 2.

 

 







<!--HONumber=Jun16_HO4-->


