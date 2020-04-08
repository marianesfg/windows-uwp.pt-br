---
description: Este artigo discute cenários avançados de hospedagem da Ilha XAML para aplicativos do C++ Win32.
title: Cenários avançados das Ilhas XAML em aplicativos C++ Win32
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml islands, wrapped controls, standard controls
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226230"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Cenários avançados das Ilhas XAML em aplicativos C++ Win32

Os artigos [Hospedar um controle UWP padrão](host-standard-control-with-xaml-islands-cpp.md) e [Hospedar um controle UWP personalizado](host-custom-control-with-xaml-islands-cpp.md) fornecem instruções e exemplos para hospedar Ilhas XAML em um aplicativo do C++ Win32. No entanto, os exemplos de código desses artigos não tratam de muitos cenários avançados que os aplicativos da área de trabalho podem precisar manipular para fornecer uma experiência de usuário agradável. Este artigo fornece diretrizes para alguns desses cenários e ponteiros para exemplos de código relacionados.

## <a name="keyboard-input"></a>Entrada por teclado

Para lidar corretamente com a entrada de teclado para cada Ilha XAML, seu aplicativo deve passar todas as mensagens do Windows para a estrutura UWP XAML para que determinadas mensagens possam ser processadas corretamente. Para fazer isso, em algum lugar do aplicativo que possa acessar o loop de mensagem, converta o objeto **DesktopWindowXamlSource** para cada Ilha XAML em uma interface COM **IDesktopWindowXamlSourceNative2**. Em seguida, chame o método **PreTranslateMessage** dessa interface e passe a mensagem atual.

  * **C++ Win32**: O aplicativo pode chamar **PreTranslateMessage** diretamente em seu loop de mensagem principal. Para obter um exemplo, veja o arquivo [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16).

  * **WPF:** O aplicativo pode chamar **PreTranslateMessage** do manipulador de eventos para o evento [ComponentDispatcher.ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage). Para obter um exemplo, confira o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) no Windows Community Toolkit.

  * **Windows Forms:** O aplicativo pode chamar **PreTranslateMessage** de uma substituição para o método [Control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage). Para obter um exemplo, confira o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) no Windows Community Toolkit.

## <a name="keyboard-focus-navigation"></a>Navegação de foco de teclado

Quando o usuário navega pelos elementos da interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando **Tab** ou tecla de direção/seta), você precisará mover o foco programaticamente para dentro e para fora do objeto **DesktopWindowXamlSource**. Quando a navegação do teclado do usuário alcançar o **DesktopWindowXamlSource**, mova o foco para o primeiro objeto [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) na ordem de navegação da sua interface do usuário, continue a mover o foco para os objetos **Windows.UI.Xaml.UIElement** posteriores, conforme o usuário percorre os elementos e, em seguida, move o foco de volta para o **DesktopWindowXamlSource** e para o elemento de interface do usuário pai.  

A API de hospedagem UWP XAML oferece vários tipos e membros para ajudá-lo a realizar essas tarefas.

* Quando a navegação do teclado entrar no **DesktopWindowXamlSource**, o evento [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) será gerado. Manipule esse evento e mova o foco por meio de programação para o primeiro **Windows.UI.Xaml.UIElement** hospedado usando o método [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus).

* Quando o usuário estiver no último elemento que pode ser focado em seu **DesktopWindowXamlSource** e pressionar a tecla **TAB** ou uma tecla de direção, o evento [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) será gerado. Manipule esse evento e mova o foco programaticamente para o próximo elemento que pode ser focado no aplicativo host. Por exemplo, em um aplicativo WPF em que o **DesktopWindowXamlSource** está hospedado em um [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o método [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir o foco para o próximo elemento que pode ser focalizado no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, confira os seguintes arquivos de código:

  * **C++/Win32**: Confira o arquivo [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp).

  * **WPF:** Veja o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) no Windows Community Toolkit.  

  * **Windows Forms:** Veja o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) no Windows Community Toolkit.

## <a name="handle-layout-changes"></a>Manipular alterações de layout

Quando o usuário alterar o tamanho do elemento pai da interface do usuário, você precisará lidar com as alterações de layout necessárias para garantir que os controles UWP sejam exibidos como esperado. Aqui estão alguns cenários importantes a considerar.

* Em um aplicativo do C++ Win32, quando seu aplicativo manipula a mensagem WM_SIZE, ele pode reposicionar a Ilha XAML hospedada usando a função [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos). Para obter um exemplo, confira o arquivo de código [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170).

* Quando o elemento pai da interface do usuário precisa obter o tamanho da área retangular necessária para ajustar o **Windows.UI.Xaml.UIElement** que você está hospedando no **DesktopWindowXamlSource**, chame o método [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) do **Windows.UI.Xaml.UIElement**. Por exemplo:

    * Em um aplicativo WPF, você pode fazer isso por meio do método [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) do [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, confira o arquivo [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no Windows Community Toolkit.

    * Em um aplicativo do Windows Forms, você pode fazer isso por meio do método [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) do [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, confira o arquivo [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no Windows Community Toolkit.

* Quando o tamanho do elemento pai da interface do usuário é alterado, chame o método [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) do **Windows.UI.Xaml.UIElement** raiz que você está hospedando no **DesktopWindowXamlSource**. Por exemplo:

    * Em um aplicativo WPF, você pode fazer isso por meio do método [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) do objeto [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, confira o arquivo [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no Windows Community Toolkit.

    * Em um aplicativo do Windows Forms, você pode fazer isso por meio do manipulador do evento [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) do [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, confira o arquivo [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no Windows Community Toolkit.

## <a name="handle-dpi-changes"></a>Manipular alterações de DPI

A estrutura UWP XAML manipula alterações de DPI para controles UWP hospedados automaticamente (por exemplo, quando o usuário arrasta a janela entre monitores com DPI de tela diferente). Para obter a melhor experiência, recomendamos que seu aplicativo do Windows Forms, WPF ou C++ Win32 esteja configurado para ter reconhecimento de DPI por monitor.

Para configurar seu aplicativo para ter reconhecimento de DPI por monitor, adicione um [manifesto do assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e defina o elemento **\<dpiAwareness\>** como **PerMonitorV2**. Para obter mais informações sobre esse valor, confira a descrição de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles UWP XAML em aplicativos da área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Como usar a API de Hospedagem UWP XAML em um aplicativo C++ Win32](using-the-xaml-hosting-api.md)
* [Hospedar um controle UWP padrão em um aplicativo C++ Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar um controle UWP personalizado em um aplicativo C++ Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Exemplos de código de Ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [Exemplo de ilhas XAML do C++ Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
