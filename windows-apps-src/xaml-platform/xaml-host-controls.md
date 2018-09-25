---
author: normesta
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Controles UWP em aplicativos da área de trabalho
ms.author: normesta
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 6b8c263b030cbb8f945ffb13a24b6dff3af28fcc
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4173629"
---
# <a name="uwp-controls-in-desktop-applications"></a>Controles UWP em aplicativos da área de trabalho

> [!NOTE]
> As APIs e os controles discutidos neste artigo estão disponíveis atualmente como uma visualização de desenvolvedor. Embora Encorajamos você a experimentá-los em seu próprio código de protótipo agora, não recomendamos que você usá-los no código de produção neste momento. Esses controles e APIs continuará a se desenvolver e estabilizar em futuras versões do Windows. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

Windows 10 agora permite que você use controles UWP em aplicativos da área de trabalho não UWP para que você possa melhorar a aparência e a funcionalidade de seus aplicativos da área de trabalho existentes com os recursos de interface do usuário do Windows 10 mais recentes que só estão disponíveis por meio de controles UWP. Isso significa que você pode usar recursos UWP, como o [Sistema de Design fluente](../design/fluent-design-system/index.md) e [Windows Ink](../design/input/pen-and-stylus-interactions.md) em seu existente WPF, formulários do Windows e aplicativos C++ Win32. Esse cenário de desenvolvedor às vezes é chamado *Ilhas XAML*.

Fornecemos várias maneiras de usar Ilhas XAML em seus aplicativos da área de trabalho, dependendo da tecnologia ou estrutura, que você está usando.

## <a name="wrapped-controls"></a>Controles empacotados

Aplicativos WPF e Windows Forms podem usar uma seleção de controles UWP empacotados no [Kit de ferramentas do Windows da comunidade](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Você pode adicionar esses controles diretamente à superfície de design do seu projeto do WPF ou Windows Forms e, em seguida, usar como qualquer outro controle WPF ou Windows Forms no designer. Nos referimos a esses controles como *encapsuladas controles* porque eles encapsulam a interface e a funcionalidade de um controle específico da UWP.

Os seguintes controles empacotados suportam Windows 10, versão 1803 e posterior.

* [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview). Esse controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da web em um aplicativo WPF ou Windows Forms.
* [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible). Esse controle é uma versão do **WebView** que é compatível com o Windows 10 e versões anteriores do Windows. Esse controle usa o mecanismo de renderização do Microsoft Edge para mostrar o conteúdo da web no Windows 10 (versão 1803 e posteriores) e o mecanismo de renderização do Internet Explorer para mostrar o conteúdo da web no Windows 7 e Windows 8. x.

Os seguintes controles empacotados dão suporte a versões de 17709 e posteriores de compilação do SDK do Windows 10 Insider Preview.

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar). Esses controles fornecem uma superfície e relacionados barras de ferramentas para interação do usuário com base em Windows Ink em seu aplicativo da área de trabalho do Windows Forms ou WPF.
* [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement). Esse controle incorpora um modo de exibição que transmite e renderiza o conteúdo de mídia, como vídeo em seu aplicativo da área de trabalho do Windows Forms ou WPF.

UWP mais resumida controles para WPF e aplicativos do Windows Forms são planejados para versões futuras do Kit de ferramentas da comunidade Windows.

> [!NOTE]
> Controles empacotados não estão disponíveis para aplicativos da área de trabalho do C++ Win32. Esses tipos de aplicativos devem usar o [XAML da UWP que hospeda a API](#uwp-xaml-hosting-api).

## <a name="host-controls"></a>Controles de host

Para cenários além daqueles coberto por controles empacotados disponíveis, aplicativos WPF e Windows Forms também podem usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no [Kit de ferramentas do Windows da comunidade](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Esse controle pode hospedar qualquer controle UWP que deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles personalizados do usuário. Esse controle oferece suporte a versões de 17709 e posteriores de compilação do SDK do Windows 10 Insider Preview.

> [!NOTE]
> Hospedar controles não estão disponíveis para aplicativos da área de trabalho do C++ Win32. Esses tipos de aplicativos devem usar o [XAML da UWP que hospeda a API](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>API de hospedagem XAML da UWP

Se você tiver um aplicativo C++ Win32, você pode usar o *XAML da UWP que hospeda a API* para hospedar qualquer controle UWP que deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) em qualquer elemento de interface do usuário em seu aplicativo que tem um identificador de janela associado (HWND). Essa API foi introduzida no SDK do Windows 10 Insider Preview compilação 17709. Para obter mais informações sobre como usar essa API, consulte [usando o XAML que hospeda a API em um aplicativo da área de trabalho](using-the-xaml-hosting-api.md).

> [!NOTE]
> Aplicativos da área de trabalho do C++ Win32 devem usar o XAML da UWP que hospeda a API para hospedar controles UWP. Controles empacotados e controles de host não estão disponíveis para esses tipos de aplicativos. Para aplicativos WPF e Windows Forms, recomendamos que você use os controles empacotados e controles de host no Kit de ferramentas de comunidade do Windows em vez do XAML da UWP que hospeda a API. Esses controles usam o XAML da UWP que hospeda a API internamente e proporcionar uma experiência de desenvolvimento mais simples. No entanto, você pode usar o XAML da UWP que hospeda API diretamente em aplicativos WPF e Windows Forms, se você escolher.

## <a name="architecture-overview"></a>Visão geral da arquitetura

Veja como esses controles são organizados na arquitetura. Os nomes usados neste diagrama estão sujeitos a alterações.  

![Arquitetura de controle de host](images/host-controls.png)

As APIs que aparecem na parte inferior do diagrama com o SDK do Windows. Os controles que você adiciona ao seu designer são fornecidos como pacotes Nuget no Kit de Ferramentas da Comunidade do Windows.

Esses novos controles possuem limitações, portanto, antes de usá-los, revise o que ainda não tem suporte e o que está funcionando apenas com soluções alternativas.

## <a name="limitations"></a>Limitações

### <a name="whats-supported"></a>O que é suportado

Na maior parte, tudo o que é compatível, a menos quando explicitamente citado na lista a seguir.

### <a name="whats-supported-only-with-workarounds"></a>O que é suportado apenas com soluções alternativas

:heavy_check_mark: hospedar vários controles de caixa de entrada em várias janelas. Você deverá colocar cada janela em seu próprio thread.

:heavy_check_mark: usando ``x:Bind`` com controles hospedados. É necessário declarar o modelo de dados em uma biblioteca .NET padrão.

:heavy_check_mark: controles de terceiros com base em C#. Se você tiver o código-fonte para um controle de terceiros, você pode compilar em relação a ele.

### <a name="whats-not-yet-supported"></a>O que ainda não tem suporte

:no_entry_sign: ferramentas de acessibilidade de funcionam sem interrupção no aplicativo e nos controles hospedados.

:no_entry_sign: conteúdo localizado nos controles que você adiciona aos aplicativos que não contêm um pacote do aplicativo do Windows.

:no_entry_sign: referências de ativo feitas em XAML nos aplicativos que não contêm um pacote do aplicativo do Windows.

:no_entry_sign: controles que respondem adequadamente às mudanças em DPI e escala.

:no_entry_sign: adicionar um controle **WebView** a um controle de usuário personalizado, (no thread, fora do thread, ou fora de proc).

:no_entry_sign: o efeito fluente de [realce de revelação](https://docs.microsoft.com/windows/uwp/design/style/reveal).

:no_entry_sign: tinta embutida, @Places e @People para controles de entrada.

:no_entry_sign: atribuir chaves de acelerador.

:no_entry_sign: controles de terceiros com base em C++.

:no_entry_sign: hospedagem de controles personalizados do usuário.

Os itens nessa lista provavelmente são alterados à medida que melhoramos a experiência fluente para a área de trabalho.  
