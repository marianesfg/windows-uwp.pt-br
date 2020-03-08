---
title: Script e automação com o Python no Windows
description: Introdução ao uso do Python para script, automação e administração de sistemas no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, administração do sistema do Python, automação de arquivos do Python, scripts do Python no Windows, configurar o Python no Windows, ambiente de desenvolvedor do Python no Windows, ambiente de desenvolvimento do Python no Windows, Python com o PowerShell, scripts do Python para tarefas do sistema de arquivos
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d465d46a0524345a45dff9b1cc7c425e4cb468a4
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853163"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Introdução ao uso do Python no Windows para script e automação

Veja a seguir um guia passo a passo para configurar seu ambiente de desenvolvedor e começar a usar o Python para criar scripts e automatizar operações do sistema de arquivos no Windows.

> [!NOTE]
> Este artigo abordará a configuração do ambiente para usar algumas das bibliotecas úteis do Python que podem automatizar tarefas entre plataformas, como pesquisa no sistema de arquivos, acesso à Internet, análise de tipos de arquivo etc., em uma abordagem centralizada no Windows. Para operações específicas do Windows, confira [ctypes](https://docs.python.org/3/library/ctypes.html), uma biblioteca de funções estrangeiras compatíveis com C para o Python, [winreg](https://docs.python.org/3/library/winreg.html), funções que expõem a API do Registro do Windows para Python e [Python/WinRT](https://pypi.org/project/winrt/), habilitação do acesso das APIs do Windows Runtime no Python.

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Ao usar o Python para escrever scripts que executam operações do sistema de arquivos, recomendamos [instalar o Python por meio da Microsoft Store](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). A instalação por meio da Microsoft Store usa o interpretador básico Python3, mas cuida da definição das configurações de PATH para o usuário atual (evitando a necessidade de acesso de administrador), além de fornecer atualizações automáticas.

Se você estiver usando o Python para **desenvolvimento para a Web** no Windows, recomendamos uma configuração diferente usando o Subsistema do Windows para Linux. Encontre um passo a passo em nosso guia: [Introdução ao uso do Python para desenvolvimento para a Web no Windows](./web-frameworks.md). Se você estiver conhecendo o Python agora, experimente nosso guia: [Introdução ao uso do Python no Windows para iniciantes](./beginners.md). Para alguns cenários avançados (como a necessidade de acessar/modificar arquivos instalados do Python, fazer cópias de binários ou usar as DLLs do Python diretamente), o ideal é considerar a possibilidade de baixar uma versão específica do Python diretamente em [python.org](https://www.python.org/downloads/) ou instalar uma [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython etc. Só recomendamos isso se você for um programador mais avançado do Python com um motivo específico para escolher uma implementação alternativa.

## <a name="install-python"></a>Instalar o Python

Para instalar o Python usando a Microsoft Store:

1. Acesse o menu **Iniciar** (ícone do Windows no canto inferior esquerdo), digite "Microsoft Store" e selecione o link para abrir a loja.

2. Quando a loja estiver aberta, selecione **Pesquisar** no menu superior direito e insira "Python". Abra "Python 3.7" nos resultados em Aplicativos. Selecione **Obter**.

3. Depois que o Python concluir o processo de download e instalação, abra o Windows PowerShell usando o menu **Iniciar** (ícone do Windows no canto inferior esquerdo). Quando o PowerShell estiver aberto, insira `Python --version` para confirmar se o Python3 foi instalado no computador.

4. A instalação do Python por meio da Microsoft Store inclui o **pip**, o gerenciador de pacotes padrão. O pip permite que você instale e gerencie pacotes adicionais que não fazem parte da biblioteca padrão do Python. Para confirmar que você também tem o pip disponível para instalar e gerenciar pacotes, insira `pip --version`.

## <a name="install-visual-studio-code"></a>Instalar o Visual Studio Code

Usando o VS Code como o editor de texto/o IDE (ambiente de desenvolvimento integrado), você pode aproveitar o [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (um recurso de preenchimento de código), o [Linting](https://code.visualstudio.com/docs/python/linting) (ajuda a evitar erros no código), o [Suporte de depuração](https://code.visualstudio.com/docs/python/debugging) (ajuda a encontrar erros no código depois de executá-lo), os [Snippets de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modelos para pequenos blocos de código reutilizáveis) e os [Testes de unidade](https://code.visualstudio.com/docs/python/unit-testing) (testes da interface do código com diferentes tipos de entrada).

Baixe o VS Code para Windows e siga as instruções de instalação: [https://code.visualstudio.com](https://code.visualstudio.com).

## <a name="install-the-microsoft-python-extension"></a>Instalar a extensão do Microsoft Python

Você precisará instalar a extensão do Microsoft Python para aproveitar os recursos de suporte do VS Code. [Saiba mais](https://code.visualstudio.com/docs/languages/python).

1. Abra a janela Extensões do VS Code inserindo **Ctrl+Shift+X** (ou use o menu para navegar até **Exibir** > **Extensões**).

2. Na caixa **Pesquisar Extensões no Marketplace** no canto superior, insira:  **Python**.

3. Encontre a extensão **Python (ms-python.python) da Microsoft** e selecione o botão verde **Instalar**.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Abrir o terminal integrado do PowerShell no VS Code

O VS Code contém um [terminal interno](https://code.visualstudio.com/docs/editor/integrated-terminal) que permite abrir uma linha de comando do Python com o PowerShell, estabelecendo um fluxo de trabalho contínuo entre o editor de código e a linha de comando.

1. Abra o terminal no VS Code, selecione **Exibir** > **Terminal** ou, como alternativa, use o atalho **Ctrl+`** (usando o caractere de acento grave).

    > [!NOTE]
    > O terminal padrão deverá ser o PowerShell, mas se você precisar alterá-lo, use **Ctrl+Shift+P** para inserir a paleta de comandos. Insira **Terminal: Selecionar Shell Padrão** e uma lista de opções de terminal será exibida, contendo o PowerShell, o prompt de comando, o WSL etc. Selecione aquela que deseja usar e insira **Ctrl+Shift+`** (usando o acento grave) para criar um terminal.

2. No terminal do VS Code, abra o Python inserindo: `python`

3. Experimente o interpretador do Python inserindo `print("Hello World")`. O Python retornará a declaração "Olá, Mundo".

    ![Linha de comando do Python no VS Code](../images/python-in-vscode.png)

4. Para sair do Python, insira `exit()`, `quit()` ou selecione Ctrl-Z.

## <a name="install-git-optional"></a>Instalar o Git (opcional)

Se você pretende colaborar com outras pessoas no código Python ou hospedar seu projeto em um site open-source (como o GitHub), o VS Code dá suporte ao [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia Controle do Código-fonte no VS Code acompanha todas as alterações e tem comandos Git comuns (add, commit, push e pull) incorporados diretamente na interface do usuário. Primeiro, você precisa instalar o Git para ativar o painel Controle do Código-fonte.

1. Baixe e instale o Git para Windows no [site do git-scm](https://git-scm.com/download/win).

2. Um Assistente de Instalação está incluído e fará uma série de perguntas sobre as configurações da instalação do Git. Recomendamos o uso de todas as configurações padrão, a menos que você tenha um motivo específico para alterar algo.

3. Se você nunca trabalhou com o Git antes, os [Guias do GitHub](https://guides.github.com/) podem ajudar você a começar a usá-lo.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Script de exemplo para exibir a estrutura do diretório do sistema de arquivos

As tarefas comuns de administração do sistema podem levar muito tempo, mas com um script Python, você pode automatizar essas tarefas para torná-las rápidas. Por exemplo, o Python pode ler o conteúdo do sistema de arquivos do computador e executar operações como imprimir uma estrutura de tópicos dos arquivos e dos diretórios, mover pastas de um diretório para outro ou renomear centenas de arquivos. Normalmente, tarefas como essas poderão levar muito tempo se você executá-las manualmente. Em vez disso, use um script Python.

Vamos começar com um script simples que examina uma árvore de diretório e exibe a estrutura dele.

1. Abra o PowerShell usando o menu **Iniciar** (ícone do Windows no canto inferior esquerdo).

2. Crie um diretório `mkdir python-scripts` para o projeto e, em seguida, abra esse diretório `cd python-scripts`.

3. Crie alguns diretórios para uso com o script de exemplo:

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. Crie alguns arquivos nesses diretórios para uso com o script:

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. Crie um arquivo do Python no diretório python-scripts:

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. Abra o projeto no VS Code inserindo: `code .`

7. Abra a janela Explorador de Arquivos do VS Code inserindo **Ctrl+Shift+E** (ou use o menu para navegar até **Exibir** > **Explorador**) e selecione o arquivo list-directory-contents.py recém-criado. A extensão do Microsoft Python carregará automaticamente um interpretador do Python. Você pode ver qual interpretador foi carregado na parte inferior da janela do VS Code.

    > [!NOTE]
    > O Python é uma linguagem interpretada, o que significa que ele funciona como uma máquina virtual, emulando um computador físico. Há diferentes tipos de interpretadores do Python que você pode usar: Python 2, Python 3, Anaconda, PyPy etc. Para executar o código Python e obter o Python IntelliSense, você precisará informar o VS Code de qual interpretador usar. Recomendamos o uso do interpretador escolhido por padrão pelo VS Code (Python 3, em nosso caso), a menos que você tenha um motivo específico para escolher algo diferente. Para alterar o interpretador do Python, selecione o interpretador atualmente exibido na barra azul na parte inferior da janela do VS Code ou abra a **paleta de comandos** (Ctrl+Shift+P) e insira o comando **Python: Selecionar Interpretador**. Isso exibirá uma lista dos interpretadores do Python atualmente instalados. [Saiba mais sobre a configuração de ambientes do Python](https://code.visualstudio.com/docs/python/environments).

    ![Selecionar um interpretador do Python no VS Code](../images/interpreterselection.gif)

8. Cole o seguinte código no arquivo list-directory-contents.py e, em seguida, selecione **Salvar**:

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. Abra o terminal integrado do VS Code (**CTRL+`** , usando o caractere de acento grave) e insira o diretório src em que você acabou de salvar o script Python:

    ```powershell
    cd src
    ```

10. Execute o script no PowerShell com:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    Você deverá ver uma saída parecida com esta:

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. Use o Python para imprimir essa saída do diretório do sistema de arquivos para seu próprio arquivo de texto inserindo este comando diretamente no terminal do PowerShell: `python3 list-directory-contents.py > food-directory.txt`

Parabéns! Você acabou de escrever um script automatizado de administração de sistemas que lê o diretório e os arquivos criados e usa o Python para exibir e, em seguida, imprimir a estrutura de diretório no próprio arquivo de texto.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Script de exemplo para modificar todos os arquivos de um diretório

Este exemplo usa os arquivos e os diretórios recém-criados, renomeando cada um dos arquivos com a adição da data da última modificação do arquivo ao início do nome do arquivo.

1. Na pasta **src** do diretório **python-scripts**, crie um arquivo do Python para o script:

    ```powershell
    new-item update-filenames.py
    ```

2. Abra o arquivo update-filenames.py, cole o seguinte código no arquivo e salve-o:

    > [!NOTE]
    > os.getmtime retorna um carimbo de data/hora em tiques, o que não é facilmente legível. Ele precisa ser convertido em uma cadeia de caracteres de datetime padrão primeiro.

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. Teste o script update-filenames.py executando-o – `python3 update-filenames.py` – e, em seguida, executando o script list-directory-contents.py novamente: `python3 list-directory-contents.py`

4. Você deverá ver uma saída parecida com esta:

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
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

5. Use o Python para imprimir os novos nomes de diretório do sistema de arquivos com o carimbo de data/hora da última modificação em seu próprio arquivo de texto digitando este comando diretamente no terminal do PowerShell: `python3 list-directory-contents.py > food-directory-last-modified.txt`

Esperamos que tenha aprendido algumas coisas divertidas sobre o uso de scripts Python para automatizar tarefas básicas de administração de sistemas. É claro que há muito mais para aprender, mas esperamos que isso tenha ajudado você a ter um bom começo. Compartilhamos abaixo alguns recursos adicionais para que você continue aprendendo.

## <a name="additional-resources"></a>Recursos adicionais

- [Documentos do Python: Acesso a arquivos e diretórios](https://docs.python.org/3.7/library/filesys.html): Documentação do Python sobre como trabalhar com sistemas de arquivos e usar módulos para ler as propriedades de arquivos, manipular caminhos de forma portátil e criar arquivos temporários.
- [Aprender a usar o Python: ](https://www.learnpython.org/en/String_Formatting)Tutorial de String_Formatting: Mais sobre o uso do operador "%" para formatação de cadeia de caracteres.
- [Dez métodos do sistema de arquivos do Python que você deve saber](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): Artigo um pouco mais aprofundado sobre a manipulação de arquivos e pastas com `os` e `shutil`.
- [O Guia do Mochileiro do Python: Administração de sistemas](https://docs.python-guide.org/scenarios/admin/): Um "guia de opinião" que oferece visões gerais e melhores práticas sobre tópicos relacionados ao Python. Esta seção aborda as estruturas e as ferramentas de administração do sistema. Este guia é hospedado no GitHub e, portanto, você pode enviar problemas e fazer contribuições.
