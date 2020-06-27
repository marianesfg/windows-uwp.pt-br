---
title: Configurar o projeto de jogo
description: A primeira etapa no desenvolvimento do jogo é configurar um projeto no Microsoft Visual Studio. Depois de configurar um projeto especificamente para desenvolvimento de jogos, você poderá reutilizá-lo posteriormente como um tipo de modelo.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, jogos, instalação, directx
ms.localizationpriority: medium
ms.openlocfilehash: c0c8d82752d9b73a3e3e7ffcec3ced1515564072
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409585"
---
# <a name="set-up-the-game-project"></a>Configurar o projeto de jogo

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

A primeira etapa no desenvolvimento do jogo é criar um projeto no Microsoft Visual Studio. Depois de configurar um projeto especificamente para desenvolvimento de jogos, você poderá reutilizá-lo posteriormente como um tipo de modelo.

## <a name="objectives"></a>Objetivos

* Crie um novo projeto no Visual Studio usando um modelo de projeto.
* Entenda o ponto de entrada e a inicialização do jogo examinando o arquivo de origem da classe do **aplicativo** .
* Examine o loop do jogo.
* Examine o arquivo **Package. appxmanifest** do projeto.

## <a name="create-a-new-project-in-visual-studio"></a>Criar um novo projeto no Visual Studio

> [!NOTE]
> Para saber mais sobre como configurar o Visual Studio para desenvolvimento em C++/WinRT, incluindo instalação e uso da VSIX (Extensão do Visual Studio) para C++/WinRT e o pacote NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](/windows/uwp/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Primeiro, instale (ou atualize) a versão mais recente da extensão do/WinRT do Visual Studio (VSIX) do C++. consulte a observação acima. Em seguida, no Visual Studio, crie um novo projeto com base no modelo de projeto do **aplicativo principal (C++/WinRT)** . Direcione a versão mais recente em disponibilidade geral (ou seja, que não esteja em versão prévia) do SDK do Windows.

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>Examine a classe de **aplicativo** para entender **IFrameworkViewSource** e **IFrameworkView**

Em seu projeto de aplicativo principal, abra o arquivo de código-fonte `App.cpp` . Há a implementação da classe **app** , que representa o aplicativo e seu ciclo de vida. Nesse caso, é claro que sabemos que o aplicativo é um jogo. Mas vamos nos referir a ele como um *aplicativo* para falar mais geralmente sobre como um aplicativo plataforma universal do Windows (UWP) é inicializado.

### <a name="the-wwinmain-function"></a>A função wWinMain

A função **wWinMain** é o ponto de entrada para o aplicativo. Aqui está a aparência de **wWinMain** (de `App.cpp` ).

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

Fazemos uma instância da classe **app** (essa é a única instância do **aplicativo** que é criada) e a passamos para o método estático [**CoreApplication. Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) . Observe que **CoreApplication. Run** espera uma interface [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Portanto, a classe de **aplicativo** precisa implementar essa interface.

As duas próximas seções neste tópico descrevem as interfaces [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) e [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Essas interfaces (bem como **CoreApplication. Run**) representam uma maneira para seu aplicativo fornecer o Windows com um *provedor de exibição*. O Windows usa esse provedor de exibição para conectar seu aplicativo com o Shell do Windows para que você possa manipular eventos de ciclo de vida do aplicativo.

### <a name="the-iframeworkviewsource-interface"></a>A interface IFrameworkViewSource

A classe de **aplicativo** realmente implementa [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource), como você pode ver na lista abaixo.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

Um objeto que implementa **IFrameworkViewSource** é um objeto de *fábrica do provedor de exibição* . O trabalho desse objeto é fabricar e retornar um objeto *View-Provider* .

**IFrameworkViewSource** tem o método único [**IFrameworkViewSource:: CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview). O Windows chama essa função no objeto que você passa para **CoreApplication. Run**. Como você pode ver acima, a implementação **App:: CreateView** desse método retorna `*this` . Em outras palavras, o objeto de **aplicativo** retorna a si mesmo. Como **IFrameworkViewSource:: CreateView** tem um tipo de valor de retorno de [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource), segue que a classe **app** também precisa implementar *essa* interface. E você pode ver que ele faz na listagem acima.

### <a name="the-iframeworkview-interface"></a>A interface IFrameworkView

Um objeto que implementa [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) é um objeto *View-Provider* . E agora fornecemos o Windows com esse provedor de exibição. É o mesmo objeto de **aplicativo** que criamos no **wWinMain**. Portanto, a classe de **aplicativo** serve como *fábrica de provedores de exibição* e *provedor de exibição*.

Agora, o Windows pode chamar as implementações da classe de **aplicativo** dos métodos de **IFrameworkView**. Nas implementações desses métodos, seu aplicativo tem a chance de executar tarefas como inicialização, começar a carregar os recursos necessários, conectar os manipuladores de eventos apropriados e receber o [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) que seu aplicativo usará para exibir sua saída.

Suas implementações dos métodos de **IFrameworkView** são chamadas na ordem mostrada abaixo.

- [**Inicializar**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**Carregar**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- O evento [**CoreApplicationView:: Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) é gerado. Portanto, se você tiver (opcionalmente) registrado para lidar com esse evento, o manipulador **OnActivated** será chamado no momento.
- [**Executar**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**Cancelar**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

E aqui está o esqueleto da classe **app** (no `App.cpp` ), mostrando as assinaturas desses métodos.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::CoreCoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::CoreCoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

Essa era apenas uma introdução ao **IFrameworkView**. Entramos em muito mais detalhes sobre esses métodos e como implementá-los, em [definir a estrutura do aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md).

### <a name="tidy-up-the-project"></a>Organizar o projeto

O projeto de aplicativo principal que você criou no modelo de projeto contém a funcionalidade que devemos organizar neste ponto. Depois disso, podemos usar o projeto para recriar o jogo da Galeria de captura (**Simple3DGameDX**). Faça as seguintes alterações na classe **app** no `App.cpp` .

- Exclua seus membros de dados.
- Excluir **OnPointerPressed**, **OnPointerMoved**e **addvisual**
- Exclua o código de **SetWindow**.

O projeto será criado e executado, mas exibirá apenas uma cor sólida na área do cliente.

## <a name="the-game-loop"></a>O loop do jogo

Para ter uma ideia da aparência de um loop de jogo, examine o código-fonte do jogo de exemplo [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que você baixou.

A classe de **aplicativo** tem um membro de dados, chamado *m_main*, do tipo **GameMain**. E esse membro é usado no **aplicativo:: execute** desta forma.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Você pode encontrar **GameMain:: Run** in `GameMain.cpp` . É o principal loop do jogo, e aqui está uma estrutura de tópicos muito aproximada que mostra os recursos mais importantes.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

E aqui está uma breve descrição do que esse loop de jogo principal faz.

Se a janela do jogo não estiver fechada, despache todos os eventos, atualize o temporizador e, em seguida, processe e apresente os resultados do pipeline de gráficos. Há muito mais a dizer sobre essas preocupações, e faremos isso nos tópicos [definir a estrutura do aplicativo UWP do jogo, o](tutorial--building-the-games-uwp-app-framework.md)Framework de [renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md)e renderização da [estrutura II: renderização do jogo](tutorial-game-rendering.md). Mas essa é a estrutura de código básica de um jogo do UWP DirectX.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Examine e atualize o arquivo package.appxmanifest

O arquivo **Package. appxmanifest** contém metadados sobre um projeto UWP. Esses metadados são usados para empacotar e iniciar o jogo e para envio para o Microsoft Store. O arquivo também contém informações importantes que o sistema do jogador usa para fornecer acesso aos recursos do sistema que o jogo precisa executar.

Inicie o **Designer de manifesto** clicando duas vezes no arquivo **Package. appxmanifest** em **Gerenciador de soluções**.

![captura de tela do editor de manifesto package.appx.](images/simple-dx-game-setup-app-manifest.png)

Para saber mais sobre o arquivo **package.appxmanifest** e empacotamento, veja [Designer de Manifesto](/previous-versions/br230259(v=vs.140)). Por enquanto, dê uma olhada na guia **recursos** e examine as opções fornecidas.

![captura de tela com as funcionalidades padrão de um aplicativo direct3d.](images/simple-dx-game-setup-capabilities.png)

Se você não selecionar os recursos que seu jogo usa, como o acesso à **Internet** para o quadro de alta pontuação global, não será possível acessar os recursos e recursos correspondentes. Ao criar um novo jogo, certifique-se de selecionar os recursos necessários para as APIs que seu jogo chama.

Agora, vamos examinar o restante dos arquivos que acompanham o modelo de **aplicativo DirectX 11 (universal do Windows)** .

## <a name="review-other-important-libraries-and-source-code-files"></a>Examinar outras bibliotecas importantes e arquivos de código-fonte

Se você pretende criar um tipo de modelo de projeto de jogo para si mesmo, para que possa reutilizá-lo como um ponto de partida para projetos futuros, convém copiar `GameMain.h` e `GameMain.cpp` sair do projeto [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que você baixou e adicioná-los ao seu novo projeto de aplicativo principal. Estude esses arquivos, saiba o que eles fazem e remova qualquer coisa específica ao **Simple3DGameDX**. Além disso, comente qualquer coisa que dependa do código que você ainda não copiou. Apenas por meio de um exemplo, `GameMain.h` depende de `GameRenderer.h` . Você poderá remover o comentário ao copiar mais arquivos de **Simple3DGameDX**.

Aqui está uma breve pesquisa de alguns dos arquivos no **Simple3DGameDX** que você achará útil incluir em seu modelo, se você estiver criando um. De qualquer forma, eles são igualmente importantes para entender como o próprio **Simple3DGameDX** funciona.

|Arquivo de origem|Pasta do arquivo|Descrição|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources.h/.cpp|Utilidades|Define a classe **DeviceResources** , que controla todos os [recursos do dispositivo](tutorial--assembling-the-rendering-pipeline.md#resource)DirectX. Também define a interface **IDeviceNotify** , usada para notificar seu aplicativo de que o dispositivo adaptador gráfico foi perdido ou recriado.|
|DirectXSample.h|Utilidades|Implementa funções auxiliares, como **ConvertDipsToPixels**. **ConvertDipsToPixels** converte um tamanho em pixels independentes de dispositivo (DIPs) para um tamanho em pixels físicos.|
|GameTimer. h/. cpp|Utilidades|Define um temporizador de alta resolução, útil para aplicativos de renderização de jogos ou interativos.|
|GameRenderer.h/.cpp|Renderização|Define a classe **GameRenderer** , que implementa um pipeline de renderização básico.|
|GameHud. h/. cpp|Renderização|Define uma classe para renderizar uma exibição de cabeçotes (HUD) para o jogo, usando Direct2D e DirectWrite.|
|VertexShader. HLSL e VertexShaderFlat. HLSL|Sombreadores|Contém o código de HLSL (linguagem de sombreamento de alto nível) para sombreadores de vértices básicos.|
|PixelShader. HLSL e PixelShaderFlat. HLSL|Sombreadores|Contém o código de HLSL (linguagem de sombreamento de alto nível) para sombreadores de pixel básicos.|
|ConstantBuffers. hlsli|Sombreadores|Contém definições de estrutura de dados para buffers de constantes e estruturas de sombreamento usadas para passar matrizes de modelo-exibição-projeção (MVP) e dados por vértice para o sombreador de vértice.|
|pch.h/.cpp|N/D|Contém C++/WinRT, Windows e DirectX comuns.| 

### <a name="next-steps"></a>Próximas etapas

Neste ponto, mostramos como criar um novo projeto UWP para um jogo do DirectX, examinamos algumas das partes e começamos a pensar em como transformar esse projeto em um tipo de modelo reutilizável para jogos. Também vimos algumas das partes importantes do jogo de exemplo **Simple3DGameDX** .

A próxima seção é [definir a estrutura do aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md). Lá, veremos mais detalhadamente como o **Simple3DGameDX** funciona.
