---
title: Novidades do Windows Docs em julho de 2017 - Desenvolver aplicativos UWP
description: Novos recursos, amostras e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 referente a julho de 2017.
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dd00d1aba0e6a58cd2128c90b2c7f714d3f8f802
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8198910"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>Novidades dos documentos de desenvolvedor do Windows em julho de 2017

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Disponibilizaram-se as visões gerais de recurso, as diretrizes para desenvolvedores e os exemplos de código a seguir, contendo informações novas e atualizadas para desenvolvedores do Windows.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/your-first-app.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="fluent-design"></a>Design Fluent

Disponíveis para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Compilações de Prévia SDK, esses novos efeitos usam profundidade, perspectiva e movimento para ajudar os usuários a se concentrarem nos elementos importantes da interface de usuário.

[Material acrílico](../design/style/acrylic.md) é um tipo de pincel que cria texturas transparentes. 

![Acrílico em tema claro](../design/style/images/Acrylic_DarkTheme_Base.png)

O [efeito Paralaxe](../design/motion/parallax.md) adiciona profundidade tridimensional e perspectiva ao seu aplicativo.

![Um exemplo de paralaxe com uma lista e uma imagem de fundo](../design/style/images/_Parallax_v2.gif)

[Revelação](../design/style/reveal.md) destaca os elementos importantes do seu aplicativo. 

![Visual do Revelação](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>Controles de interface de usuário

Disponíveis para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Compilações de Prévia SDK, esses novos controles permitem criar, de forma fácil e rápida, uma interface de usuário com ótima aparência.

O [controle do seletor de cores](../design/controls-and-patterns/color-picker.md) permite que os usuários naveguem e selecionem cores.  

![Um seletor de cores padrão](../design/controls-and-patterns/images/color-picker-default.png)

O [controle de modo de exibição de navegação](../design/controls-and-patterns/navigationview.md) torna mais fácil adicionar navegação de ponta ao seu aplicativo.

![Seções do NavigationView](../design/controls-and-patterns/images/navview_sections.png)

O [controle de imagem da pessoa](../design/controls-and-patterns/person-picture.md) exibe a imagem de avatar para uma pessoa.

![O controle de imagem da pessoa](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

O [controle de classificação](../design/controls-and-patterns/rating.md) permite que os usuários facilmente visualizem e definam classificações que refletem o grau de satisfação com o conteúdo e serviços.

![Exemplo de controle de classificações](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>Kits de ferramentas de design

Os [kits de ferramentas de design e recursos para aplicativos UWP](../design/downloads/index.md) foram expandidos com a adição dos kits de ferramentas do Sketch e do Adobe XD. Os kits de ferramentas existentes anteriormente também foram atualizados e renovados, fornecendo modelos de layout e controles mais sólidos para seus aplicativos UWP.

### <a name="dashboard-monetization-and-store-services"></a>Painel, monetização e serviços da Loja

Os seguintes novos recursos agora estão disponíveis:

* O SDK do Microsoft Advertising agora permite que você exiba [anúncios nativos](../monetize/native-ads.md) em seus aplicativos. Um anúncio nativo é um formato de anúncio com base em componente em que cada parte do criativo do anúncio (como título, imagem, descrição e texto do plano de ação) é entregue ao aplicativo como um elemento individual. Atualmente, os núncios nativos só estão disponíveis para desenvolvedores que estão participando de um programa piloto, mas nós pretendemos disponibilizar esse recurso para todos os desenvolvedores em breve.

* A [API de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md) agora fornece um método que pode ser usado para [baixar o arquivo CAB para um erro em seu app](../monetize/download-the-cab-file-for-an-error-in-your-app.md).

* [Ofertas direcionadas](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) permitem que você direcione segmentos específicos dos seus clientes com conteúdo atrativo e personalizado para aumentar o envolvimento, a retenção e a monetização. 

* A listagem da Loja do seu aplicativo agora pode incluir [trailers de vídeo](../publish/app-screenshots-and-images.md#trailers).

* Novos preços e opções de disponibilidade permitem [agendar alterações de preço](../publish/set-and-schedule-app-pricing.md) e [definir datas de lançamento precisos](..//publish/configure-precise-release-scheduling.md).

* Você pode [importar e exportar as listagens da Loja](../publish/import-and-export-store-listings.md) para fazer atualizações mais rapidamente, especialmente se você tiver listagens em vários idiomas.

### <a name="my-people"></a>Minhas Pessoas

Disponível para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Compilações de Prévia SDK, o futuro recurso Minhas Pessoas permitirá aos usuários fixar contatos de um aplicativo diretamente nas suas barras de tarefas. [Saiba como adicionar suporte para Minhas Pessoas a seu aplicativo.](../contacts-and-calendar/my-people-support.md)

![Painel de contato do recurso Minhas Pessoas](images/my-people.png)

[Compartilhamento de Minhas Pessoas](../contacts-and-calendar/my-people-sharing.md) permite aos usuários compartilhar arquivos por meio de seu aplicativo, diretamente da barra de tarefas.

![Compartilhamento de Minhas Pessoas](images/my-people-sharing.png)

[Notificações de Minhas Pessoas](../contacts-and-calendar/my-people-support.md) são um novo tipo de notificação do sistema que os usuários podem enviar para seus contatos fixados.

![Notificação de Minhas Pessoas](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>Fixar na barra de tarefas

Disponível para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Compilações de Prévia SDK, a nova classe TaskbarManager permite que você solicite a seu usuário que [fixe seu aplicativo na barra de tarefas](../design/shell/pin-to-taskbar.md).

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="media-playback"></a>Reprodução de mídia

Novas seções foram adicionadas ao artigo de reprodução de mídia básica, [Reproduzir áudio e vídeo com o MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md). A seção [Reproduzir áudio esférico com o MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) mostra como reproduzir vídeos codificados esfericamente, incluindo o ajuste do campo de exibição e orientação de exibição para formatos compatíveis. A seção [Usar o MediaPlayer no modo de servidor de quadros](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) mostra como copiar quadros de mídia reproduzidos com [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) em uma superfície de Direct3D. Isso possibilita cenários como a aplicação de efeitos em tempo real com sombreadores de pixel. O código de exemplo mostra uma implementação rápida de um efeito de desfoque para reprodução de vídeo usando Win2D.

### <a name="media-capture"></a>Captura de mídia

O artigo [Processar quadros de mídia com o MediaFrameReader](../audio-video-camera/process-media-frames-with-mediaframereader.md) foi atualizado para mostrar o uso da classe [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader), que permite que você obtenha quadros correlacionados no tempo de várias fontes de mídia. Isso é útil se você precisar processar quadros de fontes diferentes, como uma câmera de profundidade e uma câmera em cores, e você precisar se certificar de que os quadros de cada fonte foram capturados próximos entre si no tempo. Para obter mais informações, consulte [Usar MultiSourceMediaFrameReader para obter quadros correlacionados no tempo de várias fontes](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources).

### <a name="scoped-search"></a>Escopo da pesquisa

Foi adicionado um escopo "UWP" à documentação [Conceitual de UWP](../get-started/universal-application-platform-guide.md) e [Referência de API](https://docs.microsoft.com/en-us/uwp/api/) em docs.microsoft.com. A menos que esse escopo é desativado, pesquisas feitas a partir dessas áreas retornarão apenas os documentos de UWP.

![Escopo da pesquisa](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Testar seu aplicativo do Windows para o Windows 10 S

Teste seu aplicativo do Windows para garantir que ele funcione corretamente em dispositivos que executam o Windows S. Use [este novo guia](../porting/desktop-to-uwp-test-windows-s.md) para saber como. 

## <a name="samples"></a>Exemplos

### <a name="annotated-audio-app-sample"></a>Exemplo de aplicativo de áudio anotado

[Um exemplo de miniaplicativos que demonstra o áudio, a tinta e os cenários de roaming de dados do OneDrive](https://github.com/Microsoft/Windows-appsample-annotated-audio). Essa amostra grava o áudio permitindo a captura sincronizada de anotações à tinta para que posteriormente você possa chamar novamente o que estava sendo discutido no momento em que uma anotação foi feita.

![Captura de tela do exemplo de áudio anotado](images/Playback.png)  

### <a name="shopping-app-sample"></a>Exemplo de aplicativo de compras

[Um miniaplicativo que apresenta uma experiência de compras básica na qual um usuário pode comprar emoji](https://github.com/Microsoft/Windows-appsample-shopping). Este aplicativo mostra como usar as [APIs de solicitação de pagamento](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) para implementar a experiência de finalização de compra.

![Captura de tela do exemplo de aplicativo de compras](images/shoppingcart.png)  

## <a name="videos"></a>Vídeos

### <a name="accessibility"></a>Acessibilidade

A criação de acessibilidade em seus aplicativos permite a estes uma audiência muito mais ampla. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility) e saiba mais sobre como [desenvolver aplicativos para acessibilidade](https://developer.microsoft.com/en-us/windows/accessible-apps).

### <a name="payments-request-api"></a>API de solicitação de pagamentos

A API de solicitação de pagamento ajuda clientes e vendedores a concluir o processo de finalização de compra online tranquilamente. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API) e depois explore a [documento de solicitação de pagamento](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API).

### <a name="windows-10-iot-core"></a>Windows 10 IoT Core

Com o Windows 10 IoT Core e a Plataforma Universal do Windows, você pode rapidamente fazer um protótipo de e criar projetos com conexões de visão e componentes, como esse reconhecimento de porta para animais. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core) e saiba mais como [começar a usar o Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot).