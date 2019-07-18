---
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Controles UWP em aplicativos da área de trabalho
ms.date: 07/17/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 79dcd6069a5746e04565db660e6f5c03988a94a4
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303561"
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

As ilhas XAML exigem o Windows 10, versão 1903 e posterior. Para usar as ilhas XAML em seu aplicativo, você deve primeiro configurar seu projeto.

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

* Modifique seu projeto para usar Windows Runtime APIs. Para obter instruções, consulte [Este artigo](desktop-to-uwp-enhance.md#set-up-your-project).

* Instale o pacote NuGet mais recente do [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (para WPF) ou [Microsoft. Toolkit. Forms. UI. controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (para Windows Forms) em seu projeto. Certifique-se de instalar a versão 6.0.0-Preview 6.4 ou uma versão posterior do pacote.

### <a name="cwin32"></a>C++/Win32

* Modifique seu projeto para usar Windows Runtime APIs. Para obter instruções, consulte [Este artigo](desktop-to-uwp-enhance.md#set-up-your-project).
* Realize um dos seguintes procedimentos:

    **Empacote seu aplicativo em um pacote MSIX**. Empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix/) fornece muitos benefícios de implantação e tempo de execução.
    1. Instale o SDK do Windows 10, versão 1903 (ou uma versão posterior).
    2. Empacote seu aplicativo em um pacote MSIX adicionando um projeto de empacotamento de [aplicativos do Windows](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à sua solução e adicionando uma C++referência ao seu projeto/Win32.

    **Defina o valor maxversiontested no manifesto do aplicativo**. Se você não quiser empacotar seu aplicativo em um pacote MSIX, antes de poder usar as ilhas XAML, você deve adicionar um [manifesto do aplicativo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e adicionar o elemento **maxversiontested** ao manifesto para especificar que seu aplicativo é compatível com o Windows 10, versão 1903 ou posterior.
    1. Se você ainda não tiver um manifesto do aplicativo em seu projeto, adicione um novo arquivo XML ao seu projeto e nomeie-o **app. manifest**.
    2. No manifesto do aplicativo, inclua o elemento de **compatibilidade** e os elementos filho mostrados no exemplo a seguir. Substitua o atributo **ID** do elemento **maxversiontested** pelo número de versão do Windows 10 que você está direcionando (este deve ser o windows 10, versão 1903 ou uma versão posterior).

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

        > [!NOTE]
        > Ao adicionar um elemento **maxversiontested** a um manifesto do aplicativo, você poderá ver o seguinte aviso de compilação em seu projeto `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`:. Esse aviso não indica que algo está errado em seu projeto e pode ser ignorado.

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
