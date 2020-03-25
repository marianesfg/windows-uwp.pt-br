---
description: Este artigo discute cenários avançados de hospedagem da ilha XAML C++ para aplicativos Win32.
title: Cenários avançados para ilhas XAML em C++ aplicativos Win32
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, Ilhas XAML, controles encapsulados, controles padrão
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226230"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Cenários avançados para ilhas XAML em C++ aplicativos Win32

O [host a um controle UWP padrão](host-standard-control-with-xaml-islands-cpp.md) e [hospeda um artigo de controle UWP personalizado](host-custom-control-with-xaml-islands-cpp.md) fornece instruções e exemplos para hospedagem de ilhas C++ XAML em um aplicativo Win32. No entanto, os exemplos de código nesses artigos não tratam de muitos cenários avançados que os aplicativos de desktop podem precisar manipular para fornecer uma experiência de usuário tranqüila. Este artigo fornece diretrizes para alguns desses cenários e ponteiros para exemplos de código relacionados.

## <a name="keyboard-input"></a>Entrada por teclado

Para lidar corretamente com a entrada de teclado para cada ilha XAML, seu aplicativo deve passar todas as mensagens do Windows para a estrutura de XAML do UWP para que determinadas mensagens possam ser processadas corretamente. Para fazer isso, em algum lugar em seu aplicativo que possa acessar o loop de mensagem, converta o objeto **DesktopWindowXamlSource** para cada ilha XAML em uma interface com **IDesktopWindowXamlSourceNative2** . Em seguida, chame o método **PreTranslateMessage** dessa interface e passe a mensagem atual.

  * Win32:: o aplicativo pode chamar **PreTranslateMessage** diretamente em seu loop de mensagem principal. **C++** Para obter um exemplo, consulte o arquivo [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) .

  * **WPF:** O aplicativo pode chamar **PreTranslateMessage** do manipulador de eventos para o evento [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) . Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) no kit de ferramentas da Comunidade do Windows.

  * **Windows Forms:** O aplicativo pode chamar **PreTranslateMessage** de uma substituição para o método [Control. PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) . Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) no kit de ferramentas da Comunidade do Windows.

## <a name="keyboard-focus-navigation"></a>Navegação de foco de teclado

Quando o usuário navega pelos elementos da interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando **Tab** ou direção/tecla de seta), você precisará mover o foco de forma programática para dentro e para fora do objeto **DesktopWindowXamlSource** . Quando a navegação do teclado do usuário atinge o **DesktopWindowXamlSource**, mova o foco para o primeiro objeto [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) na ordem de navegação da sua interface do usuário, continue a mover o foco para os seguintes objetos **Windows. UI. XAML. UIElement** , pois ele percorre os elementos e, em seguida, move o foco para trás do **DesktopWindowXamlSource** e para o elemento pai da interface do usuário.  

A API de hospedagem XAML do UWP fornece vários tipos e membros para ajudá-lo a realizar essas tarefas.

* Quando a navegação do teclado entra em seu **DesktopWindowXamlSource**, o evento [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) é gerado. Manipule esse evento e mova o foco programaticamente para o primeiro **Windows. UI. XAML. UIElement** hospedado usando o método [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Quando o usuário está no último elemento focado em seu **DesktopWindowXamlSource** e pressiona a tecla **Tab** ou uma tecla de direção, o evento [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) é gerado. Manipule esse evento e mova o foco programaticamente para o próximo elemento focado no aplicativo host. Por exemplo, em um aplicativo WPF em que o **DesktopWindowXamlSource** está hospedado em um [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o método [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir o foco para o próximo elemento focado no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, consulte os seguintes arquivos de código:

  * /Win32: consulte o arquivo [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) .  **C++**

  * **WPF:** Consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) no kit de ferramentas da Comunidade do Windows.  

  * **Windows Forms:** Consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) no kit de ferramentas da Comunidade do Windows.

## <a name="handle-layout-changes"></a>Manipular alterações de layout

Quando o usuário alterar o tamanho do elemento pai da interface do usuário, você precisará lidar com as alterações de layout necessárias para garantir que seus controles UWP sejam exibidos como esperado. Aqui estão alguns cenários importantes a considerar.

* Em um C++ aplicativo Win32, quando seu aplicativo manipula a mensagem de WM_SIZE ele pode reposicionar a ilha XAML hospedada usando a função [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Para obter um exemplo, consulte o arquivo de código [SampleApp. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) .

* Quando o elemento pai da interface do usuário precisa obter o tamanho da área retangular necessária para ajustar o **Windows. UI. XAML. UIElement** que você está hospedando no **DesktopWindowXamlSource**, chame o método [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) do **Windows. UI. XAML. UIElement**. Por exemplo:

    * Em um aplicativo do WPF, você pode fazer isso a partir do método [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) do [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

    * Em um aplicativo Windows Forms você pode fazer isso a partir do método [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) do [controle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

* Quando o tamanho do elemento pai da interface do usuário for alterado, chame o método [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) da raiz **Windows. UI. XAML. UIElement** que você está hospedando no **DesktopWindowXamlSource**. Por exemplo:

    * Em um aplicativo do WPF, você pode fazer isso a partir do método [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) do objeto [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHost.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

    * Em um aplicativo Windows Forms, você pode fazer isso a partir do manipulador para o evento [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) do [controle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHost.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

## <a name="handle-dpi-changes"></a>Tratar alterações de DPI

A estrutura de XAML do UWP lida com alterações de DPI para controles UWP hospedados automaticamente (por exemplo, quando o usuário arrasta a janela entre monitores com DPI de tela diferente). Para obter a melhor experiência, recomendamos que seu aplicativo Windows Forms, WPF C++ ou Win32 esteja configurado para ter reconhecimento de DPI por monitor.

Para configurar seu aplicativo para ter reconhecimento de DPI por monitor, adicione um [manifesto de assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e defina o elemento **\<DpiAwareness\>** como **PerMonitorV2**. Para obter mais informações sobre esse valor, consulte a descrição de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

* [Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Usando a API de hospedagem XAML do UWP C++ em um aplicativo Win32](using-the-xaml-hosting-api.md)
* [Hospedar um controle UWP padrão em um C++ aplicativo Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar um controle UWP personalizado em um C++ aplicativo Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Exemplos de código de ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Exemplo de ilhas XAML do Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
