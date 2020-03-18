---
description: Este guia ajudará você a criar interfaces do usuário do UWP baseadas no Fluent diretamente nos aplicativos WPF e do Windows Forms
title: Controles UWP em aplicativos da área de trabalho
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 061ad7a3f63fc92dd2f865f8870c7de5edf862af
ms.sourcegitcommit: 756217c559155e172087dee4d762d328c6529db6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78935354"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hospedar controles XAML do UWP em aplicativos da área de trabalho (Ilhas XAML)

No Windows 10 em diante, versão 1903, você pode hospedar controles UWP em aplicativos da área de trabalho que não sejam UWP usando um recurso chamado *Ilhas XAML*. Esse recurso permite que você aprimore a aparência e a funcionalidade de seus aplicativos WPF, C++ Win32 e do Windows Forms existentes, com os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio dos controles UWP. Isso significa que você pode usar recursos do UWP, como o [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions), e controles que dão suporte ao [Sistema Fluent Design](/windows/uwp/design/fluent-design-system/index) em seus aplicativos WPF, Windows Forms e C++ Win32 existentes.

Você pode hospedar qualquer controle UWP derivado de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo:

* Qualquer controle UWP interno fornecido pela biblioteca do WinUI ou do SDK do Windows.
* Qualquer controle UWP personalizado (por exemplo, um controle de usuário que consiste em vários controles UWP que funcionam juntos). É necessário ter o código-fonte do controle personalizado para compilá-lo com o aplicativo.

Fundamentalmente, as Ilhas XAML são criadas com a *API de hospedagem XAML do UWP*. Essa API consiste em várias classes do Windows Runtime e interfaces COM que foram introduzidas no SDK do Windows 10, versão 1903. Também fornecemos um conjunto de controles .NET da Ilha XAML no [Kit de Ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) que usa a API de hospedagem XAML do UWP internamente e fornece uma experiência de desenvolvimento mais conveniente para aplicativos WPF e do Windows Forms.

A maneira como você usa as Ilhas XAML depende do tipo de aplicativo e dos tipos de controles UWP que deseja hospedar.

> [!NOTE]
> Caso tenha comentários sobre as Ilhas XAML, crie um problema no [repositório Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários nele. Se preferir enviar seus comentários de forma particular, envie-os para XamlIslandsFeedback@microsoft.com. Seus insights e seus cenários são extremamente importantes para nós.

## <a name="wpf-and-windows-forms-applications"></a>Aplicativos WPF e do Windows Forms

Recomendamos que os aplicativos WPF e do Windows Forms usem os controles .NET da Ilha XAML disponíveis no Kit de Ferramentas da Comunidade do Windows. Esses controles fornecem um modelo de objeto que simula (ou fornece acesso a) propriedades, métodos e eventos dos controles UWP correspondentes. Eles também processam o comportamento, como a navegação por teclado e as alterações de layout.

Há dois conjuntos de controles da Ilha XAML para aplicativos WPF e do Windows Forms: *controles encapsulados* e *controles de host*. 

### <a name="wrapped-controls"></a>Controles encapsulados

Os aplicativos WPF e do Windows Forms podem usar uma seleção de controles da Ilha XAML que encapsulam a interface e a funcionalidade de um controle UWP específico. Adicione esses controles diretamente à área de design do projeto do WPF ou do Windows Forms e use-os como qualquer outro controle WPF ou Windows Forms no designer.

Atualmente, os controles UWP encapsulados a seguir estão disponíveis no Kit de Ferramentas da Comunidade do Windows. 

| Control | Sistema operacional mínimo compatível | Descrição |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versão 1903 | Forneça uma superfície e as barras de ferramentas relacionadas para a interação do usuário baseada no Windows Ink em seu aplicativo da área de trabalho WPF ou do Windows Forms. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versão 1903 | Insere uma exibição que transmite e renderiza o conteúdo de mídia, como um vídeo no aplicativo da área de trabalho WPF ou do Windows Forms. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versão 1903 | Permite exibir um mapa simbólico ou fotorrealista no aplicativo da área de trabalho WPF ou do Windows Forms. |

Para obter um passo a passo que demonstra como usar os controles UWP encapsulados, confira [Hospedar um controle UWP padrão em um aplicativo WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Controles de host

Para controles personalizados e outros cenários além daqueles abordados pelos controles encapsulados disponíveis, os aplicativos WPF e do Windows Forms também podem usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) que está disponível no Kit de Ferramentas da Comunidade do Windows.

| Control | Sistema operacional mínimo compatível | Descrição |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, versão 1903 | Pode hospedar qualquer controle UWP derivado de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP internamente fornecido pelo SDK do Windows, bem como controles personalizados. |

Para obter passos a passos que demonstram como usar o controle **WindowsXamlHost**, confira [Hospedar um controle UWP padrão em um aplicativo WPF](host-standard-control-with-xaml-islands.md) e [Hospedar um controle UWP personalizado em um aplicativo WPF usando as Ilhas XAML](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> Só há suporte ao uso do controle **WindowsXamlHost** para hospedar controles UWP personalizados em aplicativos WPF e do Windows Forms direcionados ao .NET Core 3. Há suporte à hospedagem de controles UWP internos fornecidos pelo SDK do Windows em aplicativos direcionados ao .NET Framework ou ao .NET Core 3.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configurar o projeto para usar os controles .NET da Ilha XAML

Os controles .NET da Ilha XAML exigem o Windows 10, versão 1903 ou uma versão posterior. Para usar esses controles, instale um dos pacotes NuGet listados abaixo. Esses pacotes fornecem tudo o que você precisa para usar os controles de host e os controles encapsulados da Ilha XAML e incluem outros pacotes NuGet relacionados que também são necessários.

| Tipo de controle | Pacote NuGet  | Artigos relacionados |
|-----------------|----------------|---------------------|
| [Controles encapsulados](#wrapped-controls) | Versão 6.0.0 ou posterior destes pacotes: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hospedar um controle UWP padrão em um aplicativo WPF](host-standard-control-with-xaml-islands.md)  |
| [Controle de host](#host-controls) | Versão 6.0.0 ou posterior destes pacotes: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hospedar um controle UWP padrão em um aplicativo WPF](host-standard-control-with-xaml-islands.md)<br/>[Hospedar um controle UWP personalizado em um aplicativo WPF](host-custom-control-with-xaml-islands.md)  |

Esteja ciente dos seguintes detalhes:

* Os pacotes do controle de host também estão incluídos nos pacotes do controle encapsulado. Instale os pacotes do controle encapsulado caso deseje usar os dois conjuntos de controles.

* Se você estiver hospedando um controle UWP personalizado, o projeto do WPF ou do Windows Forms precisará ser direcionado ao .NET Core 3. Não há suporte à hospedagem de controles UWP personalizados em aplicativos direcionados ao .NET Framework. Você também precisará executar algumas etapas adicionais para referenciar o controle personalizado. Para obter mais informações, confira [Hospedar um controle UWP personalizado em um aplicativo WPF usando as Ilhas XAML](host-custom-control-with-xaml-islands.md).

### <a name="web-view-controls"></a>Controles de exibição da Web

O Kit de Ferramentas da Comunidade do Windows também fornece os controles .NET a seguir para hospedar o conteúdo da Web em aplicativos WPF e do Windows Forms. Esses controles costumam ser usados em cenários semelhantes de modernização de aplicativos da área de trabalho, como os controles da Ilha XAML, e são mantidos no mesmo [repositório Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) dos controles da Ilha XAML.

| Control | Sistema operacional mínimo compatível | Descrição |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versão 1803 | Usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fornece uma versão do **WebView** que é compatível com mais versões do sistema operacional. Esse controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da Web no Windows 10 versão 1803 e posterior e o mecanismo de renderização do Internet Explorer para mostrar o conteúdo da Web em versões anteriores do Windows 10, Windows 8.x e Windows 7. |

Para usar esses controles, instale um destes pacotes NuGet:

* WPF: [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>Aplicativos C++ Win32

Não há suporte para os controles .NET da Ilha XAML em aplicativos C++ Win32. Esses aplicativos precisam usar a *API de hospedagem XAML do UWP* fornecida pelo SDK do Windows 10 (versão 1903 e posterior).

A API de hospedagem XAML do UWP consiste em várias classes do Windows Runtime e interfaces COM C++ que o aplicativo Win32 pode usar para hospedar qualquer controle UWP derivado de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Você pode hospedar controles UWP em qualquer elemento de interface do usuário em seu aplicativo que tenha um identificador de janela associado (HWND). Para obter mais informações sobre essa API, incluindo os pré-requisitos, confira [Como usar a API de hospedagem XAML do UWP em um aplicativo C++ Win32](using-the-xaml-hosting-api.md).

> [!NOTE]
> Os controles encapsulados e os controles de host no Kit de Ferramentas da Comunidade do Windows usam a API de hospedagem XAML do UWP internamente e implementam todo o comportamento que, de outra forma, você precisará cuidar por conta própria se usar a API de hospedagem XAML do UWP diretamente, incluindo a navegação por teclado e as alterações de layout. Para aplicativos WPF e do Windows Forms, recomendamos expressamente que você use esses controles em vez da API de hospedagem XAML do UWP diretamente, porque eles abstraem muitos dos detalhes de implementação do uso da API.

## <a name="architecture-of-xaml-islands"></a>Arquitetura das Ilhas XAML

Veja a seguir uma visão rápida de como os diferentes tipos de controles da Ilha XAML são organizados em termos de arquitetura na API de hospedagem XAML do UWP.

![Arquitetura do controle de host](images/xaml-islands/host-controls.png)

As APIs exibidas na parte inferior do diagrama são fornecidas com o SDK do Windows. Os controles encapsulados e os controles de host estão disponíveis por meio de pacotes NuGet no Kit de Ferramentas da Comunidade do Windows.

## <a name="window-host-context-for-xaml-islands"></a>Contexto do host de janela para Ilhas XAML

Ao hospedar as Ilhas XAML em um aplicativo da área de trabalho, você pode ter várias árvores de conteúdo XAML em execução no mesmo thread simultaneamente. Para acessar o elemento raiz de uma árvore de conteúdo XAML em uma Ilha XAML e obter informações relacionadas sobre o contexto no qual ele está hospedado, use a classe [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). As classes [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) e [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) não fornecerão as informações corretas para as Ilhas XAML. Objetos [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) e [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) existem no thread e podem ser acessados pelo seu aplicativo, mas eles não retornarão limites nem visibilidade significativos (eles são sempre invisíveis e têm um tamanho de 1x1). Para obter mais informações, confira [Hosts de exibição em janelas](/windows/uwp/design/layout/show-multiple-views#windowing-hosts).

Por exemplo, para obter o retângulo delimitador da janela que contém um controle UWP hospedado em uma ilha XAML, use a propriedade [XamlRoot.Size](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot.size) do controle. Como todos os controles UWP que podem ser hospedados em uma ilha XAML derivam de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), você pode usar a propriedade [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.xamlroot) do controle para acessar o objeto **XamlRoot**.

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

Não use a propriedade [CoreWindows.Bounds](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.bounds) para obter o retângulo delimitador.

```csharp
// This will return incorrect information for a UWP control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

Para uma tabela de APIs relacionadas à janela comum que você deve evitar no contexto de Ilhas XAML e as substituições de [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) recomendadas, confira a tabela [nesta seção](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts).

## <a name="feature-roadmap"></a>Roteiro do recurso

Este é o estado atual dos recursos relacionados às Ilhas XAML no Windows 10, versão 1903 em diante e na versão 6.0 do Kit de Ferramentas da Comunidade do Windows:

* **Aplicativos C++ Win32:** a API de hospedagem XAML do UWP é considerada a versão 1.0 no Windows 10, versão 1903 em diante.
* **Aplicativos gerenciados direcionados ao .NET Framework 4.6.2 e posterior:** os controles da Ilha XAML que estão disponíveis nos [pacotes NuGet da versão 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls) são considerados a versão 1.0 nos aplicativos direcionados ao .NET Framework 4.6.2 e posterior.
* **Aplicativos gerenciados direcionados ao .NET Core 3.0 e posterior:** os controles que estão disponíveis nos [pacotes NuGet da versão 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls) ainda estão no Developer Preview para aplicativos direcionados ao .NET Core 3.0 e posterior. A versão 1.0 desses controles para o .NET Core 3.0 e posterior está planejada para uma versão posterior.

## <a name="additional-resources"></a>Recursos adicionais

Para obter mais informações básicas e tutoriais sobre como usar as Ilhas XAML, confira os seguintes artigos e recursos:

* [Tutorial: modernizar um aplicativo WPF](modernize-wpf-tutorial.md): esse tutorial fornece instruções passo a passo de uso dos controles encapsulados e dos controles de host no Kit de Ferramentas da Comunidade do Windows para adicionar controles UWP a um aplicativo WPF de linha de negócios existente. Esse tutorial inclui o código completo para o aplicativo WPF, bem como instruções detalhadas de cada etapa do processo.
* [Exemplos de código de Ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples): este repositório contém exemplos do Windows Forms, do WPF e do C++/Win32 que demonstram como usar as Ilhas XAML.
* [Ilhas XAML v1: atualizações e roteiro](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): Essa postagem no blog aborda muitas perguntas comuns sobre as Ilhas XAML e fornece um roteiro de desenvolvimento detalhado.
