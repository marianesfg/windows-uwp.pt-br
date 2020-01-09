---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Saiba como usar a classe AdScheduler para mostrar anúncios no conteúdo de vídeo.
title: Mostrar anúncios em conteúdo de vídeo
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, vídeo, agendador, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 6178758cd67471d56b1d65e293104e987e81fb9b
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681917"
---
# <a name="show-ads-in-video-content"></a>Mostrar anúncios em conteúdo de vídeo

Este passo a passo mostra como usar a classe **AdScheduler** para mostrar anúncios em conteúdo de vídeo em um aplicativo UWP (Plataforma Universal do Windows) que foi escrito em JavaScript com HTML.

> [!NOTE]
> No momento, este recurso só tem suporte para aplicativos UWP escritos usando JavaScript com HTML.

**AdScheduler** funciona tanto com mídia progressiva quanto com mídia de streaming e usa o padrão IAB Video Ad Serving Template (VAST) 2.0/3.0 e os formatos de carga útil VMAP. Usando os padrões, **AdScheduler** é indiferente ao serviço de publicidade com o qual ele interage.

Publicidade para conteúdo de vídeo dependendo se o programa tem menos de 10 minutos (forma abreviada) ou mais de dez minutos (forma longa). Embora o último seja mais complicado de configurar no serviço, não existe, na verdade, nenhuma diferença na maneira como alguém escreve o código do lado do cliente. Se o **AdScheduler** receber uma carga de VAST com um único anúncio em vez de um manifesto, ele será tratado como se fosse o manifesto chamado para um único anúncio de pré-lançamento (um intervalo de 00:00).

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) com o Visual Studio 2015 ou uma versão posterior.

* Seu projeto deve usar o controle do [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) para servir o conteúdo de vídeo no qual os anúncios serão agendados. Esse controle está disponível na coleção de bibliotecas do [TVHelpers](https://github.com/Microsoft/TVHelpers) disponibilizado pela Microsoft no GitHub.

  O exemplo a seguir mostra como declarar um [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) na marcação HTML. Normalmente, essa marcação pertence à seção `<body>` no arquivo index.html (ou em outro arquivo html apropriado para o seu projeto).

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  O exemplo a seguir mostra como estabelecer um [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) no código JavaScript.

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Como usar a classe AdScheduler em seu código

1. No Visual Studio, abra o projeto ou crie um novo projeto.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência para a biblioteca do **SDK do Microsoft Advertising para JavaScript** ao seu projeto.

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2. No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).
    3. No **Gerenciador de Referências**, clique em OK.

4.  Adicione o arquivo AdScheduler.js ao seu arquivo:

    1. No Visual Studio, clique em **Projeto** e **Gerenciar Pacotes NuGet**.
    2. Na caixa de pesquisa, digite **Microsoft.StoreServices.VideoAdScheduler** e instale o pacote Microsoft.StoreServices.VideoAdScheduler. O arquivo AdScheduler.js é adicionado ao subdiretório ../js em seu projeto.

5.  Abra o arquivo index.html (ou outro arquivo html apropriado para o seu projeto). Na seção `<head>`, após as referências JavaScript de default.css e main.js do projeto, adicione a referência ao ad.js e adscheduler.js.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > Esta linha deve ser colocada na seção `<head>` após a inclusão do main.js; caso contrário, você encontrará um erro ao compilar seu projeto.

6.  No arquivo main.js no seu projeto, adicione o código que cria um novo objeto **AdScheduler**. Passe o **MediaPlayer** que hospeda o conteúdo do vídeo. O código deve ser colocado para que ele seja executado após [WinJS.UI.processAll](https://docs.microsoft.com/previous-versions/windows/apps/hh440975).

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Use os métodos **requestSchedule** ou **requestScheduleByUrl** do objeto **AdScheduler** para solicitar uma programação de anúncios do servidor e insira-a na linha do tempo do **MediaPlayer** e, em seguida, reproduza a mídia de vídeo.

    * Se você for um parceiro da Microsoft que recebeu permissão para solicitar uma programação de anúncios do servidor de anúncios da Microsoft, use **requestSchedule** e especifique a ID do aplicativo e a ID da unidade de anúncio que foram fornecidas a você por seu representante da Microsoft.

        Esse método assume a forma de uma [Promise](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript), que é uma construção assíncrona onde dois ponteiros de função são passados: um ponteiro para a função **onComplete**, chamada quando a promessa é concluída com êxito e um ponteiro para a função **onError**, chamada se houver um erro. Na função **onComplete**, inicie a reprodução do conteúdo de vídeo. O anúncio começará a ser reproduzido no horário agendado. Em sua função **onError**, manipular o erro e, em seguida, inicie a reprodução do vídeo. O conteúdo do vídeo será reproduzido sem um anúncio. O argumento da função **onError** é um objeto que contém os membros a seguir.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Para solicitar uma programação de um servidor de anúncios que não sejam da Microsoft, use **requestScheduleByUrl** e passe a URI do servidor. Esse método também assume a forma de uma **Promise** que aceita ponteiros para as funções **onComplete** e **onError**. A carga de anúncio que é retornada do servidor deve estar em conformidade com os formatos de carga VAST (Video Ad Serving Template) ou VMAP (Video Multiple Ad Playlist).

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > Você deve aguardar **requestSchedule** ou **requestScheduleByUrl** para retornar antes de começar a reproduzir o conteúdo de vídeo principal no **MediaPlayer**. Começar a reproduzir mídia antes de **requestSchedule** retorna (no caso de um anúncio de pré-lançamento) resultará no pré-lançamento interrompendo o conteúdo do vídeo principal. Você deve chamar a **reprodução** mesmo que a função falhe porque o **AdScheduler** dirá ao **MediaPlayer** para ignorar os anúncios e ir direto para o conteúdo. Você pode ter um requisito de negócios diferente, como a inserção de um anúncio interno se um anúncio não puder ser obtido com êxito remotamente.

8.  Durante a reprodução, você pode manipular eventos adicionais que permitem que seu aplicativo acompanhe o progresso e/ou erros que possam ocorrer após o processo inicial de correspondência de anúncios. O código a seguir mostra alguns desses eventos, incluindo **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** e **onErrorOccurred**.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>Membros de AdScheduler

Esta seção fornece alguns detalhes sobre os membros do objeto **AdScheduler**. Para saber mais sobre esses membros, consulte os comentários e as definições no arquivo AdScheduler.js no seu projeto.

### <a name="requestschedule"></a>requestSchedule

Esse método solicita um cronograma de anúncio do servidor de anúncios da Microsoft e o insere na linha do tempo do **MediaPlayer** que foi passado para o construtor **AdScheduler**.

O terceiro parâmetro opcional (*adTags*) é uma coleção JSON de pares de nome/valor que podem ser usados para apps com segmentação avançada. Por exemplo, um app que reproduz uma variedade de vídeos autorrelacionados pode complementar a ID da unidade publicitária com a marca e modelo do carro sendo mostrado. Esse parâmetro se destina a ser usado apenas pelos parceiros que receberem aprovação da Microsoft para usar marcas de anúncios.

Os itens a seguir devem ser observados ao se fazer referência a *adTags*:

* Esse parâmetro é uma opção muito raramente usada. O fornecedor deve trabalhar em conjunto com a Microsoft antes de usar adTags.
* Os nomes e os valores devem ser predeterminados no serviço de publicidade. As marcas de anúncios não são termos de pesquisa ou palavras-chave de extremidade aberta.
* O número máximo de marcas com suporte é 10.
* Os nomes de marcas são restritos a 16 caracteres.
* Os valores de marca têm no máximo 128 caracteres.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

Esse método solicita um cronograma de anúncio do servidor de anúncios não Microsoft especificado no URI e o insere na linha do tempo do **MediaPlayer** que foi passado para o construtor **AdScheduler**. A carga de anúncio que é retornada pelo servidor de anúncios deve estar em conformidade com os formatos de carga VAST (Video Ad Serving Template) ou VMAP (Video Multiple Ad Playlist).

### <a name="mediatimeout"></a>mediaTimeout

Essa propriedade obtém ou define o número de milissegundos que a mídia permanecer reproduzível. O valor 0 informa o sistema para nunca atingir o tempo limite. O padrão é 30000 ms (30 segundos).

### <a name="playskippedmedia"></a>playSkippedMedia

Essa propriedade obtém ou define um valor **booliano** que indica se a mídia agendada reproduzirá se o usuário pular para um ponto além de uma hora de início agendada.

O cliente de anúncio e o media player imporão regras em termos de o que acontecerá aos anúncios durante o avanço rápido e o retrocesso do conteúdo do vídeo principal. Na maioria dos casos, os desenvolvedores de aplicativos não permitem que os anúncios sejam totalmente ignorados, mas querem fornecer uma experiência razoável para o usuário. As duas opções a seguir se enquadram nas necessidades da maioria dos desenvolvedores:

1. Permitir que os usuários finais ignorem pods de anúncio à vontade.
2. Permitir que os usuários ignorem pods de anúncio, mas reproduzam o pod mais recente quando a reprodução for retomada.

A propriedade **playSkippedMedia** tem as seguintes condições:

* Os anúncios não podem ser ignorados ou rapidamente avançados assim que começarem.
* Todos os anúncios em um pod de anúncios serão reproduzidos assim que o pod for iniciado.
* Uma vez reproduzido, um anúncio não será reproduzido novamente durante o conteúdo principal (filme, episódio etc.); os marcadores de anúncio serão marcados como reproduzidos ou removidos.
* Os anúncios pré-distribuição não podem ser ignorados.

Ao retomar conteúdo que contenha publicidade, defina **playSkippedMedia** como **false** ignorar para ignorar pré-distribuições e impedir que o intervalo comercial mais recente seja reproduzido. Em seguida, depois que o conteúdo for iniciado, defina **playSkippedMedia** como **true** para garantir que os usuários não poderão avançar para os anúncios subsequentes.

> [!NOTE]
> Uma pod é um grupo de anúncios reproduzido em uma sequência, como um grupo de anúncios que é reproduzido durante um intervalo comercial. Para obter mais detalhes, consulte a especificação VAST (Digital Video Ad Serving Template) do IAB.

### <a name="requesttimeout"></a>requestTimeout

Essa propriedade obtém ou define o número de milissegundos para aguardar uma resposta de solicitação de anúncio antes do tempo limite. O valor 0 informa o sistema para nunca atingir o tempo limite. O padrão é 30000 ms (30 segundos).

### <a name="schedule"></a>programa

Essa propriedade obtém os dados de agendamento recuperados do servidor de anúncios. Esse objeto inclui a hierarquia completa dos dados que corresponde à estrutura da carga do VAST (Video Ad Serving Template) ou do VMAP (Video Multiple Ad Playlist).

### <a name="onadprogress"></a>onAdProgress  

Esse evento é acionado quando a reprodução do anúncio atinge pontos de verificação de quartil. O segundo parâmetro do manipulador de eventos (*eventInfo*) é um objeto JSON com os membros a seguir:

* **progress**: o status de reprodução do anúncio (um dos valores da enumeração **MediaProgress** definidos em AdScheduler.js).
* **clip**: o clipe de vídeo que está sendo reproduzido. Essa objeto não deve ser utilizado em seu código.
* **adPackage**: um objeto que representa a parte da carga do anúncio que corresponde ao anúncio que está sendo reproduzido. Essa objeto não deve ser utilizado em seu código.

### <a name="onallcomplete"></a>onAllComplete  

Esse evento é gerado quando o conteúdo principal atinge o final e algum anúncio pós-distribuição agendada também é encerrado.

### <a name="onerroroccurred"></a>onErrorOccurred  

Esse evento é gerado quando o **AdScheduler** encontra um erro. Para obter mais informações sobre os valores de código de erro, consulte [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onPodCountdown

Esse evento é gerado quando um anúncio está sendo reproduzido e indica o tempo restante no pod atual. O segundo parâmetro do manipulador de eventos (*eventData*) é um objeto JSON com os membros a seguir:

* **remainingAdTime**: o número de segundos restantes para o anúncio atual.
* **remainingPodTime**: o número de segundos restantes para o pod atual.

> [!NOTE]
> Uma pod é um grupo de anúncios reproduzido em uma sequência, como um grupo de anúncios que é reproduzido durante um intervalo comercial. Para obter mais detalhes, consulte a especificação VAST (Digital Video Ad Serving Template) do IAB.

### <a name="onpodend"></a>onPodEnd  

Esse evento é gerado quando um pod de anúncios termina. O segundo parâmetro do manipulador de eventos (*eventData*) é um objeto JSON com os membros a seguir:

* **startTime**: a hora de início do pod, em segundos.
* **pod**: um objeto que representa o pod. Essa objeto não deve ser utilizado em seu código.

### <a name="onpodstart"></a>onPodStart

Esse evento é gerado quando um pod de anúncios inicia. O segundo parâmetro do manipulador de eventos (*eventData*) é um objeto JSON com os membros a seguir:

* **startTime**: a hora de início do pod, em segundos.
* **pod**: um objeto que representa o pod. Essa objeto não deve ser utilizado em seu código.
