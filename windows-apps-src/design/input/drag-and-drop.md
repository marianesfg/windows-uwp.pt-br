---
description: Este artigo explica como adicionar o recurso de arrastar e soltar em seu aplicativo UWP (Plataforma Universal do Windows).
title: Arrastar e soltar
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5dde0607b76d36b24c851c705ca62f71df052f65
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5863665"
---
# <a name="drag-and-drop"></a>Arrastar e soltar

Arrastar e soltar é uma maneira intuitiva de transferir dados dentro de um aplicativo ou entre aplicativos na área de trabalho do Windows. A operação arrastar e soltar permite que o usuário transfira dados entre aplicativos ou dentro de um aplicativo usando um gesto padrão (pressionar, segurar e aplicar panorâmica com o dedo ou pressionar e aplicar panorâmica com um mouse ou uma caneta).

> **APIs importantes**: [propriedade CanDrag](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag), [propriedade AllowDrop](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 

A origem de arraste, que é o aplicativo ou a área em que o gesto de arrastar é acionado, fornece os dados a serem transferidos preenchendo um objeto de pacote de dados que pode conter formatos de dados padrão, incluindo texto, RTF, HTML, bitmaps, itens de armazenamento ou formatos de dados personalizados. A origem também indica o tipo de operações com suporte: copiar, mover ou vincular. Quando o ponteiro é liberado, o item é solto. A reprodução automática, que é o aplicativo ou a área sob o ponteiro, processa o pacote de dados e retorna o tipo de operação realizada.

Durante a operação arrastar e soltar, a interface de usuário do arraste fornece uma indicação visual do tipo de operação arrastar e soltar que está ocorrendo. Esse feedback visual é inicialmente fornecido pela origem, mas pode ser alterado pelos destinos conforme o ponteiro se move sobre eles.

A operação arrastar e soltar moderna está disponível em todos os dispositivos que oferecem suporte à UWP. Ela permite a transferência de dados entre ou dentro de qualquer tipo de aplicativo, incluindo aplicativos clássicos do Windows, embora este artigo se concentre na API de XAML para a operação arrastar e soltar moderna. Uma vez implementada, a operação arrastar e soltar funciona perfeitamente em todas as direções, incluindo de aplicativo para aplicativo, de aplicativo para área de trabalho e de área de trabalho para aplicativo.

Aqui está uma visão geral do que você precisa fazer para habilitar o recurso de arrastar e soltar no seu aplicativo:

1. Habilite a operação arrastar um elemento, definindo sua propriedade **CanDrag** como true.  
2. Criar o pacote de dados. O sistema manipula imagens e texto automaticamente, mas para outros tipos de conteúdo, você precisará manipular os eventos **DragStarted** e **DragCompleted** e usá-los para construir seu próprio pacote de dados. 
3. Habilite a operação soltar definindo a propriedade **AllowDrop** como **true** em todos os elementos que podem receber conteúdo solto. 
4. Manipule o evento **DragOver** para informar ao sistema que tipo de operações de arrastar o elemento pode receber. 
5. Processe o evento **Drop** para receber o conteúdo solto. 



## <a name="enable-dragging"></a>Habilitar o recurso arrastar

Para poder arrastar um elemento, defina sua propriedade [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) como **true**. Isso torna o elemento, e os elementos que ele contém, no caso de coleções como ListView, arrastáveis.

Seja específico sobre o que pode ser arrastado. Os usuários não vão querer arrastar tudo em seu aplicativo, apenas determinados itens, como imagens ou texto. 

Veja como definir [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag).

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Você não precisa fazer mais nada para permitir arrastar, a menos que queira personalizar a interface do usuário (o que é abordado posteriormente neste artigo). Para soltar são necessárias mais algumas etapas.

## <a name="construct-a-data-package"></a>Construir um pacote de dados 

Na maioria dos casos, o sistema construirá um pacote de dados para você. O sistema manipula automaticamente:
* Imagens
* Texto 

Para outros tipos de conteúdo, você precisará manipular os eventos **DragStarted** e **DragCompleted** e usá-los para construir seu próprio [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage).

## <a name="enable-dropping"></a>Habilitar o recurso soltar

A marcação a seguir mostra como definir uma área específica do aplicativo como válida para soltar usando [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) em XAML. Se um usuário tentar soltar em outro lugar, o sistema não permitirá. Se você quiser que os usuários possam soltar os itens em qualquer lugar no seu aplicativo, defina toda a tela de fundo como reprodução automática.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>Manipular o evento DragOver

O evento [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) é acionado quando um usuário arrasta um item sobre seu aplicativo, mas ainda não soltou. Neste manipulador, você precisa especificar a que tipo de operações de seu aplicativo dá suporte usando a propriedade [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation). A cópia é a operação mais comum.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Processar o evento Drop

O evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) ocorre quando o usuário libera itens em uma área de soltar válida. Processe-os usando a propriedade [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView).

Para simplificar o exemplo a seguir, vamos supor que o usuário soltou uma única foto e acessá-la diretamente. Na realidade, os usuários podem soltar vários itens de diferentes formatos simultaneamente. Seu aplicativo deve lidar com essa possibilidade verificando quais tipos de arquivos foram soltos e quantos existem, e processar cada um de forma apropriada. Você também deve considerar notificar o usuário se ele estiver tentando fazer algo sem suporte pelo seu aplicativo.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>Personalizar a interface do usuário

O sistema fornece uma interface do usuário padrão para arrastar e soltar. No entanto, você também pode optar por personalizar diversas partes da interface do usuário definindo legendas e glifos personalizados ou optando por não mostrar uma interface do usuário. Para personalizar a interface do usuário, use a propriedade [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride).

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Abra um menu de contexto em um item que você possa arrastar com o recurso touch

Usando o recurso touch, arraste um [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) e abra o menu de contexto semelhante a gestos de toque; cada um começa com um gesto pressionar e segurar. É assim que o sistema remove a ambiguidade entre as duas ações para os elementos em seu aplicativo que dão suporte para ambos: 

* Se um usuário pressiona e segura um item e começa a arrastá-lo dentro de 500 milissegundos, o item é arrastado e o menu de contexto não é mostrado. 
* Se o usuário pressiona e mantém, mas não arrasta dentro de 500 milissegundos, o menu de contexto é aberto. 
* Após o menu de contexto ser aberto, se o usuário tentar arrastar o item (sem levantar o dedo), o menu de contexto é ignorado e o arraste será iniciado.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Designar um item em ListView ou GridView como uma pasta

Você pode especificar um [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) ou [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) como uma pasta. Isso é particularmente útil para cenários de TreeView e Explorador de Arquivos. Para fazer isso, defina explicitamente a propriedade [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) como **True** nesse item. 

O sistema irá exibir automaticamente as animações apropriadas para soltar em uma pasta em vez de um item que não pertence a pasta. O código do aplicativo deve continuar a manipular o evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) no item da pasta (bem como no item que não pertence a pasta) para atualizar a fonte de dados e adicionar o item solto para a pasta de destino.

## <a name="implementing-custom-drag-and-drop"></a>Implementar o recurso arrastar e soltar personalizado

A classe [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) faz a maior parte do trabalho de implementação do recurso arrastar e soltar para você. Mas se você quiser, você pode implementar sua própria versão usando as APIs no [namespace DragDrop](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core).

| Funcionalidade | API do WinRT |
| --- | --- |
|  Habilitar o recurso arrastar | [CoreDragOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Criar um pacote de dados | [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Entregar item arrastado para o shell  | [CoreDragOperation.StartAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Receber item solto do shell  | [CoreDragDropManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Veja também

* [Comunicação de aplicativo a aplicativo](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUiOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
