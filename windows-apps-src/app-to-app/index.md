---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: "Esta seção explica como compartilhar dados entre apps UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar."
title: "Comunicação de app a app"
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4e8c90996324e7481a7a4dab5a1ff3f30b139276
ms.lasthandoff: 02/07/2017

---

# <a name="app-to-app-communication"></a>Comunicação de app a app

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção explica como compartilhar dados entre apps UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar.

O contrato de Compartilhamento é uma maneira pela qual os usuários podem trocar dados rapidamente, de um app para outro. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um app de rede social ou salvar um link em um app de anotações para consultar mais tarde. Considere usar o contrato de Compartilhamento se seu app receber conteúdo em cenários que um usuário possa concluir rapidamente enquanto permanece em outro app.

Um app pode dar suporte ao recurso Compartilhar de duas maneiras. Uma delas pode ser um app de origem que fornece conteúdo que o usuário quer compartilhar. A segunda pode ser um app de destino que o usuário seleciona como o destino do conteúdo compartilhado. Um app também pode ser um app de origem e um app de destino. Se você quiser que o app compartilhe conteúdo como app de origem, decida quais formatos de dados o app pode fornecer.

Além do contrato de Compartilhamento, os apps também podem integrar técnicas clássicas para transferência de dados, como arrastar e soltar ou copiar e colar. Além da comunicação entre apps UWP, esses métodos também permitem o compartilhamento de e para apps da área de trabalho.



## <a name="in-this-section"></a>Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Compartilhar dados](share-data.md) | Este artigo explica como dar suporte ao contrato de Compartilhamento em um app UWP. O contrato de Compartilhamento é uma maneira fácil de compartilhar dados como texto, links, fotos e vídeos entre apps rapidamente. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um app de rede social ou salvar um link em um app de anotações para consultar mais tarde. |
| [Receber dados](receive-data.md) | Este artigo explica como receber conteúdo em seu app UWP compartilhado de outro app usando o contrato de Compartilhamento. Este contrato de Compartilhamento permite que seu app seja apresentado como uma opção quando o usuário invoca o compartilhamento. |
| [Copiar e colar](copy-and-paste.md) | Este artigo explica como dar suporte a copiar e colar em apps UWP usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre apps ou dentro de um app, e quase todos os apps podem suportar operações da área de transferência em algum grau. |
| [Arrastar e soltar](drag-and-drop.md) | Este artigo explica como adicionar o recurso de arrastar e soltar ao app UWP. Arrastar e soltar é uma maneira clássica e natural de interagir com conteúdo, como imagens e arquivos. Uma vez implementada, a função arrastar e soltar funciona diretamente em todas as direções, incluindo de app para app, de app para área de trabalho e de área de trabalho para app. |

## <a name="see-also"></a>Consulte também
- [Desenvolver apps UWP](https://developer.microsoft.com/windows/develop)

