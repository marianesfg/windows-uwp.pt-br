---
author: DelfCo
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: "Tecnologias para acessar os serviços de rede e Web."
title: "Serviços de rede e Web"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: b4e291c0948a7734b8ba188bc060dfba2e9a83d1

---

# Serviços de rede e Web

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

As seguintes tecnologias de serviços de rede e Web estão disponíveis para desenvolvedores da UWP (Plataforma Universal do Windows).

| Tópico                                                                                   | Descrição                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [Noções básicas de rede](networking-basics.md)                                               | Coisas que você deve fazer para qualquer aplicativo habilitado por rede.                     |
| [Qual tecnologia de rede?](which-networking-technology.md)                          | Uma rápida visão geral das tecnologias de rede disponíveis para um desenvolvedor UWP, com sugestões sobre como escolher as tecnologias adequadas aos seu aplicativo.               |
| [Comunicações de rede em segundo plano](network-communications-in-the-background.md) | Os aplicativos usam tarefas em segundo plano e dois mecanismos principais para manter as comunicações quando não estão em primeiro plano: agente de soquete e gatilhos de canal de controle.                  |
| [Soquetes](sockets.md)                                                                   | Você pode usar o [Windows.Networking.Sockets](https://msdn.microsoft.com/library/windows/apps/xaml/windows.networking.sockets.aspx) e o [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) para se comunicar com outros dispositivos como desenvolvedor de aplicativos UWP. Este tópico fornece orientações detalhadas sobre como usar o namespace Windows.Networking.Sockets para executar operações de rede. |
| [WebSockets](websockets.md)                                                             | Os WebSockets fornecem um mecanismo para comunicações bidirecionais rápidas e seguras entre um cliente e um servidor na Web usando HTTP(S).                 |
| [HttpClient](httpclient.md)                                                             | Use A API do namespace [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) para enviar e receber informações usando os protocolos HTTP 2.0 e HTTP 1.1.             |
| [RSS feeds/feeds Atom](web-feeds.md)                                                          | Recupere ou crie o conteúdo da Web mais atual e popular usando feeds sindicalizados gerados de acordo com os padrões RSS e Atom, usando os recursos no namespace [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632).                   |
| [Transferências em segundo plano](background-transfers.md)                                         | Use a API de transferência em segundo plano para copiar arquivos de forma confiável pela rede.           |
| [Marcando conexões de rede com a identidade EDP](tagging_network_connections_with_edp_identity.md) | Este tópico mostra como criar um contexto de thread protegido antes de criar conexões de rede em um cenário de EDP (proteção de dados empresariais). |



<!--HONumber=Jul16_HO2-->


