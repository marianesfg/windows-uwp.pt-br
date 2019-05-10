---
Description: Saiba como usar as transições de página em seus aplicativos UWP.
title: Transições de página em aplicativos UWP
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9b3244c24ff4fa8e3c85ee9970536b1b35d8efd5
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444196"
---
# <a name="page-transitions"></a>Transições de página

As transições de página navegam entre as páginas de um app, fornecendo comentários como o relacionamento entre as páginas. As transições de página ajudam os usuários a compreender se eles estão na parte superior de uma hierarquia de navegação, movendo entre as páginas irmãs ou navegando pela hierarquia da página.

Duas animações diferentes são fornecidas para navegação entre páginas em um aplicativo, *Atualização de página* e *Análise detalhada*, e são representadas por subclasses de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para <a href="xamlcontrolsgallery:/item/PageTransition">abrir o aplicativo e ver as transições de página em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>Atualização de página

A atualização de página é a combinação de uma animação de deslizamento para cima e uma animação de fade in para o conteúdo de entrada. Use a atualização de página quando o usuário for levado à parte superior de uma pilha de navegação, como navegar entre guias ou itens de navegação esquerda.

A sensação desejada é que o usuário tenha recomeçado.

![animação de atualização de página](images/page-refresh.gif)

A animação de atualização de página é representada pelo [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Observação**: Um [ **quadro** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) usa automaticamente [ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) para animar a navegação entre as duas páginas. Por padrão, a animação é a atualização de página.

## <a name="drill"></a>Análise detalhada

Use a análise detalhada quando os usuários navegarem mais profundamente em um app, por exemplo, quando estiverem exibindo mais informações após selecionar um item.

A sensação desejada é que o usuário tenha entrado mais profundamente no aplicativo.

![animação de análise detalhada](images/drill.gif)

A animação de análise detalhada é representada pela classe [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Slide horizontal

Use o slide de horizontal para mostrar que as páginas irmãs aparecem próximos uns dos outros. O [NavigationView](../controls-and-patterns/navigationview.md) controle usa automaticamente essa animação de navegação superior, mas se você estiver criando sua própria experiência de navegação horizontal, em seguida, você pode implementar slide horizontal com SlideNavigationTransitionInfo.

A sensação desejada é que o usuário está navegando entre as páginas que estão próximos uns dos outros. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
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

- [Navegar entre as duas páginas](../basics/navigate-between-two-pages.md)
- [Movimento em aplicativos UWP](index.md)
