---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: Uma rápida visão geral das tecnologias de rede disponíveis para um desenvolvedor UWP, com sugestões sobre como escolher as tecnologias adequadas aos seu aplicativo.
title: Qual tecnologia de rede?
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e418e5a159df44d6ff6e15e4faa972164447f5ee
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690919"
---
# <a name="which-networking-technology"></a>Qual tecnologia de rede?


Uma rápida visão geral das tecnologias de rede disponíveis para um desenvolvedor UWP, com sugestões sobre como escolher as tecnologias adequadas aos seu aplicativo.

## <a name="sockets"></a>Soquetes

Use [soquetes](sockets.md) quando você se comunicar com outro dispositivo e desejar usar seu próprio protocolo.

Há duas implementações de soquetes disponíveis para desenvolvedores da Plataforma Universal do Windows (UWP): [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) e [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673). Se você estiver escrevendo código novo, Windows.Networking.Sockets tem a vantagem de ser uma API moderna, desenvolvida para uso por desenvolvedores UWP. Se você estiver usando bibliotecas de rede de plataforma cruzada ou outro código Winsock existente, ou se preferir a API Winsock, use essa.

### <a name="when-to-use-sockets"></a>Quando usar soquetes

-   As duas implementações de soquetes permitem que você se comunique com outros dispositivos usando protocolos de sua escolha, usando TCP ou UDP.

-   Escolha a API de soquetes que melhor atenda às suas necessidades com base na experiência e em qualquer código existente que você esteja usando.

### <a name="when-not-to-use-sockets"></a>Quando não usar soquetes

-   Não implemente sua própria pilha HTTP(S) usando soquetes. Use [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).
-   Se os WebSockets (as classes [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) e [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)) atendem às suas necessidades de comunicações (TCP de/para um servidor Web), considere a possibilidade de usá-los em vez de desperdiçar tempo e recursos de desenvolvimento para implementar uma funcionalidade semelhante com soquetes.

## <a name="websockets"></a>WebSockets

O protocolo [WebSockets](websockets.md) define um mecanismo para comunicações bidirecionais rápidas e seguras entre um cliente e um servidor na Web. Os dados são transferidos imediatamente através de uma única conexão de soquete full-duplex, permitindo que as mensagens sejam enviadas e recebidas de ambos os pontos de extremidade em tempo real. WebSockets são ideais para uso em jogos em tempo real nos quais notificações instantâneas de redes sociais e exibições atualizadas de informações (como estatísticas de jogo) precisam ser seguras e usar transferências de dados rápidas. Os desenvolvedores UWP podem usar as classes [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) e [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) para se conectar com servidores que suportam o protocolo Websocket.

### <a name="when-to-use-websockets"></a>Quando usar um Websockets

-   Quando você deseja enviar e receber dados continuamente entre um dispositivo e um servidor.

### <a name="when-not-to-use-websockets"></a>Quando não usar Websockets

-   Se você envia ou recebe dados com pouca frequência, pode ser mais simples fazer solicitações HTTP individuais do dispositivo para o servidor, em vez de estabelecer e manter uma conexão WebSocket.
-   WebSockets podem não ser adequados para situações de grande volume. Considere a possibilidade de modelar seus fluxos de dados e simular o tráfego por meio de WebSockets antes de se comprometer a usá-los em seu design.

## <a name="httpclient"></a>HttpClient

Use [HttpClient](httpclient.md) (e o restante da API namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)) quando você estiver usando HTTP(S) para se comunicar com um serviço ou um servidor Web.

### <a name="when-to-use-httpclient"></a>Quando usar um HttpClient

-   Quando usar HTTP(S) para se comunicar com serviços Web.
-   Quando fizer upload ou transferência de um pequeno número de arquivos menores.
-   Se os WebSockets (as classes [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) e [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)) atenderem às suas necessidades de comunicações (TCP de/para um servidor Web), e o servidor Web em questão suportar WebSockets, considere a possibilidade de usá-los em vez de desperdiçar tempo e recursos de desenvolvimento para implementar funcionalidade semelhante com HttpClient.
-   Quando você está transmitindo conteúdo através da rede.

### <a name="when-not-to-use-httpclient"></a>Quando não usar HttpClient

-   Se você estiver transferindo arquivos grandes ou grande número de arquivos, considere usar transferências em segundo plano.
-   Se quiser restringir os limites de transferência/upload com base no tipo de conexão ou se quiser salvar o andamento e retomar o transferência/upload após uma interrupção, você deverá usar transferências em segundo plano.
-   Se você estiver se comunicando com dois dispositivos e nenhum deles for projetado para atuar como um servidor HTTP(S), você deverá usar soquetes. Não tente implementar seu próprio servidor HTTP e usar [HttpClient](httpclient.md) para se comunicar com ele.

## <a name="background-transfers"></a>Transferências em segundo plano

Use a [API de transferência em segundo plano](background-transfers.md) quando desejar transferir arquivos de forma confiável pela rede. A API de transferência em segundo plano fornece recursos avançados de upload e transferência que são executados em segundo plano durante a suspensão do aplicativo e persistem após o encerramento do aplicativo. A API monitora o status da rede e automaticamente suspende e retoma transferências quando a conexão é perdida. As transferências também reconhecem o sensor de dados e de bateria, ou seja, a atividade de transferência se ajusta de acordo com a conectividade atual e o status de bateria do dispositivo. Esses recursos são essenciais quando o aplicativo é executado em dispositivos móveis ou alimentados por bateria. A API é ideal para carregar e baixar arquivos muito grandes usando HTTP(S). Também há suporte a FTP, mas apenas para downloads.

Um novo recurso de transferência em segundo plano no Windows 10 é a capacidade de disparar pós-processamento quando uma transferência de arquivo for concluída, para que você possa atualizar catálogos locais, ativar outros aplicativos ou notificar o usuário quando um download for concluído.

### <a name="when-to-use-background-transfers"></a>Quando usar transferências em segundo plano

-   Use transferências em segundo plano para transferir, de forma confiável, arquivos grandes ou um grande número de arquivos.
-   Use transferências em segundo plano com grupos de conclusão de transferência em segundo plano quando desejar pós-processar transferências de arquivos com uma tarefa em segundo plano.
-   Use transferências em segundo plano se quiser retomar uma transferência em andamento após uma interrupção na rede.
-   Use transferências em segundo plano se você quiser alterar o comportamento de transferência com base nas condições da rede como sendo em um plano de dados limitado.

### <a name="when-not-to-use-background-transfers"></a>Quando não usar transferências em segundo plano

-   Se você estiver transferindo poucos arquivos pequenos e não precisar fazer qualquer pós-processamento quando a transferência for concluída, considere usar o método PUT ou POST de [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).
-   Se você quiser transmitir dados e usá-los localmente à medida que chegarem, use [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).

## <a name="additional-network-related-technologies"></a>Outras tecnologias relacionadas a redes

### <a name="connection-quality"></a>Qualidade de conexão

A API [**Windows.Networking.Connectivity**](https://msdn.microsoft.com/library/windows/apps/br207308) permite acesso a informações de uso, custo e conectividade de rede. Para obter mais informações sobre como usar essa API, consulte [Acesso a estado de conexão de rede e gerenciamento de custos de rede](https://msdn.microsoft.com/library/windows/apps/hh452983)

### <a name="dns-service-discovery"></a>Descoberta de Serviço DNS

A API [**Windows.Networking.ServiceDiscovery.Dnssd**](https://msdn.microsoft.com/library/windows/apps/dn895183) permite que você anuncie um serviço de rede para outros dispositivos na rede usando o protocolo DNS-SD, descrito em IETF [RFC 2782](http://go.microsoft.com/fwlink/?LinkId=524158).

### <a name="communicating-over-bluetooth"></a>Comunicando-se via Bluetooth

Entre outras coisas, a API [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/dn263413) permite que você use Bluetooth para se conectar a outros dispositivos e transferir dados. Para obter mais informações, consulte [Enviar ou receber arquivos com RFCOMM](https://msdn.microsoft.com/library/windows/apps/mt270289).

### <a name="push-notifications-wns"></a>Notificações por push (WNS)

A API [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) permite que você use o Serviço de Notificação do Windows (WNS) para receber notificações por push pela rede. Para obter mais informações sobre o uso dessa API, consulte a [Visão geral dos WNS (Serviços de Notificação por Push do Windows)](https://msdn.microsoft.com/library/windows/apps/mt187203)

### <a name="near-field-communications"></a>Comunicação a curta distância

A API [**Windows.Networking.Proximity**](https://msdn.microsoft.com/library/windows/apps/br241250) permite usar comunicação a curta distância em aplicativos que utilizam proximidade ou toque com dispositivos para facilitar a transferência de dados. Para obter mais informações sobre o uso dessa API, consulte [Dando suporte à proximidade e ao toque](https://msdn.microsoft.com/library/windows/apps/hh465229).

### <a name="rssatom-feeds"></a>Feeds RSS/Atom

A API [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) permite gerenciar feeds de sindicalização usando os formatos RSS e Atom. Para saber mais sobre como usar essa API, consulte [Feeds RSS/Atom](web-feeds.md).

### <a name="wi-fi-enumeration-and-connection-control"></a>Controle de conexão e enumeração de Wi-Fi

A API [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/dn975224) permite enumerar adaptadores Wi-Fi, procurar redes Wi-Fi disponíveis e conectar um adaptador a uma rede.

### <a name="radio-control"></a>Controle de rádio

A API [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/dn996447) permite que você encontre e controle rádios no dispositivo local, incluindo Wi-Fi e Bluetooth.

### <a name="wi-fi-direct"></a>Wi-Fi Direct

A API [**Windows.Devices.WiFiDirect**](https://msdn.microsoft.com/library/windows/apps/dn297687) permite que você se conecte e se comunique com outros dispositivos locais usando Wi-Fi Direct para criar redes sem fio locais ad hoc.

### <a name="wi-fi-direct-services"></a>Serviços Wi-Fi Direct

A API [**Windows.Devices.WiFiDirect.Services**](https://msdn.microsoft.com/library/windows/apps/dn996481) permite que você forneça serviços Wi-Fi Direct e se conecte a eles. Os Serviços Wi-Fi Direct são a maneira pela qual um dispositivo em uma rede Wi-Fi ad hoc direta (anunciante de um serviço) oferece funcionalidades para outro dispositivo (um seeker de serviço) por uma conexão Wi-Fi Direct.

### <a name="mobile-operators"></a>Operadoras móveis

Windows 10 expõe para um público de desenvolvedores algumas APIs que só eram expostos a fabricantes de dispositivos e operadoras móveis. Embora estejam expostas agora, essas APIs também são limitadas por recursos de aplicativo específicos que devem ser aprovados pela Microsoft para que um aplicativo possa ser publicado. O uso real dessas APIs continuará limitado principalmente a fabricantes de dispositivos e operadoras móveis.

### <a name="network-operations"></a>Operações de rede

A API [**Windows.Networking.NetworkOperators**](https://msdn.microsoft.com/library/windows/apps/br241148) lida principalmente com a configuração e o provisionamento de telefones. Dessa forma, a permissão para usar os recursos que a controlam está limitada a fabricantes de dispositivos e provedores de telecomunicações.

### <a name="sms"></a>SMS

O namespace [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/br206567) lida com SMS e mensagens relacionadas como entidades de nível inferior. Ele é fornecido para uso por operadoras móveis para uso em SMS direcionado a aplicativos e é controlado por um recurso que não será aprovado para ser usado pela maioria dos desenvolvedores de aplicativos. Caso esteja escrevendo um aplicativo para lidar com mensagens, você deve usar a API [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/dn642321), porque ela foi projetada para tratar não apenas mensagens SMS, mas também mensagens de outras origens, como aplicativos de chat em tempo real, o que possibilita uma experiência de chat/mensagens muito mais avançada.

