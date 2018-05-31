---
author: serenaz
Description: Learn how to use page transitions in your UWP apps.
title: Transições de página em aplicativos UWP
template: detail.hbs
ms.author: sezhen
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: dc42199eba00071f5dbabd83a4ae524298a619ee
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818202"
---
# <a name="page-transitions"></a>Transições de página

As transições de página são animações reproduzidas quando os usuários navegam entre as páginas de um app, fornecendo comentários como o relacionamento entre as páginas. As transições de página ajudam os usuários a compreender se eles estão na parte superior de uma hierarquia de navegação, movendo entre as páginas irmãs ou navegando pela hierarquia da página.

Duas animações diferentes são fornecidas para navegação entre páginas em um app, *atualização de página* e *análise detalhada*, e são representadas por subclasses de [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="page-refresh"></a>Atualização de página

A atualização de página é a combinação de uma animação de deslizamento para cima e uma animação de fade in para o conteúdo de entrada. A sensação desejada é que o usuário tenha recomeçado.

Use a atualização de página quando o usuário for levado à parte superior de uma pilha de navegação, como navegar entre [guias](../controls-and-patterns/tabs-pivot.md) ou itens de [navegação esquerda](../controls-and-patterns/navigationview.md). Por padrão, [Frame.Navigate()](/uwp/api/windows.ui.xaml.controls.frame.navigate) usa a atualização de página.

![animação de atualização de página](images/page-refresh.gif)

A animação de atualização de página é representada pelo [EntranceNavigationTransitionInfoClass](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());
```

## <a name="drill"></a>Análise detalhada

Use a análise detalhada quando os usuários navegarem mais profundamente em um app, por exemplo, quando estiverem exibindo mais informações após selecionar um item.

A sensação desejada é que o usuário tenha entrado mais profundamente no aplicativo.

![animação de análise detalhada](images/drill.gif)

A animação de análise detalhada é representada pela classe [DrillInNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>Supressão

A supressão da animação será útil se você estiver criando sua própria transição por meio das [Animações conectadas](connected-animation.md) ou animações de mostrar/ocultar implícitas.

Para evitar a reprodução de qualquer animação durante a navegação, use [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) no lugar de outros subtipos [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

## <a name="backwards-navigation"></a>Navegação para trás

Por padrão, [Frame.GoBack()](/uwp/api/windows.ui.xaml.controls.frame.goback) reproduz a animação de "navegação para trás" correspondente com base na animação reproduzida para navegar até a página. Por exemplo, um app que usa drill in para navegar até uma página verá um drill out quando os usuários navegarem para trás.

Para reproduzir uma transição específica ao navegar para trás, use `Frame.GoBack(NavigationTransitionInfo)`. Isso poderá ser útil quando você modificar o comportamento de navegação dinamicamente com base no tamanho da tela, por exemplo, em um cenário mestre/de detalhes dinâmico.

## <a name="related-topics"></a>Tópicos relacionados

- [Navegar entre duas páginas](../basics/navigate-between-two-pages.md)
- [Movimento em aplicativos UWP](index.md)
