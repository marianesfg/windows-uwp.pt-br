---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: "Esta seção explica como compartilhar dados entre aplicativos UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar."
title: "Comunicação de aplicativo para aplicativo"
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: dcd542257761083f3ec04eb0da2b13d5d68a19e2
ms.openlocfilehash: 63550064b6f31b85cd3b6fa2a09bac4b7cfcf895

---

# Comunicação de aplicativo para aplicativo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção explica como compartilhar dados entre aplicativos UWP (Plataforma Universal do Windows), incluindo como usar o contrato de Compartilhamento, copiar e colar, e arrastar e soltar.

O contrato de Compartilhamento é uma maneira pela qual os usuários podem trocar dados rapidamente, de um aplicativo para outro. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde. Considere usar o contrato de Compartilhamento se seu aplicativo receber conteúdo em cenários que um usuário possa concluir rapidamente enquanto permanece em outro aplicativo.

Um aplicativo pode dar suporte ao recurso Compartilhar de duas maneiras. Uma delas pode ser um aplicativo de origem que fornece conteúdo que o usuário quer compartilhar. A segunda pode ser um aplicativo de destino que o usuário seleciona como o destino do conteúdo compartilhado. Um aplicativo também pode ser um aplicativo de origem e um aplicativo de destino. Se você quiser que o aplicativo compartilhe conteúdo como aplicativo de origem, decida quais formatos de dados o aplicativo pode fornecer.

Além do contrato de Compartilhamento, os aplicativos também podem integrar técnicas clássicas para transferência de dados, como arrastar e soltar ou copiar e colar. Além da comunicação entre aplicativos UWP, esses métodos também permitem o compartilhamento de e para aplicativos da área de trabalho.

## Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Compartilhar dados](share-data.md) | Este artigo explica como dar suporte ao contrato de Compartilhamento em um aplicativo UWP. O contrato de Compartilhamento é uma maneira fácil de compartilhar dados como texto, links, fotos e vídeos entre aplicativos rapidamente. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde. |
| [Receber dados](receive-data.md) | Este artigo explica como receber conteúdo em seu aplicativo UWP compartilhado de outro aplicativo usando o contrato de Compartilhamento. Este contrato de Compartilhamento permite que seu aplicativo seja apresentado como uma opção quando o usuário invoca o compartilhamento. |
| [Copiar e colar](copy-and-paste.md) | Este artigo explica como dar suporte a copiar e colar em aplicativos UWP usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre aplicativos ou dentro de um aplicativo, e quase todos os aplicativos podem suportar operações da área de transferência em algum grau. |
| [Arrastar e soltar](drag-and-drop.md) | Este artigo explica como adicionar o recurso de arrastar e soltar ao aplicativo UWP. Arrastar e soltar é uma maneira clássica e natural de interagir com conteúdo, como imagens e arquivos. Uma vez implementado, a função arrastar e soltar funciona diretamente em todas as direções, incluindo de aplicativo para aplicativo, de aplicativo para área de trabalho e de área de trabalho para aplicativo. |
| [Usar a EDP para proteger dados empresariais transferidos entre aplicativos](use-edp-to-protect-enterprise-data-transferred-between-apps.md) | Este tópico mostra exemplos das tarefas de codificação necessárias para obter alguns dos cenários mais comuns de EDP (proteção de dados empresariais) relacionados à transferência de arquivos. |



<!--HONumber=Jun16_HO4-->


