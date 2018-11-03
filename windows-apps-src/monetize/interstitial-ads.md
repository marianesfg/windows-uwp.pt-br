---
author: Xansky
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Saiba como incluir anúncios intersticiais em um aplicativo UWP para Windows 10 usando o SDK do Microsoft Advertising.
title: Anúncios intersticiais
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, controle de anúncio, intersticial
ms.localizationpriority: medium
ms.openlocfilehash: 27ef8173db2976d58f9ccd0422a1217e2bd91d13
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5980680"
---
# <a name="interstitial-ads"></a>Anúncios intersticiais

Este passo a passo mostra como incluir anúncios intersticiais em aplicativos da UWP e jogos para Windows 10. Para obter projetos de exemplo completos que demonstram como adicionar anúncios intersticiais a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>O que são anúncios intersticiais?

Diferente dos anúncios padrão que se restringe a uma parte de uma interface do usuário em um aplicativo ou jogo, os anúncios intersticiais são mostrados na tela inteira. Duas formas básicas são frequentemente usadas em jogos.

* Com anúncios de *Acesso pago*, o usuário deve assistir a um anúncio em algum intervalo regular. Por exemplo, entre os níveis dos jogos:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Com anúncios *Baseados em recompensas*, o usuário está explicitamente buscando algum benefício, como uma dica ou tempo extra para concluir o nível, e inicializa o anúncio por meio da interface do usuário do aplicativo.

Oferecemos dois tipos de anúncios intersticiais para usar em seus aplicativos e jogos: **anúncios intersticiais em vídeo** e **anúncios intersticiais**.

> [!NOTE]
> A API para anúncios intersticiais não manipula nenhuma interface do usuário exceto no momento da reprodução de vídeo. Consulte as [práticas recomendadas de anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obter diretrizes sobre o que fazer e evitar ao considerar como integrar anúncios intersticiais no aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) com o Visual Studio 2015 ou uma versão posterior do Visual Studio. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Integrar um anúncio intersticial ao aplicativo

Para mostrar anúncios intersticiais no aplicativo, siga as instruções para o tipo de projeto:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (interoperabilidade DirectX)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

Esta seção fornece exemplos em C#, mas também há suporte para Visual Basic e C++ para projetos XAML/.NET. Para obter um exemplo de código em C#, consulte [Código de exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md).

1. Abra seu projeto no Visual Studio.
    > [!NOTE]
    > Se você estiver usando um projeto existente, abra o arquivo Package. appxmanifest em seu projeto e certifique-se de que o recurso da **Internet (cliente)** está selecionado. Seu aplicativo precisa dessa funcionalidade para receber anúncios de teste e anúncios ativos.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência ao SDK do Microsoft Advertising em seu projeto:

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
    3.  No **Gerenciador de Referências**, clique em OK.

3.  No arquivo de código apropriado no aplicativo (por exemplo, em MainPage.xaml.cs ou um arquivo de código para alguma outra página), adicione a seguinte referência de namespace.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  Em um local indicado no aplicativo (por exemplo, em ```MainPage``` ou em alguma outra página), declare um objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) e diversos campos da cadeia de caracteres que representam a ID do aplicativo e a ID da unidade de anúncio para o anúncio intersticial. O exemplo de código a seguir atribui os campos `myAppId` e `myAdUnitId` aos [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para anúncios intersticiais.

    > [!NOTE]
    > Cada **InterstitialAd** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu aplicativo para a loja, você deve [substituir esses valores por valores dinâmicos de teste](#release) do Partner Center.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Se você deseja mostrar um anúncio de *vídeo intersticial*: Aproximadamente de 30 a 60 segundos antes de precisar do anúncio, use o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType.Video** para o tipo de anúncio.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    Se você deseja mostrar um anúncio de *faixa intersticial*: aproximadamente de 5 a 8 segundos antes de precisar do anúncio, use o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType.Display** para o tipo de anúncio.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  No momento em que você deseja mostrar o vídeo intersticial ou anúncio em faixa intersticial, confirme que o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

As instruções a seguir pressupõem que você tenha criado um projeto Universal do Windows para JavaScript no Visual Studio e esteja segmentando uma CPU específica. Para obter um exemplo de código completo, consulte [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Abra seu projeto no Visual Studio.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência ao SDK do Microsoft Advertising em seu projeto:

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).
    3.  No **Gerenciador de Referências**, clique em OK.

3.  Na seção **&lt;head&gt;**, do arquivo HTML no projeto, após as referências JavaScript de default.css e default.js do projeto, adicione a referência a ad.js.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Em um arquivo .js no projeto, declare um objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) e vários campos que contenham a ID do aplicativo e a ID da unidade do anúncio intersticial. O exemplo de código a seguir atribui os campos `applicationId` e `adUnitId` aos [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para anúncios intersticiais.

    > [!NOTE]
    > Cada **InterstitialAd** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu aplicativo para a loja, você deve [substituir esses valores por valores dinâmicos de teste](#release) do Partner Center.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Se você deseja mostrar um anúncio de *vídeo intersticial*: Aproximadamente de 30 a 60 segundos antes de precisar do anúncio, use o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **InterstitialAdType.video** para o tipo de anúncio.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    Se você deseja mostrar um anúncio de *faixa intersticial*: aproximadamente de 5 a 8 segundos antes de precisar do anúncio, use o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **InterstitialAdType.display** para o tipo de anúncio.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (interoperabilidade DirectX)

Este exemplo pressupõe que você tenha criado um projeto **DirectX and XAML App (Universal Windows)** em C++ no Visual Studio e esteja segmentando uma arquitetura de CPU específica.
 
1. Abra seu projeto no Visual Studio.

3. Adicione uma referência ao SDK do Microsoft Advertising em seu projeto:

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
    3.  No **Gerenciador de Referências**, clique em OK.

2.  Em um arquivo de cabeçalho indicado para o aplicativo (por exemplo, DirectXPage.xaml. h), declare um objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) e os métodos do manipulador de eventos relacionados.  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  No mesmo arquivo de cabeçalho, declare diversos campos da sequência que representam a ID do aplicativo e a ID da unidade publicitária para o anúncio intersticial. O exemplo de código a seguir atribui os campos `myAppId` e `myAdUnitId` aos [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para anúncios intersticiais.

    > [!NOTE]
    > Cada **InterstitialAd** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu aplicativo para a loja, você deve [substituir esses valores por valores dinâmicos de teste](#release) do Partner Center.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  No arquivo .cpp em que você deseja adicionar código para mostrar um anúncio intersticial, adicione a referência de namespace a seguir. Os exemplos a seguir pressupõem que você esteja adicionando o código ao arquivo DirectXPage.xaml.cpp no aplicativo.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **InterstitialAd** e conecte os manipuladores de eventos do objeto. No exemplo a seguir, ```InterstitialAdSamplesCpp``` é o namespace do projeto; altere esse nome conforme necessário para o código.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Se você deseja mostrar um anúncio de *vídeo intersticial*: Aproximadamente de 30 a 60 segundos antes de precisar do anúncio intersticial, use o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType::Video** para o tipo de anúncio.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    Se você deseja mostrar um anúncio de *faixa intersticial*: aproximadamente de 5 a 8 segundos antes de precisar do anúncio, use o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para pré-buscar o anúncio. Isso dá tempo suficiente para solicitar e preparar o anúncio antes de precisar ser mostrado. Certifique-se de especificar **AdType::Display** para o tipo de anúncio.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  No ponto no código em que você deseja mostrar o anúncio, confirme se o **InterstitialAd** está pronto para ser mostrado e mostre-o usando o método [Mostrar](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Defina os manipuladores de eventos para o objeto **InterstitialAd**.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Compile e teste o aplicativo para confirmar se ele está mostrando anúncios de teste.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Liberar seu app com anúncios ativos

1. Verifique se o uso de anúncios intersticiais no aplicativo segue as [diretrizes para anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

2.  No Partner Center, vá para a página de [anúncios no aplicativo](../publish/in-app-ads.md) e [criar uma unidade de anúncio](set-up-ad-units-in-your-app.md#live-ad-units). Para o tipo de unidade publicitária, escolha **Vídeo intersticial** ou **Faixa intersticial**, dependendo do tipo de anúncio intersticial mostrado. Anote a ID da unidade publicitária e a ID do aplicativo.
    > [!NOTE]
    > Os valores da ID de aplicativo para unidades publicitárias de teste e unidades publicitárias dinâmicas UWP têm formatos diferentes. Valores de ID de aplicativo de teste são GUIDs. Quando você cria uma unidade de publicitária dinâmica UWP no Partner Center, o valor de ID do aplicativo para a unidade publicitária sempre corresponde a ID da loja do aplicativo (um valor de ID da loja de exemplo é semelhante a 9NBLGGH4R315).

3. Opcionalmente, você pode habilitar o controle de anúncios para o **InterstitialAd** ao definir as configurações na seção [Configurações de controle](../publish/in-app-ads.md#mediation) na página [Anúncios no app](../publish/in-app-ads.md). O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas, como Taboola e Smaato e anúncios para campanhas promocionais de aplicativos da Microsoft.

4.  No seu código, substitua os valores de unidade de anúncio de teste pelos valores dinâmicos gerados no Partner Center.

5.  [Enviar seu aplicativo](../publish/app-submissions.md) para a loja usando o Partner Center.

6.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no Partner Center.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios intersticial em seu app

Você pode usar vários controles **InterstitialAd** em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para anúncios intersticiais](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Código exemplo de anúncio intersticial em C#](interstitial-ad-sample-code-in-c.md)
* [Código de exemplo de anúncio intersticial em JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Configurar unidades publicitárias para seu app](set-up-ad-units-in-your-app.md)
