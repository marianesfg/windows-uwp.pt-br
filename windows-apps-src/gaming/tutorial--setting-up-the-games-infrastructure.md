---
title: Configurar o projeto de jogo
description: O primeiro passo na montagem do jogo é configurar um projeto no Microsoft Visual Studio de modo a minimizar a quantidade de trabalho de infraestrutura de código necessária.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
---

# Configurar o projeto de jogo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, veja o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O primeiro passo na montagem do jogo é configurar um projeto no Microsoft Visual Studio de modo a minimizar a quantidade de trabalho de infraestrutura de código necessária. Você pode poupar muito tempo e problemas usando os modelos certos e configurando o projeto especificamente para o desenvolvimento de jogos. Guiaremos você em meio à instalação e à configuração de um projeto de jogo simples.

## Objetivo


-   Saber como configurar um projeto de jogo Direct3D no Visual Studio.

## Configurando o projeto de jogo


Você pode criar um jogo do zero, com apenas um editor de texto, alguns exemplos e uma cabeça cheia de ideias. Mas essa provavelmente não é a melhor maneira de dedicar seu tempo. Se você for estiver começando no desenvolvimento de UWP (Plataforma Universal do Windows), deixe o Visual Studio ajudar você. Consulte o que fazer para iniciar seu projeto de maneira incrível.

## 1. Escolha o modelo correto.


Um modelo do Visual Studio é uma coleção de configurações e arquivos de código voltados para um tipo específico de aplicativo com base na linguagem e na tecnologia de preferência. No Microsoft Visual Studio 2015, você encontrará vários modelos que podem facilitar consideravelmente o desenvolvimento de aplicativos gráficos e jogos. Se você não usar um modelo, será necessário desenvolver grande parte da estrutura básica de exibição e renderização de elementos gráficos por conta própria. Isso pode ser um pouco complexo para um novo desenvolvedor de jogos.

O modelo certo para este tutorial é o chamado Aplicativo DirectX 11 (Universal Windows). No Visual Studio 2015, clique em **Arquivo** &gt; **Novo Projeto**e então:

1.  Em **Modelos**, selecione **Visual C++**, **Windows**, **Universal**.
2.  No painel central, selecione **Aplicativo DirectX 11 (Universal Windows)**.
3.  Dê um nome ao projeto de jogo e clique em **OK**.

![selecionando o modelo de aplicativo direct3d](images/simple-dx-game-vs-new-proj.png)

Este modelo fornece a estrutura básica para um aplicativo UWP usando DirectX com C++. Vá em frente! Crie e execute o jogo usando F5. Confira a tela azul. Dedique um tempo para revisar o código fornecido pelo modelo. O modelo cria vários arquivos de código contendo a funcionalidade básica para um aplicativo UWP que usa DirectX com C++. Falaremos mais sobre os outros arquivos de código na [etapa 3](#3-review-the-included-libraries-and-headers). Agora vamos examinar rapidamente **App.h**.

```cpp
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        // Application lifecycle event handlers.
        void OnActivated(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView, Windows::ApplicationModel::Activation::IActivatedEventArgs^ args);
        void OnSuspending(Platform::Object^ sender, Windows::ApplicationModel::SuspendingEventArgs^ args);
        void OnResuming(Platform::Object^ sender, Platform::Object^ args);

        // Window event handlers.
        void OnWindowSizeChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::WindowSizeChangedEventArgs^ args);
        void OnVisibilityChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::VisibilityChangedEventArgs^ args);
        void OnWindowClosed(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::CoreWindowEventArgs^ args);

        // DisplayInformation event handlers.
        void OnDpiChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnOrientationChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnDisplayContentsInvalidated(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);

    private:
        std::shared_ptr<DX::DeviceResources> m_deviceResources;
        std::unique_ptr<MyAwesomeGameMain> m_main;
        bool m_windowClosed;
        bool m_windowVisible;
    };
```

Crie estes cinco métodos, [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) e [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523), ao implementar a interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) que define um provedor de visualização. Esses métodos são executados pelo singleton do aplicativo criado quando o jogo é iniciado e carregam todos os recursos de seu aplicativo, além de conectar os manipuladores de eventos adequados.

Seu método **main** no arquivo-fonte **App.cpp**. Ele é semelhante ao seguinte:

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Por enquanto, ele cria uma instância do provedor de exibição Direct3D da fábrica de provedores de exibição (**Direct3DApplicationSource**, definido em **App.h**) e a transmite para o singleton do aplicativo para execução ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). Isso significa que o ponto inicial de seu jogo fica no corpo da implementação do método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505); neste caso, **App::Run**. Aqui está o código:

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Se a janela do jogo não estiver fechada, ocorrerá o envio de todos os eventos, atualização do temporizador e renderização/apresentação dos resultados do pipeline de elementos gráficos. Abordamos isso com mais detalhes em [Definindo a estrutura UWP do jogo](tutorial--building-the-games-metro-style-app-framework.md) e [Montando o pipeline de renderização](tutorial--assembling-the-rendering-pipeline.md). A esta altura, você deve ter uma noção da estrutura de código básica de um jogo UWP DirectX.

## 2. Examine e atualize o arquivo package.appxmanifest


Os arquivos de códigos não são os únicos elementos do modelo. O arquivo **package.appxmanifest** contém metadados sobre o projeto que serão usados para empacotar e iniciar o jogo e enviá-lo à Windows Store. Ele também contém informações importante usadas pelo sistema do jogador para fornecer acesso aos recursos do sistema de que o jogo precisa para ser executado.

Inicie o **Manifest Designer** clicando duas vezes no arquivo **package.appxmanifest** no **Gerenciador de Soluções**. Você verá esta exibição:

![o editor de manifesto package.appx.](images/simple-dx-game-vs-app-manifest.png)

Para saber mais sobre o arquivo **package.appxmanifest** e empacotamento, veja [Designer de Manifesto](https://msdn.microsoft.com/library/windows/apps/br230259.aspx). Por enquanto, examine a guia **Recursos** e as opções fornecidas.

![as funcionalidades padrão de um aplicativo direct3d.](images/simple-dx-game-vs-capabilities.png)

Se você não selecionar as funcionalidades usadas pelo jogo, como o acesso à **Internet** para o quadro global de melhores pontuações, não será possível acessar os recursos correspondentes. Ao criar um novo jogo, selecione as funcionalidades necessárias para que ele seja executado!

Agora vejamos o restante dos arquivos que acompanham o modelo **Aplicativo DirectX 11 (Universal Windows)**.

## 3. Revise as bibliotecas e os calendários incluídos


Nós ainda não examinamos alguns arquivos. Esses arquivos fornecem ferramentas adicionais e suporte comuns aos cenários de desenvolvimento do jogo em Direct3D.

| Arquivo de origem do modelo         | Descrição                                                                                                                                                                                                            |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StepTimer.h                  | Define um temporizador de alta resolução, útil para aplicativos de renderização de jogos ou interativos.                                                                                                                                       |
| Sample3DSceneRenderer.h/.cpp | Define uma implementação básica de renderizador, que conecta uma cadeia de troca Direct3D e adaptador de gráficos ao UWP em DirectX.                                                                                            |
| DirectXHelper.h              | Implementa um método único, **DX::ThrowIfFailed**, que converte os valores HRESULT de erro gerados por APIs do DirectX em exceções do Windows Runtime. Use esse método para inserir um ponto de interrupção para depurar erros do DirectX. |
| pch.h/.cpp                   | Contém todas as APIs do sistema do Windows usadas por um aplicativo Direct3D, inclusive APIs do DirectX 11.                                                                                                           |
| SamplePixelShader.hlsl       | Contém o código HLSL (linguagem de sombreador de alto nível) para um sombreador de pixels bastante básico.                                                                                                                                     |
| SampleVertexShader.hlsl      | Contém o código HLSL para um sombreador de vértices bem básico.                                                                                                                                    |

 

### Próximas etapas

Neste ponto, você pode criar um projeto de jogo DirectX com UWP e identificar os componentes e arquivos fornecidos pelo modelo Aplicativo DirectX 11 (Universal Windows).

No próximo tutorial, [Definindo a estrutura UWP do jogo](tutorial--building-the-games-metro-style-app-framework.md), trabalhamos com um jogo completo e examinamos como ele usa e estende muitos dos conceitos e componentes fornecidos pelo modelo.

 

 






<!--HONumber=Mar16_HO1-->


