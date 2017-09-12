---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "Saiba mais sobre diretrizes para a interface do usuário e experiência do usuário para anúncios em aplicativos."
title: "Diretrizes para a interface do usuário e experiência do usuário para anúncios"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, diretrizes, práticas recomendadas"
ms.openlocfilehash: 8dc9c00bdeb47b5f07af0b9b27ef843e2afd4456
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2017
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Diretrizes para a interface do usuário e experiência do usuário para anúncios

Este artigo fornece diretrizes para proporcionar ótimas experiências com anúncios em faixa e intersticiais em seus apps. Para obter diretrizes gerais sobre como elaborar o visual dos apps, consulte [Design e interface do usuário](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Qualquer uso de publicidade em seu app deve estar em conformidade com as políticas da Windows Store – incluindo, sem limitação, a [política 10.10](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) (Conteúdo e Conduta de Publicidade). Em particular, a implementação de anúncios em faixa ou intersticiais em seu app deve atender aos requisitos na política da Windows Store [política 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10). Este artigo contém exemplos de implementações que poderiam violar essa política. Estes exemplos são fornecidos apenas para fins informativos, como uma maneira de ajudar você a entender melhor a política. Estes exemplos não são abrangentes e pode haver muitas outras maneiras de violar as políticas da Windows Store que não estão listadas neste artigo.

## <a name="guidelines-for-banner-ads"></a>Diretrizes para anúncios em faixa

As seções a seguir fornecem recomendações sobre como implementar anúncios em faixa em seu app usando o [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e exemplos de implementações que violam a [política 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) da Windows Store.

### <a name="best-practices"></a>Práticas recomendadas

Recomendamos que você siga estas práticas recomendadas ao implementar anúncios em faixa em seu app:

* Dedique a maior parte da interface do usuário de seu app a conteúdo e controles funcionais.

* Crie publicidade em sua experiência. Dê aos seus designers um anúncio de exemplo para planejar a aparência do anúncio. Dois exemplos de anúncios bem planejados em aplicativos são o layout de anúncios como conteúdo e o layout de divisão.

  Para ver como anúncios de tamanhos diferentes serão exibidos e funcionarão em seu app durante a fase de desenvolvimento e teste, você pode usar nossas [unidades de anúncios no modo de teste](test-mode-values.md). Quando terminar os testes, lembre-se de [atualizar seu app com IDs de unidades de anúncios reais](set-up-ad-units-in-your-app.md) antes de enviar o app para certificação.

* Use [tamanhos de anúncios](supported-ad-sizes-for-banner-ads.md) que se encaixem no layout do dispositivo de execução.

* Planeje os momentos que não haverá anúncios disponíveis. Pode haver momentos em que anúncios não estarão sendo enviados ao seu app. Disponha suas páginas de forma que elas fiquem ótimas exibindo ou não anúncios. Para obter mais informações, consulte [Tratamento de erros](error-handling-with-advertising-libraries.md).

* Se você tiver um cenário de alerta do usuário que seja tratado melhor com uma sobreposição, chame [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx) durante a exibição da sobreposição e, em seguida, chame [AdControl.Resume](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.resume.aspx) quando o cenário de alerta for concluído.

<span />
### <a name="practices-to-avoid"></a>Práticas que devem ser evitadas

Recomendamos que você evite estas práticas ao implementar anúncios em faixa em seu app:

* Não fixe a publicidade em propriedades abertas. Um anúncio não deve ser colocado na primeira propriedade aberta que você encontrar. Ele deve ser incorporado ao design geral do seu app.

* Não faça anúncios demais e sature seu app. Muitos anúncios em seu app prejudicam a aparência e a usabilidade. Você quer ganhar dinheiro com anúncios, mas não à custa do próprio app.

* Não distraia o usuário de suas tarefas principais. O foco principal deve estar sempre no app. O espaço de anúncio deve ser incorporado para que ele permaneça um foco secundário.

<span />
### <a name="examples-of-policy-violations"></a>Exemplos de violações da política

Esta seção fornece exemplos de cenários de anúncios em faixa que violam a [política 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) da Windows Store. Estes exemplos são fornecidos apenas para fins de instrução, como uma maneira de ajudar você a entender melhor a política. Estes exemplos não são abrangentes e pode haver muitas outras maneiras de violar a política 10.10.1 que não estejam listadas aqui.

* Fazer algo para interferir na capacidade do usuário de ver o anúncio em faixa, como alterar a opacidade do [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou colocar outro controle sobre o **AdControl** (sem primeiro chamar [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)).

* Exigir que os usuários cliquem em um anúncio em faixa para realizar uma tarefa em seu app ou forçá-los a clicar anúncios em faixa como resultado do design de seu app.

* Ignorar o timer de atualização mínimo interno para anúncios em faixa por qualquer meio, incluindo (mas não se limitando a) trocar objetos do [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou forçar uma atualização da página sem a interação do usuário.

* Usar unidades de anúncio em tempo real (ou seja, unidades de anúncio que você obtém do painel do Centro de Desenvolvimento do Windows) durante a fase de desenvolvimento e teste ou em um emulador.

* Escrever ou distribuir um código que chame serviços de anúncios por meios que não sejam as bibliotecas do Microsoft Advertising em execução no contexto de seu app.

* Interagir com interfaces não documentadas ou objetos filho criados pelas bibliotecas do Microsoft Advertising, como **WebView** ou **MediaElement**.

<span id="interstitialbestpractices10">
## <a name="guidelines-for-interstitial-ads"></a>Diretrizes para anúncios intersticiais

Quando usados com elegância, os anúncios intersticiais podem aumentar consideravelmente a receita do app, sem afetar negativamente a satisfação do usuário. Quando usado inadequadamente, esses anúncios podem ter exatamente o efeito oposto.

As seções a seguir fornecem recomendações sobre como implementar anúncios de vídeo e banner intersticiais no aplicativo usando o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) e exemplos de implementações que violam a [política 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) da Windows Store. Como você conhece seu aplicativo melhor do que ninguém, exceto no que diz respeito à política, deixamos para você tomar a decisão final. O que é mais importante ter em mente é que suas classificações de aplicativo e receita estão intimamente ligadas.

### <a name="best-practices"></a>Práticas recomendadas

Recomendamos que você siga estas práticas recomendadas ao implementar anúncios intersticiais em seu app:

* Ajuste os anúncios intersticiais no fluxo natural do app, como entre níveis de jogo.

* Associe anúncios com vantagens tangíveis, como:

    * Dicas em relação à conclusão de nível.

    * Tempo extra para repetir um nível.

    * Recursos de avatar personalizados, como uma tatuagem ou um chapéu.

* Se o aplicativo requer que um anúncio em vídeo intersticial seja visto até o final, mencione essa regra antecipadamente para que eles não fiquem surpresos com uma mensagem de erro ao pressionar o botão Fechar.

* Faça uma busca prévia do anúncio (chamando [InterstitialAd.RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)), de preferência, de 30 a 60 segundos antes do momento em que você precisa mostrá-lo.

* Assine os quatro eventos expostos na classe [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) (**Canceled**, **Completed**, **AdReady** e **ErrorOccurred**) e use-os para tomar as decisões corretas para o seu aplicativo.

* Tenha alguma experiência interna para usar no lugar de um anúncio de correspondência de servidor. Você achará isso útil em alguns cenários:

    * Modo offline, quando não é possível acessar os servidores de anúncios.

    * Quando o evento **ErrorOccurred** é acionado.

    * Se você optar por economizar largura de banda do usuário com base em [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), existem APIs na classe **ConnectionProfile** que podem ajudar.

* Use o tempo limite padrão (30 segundos), a menos que você tenha um motivo válido para não fazê-lo. Nesse caso, não use menos do que 10 segundos. Anúncios intersticiais em vídeo levam consideravelmente mais tempo para baixar do que em banner padrão, especialmente em mercados que não têm conexões de alta velocidade.

<span/>

* Preste atenção ao plano de dados do usuário. Por exemplo, não mostre ou avise o usuário antes de mostrar um vídeo intersticial em um dispositivo móvel que esteja próximo/acima de seu limite de dados. Existem APIs na classe [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) que podem ajudar.

* Melhore continuamente seu aplicativo após o envio inicial. Analise os [relatórios de anúncios](../publish/advertising-performance-report.md) e faça alterações no design para melhorar as taxas de preenchimento e conclusão de vídeos intersticiais.

<span />
### <a name="practices-to-avoid"></a>Práticas que devem ser evitadas

Recomendamos que você evite estas práticas ao implementar anúncios intersticiais em seu app:

* Não exagere. Não mostre mais anúncios do que a cada 5 minutos, a menos que o usuário explicitamente se envolva com um benefício tangível opcional, além de apenas jogar.

* Não mostre anúncios intersticiais na inicialização do aplicativo, pois os usuários podem achar que clicaram no bloco errado.

* Não mostre anúncios intersticiais ao sair. Isso é ruim, já que as taxas de conclusão estão próximas de zero.

* Não mostre dois ou mais anúncios intersticiais consecutivos. Os usuários ficarão frustrados ao verem a barra de progresso do anúncio voltar ao ponto inicial. Muitos pensarão que é apenas um bug de codificação ou de serviço de anúncios.

* Não busque um anúncio intersticial em vídeo mais de 5 minutos antes de chamar [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx). Uma boa prática de inventário maximizará a conversão de anúncios buscados previamente em impressões rentáveis.

* Não prejudique um usuário por falhas na apresentação de anúncios, como a falta de disponibilidade de anúncios. Por exemplo, se você mostrar uma opção de interface do usuário para "Assista a um anúncio para obter *xxx*", você deverá fornecer *xxx* se o usuário fizer sua parte. Duas opções a serem consideradas:

    * Não inclua a opção, a menos que o evento [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) seja acionado.

    * Faça com que o aplicativo inclua uma experiência interna que produza o mesmo benefício que um anúncio real.

* Não use anúncios intersticiais para permitir que o usuário obtenha uma vantagem competitiva em um jogo com vários jogadores. Por exemplo, não tente atrair o usuário com uma arma melhor em um jogo de tiro em primeira pessoa se ele vir um anúncio intersticial. Uma camisa personalizada no avatar do jogador está bem, desde que não forneça camuflagem!

<span />
### <a name="examples-of-policy-violations"></a>Exemplos de violações da política

Esta seção fornece exemplos de cenários de anúncios intersticiais que violam a [política 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) da Windows Store. Estes exemplos são fornecidos apenas para fins de instrução, como uma maneira de ajudar você a entender melhor a política. Estes exemplos não são abrangentes e pode haver muitas outras maneiras de violar a política 10.10.1 que não estejam listadas aqui.

* Colocar um elemento de interface do usuário sobre o contêiner do anúncio intersticial.

* Chamar [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) enquanto o usuário estiver envolvido com o app.

* Usar anúncios intersticiais para obter qualquer coisa que possa ser consumida como uma moeda ou negociada com outros usuários.

* Solicitar um novo anúncio intersticial no contexto do manipulador de eventos para o evento [InterstitialAd.ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx). Isso pode resultar em um loop infinito e pode causar problemas operacionais para o serviço de publicidade.

* Solicitar um anúncio intersticial simplesmente para ter um anúncio de backup para uma sequência de cascata de anúncios. Se você solicitar um anúncio intersticial e, em seguida, receber o evento [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx), o próximo anúncio intersticial mostrado em seu app deve ser o anúncio que está pronto para ser exibido por meio do método [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

* Usar unidades de anúncio em tempo real (ou seja, unidades de anúncio que você obtém do painel do Centro de Desenvolvimento do Windows) durante a fase de desenvolvimento e teste ou em um emulador.

* Escrever ou distribuir um código que chame serviços de anúncios por meios que não sejam as bibliotecas do Microsoft Advertising em execução no contexto de seu app.

* Interagir com interfaces não documentadas ou objetos filho criados pelas bibliotecas do Microsoft Advertising, como **WebView** ou **MediaElement**.

 

 
