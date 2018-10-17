---
author: andrewleader
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: Visualizador de notificações
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: af8b2489346e1ef81c5cae304802814b79b8b950
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4743519"
---
# <a name="notifications-visualizer"></a>Visualizador de notificações

 


O Visualizador de notificações é um novo aplicativo UWP (Plataforma Universal do Windows) na [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ajuda os desenvolvedores no design de Blocos Dinâmicos adaptáveis e notificações do sistema interativas para Windows 10.


## <a name="overview"></a>Visão geral

O Visualizador de notificações oferece visualizações instantâneas do bloco e da notificação do sistema à medida que você edita o conteúdo XML, semelhante ao modo de exibição editor/design XAML do Visual Studio. O aplicativo também verifica se há erros, o que garante que você crie um conteúdo de notificação do sistema de bloco válido.

Esta captura de tela do aplicativo mostra a carga XML e como tamanhos de bloco são exibidos em um dispositivo selecionado:

![captura de tela do aplicativo Visualizador de Notificações com código e blocos](images/notif-visualizer-001.png)

 

Com o Visualizador de notificações, é possível criar e testar o conteúdo de bloco adaptável e notificações do sistema sem que seja necessário editar e implantar o aplicativo. Depois que tiver criado uma carga com resultados visuais ideais, você pode integrá-la ao aplicativo. Consulte [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md) e [Enviar uma notificação de sistema local](send-local-toast.md) para saber mais.

**Observação**   A simulação do Visualizador de notificações do menu Iniciar do Windows e as notificações do sistema nem sempre são notifications precisas e não oferecem suporte a algumas propriedades avançadas de carga. Quando tiver o bloco ou da notificação do sistema que você deseja, teste fixando o bloco ou usando a notificação no menu Iniciar real para verificar se ele é exibido como você deseja.

 

## <a name="features"></a>Recursos

Visualizador de Notificações acompanha várias cargas de exemplo para demonstrar o que é possível com Blocos Dinâmicos adaptáveis e notificações do sistema interativas para ajudar na introdução. É possível testar todas as opções de texto diferente, grupos/subgrupos, imagens de plano de fundo, e você pode ver como o bloco se adapta a dispositivos e telas diferentes. Depois que tiver feito alterações, você poderá salvar a carga atualizada em um arquivo para uso futuro.

O editor fornece e erros e avisos em tempo real. Por exemplo, caso a carga do aplicativo for superior a 5 KB (uma limitação da plataforma), o Visualizador de Notificações avisa que a carga excede esse limite. Ele apresenta avisos para nomes de atributo ou valores incorretos, o que ajuda a depurar problemas visuais.

É possível controlar propriedades do bloco de controle, como nome de exibição, cor, logotipos, ShowName e valor de notificação. Essas opções ajudam a entender instantaneamente como propriedades e cargas de notificação de blocos interagem, além dos resultados que elas produzem.

Esta captura de tela do aplicativo mostra o editor de blocos:

![captura de tela do editor do Visualizador de Notificações com blocos](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Tópicos relacionados

* [Obtenha o Visualizador de Notificações na Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Criar blocos adaptáveis](create-adaptive-tiles.md)
* [Notificações do sistema interativas](adaptive-interactive-toasts.md)