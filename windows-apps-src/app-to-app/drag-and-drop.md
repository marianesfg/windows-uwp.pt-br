---
description: Este artigo explica como adicionar o recurso de arrastar e soltar em seu aplicativo UWP (Plataforma Universal do Windows).
title: Arrastar e soltar
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
---
# Arrastar e soltar

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo explica como adicionar o recurso de arrastar e soltar em seu aplicativo UWP (Plataforma Universal do Windows). Arrastar e soltar é uma maneira clássica e natural de interagir com conteúdo, como imagens e arquivos. Uma vez implementada, a função arrastar e soltar funciona diretamente em todas as direções, incluindo de aplicativo para aplicativo, de aplicativo para área de trabalho e de área de trabalho para aplicativo.

## Definir áreas válidas

Use as propriedades [**AllowDrop**][AllowDrop] e [**CanDrag**][CanDrag] para designar as áreas de seu aplicativo válidas para arrastar e soltar.

A marcação a seguir mostra como definir uma área específica do aplicativo como válida para soltar usando [**AllowDrop**][AllowDrop] em XAML. Se um usuário tentar soltar em outro lugar, o sistema não permitirá. Se você quiser que os usuários possam soltar os itens em qualquer lugar no seu aplicativo, defina toda a tela de fundo como reprodução automática.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

Com arrastar, você geralmente deseja ser específico sobre o que pode ser arrastado. Os usuários querem arrastar determinados itens, como imagens, mas nem tudo em seu aplicativo. Veja a seguir como definir [**CanDrag**][CanDrag] usando XAML.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Você não precisa fazer mais nada para permitir arrastar, a menos que queira personalizar a interface do usuário (o que é abordado posteriormente neste artigo). Soltar requer mais algumas etapas.

## Manipular o evento DragOver

O evento [**DragOver**][DragOver] é acionado quando um usuário arrasta um item sobre seu aplicativo, mas ainda não soltou. Neste manipulador, você precisa especificar a que tipo de operações de seu aplicativo dá suporte usando a propriedade [**DragEventArgs.AcceptedOperation**][AcceptedOperation]. A cópia é a operação mais comum.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## Processar o evento Drop

O evento [**Drop**][Drop] ocorre quando o usuário libera itens em uma área de soltar válida. Processe-os usando a propriedade [**DragEventArgs.DataView**][DataView].

Para simplificar o exemplo a seguir, vamos supor que o usuário soltou uma única foto e acesso. Na realidade, os usuários podem soltar vários itens de diferentes formatos simultaneamente. Seu aplicativo deve manipular essa possibilidade verificando quais tipos de arquivos foram soltos e processando-os de acordo e notificando o usuário se ele estiver tentando fazer algo sem suporte pelo seu aplicativo.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## Personalizar a interface do usuário

O sistema fornece uma interface do usuário padrão para arrastar e soltar. No entanto, você também pode optar por personalizar diversas partes da interface do usuário definindo legendas e glifos personalizados ou optando por não mostrar uma interface do usuário. Para personalizar a interface do usuário, use a propriedade [**DragUIOverride**][DragUiOverride] no manipulador de eventos [**DragOver**][DragOver].

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

 <!-- LINKS -->
[AllowDrop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx
[CanDrag]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx
[DragOver]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx
[AcceptedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx
[DataView]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx
[DragUiOverride]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx
[Drop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx 



<!--HONumber=Mar16_HO5-->


