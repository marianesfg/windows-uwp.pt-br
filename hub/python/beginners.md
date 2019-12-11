---
title: Introdução ao uso do Python no Windows para iniciantes
description: Um guia para iniciantes para ajudar você a começar a usar o Python no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, introdução ao Python, Python no Windows para iniciantes, instalar o Python com a Microsoft Store, Python com o VS Code, PyGame no Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 688ae004dad8653e70d86b3b91652b6898c1e9d3
ms.sourcegitcommit: f5bb4e35d1373b982259e61547b3b1765da0e78c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881280"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Introdução ao uso do Python no Windows para iniciantes

Veja a seguir um guia passo a passo para os iniciantes que estão interessados em aprender mais sobre o Python usando o Windows 10.

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Para os iniciantes que realmente não têm noções básicas sobre o Python, recomendamos a [instalação do Python pela Microsoft Store](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). A instalação por meio da Microsoft Store usa o interpretador básico Python3, mas cuida da definição das configurações de PATH para o usuário atual (evitando a necessidade de acesso de administrador), além de fornecer atualizações automáticas. Isso será especialmente útil se você estiver em um ambiente educacional ou fizer parte de uma organização que restringe as permissões ou o acesso administrativo no computador.

Se você estiver usando o Python no Windows para o **desenvolvimento para a Web**, recomendamos uma configuração diferente para o ambiente de desenvolvimento. Em vez da instalação direta no Windows, recomendamos a instalação e o uso do Python por meio do Subsistema do Windows para Linux. Para obter ajuda, confira: [Introdução ao uso do Python para desenvolvimento para a Web no Windows](./web-frameworks.md). Se você estiver interessado em automatizar tarefas comuns no sistema operacional, confira nosso guia: [Introdução ao uso do Python no Windows para script e automação](./scripting.md). Para alguns cenários avançados (como a necessidade de acessar/modificar arquivos instalados do Python, fazer cópias de binários ou usar as DLLs do Python diretamente), o ideal é considerar a possibilidade de baixar uma versão específica do Python diretamente em [python.org](https://www.python.org/downloads/) ou instalar uma [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython etc. Só recomendamos isso se você for um programador mais avançado do Python com um motivo específico para escolher uma implementação alternativa.

## <a name="install-python"></a>Instalar o Python

Para instalar o Python usando a Microsoft Store:

1. Acesse o menu **Iniciar** (ícone do Windows no canto inferior esquerdo), digite "Microsoft Store" e selecione o link para abrir a loja.

2. Quando a loja estiver aberta, selecione **Pesquisar** no menu superior direito e insira "Python". Abra "Python 3.7" nos resultados em Aplicativos. Selecione **Obter**.

3. Depois que o Python concluir o processo de download e instalação, abra o Windows PowerShell usando o menu **Iniciar** (ícone do Windows no canto inferior esquerdo). Quando o PowerShell estiver aberto, insira `Python --version` para confirmar se o Python3 foi instalado no computador.

4. A instalação do Python por meio da Microsoft Store inclui o **pip**, o gerenciador de pacotes padrão. O pip permite que você instale e gerencie pacotes adicionais que não fazem parte da biblioteca padrão do Python. Para confirmar que você também tem o pip disponível para instalar e gerenciar pacotes, insira `pip --version`.

## <a name="install-visual-studio-code"></a>Instalar o Visual Studio Code

Usando o VS Code como o editor de texto/o IDE (ambiente de desenvolvimento integrado), você pode aproveitar o [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (um recurso de preenchimento de código), o [Linting](https://code.visualstudio.com/docs/python/linting) (ajuda a evitar erros no código), o [Suporte de depuração](https://code.visualstudio.com/docs/python/debugging) (ajuda a encontrar erros no código depois de executá-lo), os [Snippets de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modelos para pequenos blocos de código reutilizáveis) e os [Testes de unidade](https://code.visualstudio.com/docs/python/unit-testing) (testes da interface do código com diferentes tipos de entrada).

O VS Code também contém um [terminal interno](https://code.visualstudio.com/docs/editor/integrated-terminal) que permite que você abra uma linha de comando do Python com o Prompt de Comando do Windows, o PowerShell ou o que preferir, estabelecendo um fluxo de trabalho contínuo entre o editor de códigos e a linha de comando.

1. Para instalar o VS Code, baixe o VS Code para Windows: [https://code.visualstudio.com](https://code.visualstudio.com).

2. Depois que o VS Code tiver sido instalado, você também deverá instalar a extensão do Python. Para instalar a extensão do Python, você pode selecionar o link [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-python.python) ou abrir o VS Code e pesquisar **Python** no menu extensões (Ctrl+Shift+X).

3. O Python é uma linguagem interpretada e, para executar o código Python, você precisará informar o VS Code de qual interpretador usar. Recomendamos o uso do Python 3.7, a menos que você tenha um motivo específico para escolher algo diferente. Depois de ter instalado a extensão do Python, selecione um interpretador do Python 3 abrindo a **paleta de comandos** (Ctrl+Shift+P) e comece a digitar o comando **Python: Selecionar Interpretador** para pesquisar e, em seguida, selecione o comando. Use também a opção **Selecionar Ambiente do Python** na barra de status inferior, se disponível (talvez ela já mostre um interpretador selecionado). O comando apresenta uma lista dos interpretadores disponíveis que o VS Code pode localizar automaticamente, incluindo ambientes virtuais. Se o interpretador desejado não for exibido, confira [Como configurar ambientes do Python](https://code.visualstudio.com/docs/python/environments).

    ![Selecionar um interpretador do Python no VS Code](../images/interpreterselection.gif)

4. Para abrir o terminal no VS Code, selecione **Exibir** > **Terminal** ou, como alternativa, use o atalho **Ctrl+`** (usando o caractere de acento grave). O terminal padrão é o PowerShell.

5. No terminal do VS Code, abra o Python apenas inserindo o comando: `python`

6. Experimente o interpretador do Python inserindo `print("Hello World")`. O Python retornará a declaração "Olá, Mundo".

    ![Linha de comando do Python no VS Code](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Instalar o Git (opcional)

Se você pretende colaborar com outras pessoas no código Python ou hospedar seu projeto em um site open-source (como o GitHub), o VS Code dá suporte ao [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia Controle do Código-fonte no VS Code acompanha todas as alterações e tem comandos Git comuns (add, commit, push e pull) incorporados diretamente na interface do usuário. Primeiro, você precisa instalar o Git para ativar o painel Controle do Código-fonte.

1. Baixe e instale o Git para Windows no [site do git-scm](https://git-scm.com/download/win).

2. Um Assistente de Instalação está incluído e fará uma série de perguntas sobre as configurações da instalação do Git. Recomendamos o uso de todas as configurações padrão, a menos que você tenha um motivo específico para alterar algo.

3. Se você nunca trabalhou com o Git antes, os [Guias do GitHub](https://guides.github.com/) podem ajudar você a começar a usá-lo.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Tutorial de Olá, Mundo para aprender alguns conceitos básicos do Python

O Python, de acordo com o criador Guido van Rossum, é uma "linguagem de programação de alto nível e a filosofia de design principal resume-se à legibilidade do código e a uma sintaxe que permite aos programadores expressar conceitos em algumas linhas de código”.

O Python é uma linguagem interpretada. Em contraste com as linguagens compiladas, em que o código escrito precisa ser convertido no código do computador para ser executado pelo processador do computador, o código Python é passado para um interpretador e executado diretamente. Basta digitar seu código e executá-lo. Vamos testá-lo!

1. Com a linha de comando do PowerShell aberta, insira `python` para executar o interpretador do Python 3. (Algumas instruções preferem usar o comando `py` ou `python3`, e isso também deverá funcionar). Você saberá que obteve êxito porque um aviso >>> com três símbolos maior que será exibido.

2. Há vários métodos internos que permitem que você faça modificações em cadeias de caracteres no Python. Crie uma variável com `variable = 'Hello World!'`. Pressione Enter para ir para uma nova linha.

3. Imprima a variável com `print(variable)`. Isso exibirá o texto "Olá, Mundo!".

4. Descubra o tamanho e a quantidade de caracteres usada da variável de cadeia de caracteres com `len(variable)`. Isso mostrará que há 12 caracteres usados. (Observe que o espaço em branco contado como um caractere no tamanho total.)

5. Converta a variável de cadeia de caracteres em letras maiúsculas: `variable.upper()`. Agora, converta a variável de cadeia de caracteres em letras minúsculas: `variable.lower()`.

6. Conte quantas vezes a letra "l" é usada na variável de cadeia de caracteres: `variable.count("l")`.

7. Pesquise um caractere específico na variável de cadeia de caracteres. Vamos encontrar o ponto de exclamação com `variable.find("!")`. Isso mostrará que o ponto de exclamação é encontrado no caractere na 11ª posição da cadeia de caracteres.

8. Substitua o ponto de exclamação por um ponto de interrogação: `variable.replace("!", "?")`.

9. Para sair do Python, insira `exit()`, `quit()` ou selecione Ctrl-Z.

![Captura de tela do PowerShell deste tutorial](../images/hello-world-basics.png)

Esperamos que tenha se divertido usando alguns dos métodos internos de modificação de cadeia de caracteres do Python. Agora tente criar um arquivo de programa do Python e executá-lo com o VS Code.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Tutorial de Olá, Mundo para usar o Python com o VS Code

A equipe do VS Code compilou um excelente tutorial [Introdução ao Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) descrevendo como criar um programa Olá, Mundo com o Python, executar o arquivo de programa, configurar e executar o depurador e instalar pacotes como *matplotlib* e *numpy* para criar um gráfico em um ambiente virtual.

1. Abra o PowerShell e crie uma pasta vazia chamada "hello", navegue até essa pasta e abra-a no VS Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Quando o VS Code for aberto exibindo a nova pasta *hello* na janela do **Explorador** no lado esquerdo, abra uma janela de linha de comando no painel inferior do VS Code pressionando **CTRL+`** (usando o caractere de acento grave) ou selecionando **Exibir** > **Terminal**. Iniciando o VS Code em uma pasta, essa pasta torna-se o "workspace". O VS Code armazena configurações específicas para esse workspace em .vscode/settings.json, que são separadas das configurações de usuário armazenadas globalmente.

3. Continue o tutorial nos documentos do VS Code: [Crie um arquivo de código-fonte Olá, Mundo do Python](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Criar um jogo simples com o Pygame

![PyGame executando um jogo de exemplo](../images/pygame-shmup.jpg)

O PyGame é um pacote do Python popular para escrever jogos, incentivando os alunos a aprender a programar enquanto criam algo divertido. O PyGame exibe gráficos em uma nova janela e, portanto, não funcionará na abordagem somente de linha de comando do WSL. No entanto, se você instalou o Python por meio da Microsoft Store conforme detalhado neste tutorial, ele funcionará corretamente.

1. Depois de instalar o Python, instale o PyGame por meio da linha de comando (ou do terminal no VS Code) digitando `python -m pip install -U pygame --user`.

2. Teste a instalação executando um jogo de exemplo: `python -m pygame.examples.aliens`

3. Se tudo estiver certo, o jogo abrirá uma janela. Feche a janela quando terminar de jogar.

Veja como começar a escrever seu próprio jogo.

1. Abra o PowerShell (ou o Prompt de Comando do Windows) e crie uma pasta vazia chamada "bounce". Navegue até essa pasta e crie um arquivo chamado "bounce.py". Abra a pasta no VS Code:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. Usando o VS Code, insira o seguinte código Python (ou copie-o e cole-o):

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

3. Salve-o como `bounce.py`.

4. No terminal do PowerShell, execute-o inserindo `python bounce.py`.

    ![PyGame executando o próximo jogo do momento](../images/pygame.jpg)

Tente ajustar alguns dos números para ver o efeito que eles têm na bola saltitante.

Leia mais sobre como escrever jogos com o PyGame em [pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Recursos para aprendizado contínuo

Recomendamos os recursos a seguir para ajudar você a continuar aprendendo sobre o desenvolvimento com o Python no Windows.

### <a name="online-courses-for-learning-python"></a>Cursos online de introdução ao Python

- [Introdução ao Python no Microsoft Learn](https://docs.microsoft.com/learn/modules/intro-to-python/): Experimente a plataforma interativa do Microsoft Learn e ganhe pontos de experiência por concluir este módulo abordando os conceitos básicos sobre como escrever um código Python básico, declarar variáveis e trabalhar com a entrada e a saída do console. O ambiente interativo de área restrita faz desse um ótimo lugar para começar para as pessoas que ainda não têm seu próprio ambiente de desenvolvimento do Python configurado.

- [Python no Pluralsight: 8 cursos, 29 horas](https://app.pluralsight.com/paths/skills/python): O roteiro de aprendizagem do Python no Pluralsight oferece cursos online que abrangem uma variedade de tópicos relacionados ao Python, incluindo uma ferramenta para avaliar suas habilidades e encontrar suas lacunas.

- [Tutoriais de LearnPython.org](https://www.learnpython.org/): Comece a aprender a usar o Python sem precisar instalar nem definir nada com estes tutoriais interativos e gratuitos do Python da equipe do DataCamp.

- [Os tutoriais do Python.org](https://docs.python.org/3/tutorial/index.html): Apresenta o leitor informalmente aos conceitos básicos e aos recursos da linguagem Python e do sistema.

- [Introdução ao Python no Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Uma introdução básica ao Python.

### <a name="working-with-python-in-vs-code"></a>Como trabalhar com o Python no VS Code

- [Como editar o Python no VS Code](https://code.visualstudio.com/docs/python/editing): Saiba mais sobre como aproveitar o preenchimento automático e o suporte do IntelliSense do VS Code para o Python, incluindo como personalizar o comportamento deles ou apenas desligá-los.

- [Linting do Python](https://code.visualstudio.com/docs/python/linting): Linting é o processo de execução de um programa que analisará o código em busca de possíveis erros. Saiba mais sobre as diferentes formas de suporte de linting fornecidas pelo VS Code para o Python e como configurá-lo.

- [Depuração do Python](https://code.visualstudio.com/docs/python/debugging): Depuração é o processo de identificação e remoção de erros de um programa de computador. Este artigo aborda como inicializar e configurar a depuração para o Python com o VS Code, como definir e validar pontos de interrupção, anexar um script local, executar a depuração para diferentes tipos de aplicativos ou em um computador remoto e algumas soluções de problemas básicas.

- [Teste de unidade do Python](https://code.visualstudio.com/docs/python/unit-testing): Aborda uma explicação preliminar sobre o significado do teste de unidade, um passo a passo de exemplo, a habilitação de uma estrutura de teste, a criação e a execução dos testes, a depuração de testes e definições de configuração de teste. 
