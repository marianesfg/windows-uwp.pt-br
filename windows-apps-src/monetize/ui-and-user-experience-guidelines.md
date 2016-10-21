---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "Saiba mais sobre diretrizes para a interface do usuário e experiência do usuário para anúncios em aplicativos."
title: "Diretrizes para a interface do usuário e experiência do usuário para anúncios em aplicativos"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d464a2de442e6f1833f429c8460c27bf85e577d1


---

# Diretrizes para a interface do usuário e experiência do usuário para anúncios em aplicativos




## Recursos gerais da interface do usuário para aplicativos do Windows

Você pode encontrar informações sobre como projetar a aparência para aplicativos em [Design e interface do usuário](https://developer.microsoft.com/windows/design).

## Práticas recomendadas para o AdControl

* [Práticas recomendadas para o AdControl: O QUE FAZER](#adcontrolbestpracticesdo10)
* [Práticas recomendadas para o AdControl: O QUE NÃO FAZER](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### Práticas recomendadas para o AdControl: O QUE FAZER

* Crie publicidade em sua experiência. Dê aos seus designers um anúncio de exemplo para planejar a aparência do anúncio. Dois exemplos de anúncios bem planejados em aplicativos são o layout de anúncios como conteúdo e o layout de divisão.

  Para ver como anúncios de tamanhos diferentes serão exibidos e funcionarão em seu aplicativo, você pode usar nossas unidades de anúncios no modo de teste para Windows Phone, Windows 8.1 e Windows 10. Quando terminar com as unidades de anúncios no modo de teste, lembre-se de [atualizar seu aplicativo com IDs](set-up-ad-units-in-your-app.md) de unidades de anúncios reais do painel do Centro de Desenvolvimento do Windows antes de enviar o aplicativo para certificação.

* Planeje os momentos que não haverá anúncios disponíveis. Pode haver momentos em que anúncios não estarão sendo enviados ao seu aplicativo. Disponha suas páginas de forma que elas fiquem ótimas exibindo ou não anúncios. Para obter mais informações, consulte [Tratamento de erros](error-handling-with-advertising-libraries.md).

<span id="adcontrolbestpracticesdont10"/>
### Práticas recomendadas para o AdControl: O QUE NÃO FAZER

* Publicidade em propriedades abertas. Um anúncio não deve ser colocado na primeira propriedade aberta que você encontrar. Ele deve ser incorporado ao design geral do seu aplicativo.

* Fazer anúncios demais e saturar seu aplicativo. Muitos anúncios em seu aplicativo prejudicam a aparência e a usabilidade. Você quer ganhar dinheiro com anúncios, mas não à custa do próprio aplicativo.

* Distrair o usuário de suas tarefas principais. O foco principal deve estar sempre no aplicativo. O espaço de anúncio deve ser incorporado para que ele permaneça um foco secundário.

<span id="interstitialbestpractices10"/>
## Práticas recomendadas para anúncios intersticiais

* [Práticas recomendadas para anúncios intersticiais: O QUE FAZER](#interstitialbestpracticesdo10)
* [Práticas recomendadas para anúncios intersticiais: O QUE EVITAR](#interstitialbestpracticesavoid10)
* [Práticas recomendadas para anúncios intersticiais: NUNCA (política imposta)](#interstitialbestpracticesnever10)

Quando usado com elegância, anúncios intersticiais em vídeo podem aumentar consideravelmente a receita do aplicativo, sem afetar negativamente a satisfação do usuário. Quando usado inadequadamente, esses anúncios podem ter exatamente o efeito oposto.

Aqui, nosso objetivo é ajudar você a atingir a elegância. Como você conhece seu aplicativo melhor do que ninguém, exceto no que diz respeito à política, deixamos para você tomar a decisão final. O que é mais importante ter em mente é que suas classificações de aplicativo e receita estão intimamente ligadas.

<span id="interstitialbestpracticesdo10"/>
### Práticas recomendadas para anúncios intersticiais: O QUE FAZER

* Ajuste os anúncios intersticiais no fluxo natural do aplicativo, como entre níveis de jogo.

* Associe anúncios com vantagens tangíveis, como:

    * Dicas em relação à conclusão de nível.

    * Tempo extra para repetir um nível.

    * Recursos de avatar personalizados, como uma tatuagem ou um chapéu.

* Se seu aplicativo requer que um anúncio em vídeo seja visto até o final, mencione essa regra antecipadamente para que eles não fiquem surpresos com uma mensagem de erro ao pressionar o botão Fechar.

* Pré-busque o anúncio (chamando [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)), de preferência, de 30 a 60 segundos antes do momento em que você precisa mostrá-lo.

* Assine os quatro eventos expostos na classe [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) (**Canceled**, **Completed**, **AdReady** e **ErrorOccurred**) e use-os para tomar as decisões corretas para o seu aplicativo.

* Tenha alguma experiência interna para usar no lugar de um anúncio de correspondência de servidor. Você achará isso útil em alguns cenários:

    * Modo offline, quando não é possível acessar os servidores de anúncios.

    * Quando o evento **ErrorOccurred** é acionado.

    * Se você optar por economizar largura de banda do usuário com base em [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), existem APIs na classe **ConnectionProfile** que podem ajudar.

* Use o tempo limite padrão (30 s), a menos que você tenha um motivo válido para não fazê-lo. Nesse caso, não use menos do que 10 s.

    * Anúncios em vídeo levam consideravelmente mais tempo para baixar do que em banner, especialmente em mercados que não têm conexões de alta velocidade.


* Preste atenção ao plano de dados do usuário. Por exemplo, não mostre ou aviso o usuário antes de mostrar um vídeo em um dispositivo móvel que esteja próximo/acima de seu limite de dados. Existem APIs na classe [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) que podem ajudar.

* Melhore continuamente seu aplicativo após o envio inicial. Analise os relatórios de anúncios e faça alterações no design para melhorar as taxas de preenchimento e conclusão de vídeos.

<span id="interstitialbestpracticesavoid10"/>
### Práticas recomendadas para anúncios intersticiais: O QUE EVITAR

* Excesso. Não mostre mais anúncios do que a cada 5 minutos, a menos que o usuário explicitamente se envolva com um benefício tangível opcional, além de apenas jogar.

* Anúncios intersticiais em vídeo na inicialização do aplicativo, pois os usuários podem achar que clicaram no bloco errado.

* Anúncios intersticiais em vídeo ao sair. Isso é ruim, já que as taxas de conclusão estão próximas de zero.

    * Dois ou mais anúncios em vídeo consecutivos.

    * Os usuários ficarão frustrados ao verem a barra de progresso do anúncio voltar ao ponto inicial.

    * Muitos pensarão que é apenas um bug de codificação ou de serviço de anúncios.

* Buscar um anúncio em vídeo mais do que 5 minutos antes de chamar [Mostrar](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

    * Uma boa prática maximizará a conversão de anúncios pré-obtidos em impressões faturáveis.


* Prejudicar um usuário por falhas na apresentação de anúncios, como a falta de disponibilidade de anúncios. Por exemplo, se você mostrar uma opção de interface do usuário para "Assista a um anúncio para obter *xxx*", você deverá fornecer *xxx* se o usuário fizer sua parte. Duas opções a serem consideradas:

    * Não inclua a opção, a menos que o evento [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) seja acionado.

    * Faça com que o aplicativo inclua uma experiência interna que produza o mesmo benefício que um anúncio real.

* Permitir que um usuário obtenha uma vantagem competitiva em um jogo com vários jogadores.

    * Uma arma melhor em um jogo de tiro em primeira pessoa claramente se enquadraria nessa classificação.

    * Uma camisa personalizada no avatar do jogador está bem, desde que não forneça camuflagem!

<span id="interstitialbestpracticesnever10"/>
### Práticas recomendadas para anúncios intersticiais: NUNCA (política imposta)

* Coloque todos os elementos da interface do usuário sobre o contêiner de anúncios.

    * Os anunciantes pagaram pela tela inteira.


* Chame **Mostrar** enquanto o usuário está envolvido com o aplicativo.

    * Como o **InterstitialAd** criará uma sobreposição de tela inteira, o usuário achará isso dissonante.

    * Também pode levar a taxas de cliques exageradas.

* Use anúncios para obter qualquer coisa que possa ser consumida como uma moeda ou negociada com outros usuários.

 

 



<!--HONumber=Aug16_HO3-->


