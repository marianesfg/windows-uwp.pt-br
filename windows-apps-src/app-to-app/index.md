---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: Esta seção explica como compartilhar dados entre aplicativos UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar.
title: Comunicação de app a app
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9dfd86d53805573d002984aaf33ba5f1bf17241c
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8324886"
---
# <a name="app-to-app-communication"></a>Comunicação entre apps


Esta seção explica como compartilhar dados entre aplicativos UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar.

O contrato de Compartilhamento é uma maneira pela qual os usuários podem trocar dados rapidamente, de um aplicativo para outro. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde. Considere usar o contrato de Compartilhamento se seu aplicativo receber conteúdo em cenários que um usuário possa concluir rapidamente enquanto permanece em outro aplicativo.

Um aplicativo pode dar suporte ao recurso Compartilhar de duas maneiras. Uma delas pode ser um aplicativo de origem que fornece conteúdo que o usuário quer compartilhar. A segunda pode ser um aplicativo de destino que o usuário seleciona como o destino do conteúdo compartilhado. Um aplicativo também pode ser um aplicativo de origem e um aplicativo de destino. Se você quiser que o aplicativo compartilhe conteúdo como aplicativo de origem, decida quais formatos de dados o aplicativo pode fornecer.

Além do contrato de Compartilhamento, os aplicativos também podem integrar técnicas clássicas para transferência de dados, como arrastar e soltar ou copiar e colar. Além da comunicação entre aplicativos UWP, esses métodos também permitem o compartilhamento de e para aplicativos da área de trabalho.



## <a name="in-this-section"></a>Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Compartilhar dados](share-data.md) | Este artigo explica como dar suporte ao contrato de Compartilhamento em um aplicativo UWP. O contrato de Compartilhamento é uma maneira fácil de compartilhar dados como texto, links, fotos e vídeos entre aplicativos rapidamente. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde. |
| [Receber dados](receive-data.md) | Este artigo explica como receber conteúdo em seu aplicativo UWP compartilhado de outro aplicativo usando o contrato de Compartilhamento. Este contrato de Compartilhamento permite que seu aplicativo seja apresentado como uma opção quando o usuário invoca o compartilhamento. |
| [Copiar e colar](copy-and-paste.md) | Este artigo explica como dar suporte a copiar e colar em aplicativos UWP usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre aplicativos ou dentro de um aplicativo, e quase todos os aplicativos podem suportar operações da área de transferência em algum grau. |
| [Arrastar e soltar](../design/input/drag-and-drop.md) | Este artigo explica como adicionar o recurso de arrastar e soltar ao aplicativo UWP. Arrastar e soltar é uma maneira clássica e natural de interagir com conteúdo, como imagens e arquivos. Uma vez implementada, a função arrastar e soltar funciona diretamente em todas as direções, incluindo de aplicativo para aplicativo, de aplicativo para área de trabalho e de área de trabalho para aplicativo. |

## <a name="see-also"></a>Consulte também
- [Desenvolver aplicativos UWP](https://developer.microsoft.com/windows/develop)
