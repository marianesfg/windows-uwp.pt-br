---
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Controles UWP em aplicativos da área de trabalho
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 52287576dbc395af60e15b5f4b4a403db7e92900
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72313445"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)

A partir do Windows 10, versão 1903, você pode hospedar controles UWP em aplicativos de área de trabalho não UWP usando um recurso chamado *ilhas XAML*. Esse recurso permite aprimorar a aparência e a funcionalidade de seus aplicativos WPF, Windows Forms e C++ Win32 existentes com os recursos mais recentes da interface do usuário do Windows 10 que só estão disponíveis por meio de controles UWP. Isso significa que você pode usar recursos de UWP, como o [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) e controles que dão suporte ao [sistema de design fluente](/windows/uwp/design/fluent-design-system/index) em seus aplicativos WPF C++ , Windows Forms e Win32 existentes.

Você pode hospedar qualquer controle UWP derivado de [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo:

* Qualquer controle UWP de primeira parte fornecido pela biblioteca SDK do Windows ou WinUI.
* Qualquer controle UWP personalizado (por exemplo, um controle de usuário que consiste em vários controles UWP que funcionam juntos). Você deve ter o código-fonte do controle personalizado para que possa compilá-lo com seu aplicativo.

Fundamentalmente, as ilhas XAML são criadas usando a *API de hospedagem XAML do UWP*. Essa API consiste em várias classes Windows Runtime e interfaces COM que foram introduzidas no SDK do Windows 10, versão 1903. Também fornecemos um conjunto de controles .NET da ilha XAML no [Kit de ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) que usam a API de hospedagem XAML do UWP internamente e proporcionam uma experiência de desenvolvimento mais conveniente para aplicativos do WPF e Windows Forms.

A maneira como você usa as ilhas XAML depende do tipo de aplicativo e dos tipos de controles UWP que você deseja hospedar.

> [!NOTE]
> Se você tiver comentários sobre as ilhas XAML, crie um novo problema no [repositório Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários lá. Se preferir enviar seus comentários de forma privada, você poderá enviá-los para XamlIslandsFeedback@microsoft.com. Suas ideias e cenários são extremamente importantes para nós.

## <a name="wpf-and-windows-forms-applications"></a>Aplicativos do WPF e Windows Forms

Recomendamos que os aplicativos WPF e Windows Forms usem os controles .NET da ilha XAML que estão disponíveis no kit de ferramentas da Comunidade do Windows. Esses controles fornecem um modelo de objeto que imita (ou fornece acesso a) Propriedades, métodos e eventos dos controles UWP correspondentes. Eles também tratam do comportamento, como a navegação do teclado e as alterações de layout.

Há dois conjuntos de controles de ilha XAML para aplicativos WPF e Windows Forms: *controles encapsulados* e *controles de host*. A partir do Windows 10, versão 1903, esses controles estão [disponíveis como uma visualização do desenvolvedor](#feature-roadmap).

### <a name="wrapped-controls"></a>Controles encapsulados

Os aplicativos WPF e Windows Forms podem usar uma seleção de controles de ilha XAML que encapsulam a interface e a funcionalidade de um controle UWP específico. Você pode adicionar esses controles diretamente à superfície de design do seu projeto do WPF ou Windows Forms e, em seguida, usá-los como qualquer outro controle WPF ou Windows Forms no designer.

Os seguintes controles UWP encapsulados estão disponíveis atualmente no kit de ferramentas da Comunidade do Windows. 

| Controle | Sistema operacional mínimo com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versão 1903 | Forneça uma superfície e barras de ferramentas relacionadas para interação do usuário baseada em Windows Ink em seu Windows Forms ou aplicativo de área de trabalho do WPF. |
| [MediaPlayerelement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versão 1903 | Insere uma exibição que transmite e renderiza conteúdo de mídia, como vídeo em seu Windows Forms ou aplicativo de área de trabalho do WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versão 1903 | Permite exibir um mapa simbólico ou de foto real em seu Windows Forms ou aplicativo de área de trabalho do WPF. |

Para obter instruções que demonstram como usar os controles UWP encapsulados, consulte [hospedar um controle UWP padrão em um aplicativo do WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Controles de host

Para cenários além daqueles cobertos pelos controles encapsulados disponíveis, os aplicativos WPF e Windows Forms também podem usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) que está disponível no kit de ferramentas da Comunidade do Windows.

| Controle | Sistema operacional mínimo com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, versão 1903 | Pode hospedar qualquer controle UWP derivado de [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP de primeira empresa fornecido pelo SDK do Windows, bem como controles personalizados. |

Para obter orientações que demonstram como usar o controle **WindowsXamlHost** , consulte [hospedar um controle UWP padrão em um aplicativo do WPF](host-standard-control-with-xaml-islands.md) e [hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> O uso do controle **WindowsXamlHost** para hospedar controles UWP personalizados tem suporte apenas no WPF e Windows Forms aplicativos direcionados ao .NET Core 3. A hospedagem de controles de UWP de primeira parte fornecidos pelo SDK do Windows tem suporte em aplicativos direcionados ao .NET Framework ou ao .NET Core 3.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configurar seu projeto para usar os controles .NET da ilha XAML

Os controles .NET da ilha XAML exigem o Windows 10, versão 1903 ou uma versão posterior. Para usar esses controles, instale um dos pacotes NuGet listados abaixo. Esses pacotes fornecem tudo o que você precisa para usar os controles empacotados da ilha XAML e os controles de host, e eles incluem outros pacotes NuGet relacionados que também são necessários.

| Tipo de controle | Pacote NuGet  | Artigos relacionados |
|-----------------|----------------|---------------------|
| [Controles encapsulados](#wrapped-controls) | Versão 6.0.0-preview7 ou posterior destes pacotes: <ul><li>WPF: [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hospedar um controle UWP padrão em um aplicativo do WPF](host-standard-control-with-xaml-islands.md)  |
| [Controle de host](#host-controls) | Versão 6.0.0-preview7 ou posterior destes pacotes: <ul><li>WPF: [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hospedar um controle UWP padrão em um aplicativo do WPF](host-standard-control-with-xaml-islands.md)<br/>[Hospedar um controle UWP personalizado em um aplicativo do WPF](host-custom-control-with-xaml-islands.md)  |

Lembre-se dos seguintes detalhes:

* Os pacotes de controle de host também estão incluídos nos pacotes de controle encapsulado. Você pode instalar os pacotes de controle encapsulado se quiser usar os dois conjuntos de controles.

* Se você estiver hospedando um controle UWP personalizado, seu projeto do WPF ou Windows Forms deverá ter como destino o .NET Core 3. Não há suporte para a hospedagem de controles UWP personalizados em aplicativos direcionados ao .NET Framework. Você também precisará executar algumas etapas adicionais para fazer referência ao controle personalizado. Para obter mais informações, consulte [hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML](host-custom-control-with-xaml-islands.md).

* As versões anteriores dessas instruções tinham que adicionar o elemento `maxversiontested` a um manifesto do aplicativo em seu projeto do WPF ou Windows Forms. Contanto que você esteja usando as versões de visualização mais recentes dos pacotes NuGet listados acima, você não precisa mais adicionar esse elemento ao seu manifesto.

### <a name="architecture-of-xaml-island-net-controls"></a>Arquitetura dos controles .NET da ilha XAML

Aqui está uma visão rápida de como os diferentes tipos de controles da ilha XAML são organizados de forma arquitetônica sobre a API de hospedagem XAML do UWP.

![Arquitetura de controle de host](images/xaml-islands/host-controls.png)

As APIs que aparecem na parte inferior do diagrama com o SDK do Windows. Os controles encapsulados e os controles de host estão disponíveis por meio de pacotes NuGet no kit de ferramentas da Comunidade do Windows.

### <a name="web-view-controls"></a>Controles de exibição da Web

O Windows Community Toolkit também fornece os seguintes controles .NET para hospedar conteúdo da Web em aplicativos WPF e Windows Forms. Esses controles são frequentemente usados em cenários de modernização de aplicativo de desktop semelhantes, como os controles da ilha XAML, e são mantidos no mesmo repositório de [repositório Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) que os controles da ilha XAML.

| Controle | Sistema operacional mínimo com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versão 1803 | Usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fornece uma versão do **WebView** que é compatível com mais versões do sistema operacional. Esse controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da Web no Windows 10 versão 1803 e posterior e o mecanismo de renderização do Internet Explorer para mostrar o conteúdo da Web em versões anteriores do Windows 10, Windows 8. x e Windows 7. |

Para usar esses controles, instale um destes pacotes NuGet:

* WPF: [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Aplicativos Win32

Os controles .NET da ilha XAML não têm suporte C++ em aplicativos Win32. Em vez disso, esses aplicativos devem usar a *API de hospedagem XAML do UWP* fornecida pelo SDK do Windows 10 (versão 1903 e posterior).

A API de hospedagem XAML do UWP consiste em várias classes Windows Runtime e interfaces COM C++ que seu aplicativo Win32 pode usar para hospedar qualquer controle UWP derivado de [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Você pode hospedar controles UWP em qualquer elemento de interface do usuário em seu aplicativo que tenha um identificador de janela associado (HWND). Para obter mais informações sobre essa API, incluindo pré-requisitos, consulte [usando a API de hospedagem do UWP XAML C++ em um aplicativo Win32](using-the-xaml-hosting-api.md).

> [!NOTE]
> Os controles encapsulados e os controles de host no kit de ferramentas da Comunidade do Windows usam a API de hospedagem XAML do UWP internamente e implementam todo o comportamento que, de outra forma, você precisaria lidar se usava a API de hospedagem XAML do UWP diretamente, incluindo a navegação por teclado e alterações de layout. Para aplicativos do WPF e Windows Forms, é altamente recomendável que você use esses controles em vez da API de hospedagem XAML do UWP diretamente porque eles abstraim muitos dos detalhes de implementação do uso da API.

## <a name="feature-roadmap"></a>Roteiro de recursos

A partir do lançamento do Windows 10, versão 1903, os controles .NET da ilha XAML no kit de ferramentas da Comunidade do Windows ainda estão na visualização do desenvolvedor até que a versão 1,0 dos controles esteja disponível.

* A versão 1,0 dos controles para o .NET Framework 4.6.2 e posterior está planejada para ser lançada na [versão 6,0 do kit de ferramentas](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* A versão 1,0 dos controles para o .NET Core 3 está planejada para uma versão posterior do kit de ferramentas.
* Se você quiser experimentar as versões mais recentes dos lançamentos da versão 1,0 desses controles para o .NET Framework e o .NET Core 3, consulte os pacotes NuGet da versão 6.0.0-preview7 (ou posterior) na Galeria [da ferramenta UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) .

Para obter mais detalhes, consulte [esta postagem de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Recursos adicionais

Para obter informações mais detalhadas e tutoriais sobre como usar as ilhas XAML, consulte os seguintes artigos e recursos:

* [Modernizar um tutorial de aplicativo do WPF](modernize-wpf-tutorial.md): Este tutorial fornece instruções passo a passo para usar os controles encapsulados e controles de host no kit de ferramentas da Comunidade do Windows para adicionar controles UWP a um aplicativo de linha de negócios existente do WPF. Este tutorial inclui o código completo para o aplicativo do WPF, bem como instruções detalhadas para cada etapa no processo.
* [Ilhas XAML v1-atualizações e roteiro](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): Esta postagem de blog aborda muitas perguntas comuns sobre as ilhas XAML e fornece um roteiro de desenvolvimento detalhado.
