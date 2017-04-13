---
author: mcleanbyron
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: "Saiba como usar a classe AdScheduler para adicionar anúncios ao conteúdo de vídeo."
title: "Adicionar anúncios ao conteúdo de vídeo"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, vídeo, agendador, javascript"
ms.openlocfilehash: 88e0bb4ceb9cba12d1eb5857761f5b59afaa15f2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="add-advertisements-to-video-content"></a>Adicionar anúncios ao conteúdo de vídeo


Este passo a passo mostra como usar a classe [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) para adicionar anúncios ao conteúdo de vídeo em um aplicativo UWP (Plataforma Universal do Windows) que foi escrito em JavaScript com HTML.

>**Observação**&nbsp;&nbsp;No momento, este recurso é suportado somente para aplicativos UWP que foram escritos usando JavaScript com HTML.

[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) funciona tanto com mídia progressiva quanto com mídia de streaming e usa o padrão IAB Video Ad Serving Template (VAST) 2.0/3.0 e os formatos de carga útil VMAP. Usando os padrões, [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) é indiferente ao serviço de publicidade com o qual ele interage.

Publicidade para conteúdo de vídeo dependendo se o programa tem menos de 10 minutos (forma abreviada) ou mais de dez minutos (forma longa). Embora o último seja mais complicado de configurar no serviço, não existe, na verdade, nenhuma diferença na maneira como alguém escreve o código do lado do cliente. Se o [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) receber uma carga de VAST com um único anúncio em vez de um manifesto, ele será tratado como se fosse o manifesto chamado para um único anúncio de pré-lançamento (um intervalo de 00:00).

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) com o Visual Studio 2015.

* Seu projeto deve usar o controle do [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) para servir o conteúdo de vídeo no qual os anúncios serão agendados. Esse controle está disponível na coleção de bibliotecas do [TVHelpers](https://github.com/Microsoft/TVHelpers) disponibilizado pela Microsoft no GitHub.

  O exemplo a seguir mostra como declarar um [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) na marcação HTML. Normalmente, essa marcação pertence à seção `<body>` no arquivo index.html (ou em outro arquivo html apropriado para o seu projeto).

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  O exemplo a seguir mostra como estabelecer um [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) no código JavaScript.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Como usar a classe AdScheduler em seu código

1. No Visual Studio, abra o projeto ou crie um novo projeto.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência para a biblioteca do **Microsoft Advertising SDK para JavaScript** ao seu projeto.

  a. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

  b. No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).

  c. No **Gerenciador de Referências**, clique em OK.

4.  Adicione o arquivo AdScheduler.js ao seu arquivo:

  a.  No Visual Studio, clique em **Projeto** e **Gerenciar Pacotes NuGet**.

  b.  Na caixa de pesquisa, digite **Microsoft.StoreServices.VideoAdScheduler** e instale o pacote Microsoft.StoreServices.VideoAdScheduler. O arquivo AdScheduler.js é adicionado ao subdiretório ../js em seu projeto.

5.  Abra o arquivo index.html (ou outro arquivo html apropriado para o seu projeto). Na seção `<head>`, após as referências JavaScript de default.css e main.js do projeto, adicione a referência ao ad.js e adscheduler.js.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  <script src="/js/adscheduler.js"></script>
  ```

  <span/>
  > **Observação**&nbsp;&nbsp;Esta linha deve ser colocada na seção `<head>` após a inclusão do main.js; caso contrário, você encontrará um erro ao compilar seu projeto.

6.  No arquivo main.js no seu projeto, adicione o código que cria um novo objeto [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx). Passe o **MediaPlayer** que hospeda o conteúdo do vídeo. O código deve ser colocado para que ele seja executado após [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/hh440975.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Use os métodos [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) ou [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) para solicitar uma programação de anúncios do servidor e insira-a na linha do tempo do **MediaPlayer** e, em seguida, reproduza a mídia de vídeo.

  * Se você for um parceiro da Microsoft que recebeu permissão para solicitar uma programação de anúncios do servidor de anúncios da Microsoft, use [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) e especifique a ID do aplicativo e a ID da unidade de anúncio que foram fornecidas a você por seu representante da Microsoft. Esse método assume a forma de uma **Promessa**, que é uma construção assíncrona na qual dois ponteiros de função são passados para lidar com os casos de sucesso e falha, respectivamente. Para obter mais informações, consulte [Padrões assíncronos no UWP usando JavaScript](https://msdn.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript).

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

  * Para solicitar uma programação de um servidor de anúncios que não sejam da Microsoft, use [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) e passe a URL do servidor. Esse método também assume a forma de uma **Promessa**.

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    <span/>
    >**Observação**&nbsp;&nbsp;Você deve chamar a **reprodução** mesmo que a função falhe porque o [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) dirá ao **MediaPlayer** para ignorar os anúncios e ir direto para o conteúdo. Você pode ter um requisito de negócios diferente, como a inserção de um anúncio interno se um anúncio não puder ser obtido com êxito remotamente.

8.  Durante a reprodução, você pode manipular eventos adicionais que permitem que seu aplicativo acompanhe o progresso e/ou erros que possam ocorrer após o processo inicial de correspondência de anúncios. O código a seguir mostra alguns desses eventos, incluindo [onPodStart](https://msdn.microsoft.com/library/windows/apps/mt732206.aspx), [onPodEnd](https://msdn.microsoft.com/library/windows/apps/mt732205.aspx), [onPodCountdown](https://msdn.microsoft.com/library/windows/apps/mt732204.aspx), [onAdProgress](https://msdn.microsoft.com/library/windows/apps/mt732201.aspx), [onAllComplete](https://msdn.microsoft.com/library/windows/apps/mt732202.aspx) e [onErrorOccurred](https://msdn.microsoft.com/library/windows/apps/mt732203.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]
