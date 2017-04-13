---
description: Este artigo explica como adicionar o recurso de arrastar e soltar em seu aplicativo UWP (Plataforma Universal do Windows).
title: Arrastar e soltar
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 84f78d43a0d9a34b8ba992a2357f08ad374b32d1
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="drag-and-drop"></a>Arrastar e soltar

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo explica como adicionar o recurso de arrastar e soltar em seu aplicativo UWP (Plataforma Universal do Windows). Arrastar e soltar é uma maneira clássica e natural de interagir com conteúdo, como imagens e arquivos. Uma vez implementada, a função arrastar e soltar funciona diretamente em todas as direções, incluindo de aplicativo para aplicativo, de aplicativo para área de trabalho e de área de trabalho para aplicativo.

## <a name="set-valid-areas"></a>Definir áreas válidas

Use as propriedades [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) e [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) para designar as áreas de seu aplicativo válidas para arrastar e soltar.

A marcação a seguir mostra como definir uma área específica do aplicativo como válida para soltar usando [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) em XAML. Se um usuário tentar soltar em outro lugar, o sistema não permitirá. Se você quiser que os usuários possam soltar os itens em qualquer lugar no seu aplicativo, defina toda a tela de fundo como reprodução automática.

[!code-xml[Principal](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

Com arrastar, você geralmente deseja ser específico sobre o que pode ser arrastado. Os usuários querem arrastar determinados itens, como imagens, mas nem tudo em seu aplicativo. Veja a seguir como definir [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) usando XAML.

[!code-xml[Principal](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Você não precisa fazer mais nada para permitir arrastar, a menos que queira personalizar a interface do usuário (o que é abordado posteriormente neste artigo). Soltar requer mais algumas etapas.

## <a name="handle-the-dragover-event"></a>Manipular o evento DragOver

O evento [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) é acionado quando um usuário arrasta um item sobre seu aplicativo, mas ainda não soltou. Neste manipulador, você precisa especificar a que tipo de operações de seu aplicativo dá suporte usando a propriedade [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation). A cópia é a operação mais comum.

[!code-cs[Principal](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Processar o evento Drop

O evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) ocorre quando o usuário libera itens em uma área de soltar válida. Processe-os usando a propriedade [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView).

Para simplificar o exemplo a seguir, vamos supor que o usuário soltou uma única foto e acesso. Na realidade, os usuários podem soltar vários itens de diferentes formatos simultaneamente. Seu aplicativo deve lidar com essa possibilidade verificando quais tipos de arquivos foram soltos e processando-os de acordo e notificando o usuário se ele estiver tentando fazer algo sem suporte pelo seu aplicativo.

[!code-cs[Principal](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>Personalizar a interface do usuário

O sistema fornece uma interface do usuário padrão para arrastar e soltar. No entanto, você também pode optar por personalizar diversas partes da interface do usuário definindo legendas e glifos personalizados ou optando por não mostrar uma interface do usuário. Para personalizar a interface do usuário, use a propriedade [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride).

[!code-cs[Principal](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Abra um menu de contexto em um item que você possa arrastar com o recurso touch

Usando o recurso touch, arraste um [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) e abra o menu de contexto semelhante a gestos de toque; cada um começa com um gesto pressionar e segurar. É assim que o sistema remove a ambiguidade entre as duas ações para os elementos em seu aplicativo que dão suporte para ambos: 

* Se um usuário pressiona e segura um item e começa a arrastá-lo dentro de 500 milissegundos, o item é arrastado e o menu de contexto não é mostrado. 
* Se o usuário pressiona e mantém, mas não arrasta dentro de 500 milissegundos, o menu de contexto é aberto. 
* Após o menu de contexto ser aberto, se o usuário tentar arrastar o item (sem levantar o dedo), o menu de contexto é ignorado e o arraste será iniciado.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Designar um item em ListView ou GridView como uma pasta

Você pode especificar um [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) ou [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) como uma pasta. Isso é particularmente útil para cenários de TreeView e Explorador de Arquivos. Para fazer isso, defina explicitamente a propriedade [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) como **True** nesse item. 

O sistema irá exibir automaticamente as animações apropriadas para soltar em uma pasta em vez de um item que não pertence a pasta. O código do aplicativo deve continuar a manipular o evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) no item da pasta (bem como no item que não pertence a pasta) para atualizar a fonte de dados e adicionar o item solto para a pasta de destino.

## <a name="see-also"></a>Consulte também

* [Comunicação de aplicativo a aplicativo](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUiOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
