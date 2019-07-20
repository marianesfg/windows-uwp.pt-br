---
title: Scripts e automação com Python no Windows
description: Como começar a usar o Python para scripting, automação e administração de sistemas no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: Python, Windows 10, Microsoft, Python System Administration, Python File Automation, scripts Python no Windows, configurar o Python no Windows, ambiente de desenvolvedor Python no Windows, ambiente de desenvolvimento do Python no Windows, Python com PowerShell, scripts Python para tarefas do sistema de arquivos
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 0571d442d17cdac8989df10d7c11f3e762ab6fb6
ms.sourcegitcommit: afb5157ec4bcb6588ac4cf74352688b30ed32257
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68349517"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Comece a usar o Python no Windows para scripts e automação

Veja a seguir um guia passo a passo para configurar seu ambiente de desenvolvedor e começar a usar o Python para criar scripts e automatizar operações do sistema de arquivos no Windows.

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Ao usar o Python para gravar scripts que executam operações do sistema de arquivos, recomendamos que você [Instale o Python da Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). 

> [!IMPORTANT]
> Se você estiver usando o Python no Windows para desenvolvimento para a **Web**, recomendamos uma configuração diferente para seu ambiente de desenvolvimento. Em vez de instalar diretamente no Windows, instale o Python por meio do subsistema do Windows para Linux. Encontre instruções em nosso guia: Comece [a usar o Python para desenvolvimento para a Web no Windows](./python-for-web.md). Se você for um novato no Python, experimente nosso guia: Comece [a usar o Python no Windows para iniciantes](./python-for-education.md). <br>Para alguns cenários avançados, talvez você queira baixar uma versão específica do Python diretamente do [Python.org](https://www.python.org/downloads/windows/) ou considerar a instalação de uma [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython etc. Só recomendamos isso se você for um programador de Python mais avançado com um motivo específico para escolher uma implementação alternativa.

## <a name="install-python"></a>Instalar o Python

Para instalar o Python usando o Microsoft Store:

1. Vá para o menu **Iniciar** (ícone inferior esquerdo do Windows), digite "Microsoft Store", selecione o link para abrir o repositório.

2. Quando o repositório estiver aberto, selecione **Pesquisar** no menu superior direito e insira "Python". Abra "Python 3,7" nos resultados em aplicativos. Selecione **obter**.

3. Depois que o Python concluir o processo de download e instalação, abra o Windows PowerShell usando o menu **Iniciar** (ícone inferior esquerdo do Windows). Quando o PowerShell estiver aberto, `Python --version` insira para confirmar que o Python3 foi instalado em seu computador.

4. O Microsoft Store instalação do Python inclui **Pip**, o Gerenciador de pacotes padrão. Pip permite que você instale e gerencie pacotes adicionais que não fazem parte da biblioteca padrão do Python. Para confirmar que você também tem o Pip disponível para instalar e gerenciar pacotes, `pip --version`digite.

## <a name="install-visual-studio-code"></a>Instalar Visual Studio Code

Usando VS Code como seu editor de texto/ambiente de desenvolvimento integrado (IDE), você pode tirar proveito do [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (um auxílio de auto-completar de código), rebaixando [(ajuda](https://code.visualstudio.com/docs/python/linting) a evitar a criação de erros em seu código), o [suporte à depuração](https://code.visualstudio.com/docs/python/debugging) (ajuda a encontrar erros em seu código depois de executá-lo), trechos de [código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modelos para pequenos blocos de código reutilizáveis) e [testes de unidade](https://code.visualstudio.com/docs/python/unit-testing) (testando a interface do seu código com diferentes tipos de entrada).

Baixe VS Code para Windows e siga as instruções de instalação [https://code.visualstudio.com](https://code.visualstudio.com):.

## <a name="install-the-microsoft-python-extension"></a>Instalar a extensão do Microsoft Python

Você precisará instalar a extensão do Microsoft Python para aproveitar os recursos de suporte do VS Code. [Saiba mais](https://code.visualstudio.com/docs/languages/python).

1. Abra a janela extensões de vs Code digitando **Ctrl + Shift + X** (ou use o menu para navegar até**as extensões**de **exibição** > ).

2. Na caixa **extensões de pesquisa principais no Marketplace** , digite:  **Python**.

3. Localize a extensão **Python (MS-Python. Python) pela Microsoft** e selecione o botão de **instalação** verde.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Abra o terminal do PowerShell integrado no VS Code

VS Code contém um [terminal interno](https://code.visualstudio.com/docs/editor/integrated-terminal) que permite que você abra uma linha de comando do Python com o PowerShell, estabelecendo um fluxo de trabalho contínuo entre o editor de código e a linha de comando.

1. Abra o terminal no vs Code, selecione **Exibir** > **terminal**ou, como alternativa, use o atalho **Ctrl + '** (usando o caractere de acento grave).

    > [!NOTE]
    > O terminal padrão deve ser o PowerShell, mas se você precisar alterá-lo, use **Ctrl + Shift + P** para inserir o comando Pallette. Insira **o terminal: Selecione Shell** padrão e uma lista de opções de terminal será exibida contendo PowerShell, prompt de comando, WSL, etc. Selecione aquele que você deseja usar e **pressione Ctrl + Shift + '** (usando o acento grave) para criar um novo terminal.

2. Dentro de seu Terminal VS Code, abra o Python digitando:`python`

3. Experimente o interpretador do Python digitando `print("Hello World")`:. O Python retornará sua instrução "Olá, Mundo".

    ![Linha de comando do Python no VS Code](../../images/python-in-vscode.png)

4. Para sair do Python, você pode `exit()`inserir `quit()`, ou selecionar CTRL-Z.

## <a name="install-git-optional"></a>Instalar o Git (opcional)

Se você planeja colaborar com outras pessoas em seu código Python ou hospedar seu projeto em um site de software livre (como o GitHub), VS Code dá suporte ao [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia controle do código-fonte no VS Code rastreia todas as suas alterações e tem comandos git comuns (adicionar, confirmar, enviar por push, pull) criados diretamente na interface do usuário. Primeiro, você precisa instalar o Git para ligar o painel de controle do código-fonte.

1. Baixe e instale o Git para Windows no [site do git-SCM](https://git-scm.com/download/win).

2. Um assistente de instalação está incluído e fará uma série de perguntas sobre as configurações da instalação do git. É recomendável usar todas as configurações padrão, a menos que você tenha um motivo específico para alterar algo.

3. Se você nunca trabalhou com o Git antes, os [guias do GitHub](https://guides.github.com/) podem ajudá-lo a começar.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Script de exemplo para exibir a estrutura do diretório do sistema de arquivos

As tarefas comuns de administração do sistema podem levar um grande tempo, mas com um script Python, você pode automatizar essas tarefas para que elas não tenham nenhum tempo. Por exemplo, o Python pode ler o conteúdo do sistema de arquivos do seu computador e executar operações como imprimir uma estrutura de tópicos de seus arquivos e diretórios, mover pastas de um diretório para outro ou renomear centenas de arquivos. Normalmente, tarefas como essas poderiam levar um grande tempo se você fosse executá-las manualmente. Em vez disso, use um script Python!

Vamos começar com um script simples que examina uma árvore de diretório e exibe a estrutura do diretório.

1. Abra o PowerShell usando o menu **Iniciar** (ícone inferior esquerdo do Windows).

2. Crie um diretório para seu projeto: `mkdir python-scripts`e, em seguida, abra `cd python-scripts`esse diretório:.

3. Crie alguns diretórios para usar com nosso script de exemplo:

    ```powershell
    mkdir food, food/fruits, food/fruits/apples, food/fruits/oranges, food/vegetables
    ```

4. Crie alguns arquivos dentro desses diretórios para usar com nosso script:

    ```powershell
    new-item food/fruits/banana.txt, food/fruits/strawberry.txt, food/fruits/blueberry.txt, food/fruits/apples/honeycrisp.txt, food/fruits/oranges/mandarin.txt, food/vegetables/carrot.txt
    ```

5. Crie um novo arquivo Python em seu diretório Python-scripts:

    ```powershell
    mkdir src
    new-item src/list-directory-contents.py
    ```

6. Abra seu projeto no VS Code digitando:`code .`

7. Abra a janela vs Code explorador de arquivos digitando **Ctrl + Shift + E** (ou use o menu para navegar até o**Gerenciador**de **exibição** > ) e selecione o arquivo list-Directory-Contents.py que você acabou de criar. A extensão do Microsoft Python carregará automaticamente um intérprete do Python. Você pode ver qual intérprete foi carregado na parte inferior da janela de VS Code.

    > [!NOTE]
    > O Python é uma linguagem interpretada, o que significa que ele atua como uma máquina virtual, emulando um computador físico. Há diferentes tipos de intérpretes de Python que você pode usar: Python 2, Python 3, Anaconda, PyPy, etc. Para executar o código Python e obter o IntelliSense do Python, você deve informar VS Code qual intérprete usar. É recomendável ficar com o intérprete que VS Code escolhe por padrão (Python 3 em nosso caso), a menos que você tenha um motivo específico para escolher algo diferente. Para alterar o intérprete do Python, selecione o intérprete atualmente exibido na barra azul na parte inferior da janela de vs Code ou abra a **paleta de comandos** (Ctrl + Shift + P) e insira o comando **Python: Selecione intérprete**. Isso exibirá uma lista dos interpretadores do Python que você instalou atualmente. [Saiba mais sobre a configuração de ambientes do Python](https://code.visualstudio.com/docs/python/environments).

    ![Selecionar interpretador do Python no VS Code](../../images/interpreterselection.gif)

8. Cole o código a seguir em seu arquivo list-directory-contents.py e, em seguida, selecione **salvar**:

    ```python
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory: ' + directory)
        for name in subdir_list:
            print ('Subdirectory: ' + name)
        for name in file_list:
            print('File: ' + name)
        print(os.linesep)
    ```

9. Abra o terminal integrado VS Code (**Ctrl + '** , usando o caractere de acento grave) e insira o diretório src em que você acabou de salvar o script Python:

    ```powershell
    cd src
    ```

10. Execute o script no PowerShell com:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    Você deverá ver uma saída parecida com esta:

    ```powershell
    Directory: ../food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ../food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ../food\fruits\apples
    File: honeycrisp.txt

    Directory: ../food\fruits\oranges
    File: mandarin.txt

    Directory: ../food\vegetables
    File: carrot.txt
    ```

11. Use o Python para imprimir essa saída do diretório do sistema de arquivos para seu próprio arquivo de texto inserindo este comando diretamente no seu terminal do PowerShell:`python3 list-directory-contents.py > food-directory.txt`

Parabéns! Você acabou de escrever um script de administração de sistemas automatizados que lê o diretório e os arquivos que você criou e usa o Python para exibir e, em seguida, imprimir a estrutura de diretório no próprio arquivo de texto.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Script de exemplo para modificar todos os arquivos em um diretório

Este exemplo usa os arquivos e diretórios que você acabou de criar, renomeando cada um dos arquivos adicionando a data da última modificação do arquivo ao início do nome de arquivo.

1. Dentro da pasta **src** em seu diretório **Python-scripts** , crie um novo arquivo Python para o script:

    ```powershell
    new-item update-filenames.py
    ```

2. Abra o arquivo update-filenames.py, Cole o código a seguir no arquivo e salve-o:

    > [!NOTE]
    > os. getmtime retorna um carimbo de data/hora em tiques, o que não é facilmente legível. Ele deve ser convertido em uma cadeia de caracteres DateTime padrão primeiro.

    ```python
    import datetime
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = '%s%s%s' % (directory, os.path.sep, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = '%s%s%s_%s' % (directory, os.path.sep, modified_date, name)

            print ('Renaming: %s to: %s' % (source_name, target_name))

            os.rename(source_name, target_name)
    ```

3. Teste seu script Update-FileNames.py executando-o: `python3 update-filenames.py` e, em seguida, executando o script List-Directory-Contents.py novamente:`python3 list-directory-contents.py`

4. Você deverá ver uma saída parecida com esta:

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    ~/src/python-scripting/src$ python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Use o Python para imprimir os novos nomes de diretório do sistema de arquivos com o carimbo de data/hora modificado pela última vez em seu próprio arquivo de texto digitando este comando diretamente no seu terminal do PowerShell:`python3 list-directory-contents.py > food-directory-last-modified.txt`

Espero que tenha aprendido algumas coisas divertidas sobre o uso de scripts Python para automatizar tarefas básicas de administração de sistemas. Há, é claro, uma infinidade de saber, mas esperamos que isso tenha sido iniciado no pé certo. Nós compartilhamos alguns recursos adicionais para continuar aprendendo abaixo.

## <a name="additional-resources"></a>Recursos adicionais

- [Documentos do Python: Acesso](https://docs.python.org/3.7/library/filesys.html)a arquivos e diretórios: Documentação do Python sobre como trabalhar com sistemas de arquivos e usar módulos para ler as propriedades de arquivos, manipular caminhos de forma portátil e criar arquivos temporários.
- [Aprenda o Python: Tutorial](https://www.learnpython.org/en/String_Formatting)do String_Formatting: Mais sobre o uso do operador "%" para formatação de cadeia de caracteres.
- [10 métodos do sistema de arquivos Python que você deve saber](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): Artigo médio sobre a manipulação de arquivos e pastas `os` com `shutil`o e o.
- [O guia de hitchhikers para Python: Administração](https://docs.python-guide.org/scenarios/admin/)de sistemas: Um "guia do conceituada" que oferece visões gerais e práticas recomendadas sobre tópicos relacionados ao Python. Esta seção aborda as ferramentas de administração do sistema e as estruturas do. Este guia é hospedado no GitHub para que você possa arquivar problemas e fazer contribuições.
