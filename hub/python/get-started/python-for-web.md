---
title: Desenvolvimento para a Web com Python no Windows
description: Como começar a usar o Python para desenvolvimento para a Web no Windows, incluindo a configuração de estruturas como Flask e Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python no Windows, Python Web com WSL, aplicativo Web Python com subsistema do Windows para Linux, desenvolvimento para a Web do Python no Windows, aplicativo Flask no Windows, Django app on Windows, Python Web, Flask web dev no Windows, Django dev na Web no Windows, Windows Web dev com Python, vs Code desenvolvimento da Web Python, extensão WSL remota, Ubuntu, WSL, venv, Pip, extensão Microsoft Python, executar Python no Windows, usar python no Windows, criar com Python no Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fa6da9f5151d9457aafd255c9d10c91e3d219cee
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959081"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Comece a usar o Python para desenvolvimento para a Web no Windows

Veja a seguir um guia passo a passo para ajudá-lo a começar a usar o Python para desenvolvimento para a Web no Windows, usando o subsistema do Windows para Linux (WSL).

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

É recomendável instalar o Python no WSL ao criar aplicativos Web. Muitos dos tutoriais e instruções do desenvolvimento para a Web do Python são escritos para usuários do Linux e usam ferramentas de instalação e empacotamento baseadas em Linux. A maioria dos aplicativos Web também é implantada no Linux, portanto, isso garantirá a consistência entre seus ambientes de desenvolvimento e produção.

Se você estiver usando o Python para algo diferente do desenvolvimento para a Web, recomendamos que instale o Python diretamente no Windows 10 usando o Microsoft Store. O WSL não dá suporte a áreas de trabalho de GUI ou aplicativos (como PyGame, GNOME, KDE, etc.). Instale e use o Python diretamente no Windows para esses casos. Se você for novo no Python, consulte nosso guia: Comece [a usar o Python no Windows para iniciantes](./python-for-education.md). Se você estiver interessado em automatizar tarefas comuns em seu sistema operacional, consulte nosso guia: Comece [a usar o Python no Windows para scripts e automação](./python-for-scripting.md). Para alguns cenários avançados, talvez você queira baixar uma versão específica do Python diretamente do [Python.org](https://www.python.org/downloads/windows/) ou considerar a instalação de uma [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython etc. Só recomendamos isso se você for um programador de Python mais avançado com um motivo específico para escolher uma implementação alternativa.

## <a name="enable-windows-subsystem-for-linux"></a>Habilitar o subsistema do Windows para Linux

O WSL permite que você execute um ambiente GNU/Linux, incluindo a maioria das ferramentas de linha de comando, utilitários e aplicativos, diretamente no Windows, sem modificações e totalmente integradas ao sistema de arquivos do Windows e a ferramentas favoritas, como Visual Studio Code. Antes de habilitar o WSL, verifique se você tem a [versão mais recente do Windows 10](https://www.microsoft.com/software-download/windows10).

Para habilitar o WSL em seu computador, você precisa:

1. Vá para o menu **Iniciar** (ícone inferior esquerdo do Windows), digite "ativar ou desativar recursos do Windows" e selecione o link para o **painel de controle** para abrir o menu pop-up **recursos do Windows** . Localize "subsistema do Windows para Linux" na lista e marque a caixa de seleção para ativar o recurso.

2. Reinicie o computador quando solicitado.

## <a name="install-a-linux-distribution"></a>Instalar uma distribuição do Linux

Há várias distribuições do Linux disponíveis para execução no WSL. Você pode encontrar e instalar seu favorito no Microsoft Store. É recomendável começar com o [Ubuntu 18, 4 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , pois ele é atual, popular e bem suportado.

1. Abra este link do [Ubuntu 18, 4 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , abra o Microsoft Store e selecione **obter**. *(Esse é um download razoavelmente grande e pode levar algum tempo para ser instalado.)*

2. Após a conclusão do download, selecione **Iniciar** no Microsoft Store ou iniciar digitando "Ubuntu 18, 4 LTS" no menu **Iniciar** .

3. Você será solicitado a criar um nome de conta e uma senha ao executar a distribuição pela primeira vez. Depois disso, você será automaticamente conectado como esse usuário por padrão. Você pode escolher qualquer nome de usuário e senha. Eles não têm nenhuma influência sobre seu nome de usuário do Windows.

Você pode verificar a distribuição do Linux que você está usando no momento digitando: `lsb_release -d`. Para atualizar sua distribuição do Ubuntu, use `sudo apt update && sudo apt upgrade`:. É recomendável atualizar regularmente para garantir que você tenha os pacotes mais recentes. O Windows não manipula automaticamente essa atualização. Para obter links para outras distribuições do Linux disponíveis no Microsoft Store, métodos de instalação alternativos ou solução de problemas, consulte [Guia de instalação do subsistema do Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="set-up-visual-studio-code"></a>Configurar Visual Studio Code

Aproveite o [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), o [despanoe](https://code.visualstudio.com/docs/python/linting), o [suporte de depuração](https://code.visualstudio.com/docs/python/debugging), os [trechos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) e os [testes de unidade](https://code.visualstudio.com/docs/python/unit-testing) usando vs Code. O VS Code se integra perfeitamente com o subsistema do Windows para Linux, fornecendo um [terminal interno](https://code.visualstudio.com/docs/editor/integrated-terminal) para estabelecer um fluxo de trabalho contínuo entre o editor de código e a linha de comando, além de oferecer suporte ao [git para controle de versão](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) com git comum comandos (adicionar, confirmar, enviar por push, pull) criados diretamente na interface do usuário.

1. [Baixe e instale o vs Code para Windows](https://code.visualstudio.com). O VS Code também está disponível para Linux, mas o subsistema do Windows para Linux não oferece suporte a aplicativos de GUI, portanto, precisamos instalá-lo no Windows. Não se preocupe, você ainda poderá se integrar à sua linha de comando e às ferramentas do Linux usando a extensão WSL remota.

2. Instale a [extensão Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) no vs Code. Isso permite que você use o WSL como seu ambiente de desenvolvimento integrado e irá manipular a compatibilidade e o caminho para você. [Saiba mais](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Se você já tiver VS Code instalado, você precisará garantir que o [1,35 pode ser liberado](https://code.visualstudio.com/updates/v1_35) ou posterior para instalar a [extensão WSL remota](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Não recomendamos o uso de WSL em VS Code sem a extensão WSL remota, pois você perderá o suporte para preenchimento automático, depuração, refiapo, etc. Fato divertido: Essa extensão WSL é instalada em $HOME/.vscode-Server/Extensions.

## <a name="create-a-new-project"></a>Criar um novo projeto

Vamos criar um novo diretório de projeto em nosso sistema de arquivos Linux (Ubuntu) que, em seguida, trabalharemos com aplicativos e ferramentas do Linux usando VS Code.

1. Feche VS Code e abra o Ubuntu 18, 4 (sua linha de comando do WSL) acessando o menu **Iniciar** (ícone inferior esquerdo do Windows) e digitando: "Ubuntu 18, 4".

2. Na linha de comando do Ubuntu, navegue até o local em que deseja colocar o projeto e crie um diretório para ele `mkdir HelloWorld`:.

![Terminal do Ubuntu](../../images/ubuntu-terminal.png)

> [!TIP]
> Um ponto importante a ser lembrado ao usar o subsistema do Windows para Linux (WSL) é que **agora você está trabalhando entre dois sistemas de arquivos diferentes**: 1) o sistema de arquivos do Windows e 2) seu sistema de arquivos do Linux (WSL), que é o Ubuntu para nosso exemplo. Você precisará prestar atenção para onde instalar pacotes e armazenar arquivos. Você pode instalar uma versão de uma ferramenta ou pacote no sistema de arquivos do Windows e uma versão completamente diferente no sistema de arquivos do Linux. Atualizar a ferramenta no sistema de arquivos do Windows não terá nenhum efeito sobre a ferramenta no sistema de arquivos do Linux e vice-versa. O WSL monta as unidades fixas em seu computador na pasta<drive> /mnt/em sua distribuição do Linux. Por exemplo, a unidade C: do Windows é montada em `/mnt/c/`. Você pode acessar seus arquivos do Windows por meio do terminal Ubuntu e usar aplicativos e ferramentas do Linux nesses arquivos e vice-versa. É recomendável trabalhar no sistema de arquivos Linux para desenvolvimento na Web do Python, uma vez que grande parte das ferramentas da Web é originalmente escrita para Linux e implantada em um ambiente de produção Linux. Ele também evita a mistura de semânticas de sistema de arquivos (como o Windows que não diferencia maiúsculas de minúsculas em relação aos nomes de arquivo). Dito isso, o WSL agora dá suporte ao salto entre os sistemas de arquivos do Linux e do Windows, para que você possa hospedar seus arquivos em qualquer um deles. [Saiba mais](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). Também estamos empolgados em compartilhar que o [WSL2 estará disponível em breve para o Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) e oferecerá algumas ótimas melhorias. Você pode [experimentá-lo agora no Build de 18917 do Windows](https://docs.microsoft.com/windows/wsl/wsl2-install).

## <a name="install-python-pip-and-venv"></a>Instalar Python, Pip e venv

O Ubuntu 18, 4 LTS vem com o Python 3,6 já instalado, mas não vem com alguns dos módulos que você pode esperar obter com outras instalações do Python. Ainda precisaremos instalar o **Pip**, o Gerenciador de pacotes padrão para Python e o **venv**, o módulo padrão usado para criar e gerenciar ambientes virtuais leves.  

1. Confirme se o Python3 já está instalado abrindo o terminal Ubuntu e inserindo `python3 --version`:. Isso deve retornar o número de versão do Python. Se você precisar atualizar sua versão do Python, primeiro atualize sua versão do Ubuntu digitando: `sudo apt update && sudo apt upgrade`e, em seguida, `sudo apt upgrade python3`atualize o Python usando.

2. Instale o **Pip** digitando `sudo apt install python3-pip`:. Pip permite que você instale e gerencie pacotes adicionais que não fazem parte da biblioteca padrão do Python.

3. Instale o **venv** digitando `sudo apt install python3-venv`:.

## <a name="create-a-virtual-environment"></a>Criar um ambiente virtual

O uso de ambientes virtuais é uma prática recomendada para projetos de desenvolvimento do Python. Ao criar um ambiente virtual, você pode isolar suas ferramentas de projeto e evitar conflitos de controle de versão com ferramentas para seus outros projetos. Por exemplo, você pode estar mantendo um projeto Web mais antigo que requer a estrutura da Web do Django 1,2, mas um novo projeto empolgante vem usando o Django 2,2. Se você atualizar o Django globalmente, fora de um ambiente virtual, poderá encontrar alguns problemas de controle de versão posteriormente. Além de evitar conflitos acidentais de controle de versão, os ambientes virtuais permitem que você instale e gerencie pacotes sem privilégios administrativos.

1. Abra seu terminal e, dentro da pasta do projeto *HelloWorld* , use o comando a seguir para criar um ambiente virtual chamado **. venv**: `python3 -m venv .venv`.

2. Para ativar o ambiente virtual, digite: `source .venv/bin/activate`. Se funcionou, você deve ver **(. venv)** antes do prompt de comando. Agora você tem um ambiente autônomo pronto para escrever código e instalar pacotes. Quando tiver concluído o seu ambiente virtual, digite o seguinte comando para desativá-lo `deactivate`:.

    ![Criar um ambiente virtual](../../images/wsl-venv.png)

> [!TIP]
> É recomendável criar o ambiente virtual dentro do diretório no qual você planeja ter seu projeto. Como cada projeto deve ter seu próprio diretório separado, cada um terá seu próprio ambiente virtual, portanto, não há necessidade de nomes exclusivos. Nossa sugestão é usar o name **. venv** para seguir a Convenção do Python. Algumas ferramentas (como pipenv) também são padrão para esse nome se você instalar no diretório do projeto. Você não deseja usar **. env** , pois isso entra em conflito com os arquivos de definição de variável de ambiente. Em geral, não recomendamos nomes não-ponto, pois você não precisa `ls` se lembrar constantemente de que o diretório existe. Também recomendamos adicionar **. venv** ao seu arquivo. gitignore. (Aqui está o [modelo de gitignore padrão do GitHub](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) para o Python para referência.) Para obter mais informações sobre como trabalhar com ambientes virtuais no VS Code, consulte [usando ambientes Python no vs Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Abrir uma janela WSL-Remote

VS Code usa a extensão Remote-WSL (instalada anteriormente) para tratar seu subsistema Linux como um servidor remoto. Isso permite que você use o WSL como seu ambiente de desenvolvimento integrado. [Saiba mais](https://code.visualstudio.com/docs/remote/wsl). 

1. Abra a pasta do projeto no vs Code do seu terminal Ubuntu digitando `code .` : (o "." informa vs Code para abrir a pasta atual).

2. Um alerta de segurança será exibido no Windows Defender, selecione "permitir acesso". Quando vs Code abrir, você deverá ver o indicador do host de conexão remota, no canto inferior esquerdo, informando que você está editando **em WSL: Ubuntu-18, 4**.

    ![VS Code indicador de host de conexão remota](../../images/wsl-remote-extension.png)

3. Feche seu terminal Ubuntu. Avançando, usaremos o terminal WSL integrado ao VS Code.

4. Abra o terminal WSL no vs Code pressionando **Ctrl + '** (usando o caractere de acento grave) ou selecionando **Exibir** > **terminal**. Isso abrirá uma linha de comando bash (WSL) aberta no caminho da pasta do projeto que você criou em seu terminal Ubuntu.

    ![VS Code com terminal WSL](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Instalar a extensão do Microsoft Python

Você precisará instalar todas as extensões de VS Code para seu WSL remoto. As extensões já instaladas localmente no VS Code não estarão disponíveis automaticamente. [Saiba mais](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. Abra a janela extensões de vs Code digitando **Ctrl + Shift + X** (ou use o menu para navegar até**as extensões**de **exibição** > ).

2. Na caixa **extensões de pesquisa principais no Marketplace** , digite:  **Python**.

3. Localize a extensão **Python (MS-Python. Python) pela Microsoft** e selecione o botão de **instalação** verde.

4. Depois que a instalação for concluída, será necessário selecionar o botão azul **recarregar necessário** . Isso recarregará vs Code e exibirá **um WSL: Ubuntu-18, 4-instalado** na janela extensões de vs Code mostrando que você instalou a extensão do Python.

## <a name="run-a-simple-python-program"></a>Executar um programa Python simples

O Python é uma linguagem interpretada e dá suporte a diferentes tipos de intérpretes (Python2, Anaconda, PyPy, etc.). VS Code deve padronizar para o intérprete associado ao seu projeto. Se você tiver um motivo para alterá-lo, selecione o intérprete atualmente exibido na barra azul na parte inferior da janela de vs Code ou abra a **paleta de comandos** (Ctrl + Shift + P) e insira **o comando Python: Selecione intérprete**. Isso exibirá uma lista dos interpretadores do Python que você instalou atualmente. [Saiba mais sobre a configuração de ambientes do Python](https://code.visualstudio.com/docs/python/environments).

Vamos criar e executar um programa Python simples como um teste e garantir que tenhamos o intérprete do Python correto selecionado.

1. Abra a janela vs Code explorador de arquivos digitando **Ctrl + Shift + E** (ou use o menu para navegar até o**Gerenciador**de **exibição** > ).

2. Se ainda não estiver aberto, abra o terminal WSL integrado digitando **Ctrl + Shift +** e verifique se a pasta do projeto do Python do **HelloWorld** está selecionada.

3. Crie um arquivo Python digitando: `touch test.py`. Você deve ver o arquivo que acabou de criar aparecer na janela do Gerenciador nas pastas. venv e. vscode que já estão no diretório do projeto.

4. Selecione o arquivo **Test.py** que você acabou de criar na janela do Explorer para abri-lo no vs Code. Como o. py em nosso nome de arquivo informa VS Code de que se trata de um arquivo Python, a extensão do Python carregada anteriormente escolherá e carregará automaticamente um intérprete do Python que você verá na parte inferior da janela do VS Code.

    ![Selecionar interpretador do Python no VS Code](../../images/interpreterselection.gif)

5. Cole este código Python em seu arquivo test.py e salve o arquivo (Ctrl + S): 

    ```python
    print("Hello World")
    ```

6. Para executar o programa "Olá, Mundo" do Python que acabamos de criar, selecione o arquivo **Test.py** na janela do vs Code Explorer e clique com o botão direito do mouse no arquivo para exibir um menu de opções. Selecione **Executar arquivo Python no terminal**. Como alternativa, na janela do terminal WSL integrado, digite: `python test.py` para executar o programa "Olá, mundo". O intérprete do Python imprimirá "Olá, Mundo" na janela do seu terminal.

Parabéns. Você está pronto para criar e executar programas Python! Agora, vamos tentar criar um aplicativo Olá, Mundo com duas das estruturas da Web mais populares do Python: Flask e Django.

## <a name="hello-world-tutorial-for-flask"></a>Tutorial de Olá, Mundo para Flask

[Flask](http://flask.pocoo.org/) é uma estrutura de aplicativo Web para Python. Neste breve tutorial, você criará um pequeno aplicativo Flask "Olá, Mundo" usando VS Code e WSL.

1. Abra o Ubuntu 18, 4 (sua linha de comando do WSL) acessando o menu **Iniciar** (ícone inferior esquerdo do Windows) e digitando: "Ubuntu 18, 4".

2. Crie um diretório para seu projeto: `mkdir HelloWorld-Flask`, em `cd HelloWorld-Flask` seguida, para inserir o diretório.

3. Crie um ambiente virtual para instalar as ferramentas de projeto:`python3 -m venv .venv`

4. Abra o projeto **HelloWorld-Flask** no vs Code digitando o comando:`code .`

5. Dentro de VS Code, abra seu terminal WSL integrado (também conhecido como bash) digitando **Ctrl + Shift + '** (sua pasta de projeto **HelloWorld-Flask** já deve estar selecionada). *Feche a linha de comando do Ubuntu, pois trabalharemos no terminal WSL integrado com VS Code avançando.*

6. Ative o ambiente virtual que você criou na etapa #3 usando seu terminal Bash no VS Code: `source .venv/bin/activate`. Se funcionou, você deve ver (. venv) antes do prompt de comando.

7. Instale o Flask no ambiente virtual digitando: `python3 -m pip install flask`. Verifique se ele está instalado digitando: `python3 -m flask --version`.

8. Crie um novo arquivo para seu código Python:`touch app.py`

9. Abra o arquivo **app.py** no explorador de arquivos do vs Code`Ctrl+Shift+E`(, em seguida, selecione o arquivo app.py). Isso ativará a extensão do Python para escolher um intérprete. Ele deve padrão para **Python 3.6.8 64-bit ('. venv ': venv)** . Observe que ele também detectou seu ambiente virtual.

    ![Ambiente virtual ativado](../../images/virtual-environment.png)

10. No **app.py**, adicione o código para importar Flask e crie uma instância do objeto Flask:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. Também em **app.py**, adicione uma função que retorne o conteúdo, neste caso, uma cadeia de caracteres simples. Use o decorador **app. Route** do Flask para mapear a rota de URL "/" para essa função:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Você pode usar vários decoradores na mesma função, um por linha, dependendo de quantas rotas diferentes você deseja mapear para a mesma função.

12. Salve o arquivo **app.py** (**Ctrl + S**).

13. No terminal, execute o aplicativo digitando o seguinte comando:

    ```python
    python3 -m flask run
    ```

    Isso executa o servidor de desenvolvimento Flask. O servidor de desenvolvimento procura por **app.py** por padrão. Ao executar o Flask, você deverá ver uma saída semelhante à seguinte:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Abra o navegador da Web padrão na página renderizada, **Ctrl + clique** na http://127.0.0.1:5000/ URL no terminal. Você deverá ver a seguinte mensagem em seu navegador:

    ![Olá, Flask!](../../images/hello-flask.png)

15. Observe que quando você visita uma URL como "/", uma mensagem é exibida no terminal de depuração mostrando a solicitação HTTP:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Pare o aplicativo usando **Ctrl + C** no terminal.

> [!TIP]
> Se você quiser usar um nome de arquivo diferente de **app.py**, como **Program.py**, defina uma variável de ambiente chamada **FLASK_APP** e defina seu valor para o arquivo escolhido. Em seguida, o servidor de desenvolvimento do Flask usa o valor de **FLASK_APP** em vez do arquivo padrão **app.py**. Para obter mais informações, consulte a [documentação da interface de linha de comando do Flask](http://flask.pocoo.org/docs/1.0/cli/).

Parabéns, você criou um aplicativo Web Flask usando o Visual Studio Code e o subsistema do Windows para Linux! Para obter um tutorial mais aprofundado usando VS Code e Flask, consulte o [tutorial do Flask em Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Tutorial de Olá, Mundo para Django

[Django](https://www.djangoproject.com) é uma estrutura de aplicativo Web para Python. Neste breve tutorial, você criará um pequeno aplicativo Django "Olá, Mundo" usando VS Code e WSL.

1. Abra o Ubuntu 18, 4 (sua linha de comando do WSL) acessando o menu **Iniciar** (ícone inferior esquerdo do Windows) e digitando: "Ubuntu 18, 4".

2. Crie um diretório para seu projeto: `mkdir HelloWorld-Django`, em `cd HelloWorld-Django` seguida, para inserir o diretório.

3. Crie um ambiente virtual para instalar as ferramentas de projeto:`python3 -m venv .venv`

4. Abra o projeto **HelloWorld-Django** no vs Code digitando o comando:`code .`

5. Dentro de VS Code, abra seu terminal WSL integrado (também conhecido como bash) digitando **Ctrl + Shift + '** (sua pasta de projeto **HelloWorld-Django** já deve estar selecionada). *Feche a linha de comando do Ubuntu, pois trabalharemos no terminal WSL integrado com VS Code avançando.*

6. Ative o ambiente virtual que você criou na etapa #3 usando seu terminal Bash no VS Code: `source .venv/bin/activate`. Se funcionou, você deve ver (. venv) antes do prompt de comando.

7. Instale o Django no ambiente virtual com o comando: `python3 -m pip install django`. Verifique se ele está instalado digitando: `python3 -m django --version`.

8. Em seguida, execute o seguinte comando para criar o projeto Django:

    ```bash
    django-admin startproject web_project .
    ```

    O `startproject` comando pressupõe (pelo uso de `.` no final) que a pasta atual é a pasta do projeto e cria o seguinte dentro dela:

    - `manage.py`: O utilitário administrativo da linha de comando Django para o projeto. Você executa comandos administrativos para o projeto usando `python manage.py <command> [options]`o.

    - Uma subpasta `web_project`chamada, que contém os seguintes arquivos:
        - `__init__.py`: um arquivo vazio que informa ao Python que essa pasta é um pacote do Python.
        - `wsgi.py`: um ponto de entrada para servidores Web compatíveis com WSGI para atender ao seu projeto. Normalmente, você deixa esse arquivo como está, pois ele fornece os ganchos para servidores Web de produção.
        - `settings.py`: contém configurações para o projeto Django, que você modifica no curso do desenvolvimento de um aplicativo Web.
        - `urls.py`: contém um sumário para o projeto Django, que você também modifica no curso de desenvolvimento.

9. Para verificar o projeto Django, inicie o servidor de desenvolvimento do Django usando `python3 manage.py runserver`o comando. O servidor é executado na porta padrão 8000 e você deve ver uma saída semelhante à seguinte saída na janela do terminal:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Quando você executa o servidor pela primeira vez, ele cria um banco de dados SQLite padrão no `db.sqlite3`arquivo, que é destinado a fins de desenvolvimento, mas pode ser usado na produção para aplicativos Web de baixo volume. Além disso, o servidor Web interno do Django é destinado *apenas* para fins de desenvolvimento local. No entanto, quando você implanta em um host da Web, o Django usa o servidor Web do host em vez disso. O `wsgi.py` módulo no projeto Django cuida da conexão com os servidores de produção.

    Se você quiser usar uma porta diferente da 8000 padrão, especifique o número da porta na linha de comando, como `python3 manage.py runserver 5000`.

10. `Ctrl+click`a `http://127.0.0.1:8000/` URL na janela saída do terminal para abrir o navegador padrão para esse endereço. Se o Django for instalado corretamente e o projeto for válido, você verá uma página padrão. A janela saída de terminal do VS Code também mostra o log do servidor.

11. Quando terminar, feche a janela do navegador e interrompa o servidor em vs Code usando `Ctrl+C` conforme indicado na janela saída do terminal.

12. Agora, para criar um aplicativo Django, execute o comando do `startapp` utilitário administrativo na pasta do projeto (onde `manage.py` reside):

    ```bash
    python3 manage.py startapp hello
    ```

    O comando cria uma pasta chamada `hello` que contém um número de arquivos de código e uma subpasta. Desses, com frequência, você trabalha `views.py` com (que contém as funções que definem páginas em seu aplicativo Web `models.py` ) e (que contém classes que definem seus objetos de dados). A `migrations` pasta é usada pelo utilitário administrativo do Django para gerenciar versões de banco de dados, conforme discutido posteriormente neste tutorial. Também há arquivos `apps.py` (configuração de aplicativo), `admin.py` (para criar uma interface administrativa) e `tests.py` (para testes), que não são abordados aqui.

13. Modifique `hello/views.py` para corresponder ao código a seguir, que cria uma única exibição para o home page do aplicativo:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Crie um arquivo, `hello/urls.py`, com o conteúdo abaixo. O `urls.py` arquivo é onde você especifica padrões para rotear URLs diferentes para suas exibições apropriadas. O código abaixo contém uma rota para mapear a URL raiz do aplicativo`""`() para `views.home` a função que você acabou de `hello/views.py`adicionar a:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. A `web_project` pasta também contém um `urls.py` arquivo, que é onde o roteamento de URL é realmente Tratado. Abra `web_project/urls.py` e modifique-o para corresponder ao código a seguir (você pode manter os comentários instrutivos, se desejar). Esse código efetua pull no uso `hello/urls.py` `django.urls.include`do aplicativo, que mantém as rotas do aplicativo contidas no aplicativo. Essa separação é útil quando um projeto contém vários aplicativos.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Salvar todos os arquivos modificados.

17. No vs Code terminal, execute o servidor de desenvolvimento com `python manage.py runserver` e abra um navegador para `http://127.0.0.1:8000/` ver uma página que renderiza "Olá, Django".

Parabéns, você criou um aplicativo Web Django usando o VS Code e o subsistema do Windows para Linux! Para obter um tutorial mais aprofundado usando VS Code e Django, consulte o [tutorial do Django em Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Recursos adicionais

- [Tutorial do Python com o vs Code](https://code.visualstudio.com/docs/python/python-tutorial): Um tutorial introdutório para VS Code como um ambiente Python, principalmente como editar, executar e depurar código.
- [Suporte do git no vs Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Saiba como usar noções básicas de controle de versão do git no VS Code.  
- [Saiba mais sobre as atualizações em breve com o WSL 2!](https://docs.microsoft.com/windows/wsl/wsl2-index): Essa nova versão altera como as distribuições do Linux interagem com o Windows, aumentando o desempenho do sistema de arquivos e adicionando compatibilidade total com chamadas do sistema.
- [Trabalhando com várias distribuições do Linux no Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Saiba como gerenciar várias distribuições Linux diferentes em seu computador Windows.
