---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Aprenda a incluir anúncios intersticiais em um aplicativo do Windows 10, Windows 8.1 ou Windows Phone 8.1 usando as bibliotecas do Microsoft Advertising no SDK do Microsoft Store Engagement and Monetization.
title: Anúncios intersticiais
---

# Anúncios intersticiais


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este passo a passo mostra como incluir anúncios intersticiais em um aplicativo do Windows 10, Windows 8.1 ou Windows Phone 8.1 usando as bibliotecas do Microsoft Advertising do SDK do Microsoft Store Engagement and Monetization.

Para obter projetos de exemplo completos que demonstram como adicionar anúncios intersticiais a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## O que são anúncios intersticiais?

Diferentemente de anúncios, os anúncios intersticiais (ou *intersticiais*) são mostrados em toda a tela do aplicativo. Duas formas básicas são frequentemente usadas em jogos.

* Com anúncios de *Acesso pago*, o usuário deve assistir a um anúncio em algum intervalo regular. Por exemplo, entre os níveis dos jogos:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Com anúncios *Baseados em recompensas*, o usuário está explicitamente buscando algum benefício, como uma dica ou tempo extra para concluir o nível, e inicializa o vídeo do anúncio por meio da interface do usuário do aplicativo.

    É importante observar que este SDK não manipula nenhuma interface do usuário, exceto no momento da reprodução do vídeo. Consulte as [práticas recomendadas de anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obter diretrizes sobre o que fazer e evitar ao considerar como integrar anúncios intersticiais em seu aplicativo.

## Criando um aplicativo com anúncios intersticiais


### Pré-requisitos

1.  Instale o [SDK do Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.

2.  No Visual Studio, abra o projeto ou crie um novo projeto.

### Desenvolvimento de código

* [Etapas para um aplicativo XAML/.NET](#interstitialadsxaml10)

* [Etapas para HTML/JavaScript](#interstitialadshtml10)

* [Etapas para C++ (Interoperabilidade com o DirectX)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### Anúncios intersticiais (XAML/.NET)

> **Observação**   Esta seção fornece exemplos em C#, mas também há suporte para Visual Basic e C++.
 
1. Abra seu projeto no Visual Studio.
2. Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

    -   Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

    -   Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e, em seguida, marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

    -   Para um projeto do Windows Phone 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows Phone 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

3.  No código do aplicativo, inclua a seguinte referência ao namespace.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  Declare suas propriedades `MyAppId` e `MyAdUnitId`.

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **Observação**   Você substituirá os valores de teste por valores dinâmicos antes de enviar seu aplicativo.

5.  Instancie um [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), ative todos os manipuladores de eventos e solicite um anúncio.

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  No ponto em seu código onde o anúncio deve ser exibido, certifique-se de que o anúncio esteja pronto e mostre-o.

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  Defina e codifique os eventos.

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  Atribua a propriedade `MyAppId` ao valor de teste fornecido em [Valores de modo de teste](test-mode-values.md). Esse valor é usado somente para testes; você o substituirá por um valor dinâmico antes de publicar seu aplicativo.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Atribua a propriedade `MyAdUnitId` ao valor de teste fornecido em [Valores de modo de teste](test-mode-values.md). Esse valor é usado somente para testes; você o substituirá por um valor dinâmico antes de publicar seu aplicativo.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  Compile e teste seu aplicativo para confirmar que ele está mostrando anúncios de teste.

<span id="interstitialadshtml10"/>
### Anúncios intersticiais (HTML/JavaScript)

Neste exemplo, supõe-se que você tenha criado um projeto de aplicativo universal para JavaScript no Visual Studio 2015 e esteja direcionando para uma CPU específica.

1. Abra seu projeto no Visual Studio.
2.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

    -   Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).

    -   Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows 8.1 nativo (JS)**.

    -   Para um projeto do Windows 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows Phone 8.1 nativo (JS)**.

3.  No HTML, inclua a referência de script a seguir.

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Declare suas propriedades `myAppId` e `myAdUnitId`.

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  Instancie um **InterstitialAd**, ative todos os manipuladores de eventos e solicite um anúncio.

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  No ponto em seu código onde o anúncio deve ser exibido, certifique-se de que o anúncio esteja pronto e mostre-o.

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  Defina e codifique os eventos.

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  Atribua a propriedade `MyAppId` ao valor de teste fornecido em [Valores de modo de teste](test-mode-values.md). Esse valor é usado somente para testes; você o substituirá por um valor dinâmico antes de publicar seu aplicativo.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  Atribua a propriedade `MyAdUnitId` ao valor de teste fornecido em [Valores de modo de teste](test-mode-values.md). Esse valor é usado somente para testes; você o substituirá por um valor dinâmico antes de publicar seu aplicativo.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  Compile e teste seu aplicativo para confirmar que ele está mostrando anúncios de teste.

<span id="interstitialadsdirectx10"/>
### Anúncios intersticiais (C++ e DirectX com interoperabilidade com XAML)

Neste exemplo, supõe-se que você tenha criado um projeto de aplicativo Universal para XAML no Visual Studio 2015 e esteja direcionando para uma arquitetura de CPU específica.

> **Importante**   Esse código é escrito em C++ conforme necessário para o DirectX.

 
1. Abra seu projeto no Visual Studio.
1.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

    -   Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

    -   Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e, em seguida, marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

    -   Para um projeto do Windows Phone 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Ad Mediator para Windows Phone 8.1 XAML**. Essa opção adiciona as bibliotecas do Microsoft Advertising e do Ad Mediator ao seu projeto, mas você pode ignorar as bibliotecas do Ad Mediator.

2.  No arquivo de cabeçalho apropriado para seu aplicativo, declare o objeto de anúncio intersticial e as propriedades/métodos relacionados.

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  Declare suas propriedades `AppId` e `AdUnitId`.

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  No arquivo. cpp, adicione uma referência de namespace.

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  Instancie um **InterstitialAd**, ative todos os manipuladores de eventos e solicite um anúncio.

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  No ponto em seu código onde o anúncio deve ser exibido, certifique-se de que o anúncio esteja pronto e mostre-o.

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  Defina e codifique os eventos.

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  Atribua a propriedade `AppId` ao valor de teste fornecido em [Valores de modo de teste](test-mode-values.md). Esse valor é usado somente para testes; você o substituirá por um valor dinâmico antes de publicar seu aplicativo.

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Atribua a propriedade `AdUnitId` ao valor de teste fornecido em [Valores de modo de teste](test-mode-values.md). Esse valor é usado somente para testes; você o substituirá por um valor dinâmico antes de publicar seu aplicativo.

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. Compile e teste seu aplicativo para confirmar que ele está mostrando anúncios de teste.

### Lançar seu aplicativo com anúncios dinâmicos usando o Centro de Desenvolvimento do Windows

1.  No painel do Centro de Desenvolvimento, vá para a página **Monetização**&gt;**Monetizar com anúncios** para seu aplicativo e [crie uma unidade autônoma do Microsoft Advertising](../publish/monetize-with-ads.md). Para o tipo de unidade de anúncio, especifique **Vídeo intersticial**. Anote a ID da unidade de anúncios e a ID do aplicativo.

2.  Em seu código, substitua os valores de unidade de anúncio de teste pelos valores dinâmicos gerados no Centro de Desenvolvimento.

3.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento do Windows.

4.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

<span id="interstitialbestpractices10"/>
## Práticas recomendadas para anúncios intersticiais


Para obter mais informações sobre como usar anúncios intersticiais com eficácia, consulte [Diretrizes da experiência do usuário e interface do usuário](ui-and-user-experience-guidelines.md).

<span id="targetplatform10"/>
## Remover erros de referência: direcionar uma plataforma específica da CPU (XAML e HTML)


Ao usar as bibliotecas do Microsoft Advertising, você não pode direcionar para **Any CPU** em seu projeto. Se o seu projeto for direcionado para a plataforma **Any CPU**, você poderá ver um aviso em seu projeto depois de adicionar uma referência às bibliotecas do Microsoft Advertising. Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Para saber mais, consulte [Problemas conhecidos](known-issues-for-the-advertising-libraries.md).

## Tópicos relacionados


* [Código de exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md)
* [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 

 


<!--HONumber=May16_HO2-->


