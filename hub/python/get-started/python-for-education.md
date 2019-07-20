---
title: Introdução ao uso do Python no Windows para iniciantes
description: Um guia para ajudá-lo a começar se sua marca estiver totalmente nova para usar o Python no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: Python, Windows 10, Microsoft, Learning Python, Python no Windows para iniciantes, instalar o Python com a Microsoft Store, Python com vs Code, Pygame no Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9bc9a75b11e90b53f46e8a2b267921078b27ad36
ms.sourcegitcommit: afb5157ec4bcb6588ac4cf74352688b30ed32257
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68349351"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Comece a usar o Python no Windows para iniciantes

Veja a seguir um guia passo a passo para iniciantes interessados em aprender sobre Python usando o Windows 10.

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Para iniciantes que são novos no Python, recomendamos que você [Instale o Python da Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). Isso é especialmente importante se você estiver em um ambiente educacional ou em uma parte de uma organização que restringe permissões ou acesso administrativo em seu computador. Usando o Microsoft Store para instalar o Python, ele será colocado no caminho do seu sistema operacional para o usuário atual (evitando a necessidade de acesso de administrador).

> [!IMPORTANT]
> Se você estiver usando o Python no Windows para desenvolvimento para a **Web**, recomendamos uma configuração diferente para seu ambiente de desenvolvimento. Em vez de instalar diretamente no Windows, recomendamos instalar e usar o Python por meio do subsistema do Windows para Linux. Para obter ajuda, consulte: Comece [a usar o Python para desenvolvimento para a Web no Windows](./python-for-web.md). Se você estiver interessado em automatizar tarefas comuns em seu sistema operacional, consulte nosso guia: Comece [a usar o Python no Windows para scripts e automação](./python-for-scripting.md). <br>Para alguns cenários avançados, talvez você queira baixar uma versão específica do Python diretamente do [Python.org](https://www.python.org/downloads/windows/) ou considerar a instalação de uma [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython etc. Só recomendamos isso se você for um programador de Python mais avançado com um motivo específico para escolher uma implementação alternativa.

## <a name="install-python"></a>Instalar o Python

Para instalar o Python usando o Microsoft Store:

1. Vá para o menu **Iniciar** (ícone inferior esquerdo do Windows), digite "Microsoft Store", selecione o link para abrir o repositório.

2. Quando o repositório estiver aberto, selecione **Pesquisar** no menu superior direito e insira "Python". Abra "Python 3,7" nos resultados em aplicativos. Selecione **obter**.

3. Depois que o Python concluir o processo de download e instalação, abra o Windows PowerShell usando o menu **Iniciar** (ícone inferior esquerdo do Windows). Quando o PowerShell estiver aberto, `Python --version` digite para confirmar se o Python3 foi instalado em seu computador.

4. O Microsoft Store instalação do Python inclui **Pip**, o Gerenciador de pacotes padrão. Pip permite que você instale e gerencie pacotes adicionais que não fazem parte da biblioteca padrão do Python. Para confirmar que você também tem o Pip disponível para instalar e gerenciar pacotes, `pip --version`digite.

## <a name="install-visual-studio-code"></a>Instalar Visual Studio Code

Usando VS Code como seu editor de texto/ambiente de desenvolvimento integrado (IDE), você pode tirar proveito do [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (um auxílio de auto-completar de código), rebaixando [(ajuda](https://code.visualstudio.com/docs/python/linting) a evitar a criação de erros em seu código), o [suporte à depuração](https://code.visualstudio.com/docs/python/debugging) (ajuda a encontrar erros em seu código depois de executá-lo), trechos de [código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modelos para pequenos blocos de código reutilizáveis) e [testes de unidade](https://code.visualstudio.com/docs/python/unit-testing) (testando a interface do seu código com diferentes tipos de entrada).

O VS Code também contém um [terminal interno](https://code.visualstudio.com/docs/editor/integrated-terminal) que permite que você abra uma linha de comando do Python com o prompt de comando do Windows, o PowerShell ou o que preferir, estabelecendo um fluxo de trabalho contínuo entre o editor de código e a linha de comando.

1. Para instalar VS Code, baixe o VS Code para Windows [https://code.visualstudio.com](https://code.visualstudio.com):.

2. O Python é uma linguagem interpretada e, para executar o código Python, você deve informar VS Code qual intérprete usar. É recomendável ficar com o Python 3,7, a menos que você tenha um motivo específico para escolher algo diferente. Selecione um intérprete Python 3 abrindo a paleta de **comandos** (Ctrl + Shift + P), comece a digitar o comando **Python: Selecione interpretador** para pesquisar e, em seguida, selecione o comando. Você também pode usar a opção **selecionar ambiente Python** na barra de status inferior, se disponível (talvez ele já mostre um intérprete selecionado). O comando apresenta uma lista de intérpretes disponíveis que VS Code pode localizar automaticamente, incluindo ambientes virtuais. Se você não vir o intérprete desejado, consulte Configurando [ambientes Python](https://code.visualstudio.com/docs/python/environments).

    ![Selecionar interpretador do Python no VS Code](../../images/interpreterselection.gif)

3. Para abrir o terminal no vs Code, selecione **Exibir** > **terminal**ou, como alternativa, use o atalho **Ctrl + '** (usando o caractere de acento grave). O terminal padrão é o PowerShell.

4. Dentro de seu terminal de VS Code, abra o Python simplesmente digitando o comando:`python`

5. Experimente o interpretador do Python digitando `print("Hello World")`:. O Python retornará sua instrução "Olá, Mundo".

    ![Linha de comando do Python no VS Code](../../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Instalar o Git (opcional)

Se você planeja colaborar com outras pessoas em seu código Python ou hospedar seu projeto em um site de software livre (como o GitHub), VS Code dá suporte ao [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia controle do código-fonte no VS Code rastreia todas as suas alterações e tem comandos git comuns (adicionar, confirmar, enviar por push, pull) criados diretamente na interface do usuário. Primeiro, você precisa instalar o Git para ligar o painel de controle do código-fonte.

1. Baixe e instale o Git para Windows no [site do git-SCM](https://git-scm.com/download/win).

2. Um assistente de instalação está incluído e fará uma série de perguntas sobre as configurações da instalação do git. É recomendável usar todas as configurações padrão, a menos que você tenha um motivo específico para alterar algo.

3. Se você nunca trabalhou com o Git antes, os [guias do GitHub](https://guides.github.com/) podem ajudá-lo a começar.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Olá, Mundo tutorial para algumas noções básicas do Python

O Python, de acordo com seu criador Guido van Rossum, é uma linguagem de programação de alto nível, e sua filosofia de design principal é sobre a legibilidade do código e uma sintaxe que permite aos programadores expressar conceitos em algumas linhas de código. "

Python é uma linguagem interpretada. Em contraste com as linguagens compiladas, em que o código que você escreve precisa ser traduzido no código do computador para ser executado pelo processador de seus computadores, o código Python é passado diretamente para um intérprete e executado diretamente. Basta digitar seu código e executá-lo. Vamos experimentar!

1. Com a linha de comando do PowerShell aberta `python` , insira para executar o interpretador do Python 3. (Algumas instruções preferem usar o comando `py` ou `python3`, elas também devem funcionar). Você saberá que tem êxito porque um > > > prompt com três símbolos maior que o que será exibido.

2. Há vários métodos internos que permitem que você faça modificações em cadeias de caracteres no Python. Crie uma variável, com: `variable = 'Hello World!'`. Pressione Enter para uma nova linha.

3. Imprima sua variável com: `print(variable)`. Isso exibirá o texto "Olá, Mundo!".

4. Descubra o comprimento, quantos caracteres são usados, da sua variável de cadeia de caracteres com `len(variable)`:. Isso exibirá que há 12 caracteres usados. (Observe que o espaço em branco que ele conta como um caractere no comprimento total.)

5. Converta sua variável de cadeia de caracteres em letras `variable.upper()`maiúsculas:. Agora, converta sua variável de cadeia de caracteres em `variable.lower()`letras minúsculas:.

6. Conte quantas vezes a letra "l" é usada na variável de cadeia de `variable.count("l")`caracteres:.

7. Pesquisar um caractere específico em sua variável de cadeia de caracteres, vamos encontrar o ponto de exclamação, com `variable.find("!")`:. Isso exibirá que o ponto de exclamação é encontrado no caractere de posição 11 da cadeia de caracteres.

8. Substitua o ponto de exclamação por uma interrogação: `variable.replace("!", "?")`.

9. Para sair do Python, você pode `exit()`inserir `quit()`, ou selecionar CTRL-Z.

![Captura de tela do PowerShell deste tutorial](../../images/hello-world-basics.png)

Espero que tenha se divertido usando alguns dos métodos internos de modificação de cadeia de caracteres do Python. Agora, tente criar um arquivo de programa Python e executá-lo com VS Code.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Olá, Mundo tutorial para usar o Python com VS Code

A equipe de VS Code reuniu um excelente [introdução com](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) o tutorial do Python examinando como criar um programa de Olá, mundo com o Python, executar o arquivo de programa, configurar e executar o depurador e instalar pacotes como *matplotlib* e  *numpy* para criar uma plotagem gráfica dentro de um ambiente virtual.

1. Abra o PowerShell e crie uma pasta vazia chamada "Olá", navegue até essa pasta e abra-a no VS Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Quando VS Code abrir, exibindo a nova pasta *Hello* na janela do **Gerenciador** do lado esquerdo, abra uma janela de linha de comando no painel inferior de vs Code pressionando **Ctrl + '** (usando o caractere de acento grave) ou selecionando **Exibir**  >  **Terminal**. Iniciando VS Code em uma pasta, essa pasta torna-se seu "espaço de trabalho". VS Code armazena configurações específicas para esse espaço de trabalho em. vscode/Settings. JSON, que são separadas das configurações de usuário que são armazenadas globalmente.

3. Continue o tutorial no VS Code docs: [Crie um arquivo de código-fonte do Python Olá, mundo](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Criar um jogo simples com o Pygame

![Pygame executando um jogo de exemplo](../../images/pygame-shmup.jpg)

Pygame é um pacote de Python popular para escrever jogos, incentivando os alunos a aprender a programar ao criar algo divertido. O Pygame exibe gráficos em uma nova janela e, portanto, não funcionará na abordagem somente de linha de comando de WSL. No entanto, se você instalou o Python por meio do Microsoft Store conforme detalhado neste tutorial, ele funcionará bem.

1. Depois de instalar o Python, instale o Pygame na linha de comando (ou no terminal de dentro do VS Code) `python -m pip install -U pygame --user`digitando.

2. Teste a instalação executando um jogo de exemplo:`python -m pygame.examples.aliens`

3. Tudo bem, o jogo abrirá uma janela. Feche a janela quando terminar de jogar.

Veja como começar a escrever seu próprio jogo.

1. Abra o PowerShell (ou prompt de comando do Windows) e crie uma pasta vazia chamada "Bounce". Navegue até essa pasta e crie um arquivo chamado "bounce.py". Abra a pasta no VS Code:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. Usando VS Code, insira o seguinte código Python (ou copie e cole-o):

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. Salve-o como `bounce.py`:.

4. No terminal do PowerShell, execute-o digitando `python bounce.py`:.

    ![Pygame executando a próxima grande coisa](../../images/pygame.jpg)

Tente ajustar alguns dos números para ver o efeito que eles têm em sua bola de saltação.

Leia mais sobre como escrever jogos com Pygame em [Pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Recursos para aprendizado contínuo

Recomendamos os seguintes recursos para dar suporte à continuação de como aprender sobre o desenvolvimento em Python no Windows.

### <a name="online-courses-for-learning-python"></a>Cursos online para aprendizagem do Python

- [Introdução ao Python no Microsoft Learn](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/): Experimente a plataforma de Microsoft Learn interativa e ganhe pontos de experiência para concluir este módulo cobrindo as noções básicas sobre como escrever código Python básico, declarar variáveis e trabalhar com entrada e saída do console. O ambiente de área restrita interativa torna esse um ótimo lugar para começar a pessoas que não têm seu ambiente de desenvolvimento do Python configurado ainda.

- [Python no Pluralsight: 8 cursos, 29 horas](https://app.pluralsight.com/paths/skills/python): O roteiro de aprendizagem do Python na Pluralsight oferece cursos online que abrangem uma variedade de tópicos relacionados ao Python, incluindo uma ferramenta para medir suas habilidades e encontrar suas lacunas.

- [Tutoriais do LearnPython.org](https://www.learnpython.org/): Comece a aprender sobre o Python sem precisar instalar ou definir nada com esses tutoriais de Python interativos gratuitos do pessoal em datacamp.

- [Os tutoriais do Python.org](https://docs.python.org/3/tutorial/index.html): Apresenta o leitor informalmente aos conceitos e recursos básicos da linguagem Python e do sistema.

- [Aprendendo sobre o Python no Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Uma introdução básica ao Python.

### <a name="working-with-python-in-vs-code"></a>Trabalhando com Python no VS Code

- [Editando Python no vs Code](https://code.visualstudio.com/docs/python/editing): Saiba mais sobre como aproveitar o suporte automático e IntelliSense do VS Code para Python, incluindo como personalizar seu behvior... ou simplesmente desativá-los.

- [Refiapoing Python](https://code.visualstudio.com/docs/python/linting): O refiapoing é o processo de execução de um programa que analisará o código em busca de possíveis erros. Saiba mais sobre as diferentes formas de resumir suporte VS Code fornece para Python e como configurá-lo.

- [Depuração Python](https://code.visualstudio.com/docs/python/debugging): A depuração é o processo de identificação e remoção de erros de um programa de computador. Este artigo aborda como inicializar e configurar a depuração para Python com VS Code, como definir e validar pontos de interrupção, anexar um script local, executar a depuração para diferentes tipos de aplicativo ou em um computador remoto e algumas soluções de problemas básicas.

- [Python de teste de unidade](https://code.visualstudio.com/docs/python/unit-testing): Aborda alguns planos de fundo que explicam o que significa teste de unidade, um exemplo de explicação, habilitando uma estrutura de teste, criando e executando seus testes, testes de depuração e definições de configuração de teste. 
