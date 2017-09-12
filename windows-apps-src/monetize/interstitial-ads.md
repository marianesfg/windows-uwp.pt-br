---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Aprenda a incluir anúncios intersticiais em um aplicativo do Windows 10, Windows 8.1 ou Windows Phone 8.1 usando as bibliotecas do Microsoft Advertising."
title: "Anúncios intersticiais"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, controle de anúncio, intersticial"
ms.openlocfilehash: 16cb78d9419d54a1bd56fb855ca1fad6fedf665a
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2017
---
# <a name="interstitial-ads"></a>Anúncios intersticiais

Este passo a passo mostra como incluir anúncios intersticiais em aplicativos da Plataforma Universal do Windows (UWP) e jogos para Windows 10, e nos aplicativos para Windows 8.1 ou Windows Phone 8.1. Para obter projetos de exemplo completos que demonstram como adicionar anúncios intersticiais a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>O que são anúncios intersticiais?

Diferente dos anúncios padrão que se restringe a uma parte de uma interface do usuário em um aplicativo ou jogo, os anúncios intersticiais são mostrados na tela inteira. Duas formas básicas são frequentemente usadas em jogos.

* Com anúncios de *Acesso pago*, o usuário deve assistir a um anúncio em algum intervalo regular. Por exemplo, entre os níveis dos jogos:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Com anúncios *Baseados em recompensas*, o usuário está explicitamente buscando algum benefício, como uma dica ou tempo extra para concluir o nível, e inicializa o anúncio por meio da interface do usuário do aplicativo.

Oferecemos dois tipos de anúncios intersticiais para usar em seus aplicativos e jogos:

* **Anúncios intersticiais em vídeo**: estão disponíveis para aplicativos da Plataforma Universal do Windows (UWP) para Windows 10 e nos aplicativos para Windows 8.1 ou Windows Phone 8.1.

* **Anúncios intersticiais**: estão disponíveis somente nos aplicativos UWP para Windows 10.

> [!NOTE]
> A API para anúncios intersticiais não manipula nenhuma interface do usuário exceto no momento da reprodução de vídeo. Consulte as [práticas recomendadas de anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obter diretrizes sobre o que fazer e evitar ao considerar como integrar anúncios intersticiais no aplicativo.

## <a name="build-an-app-with-interstitial-ads"></a>Compilar um aplicativo com anúncios intersticiais

Para mostrar anúncios intersticiais no aplicativo, siga as instruções para o tipo de projeto:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (interoperabilidade DirectX)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>Pré-requisitos

* Para aplicativos UWP: instalar o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) com o Visual Studio 2015 ou uma versão posterior.

* Em aplicativos do Windows 8.1 ou Windows Phone 8.1: instale o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.

<span id="interstitialadsxaml10"/>
### <a name="xamlnet"></a>XAML/.NET

Esta seção fornece exemplos em C#, mas também há suporte para Visual Basic e C++ para projetos XAML/.NET. Para obter um exemplo de código em C#, consulte [Código de exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md).

1. Abra o projeto no Visual Studio.

2. Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

  * Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

  * Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e, em seguida, marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

  * Para um projeto do Windows Phone 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows Phone 8.1 XAML**. Essa opção adicionará as bibliotecas do Microsoft Advertising e do Ad Mediator ao projeto, mas é possível ignorar as bibliotecas do Ad Mediator.

3.  No arquivo de código apropriado no aplicativo (por exemplo, em MainPage.xaml.cs ou um arquivo de código para alguma outra página), adicione a seguinte referência de namespace.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  Em um local indicado no aplicativo (por exemplo, em ```MainPage``` ou em alguma outra página), declare um objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e diversos campos da cadeia de caracteres que representam a ID do aplicativo e a ID da unidade de anúncio para o anúncio intersticial. O seguinte exemplo de código atribui os campos `myAppId` e `myAdUnitId` aos valores de teste para anúncio de vídeo intersticiais fornecidos nos [Valores do modo de teste](test-mode-values.md).

  > [!NOTE]
  > Cada **InterstitialAd** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Loja, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Se você deseja mostrar um anúncio de *vídeo intersticial*: Aproximadamente de 30 a 60 segundos antes de precisar do anúncio, use o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType.Video** para o tipo de anúncio.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

  Se você quiser mostrar um anúncio de *banner intersticial* (somente para aplicativos UWP): aproximadamente de 5 a 8 segundos antes do anúncio, você precisa usar o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType.Display** para o tipo de anúncio.

  ```csharp
  myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
  ```

6.  No momento em que você deseja mostrar o vídeo intersticial ou anúncio em faixa intersticial, confirme que o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="interstitialadshtml10"/>
### <a name="htmljavascript"></a>HTML/JavaScript

As instruções a seguir pressupõem que você tenha criado um projeto Universal do Windows para JavaScript no Visual Studio e esteja segmentando uma CPU específica. Para obter um exemplo de código completo, consulte [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Abra o projeto no Visual Studio.

2.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

  * Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).

  * Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows 8.1 nativo (JS)**.

  * Para um projeto do Windows 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)**.

3.  Na seção **&lt;head&gt;**, do arquivo HTML no projeto, após as referências JavaScript de default.css e default.js do projeto, adicione a referência a ad.js. Em um projeto UWP, adicione a referência a seguir.

  ``` HTML
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  Em um projeto do Windows 8.1 ou do Windows Phone 8.1, adicione a referência a seguir.

  ``` HTML
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  Em um arquivo .js no projeto, declare um objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e vários campos que contenham a ID do aplicativo e a ID da unidade do anúncio intersticial. O seguinte exemplo de código atribui os campos `applicationId` e `adUnitId` aos valores de teste para anúncio de vídeo intersticiais fornecidos nos [Valores do modo de teste](test-mode-values.md).

  > [!NOTE]
  > Cada **InterstitialAd** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Loja, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Se você deseja mostrar um anúncio de *vídeo intersticial*: Aproximadamente de 30 a 60 segundos antes de precisar do anúncio, use o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **InterstitialAdType.video** para o tipo de anúncio.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

  Se você quiser mostrar um anúncio de *banner intersticial* (somente para aplicativos UWP): aproximadamente de 5 a 8 segundos antes do anúncio, você precisa usar o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **InterstitialAdType.display** para o tipo de anúncio.

  ```js
  if (interstitialAd) {
      interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
  }
  ```

6.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="interstitialadsdirectx10"/>
### <a name="c-directx-interop"></a>C++ (interoperabilidade DirectX)

Este exemplo pressupõe que você tenha criado um projeto **DirectX and XAML App (Universal Windows)** em C++ no Visual Studio e esteja segmentando uma arquitetura de CPU específica.
 
1. Abra o projeto no Visual Studio.

1.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

  * Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

  * Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e, em seguida, marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

  * Para um projeto do Windows Phone 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows Phone 8.1 XAML**. Essa opção adicionará as bibliotecas do Microsoft Advertising e do Ad Mediator ao projeto, mas é possível ignorar as bibliotecas do Ad Mediator.

2.  Em um arquivo de cabeçalho indicado para o aplicativo (por exemplo, DirectXPage.xaml. h), declare um objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e os métodos do manipulador de eventos relacionados.  

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  No mesmo arquivo de cabeçalho, declare diversos campos da cadeia de caracteres que representam a ID do app e a ID da unidade de anúncio para o anúncio intersticial. O seguinte exemplo de código atribui os campos `myAppId` e `myAdUnitId` aos valores de teste para anúncio de vídeo intersticiais fornecidos nos [Valores do modo de teste](test-mode-values.md).

  > [!NOTE]
  > Cada **InterstitialAd** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Loja, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  No arquivo .cpp em que você deseja adicionar código para mostrar um anúncio intersticial, adicione a referência de namespace a seguir. Os exemplos a seguir pressupõem que você esteja adicionando o código ao arquivo DirectXPage.xaml.cpp no aplicativo.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto. No exemplo a seguir, ```InterstitialAdSamplesCpp``` é o namespace do projeto; altere esse nome conforme necessário para o código.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Se você deseja mostrar um anúncio de *vídeo intersticial*: Aproximadamente de 30 a 60 segundos antes de precisar do anúncio intersticial, use o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType::Video** para o tipo de anúncio.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

  Se você quiser mostrar um anúncio de *banner intersticial* (somente para aplicativos UWP): aproximadamente de 5 a 8 segundos antes do anúncio, você precisa usar o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType::Display** para o tipo de anúncio.

  ```cpp
  m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
  ```

7.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Lançar o app com anúncios ativos usando o Centro de Desenvolvimento do Windows

1.  No painel do Centro de Desenvolvimento, vá até a página [Monetizar com anúncios](../publish/monetize-with-ads.md) do app e [crie uma unidade publicitária](../monetize/set-up-ad-units-in-your-app.md). Para o tipo de unidade publicitária, escolha **Vídeo intersticial** ou **Faixa intersticial**, dependendo do tipo de anúncio intersticial mostrado. Anote a ID da unidade de anúncio e a ID do aplicativo.

2. Se o seu aplicativo for um aplicativo UWP para o Windows 10, você pode, opcionalmente, ativar o controle de anúncios para o **InterstitialAd** definindo as configurações na seção [Controle de anúncios](../publish/monetize-with-ads.md#mediation) na página [Monetizar com anúncios](../publish/monetize-with-ads.md). O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas, como Taboola e Smaato e anúncios para campanhas promocionais de aplicativos da Microsoft.

3.  Em seu código, substitua os valores de unidade publicitária de teste pelos valores dinâmicos gerados no Centro de Desenvolvimento.

4.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento do Windows.

5.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios intersticial em seu app

Você pode usar vários controles **InterstitialAd** em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/monetize-with-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>Políticas e práticas recomendadas para anúncios intersticiais


Para obter mais informações sobre como usar anúncios intersticiais com eficácia e políticas que devem ser seguidas, consulte [Políticas e práticas recomendadas para anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>Remover erros de referência: direcionar uma plataforma específica da CPU (XAML e HTML)


Ao usar as bibliotecas do Microsoft Advertising, você não pode direcionar para **Any CPU** em seu projeto. Se o seu projeto for direcionado para **qualquer plataforma de CPU**, você poderá ver um aviso em seu projeto depois de adicionar uma referência às bibliotecas do Microsoft Advertising. Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Para saber mais, consulte [Problemas conhecidos](known-issues-for-the-advertising-libraries.md).

## <a name="related-topics"></a>Tópicos relacionados


* [Código exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md)
* [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Configurar unidades publicitárias para seu app](../monetize/set-up-ad-units-in-your-app.md)
