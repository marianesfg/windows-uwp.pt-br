---
title: Novidades do Windows Docs em dezembro de 2017 – Desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 referente a dezembro de 2017
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, dezembro
ms.date: 12/14/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f785ad5d7898f838435e0a05cf8dea5c778e70f3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684749"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>Novidades dos Documentos de Desenvolvedor do Windows em dezembro de 2017

A Documentação do Desenvolvedor Windows recebe atualizações constantes com informações sobre novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Após o lançamento do Fall Creators Update, foram disponibilizadas as visões gerais de recursos, diretrizes para desenvolvedores e amostras a seguir, contendo informações novas e atualizadas para desenvolvedores do Windows.

[Instale as ferramentas e o SDK](https://developer.microsoft.com/windows/downloads#_blank) no Windows 10 e você estará pronto para [criar um aplicativo universal do Windows](../get-started/create-uwp-apps.md) ou explorar como você pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality: Guia do entusiasta

Destinado a entusiastas de tecnologia que estão explorando o mundo da realidade misturada, o [Guia do entusiasta](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/) responde às principais questões sobre o Windows Mixed Reality. 

Neste guia, você encontrará: 
- Perguntas frequentes antes da compra, 
- Como verificar a compatibilidade de seu computador, 
- Instruções de configuração, 
- Como usar fone de ouvido e controladores, 
- Como baixar e executar jogos imersivos, vídeos em 360°, aplicativos 2D, WebVR e SteamVR, 
- Como solucionar problemas e muito mais.

![Usuário de headset e controladores de movimentos do Windows Mixed Reality](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>Interações por teclado

Crie e otimize aplicativos UWP para fornecer uma experiência acessível e recursos para usuários avançados com [Interações por teclado](../design/input/keyboard-interactions.md) avançadas. Atualizamos nossas recomendações e diretrizes para que reflitam as novas melhorias dessas interações adicionadas no Fall Creators Update.

Consulte [Aceleradores de teclado](../design/input/keyboard-accelerators.md) e [Interações por teclado personalizadas](../design/input/custom-keyboard-interactions.md) para expandir a funcionalidade de teclado dos aplicativos.

Em dispositivos com suporte para interações por toque, adicione a funcionalidade de teclado com os artigos [Responder à presença do teclado virtual](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) e [Usar o escopo de entrada para alterar o teclado virtual](../design/input/use-input-scope-to-change-the-touch-keyboard.md).

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

O portal Microsoft Collaborate fornece ferramentas e serviços para simplificar a colaboração de engenharia dentro do ecossistema da Microsoft, habilitando o compartilhamento de itens de trabalho do sistema de engenharia (bugs, solicitações de recursos etc.) e a distribuição de conteúdo (builds, documentos, especificações). [Saiba mais](https://docs.microsoft.com/collaborate/).

![Microsoft Collaborate no Partner Center](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>Empacotar aplicativos da área de trabalho com projetos UWP

A versão 15.5 do Visual Studio 2017 atualizou o modelo **Projeto de empacotamento de aplicativos do Windows** para que seja muito mais fácil incluir um projeto UWP. Você não precisará mais usar um projeto de empacotamento baseado em JavaScript e depois ajustar manualmente o manifesto do pacote.  

Consulte [Empacotar um aplicativo usando o Visual Studio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) para saber como usar esse novo modelo para empacotar o aplicativo da área de trabalho.

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) para saber como adicionar um projeto UWP ao pacote.

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Agora os complementos de assinatura estão disponíveis para os desenvolvedores no Programa Insider do Centro de Desenvolvimento do Windows

Todos os desenvolvedores que ingressaram no Programa Insider do Centro de Desenvolvimento agora podem usar complementos de assinatura para vender produtos digitais nos aplicativos (como recursos de aplicativo ou conteúdo digital) com períodos de cobrança recorrentes automatizados. Para obter mais detalhes, confira [Habilitar complementos de assinatura para o aplicativo](../monetize/enable-subscription-add-ons-for-your-app.md).

## <a name="developer-guidance"></a>Diretrizes do desenvolvedor

### <a name="color"></a>Cor

Adicionamos algumas novas diretrizes sobre como usar cor no aplicativo para obter a melhor experiência de usuário possível. Isso inclui cenários de uso de API e orientações gerais sobre design de interface do usuário e acessibilidade. Também atualizamos a lista de cores de destaque do usuário disponível no Xbox. [Confira aqui o artigo sobre cores atualizado.](../design/style/color.md)

![paleta de cores universal do windows](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>Guias de acesso a dados

Adicionamos um [Guia do SQL Server](../data-access/sql-server-databases.md) para mostrar como o aplicativo pode acessar diretamente um banco de dados do SQL Server. Nenhuma camada de serviço é necessária.

Além disso, adicionamos nosso [Guia do SQLite](../data-access/sqlite-databases.md) completamente remodelado com uma aparência mais limpa e incluímos as práticas recomendadas para armazenamento e recuperação de dados mais recentes em um banco de dados leve no dispositivo do usuário.

### <a name="forms"></a>Formulários

Adicionamos um novo artigo sobre [Como construir formulários nos seus aplicativos](../design/controls-and-patterns/forms.md), para coletar e enviar dados dos usuários. Isso inclui informações específicas sobre implementação de formulários e diretrizes gerais sobre quando e onde usá-los.

### <a name="intro-to-app-design"></a>Introdução ao design de aplicativo

As orientações sobre design da UWP (Plataforma Universal do Windows) são um recurso que ajuda você a projetar e criar aplicativos belos e refinados. [Nossa nova introdução](../design/basics/design-and-ui-intro.md) fornece uma visão geral dos recursos de design universais incluídos em todos os aplicativos UWP e mostra como você pode usar os documentos para criar interfaces de usuário que se adequam perfeitamente a vários dispositivos.


### <a name="request-ratings-and-reviews"></a>Solicitar classificações e opiniões

Adicionamos um novo artigo que mostra como você pode [solicitar classificações e opiniões para o seu aplicativo](../monetize/request-ratings-and-reviews.md). Você pode mostrar uma caixa de diálogo de classificações e análises no contexto do aplicativo ou abrir uma página de classificações e análises para o aplicativo na Store.

## <a name="samples"></a>Amostras

### <a name="customer-orders"></a>Pedidos de clientes

A amostra do [Banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) foi atualizada para mostrar as práticas recomendadas para acesso a dados, como usar o padrão de repositório e se conectar a várias fontes de dados (incluindo Sqlite, SQL Azure e um serviço REST).

## <a name="videos"></a>Vídeos

### <a name="package-a-net-app-in-visual-studio"></a>Empacotar um aplicativo .NET no Visual Studio

Ficou muito mais fácil trazer seu aplicativo da área de trabalho para a Plataforma Universal do Windows. [Assista ao vídeo](https://www.youtube.com/watch?v=fJkbYPyd08w) para saber como empacotar seu aplicativo .NET para distribuição e depois [confira esta página](../porting/desktop-to-uwp-packaging-dot-net.md) para obter mais informações.