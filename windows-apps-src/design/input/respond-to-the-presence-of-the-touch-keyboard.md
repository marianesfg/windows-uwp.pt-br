---
Description: Saiba como adaptar a interface do usuário do seu aplicativo ao mostrar ou ocultar o teclado virtual.
title: Responder à presença do teclado virtual
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: teclado, acessibilidade, navegação, foco, texto, entrada e interação do usuário
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: e44b7cf5a61a795e52490f6d603aea0bcf87bea2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658291"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Responder à presença do teclado virtual

Saiba como adaptar a interface do usuário do seu aplicativo ao mostrar ou ocultar o teclado virtual.

### <a name="important-apis"></a>APIs Importantes

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![o teclado virtual no modo de layout padrão](images/keyboard/default.png)

<sup>O toque de teclado padrão modo de layout</sup>

O teclado virtual permite a entrada de texto para dispositivos que dão suporte para toque. Controles de entrada de texto da Plataforma Universal do Windows (UWP) invocam o teclado virtual por padrão quando um usuário toca em um campo de entrada editável. O teclado virtual normalmente fica visível enquanto o usuário navega entre controles em um formulário, mas esse comportamento pode variar com base nos outros tipos de controle dentro do formulário.

Para dar suporte a comportamento de teclado de toque correspondente em um controle de entrada de texto personalizado que não é derivado de um controle de entrada de texto padrão, você deve usar o <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> classe para expor seus controles para automação de interface do usuário da Microsoft e Implemente os padrões de controle de automação de interface do usuário corretos. Consulte [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) e [Pares de automação personalizados](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers).

Depois que esse suporte for adicionado ao seu controle personalizado, você poderá responder adequadamente à presença do teclado virtual.

**Pré-requisitos:**

Este tópico complementa [Interações por teclado](keyboard-interactions.md).

Você deve ter um conhecimento básico de interações por teclado padrão, manipulação de entradas e eventos por teclado e Automação da Interface do Usuário.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

- [Crie seu primeiro aplicativo](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

**Diretrizes de experiência do usuário:**

Para obter dicas úteis sobre a criação de um aplicativo útil e envolvente otimizado para entrada do teclado, consulte [interações de teclado](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Teclado virtual e uma interface do usuário personalizada

Estas são algumas recomendações básicas para controles de entrada de texto personalizados.

- Exiba o teclado virtual durante toda a interação com seu formulário.

- Certifique-se de que seus controles personalizados tenham a automação de interface do usuário apropriada [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) do teclado persistir quando o foco sai de um campo de entrada de texto enquanto estiver no contexto de entrada de texto. Por exemplo, se você tiver um menu que é aberto no meio de um cenário de entrada de texto e quiser que o teclado persista, o menu deverá ter o **AutomationControlType** do Menu.

- Não manipule as propriedades da Automação da Interface do Usuário para controlar o teclado virtual. Outras ferramentas de acessibilidade dependem da precisão das propriedades da Automação da Interface do Usuário

- Garanta que os usuários sempre possam ver o campo de entrada com o qual estejam interagindo.

    Devido ao fato de o teclado virtual obstruir uma grande parte da tela, a UWP garante que o campo de entrada com foco role para a exibição à medida um usuário navegar pelos controles no formulário, incluindo os controles que não estejam em exibição no momento.

    Ao personalizar a interface do usuário, forneça um comportamento semelhante na aparência do teclado de toque manipulando o [mostrando](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) e [ocultando](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) eventos expostos pela [ **InputPane** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) objeto.

    ![um formulário com e sem o teclado virtual em exibição](images/touch-keyboard-pan1.png)

    Em alguns casos, há elementos da interface do usuário que devem ficar na tela o tempo todo. Projete a interface do usuário de forma que os controles do formulário fiquem em uma região de movimento panorâmico e os elementos importantes da interface fiquem estáticos. Por exemplo:

    ![um formulário que contém áreas que devem ser sempre exibidas](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Manipulando os eventos Showing e Hiding

Aqui está um exemplo de anexar manipuladores de eventos para o [mostrando](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) e [ocultando](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) eventos de teclado de toque.

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

## <a name="related-articles"></a>Artigos relacionados

- [Interações de teclado](keyboard-interactions.md)
- [Acessibilidade do teclado](https://msdn.microsoft.com/library/windows/apps/mt244347)
- [Pares de automação personalizados](https://msdn.microsoft.com/library/windows/apps/mt297667)

**Exemplos**

- [Exemplo de teclado de toque](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**Amostras de arquivo-morto**

- [Entrada: Exemplo de teclado de toque](https://go.microsoft.com/fwlink/p/?linkid=246019)
- [Respondendo à aparência do teclado na tela de exemplo](https://go.microsoft.com/fwlink/p/?linkid=231633)
- [Exemplo de edição de texto XAML](https://go.microsoft.com/fwlink/p/?LinkID=251417)
- [Exemplo de acessibilidade do XAML](https://go.microsoft.com/fwlink/p/?linkid=238570)
