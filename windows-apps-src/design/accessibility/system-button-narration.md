---
Description: ''
title: Leitores de tela e eventos de botão de hardware
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: Windows 10, UWP, acessibilidade, narrador, leitor de tela
ms.localizationpriority: medium
ms.openlocfilehash: 41c6ed531f21a922b407ff3ba5aae93afb8d917e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234929"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>Leitores de tela e botões de sistema de hardware

Leitores de tela, como [o narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator), devem ser capazes de reconhecer e manipular eventos de botão do sistema de hardware e comunicar seu estado aos usuários. Em alguns casos, o leitor de tela pode precisar lidar exclusivamente com esses eventos de botão de hardware e não deixá-los emergir em outros manipuladores.

A partir do Windows 10 versão 2004, os aplicativos UWP podem escutar e manipular os eventos de botão do sistema de hardware **FN** da mesma maneira que outros botões de hardware. Anteriormente, esse botão do sistema atuava apenas como um modificador de como outros botões de hardware relataram seus eventos e estado.

> [!NOTE]
> O suporte ao botão Fn é específico do OEM e pode incluir recursos como, por exemplo, a capacidade de alternância/bloqueio (vs. uma combinação de teclas de pressionar e manter), juntamente com uma luz do indicador de bloqueio correspondente (o que pode não ser útil para usuários cegos ou com deficiência visual).

Os eventos do botão Fn são expostos por meio de uma nova [classe SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) no namespace [Windows. UI. Input](/uwp/api/windows.ui.input) . O objeto SystemButtonEventController dá suporte aos seguintes eventos:

- [SystemFunctionButtonPressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> O SystemButtonEventController não poderá receber esses eventos se eles já tiverem sido manipulados por um manipulador de prioridade mais alta.

## <a name="examples"></a>Exemplos

Nos exemplos a seguir, mostramos como criar um [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) com base em um DispatcherQueue e manipular os quatro eventos com suporte neste objeto.

É comum que mais de um dos eventos com suporte seja acionado quando o botão Fn é pressionado. Por exemplo, pressionar o botão Fn em um teclado de superfície aciona SystemFunctionButtonPressed, SystemFunctionLockChanged e SystemFunctionLockIndicatorChanged ao mesmo tempo.

1. Neste primeiro trecho de código, simplesmente incluímos os namespaces necessários e especificamos alguns objetos globais, incluindo o [DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) e os objetos [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller) para gerenciar o thread [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

   Em seguida, especificamos os [tokens de evento](/uwp/cpp-ref-for-winrt/event-token) retornados ao registrar os delegados de manipulação de eventos [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. Também especificamos um token de evento para o evento [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged) junto com um bool para indicar se o aplicativo está no "modo de aprendizado" (no qual o usuário está simplesmente tentando explorar o teclado sem executar nenhuma função).

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. Esse terceiro trecho de código inclui os delegados do manipulador de eventos correspondentes para cada evento suportado pelo objeto [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

   Cada manipulador de eventos anuncia o evento que ocorreu. Além disso, o manipulador FunctionLockIndicatorChanged também controla se o aplicativo está no modo de "aprendizado" ( `_isLearningMode` = true), que impede o evento de bolha a outros manipuladores e permite que o usuário Explore os recursos do teclado sem realmente executar a ação.

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>Veja também

[Classe SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)
