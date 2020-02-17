---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: Esta seção explica como compartilhar dados entre aplicativos UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar.
title: Comunicação de aplicativo a aplicativo
ms.date: 02/12/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2de9d5ae04c526c7c35fda4a7aef713814b4fbc2
ms.sourcegitcommit: 366c96d16e9a330bd4fbd9e1e19983f890f72642
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77258332"
---
# <a name="app-to-app-communication"></a>Comunicação de aplicativo a aplicativo

Esta seção explica como compartilhar dados entre aplicativos da UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento e os serviços de aplicativo, além de como copiar e colar e arrastar e soltar.

O contrato de Compartilhamento é uma maneira pela qual os usuários podem trocar dados rapidamente, de um aplicativo para outro. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde. Considere usar o contrato de Compartilhamento se seu aplicativo receber conteúdo em cenários que um usuário possa concluir rapidamente enquanto permanece em outro aplicativo.

Um aplicativo pode dar suporte ao recurso Compartilhar de duas maneiras. A primeira delas pode ser um [aplicativo de origem que fornece conteúdo que o usuário quer compartilhar](share-data.md). A segunda pode ser um [aplicativo de destino que o usuário seleciona como o destino do conteúdo compartilhado](receive-data.md). Um aplicativo também pode ser um aplicativo de origem e um aplicativo de destino. Se você quiser que o aplicativo compartilhe conteúdo como aplicativo de origem, decida quais formatos de dados o aplicativo pode fornecer.

Além do contrato de Compartilhamento, os aplicativos também podem integrar técnicas clássicas para transferência de dados, como [arrastar e soltar](../design/input/drag-and-drop.md) ou [copiar e colar](copy-and-paste.md). Além da comunicação entre aplicativos UWP, esses métodos também permitem o compartilhamento de e para aplicativos da área de trabalho.

Os aplicativos UWP também podem criar [serviços de aplicativos](../launch-resume/how-to-create-and-consume-an-app-service.md) que forneçam funcionalidade a outros aplicativos UWP. Um serviço de aplicativo é executado como uma tarefa em segundo plano no aplicativo host e pode fornecer seu serviço a outros aplicativos. Por exemplo, um serviço de aplicativo pode fornecer um serviço de scanner de código de barras que outros aplicativos possam usar.

## <a name="in-this-section"></a>Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Compartilhar dados](share-data.md) | Este artigo explica como dar suporte ao contrato de Compartilhamento em um aplicativo UWP. O contrato de Compartilhamento é uma maneira fácil de compartilhar dados como texto, links, fotos e vídeos entre aplicativos rapidamente. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde. |
| [Receber dados](receive-data.md) | Este artigo explica como receber conteúdo em seu aplicativo UWP compartilhado de outro aplicativo usando o contrato de Compartilhamento. Este contrato de Compartilhamento permite que seu aplicativo seja apresentado como uma opção quando o usuário invoca o compartilhamento. |
| [Copiar e colar](copy-and-paste.md) | Este artigo explica como dar suporte a copiar e colar em aplicativos UWP usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre aplicativos ou dentro de um aplicativo, e quase todos os aplicativos podem suportar operações da área de transferência em algum grau. |
| [Arrastar e soltar](../design/input/drag-and-drop.md) | Este artigo explica como adicionar o recurso de arrastar e soltar ao aplicativo UWP. Arrastar e soltar é uma maneira clássica e natural de interagir com conteúdo, como imagens e arquivos. Uma vez implementada, a função arrastar e soltar funciona diretamente em todas as direções, incluindo de aplicativo para aplicativo, de aplicativo para área de trabalho e de área de trabalho para aplicativo. |
| [Criar e consumir um serviço de aplicativo](../launch-resume/how-to-create-and-consume-an-app-service.md) | Este artigo explica como criar um serviço de aplicativo em um aplicativo UWP para fornecer serviços a outros aplicativos UWP.  |

## <a name="see-also"></a>Veja também

- [Desenvolver aplicativos UWP](https://docs.microsoft.com/windows/uwp/develop/)
