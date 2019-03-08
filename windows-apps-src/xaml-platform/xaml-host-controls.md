---
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Controles UWP em aplicativos da área de trabalho
ms.date: 01/11/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619701"
---
# <a name="uwp-controls-in-desktop-applications"></a>Controles UWP em aplicativos da área de trabalho

> [!NOTE]
> Ilhas XAML estão atualmente disponíveis como uma visualização do desenvolvedor. Embora incentivamos você a experimentá-las em seu próprio código de protótipo agora, não recomendamos que você usá-los no código de produção neste momento. Esses controles e APIs continuará a amadurecer e se estabilizar em futuras versões do Windows. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.
>
> Se você tiver comentários sobre Ilhas XAML, crie um novo problema nos [WindowsCommunityToolkit repositório](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) e deixe seus comentários lá. Se você preferir a enviar seus comentários em particular, você pode enviá-lo para XamlIslandsFeedback@microsoft.com. Seus insights e os cenários são extremamente importantes para nós.

Windows 10 agora permite que você use controles UWP em aplicativos de área de trabalho não UWP para que você pode aprimorar a aparência e a funcionalidade dos seus aplicativos de área de trabalho existentes com os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de controles da UWP. Isso significa que você pode usar os recursos UWP, como [Windows Ink](../design/input/pen-and-stylus-interactions.md) e os controles que dão suporte a [Fluent Design System](../design/fluent-design-system/index.md) em existentes do WPF, Windows Forms e aplicativos do Win32 em C++. Às vezes é chamado neste cenário de desenvolvedor *Ilhas XAML*.

Fornecemos várias maneiras de usar Ilhas XAML em seus aplicativos WPF, Windows Forms e Win32 em C++, dependendo da tecnologia ou estrutura que você está usando.

## <a name="wrapped-controls"></a>Controles encapsulados

Aplicativos WPF e Windows Forms podem usar uma seleção de controles da UWP encapsulados na [Kit de ferramentas de comunidade Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Fazemos referência a esses controles conforme *encapsulado controles* porque eles encapsulam a interface e a funcionalidade de um controle específico de UWP. Você pode adicionar esses controles diretamente para a superfície de design do seu projeto WPF ou Windows Forms e, em seguida, usá-los como qualquer outro controle WPF ou Windows Forms no designer.

> [!NOTE]
> Controles encapsulados não estão disponíveis para aplicativos da área de trabalho do C++ do Win32. Esses tipos de aplicativos devem usar o [XAML UWP API de hospedagem](#uwp-xaml-hosting-api).

Os seguintes controles UWP encapsulados estão atualmente disponíveis para aplicativos WPF e Windows Forms. Mais controles UWP encapsulado são planejados para versões futuras do Kit de ferramentas de comunidade do Windows.

| Controle | Mínimo de sistema operacional com suporte | Descrição |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versão 1803 | Usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fornece uma versão do **WebView** que é compatível com versões de sistema operacional mais. Este controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da web no Windows 10 versão 1803 e posterior e o mecanismo de renderização do Internet Explorer para mostrar o conteúdo da web em versões anteriores do Windows 10, Windows 8.x e Windows 7. |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versão 1809 (build 17763) | Fornece um barras de ferramentas na superfície e relacionados com base em Windows Ink interação do usuário em seu aplicativo de área de trabalho do Windows Forms ou WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versão 1809 (build 17763) | Insere um modo de exibição que transmite e renderiza o conteúdo de mídia, como o vídeo em seu aplicativo de área de trabalho do Windows Forms ou WPF. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versão 1809 (build 17763) | Permite que você exibir um mapa simbólico ou com realismo em seu aplicativo de área de trabalho do Windows Forms ou WPF. |

## <a name="host-controls"></a>Controles de host

Para cenários além daquelas abordadas pelos controles encapsulados disponíveis, os aplicativos WPF e Windows Forms também podem usar o [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no controlar a [Kit de ferramentas de comunidade Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Esse controle pode hospedar qualquer controle UWP que deriva de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecida pelo SDK do Windows, bem como controles de usuário personalizada. Esse controle dá suporte a SDK do Windows 10 Insider Preview build 17709 e versões mais recentes.

> [!NOTE]
> Controles de host não estão disponíveis para aplicativos da área de trabalho do C++ do Win32. Esses tipos de aplicativos devem usar o [XAML UWP API de hospedagem](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>XAML UWP API de hospedagem

Se você tiver um aplicativo Win32 em C++, você pode usar o *XAML UWP API de hospedagem* para hospedar qualquer controle UWP que deriva [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) em qualquer elemento de interface do usuário em seu aplicativo que tem um identificador de janela associado (HWND). Essa API foi introduzida no SDK de visualização do Windows 10 Insider build 17709. Para obter mais informações sobre como usar essa API, consulte [usando a API de hospedagem em um aplicativo da área de trabalho de XAML](using-the-xaml-hosting-api.md).

> [!NOTE]
> Aplicativos de desktop do Win32 em C++ devem usar o XAML UWP API de hospedagem para hospedar controles da UWP. Controles encapsulados e controles de host não estão disponíveis para esses tipos de aplicativos. Para aplicativos WPF e Windows Forms, é recomendável que você use os controles de host e controles encapsulados no Kit de ferramentas de comunidade do Windows em vez da XAML UWP API de hospedagem. Esses controles usam o XAML UWP API de hospedagem internamente e fornecem uma experiência de desenvolvimento mais simples. No entanto, você pode usar o XAML UWP API de hospedagem diretamente em aplicativos WPF e Windows Forms se você escolher.

## <a name="architecture-overview"></a>Visão geral da arquitetura

Veja como esses controles são organizados na arquitetura. Os nomes usados neste diagrama estão sujeitos a alterações.  

![Arquitetura de controle de host](images/host-controls.png)

As APIs que aparecem na parte inferior do diagrama com o SDK do Windows. Os controles que você adiciona ao seu designer são fornecidos como pacotes Nuget no Kit de Ferramentas da Comunidade do Windows.

Esses novos controles possuem limitações, portanto, antes de usá-los, revise o que ainda não tem suporte e o que está funcionando apenas com soluções alternativas.

## <a name="limitations"></a>Limitações

### <a name="whats-supported"></a>O que é suportado

Na maior parte, tudo o que é compatível, a menos quando explicitamente citado na lista a seguir.

### <a name="whats-supported-only-with-workarounds"></a>O que é suportado apenas com soluções alternativas

:heavy_check_mark: Hospedando vários controles de caixa de entrada dentro de várias janelas. Você deverá colocar cada janela em seu próprio thread.

:heavy_check_mark: Usando ``x:Bind`` com controles hospedados. É necessário declarar o modelo de dados em uma biblioteca .NET padrão.

:heavy_check_mark: C#-com base em controles de terceiros. Se você tiver o código-fonte para um controle de terceiros, você pode compilar em relação a ele.

### <a name="whats-not-yet-supported"></a>O que ainda não tem suporte

:no_entry_sign: Ferramentas de acessibilidade que funcionam perfeitamente no aplicativo e os controles hospedados.

:no_entry_sign: Conteúdo localizado nos controles que você adiciona a aplicativos que não contêm um pacote de aplicativo do Windows.

:no_entry_sign: Referências de ativos feitas no XAML nos aplicativos que não contêm um pacote de aplicativo do Windows.

:no_entry_sign: Controles de responder corretamente a alterações em DPI e escala.

:no_entry_sign: Adicionando um **WebView** controle a um controle de usuário personalizada, (no thread, fora do thread ou fora do processo).

:no_entry_sign: O [revelam Realce](https://docs.microsoft.com/windows/uwp/design/style/reveal) efeito Fluent.

:no_entry_sign: Escrita à tinta embutido, @Places, e @People para controles de entrada.

:no_entry_sign: Atribuindo teclas de aceleração.

:no_entry_sign: Controles de terceiros baseados em C++.

:no_entry_sign: Hospedando controles de usuário personalizados.

Os itens nessa lista provavelmente são alterados à medida que melhoramos a experiência fluente para a área de trabalho.  
