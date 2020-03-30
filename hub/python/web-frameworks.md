---
title: Desenvolvimento para a Web com o Python no Windows
description: Introdução ao uso do Python para desenvolvimento para a Web no Windows, incluindo a configuração de estruturas como Flask e Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python no Windows, Web para Python com WSL, aplicativo Web Python com Subsistema do Windows para Linux, desenvolvimento para a Web com o Python no Windows, aplicativo Flask no Windows, aplicativo Django no Windows, Web para Python, desenvolvimento para a Web com o Flask no Windows, desenvolvimento para a Web com o Django no Windows, desenvolvimento para a Web no Windows com o Python, desenvolvimento para a Web no VS Code com o Python, extensão do WSL remoto, Ubuntu, WSL, venv, pip, extensão do Microsoft Python, execução do Python no Windows, uso do Python no Windows, criação com o Python no Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 8cbc8343764e4de57bd418ecdb36bd606b037c68
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218476"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Introdução ao uso do Python para desenvolvimento para a Web no Windows

Veja a seguir um guia passo a passo para ajudar você a começar a usar o Python para desenvolvimento para a Web no Windows usando o WSL (Subsistema do Windows para Linux).

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Recomendamos a instalação do Python no WSL ao criar aplicativos Web. Muitos dos tutoriais e muitas das instruções de desenvolvimento para a Web do Python são escritos para usuários do Linux e usam ferramentas de instalação e empacotamento baseadas em Linux. A maioria dos aplicativos Web também é implantada no Linux, portanto, isso garantirá a consistência entre os ambientes de desenvolvimento e produção.

Se você estiver usando o Python para algo que não seja o desenvolvimento para a Web, recomendamos a instalação do Python diretamente no Windows 10 usando a Microsoft Store. O WSL não dá suporte a aplicativos nem áreas de trabalho de GUI (por exemplo, PyGame, Gnome, KDE etc.). Instale e use o Python diretamente no Windows para esses casos. Se você estiver conhecendo o Python agora, confira nosso guia: [Introdução ao uso do Python no Windows para iniciantes](./beginners.md). Se você estiver interessado em automatizar tarefas comuns no sistema operacional, confira nosso guia: [Introdução ao uso do Python no Windows para script e automação](./scripting.md). Para alguns cenários avançados, o ideal é considerar a possibilidade de baixar uma versão específica do Python diretamente em [python.org](https://www.python.org/downloads/windows/) ou instalar uma [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython etc. Só recomendamos isso se você for um programador mais avançado do Python com um motivo específico para escolher uma implementação alternativa.

## <a name="enable-windows-subsystem-for-linux"></a>Habilitar o Subsistema do Windows para Linux

O WSL permite executar um ambiente GNU/Linux – incluindo a maioria das ferramentas, dos utilitários e dos aplicativos de linha de comando – diretamente no Windows, sem modificações e totalmente integrado ao sistema de arquivos do Windows e às suas ferramentas favoritas, como o Visual Studio Code. Antes de habilitar o WSL, verifique se você tem a [versão mais recente do Windows 10](https://www.microsoft.com/software-download/windows10).

Para habilitar o WSL no computador, você precisará:

1. Acessar o menu **Iniciar** (ícone do Windows no canto inferior esquerdo), digitar "Ativar ou desligar recursos do Windows" e selecionar o link para o **Painel de Controle** para abrir o menu pop-up **recursos do Windows**. Encontrar "Subsistema do Windows para Linux" na lista e marcar a caixa de seleção para ativar o recurso.

2. Reinicie seu computador quando solicitado.

## <a name="install-a-linux-distribution"></a>Instalar uma distribuição do Linux

Existem várias distribuições do Linux disponíveis para execução no WSL. Encontre suas favoritas na Microsoft Store e instale-as. Recomendamos começar com o [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), pois é atual, popular e tem um bom suporte.

1. Abra este link do [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), abra a Microsoft Store e selecione **Obter**. *(Esse é um download razoavelmente grande e pode levar algum tempo para ser instalado.)*

2. Após a conclusão do download, selecione **Iniciar** na Microsoft Store ou inicie-o digitando "Ubuntu 18.04 LTS" no menu **Iniciar**.

3. Você deverá criar um nome e uma senha de conta ao executar uma distribuição pela primeira vez. Depois disso, você será automaticamente conectado como esse usuário por padrão. Escolha qualquer nome de usuário e senha. Eles não têm nenhuma influência no seu nome de usuário do Windows.

Verifique a distribuição do Linux que você está usando no momento inserindo `lsb_release -d`. Para atualizar a distribuição do Ubuntu, use `sudo apt update && sudo apt upgrade`. Recomendamos fazer uma atualização regular para garantir que você tenha os pacotes mais recentes. O Windows não cuida automaticamente dessa atualização. Para obter links para outras distribuições do Linux disponíveis na Microsoft Store, métodos de instalação alternativos ou solução de problemas, confira [Guia de Instalação do Subsistema do Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

> [!TIP]
> Considere a possibilidade de experimentar o novo [terminal do Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) se você planeja usar várias linhas de comando (Ubuntu, PowerShell, prompt de comando do Windows etc.) ou se quiser [personalizar seu terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), incluindo texto, cores de tela de fundo, associações de teclas etc.

## <a name="set-up-visual-studio-code"></a>Configurar o Visual Studio Code

Aproveite o [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), o [Linting](https://code.visualstudio.com/docs/python/linting), o [Suporte de depuração](https://code.visualstudio.com/docs/python/debugging), os [Snippets de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) e os [Testes de unidade](https://code.visualstudio.com/docs/python/unit-testing) usando o VS Code. O VS Code se integra perfeitamente ao Subsistema do Windows para Linux, fornecendo um [terminal interno](https://code.visualstudio.com/docs/editor/integrated-terminal) para estabelecer um fluxo de trabalho contínuo entre o editor de códigos e a linha de comando, além de dar suporte ao [Git para controle de versão](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) com os comandos comuns do Git (add, commit, push e pull) incorporados diretamente na interface do usuário.

1. [Baixe e instale o VS Code para Windows](https://code.visualstudio.com). O VS Code também está disponível para Linux, mas o Subsistema do Windows para Linux não dá suporte a aplicativos de GUI e, portanto, precisamos instalá-lo no Windows. Não se preocupe, você ainda poderá fazer a integração à linha de comando e às ferramentas do Linux usando a Extensão WSL – Remoto.

2. Instale a [Extensão WSL – Remoto](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) no VS Code. Isso permite que você use o WSL como o ambiente de desenvolvimento integrado e cuidará da compatibilidade e dos caminhos para você. [Saiba mais](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Se você já tiver o VS Code instalado, precisará verificar se tem a [versão de maio 1.35](https://code.visualstudio.com/updates/v1_35) ou posterior para instalar a [Extensão do WSL Remoto](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Não recomendamos o uso do WSL no VS Code sem a extensão do WSL Remoto, pois você perderá o suporte de preenchimento automático, depuração, linting etc. Curiosidade: Essa extensão do WSL é instalada em $HOME/.vscode-server/extensions.

## <a name="create-a-new-project"></a>Criar um novo projeto

Vamos criar um diretório de projeto no sistema de arquivos do Linux (Ubuntu) e, em seguida, trabalharemos com aplicativos e ferramentas do Linux usando o VS Code.

1. Feche o VS Code e abra o Ubuntu 18.04 (a linha de comando do WSL) acessando o menu **Iniciar** (ícone do Windows no canto inferior esquerdo) e digitando: "Ubuntu 18.04".

2. Na linha de comando do Ubuntu, navegue até o local em que deseja colocar o projeto e crie um diretório para ele: `mkdir HelloWorld`.

![Terminal do Ubuntu](../images/ubuntu-terminal.png)

> [!TIP]
> Um ponto importante a ser lembrado ao usar o WSL (Subsistema do Windows para Linux) é que **agora você está trabalhando entre dois sistemas de arquivos diferentes**: 1) o sistema de arquivos do Windows e 2) o sistema de arquivos do Linux (WSL), que é o Ubuntu em nosso exemplo. Você precisará prestar atenção no local de instalação dos pacotes e armazenamento dos arquivos. Você pode instalar uma versão de uma ferramenta ou um pacote no sistema de arquivos do Windows e uma versão completamente diferente no sistema de arquivos do Linux. A atualização da ferramenta no sistema de arquivos do Windows não terá nenhum efeito na ferramenta no sistema de arquivos do Linux e vice-versa. O WSL monta as unidades fixas no computador na pasta `/mnt/<drive>` da distribuição do Linux. Por exemplo, a unidade C: do Windows está montada em `/mnt/c/`. Acesse seus arquivos do Windows por meio do terminal do Ubuntu e use aplicativos e ferramentas do Linux nesses arquivos e vice-versa. Recomendamos trabalhar no sistema de arquivos do Linux para o desenvolvimento na Web com o Python, considerando que grande parte das ferramentas da Web foi originalmente escrita para o Linux e implantada em um ambiente de produção do Linux. Isso também evita a mistura de semântica do sistema de arquivos (como o Windows não diferenciando maiúsculas de minúsculas em relação aos nomes de arquivos). Dito isso, o WSL agora dá suporte à alternância entre os sistemas de arquivos do Linux e do Windows, de modo que você possa hospedar seus arquivos em um deles. [Saiba mais](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). Também estamos animados em anunciar que o [WSL2 estará disponível em breve para o Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) e oferecerá excelentes melhorias. [Experimente-o agora no Windows Insider build 18917](https://docs.microsoft.com/windows/wsl/wsl2-install).

## <a name="install-python-pip-and-venv"></a>Instalar o Python, o pip e o venv

O Ubuntu 18.04 LTS é fornecido com o Python 3.6 já instalado, mas não vem com alguns dos módulos que você pode esperar obter com outras instalações do Python. Ainda precisaremos instalar o **pip**, o gerenciador de pacotes padrão para o Python, e o **venv**, o módulo padrão usado para criar e gerenciar ambientes virtuais leves.  

1. Confirme se o Python3 já está instalado abrindo o terminal do Ubuntu e inserindo `python3 --version`. Isso deverá retornar o número de versão do Python. Caso precise atualizar sua versão do Python, primeiro, atualize sua versão do Ubuntu inserindo `sudo apt update && sudo apt upgrade` e, em seguida, atualize o Python usando `sudo apt upgrade python3`.

2. Instale o **pip** inserindo `sudo apt install python3-pip`. O pip permite que você instale e gerencie pacotes adicionais que não fazem parte da biblioteca padrão do Python.

3. Instale o **venv** inserindo `sudo apt install python3-venv`.

## <a name="create-a-virtual-environment"></a>Criar um ambiente virtual

O uso de ambientes virtuais é uma melhor prática recomendada para projetos de desenvolvimento do Python. Ao criar um ambiente virtual, você pode isolar suas ferramentas de projeto e evitar conflitos de controle de versão com ferramentas para seus outros projetos. Por exemplo, você pode manter um projeto Web mais antigo que exija a estrutura da Web Django 1.2, mas um novo projeto interessante surge usando o Django 2.2. Se você atualizar o Django globalmente, fora de um ambiente virtual, poderá ter alguns problemas de controle de versão posteriormente. Além de evitar conflitos acidentais de controle de versão, os ambientes virtuais permitem que você instale e gerencie pacotes sem privilégios administrativos.

1. Abra o terminal e, na pasta do projeto *HelloWorld*, use o seguinte comando para criar um ambiente virtual chamado **.venv**: `python3 -m venv .venv`.

2. Para ativar o ambiente virtual, insira `source .venv/bin/activate`. Se isso funcionou, você deve ver **(.venv)** antes do prompt de comando. Agora você tem um ambiente autossuficiente pronto para codificação e instalação de pacotes. Quando terminar de trabalhar com o ambiente virtual, insira o seguinte comando para desativá-lo: `deactivate`.

    ![Criar um ambiente virtual](../images/wsl-venv.png)

> [!TIP]
> Recomendamos a criação do ambiente virtual dentro do diretório no qual você planeja ter o projeto. Como cada projeto deve ter seu próprio diretório separado, cada um terá seu próprio ambiente virtual e, portanto, não há necessidade de nomes exclusivos. Nossa sugestão é usar o nome **.venv** para seguir a convenção do Python. Algumas ferramentas (como o pipenv) também usarão esse nome como padrão se você instalá-lo no diretório do projeto. Você não deseja usar **.env**, pois isso entra em conflito com os arquivos de definição de variável de ambiente. Em geral, não recomendamos usar nomes sem um ponto à esquerda, pois você não precisa que `ls` lembre você constantemente de que o diretório existe. Também recomendamos a adição de **.venv** ao arquivo .gitignore. (Este é [modelo de gitignore padrão do GitHub para Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) para referência.) Para obter mais informações sobre como trabalhar com ambientes virtuais no VS Code, confira [Como usar ambientes do Python no VS Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Abrir uma janela do WSL Remoto

O VS Code usa a Extensão do WSL Remoto (instalada anteriormente) para tratar o subsistema Linux como um servidor remoto. Isso permite que você use o WSL como o ambiente de desenvolvimento integrado. [Saiba mais](https://code.visualstudio.com/docs/remote/wsl). 

1. Abra a pasta do projeto no VS Code do terminal do Ubuntu inserindo `code .` (o "." instrui o VS Code a abrir a pasta atual).

2. Um alerta de segurança será exibido em um pop-up do Windows Defender. Selecione "Permitir acesso". Quando o VS Code for aberto, você deverá ver o indicador de Host de Conexão Remota, no canto inferior esquerdo, informando que a edição está sendo feita no **WSL: Ubuntu-18.04**.

    ![Indicador de Host de Conexão Remota do VS Code](../images/wsl-remote-extension.png)

3. Feche o terminal do Ubuntu. Mais adiante, usaremos o terminal do WSL integrado ao VS Code.

4. Abra o terminal do WSL no VS Code pressionando **CTRL+`** (usando o caractere de acento grave) ou selecionando **Exibir** > **Terminal**. Isso abrirá uma linha de comando do Bash (WSL) aberta no caminho da pasta do projeto que você criou no terminal do Ubuntu.

    ![VS Code com o terminal do WSL](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Instalar a extensão do Microsoft Python

Você precisará instalar as extensões do VS Code para o WSL Remoto. As extensões já instaladas localmente no VS Code não estarão disponíveis automaticamente. [Saiba mais](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. Abra a janela Extensões do VS Code inserindo **Ctrl+Shift+X** (ou use o menu para navegar até **Exibir** > **Extensões**).

2. Na caixa **Pesquisar Extensões no Marketplace** no canto superior, insira:  **Python**.

3. Encontre a extensão **Python (ms-python.python) da Microsoft** e selecione o botão verde **Instalar**.

4. Quando a instalação da extensão for concluída, você precisará selecionar o botão azul **Recarregamento Necessário**. Isso recarregará o VS Code e exibirá uma seção **WSL: UBUNTU-18.04 – Instalado** na janela Extensões do VS Code, mostrando que você instalou a extensão do Python.

## <a name="run-a-simple-python-program"></a>Executar um programa simples do Python

O Python é uma linguagem interpretada e dá suporte a diferentes tipos de interpretadores (Python2, Anaconda, PyPy etc.). O VS Code deve usar como padrão o interpretador associado ao projeto. Se você tiver um motivo para alterá-lo, selecione o interpretador atualmente exibido na barra azul na parte inferior da janela do VS Code ou abra a **paleta de comandos** (Ctrl+Shift+P) e insira o comando **Python: Selecionar Interpretador**. Isso exibirá uma lista dos interpretadores do Python atualmente instalados. [Saiba mais sobre a configuração de ambientes do Python](https://code.visualstudio.com/docs/python/environments).

Vamos criar e executar um programa simples do Python como um teste e verificar se selecionamos o interpretador do Python correto.

1. Abra a janela do Explorador de Arquivos do VS Code inserindo **Ctrl+Shift+E** (ou use o menu para navegar até **Exibir** > **Explorador**).

2. Se ele ainda não estiver aberto, abra o terminal integrado do WSL inserindo **Ctrl+Shift+`** e verifique se a pasta do projeto **HelloWorld** do Python está selecionada.

3. Crie um arquivo do Python inserindo `touch test.py`. Você deverá ver o arquivo recém-criado exibido na janela do Explorador nas pastas .venv e .vscode que já estão no diretório do projeto.

4. Selecione o arquivo **test.py** recém-criado na janela do Explorador para abri-lo no VS Code. Como o .py no nome de arquivo informa o VS Code de que se trata de um arquivo do Python, a extensão do Python carregada anteriormente escolherá e carregará automaticamente um interpretador do Python que você verá na parte inferior da janela do VS Code.

    ![Selecionar um interpretador do Python no VS Code](../images/interpreterselection.gif)

5. Cole este código Python no arquivo test.py e, em seguida, salve o arquivo (Ctrl+S): 

    ```python
    print("Hello World")
    ```

6. Para executar o programa "Olá, Mundo" do Python recém-criado, selecione o arquivo **test.py** na janela do Explorador do VS Code e, em seguida, clique com o botão direito do mouse no arquivo para exibir um menu de opções. Selecione **Executar o Arquivo Python no Terminal**. Como alternativa, na janela do terminal integrado do WSL, insira `python test.py` para executar o programa "Olá, Mundo". O interpretador do Python imprimirá "Olá, Mundo" na janela do terminal.

Parabéns! Você está pronto para criar e executar programas do Python. Agora, vamos tentar criar um aplicativo Olá, Mundo com duas das estruturas da Web mais populares do Python: Flask e Django.

## <a name="hello-world-tutorial-for-flask"></a>Tutorial de Olá, Mundo para Flask

O [Flask](http://flask.pocoo.org/) é uma estrutura de aplicativo Web para o Python. Neste breve tutorial, você criará um pequeno aplicativo Flask "Olá, Mundo" usando o VS Code e o WSL.

1. Abra o Ubuntu 18.04 (a linha de comando do WSL) acessando o menu **Iniciar** (ícone do Windows no canto inferior esquerdo) e digitando: "Ubuntu 18.04".

2. Crie um diretório `mkdir HelloWorld-Flask` para o projeto e, em seguida, `cd HelloWorld-Flask` para inserir o diretório.

3. Crie um ambiente virtual para instalar as ferramentas do projeto: `python3 -m venv .venv`

4. Abra o projeto **HelloWorld-Flask** no VS Code inserindo o comando: `code .`

5. No VS Code, abra o terminal integrado do WSL (também conhecido como Bash) inserindo **Ctrl+Shift+`** (a pasta do projeto **HelloWorld-Flask** já deverá estar selecionada). *Feche a linha de comando do Ubuntu, pois trabalharemos no terminal do WSL integrado ao VS Code mais adiante.*

6. Ative o ambiente virtual criado na etapa nº 3 usando o terminal do Bash no VS Code: `source .venv/bin/activate`. Se isso funcionou, você deve ver (.venv) antes do prompt de comando.

7. Instale o Flask no ambiente virtual inserindo `python3 -m pip install flask`. Verifique se ele está instalado inserindo `python3 -m flask --version`.

8. Crie um arquivo para o código Python: `touch app.py`

9. Abra o arquivo **app.py** no Explorador de Arquivos do VS Code (`Ctrl+Shift+E` e, em seguida, selecione o arquivo app.py). Isso ativará a Extensão do Python para a escolha de um interpretador. Ela deve usar como padrão o **Python 3.6.8 de 64 bits ('.venv': venv)** . Observe que ela também detectou o ambiente virtual.

    ![Ambiente virtual ativado](../images/virtual-environment.png)

10. Em **app.py**, adicione o código para importar o Flask e crie uma instância do objeto Flask:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. Também em **app.py**, adicione uma função que retorna o conteúdo, neste caso, uma cadeia de caracteres simples. Use o decorador **app.route** do Flask para mapear a rota de URL "/" para essa função:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Você pode usar vários decoradores na mesma função, um por linha, dependendo de quantas rotas diferentes você deseja mapear para a mesma função.

12. Salve o arquivo **app.py** (**Ctrl+S**).

13. No terminal, execute o aplicativo inserindo o seguinte comando:

    ```python
    python3 -m flask run
    ```

    Isso executará o servidor de desenvolvimento do Flask. O servidor de desenvolvimento procura **app.py** por padrão. Ao executar o Flask, você deverá ver uma saída semelhante à seguinte:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Abra o navegador da Web padrão na página renderizada, **Ctrl+Clique** na URL http://127.0.0.1:5000/ no terminal. Você deverá ver a seguinte mensagem no navegador:

    ![Olá, Flask!](../images/hello-flask.png)

15. Observe que, quando você visita uma URL como "/", uma mensagem é exibida no terminal de depuração mostrando a solicitação HTTP:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Pare o aplicativo usando **Ctrl+C** no terminal.

> [!TIP]
> Caso deseje usar um nome de arquivo diferente de **app.py**, como **program.py**, defina uma variável de ambiente chamada **FLASK_APP** e defina o valor como o arquivo escolhido. O servidor de desenvolvimento do Flask usa o valor **FLASK_APP** em vez do arquivo padrão **app.py**. Para obter mais informações, confira [Documentação da interface de linha de comando do Flask](http://flask.pocoo.org/docs/1.0/cli/).

Parabéns, você criou um aplicativo Web do Flask usando o Visual Studio Code e o Subsistema do Windows para Linux! Para obter um tutorial mais aprofundado sobre como usar o VS Code e o Flask, confira [Tutorial do Flask no Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Tutorial de Olá, Mundo para Django

O [Django](https://www.djangoproject.com) é uma estrutura de aplicativo Web para o Python. Neste breve tutorial, você criará um pequeno aplicativo Django "Olá, Mundo" usando o VS Code e o WSL.

1. Abra o Ubuntu 18.04 (a linha de comando do WSL) acessando o menu **Iniciar** (ícone do Windows no canto inferior esquerdo) e digitando: "Ubuntu 18.04".

2. Crie um diretório `mkdir HelloWorld-Django` para o projeto e, em seguida, `cd HelloWorld-Django` para inserir o diretório.

3. Crie um ambiente virtual para instalar as ferramentas do projeto: `python3 -m venv .venv`

4. Abra o projeto **HelloWorld-Django** no VS Code inserindo o comando: `code .`

5. No VS Code, abra o terminal integrado do WSL (também conhecido como Bash) inserindo **Ctrl+Shift+`** (a pasta do projeto **HelloWorld-Django** já deverá estar selecionada). *Feche a linha de comando do Ubuntu, pois trabalharemos no terminal do WSL integrado ao VS Code mais adiante.*

6. Ative o ambiente virtual criado na etapa nº 3 usando o terminal do Bash no VS Code: `source .venv/bin/activate`. Se isso funcionou, você deve ver (.venv) antes do prompt de comando.

7. Instale o Django no ambiente virtual com o comando `python3 -m pip install django`. Verifique se ele está instalado inserindo `python3 -m django --version`.

8. Em seguida, execute o seguinte comando para criar o projeto do Django:

    ```bash
    django-admin startproject web_project .
    ```

    O comando `startproject` pressupõe (pelo uso de `.` no final) que a pasta atual seja a pasta do projeto e cria o seguinte dentro dela:

    - `manage.py`: O utilitário administrativo da linha de comando do Django para o projeto. Execute comandos administrativos para o projeto usando `python manage.py <command> [options]`.

    - Uma subpasta chamada `web_project`, que contém os seguintes arquivos:
        - `__init__.py`: um arquivo vazio que informa o Python de que essa pasta é um pacote do Python.
        - `wsgi.py`: um ponto de entrada para servidores Web compatíveis com WSGI para atender ao projeto. Normalmente, esse arquivo é mantido no estado em que se encontra, pois ele fornece os ganchos para servidores Web de produção.
        - `settings.py`: contém configurações para o projeto do Django, que você modificará no decorrer do desenvolvimento de um aplicativo Web.
        - `urls.py`: contém um sumário para o projeto do Django, que você também modificará no curso de desenvolvimento.

9. Para verificar o projeto do Django, inicie o servidor de desenvolvimento do Django usando o comando `python3 manage.py runserver`. O servidor é executado na porta padrão 8000 e você deverá ver uma saída semelhante à seguinte saída na janela do terminal:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Quando você executa o servidor pela primeira vez, ele cria um banco de dados SQLite padrão no arquivo `db.sqlite3`, que é destinado para fins de desenvolvimento, mas pode ser usado em produção para aplicativos Web de baixo volume. Além disso, o servidor Web interno do Django é destinado *apenas* para fins de desenvolvimento local. No entanto, quando você implanta em um host da Web, o Django usa o servidor Web do host. O módulo `wsgi.py` no projeto do Django cuida da conexão com os servidores de produção.

    Caso você deseje usar uma porta diferente da 8000 padrão, especifique o número da porta na linha de comando, como `python3 manage.py runserver 5000`.

10. `Ctrl+click` na URL `http://127.0.0.1:8000/` na Janela de Saída do terminal para abrir o navegador padrão nesse endereço. Se o Django for instalado corretamente e o projeto for válido, você verá uma página padrão. A Janela de Saída de terminal do VS Code também mostra o log do servidor.

11. Quando terminar, feche a janela do navegador e pare o servidor no VS Code usando `Ctrl+C`, conforme indicado na Janela de Saída do terminal.

12. Agora, para criar um aplicativo do Django, execute o comando `startapp` do utilitário administrativo na pasta do projeto (em que `manage.py` reside):

    ```bash
    python3 manage.py startapp hello
    ```

    O comando cria uma pasta chamada `hello` que contém vários arquivos de código e uma subpasta. Deles, com frequência, você trabalha com `views.py` (que contém as funções que definem as páginas no aplicativo Web) e `models.py` (que contém as classes que definem os objetos de dados). A pasta `migrations` é usada pelo utilitário administrativo do Django para gerenciar versões de banco de dados, conforme abordado mais adiante neste tutorial. Também há arquivos `apps.py` (configuração de aplicativo), `admin.py` (para criar uma interface administrativa) e `tests.py` (para testes), que não são abordados aqui.

13. Modifique `hello/views.py` para que ele corresponda ao seguinte código, que cria uma única exibição para a home page do aplicativo:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Crie um arquivo, `hello/urls.py`, com o conteúdo abaixo. O arquivo `urls.py` é o local em que você especifica padrões para rotear diferentes URLs para suas respectivas exibições apropriadas. O código abaixo contém uma rota para mapear a URL raiz do aplicativo (`""`) para a função `views.home` recém-adicionada a `hello/views.py`:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. A pasta `web_project` também contém um arquivo `urls.py`, que é o local em que o roteamento de URL é realmente tratado. Abra `web_project/urls.py` e modifique-o para que ele corresponda ao código a seguir (mantenha os comentários instrutivos se desejar). Esse código efetua pull de `hello/urls.py` do aplicativo usando `django.urls.include`, que mantém as rotas do aplicativo contidas no aplicativo. Essa separação é útil quando um projeto contém vários aplicativos.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Salve todos os arquivos modificados.

17. No terminal do VS Code, execute o servidor de desenvolvimento com `python3 manage.py runserver` e abra um navegador em `http://127.0.0.1:8000/` para ver uma página que renderiza "Olá, Django".

Parabéns, você criou um aplicativo Web do Django usando o VS Code e o Subsistema do Windows para Linux! Para obter um tutorial mais aprofundado sobre como usar o VS Code e o Django, confira [Tutorial do Django no Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Recursos adicionais

- [Tutorial do Python com o VS Code](https://code.visualstudio.com/docs/python/python-tutorial): Um tutorial introdutório do VS Code como um ambiente do Python, principalmente sobre como editar, executar e depurar o código.
- [Suporte ao Git no VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Saiba como usar os conceitos básicos de controle de versão do Git no VS Code.  
- [Saiba mais sobre as atualizações que serão lançadas em breve com o WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-index): Essa nova versão altera a forma como as distribuições do Linux interagem com o Windows, aumentando o desempenho do sistema de arquivos e adicionando compatibilidade total com chamadas do sistema.
- [Como trabalhar com várias distribuições do Linux no Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Saiba como gerenciar várias distribuições do Linux diferentes no computador Windows.
