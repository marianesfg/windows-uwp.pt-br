---
author: joannaleecy
title: Configurar o projeto de jogo
description: O primeiro passo na montagem do jogo é configurar um projeto no Microsoft Visual Studio de modo a minimizar a quantidade de trabalho de infraestrutura de código necessária.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, instalação, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9100e80e0b4ac436ae872698e94fe29e5c8cab46
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6848620"
---
# <a name="set-up-the-game-project"></a>Configurar o projeto de jogo

Este tópico aborda como configurar um jogo DirectX UWP simples usando os modelos do Visual Studio. O primeiro passo na montagem do jogo é configurar um projeto no Microsoft Visual Studio de modo a minimizar a quantidade de trabalho de infraestrutura de código necessária. Saiba como poupar tempo usando o modelo certo e configurar o projeto especificamente para o desenvolvimento de jogos.

## <a name="objectives"></a>Objetivos

* Configurar um projeto de jogo Direct3D no Visual Studio usando um modelo
* Entenda o ponto de entrada principal do jogo examinando os arquivos de origem **App**
* Analise o arquivo **package.appxmanifest** do projeto
* Descubra quais ferramentas de desenvolvimento de jogos e suporte estão incluídos no projeto

## <a name="how-to-set-up-the-game-project"></a>Como configurar o projeto de jogo

Se você for iniciante no desenvolvimento de UWP (Plataforma Universal do Windows), recomendamos o uso de modelos do Visual Studio para configurar a estrutura de código básica.

>[!Note]
>Este artigo é parte de uma série de tutoriais com base em um exemplo de jogo. Você pode obter o código mais recente em [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

### <a name="use-directx-template-to-create-a-project"></a>Use o modelo do DirectX para criar um projeto

Um modelo do Visual Studio é uma coleção de configurações e arquivos de código voltados para um tipo específico de aplicativo com base na linguagem e na tecnologia de preferência. No Microsoft Visual Studio2017, você encontrará vários modelos que podem facilitar consideravelmente o desenvolvimento de aplicativos gráficos e jogos. Se você não usar um modelo, será necessário desenvolver grande parte da estrutura básica de exibição e renderização de elementos gráficos por conta própria. Isso pode ser um pouco complexo para um novo desenvolvedor de jogos.

O modelo usado para este tutorial é chamado **DirectX 11 App (Universal Windows)**. 

Etapas para criar um projeto de jogo em DirectX 11 no Visual Studio:
1.  Selecione **Arquivo...** &gt; **Novo**  &gt; **Projeto...**
2.  No painel esquerdo, selecione **Instalado** &gt; **Modelos** &gt; **Visual C++** &gt; **Universal do Windows**
3.  No painel central, selecione **Aplicativo DirectX 11 (Universal Windows)**
4.  Dê um nome ao projeto de jogo e clique em **OK**.

![captura de tela que mostra como selecionar o modelo em directx11 para criar um projeto de jogo](images/simple-dx-game-setup-new-project.png)

Este modelo fornece a estrutura básica para um aplicativo UWP usando DirectX com C++. Pressione F5 para compilá-lo e executá-lo. Confira a tela azul. O modelo cria vários arquivos de código contendo a funcionalidade básica para um aplicativo UWP que usa DirectX com C++.

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>Revise o ponto de entrada principal do aplicativo entendendo o IFrameworkView

A classe **App** é herdada da classe **IFrameworkView**.

### <a name="inspect-apph"></a>Inspecione **App.h**.

Vamos analisar rapidamente os 5 métodos em **App.h** &mdash; [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) e [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) ao implementar a interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) que define um provedor de visualização. Esses métodos são executados pelo singleton do aplicativo criado quando o jogo é iniciado e carregam todos os recursos de seu aplicativo, além de conectar os manipuladores de eventos adequados.

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
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
        ...
    };
```

### <a name="inspect-appcpp"></a>Inspecione **App.cpp**.

Aqui está o método **main** no arquivo-fonte **App.cpp**:

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Nesse método, ele cria uma instância do provedor de exibição Direct3D da fábrica de provedores de exibição (**Direct3DApplicationSource**, definido em **App.h**) e a transmite para o singleton do aplicativo chamando ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). Isso significa que o ponto inicial de seu jogo fica no corpo da implementação do método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) e, neste caso, é o **App::Run**. 

Role a tela para localizar o método **App::Run** em **App.cpp**. Aqui está o código:

```cpp
//This method is called after the window becomes active.
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

O que esse método faz: se a janela do jogo não estiver fechada, ocorrerá o envio de todos os eventos, atualização do temporizador e depois renderização/apresentação dos resultados do pipeline de elementos gráficos. Falaremos sobre isso mais detalhadamente em [Definir a estrutura do aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md), [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md) e [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md). A esta altura, você deve ter uma noção da estrutura de código básica de um jogo UWP DirectX.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Examine e atualize o arquivo package.appxmanifest


Os arquivos de códigos não são os únicos elementos do modelo. O arquivo **Package.appxmanifest** contém metadados sobre o projeto que serão usados para empacotar e iniciar o jogo e enviá-lo à Microsoft Store. Ele também contém informações importante usadas pelo sistema do jogador para fornecer acesso aos recursos do sistema de que o jogo precisa para ser executado.

Inicie o **manifest designer** clicando duas vezes no arquivo **Package.appxmanifest** no **Gerenciador de Soluções**.

![captura de tela do editor de manifesto package.appx.](images/simple-dx-game-setup-app-manifest.png)

Para saber mais sobre o arquivo **package.appxmanifest** e empacotamento, veja [Designer de Manifesto](https://msdn.microsoft.com/library/windows/apps/br230259.aspx). Por enquanto, examine a guia **Recursos** e as opções fornecidas.

![captura de tela com as funcionalidades padrão de um aplicativo direct3d.](images/simple-dx-game-setup-capabilities.png)

Se você não selecionar as funcionalidades usadas pelo jogo, como o acesso à **Internet** para o quadro global de melhores pontuações, não será possível acessar os recursos correspondentes. Ao criar um novo jogo, selecione as funcionalidades necessárias para que ele seja executado!

Agora vejamos o restante dos arquivos que acompanham o modelo **Aplicativo DirectX 11 (Universal Windows)**.

## <a name="review-the-included-libraries-and-headers"></a>Revise as bibliotecas e os calendários incluídos

Nós ainda não examinamos alguns arquivos. Esses arquivos fornecem ferramentas adicionais e suporte comuns aos cenários de desenvolvimento do jogo em Direct3D. Para obter a lista completa dos arquivos que vem com o projeto de jogo DirectX básico, consulte [Modelos de projeto de jogo DirectX](user-interface.md#template-structure).

| Arquivo de origem do modelo         | Pasta de arquivos            | Descrição |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | Comum                 | Define um objeto de classe que controla todos os [recursos de dispositivo](tutorial--assembling-the-rendering-pipeline.md#resource) do DirectX. Também inclui uma interface para o aplicativo que possui DeviceResources a ser notificado quando o dispositivo é perdido ou criado.                                                |
| DirectXHelper.h              | Comum                 | Implementa métodos incluindo **DX::ThrowIfFailed**, **ReadDataAsync** e **ConvertDipsToPixels. **DX::ThrowIfFailed** converte os valores HRESULT de erro gerados por APIs do DirectX Win32 em exceções do Windows Runtime. Use esse método para inserir um ponto de interrupção para depurar erros do DirectX. Para obter mais informações, consulte [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed). **ReadDataAsync** lê a partir de um arquivo binário de forma assíncrona. **ConvertDipsToPixels** converte um tamanho em pixels independentes de dispositivo (DIPs) para um tamanho em pixels físicos. |
| StepTimer.h                  | Comum                 | Define um temporizador de alta resolução, útil para aplicativos de renderização de jogos ou interativos.   |
| Sample3DSceneRenderer.h/.cpp | Conteúdo                | Define um objeto de classe para instanciar um pipeline de renderização básica. Cria uma implementação básica de renderizador, que conecta uma cadeia de troca Direct3D e adaptador de gráficos ao UWP em DirectX.   |
| SampleFPSTextRenderer.h/.cpp | Conteúdo                | Define um objeto de classe para renderizar o valor atual de Fps (quadros por segundo) no canto inferior direito da tela usando Direct2D e DirectWrite.  |
| SamplePixelShader.hlsl       | Conteúdo                | Contém o código HLSL (linguagem de sombreador de alto nível) para um sombreador de pixels bastante básico.                                            |
| SampleVertexShader.hlsl      | Conteúdo                | Contém o código HLSL para um sombreador de vértices bem básico.                                           |
| ShaderStructures.h           | Conteúdo                | Contém estruturas de sombreador que podem ser usadas para enviar dados de matrizes MVP e por vértice para o sombreador de vértice.  |
| pch.h/.cpp                   | Principal                   | Contém todas as APIs do sistema do Windows usadas por um aplicativo Direct3D, inclusive APIs do DirectX 11.| 

### <a name="next-steps"></a>Próximas etapas

Neste ponto, você já aprendeu como criar um projeto de jogo DirectX UWP usando o modelo **DirectX 11 App (Universal Windows)** e está familiarizado com alguns componentes e arquivos fornecidos por esse projeto.

A próxima seção é [Definindo a estrutura UWP do jogo](tutorial--building-the-games-uwp-app-framework.md). Examinaremos como esse jogo usa e amplia muitos dos conceitos e componentes fornecidos pelo modelo.

 

 




