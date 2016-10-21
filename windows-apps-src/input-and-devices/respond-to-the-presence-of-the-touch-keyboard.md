---
author: Karl-Bridge-Microsoft
Description: "Saiba como adaptar a interface do usuário do seu aplicativo ao mostrar ou ocultar o teclado virtual."
title: "Responder à presença do teclado virtual"
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 97a626aedff1b0915c845f151b16b3678e1cf977

---

# Responder à presença do teclado virtual

Saiba como adaptar a interface do usuário do seu aplicativo ao mostrar ou ocultar o teclado virtual.


**APIs importantes**

-   [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185)
-   [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255)



![o teclado virtual no modo de layout padrão](images/touchkeyboard-standard.png)

<sup>o teclado \\virtual\\ \\no\\ modo de \\layout\\ padrão</sup>

O teclado virtual permite a entrada de texto para dispositivos que dão suporte para toque. Controles de entrada de texto da Plataforma Universal do Windows (UWP) invocam o teclado virtual por padrão quando um usuário toca em um campo de entrada editável. O teclado virtual normalmente fica visível enquanto o usuário navega entre controles em um formulário, mas esse comportamento pode variar com base nos outros tipos de controle dentro do formulário.

Para dar suporte ao comportamento do teclado virtual correspondente em um controle de entrada de texto personalizado que não derive de um controle de entrada de texto padrão, você deve usar a classe [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185) para expor seus controles à Automação da Interface do Usuário da Microsoft e implementar os padrões de controle corretos da Automação da Interface do Usuário. Consulte [Acessibilidade do teclado](https://msdn.microsoft.com/library/windows/apps/mt244347) e [Pares de automação personalizados](https://msdn.microsoft.com/library/windows/apps/mt297667).

Depois que esse suporte for adicionado ao seu controle personalizado, você poderá responder adequadamente à presença do teclado virtual.

**Pré-requisitos:  **

Este tópico complementa [Interações por teclado](keyboard-interactions.md).

Você deve ter um conhecimento básico de interações por teclado padrão, manipulação de entradas e eventos por teclado e Automação da Interface do Usuário.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário:  **

Para obter dicas úteis sobre o design de um aplicativo funcional e interessante, otimizado para entrada por teclado, consulte [Diretrizes de design do teclado](https://msdn.microsoft.com/library/windows/apps/hh972345).

## Teclado virtual e uma interface do usuário personalizada


Estas são algumas recomendações básicas para controles de entrada de texto personalizados.

-   Exiba o teclado virtual durante toda a interação com seu formulário.

-   Certifique-se de que os controles personalizados tenham o [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/br209182) adequado da Automação da Interface do Usuário para garantir a persistência do teclado quando o foco se mover de um campo de entrada de texto, no contexto da entrada de texto. Por exemplo, se você tiver um menu que é aberto no meio de um cenário de entrada de texto e quiser que o teclado persista, o menu deverá ter o **AutomationControlType** do Menu.

-   Não manipule as propriedades da Automação da Interface do Usuário para controlar o teclado virtual. Outras ferramentas de acessibilidade dependem da precisão das propriedades da Automação da Interface do Usuário

-   Garanta que os usuários sempre possam ver o campo de entrada com o qual estejam interagindo.

    Devido ao fato de o teclado virtual obstruir uma grande parte da tela, a UWP garante que o campo de entrada com foco role para a exibição à medida um usuário navegar pelos controles no formulário, incluindo os controles que não estejam em exibição no momento.

    Ao personalizar sua interface do usuário, forneça um comportamento semelhante sobre a aparência do teclado virtual manipulando os eventos [**Showing**](https://msdn.microsoft.com/library/windows/apps/br242262) e [**Hiding**](https://msdn.microsoft.com/library/windows/apps/br242260), expostos pelo objeto [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255).

    ![um formulário com e sem o teclado virtual em exibição](images/touch-keyboard-pan1.png)

    Em alguns casos, há elementos da interface do usuário que devem ficar na tela o tempo todo. Projete a interface do usuário de forma que os controles do formulário fiquem em uma região de movimento panorâmico e os elementos importantes da interface fiquem estáticos. Por exemplo:

    ![um formulário que contém áreas que devem ser sempre exibidas](images/touch-keyboard-pan2.png)

## Manipulando os eventos Showing e Hiding


Este é um exemplo de como anexar manipuladores de eventos para os eventos [**showing**](https://msdn.microsoft.com/library/windows/apps/br242262) e [**hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) do teclado virtual.

```CSharp
public class MyApplication
{
    public MyApplication()
    {
        // Grab the input pane for the main application window and attach
        // touch keyboard event handlers.
        Windows.Foundation.Application.InputPane.GetForCurrentView().Showing  
            += new EventHandler(_OnInputPaneShowing);
        Windows.Foundation.Application.InputPane.GetForCurrentView().Hiding 
            += new EventHandler(_OnInputPaneHiding);
    }

    private void _OnInputPaneShowing(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        // If the size of this window is going to be too small, the app uses 
        // the Showing event to begin some element removal animations.
        if (eventArgs.OccludedRect.Top < 400)
        {
            _StartElementRemovalAnimations();

            // Don&#39;t use framework scroll- or visibility-related 
            // animations that might conflict with the app&#39;s logic.
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _OnInputPaneHiding(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        if (_ResetToDefaultElements())
        {
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _StartElementRemovalAnimations()
    {
        // This function starts the process of removing elements 
        // and starting the animation.
    }

    private void _ResetToDefaultElements()
    {
        // This function resets the window&#39;s elements to their default state.
    }
}
```

## Artigos relacionados

* [Interações por teclado](keyboard-interactions.md)
* [Acessibilidade do teclado](https://msdn.microsoft.com/library/windows/apps/mt244347)
* [Pares de automação personalizados](https://msdn.microsoft.com/library/windows/apps/mt297667)


**Amostras de arquivo-morto**
* [Entrada: amostra de teclado virtual](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Respondendo ao aparecimento da amostra de teclado virtual](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Amostra de edição de texto XAML](http://go.microsoft.com/fwlink/p/?LinkID=251417)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 







<!--HONumber=Aug16_HO3-->


