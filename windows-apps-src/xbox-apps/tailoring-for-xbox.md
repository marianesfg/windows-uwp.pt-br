---
author: payzer
title: Práticas recomendadas para o Xbox
description: Como otimizar seu aplicativo para Xbox.
ms.author: wdg-dev-content
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1794e4b7ddf97b0b5c41ff29295892e512453a07
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "927251"
---
# <a name="xbox-best-practices"></a>Práticas recomendadas para o Xbox

Por padrão, todos os aplicativos UWP serão executados em Xbox One sem nenhum esforço extra de sua parte. No entanto, se quiser que o aplicativo brilhe, encante os clientes e concorra com as melhores experiências de aplicativo em Xbox, você deverá seguir as práticas abaixo.
  > [!NOTE]
  > Antes de começar, examine as diretrizes de design dispostas em [Projetando para TV e Xbox](../design/devices/designing-for-tv.md).   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>Para criar as melhores experiências para Xbox One

### <a name="do-turn-off-mouse-mode"></a>*Desativar:* modo mouse

Os usuários do Xbox amor seus controladores. Para otimizar a entrada do controlador, [desabilite o modo mouse](how-to-disable-mouse-mode.md) e habilite a navegação direcional (também conhecida como [foco X-Y](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction)). Cuidado com foco desvios e UI inacessível.

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*Desenhe:* um retângulo de foco apropriado para uma experiência de 10 pés

Como a maioria dos usuários do Xbox permanece sentada na sala de estar diante da TV, lembre-se de que o retângulo de foco padrão é difícil de ver a dez pés de distância. Para garantir que o elemento da interface do usuário com o foco de entrada esteja claramente visível para o usuário sempre, siga as diretrizes de [Foco visual](../design/devices/designing-for-tv.md#focus-visual). Em XAML, você terá esse comportamento gratuitamente quando o aplicativo for executado em Xbox, mas aplicativos HTML precisarão usar um estilo CSS personalizado.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*Integre-se:* à classe SystemMediaTransportControls

Os usuários do Xbox querem controlar aplicativos de mídia com o Xbox Media Remote, a Cortana (especialmente os comandos de voz "Executar" e "Pausar") e o Xbox SmartGlass. Para obter esses recursos gratuitamente, o aplicativo deve usar a classe [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.media.systemmediatransportcontrols.aspx), incluída automaticamente nos controles de mídia do Xbox. Se o aplicativo tiver controles de mídia personalizados, verifique se eles se integram à classe **SystemMediaTransportControls** para oferecer esses recursos aos usuários. Se você estiver criando um aplicativo de música em segundo plano, integre-o à classe **SystemMediaTransportControls** para garantir que os controles de música em segundo plano funcionem corretamente na guia multitarefa do Xbox.

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*Considere:* desenhar a borda da tela

Muitas TVs cortam as bordas da tela, de maneira que todo o conteúdo importante do aplicativo deva ser exibido dentro da [área de segurança da TV](../design/devices/designing-for-tv.md#tv-safe-area). A UWP usa *overscan* para manter o conteúdo dentro da área de segurança da TV, mas esse comportamento padrão pode desenhar uma borda evidenciada em torno do aplicativo. Para proporcionar a melhor experiência, desative o comportamento padrão e siga as instruções em [Como desenhar a interface para a borda da tela](turn-off-overscan.md).
> [!IMPORTANT]
  > Se você desabilitar overscan, será sua responsabilidade se certificar de que os elementos interativos e o texto permaneçam dentro da área de segurança da TV. 

### <a name="consider-use-tv-safe-colors"></a>*Considere:* o uso de cores seguras para TV

As TVs não manipulam intensidades de cores extremas, assim como fazem os monitores de computadores. Evite cores de alta intensidade no aplicativo, de maneira que os usuários não vejam efeitos de faixa estranhos ou uma imagem lavada. Além disso, lembre-se de que as diferenças entre as TVs indicam que as cores que aparente corretas na *sua* TV podem parecer muito diferentes para os usuários. Leia [as cores](../design/devices/designing-for-tv.md#colors) para compreender como fazer com que o seu aplicativo pareça ideais para todas as pessoas!

### <a name="remember-you-can-disable-scaling"></a>*Lembre-se:* você pode desabilitar a escala

Os aplicativos UWP são dimensionados automaticamente para garantir que os elementos da interface do usuário, como controles e fontes, permaneçam legíveis em todos os dispositivos. Os aplicativos que usam XAML são dimensionados em 200%, e os aplicativos que usam HTML são dimensionados em 150%. Se você quiser mais controle sobre a aparência do aplicativo no Xbox, desabilite o fator de escala padrão para usar as dimensões de pixel reais de uma HDTV (1920x1080). Observe [Como desativar a colocação em escala](disable-scaling.md) e [Dimensionamento e pixels efetivos](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling) para obter informações sobre como personalizar o aplicativo para deixá-lo incrível no Xbox.

Para obter uma prévia dessas práticas aplicadas a um aplicativo UWP, confira este vídeo!
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>Channel 9

A seguir, no [Channel 9](https://channel9.msdn.com/), uma excelente fonte de informações para a compilação de aplicativos incríveis em Xbox:

- [Compilação de excelentes aplicativos da Plataforma Universal do Windows (UWP) para Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
- [Adaptar o aplicativo para Xbox One e TV](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [Desenvolvimento de UWP 1: compilação de uma interface do usuário adaptável](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Aplicativos Web além do navegador: plataforma cruzada encontra entre dispositivos](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>Desenvolvimento de aplicativos no Xbox

O evento de **Desenvolvimento de aplicativos no Xbox** é um ponto de partida excelente para desenvolvedores de novos para criar aplicativos Xbox.

* [Observação as sessões gravadas](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [Leia as postagens de blog](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>Veja também

- [UWP no Xbox One](index.md)
- [Projetando para TV e Xbox](../design/devices/designing-for-tv.md)
