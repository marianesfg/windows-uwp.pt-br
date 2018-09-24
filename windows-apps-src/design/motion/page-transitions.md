---
author: QuinnRadich
Description: Learn how to use page transitions in your UWP apps.
title: Transições de página em aplicativos UWP
template: detail.hbs
ms.author: quradic
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: 0afc2c55ab0d0bdd2bee0206f986b2724d331eaf
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4152645"
---
# <a name="page-transitions"></a>Transições de página

As transições de página navegam entre as páginas de um app, fornecendo comentários como o relacionamento entre as páginas. As transições de página ajudam os usuários a compreender se eles estão na parte superior de uma hierarquia de navegação, movendo entre as páginas irmãs ou navegando pela hierarquia da página.

Duas animações diferentes são fornecidas para navegação entre páginas em um aplicativo, *Atualização de página* e *Análise detalhada*, e são representadas por subclasses de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="page-refresh"></a>Atualização de página

A atualização de página é a combinação de uma animação de deslizamento para cima e uma animação de fade in para o conteúdo de entrada. Use a atualização de página quando o usuário for levado à parte superior de uma pilha de navegação, como navegar entre guias ou itens de navegação esquerda.

A sensação desejada é que o usuário tenha recomeçado.

![animação de atualização de página](images/page-refresh.gif)

A animação de atualização de página é representada pelo [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Observação**: um [**Quadro**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) usa automaticamente [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) para animar a navegação entre duas páginas. Por padrão, a animação é a atualização de página.

## <a name="drill"></a>Análise detalhada

Use a análise detalhada quando os usuários navegarem mais profundamente em um app, por exemplo, quando estiverem exibindo mais informações após selecionar um item.

A sensação desejada é que o usuário tenha entrado mais profundamente no aplicativo.

![animação de análise detalhada](images/drill.gif)

A animação de análise detalhada é representada pela classe [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>Supressão

Para evitar a reprodução de qualquer animação durante a navegação, use [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) em vez de outros subtipos de **NavigationTransitionInfo**.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

A supressão da animação será útil se você estiver criando sua própria transição por meio das [Animações conectadas](connected-animation.md) ou animações de mostrar/ocultar implícitas.

## <a name="backwards-navigation"></a>Navegação para trás

Para reproduzir uma transição específica ao navegar para trás, use `Frame.GoBack(NavigationTransitionInfo)`.

Isso poderá ser útil quando você modificar o comportamento de navegação dinamicamente com base no tamanho da tela, por exemplo, em um cenário mestre/de detalhes dinâmico.

## <a name="related-topics"></a>Tópicos relacionados

- [Navegar entre duas páginas](../basics/navigate-between-two-pages.md)
- [Movimento em aplicativos UWP](index.md)
