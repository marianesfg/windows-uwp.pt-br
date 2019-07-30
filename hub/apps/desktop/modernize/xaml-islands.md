---
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Controles UWP em aplicativos da área de trabalho
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 560d339476ef3cd45f30bfc678661fb0a4a11ee1
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601543"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)

A partir do Windows 10, versão 1903, você pode hospedar controles UWP em aplicativos de área de trabalho não UWP usando um recurso chamado *ilhas XAML*. Esse recurso permite aprimorar a aparência e a funcionalidade de seus aplicativos de área de trabalho existentes com os recursos mais recentes da interface do usuário do Windows 10 que estão disponíveis apenas por meio de controles UWP. Isso significa que você pode usar recursos de UWP, como o [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) e controles que dão suporte ao [sistema de design fluente](/windows/uwp/design/fluent-design-system/index) em seus aplicativos WPF C++ , Windows Forms e Win32 existentes.

Fornecemos várias maneiras de usar as ilhas XAML em seus aplicativos WPF, Windows Forms C++ e Win32, dependendo da tecnologia ou da estrutura que você está usando. 

> [!NOTE]
> Se você tiver comentários sobre as ilhas XAML, crie um novo problema no [repositório Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários lá. Se preferir enviar seus comentários de forma privada, você poderá enviá-los para XamlIslandsFeedback@microsoft.como. Suas ideias e cenários são extremamente importantes para nós.

## <a name="how-do-xaml-islands-work"></a>Como funcionam as ilhas XAML?

A partir do Windows 10, versão 1903, fornecemos duas maneiras de usar as ilhas XAML em seus aplicativos WPF, C++ Windows Forms e Win32:

* O SDK do Windows fornece várias classes de Windows Runtime e interfaces COM que seu aplicativo pode usar para hospedar qualquer controle UWP derivado de [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Coletivamente, essas classes e interfaces são chamadas de *API de hospedagem XAML do UWP*e permitem hospedar controles UWP em qualquer elemento de interface do usuário em seu aplicativo que tenha um identificador de janela associado (HWND). Para obter mais informações sobre essa API, consulte [usando a API de hospedagem XAML](using-the-xaml-hosting-api.md).

* O [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) também fornece controles adicionais da ilha XAML para WPF e Windows Forms. Esses controles usam a API de Hospedagem de XAML do UWP internamente e implementam todo o comportamento que, de outra forma, você precisaria lidar se usou a API de hospedagem XAML do UWP diretamente, incluindo a navegação do teclado e as alterações de layout. Para aplicativos do WPF e Windows Forms, é altamente recomendável que você use esses controles em vez da API de hospedagem XAML do UWP diretamente porque eles abstraim muitos dos detalhes de implementação do uso da API. Observe que, a partir do Windows 10, versão 1903, esses controles estão [disponíveis como uma visualização do desenvolvedor](#feature-roadmap).

> [!NOTE]
> C++Os aplicativos da área de trabalho do Win32 devem usar a API de hospedagem XAML do UWP para hospedar os controles UWP. Os controles da ilha XAML no kit de ferramentas da Comunidade do Windows C++ não estão disponíveis para aplicativos da área de trabalho Win32.

Há dois tipos de controles de ilha XAML fornecidos pelo kit de ferramentas da Comunidade do Windows para aplicativos do WPF e do Windows Forms: *controles encapsulados* e *controles de host*. 

### <a name="wrapped-controls"></a>Controles encapsulados

Os aplicativos WPF e Windows Forms podem usar uma seleção de controles UWP encapsulados no [Kit de ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Eles são chamados de *controles encapsulados* porque encapsulam a interface e a funcionalidade de um controle UWP específico. Você pode adicionar esses controles diretamente à superfície de design do seu projeto do WPF ou Windows Forms e, em seguida, usá-los como qualquer outro controle WPF ou Windows Forms no seu designer.

Os seguintes controles UWP encapsulados para a implementação de ilhas XAML estão disponíveis atualmente para o WPF (consulte o pacote [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) ) e Windows Forms aplicativos (consulte o pacote [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) ).

| Controle | Sistema operacional mínimo com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versão 1903 | Forneça uma superfície e barras de ferramentas relacionadas para interação do usuário baseada em Windows Ink em seu Windows Forms ou aplicativo de área de trabalho do WPF. |
| [MediaPlayerelement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versão 1903 | Insere uma exibição que transmite e renderiza conteúdo de mídia, como vídeo em seu Windows Forms ou aplicativo de área de trabalho do WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versão 1903 | Permite exibir um mapa simbólico ou de foto real em seu Windows Forms ou aplicativo de área de trabalho do WPF. |

Além dos controles encapsulados para as ilhas XAML, o Windows Community Toolkit também fornece os seguintes controles para hospedar conteúdo da Web no WPF (consulte o pacote [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView) ) e Windows Forms aplicativos (consulte o pacote [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView) ).

| Controle | Sistema operacional mínimo com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versão 1803 | Usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fornece uma versão do **WebView** que é compatível com mais versões do sistema operacional. Esse controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da Web no Windows 10 versão 1803 e posterior e o mecanismo de renderização do Internet Explorer para mostrar o conteúdo da Web em versões anteriores do Windows 10, Windows 8. x e Windows 7. |

### <a name="host-controls"></a>Controles de host

Para cenários além daqueles cobertos pelos controles encapsulados disponíveis, os aplicativos WPF e Windows Forms também podem usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no [Kit de ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Esse controle pode hospedar qualquer controle UWP derivado de [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles de usuário personalizados. Esse controle dá suporte ao SDK do Windows 10 Insider Preview versão 17709 e versões posteriores.

Esses controles estão disponíveis nos pacotes [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (para WPF) e [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (para Windows Forms). Esses pacotes são incluídos nos pacotes [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) e [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) que contêm os controles encapsulados.

### <a name="architecture-overview"></a>Visão geral da arquitetura

Veja como esses controles são organizados na arquitetura.

![Arquitetura de controle de host](images/xaml-islands/host-controls.png)

As APIs que aparecem na parte inferior do diagrama com o SDK do Windows. Os controles encapsulados e os controles de host estão disponíveis por meio de pacotes NuGet no kit de ferramentas da Comunidade do Windows.

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>Configurar seu projeto para usar ilhas XAML

As ilhas XAML exigem o Windows 10, versão 1903 e posterior. Para usar as ilhas XAML em seu aplicativo, você deve primeiro configurar seu projeto:

1. Modifique seu projeto para usar Windows Runtime APIs. Para obter instruções, consulte [Este artigo](desktop-to-uwp-enhance.md#set-up-your-project).
2. Instale um desses pacotes NuGet em seu projeto. Certifique-se de instalar a versão 6.0.0-Preview 6.4 ou uma versão posterior do pacote.
    * WPF: Instalar [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)
    * Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)
    * C++Win32 [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)

> [!NOTE]
> As versões anteriores dessas instruções tinham que adicionar o elemento **maxversiontested** a um manifesto do aplicativo em seu projeto. A partir das versões de visualização mais recentes dos pacotes NuGet, você não precisa mais adicionar esse elemento ao seu manifesto.

## <a name="feature-roadmap"></a>Roteiro de recursos

A partir do lançamento do Windows 10, versão 1903, os controles encapsulados e os controles de host no kit de ferramentas da Comunidade do Windows ainda estão na visualização do desenvolvedor até que a versão 1,0 dos controles esteja disponível.

* A versão 1,0 dos controles para o .NET Framework 4.6.2 e posterior está planejada para ser lançada na [versão 6,0 do kit de ferramentas](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* A versão 1,0 dos controles para o .NET Core 3 está planejada para uma versão posterior do kit de ferramentas.
* Se você quiser experimentar as versões mais recentes dos lançamentos da versão 1,0 desses controles para o .NET Framework e o .NET Core 3, consulte os pacotes NuGet **6.0.0-Preview 6.4** na Galeria [da ferramenta UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) .

Para obter mais detalhes, consulte [esta postagem de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Recursos adicionais

Para obter informações mais detalhadas e tutoriais sobre como usar as ilhas XAML, consulte os seguintes artigos e recursos:

* [Modernizar um tutorial de aplicativo do WPF](modernize-wpf-tutorial.md): Este tutorial fornece instruções passo a passo para usar os controles encapsulados e controles de host no kit de ferramentas da Comunidade do Windows para adicionar controles UWP a um aplicativo de linha de negócios existente do WPF. Este tutorial inclui o código completo para o aplicativo do WPF, bem como instruções detalhadas para cada etapa no processo.
* [Ilhas XAML v1-atualizações e roteiro](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): Esta postagem de blog aborda muitas perguntas comuns sobre as ilhas XAML e fornece um roteiro de desenvolvimento detalhado.
