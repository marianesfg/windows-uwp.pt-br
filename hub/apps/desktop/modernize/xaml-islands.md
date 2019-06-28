---
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Controles da UWP em aplicativos da área de trabalho
ms.date: 04/16/2019
ms.topic: article
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e6074202a05c80a9dc759cdf81b2c20c7cc17d07
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414096"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Controles de host UWP XAML em aplicativos da área de trabalho (Ilhas de XAML)

A partir do Windows 10, versão 1903, você pode hospedar controles UWP em aplicativos de área de trabalho não UWP usando um recurso chamado *XAML Ilhas*. Esse recurso permite que você aprimorar a aparência, a aparência e a funcionalidade dos seus aplicativos de área de trabalho existentes com os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de controles da UWP. Isso significa que você pode usar os recursos UWP, como [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) e os controles que dão suporte a [Fluent Design System](/windows/uwp/design/fluent-design-system/index) em existentes do WPF, Windows Forms e aplicativos do Win32 em C++.

Fornecemos várias maneiras de usar ilhas de XAML no seu WPF, Windows Forms, e C++ aplicativos do Win32, dependendo da tecnologia ou estrutura que você está usando. 

> [!NOTE]
> Se você tiver comentários sobre ilhas de XAML, crie um novo problema nos [Microsoft.Toolkit.Win32 repositório](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários lá. Se você preferir a enviar seus comentários em particular, você pode enviá-lo para XamlIslandsFeedback@microsoft.com. Seus insights e os cenários são extremamente importantes para nós.

## <a name="how-do-xaml-islands-work"></a>Como funcionam as ilhas de XAML?

A partir do Windows 10, versão 1903, fornecemos dois modos de usar ilhas de XAML no seu WPF, Windows Forms, e C++ aplicativos Win32:

* O SDK do Windows fornece várias classes de tempo de execução do Windows e interfaces COM que seu aplicativo pode usar para hospedar qualquer controle UWP que deriva de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Coletivamente, essas classes e interfaces são chamados de *XAML UWP API de hospedagem*, e eles permitem que você para hospedar controles da UWP em qualquer elemento de interface do usuário em seu aplicativo que tem um identificador de janela associado (HWND). Para obter mais informações sobre essa API, consulte [usando a API de hospedagem de XAML](using-the-xaml-hosting-api.md).

* O [Kit de ferramentas de comunidade Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) também fornece controles adicionais da ilha de XAML para WPF e Windows Forms. Esses controles usam o XAML UWP API de hospedagem internamente e implementam todo o comportamento que seriam necessários para lidar com você mesmo se você tiver usado o XAML UWP API diretamente, incluindo alterações de layout e navegação de teclado de hospedagem. Para aplicativos WPF e Windows Forms, é altamente recomendável que você use esses controles em vez da XAML UWP API de hospedagem diretamente porque eles abstraem a muitos dos detalhes de implementação de usar a API. Observe que, a partir do Windows 10, versão 1903, esses controles são [disponível como uma visualização do desenvolvedor](#feature-roadmap).

> [!NOTE]
> Aplicativos de desktop do Win32 em C++ devem usar o XAML UWP API de hospedagem para hospedar controles da UWP. Os controles da ilha de XAML no Kit de ferramentas de comunidade do Windows não estão disponíveis para C++ aplicativos da área de trabalho do Win32.

Há dois tipos de controles da ilha de XAML fornecidos pelo Kit de ferramentas de comunidade do Windows para aplicativos WPF e Windows Forms: *encapsulado controles* e *hospedar controles*. 

### <a name="wrapped-controls"></a>Controles encapsulados

Aplicativos WPF e Windows Forms podem usar uma seleção de controles da UWP encapsulados na [Kit de ferramentas de comunidade Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Eles são chamados *encapsulado controles* porque eles encapsulam a interface e a funcionalidade de um controle específico de UWP. Você pode adicionar esses controles diretamente para a superfície de design do seu projeto WPF ou Windows Forms e, em seguida, usá-los como qualquer outro controle WPF ou Windows Forms no designer.

Os seguintes controles UWP encapsulados para a implementação de XAML Ilhas estão atualmente disponíveis para aplicativos WPF e Windows Forms.

| Controle | Mínimo de sistema operacional com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versão 1903 | Fornece um barras de ferramentas na superfície e relacionados com base em Windows Ink interação do usuário em seu aplicativo de área de trabalho do Windows Forms ou WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versão 1903 | Insere um modo de exibição que transmite e renderiza o conteúdo de mídia, como o vídeo em seu aplicativo de área de trabalho do Windows Forms ou WPF. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versão 1903 | Permite que você exibir um mapa simbólico ou com realismo em seu aplicativo de área de trabalho do Windows Forms ou WPF. |

Além dos controles encapsulados para ilhas de XAML, o Kit de ferramentas de comunidade do Windows também fornece os seguintes controles para hospedar o conteúdo da web.

| Controle | Mínimo de sistema operacional com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versão 1803 | Usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fornece uma versão do **WebView** que é compatível com versões de sistema operacional mais. Este controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da web no Windows 10 versão 1803 e posterior e o mecanismo de renderização do Internet Explorer para mostrar o conteúdo da web em versões anteriores do Windows 10, Windows 8.x e Windows 7. |

### <a name="host-controls"></a>Controles de host

Para cenários além daquelas abordadas pelos controles encapsulados disponíveis, os aplicativos WPF e Windows Forms também podem usar o [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no controlar a [Kit de ferramentas de comunidade Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Esse controle pode hospedar qualquer controle UWP que deriva de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecida pelo SDK do Windows, bem como controles de usuário personalizada. Esse controle dá suporte a SDK do Windows 10 Insider Preview build 17709 e versões mais recentes.

### <a name="architecture-overview"></a>Visão geral da arquitetura

Veja como esses controles são organizados na arquitetura.

![Arquitetura de controle de host](images/xaml-islands/host-controls.png)

As APIs que aparecem na parte inferior do diagrama com o SDK do Windows. Os controles de host e controles encapsulados estão disponíveis por meio de pacotes do Nuget no Kit de ferramentas de comunidade do Windows.

## <a name="requirements"></a>Requisitos

Ilhas de XAML exigem o Windows 10, versão 1903 e posterior. Para usar ilhas de XAML em seu aplicativo, você deve primeiro configurar seu projeto.

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>Etapa 1: Modifique seu projeto para usar APIs do Windows Runtime

Para obter instruções, consulte [deste artigo](desktop-to-uwp-enhance.md#set-up-your-project).

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>Etapa 2: Suporte a ilha de XAML de habilitar no seu projeto

Fazer uma das seguintes alterações ao seu projeto para habilitar o suporte a ilha de XAML. Para obter mais detalhes, consulte [esta postagem de blog](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117).

#### <a name="option-1-package-your-application-in-an-msix-package"></a>Opção 1: Empacotar o aplicativo em um pacote MSIX  

Instale o Windows 10, versão SDK 1903 (ou uma versão posterior). Em seguida, empacotar o aplicativo em um pacote MSIX adicionando um [Windows Application Packaging Project](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) para sua solução e adicionando uma referência ao seu projeto WPF ou Windows Forms.

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>Opção 2: Defina o valor de maxVersionTested no manifesto do assembly

Se você não quiser empacotar o aplicativo em um pacote MSIX, você pode adicionar um [manifesto do assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e adicione o **maxVersionTested** valor para o manifesto para especificar que seu aplicativo é compatível com o Windows 10, versão 1903 ou posterior.

1. Se você ainda não tiver um assembly do manifesto em seu projeto, adicionar um novo arquivo XML ao seu projeto e nomeie- **manifest**. Para um aplicativo WPF ou Windows Forms, verifique se você também atribuir a **manifesto** propriedade a ser **. manifest** no **aplicativo** página do seu [projeto propriedades](https://docs.microsoft.com/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources).
2. Em seu manifesto do assembly, incluir a **compatibilidade** elemento e os elementos filho mostrados no exemplo a seguir. Substitua os **Id** atributo da **maxVersionTested** elemento com o número de versão de destino do Windows 10 (deve ser Windows 10, versão 1903 ou uma versão posterior). 

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

## <a name="feature-roadmap"></a>Roteiro de recursos

A partir da versão do Windows 10, versão 1903, os controles de host no Kit de ferramentas de comunidade do Windows e encapsulados controles ainda estão na visualização do desenvolvedor até o lançamento da versão 1.0 dos controles está disponível.

* A versão 1.0 dos controles para o .NET Framework 4.6.2 e posterior são planejados para ser lançado na [versão 6.0 do Kit de ferramentas](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* A versão 1.0 dos controles do .NET Core 3 são planejados para uma versão posterior do Kit de ferramentas.
* Se você quiser experimentar mais recentes visualizações das versões desses controles versão 1.0 para o .NET Framework e .NET Core 3, consulte o **6.0.0-preview3** de pacotes NuGet na [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) Galeria.

Para obter mais detalhes, consulte [esta postagem de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Recursos adicionais

Para obter mais informações em segundo plano e tutoriais sobre como usar ilhas de XAML, consulte os artigos e recursos a seguir:

* [Modernize um tutorial de aplicativo do WPF](modernize-wpf-tutorial.md): Este tutorial fornece instruções passo a passo para usar os controles de host e controles encapsulados no Kit de ferramentas de comunidade do Windows para adicionar controles UWP para um aplicativo de linha de negócios existente do WPF. Este tutorial inclui o código completo para o aplicativo do WPF, bem como instruções detalhadas para cada etapa no processo.
* [Ilhas de XAML v1 - atualizações e roteiro](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): Esta postagem de blog aborda muitas perguntas comuns sobre ilhas de XAML e fornece um roteiro de desenvolvimento detalhadas.
