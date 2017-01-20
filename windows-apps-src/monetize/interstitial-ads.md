---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Aprenda a incluir anúncios intersticiais em um aplicativo do Windows 10, Windows 8.1 ou Windows Phone 8.1 usando as bibliotecas do Microsoft Advertising no Microsoft Store Services SDK."
title: "Anúncios intersticiais"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: fae0fc57eca3477bf46a6f3ac43ec35781241a6e

---

# <a name="interstitial-ads"></a>Anúncios intersticiais

Este passo a passo mostra como incluir anúncios intersticiais em um aplicativo do Windows 10, Windows 8.1 ou Windows Phone 8.1 usando as bibliotecas do Microsoft Advertising do Microsoft Store Services SDK.

Para obter projetos de exemplo completos que demonstram como adicionar anúncios intersticiais a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>O que são anúncios intersticiais?

Diferentemente de anúncios, os anúncios intersticiais (ou *intersticiais*) são mostrados em toda a tela do aplicativo. Duas formas básicas são frequentemente usadas em jogos.

* Com anúncios de *Acesso pago*, o usuário deve assistir a um anúncio em algum intervalo regular. Por exemplo, entre os níveis dos jogos:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Com anúncios *Baseados em recompensas*, o usuário está explicitamente buscando algum benefício, como uma dica ou tempo extra para concluir o nível, e inicializa o vídeo do anúncio por meio da interface do usuário do aplicativo.

    É importante observar que este SDK não manipula nenhuma interface do usuário, exceto no momento da reprodução do vídeo. Consulte as [práticas recomendadas de anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obter diretrizes sobre o que fazer e evitar ao considerar como integrar anúncios intersticiais no aplicativo.

## <a name="build-an-app-with-interstitial-ads"></a>Compilar um aplicativo com anúncios intersticiais

Para mostrar anúncios intersticiais no aplicativo, siga as instruções para o tipo de projeto:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (interoperabilidade DirectX)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>Pré-requisitos

* Para aplicativos UWP: instale o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) com o Visual Studio 2015.
* Para aplicativos do Windows 8.1 ou do Windows Phone 8.1: instale o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.

<span id="interstitialadsxaml10"/>
###<a name="xamlnet"></a>XAML/.NET

Esta seção fornece exemplos em C#, mas também há suporte para Visual Basic e C++ para projetos XAML/.NET. Para obter um exemplo de código em C#, consulte [Código de exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md).

1. Abra o projeto no Visual Studio.

2. Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

  * Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

  * Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e, em seguida, marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

  * Para um projeto do Windows Phone 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows Phone 8.1 XAML**. Essa opção adicionará as bibliotecas do Microsoft Advertising e do Ad Mediator ao projeto, mas é possível ignorar as bibliotecas do Ad Mediator.

3.  No arquivo de código apropriado no aplicativo (por exemplo, em MainPage.xaml.cs ou um arquivo de código para alguma outra página), adicione a seguinte referência de namespace.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  Em um local indicado no aplicativo (por exemplo, em ```MainPage``` ou em alguma outra página), declare um objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e diversos campos da cadeia de caracteres que representam a ID do aplicativo e a ID da unidade de anúncio para o anúncio intersticial. O exemplo de código a seguir atribui os campos `myAppId` e `myAdUnitId` aos valores de teste fornecidos em [Valores de modo de teste](test-mode-values.md). Esses valores só são usados em testes; você deve substituí-los por valores dinâmicos do Centro de Desenvolvimento do Windows antes de publicar o aplicativo.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Aproximadamente de 30 a 60 segundos antes de precisar do anúncio intersticial, use o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

6.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="interstitialadshtml10"/>
###<a name="htmljavascript"></a>HTML/JavaScript

As instruções a seguir pressupõem que você tenha criado um projeto Universal do Windows para JavaScript no Visual Studio 2015 e esteja segmentando uma CPU específica. Para obter um exemplo de código completo, consulte [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Abra o projeto no Visual Studio.

2.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

  * Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).

  * Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows 8.1 nativo (JS)**.

  * Para um projeto do Windows 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)**.

3.  Na seção **&lt;head&gt;**, do arquivo HTML no projeto, após as referências JavaScript de default.css e default.js do projeto, adicione a referência a ad.js. Em um projeto UWP, adicione a referência a seguir.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  Em um projeto do Windows 8.1 ou do Windows Phone 8.1, adicione a referência a seguir.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  Em um arquivo .js no projeto, declare um objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e vários campos que contenham a ID do aplicativo e a ID da unidade do anúncio intersticial. O exemplo de código a seguir atribui os campos `applicationId` e `adUnitId` aos valores de teste fornecidos em [Valores de modo de teste](test-mode-values.md). Esses valores só são usados em testes; você deve substituí-los por valores dinâmicos do Centro de Desenvolvimento do Windows antes de publicar o aplicativo.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Aproximadamente de 30 a 60 segundos antes de precisar do anúncio intersticial, use o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

6.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="interstitialadsdirectx10"/>
###<a name="c-directx-interop"></a>C++ (interoperabilidade DirectX)

Este exemplo pressupõe que você tenha criado um projeto **DirectX and XAML App (Universal Windows)** em C++ no Visual Studio 2015 e esteja segmentando uma arquitetura de CPU específica.
 
1. Abra o projeto no Visual Studio.

1.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

  * Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

  * Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e, em seguida, marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

  * Para um projeto do Windows Phone 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows Phone 8.1 XAML**. Essa opção adicionará as bibliotecas do Microsoft Advertising e do Ad Mediator ao projeto, mas é possível ignorar as bibliotecas do Ad Mediator.

2.  Em um arquivo de cabeçalho indicado para o aplicativo (por exemplo, DirectXPage.xaml. h), declare um objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e os métodos do manipulador de eventos relacionados.  

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  No mesmo arquivo de cabeçalho, declare diversos campos da cadeia de caracteres que representam a ID do aplicativo e a ID da unidade de anúncio para o anúncio intersticial. O exemplo de código a seguir atribui os campos `myAppId` e `myAdUnitId` aos valores de teste fornecidos em [Valores de modo de teste](test-mode-values.md). Esses valores só são usados em testes; você deve substituí-los por valores dinâmicos do Centro de Desenvolvimento do Windows antes de publicar o aplicativo.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  No arquivo .cpp em que você deseja adicionar código para mostrar um anúncio intersticial, adicione a referência de namespace a seguir. Os exemplos a seguir pressupõem que você esteja adicionando o código ao arquivo DirectXPage.xaml.cpp no aplicativo.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto. No exemplo a seguir, ```InterstitialAdSamplesCpp``` é o namespace do projeto; altere esse nome conforme necessário para o código.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Aproximadamente de 30 a 60 segundos antes de precisar do anúncio intersticial, use o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

7.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span/>
### <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Lançar o aplicativo com anúncios ativos usando o Centro de Desenvolvimento do Windows

1.  No painel do Centro de Desenvolvimento, vá até a página **Monetização** &gt; **Monetizar com anúncios** do aplicativo e [crie uma unidade de anúncio](../publish/monetize-with-ads.md). Para o tipo de unidade de anúncio, especifique **Vídeo intersticial**. Anote a ID da unidade de anúncio e a ID do aplicativo.

2.  Em seu código, substitua os valores de unidade de anúncio de teste pelos valores dinâmicos gerados no Centro de Desenvolvimento.

3.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento do Windows.

4.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>Políticas e práticas recomendadas para anúncios intersticiais


Para obter mais informações sobre como usar anúncios intersticiais com eficácia e políticas que devem ser seguidas, consulte [Políticas e práticas recomendadas para anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>Remover erros de referência: direcionar uma plataforma específica da CPU (XAML e HTML)


Ao usar as bibliotecas do Microsoft Advertising, você não pode direcionar para **Any CPU** em seu projeto. Se o seu projeto for direcionado para **qualquer plataforma de CPU**, você poderá ver um aviso em seu projeto depois de adicionar uma referência às bibliotecas do Microsoft Advertising. Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Para saber mais, consulte [Problemas conhecidos](known-issues-for-the-advertising-libraries.md).

## <a name="related-topics"></a>Tópicos relacionados


* [Código de exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md)
* [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Dec16_HO2-->


