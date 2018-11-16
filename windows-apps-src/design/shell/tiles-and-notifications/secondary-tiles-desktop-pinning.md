---
author: vladimp
Description: Windows desktop applications can pin secondary tiles thanks to the Desktop Bridge!
title: Fixar blocos secundários do aplicativo da área de trabalho
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: mijacobs
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, ponte de desktop, blocos secundários, fixar, fixando, guia de início rápido, exemplo de código, exemplo, secondarytile, aplicativo da área de trabalho, win32, winforms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 44e37b47e22d10f509afd5d7503fa8f7a43ab365
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995747"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Fixar blocos secundários do aplicativo da área de trabalho


Graças à [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop), os aplicativos da área de trabalho do Windows (como Win32, Windows Forms e WPF) podem fixar blocos secundários!

![Captura de tela de blocos secundários](images/secondarytiles.png)

> [!IMPORTANT]
> **Requer a Fall Creators Update**: você deve usar o SDK 16299 e executar o build 16299 ou posterior para marcar blocos secundários nos aplicativos de Ponte de Desktop.

Adicionar um bloco secundário do aplicativo WPF ou WinForms é muito parecido com um aplicativo UWP puro. A única diferença é que você deve especificar o identificador da janela principal (HWND). Isso ocorre porque, ao fixar um bloco, o Windows exibe uma caixa de diálogo modal solicitando que o usuário confirme se deseja fixar o bloco. Se o aplicativo da área de trabalho não configurar o objeto SecondaryTile com a janela do proprietário, o Windows não saberá onde exibir a caixa de diálogo, e a operação falhará.


## <a name="package-your-app-with-desktop-bridge"></a>Empacotar o aplicativo com a Ponte de Desktop

Se você não tiver empacotado o aplicativo com a Ponte de Desktop, [deverá fazer isso primeiro](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) para conseguir usar quaisquer APIs UWP.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Habilitar acesso à interface IInitializeWithWindow

Se o aplicativo estiver escrito em uma linguagem gerenciada, como C# ou Visual Basic, declare a interface IInitializeWithWindow no código do aplicativo com o atributo [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) e Guid conforme mostrado no exemplo em C# a seguir. Esse exemplo pressupõe que o arquivo de código tenha uma instrução using para o namespace System.Runtime.InteropServices

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Como alternativa, se você estiver usando C++, adicione uma referência ao arquivo de cabeçalho **shobjidl.h** no código. Esse arquivo de cabeçalho contém a declaração da interface *IInitializeWithWindow*.


## <a name="initialize-the-secondary-tile"></a>Inicializar o bloco secundário

Inicialize um novo objeto de bloco secundário exatamente como você faria com um aplicativo UWP normal. Para saber mais sobre como criar e fixar blocos secundários, consulte [Fixar blocos secundários](secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Atribuir o identificador de janela

Esta é a etapa principal para aplicativos da área de trabalho. Converta o objeto em um objeto [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Em seguida, chame o método [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) e passe o identificador da janela da qual você deseja ser o proprietário para a caixa de diálogo modal. O exemplo em C# a seguir mostra como passar o identificador da janela principal do aplicativo para o método.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Fixar o bloco

Por fim, faça a solicitação para fixar o bloco como você faria com um aplicativo UWP normal.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Enviar notificações de bloco

> [!IMPORTANT]
> **Requer a versão 17134.81 de abril de 2018 ou posterior**: você deve executar a versão 17134.81 ou posterior para enviar as notificações de bloco para blocos secundários pelos aplicativos da Ponte de Desktop. Antes da atualização de serviço .81, uma exceção de 0x80070490 *Elemento não encontrado* não ocorreria ao enviar notificações de bloco para blocos secundários pelos aplicativos de Ponte de Desktop.

O envio de notificações de bloco ou selo é o mesmo para aplicativos UWP. Consulte [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md) para começar.


## <a name="resources"></a>Recursos

* [Exemplo de código completo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Visão geral de blocos secundários](secondary-tiles.md)
* [Fixar blocos secundários (UWP)](secondary-tiles-pinning.md)
* [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop)
* [Exemplos de código da Ponte de Desktop](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)