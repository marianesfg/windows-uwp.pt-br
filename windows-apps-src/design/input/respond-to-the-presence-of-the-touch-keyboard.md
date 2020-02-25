---
Description: Saiba como adaptar a interface do usuário do seu aplicativo ao mostrar ou ocultar o teclado virtual.
title: Responder à presença do teclado virtual
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: teclado, acessibilidade, navegação, foco, texto, entrada e interação do usuário
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: e26bbe00bba8b3d91d7ee842cb4d9c984a941f2b
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521357"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Responder à presença do teclado virtual

Saiba como adaptar a interface do usuário do seu aplicativo ao mostrar ou ocultar o teclado virtual.

### <a name="important-apis"></a>APIs Importantes

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![o teclado virtual no modo de layout padrão](images/keyboard/default.png)

<sup>O teclado de toque no modo de layout padrão</sup>

O teclado virtual permite a entrada de texto para dispositivos que dão suporte para toque. Controles de entrada de texto da Plataforma Universal do Windows (UWP) invocam o teclado virtual por padrão quando um usuário toca em um campo de entrada editável. O teclado virtual normalmente fica visível enquanto o usuário navega entre controles em um formulário, mas esse comportamento pode variar com base nos outros tipos de controle dentro do formulário.

Para dar suporte ao comportamento de teclado de toque correspondente em um controle de entrada de texto personalizado que não é derivado de um controle de entrada de texto padrão, você deve usar a classe <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> para expor seus controles à automação da interface do usuário da Microsoft e implementar os padrões de controle de automação da interface do usuário corretos. Consulte [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) e [Pares de automação personalizados](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers).

Depois que esse suporte for adicionado ao seu controle personalizado, você poderá responder adequadamente à presença do teclado virtual.

**Pré-requisitos**

Este tópico complementa [Interações por teclado](keyboard-interactions.md).

Você deve ter um conhecimento básico de interações por teclado padrão, manipulação de entradas e eventos por teclado e Automação da Interface do Usuário.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

- [Crie seu primeiro aplicativo](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

**Diretrizes de experiência do usuário:**

Para obter dicas úteis sobre como criar um aplicativo útil e atraente otimizado para entrada de teclado, consulte [interações de teclado](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Teclado virtual e uma interface do usuário personalizada

Estas são algumas recomendações básicas para controles de entrada de texto personalizados.

- Exiba o teclado virtual durante toda a interação com seu formulário.

- Verifique se os controles personalizados têm a [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) de automação da interface do usuário apropriada para o teclado persistir quando o foco se move de um campo de entrada de texto no contexto de entrada de texto. Por exemplo, se você tiver um menu que é aberto no meio de um cenário de entrada de texto e quiser que o teclado persista, o menu deverá ter o **AutomationControlType** do Menu.

- Não manipule as propriedades da Automação da Interface do Usuário para controlar o teclado virtual. Outras ferramentas de acessibilidade dependem da precisão das propriedades da Automação da Interface do Usuário

- Garanta que os usuários sempre possam ver o campo de entrada com o qual estejam interagindo.

    Devido ao fato de o teclado virtual obstruir uma grande parte da tela, a UWP garante que o campo de entrada com foco role para a exibição à medida um usuário navegar pelos controles no formulário, incluindo os controles que não estejam em exibição no momento.

    Ao personalizar sua interface do usuário, forneça um comportamento semelhante na aparência do teclado de toque manipulando os eventos de [exibição](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) e [ocultação](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) expostos pelo objeto [**InputPane**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) .

    ![um formulário com e sem o teclado virtual em exibição](images/touch-keyboard-pan1.png)

    Em alguns casos, há elementos da interface do usuário que devem ficar na tela o tempo todo. Projete a interface do usuário de forma que os controles do formulário fiquem em uma região de movimento panorâmico e os elementos importantes da interface fiquem estáticos. Por exemplo:

    ![um formulário que contém áreas que devem ser sempre exibidas](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Manipulando os eventos Showing e Hiding

Aqui está um exemplo de anexação de manipuladores de eventos para [Mostrar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) e [ocultar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) eventos do teclado de toque.

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>{1&gt;{2&gt;Artigos relacionados&lt;2}&lt;1}

- [Interações de teclado](keyboard-interactions.md)
- [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)
- [Pares de automação personalizados](https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers)

**Exemplos**

- [Exemplo de teclado de toque](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**Amostras de arquivo-morto**

- [Entrada: exemplo de teclado de toque](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [Respondendo à aparência do exemplo de teclado na tela](https://code.msdn.microsoft.com/windowsapps/keyboard-events-sample-866ba41c)
- [Exemplo de edição de texto XAML](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
- [Exemplo de acessibilidade XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
