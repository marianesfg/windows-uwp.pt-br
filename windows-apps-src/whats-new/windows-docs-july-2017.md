---
title: Novidades nos documentos do Windows em julho de 2017 – Desenvolver aplicativos UWP
description: Novos recursos, amostras e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 referente a julho de 2017
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 65c3c2fb4b7a5a7f0b5f4b3c89773f3e21bd654d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684746"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>Novidades nos documentos do desenvolvedor do Windows em julho de 2017

A Documentação do Desenvolvedor Windows recebe atualizações constantes com informações sobre novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Foram disponibilizadas visões gerais de recursos, diretrizes para desenvolvedores e exemplos de código a seguir, contendo informações novas e atualizadas para desenvolvedores do Windows.

[Instale as ferramentas e o SDK](https://developer.microsoft.com/windows/downloads#_blank) no Windows 10 e você estará pronto para [criar um aplicativo universal do Windows](../get-started/your-first-app.md) ou explorar como você pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="fluent-design"></a>Design Fluente

Disponíveis para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Versões Prévias do SDK, esses novos efeitos usam profundidade, perspectiva e movimento para ajudar os usuários a se concentrarem nos elementos importantes da interface de usuário.

[Material acrílico](../design/style/acrylic.md) é um tipo de pincel que cria texturas transparentes. 

![Acrílico em tema claro](../design/style/images/Acrylic_DarkTheme_Base.png)

O [efeito Paralaxe](../design/motion/parallax.md) adiciona profundidade tridimensional e perspectiva ao seu aplicativo.

![Um exemplo de paralaxe com uma lista e uma imagem de fundo](../design/style/images/_Parallax_v2.gif)

[Revelação](../design/style/reveal.md) destaca os elementos importantes do seu aplicativo. 

![Visual da Revelação](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>Controles da interface do usuário

Disponíveis para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Versões Prévias do SDK, esses novos controles permitem criar, de forma fácil e rápida, uma interface de usuário com ótima aparência.

Com o [controle do seletor de cor](../design/controls-and-patterns/color-picker.md), os usuários podem procurar e selecionar cores.  

![Um seletor de cores padrão](../design/controls-and-patterns/images/color-picker-default.png)

O [controle de exibição de navegação](../design/controls-and-patterns/navigationview.md) facilita a adição de navegação de ponta ao seu aplicativo.

![Seções de NavigationView](../design/controls-and-patterns/images/navview_sections.png)

O [controle de imagem da pessoa](../design/controls-and-patterns/person-picture.md) exibe a imagem de avatar de uma pessoa.

![O controle de imagem da pessoa](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

Com o [controle de classificação](../design/controls-and-patterns/rating.md), os usuários podem ver e definir facilmente classificações que refletem o grau de satisfação com o conteúdo e os serviços.

![Exemplo de controle de classificações](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>Kits de ferramentas de design

Os [kits de ferramentas de design e recursos para aplicativos UWP](../design/downloads/index.md) foram expandidos com a adição dos kits de ferramentas do Sketch e do Adobe XD. Os kits de ferramentas que já existiam também foram atualizados e renovados, fornecendo modelos de layout e controles mais sólidos para seus aplicativos UWP.

### <a name="dashboard-monetization-and-store-services"></a>Painel, monetização e serviços da Microsoft Store

Os seguintes recursos novos estão disponíveis:

* O SDK do Microsoft Advertising agora permite que você exiba [anúncios nativos](../monetize/native-ads.md) em seus aplicativos. Um anúncio nativo é um formato de anúncio com base em componente em que cada parte do criativo do anúncio (como título, imagem, descrição e texto do plano de ação) é entregue ao aplicativo como um elemento individual. Atualmente, os anúncios nativos só estão disponíveis para desenvolvedores que estão participando de um programa piloto, mas pretendemos disponibilizar esse recurso para todos os desenvolvedores em breve.

* Agora, a [API de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md) fornece um método que pode ser usado para [baixar o arquivo CAB para um erro em seu aplicativo](../monetize/download-the-cab-file-for-an-error-in-your-app.md).

* As [ofertas direcionadas](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) permitem que você direcione segmentos específicos dos seus clientes com conteúdo atraente e personalizado para aumentar o envolvimento, a retenção e a monetização. 

* A listagem da Store do seu aplicativo agora pode incluir [trailers de vídeo](../publish/app-screenshots-and-images.md#trailers).

* Novos preços e opções de disponibilidade permitem [agendar alterações de preço](../publish/set-and-schedule-app-pricing.md) e [definir datas de lançamento precisas](..//publish/configure-precise-release-scheduling.md).

* É possível [importar e exportar as listagens da Store](../publish/import-and-export-store-listings.md) para fazer atualizações de forma mais rápida, especialmente se você tiver listagens em vários idiomas.

### <a name="my-people"></a>Minhas Pessoas

Disponível para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Versões Prévias do SDK, o futuro recurso Minhas Pessoas permitirá aos usuários fixar contatos de um aplicativo diretamente na barra de tarefas. [Saiba como adicionar suporte para Minhas Pessoas ao seu aplicativo.](../contacts-and-calendar/my-people-support.md)

![Painel de contato do recurso Minhas Pessoas](images/my-people.png)

O [compartilhamento de Minhas Pessoas](../contacts-and-calendar/my-people-sharing.md) permite aos usuários compartilhar arquivos por meio de seu aplicativo, diretamente da barra de tarefas.

![Compartilhamento de Minhas Pessoas](images/my-people-sharing.png)

As [notificações de Minhas Pessoas](../contacts-and-calendar/my-people-support.md) são um novo tipo de notificação do sistema que os usuários podem enviar para seus contatos fixados.

![Notificação de Minhas Pessoas](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>Fixar à barra de tarefas

Disponível para [Participantes do Programa Windows Insider](https://insider.windows.com/) nas Versões Prévias do SDK, a nova classe TaskbarManager permite que você solicite ao usuário que [fixe o aplicativo na barra de tarefas](../design/shell/pin-to-taskbar.md).

## <a name="developer-guidance"></a>Diretrizes do desenvolvedor

### <a name="media-playback"></a>Reprodução de mídia

Novas seções foram adicionadas ao artigo de reprodução de mídia básica, [Reproduzir áudio e vídeo com o MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md). A seção [Reproduzir vídeo esférico com o MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) mostra como reproduzir vídeos codificados esfericamente, incluindo o ajuste do campo de exibição e orientação de exibição para formatos compatíveis. A seção [Usar o MediaPlayer no modo de servidor de quadros](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) mostra como copiar quadros de mídia reproduzidos com [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) em uma superfície Direct3D. Isso possibilita, por exemplo, a aplicação de efeitos em tempo real com sombreadores de pixel. O código de exemplo mostra uma implementação rápida de um efeito de desfoque para reprodução de vídeo usando Win2D.

### <a name="media-capture"></a>Captura de mídia

O artigo [Processar quadros de mídia com o MediaFrameReader](../audio-video-camera/process-media-frames-with-mediaframereader.md) foi atualizado para mostrar o uso da nova classe [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader), que permite que você obtenha quadros correlacionados no tempo de várias fontes de mídia. Isso será útil se você precisar processar quadros de fontes diferentes, como uma câmera de profundidade e uma câmera de cores, e precisar se certificar de que os quadros de cada fonte foram capturados próximos entre si no tempo. Para saber mais, consulte [Usar MultiSourceMediaFrameReader para obter quadros correlacionados no tempo de várias fontes](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources).

### <a name="scoped-search"></a>Escopo da pesquisa

Foi adicionado um escopo "UWP" à documentação [Conceitual de UWP](../get-started/universal-application-platform-guide.md) e [Referência de API](https://docs.microsoft.com/uwp/api/) em docs.microsoft.com. A menos que esse escopo seja desativado, as pesquisas feitas a partir dessas áreas retornarão apenas os documentos da UWP.

![Escopo da pesquisa](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Testar seu aplicativo do Windows para Windows 10 S

Teste seu aplicativo do Windows para garantir que ele funcione corretamente em dispositivos que executam o Windows S. Use [este novo guia](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-test-windows-s) para saber como.

## <a name="samples"></a>Amostras

### <a name="annotated-audio-app-sample"></a>Exemplo de aplicativo de áudio anotado

[Um exemplo de miniaplicativos que demonstra o áudio, a tinta e os cenários de roaming de dados do OneDrive](https://github.com/Microsoft/Windows-appsample-annotated-audio). Essa amostra grava o áudio permitindo a captura sincronizada de anotações à tinta para que posteriormente você possa chamar novamente o que estava sendo discutido no momento em que uma anotação foi feita.

![Captura de tela do exemplo de áudio anotado](images/Playback.png)  

### <a name="shopping-app-sample"></a>Exemplo de aplicativo de compras

[Um miniaplicativo que apresenta uma experiência de compras básica na qual um usuário pode comprar emoji](https://github.com/Microsoft/Windows-appsample-shopping). Esse aplicativo mostra como usar as [APIs de Solicitação de Pagamento](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) para implementar a experiência de finalização de compra.

![Captura de tela do exemplo de aplicativo de compras](images/shoppingcart.png)  

## <a name="videos"></a>Vídeos

### <a name="accessibility"></a>Acessibilidade

A criação de acessibilidade em seus aplicativos os expande para um público muito mais amplo. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility) e saiba mais sobre como [desenvolver aplicativos para acessibilidade](https://developer.microsoft.com/windows/accessible-apps).

### <a name="payments-request-api"></a>API de Solicitação de Pagamento

A API de Solicitação de Pagamento ajuda clientes e vendedores a concluir o processo de finalização de compra online tranquilamente. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API) e explore o [documento de Solicitação de Pagamento](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API).

### <a name="windows-10-iot-core"></a>Windows 10 IoT Core

Com o Windows 10 IoT Core e a Plataforma Universal do Windows, você pode criar rapidamente um protótipo e elaborar projetos com conexões de visão e componentes, como essa Porta para Reconhecimento de Animais. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core) e saiba mais sobre como [começar a usar o Windows 10 IoT Core](https://developer.microsoft.com/windows/iot).