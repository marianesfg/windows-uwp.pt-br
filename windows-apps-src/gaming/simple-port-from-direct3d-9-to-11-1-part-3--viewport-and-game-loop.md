---
title: Fazer a portabilidade do loop do jogo
description: Mostra como implementar uma janela para um jogo da Plataforma Universal do Windows (UWP) e como ativar o loop do jogo, inclusive como criar uma IFrameworkView para controlar uma CoreWindow em tela inteira.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, fazendo a portabilidade, loop do jogo, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: bd6a17b5e1684fbee21965158295dba123737bd6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367908"
---
# <a name="port-the-game-loop"></a>Fazer a portabilidade do loop do jogo



**Resumo**

-   [Parte 1: Inicializar o Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Parte 2: Converter o framework de renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Parte 3: Fazer a portabilidade do loop do jogo


Mostra como implementar uma janela para um jogo da Plataforma Universal do Windows (UWP) e como ativar o loop do jogo, inclusive como criar uma [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) para controlar uma [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) em tela inteira. Parte 3 do guia passo a passo de [portabilidade de um aplicativo simples em Direct3D 9 para o DirectX 11 e a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="create-a-window"></a>Criar uma janela


Para configurar uma janela da área de trabalho com um visor do Direct3D 9, era necessário implementar a estrutura original de janelas para aplicativos de área de trabalho. Tínhamos que criar um HWND, definir o tamanho da janela, fornecer um retorno de chamada de processamento de janela, torná-la visível, entre outras coisas.

O ambiente UWP tem um sistema muito mais simples. Em vez de configurar uma janela tradicional, um jogo da Microsoft Store que usa DirectX implementa [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Essa interface existe para que aplicativos e jogos em DirectX sejam executados diretamente em uma [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), dentro do contêiner de aplicativo.

> **Observação**    Windows fornece ponteiros gerenciados a recursos como o objeto de aplicativo de origem e o [ **CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). Consulte [**operador Handle to Object (^)** ]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

Sua classe de "main" deve herdar de [ **IFrameworkView** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) e implementar os cinco **IFrameworkView** métodos: [**Inicializar**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [ **SetWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [ **carga**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load), [ **executar** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run), e [ **Cancelar inicialização**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize). Além de criar **IFrameworkView**, que é (essencialmente) onde ficará o jogo, você precisa implementar uma classe de fábrica que crie uma instância da **IFrameworkView**. O jogo ainda possui um executável com um método chamado **main()** , mas tudo que ele pode fazer é usar a fábrica para criar a instância de **IFrameworkView**.

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

O loop do jogo vai no método [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) (em vez de **main()** ) porque o jogo funciona na classe [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView).

Em vez de implementar uma estrutura de manipulação de mensagens e chamar [**PeekMessage**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-peekmessagea), podemos chamar o método [**ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) interno ao [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) da janela do aplicativo. O jogo não precisa ramificar e manipular mensagens; basta chamar **ProcessEvents** e continuar.

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

## <a name="where-do-i-go-from-here"></a>Para onde vou daqui?


Adicione as [perguntas frequentes sobre portabilidade do DirectX 11](directx-porting-faq.md) aos favoritos.

Os modelos UWP DirectX incluem uma infraestrutura de dispositivo Direct3D robusta, pronta para uso com o jogo. Consulte [Criar um projeto de jogo em DirectX usando um modelo](user-interface.md) para obter instruções sobre a escolha do modelo certo.

Visite os seguintes artigos detalhados de desenvolvimento de jogos do Microsoft Store:

-   [Passo a passo: um simple jogo UWP com o DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Áudio para jogos](working-with-audio-in-your-directx-game.md)
-   [Controles de mover a consulta para jogos](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




