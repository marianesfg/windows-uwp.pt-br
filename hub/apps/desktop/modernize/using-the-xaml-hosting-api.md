---
description: Este artigo descreve como hospedar a interface do usuário do UWP XAML C++ em seu aplicativo Win32 da área de trabalho.
title: Usar a API de hospedagem XAML UWP em um aplicativo C++ Win32
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, Ilhas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218456"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Usar a API de hospedagem XAML UWP em um aplicativo C++ Win32

A partir do Windows 10, versão 1903, aplicativos de área de trabalho não C++ UWP (incluindo Win32, WPF e aplicativos de Windows Forms) podem usar a *API de hospedagem XAML do UWP* para hospedar controles UWP em qualquer elemento de interface do usuário que esteja associado a um identificador de janela (HWND). Essa API permite que aplicativos de área de trabalho não UWP usem os recursos mais recentes da interface do usuário do Windows 10 que só estão disponíveis por meio de controles UWP. Por exemplo, aplicativos de área de trabalho não UWP podem usar essa API para hospedar controles UWP que usam o [sistema de design fluente](/windows/uwp/design/fluent-design-system/index) e dão suporte ao [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

A API de hospedagem XAML do UWP fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores tragam a interface do usuário fluente para aplicativos de área de trabalho não UWP. Esse recurso é chamado de *ilhas XAML*. Para obter uma visão geral desse recurso, consulte [hospedar controles XAML do UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md).

> [!NOTE]
> Caso tenha comentários sobre as Ilhas XAML, crie um problema no [repositório Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários nele. Se preferir enviar seus comentários de forma particular, envie-os para XamlIslandsFeedback@microsoft.com. Seus insights e seus cenários são extremamente importantes para nós.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>A API de hospedagem do UWP XAML é a escolha certa para seu aplicativo de desktop?

A API de hospedagem do UWP XAML fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos da área de trabalho. Alguns tipos de aplicativos da área de trabalho têm a opção de usar APIs alternativas e mais convenientes para atingir esse objetivo.

* Se você tiver um C++ aplicativo de área de trabalho Win32 e quiser hospedar os controles UWP em seu aplicativo, deverá usar a API de hospedagem XAML do UWP. Não há alternativas para esses tipos de aplicativos.

* Para aplicativos do WPF e Windows Forms, é altamente recomendável que você use os [controles .NET da ilha XAML](xaml-islands.md#wpf-and-windows-forms-applications) no kit de ferramentas da Comunidade do Windows em vez de usar a API de hospedagem XAML do UWP diretamente. Esses controles usam a API de Hospedagem de XAML do UWP internamente e implementam todo o comportamento que, de outra forma, você precisaria lidar se usou a API de hospedagem XAML do UWP diretamente, incluindo a navegação do teclado e as alterações de layout.

Como recomendamos que apenas C++ os aplicativos Win32 usem a API de hospedagem XAML do UWP, este artigo fornece principalmente instruções C++ e exemplos para aplicativos Win32. No entanto, você pode usar a API de hospedagem XAML do UWP no WPF e Windows Forms aplicativos se escolher. Este artigo aponta para o código-fonte relevante para os [controles de host](xaml-islands.md#host-controls) para WPF e Windows Forms no kit de ferramentas da Comunidade do Windows para que você possa ver como a API de hospedagem XAML do UWP é usada por esses controles.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Saiba como usar a API de hospedagem XAML

Para seguir instruções passo a passo com exemplos de código para usar a API de hospedagem XAML em C++ aplicativos Win32, consulte estes artigos:

* [Hospedar um controle UWP padrão](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar um controle UWP personalizado](host-custom-control-with-xaml-islands-cpp.md)
* [Cenários avançados](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Amostras

A maneira como você usa a API de hospedagem XAML do UWP em seu código depende do tipo de aplicativo, do design do seu aplicativo e de outros fatores. Para ajudar a ilustrar como usar essa API no contexto de um aplicativo completo, este artigo refere-se ao código dos exemplos a seguir.

### <a name="c-win32"></a>C++Win32

Os exemplos a seguir demonstram como usar a API de hospedagem XAML do C++ UWP em um aplicativo Win32:

* [Exemplo de ilha XAML simples](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Este exemplo demonstra uma implementação básica de Hospedagem de um controle UWP em um aplicativo C++ Win32 não empacotado (ou seja, um aplicativo que não é compilado em um pacote MSIX).

* [Ilha XAML com exemplo de controle personalizado](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Este exemplo demonstra uma implementação completa da Hospedagem de um controle UWP personalizado em um aplicativo C++ Win32 empacotado, bem como o tratamento de outro comportamento, como entrada de teclado e navegação de foco.

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows serve como um exemplo de referência para usar a API de hospedagem UWP no WPF e Windows Forms aplicativos. O código-fonte está disponível nos seguintes locais:

* Para a versão do WPF do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão do WPF deriva de [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Para a versão Windows Forms do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão Windows Forms deriva de [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

> [!NOTE]
> É altamente recomendável que você use os [controles .NET da ilha XAML](xaml-islands.md#wpf-and-windows-forms-applications) no kit de ferramentas da Comunidade do Windows em vez de usar a API de hospedagem XAML do UWP diretamente no WPF e Windows Forms aplicativos. Os links de exemplo do WPF e Windows Forms neste artigo são apenas para fins ilustrativos.

## <a name="architecture-of-the-api"></a>Arquitetura da API

A API de hospedagem XAML do UWP inclui esses tipos de Windows Runtime principais e interfaces COM.

|  Tipo ou interface | Descrição |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Essa classe representa a estrutura de XAML do UWP. Essa classe fornece um único método [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático que inicializa a estrutura de XAML do UWP no thread atual no aplicativo da área de trabalho. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Essa classe representa uma instância do conteúdo do UWP XAML que você está hospedando em seu aplicativo de área de trabalho. O membro mais importante dessa classe é a propriedade [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Atribua essa propriedade a um [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que você deseja hospedar. Essa classe também tem outros membros para direcionar a navegação de foco de teclado para dentro e fora das Ilhas XAML. |
| IDesktopWindowXamlSourceNative | Essa interface COM fornece o método **AttachToWindow** , que você usa para anexar uma ilha XAML em seu aplicativo a um elemento pai da interface do usuário. Cada objeto **DesktopWindowXamlSource** implementa essa interface. Essa interface é declarada em Windows. UI. XAML. Hosting. desktopwindowxamlsource. h. |
| IDesktopWindowXamlSourceNative2 | Essa interface COM fornece o método **PreTranslateMessage** , que permite que a estrutura de XAML do UWP processe determinadas mensagens do Windows corretamente. Cada objeto **DesktopWindowXamlSource** implementa essa interface. Essa interface é declarada em Windows. UI. XAML. Hosting. desktopwindowxamlsource. h. |

O diagrama a seguir ilustra a hierarquia de objetos em uma ilha XAML hospedada em um aplicativo de área de trabalho.

* No nível base está o elemento de interface do usuário em seu aplicativo em que você deseja hospedar a ilha XAML. Este elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário nos quais você pode hospedar uma ilha [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) XAML incluem C++ uma janela para aplicativos Win32, um [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos do WPF e um [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para Windows Forms aplicativos.

* No próximo nível está um objeto **DesktopWindowXamlSource** . Esse objeto fornece a infraestrutura para hospedar a ilha XAML. Seu código é responsável por criar esse objeto e anexá-lo ao elemento pai da interface do usuário.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto cria automaticamente uma janela filho nativa para hospedar seu controle UWP. Essa janela filho nativa é basicamente abstraida para fora do seu código, mas você pode acessar seu identificador (HWND), se necessário.

* Por fim, no nível superior está o controle UWP que você deseja hospedar em seu aplicativo de desktop. Pode ser qualquer objeto UWP derivado de [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles de usuário personalizados.

![Arquitetura do DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Ao hospedar as Ilhas XAML em um aplicativo da área de trabalho, você pode ter várias árvores de conteúdo XAML em execução no mesmo thread simultaneamente. Para acessar o elemento raiz de uma árvore de conteúdo XAML em uma Ilha XAML e obter informações relacionadas sobre o contexto no qual ele está hospedado, use a classe [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). As APIs [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)e [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) não fornecerão as informações corretas para as ilhas XAML. Para obter mais informações, consulte [esta seção](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro ao usar a API de hospedagem XAML do UWP em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "não é possível ativar DesktopWindowXamlSource. Este tipo não pode ser usado em um aplicativo UWP. " ou "não é possível ativar WindowsXamlManager. Este tipo não pode ser usado em um aplicativo UWP. " | Esse erro indica que você está tentando usar a API de hospedagem XAML do UWP (especificamente, está tentando instanciar os tipos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) em um aplicativo UWP. A API de hospedagem XAML do UWP destina-se apenas a ser usada em aplicativos de área de trabalho não UWP, como WPF C++ , Windows Forms e aplicativos Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Erro ao tentar usar os tipos WindowsXamlManager ou DesktopWindowXamlSource

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma exceção com a seguinte mensagem: "WindowsXamlManager e DesktopWindowXamlSource têm suporte para aplicativos destinados à versão 10.0.18226.0 do Windows e posterior. Verifique o manifesto do aplicativo ou o manifesto do pacote e certifique-se de que a propriedade MaxTestedVersion seja atualizada. " | Esse erro indica que seu aplicativo tentou usar os tipos **WindowsXamlManager** ou **DesktopWindowXamlSource** na API de hospedagem XAML do UWP, mas o sistema operacional não pode determinar se o aplicativo foi criado para o Windows 10 de destino, versão 1903 ou posterior. A API de hospedagem XAML do UWP foi introduzida pela primeira vez como uma visualização em uma versão anterior do Windows 10, mas só tem suporte a partir do Windows 10, versão 1903.</p></p>Para resolver esse problema, crie um pacote MSIX para o aplicativo e execute-o do pacote ou instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) em seu projeto.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro ao anexar a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "o método AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou a ele o HWND de uma janela que foi criada em um thread diferente. Você deve passar esse método a HWND de uma janela que foi criada no mesmo thread que o código do qual você está chamando o método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro ao anexar a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "o método AttachToWindow falhou porque o HWND especificado descende de uma janela de nível superior diferente da HWND que foi passada anteriormente para AttachToWindow no mesmo thread". | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou a ele o HWND de uma janela que descende de uma janela de nível superior diferente de uma janela que você especificou em uma chamada anterior para esse método no mesmo thread.</p></p>Depois que o aplicativo chama **AttachToWindow** em um thread específico, todos os outros objetos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) no mesmo thread só podem se anexar a janelas descendentes da mesma janela de nível superior que foi passada na primeira chamada para **AttachToWindow**. Quando todos os objetos **DesktopWindowXamlSource** são fechados para um thread específico, a próxima **DesktopWindowXamlSource** é gratuita para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os objetos **DesktopWindowXamlSource** que estão vinculados a outras janelas de nível superior nesse thread ou crie um novo thread para esse **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Hospedar um controle UWP padrão em um C++ aplicativo Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar um controle UWP personalizado em um C++ aplicativo Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Cenários avançados para ilhas XAML em C++ aplicativos Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemplos de código de ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Exemplo de ilhas XAML do Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
