---
title: Fazer a portabilidade do loop do jogo
description: Mostra como implementar uma janela para um jogo da Plataforma Universal do Windows (UWP) e como ativar o loop do jogo, inclusive como criar uma IFrameworkView para controlar uma CoreWindow em tela inteira.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, fazendo a portabilidade, loop do jogo, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 8b0cf6352d400371b54a54d71176c4d8e1dc457d
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469105"
---
# <a name="port-the-game-loop"></a>Fazer a portabilidade do loop do jogo



**Resumo**

-   [Parte 1: inicializar o Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Parte 2: converter a estrutura de renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Parte 3: fazer a portabilidade do loop do jogo


Mostra como implementar uma janela para um jogo da Plataforma Universal do Windows (UWP) e como ativar o loop do jogo, inclusive como criar uma [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) para controlar uma [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) em tela inteira. Parte 3 do guia passo a passo de [portabilidade de um aplicativo simples em Direct3D 9 para o DirectX 11 e a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="create-a-window"></a>Criar uma janela


Para configurar uma janela da área de trabalho com um visor do Direct3D 9, era necessário implementar a estrutura original de janelas para aplicativos de área de trabalho. Tínhamos que criar um HWND, definir o tamanho da janela, fornecer um retorno de chamada de processamento de janela, torná-la visível, entre outras coisas.

O ambiente UWP tem um sistema muito mais simples. Em vez de configurar uma janela tradicional, um jogo da Microsoft Store que usa DirectX implementa [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Essa interface existe para que aplicativos e jogos em DirectX sejam executados diretamente em uma [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), dentro do contêiner de aplicativo.

> **Observação**  Windows oferece ponteiros gerenciados para recursos como o objeto de aplicativo de origem e a [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Consulte [**manipular o operador de objeto (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

Sua classe "principal" precisa ser herdada de [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) e implementar os cinco métodos de **IFrameworkView**: [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) e [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523). Além de criar **IFrameworkView**, que é (essencialmente) onde ficará o jogo, você precisa implementar uma classe de fábrica que crie uma instância da **IFrameworkView**. O jogo ainda possui um executável com um método chamado **main()**, mas tudo que ele pode fazer é usar a fábrica para criar a instância de **IFrameworkView**.

Função principal

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Fábrica IFrameworkView

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>Fazer a portabilidade do loop do jogo


Consultemos o loop do jogo na implementação do Direct3D 9. Este código existe na função principal do aplicativo. Cada iteração desse loop processa uma mensagem na janela ou renderiza um quadro.

Loop em um jogo de área de trabalho com Direct3D 9

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

O loop do jogo é semelhante (porém, mais simples) na versão UWP:

O loop do jogo vai no método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) (em vez de **main()**) porque o jogo funciona na classe [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478).

Em vez de implementar uma estrutura de manipulação de mensagens e chamar [**PeekMessage**](https://msdn.microsoft.com/library/windows/desktop/ms644943), podemos chamar o método [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) interno ao [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) da janela do aplicativo. O jogo não precisa ramificar e manipular mensagens; basta chamar **ProcessEvents** e continuar.

Loop em um jogo da Microsoft Store com Direct3D 11

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Agora temos um aplicativo UWP que configura a mesma infraestrutura de elementos gráficos básica e renderiza o mesmo cubo colorido, como o exemplo em DirectX 9.

## <a name="where-do-i-go-from-here"></a>Para onde ir em seguida?


Adicione as [perguntas frequentes sobre portabilidade do DirectX 11](directx-porting-faq.md) aos favoritos.

Os modelos UWP DirectX incluem uma infraestrutura de dispositivo Direct3D robusta, pronta para uso com o jogo. Consulte [Criar um projeto de jogo em DirectX usando um modelo](user-interface.md) para obter instruções sobre a escolha do modelo certo.

Visite os seguintes artigos aprofundados sobre desenvolvimento de jogos da Microsoft Store:

-   [Guia passo a passo: um jogo simples UWP com DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Áudio para jogos](working-with-audio-in-your-directx-game.md)
-   [Controles move-look para jogos](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




