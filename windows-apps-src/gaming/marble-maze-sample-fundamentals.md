---
title: Princípios básicos da amostra do Marble Maze
description: Este documento descreve as características fundamentais do projeto Marble Maze; Por exemplo, como ele usa o Visual C++ no ambiente do Windows Runtime, como ele é criado e estruturado e como ele é compilado.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, jogos, amostra, directx, conceitos básicos
ms.localizationpriority: medium
ms.openlocfilehash: 94dd22a6f6b1ace5589104574a695b236c1ebd39
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7991199"
---
# <a name="marble-maze-sample-fundamentals"></a>Princípios básicos de exemplo do Marble Maze




Este tópico descreve as características fundamentais do projeto Marble Maze, por exemplo, como ele usa o Visual C++ no ambiente do Windows Runtime, como ele é criado e estruturado e como é compilado. O tópico também descreve várias das convenções que são usadas no código.

> [!NOTE]
> O exemplo de código que corresponde a este documento pode ser encontrado no [Exemplo do jogo Marble Maze em DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

Consulte a seguir alguns dos pontos-chave que este documento discute para quando você planejar e desenvolver seu jogo UWP (Plataforma Universal do Windows).

-   Use o modelo **Aplicativo DirectX 11 (Universal do Windows)** do Visual C++ no Visual Studio para criar seu jogo UWP DirectX.
-   O Windows Runtime oferece classes e interfaces para que seja possível desenvolver aplicativos UWP com o uso de um método mais moderno orientado a objetos.
-   Use referências de objeto com o símbolo de circunflexo (^) para gerenciar o tempo de vida de variáveis do Windows Runtime, [Microsoft::WRL::ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) para gerenciar o tempo de vida de objetos COM e [std::shared\_ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) ou [std::unique\_ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) para gerenciar o tempo de vida de todos os outros objetos C++ alocados ao heap.
-   Na maioria dos casos, use o tratamento de exceções em vez de códigos de resultado para lidar com erros inesperados.
-   Use [anotações sal](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) junto com ferramentas de análise de código para ajudar a descobrir erros no seu aplicativo.

## <a name="creating-the-visual-studio-project"></a>Criando o projeto do Visual Studio


Se você baixou e extraiu o exemplo, você pode abrir o arquivo **MarbleMaze_VS2017.sln** (na pasta **C++** ) no Visual Studio, e você terá o código na frente de você.

Quando criamos o projeto do Visual Studio para o Marble Maze, começamos com um projeto existente. No entanto, se você ainda não tem um projeto existente que forneça a funcionalidade básica necessária para o seu jogo UWP DirectX, convém criar um projeto com base no modelo **Aplicativo DirectX 11 (Universal do Windows)** do Visual Studio, pois ele fornece um aplicativo 3D de trabalho básico. Para fazer isso, execute estas etapas:

1. No Visual Studio 2017, selecione **arquivo > Novo > projeto...**

2. Na janela **Novo projeto** , na barra lateral esquerda, selecione **instalado > Modelos > Visual C++**.

3. Na lista do meio, selecione o **Aplicativo DirectX 11 (Universal Windows)**. Se você não vir essa opção, você não pode ter os componentes necessários instalados&mdash;consulte [Modificar Visual Studio 2017, adicionando ou removendo cargas de trabalho e componentes](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) para obter informações sobre como instalar componentes adicionais.

4. Dê ao projeto de um **nome**, um **local** para os arquivos sejam armazenados e um **nome de solução**e, em seguida, clique em **Okey**.

![Novo projeto](images/marble-maze-sample-fundamentals-1.png)

Uma definição de projeto importante no modelo de **Aplicativo DirectX 11 (Universal do Windows)** é a opção **/ZW**, que permite que o programa use as extensões de linguagem do Windows Runtime. Essa opção é habilitada por padrão quando você usa o modelo do Visual Studio. Consulte [Definindo opções do compilador](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options) para obter mais informações sobre como definir opções do compilador no Visual Studio.

> **Cuidado**  a opção **/ZW** não é compatível com opções como **/clr**. No caso de **/clr**, isso significa que você não pode direcionar tanto o .NET Framework quanto o Windows Runtime a partir do mesmo projeto Visual C++.

 

Todos os aplicativos UWP que você adquire na Microsoft Store vem na forma de um pacote do aplicativo. Um pacote de aplicativo contém um manifesto de pacote, que contém informações sobre o aplicativo. Por exemplo, você pode especificar os recursos (isto é, o acesso necessário a recursos protegidos do sistema ou dados de usuário) do seu aplicativo. Se você determinar que o seu aplicativo exige determinados recursos, use o manifesto de pacote para declarar os recursos necessários. O manifesto também permite especificar as propriedades do projeto, como rotações de dispositivos com suporte, imagens de blocos e a tela inicial. Você pode editar o manifesto abrindo **Package.appxmanifest** no projeto. Para saber mais sobre pacotes de aplicativos, consulte [Empacotando aplicativos](https://msdn.microsoft.com/library/windows/apps/mt270969).

##  <a name="building-deploying-and-running-the-game"></a>Criando, implantando e executando o jogo

Nos menus suspensos na parte superior do Visual Studio, à esquerda do botão verde Reproduzir, selecione a configuração de implantação. A recomendação é defini-la como **Depurar** direcionando a arquitetura do dispositivo (**x86** para 32 bits, **x64** para 64 bits) e para o **Computador local**. Também é possível testar em um **Computador remoto** ou para um **Dispositivo** conectado via USB. Em seguida, clique no botão verde Reproduzir para compilar e implantar em seu dispositivo.

![Depurar; x64; Computador local](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>Controlando o jogo

Você pode usar toque, o acelerômetro, o controlador do Xbox One ou o mouse para controlar o Marble Maze.

-   Use o teclado direcional no controle para mudar o item de menu ativo.
-   Use toque, o início ou um botão no controlador de, ou o mouse para selecionar um item de menu.
-   Use toque, o acelerômetro, o botão de controle esquerdo ou o mouse para inclinar o labirinto.
-   Use toque, o início ou um botão no controlador de, ou o mouse para fechar menus, como a tabela de pontuações altas.
-   Use o botão Iniciar no controlador ou a tecla P no teclado para pausar ou retomar o jogo.
-   Use o botão Voltar no controlador ou a tecla Página Inicial no teclado para reiniciar o jogo.
-   Quando a tabela de pontuações altas estiver visível, use o botão Voltar no controlador de ou a tecla página inicial no teclado para limpar todas as pontuações.

##  <a name="code-conventions"></a>Convenções de código


O Windows Runtime é uma interface de programação que pode ser usada para criar aplicativos UWP que são executados somente e um aplicativo especial. Esses aplicativos usam funções autorizadas, tipos de dados e dispositivos e são distribuídos da Microsoft Store. No nível mais baixo, o Windows Runtime é formado por uma ABI (Interface Binária de Aplicativo). A ABI é um contrato binário de nível inferior que torna as APIs do Windows Runtime mais acessíveis a várias linguagens de programação, como o JavaScript, as linguagens .NET e o Microsoft Visual C++.

Para chamar APIs do Windows Runtime do JavaScript e do .NET, essas linguagens exigem projeções que são específicas do ambiente de cada linguagem. Quando você chama uma API do Windows Runtime do JavaScript ou do .NET, invoca a projeção, que, em seguida, chama a função ABI subjacente. Embora você possa chamar as funções ABI diretamente em C++, a Microsoft também oferece projeções para C++, pois elas simplificam muito o consumo das APIs do Tempo de Execução do Windows, ao mesmo tempo mantendo o alto desempenho. A Microsoft também disponibiliza extensões de linguagem para o Visual C++ que oferecem suporte especificamente às projeções do Windows Runtime. Muitas dessas extensões de linguagem lembram a sintaxe da linguagem C++/CLI. No entanto, em vez de se voltarem para o CLR (Common Language Runtime), os aplicativos nativos usam essa sintaxe para se voltarem ao Windows Runtime. O modificador de referência de objeto, ou circunflexo (^), é uma parte importante dessa nova sintaxe, pois permite a exclusão automática de objetos de tempo de execução por meio da contagem de referência. Em vez de chamar métodos, como [AddRef](https://msdn.microsoft.com/library/windows/desktop/ms691379) e [Release](https://msdn.microsoft.com/library/windows/desktop/ms682317) para gerenciar o tempo de vida de um objeto do Windows Runtime, o tempo de execução exclui o objeto quando não há outras referências de componentes, por exemplo, quando ele sai do escopo ou define todas as referências como **nullptr**. Outra parte importante do uso do Visual C++ para criar aplicativos UWP é a palavra-chave **ref new**. Use **ref new** em vez de **new** para criar objetos de contagem de referências do Windows Runtime. Para obter mais informações, consulte o artigo [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822).

> [!IMPORTANT]
> Você só precisa usar **^** e **ref new** quando criar objetos ou componentes do Tempo de Execução do Windows. Você pode usar a sintaxe do C++ padrão ao criar o código básico do aplicativo que não usa o Windows Runtime.

O Marble Maze usa **^** junto com **Microsoft::WRL::ComPtr** para gerenciar objetos alocados em pilha e reduzir a perda de memória. Recomendamos que você use ^ para gerenciar o tempo de vida de variáveis de tempo de execução do Windows, **ComPtr** para gerenciar a vida útil das variáveis COM (por exemplo, quando você usa o DirectX) e **std::shared\_ptr** ou **std::unique\_ptr** para gerenciar o tempo de vida de todos os outros objetos C++ alocados em pilha.

 

Para saber mais sobre as extensões de linguagem que estão disponíveis para um aplicativo UWP em C++, consulte [Referência da linguagem Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871).

###  <a name="error-handling"></a>Tratamento de erros

O Marble Maze usa o tratamento de exceções como a principal maneira de lidar com erros inesperados. Embora o código do jogo use tradicionalmente códigos de erro ou registro em log, como valores de **HRESULT** para indicar erros, o tratamento de exceções apresenta duas vantagens principais. Em primeiro lugar, ele pode facilitar a leitura e a manutenção do código. Em termos de código, o tratamento de exceções é a maneira mais eficiente de propagar um erro para uma rotina que pode lidar com esse erro. O uso de códigos de erro geralmente exige que cada função propague os erros de maneira explícita. Uma segunda vantagem é que você também pode configurar o depurador do Visual Studio para ser interrompido quando ocorrer uma exceção, parando imediatamente no local e no contexto do erro. O Windows Runtime também usa amplamente o tratamento de exceções. Assim, ao usar o tratamento de exceções no seu código, você pode consolidar o tratamento de erros em um único modelo.

Recomendamos o uso das seguintes convenções no seu modelo de tratamento de erros:

-   Use exceções para comunicar erros inesperados.
-   Não use exceções para controlar o fluxo do código.
-   Capture somente as exceções que podem ser tratadas e recuperadas com segurança. Caso contrário, não capture a exceção e permita que o aplicativo seja encerrado.
-   Quando você chamar uma rotina DirectX que retorna **HRESULT**, use a função **DX::ThrowIfFailed**. Essa função é definida em [directxhelper. H](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h). **ThrowIfFailed** lança uma exceção se o **HRESULT** fornecido é um código de erro. Por exemplo, **E\_POINTER** faz com que **ThrowIfFailed** gere [Platform:: NullReferenceException](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx).

    Quando você usar **ThrowIfFailed**, coloque a chamada DirectX em uma linha separada para ajudar a melhorar a legibilidade do código, conforme mostrado no exemplo a seguir.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Embora seja recomendável que você evite o uso de **HRESULT** para erros inesperados, é mais importante evitar o uso do tratamento de exceções para controlar o fluxo do código. Portanto, é preferível usar um valor de retorno de **HRESULT** quando necessário para controlar o fluxo do código.

###  <a name="sal-annotations"></a>Anotações SAL

Use anotações SAL junto com ferramentas de análise de código para ajudar a descobrir erros no seu aplicativo.

Usando a linguagem de anotação de código-fonte da Microsoft (SAL), você pode anotar, ou descrever, como uma função usa seus parâmetros. Anotações SAL também descrevem valores de retorno. Anotações SAL trabalham com a ferramenta de Análise de Código C/C++ para descobrir possíveis defeitos no o código fonte C e C++. Erros de codificação comuns relatados pela ferramenta incluem saturações de buffer, memória não inicializada, cancelamentos de referência de ponteiro nulo e vazamentos de memória e recursos.

Considere o método **basicloader:: Loadmesh** , que é declarado em [basicloader. H](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h). Esse método usa `_In_` para especificar esse *nome de arquivo* é um parâmetro de entrada (e, portanto, apenas serão lidos de), `_Out_` para especificar que *vertexBuffer* e *indexBuffer* são parâmetros de saída (e, portanto, apenas serão gravadas em) e `_Out_opt_` para especificar *vertexCount* e *indexCount* são opcionais parâmetros de saída (e podem ser gravados). Como *vertexCount* e *indexCount* são parâmetros de saída opcionais, eles podem ser **nullptr**. A ferramenta de Análise de Código C/C++ examina chamadas para esse método de forma a garantir que os parâmetros que ela transmitir irão atender a esses critérios.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Para realizar a análise de código em seu aplicativo, na barra de menus, escolha **Construir > Executar análise de código na solução**. Para saber mais sobre a análise de código, consulte [Analisando a qualidade do código C/C++ com o uso da análise de código](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

A lista completa de anotações disponíveis está definida em sal.h. Para saber mais, veja [Anotações SAL](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Próximas etapas


Leia [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md) para saber mais sobre como o código do aplicativo Marble Maze é estruturado e como a estrutura de um aplicativo UWP DirectX é diferente daquela de um aplicativo de área de trabalho tradicional.

## <a name="related-topics"></a>Tópicos relacionados


* [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md)
* [Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




