---
description: Este artigo descreve como hospedar interface do usuário XAML UWP no aplicativo C++ Win32 da área de trabalho.
title: Usar a API de hospedagem XAML UWP em um aplicativo C++ Win32
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, ilhas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80218456"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Usar a API de hospedagem XAML UWP em um aplicativo C++ Win32

A partir do Windows 10, versão 1903, aplicativos de área de trabalho não UWP (incluindo os aplicativos Windows Forms, C++ Win32 e WPF) podem usar a *API de hospedagem XAML UWP* para hospedar controles UWP em qualquer elemento de interface do usuário que esteja associado a um identificador de janela (HWND). Essa API permite que aplicativos de área de trabalho não UWP usem os recursos mais recentes da interface do usuário do Windows 10 que só estão disponíveis por meio de controles UWP. Por exemplo, aplicativos de área de trabalho não UWP podem usar essa API para hospedar controles UWP que usam o [Sistema Fluent Design](/windows/uwp/design/fluent-design-system/index) e dão suporte ao [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

A API de hospedagem XAML UWP fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores tragam a interface do usuário do Fluent para aplicativos da área de trabalho não UWP. Esse recurso é chamado de *Ilhas XAML*. Para obter uma visão geral desse recurso, confira [Hospedar controles XAML UWP em aplicativos da área de trabalho (Ilhas XAML)](xaml-islands.md).

> [!NOTE]
> Caso tenha comentários sobre as Ilhas XAML, crie um problema no [repositório Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários nele. Se preferir enviar seus comentários de forma particular, envie-os para XamlIslandsFeedback@microsoft.com. Seus insights e seus cenários são extremamente importantes para nós.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>A API de hospedagem XAML UWP é a escolha certa para seu aplicativo da área de trabalho?

A API de hospedagem XAML UWP fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos da área de trabalho. Alguns tipos de aplicativos da área de trabalho têm a opção de usar APIs alternativas e mais convenientes para atingir esse objetivo.

* Se você tiver um aplicativo de área de trabalho C++ Win32 e quiser hospedar os controles UWP em seu aplicativo, deverá usar a API de hospedagem XAML UWP. Não há alternativas para esses tipos de aplicativos.

* Para aplicativos WPF e Windows Forms, é altamente recomendável que você use os [controles .NET da Ilha XAML](xaml-islands.md#wpf-and-windows-forms-applications) no Kit de ferramentas da Comunidade do Windows em vez de usar a API de hospedagem XAML UWP diretamente. Esses controles usam a API de hospedagem XAML UWP internamente e implementam todo o comportamento do qual, de outra forma, você precisaria cuidar por conta própria se usasse a API de hospedagem XAML UWP diretamente, incluindo a navegação por teclado e as alterações de layout.

Como recomendamos que apenas os aplicativos C++ Win32 usem a API de hospedagem XAML UWP, este artigo fornece principalmente instruções e exemplos para aplicativos C++ Win32. No entanto, você poderá usar a API de hospedagem XAML UWP em aplicativos WPF e Windows Forms, se preferir. Este artigo aponta para o código-fonte relevante dos [controles de host](xaml-islands.md#host-controls) para WPF e Windows Forms no Kit de ferramentas da Comunidade do Windows para que você possa ver como a API de hospedagem XAML UWP é usada por esses controles.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Como usar a API de hospedagem XAML

Para seguir instruções passo a passo com exemplos de código para usar a API de hospedagem XAML em aplicativos C++ Win32, confira estes artigos:

* [Hospedar um controle UWP padrão](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar um controle UWP personalizado](host-custom-control-with-xaml-islands-cpp.md)
* [Cenários avançados](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Amostras

A maneira como você usa a API de hospedagem XAML UWP em seu código depende do tipo de aplicativo, do design do seu aplicativo e de outros fatores. Para ajudar a ilustrar como usar essa API no contexto de um aplicativo completo, este artigo refere-se ao código dos exemplos a seguir.

### <a name="c-win32"></a>C++ Win32

Os seguintes exemplos demonstram como usar a API de hospedagem XAML UWP em um aplicativo C++ Win32:

* [Exemplo de Ilha XAML](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Este exemplo demonstra uma implementação básica de hospedagem de um controle UWP em um aplicativo C++ Win32 não empacotado (ou seja, um aplicativo que não é integrado a um pacote MSIX).

* [Ilha XAML com exemplo de controle personalizado](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Este exemplo demonstra uma implementação completa da hospedagem de um controle UWP personalizado em um aplicativo C++ Win32 empacotado, bem como o tratamento de outro comportamento, como entrada de teclado e navegação de foco.

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no Kit de ferramentas da Comunidade do Windows serve como um exemplo de referência para usar a API de hospedagem UWP nos aplicativos Windows Forms e WPF. O código-fonte está disponível nos seguintes locais:

* Para a versão WPF do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão WPF é derivada de [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Para a versão Windows Forms do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão Windows Forms é derivada de [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

> [!NOTE]
> Recomendamos que você use os [controles .NET da Ilha XAML](xaml-islands.md#wpf-and-windows-forms-applications) no Kit de ferramentas da Comunidade do Windows em vez de usar a API de hospedagem XAML UWP diretamente nos aplicativos Windows Forms e WPF. Os links de exemplo do Windows Forms e WPF neste artigo são apenas para fins ilustrativos.

## <a name="architecture-of-the-api"></a>Arquitetura da API

A API de hospedagem XAML UWP inclui os tipos principais de Windows Runtime e interfaces COM.

|  Tipo ou interface | Descrição |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Essa classe representa a estrutura de XAML UWP. Essa classe fornece um único método estático [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) que inicializa a estrutura XAML UWP no thread atual no aplicativo da área de trabalho. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Essa classe representa uma instância do conteúdo do XAML UWP que você está hospedando em seu aplicativo da área de trabalho. O membro mais importante dessa classe é a propriedade de [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content). Atribua essa propriedade a um [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que você queira hospedar. Essa classe também tem outros membros para rotear a navegação de foco de teclado para dentro e fora das Ilhas XAML. |
| IDesktopWindowXamlSourceNative | Essa interface COM fornece o método **AttachToWindow**, que você usa para anexar uma Ilha XAML de seu aplicativo a um elemento pai da interface do usuário. Cada objeto **DesktopWindowXamlSource** implementa essa interface. Essa interface é declarada em windows.ui.xaml.hosting.desktopwindowxamlsource.h. |
| IDesktopWindowXamlSourceNative2 | Essa interface COM fornece o método **PreTranslateMessage**, que permite à estrutura XAML UWP processar determinadas mensagens do Windows corretamente. Cada objeto **DesktopWindowXamlSource** implementa essa interface. Essa interface é declarada em windows.ui.xaml.hosting.desktopwindowxamlsource.h. |

O diagrama a seguir ilustra a hierarquia de objetos em uma Ilha XAML hospedada em um aplicativo da área de trabalho.

* No nível base está o elemento de interface do usuário em seu aplicativo em que você deseja hospedar a Ilha XAML. Esse elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário nos quais você pode hospedar uma Ilha XAML incluem uma [janela](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para aplicativos C++ Win32, um [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos WPF e um [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicativos Windows Forms.

* No próximo nível está um objeto **DesktopWindowXamlSource**. Esse objeto fornece a infraestrutura para hospedar a Ilha XAML. Seu código é responsável por criar esse objeto e anexá-lo ao elemento pai da interface do usuário.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto cria automaticamente uma janela filho nativa para hospedar seu controle UWP. Essa janela filho nativa é basicamente abstraída de seu código, mas você pode acessar o identificador (HWND), caso seja necessário.

* Por fim, no nível superior está o controle UWP que você deseja hospedar em seu aplicativo da área de trabalho. Pode ser qualquer objeto UWP derivado de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles personalizados do usuário.

![Arquitetura DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Ao hospedar as Ilhas XAML em um aplicativo da área de trabalho, você pode ter várias árvores de conteúdo XAML em execução no mesmo thread simultaneamente. Para acessar o elemento raiz de uma árvore de conteúdo XAML em uma Ilha XAML e obter informações relacionadas sobre o contexto no qual ele está hospedado, use a classe [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). As APIs [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) e [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) não fornecerão as informações corretas para as Ilhas XAML. Para obter mais informações, consulte [esta seção](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro ao usar a API de hospedagem XAML UWP em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "Não é possível ativar DesktopWindowXamlSource. Esse tipo não pode ser usado em um aplicativo UWP." ou "Não é possível ativar WindowsXamlManager. Esse tipo não pode ser usado em um aplicativo UWP." | Esse erro indica que você está tentando usar a API de hospedagem XAML UWP (especificamente, está tentando criar uma instância dos tipos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)) em um aplicativo UWP. A API de hospedagem XAML UWP destina-se apenas a ser usada em aplicativos não UWP da área de trabalho, como aplicativos WPF, Windows Forms e C++ Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Erro ao tentar usar os tipos WindowsXamlManager ou DesktopWindowXamlSource

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma exceção com a seguinte mensagem: "WindowsXamlManager e DesktopWindowXamlSource têm suporte para aplicativos destinados à versão 10.0.18226.0 do Windows e posterior. Verifique o manifesto do aplicativo ou o manifesto do pacote e certifique-se de que a propriedade MaxTestedVersion esteja atualizada." | Esse erro indica que seu aplicativo tentou usar os tipos **WindowsXamlManager** ou **DesktopWindowXamlSource** na API de hospedagem XAML UWP, mas o sistema operacional não pode determinar se o aplicativo foi criado para o Windows 10, versão 1903 ou posterior. A API de hospedagem XAML UWP foi introduzida pela primeira vez como versão prévia em uma versão anterior do Windows 10, mas só tem suporte a partir do Windows 10, versão 1903.</p></p>Para resolver esse problema, crie um pacote MSIX para o aplicativo e execute-o do pacote ou instale o pacote NuGet [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) em seu projeto.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro ao anexar a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "O método AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative::AttachToWindow** e passou o HWND de uma janela que foi criada em um thread diferente. Você deve passar esse método a HWND de uma janela que foi criada no mesmo thread que o código do qual você está chamando o método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro ao anexar a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "O método AttachToWindow falhou porque o HWND especificado descende de uma janela de nível superior diferente do HWND que foi passado anteriormente para AttachToWindow no mesmo thread." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou o HWND de uma janela que descende de uma janela de nível superior diferente daquela que você especificou em uma chamada anterior para esse método no mesmo thread.</p></p>Depois que o aplicativo chama **AttachToWindow** em um thread específico, todos os outros objetos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) no mesmo thread só podem ser anexados a janelas descendentes da mesma janela de nível superior passada na primeira chamada para **AttachToWindow**. Quando todos os objetos **DesktopWindowXamlSource** estão fechados para um thread específico, o próximo **DesktopWindowXamlSource** é liberado para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os objetos **DesktopWindowXamlSource** associados a outras janelas de nível superior nesse thread ou crie um novo thread para esse **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles XAML do UWP em aplicativos da área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Hospedar um controle UWP padrão em um aplicativo C++ Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar um controle UWP personalizado em um aplicativo C++ Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Cenários avançados de Ilhas XAML em aplicativos C++ Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemplos de código de Ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [Exemplo de Ilhas XAML do C++ Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
