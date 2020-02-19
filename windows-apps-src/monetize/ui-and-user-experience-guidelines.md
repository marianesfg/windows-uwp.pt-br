---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Saiba mais sobre diretrizes para a interface do usuário e experiência do usuário para anúncios em aplicativos.
title: Diretrizes para a interface do usuário e experiência do usuário para anúncios
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, diretrizes, práticas recomendadas
ms.localizationpriority: medium
ms.openlocfilehash: 2ce51f1ec99b080de6483b1d703492050c7a434c
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463918"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Diretrizes para a interface do usuário e experiência do usuário para anúncios

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://aka.ms/ad-monetization-shutdown)

Este artigo fornece diretrizes para proporcionar ótimas experiências com anúncios em faixa, intersticiais e nativos em seus aplicativos. Para obter diretrizes gerais sobre como elaborar o visual dos apps, consulte [Design e interface do usuário](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Qualquer uso de publicidade em seu app deve estar em conformidade com as políticas da Microsoft Store – incluindo, sem limitação, a [política 10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (Conteúdo e Conduta de Publicidade). Em particular, a implementação de anúncios em faixa ou intersticiais em seu app deve atender aos requisitos na política da Microsoft Store [política 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content). Este artigo contém exemplos de implementações que poderiam violar essa política. Estes exemplos são fornecidos apenas para fins informativos, como uma maneira de ajudar você a entender melhor a política. Estes exemplos não são abrangentes e pode haver muitas outras maneiras de violar as políticas da Microsoft Store que não estão listadas neste artigo.

## <a name="general-best-practices"></a>Práticas recomendadas gerais

Antes de examinar nossas diretrizes para tipos diferentes de anúncios neste artigo, primeiro examine estas práticas recomendadas gerais para melhorar sua receita de anúncios.

* [Planeje cuidadosamente a veiculação dos anúncios](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). Veja as instruções relacionadas sobre como [otimizar a visualização de suas unidades de anúncio](optimize-ad-unit-viewability.md).
* [Usar anúncios de faixa como um fallback para anúncios intersticiais em vídeo](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video/).
* [Conheça os usuários para veicular anúncios segmentados melhores](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Use as bibliotecas de publicidade mais recentes](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Defina as configurações de COPPA corretas para seu aplicativo](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app/).


## <a name="guidelines-for-banner-ads"></a>Diretrizes para anúncios em faixa

As seções a seguir fornecem recomendações sobre como implementar [anúncios em faixa](banner-ads.md) em seu aplicativo usando o [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) e exemplos de implementações que violam a [política 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) das Políticas da Microsoft Store.

### <a name="best-practices"></a>Práticas recomendadas

Recomendamos que você siga estas práticas recomendadas ao implementar anúncios em faixa em seu app:

* [Use tamanhos de anúncios interativos](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes/) que se encaixem no layout do dispositivo.

* Dedique a maior parte da interface do usuário de seu app a conteúdo e controles funcionais.

* Crie publicidade em sua experiência. Dê aos seus designers um anúncio de exemplo para planejar a aparência do anúncio. Dois exemplos de anúncios bem planejados em aplicativos são o layout de anúncios como conteúdo e o layout de divisão.

  Para ver como anúncios de tamanhos diferentes serão exibidos e funcionarão em seu app durante a fase de desenvolvimento e teste, você pode usar nossas [unidades de anúncios de teste](set-up-ad-units-in-your-app.md#test-ad-units). Quando terminar os testes, lembre-se de [atualizar seu aplicativo com unidades de anúncios ativos](set-up-ad-units-in-your-app.md#live-ad-units) antes de enviar o aplicativo para certificação.

* Planeje os momentos que não haverá anúncios disponíveis. Pode haver momentos em que anúncios não estarão sendo enviados ao seu app. Disponha suas páginas de forma que elas fiquem ótimas exibindo ou não anúncios. Para obter mais informações, consulte [Processamento de erros de anúncio](error-handling-with-advertising-libraries.md).

* Se você tiver um cenário de alerta do usuário que seja tratado melhor com uma sobreposição, chame [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) durante a exibição da sobreposição e, em seguida, chame [AdControl.Resume](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) quando o cenário de alerta for concluído.

### <a name="practices-to-avoid"></a>Práticas que devem ser evitadas

Recomendamos que você evite estas práticas ao implementar anúncios em faixa em seu app:

* Não fixe a publicidade em propriedades abertas. Um anúncio não deve ser colocado na primeira propriedade aberta que você encontrar. Ele deve ser incorporado ao design geral do seu app.

* Não faça anúncios demais e sature seu app. Muitos anúncios em seu app prejudicam a aparência e a usabilidade. Você quer ganhar dinheiro com anúncios, mas não à custa do próprio app.

* Não distraia o usuário de suas tarefas principais. O foco principal deve estar sempre no app. O espaço de anúncio deve ser incorporado para que ele permaneça um foco secundário.

### <a name="examples-of-policy-violations"></a>Exemplos de violações da política

Esta seção fornece exemplos de cenários de anúncios em faixa que violam a [política 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) da Microsoft Store. Estes exemplos são fornecidos apenas para fins de instrução, como uma maneira de ajudar você a entender melhor a política. Estes exemplos não são abrangentes e pode haver muitas outras maneiras de violar a política 10.10.1 que não estejam listadas aqui.

* Fazer algo para interferir na capacidade do usuário de ver o anúncio em faixa, como alterar a opacidade do [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) ou colocar outro controle sobre o **AdControl** (sem primeiro chamar [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Exigir que os usuários cliquem em um anúncio em faixa para realizar uma tarefa em seu app ou forçá-los a clicar anúncios em faixa como resultado do design de seu app.

* Ignorar o timer de atualização mínimo interno para anúncios em faixa por qualquer meio, incluindo (mas não se limitando a) trocar objetos do [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) ou forçar uma atualização da página sem a interação do usuário.

* Usar unidades do AD Live (ou seja, unidades do AD que você obtém do Partner Center) durante o desenvolvimento e teste ou em um emulador.

* Escrever ou distribuir um código que chame serviços de anúncios por meios que não sejam as bibliotecas do Microsoft Advertising em execução no contexto de seu app.

* Interagir com interfaces não documentadas ou objetos filho criados pelas bibliotecas do Microsoft Advertising, como **WebView** ou **MediaElement**.

* Colocar anúncios em um Viewbox para reduzir o tamanho dos anúncios a fim de permitir mais anúncios em uma página do que o normal.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Diretrizes para anúncios intersticiais

Quando usados com elegância, os [anúncios intersticiais](interstitial-ads.md) podem aumentar consideravelmente a receita do aplicativo, sem afetar negativamente a satisfação do usuário. Quando usado inadequadamente, esses anúncios podem ter exatamente o efeito oposto.

As seções a seguir fornecem recomendações sobre como implementar anúncios de vídeo e banner intersticiais no aplicativo usando o [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) e exemplos de implementações que violam a [política 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) da Microsoft Store. Como você conhece seu aplicativo melhor do que ninguém, exceto no que diz respeito à política, deixamos para você tomar a decisão final. O que é mais importante ter em mente é que suas classificações de aplicativo e receita estão intimamente ligadas.

### <a name="best-practices"></a>Práticas recomendadas

Recomendamos que você siga estas práticas recomendadas ao implementar anúncios intersticiais em seu app:

* Ajuste os anúncios intersticiais no fluxo natural do app, como entre níveis de jogo.

* Associe anúncios com vantagens tangíveis, como:

    * Dicas em relação à conclusão de nível.

    * Tempo extra para repetir um nível.

    * Recursos de avatar personalizados, como uma tatuagem ou um chapéu.

* Se o aplicativo requer que um anúncio em vídeo intersticial seja visto até o final, mencione essa regra antecipadamente para que eles não fiquem surpresos com uma mensagem de erro ao pressionar o botão Fechar.

* Faça uma busca prévia do anúncio (chamando [InterstitialAd.RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)), de preferência, de 30 a 60 segundos antes do momento em que você precisa mostrá-lo.

* Assine os quatro eventos expostos na classe [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) (**Canceled**, **Completed**, **AdReady** e **ErrorOccurred**) e use-os para tomar as decisões corretas para o seu aplicativo.

* Tenha alguma experiência interna para usar no lugar de um anúncio de correspondência de servidor. Você achará isso útil em alguns cenários:

    * Modo offline, quando não é possível acessar os servidores de anúncios.

    * Quando o evento **ErrorOccurred** é acionado.

    * Se você optar por economizar largura de banda do usuário com base em [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile), existem APIs na classe **ConnectionProfile** que podem ajudar.

* Use o tempo limite padrão (30 segundos), a menos que você tenha um motivo válido para não fazê-lo. Nesse caso, não use menos do que 10 segundos. Anúncios intersticiais em vídeo levam consideravelmente mais tempo para baixar do que em banner padrão, especialmente em mercados que não têm conexões de alta velocidade.

* Preste atenção ao plano de dados do usuário. Por exemplo, não mostre ou avise o usuário antes de mostrar um vídeo intersticial em um dispositivo móvel que esteja próximo/acima de seu limite de dados. Existem APIs na classe [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) que podem ajudar.

* Melhore continuamente seu aplicativo após o envio inicial. Analise os [relatórios de anúncios](../publish/advertising-performance-report.md) e faça alterações no design para melhorar as taxas de preenchimento e conclusão de vídeos intersticiais.

### <a name="practices-to-avoid"></a>Práticas que devem ser evitadas

Recomendamos que você evite estas práticas ao implementar anúncios intersticiais em seu app:

* Não exagere. Não mostre mais anúncios do que a cada 5 minutos, a menos que o usuário explicitamente se envolva com um benefício tangível opcional, além de apenas jogar.

* Não mostre anúncios intersticiais na inicialização do aplicativo, pois os usuários podem achar que clicaram no bloco errado.

* Não mostre anúncios intersticiais ao sair. Isso é ruim, já que as taxas de conclusão estão próximas de zero.

* Não mostre dois ou mais anúncios intersticiais consecutivos. Os usuários ficarão frustrados ao verem a barra de progresso do anúncio voltar ao ponto inicial. Muitos pensarão que é apenas um bug de codificação ou de serviço de anúncios.

* Não busque um anúncio intersticial em vídeo mais de 5 minutos antes de chamar [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show). Uma boa prática de inventário maximizará a conversão de anúncios buscados previamente em impressões rentáveis.

* Não prejudique um usuário por falhas na apresentação de anúncios, como a falta de disponibilidade de anúncios. Por exemplo, se você mostrar uma opção de interface do usuário para "Assista a um anúncio para obter *xxx*", você deverá fornecer *xxx* se o usuário fizer sua parte. Duas opções a serem consideradas:

    * Não inclua a opção, a menos que o evento [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) seja acionado.

    * Faça com que o aplicativo inclua uma experiência interna que produza o mesmo benefício que um anúncio real.

* Não use anúncios intersticiais para permitir que o usuário obtenha uma vantagem competitiva em um jogo com vários jogadores. Por exemplo, não tente atrair o usuário com uma arma melhor em um jogo de tiro em primeira pessoa se ele vir um anúncio intersticial. Uma camisa personalizada no avatar do jogador está bem, desde que não forneça camuflagem!

### <a name="examples-of-policy-violations"></a>Exemplos de violações da política

Esta seção fornece exemplos de cenários de anúncios intersticiais que violam a [política 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) da Microsoft Store. Estes exemplos são fornecidos apenas para fins de instrução, como uma maneira de ajudar você a entender melhor a política. Estes exemplos não são abrangentes e pode haver muitas outras maneiras de violar a política 10.10.1 que não estejam listadas aqui.

* Colocar um elemento de interface do usuário sobre o contêiner do anúncio intersticial.

* Chamar [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) enquanto o usuário estiver envolvido com o app.

* Usar anúncios intersticiais para obter qualquer coisa que possa ser consumida como uma moeda ou negociada com outros usuários.

* Solicitar um novo anúncio intersticial no contexto do manipulador de eventos para o evento [InterstitialAd.ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Isso pode resultar em um loop infinito e pode causar problemas operacionais para o serviço de publicidade.

* Solicitar um anúncio intersticial simplesmente para ter um anúncio de backup para uma sequência de cascata de anúncios. Se você solicitar um anúncio intersticial e, em seguida, receber o evento [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready), o próximo anúncio intersticial mostrado em seu app deve ser o anúncio que está pronto para ser exibido por meio do método [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

* Usar unidades do AD Live (ou seja, unidades do AD que você obtém do Partner Center) durante o desenvolvimento e teste ou em um emulador.

* Escrever ou distribuir um código que chame serviços de anúncios por meios que não sejam as bibliotecas do Microsoft Advertising em execução no contexto de seu app.

* Interagir com interfaces não documentadas ou objetos filho criados pelas bibliotecas do Microsoft Advertising, como **WebView** ou **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Diretrizes para anúncios nativos

Os [anúncios nativos](native-ads.md) oferecem muito controle sobre como você apresenta o conteúdo publicitário para seus usuários. Siga estes requisitos e diretrizes para ajudar a garantir que a mensagem do anunciante chegue aos usuários enquanto também ajuda a evitar o fornecimento de uma experiência de anúncios nativos confusa para os usuários.

### <a name="register-the-container-for-your-native-ad"></a>Registrar o contêiner para seu anúncio nativo

Em seu código, você deve chamar o método [RegisterAdContainer](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) do objeto [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) para registrar o elemento de interface do usuário que age como um contêiner para o anúncio nativo e, opcionalmente, quaisquer controles específicos que você deseja registrar como destinos clicáveis para o anúncio. Isso é necessário para controlar corretamente as impressões e os cliques de anúncios.

Há duas sobrecargas para o método **RegisterAdContainer** que você pode usar:

* Se você quiser que o contêiner inteiro para todos os elementos de anúncio nativo individuais seja clicável, chame o método **RegisterAdContainer(FrameworkElement)** e passe o controle de contêiner para o método. Por exemplo, se você exibir todos os elementos de anúncio nativo em controles separados hospedados em um **StackPanel** e se quiser que todo o **StackPanel** seja clicável, passe o **StackPanel** para esse método.

* Se você quiser que somente determinados elementos de anúncio nativo sejam clicáveis, chave o método **RegisterAdContainer (FrameworkElement, IVector(FrameworkElement))** . Somente os controles que você passa para o segundo parâmetro serão clicáveis.

### <a name="required-native-ad-elements"></a>Elementos de anúncio nativo necessários

No mínimo, você sempre deverá mostrar os seguintes elementos de anúncio nativo fornecidos pelas propriedades do objeto [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) para o usuário em seu design de anúncio nativo. Se você falhar ao incluir esses elementos, talvez veja um desempenho ruim e resultados baixos para sua unidade publicitária.

1. Sempre exiba o título do anúncio nativo (disponível na propriedade **Title**). Forneça espaço suficiente para exibir pelo menos 25 caracteres. Se o título for mais longo, substitua o texto adicional por reticências.
2. Sempre exiba pelo menos um dos elementos a seguir para ajudar a diferenciar a experiência do anúncio nativo do resto do seu app e mostre claramente que o conteúdo é fornecido por um anunciante:
    * O ícone *ad* distinguível (disponível na propriedade **AdIcon**). Esse ícone é fornecido pela Microsoft.
    * O texto *patrocinado* (disponível na propriedade **SponsoredBy**). Esse texto é fornecido pelo anunciante.
    * Como uma alternativa para o texto *patrocinado por*, você pode optar por exibir algum outro texto que ajude a diferenciar a experiência do anúncio nativo do resto do seu app, como "Conteúdo patrocinado", "Conteúdo promocional", "Conteúdo recomendado" etc.

### <a name="user-experience"></a>Experiência de usuário

Seu anúncio nativo deve ser claramente delineado do resto do seu aplicativo e ter espaço ao redor dele para impedir cliques acidentais. Use bordas, telas de fundo diferentes ou alguma outra interface do usuário para separar o conteúdo do anúncio do resto do seu aplicativo. Tenha em mente que os cliques acidentais em anúncios não são benéficos para sua receita baseada em anúncio ou para a experiência do seu usuário final a longo prazo.

### <a name="description"></a>Descrição

Se você optar por mostrar a descrição para o anúncio (disponível na opção **Description** do objeto **NativeAdV2**), forneça espaço suficiente para exibir pelo menos 75 caracteres. Recomendamos que você use uma animação para mostrar todo o conteúdo da descrição do anúncio.

### <a name="call-to-action"></a>Plano de ação

O texto *chamada para ação* (disponível na propriedade **CallToAction** do objeto **NativeAdV2**) é um componente essencial do anúncio. Se você optar por mostrar esse texto, siga estas diretrizes:

* Sempre exiba o texto *chamada para ação* para o usuário em um controle clicável, como um botão ou um hiperlink.
* Sempre exiba o texto *chamada para ação* em sua totalidade.
* Verifique se o texto *chamada para ação* está separado do resto do texto promocional do anunciante.
