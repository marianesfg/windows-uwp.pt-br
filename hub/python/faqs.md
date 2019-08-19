---
title: Perguntas frequentes sobre como usar o Python no Windows
description: Perguntas frequentes sobre como usar o Python no Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Pip, py. exe, caminhos de arquivo, PYTHONPATH, implantação do Python, empacotamento do Python
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: f2dbc455578a3fbe79c2558e2ba16c8f82cea522
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959046"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Perguntas frequentes sobre como usar o Python no Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>Por que não posso "instalar o PIP" um determinado pacote?

Há uma série de motivos pelos quais uma instalação falhará, na maioria dos casos, a solução certa é entrar em contato com o desenvolvedor do pacote.

A causa mais comum de problemas é tentar instalar em um local que você não tem permissão para modificar. Por exemplo, o local de instalação padrão pode exigir privilégios administrativos, mas, por padrão, o Python não os terá. A melhor solução é criar um ambiente virtual e instalá-lo.

Alguns pacotes incluem código nativo que exige que um C C++ ou compilador seja instalado. Em geral, os desenvolvedores de pacotes devem publicar versões pré-compiladas, mas, muitas vezes, não. Alguns desses pacotes podem funcionar se você [instalar as ferramentas de compilação para o Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) e C++ selecionar a opção, no entanto, na maioria dos casos, será necessário entrar em contato com o desenvolvedor do pacote.

[Siga a discussão em StackOverflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>O que é py. exe?

Você pode acabar com várias versões do Python instaladas em seu computador porque você está trabalhando em diferentes tipos de projetos do Python. Como todos usam o `python` comando, talvez não seja óbvio qual versão do Python você está usando. Como padrão, é recomendável usar o `python3` comando (ou `python3.7` para selecionar uma versão específica).

O [iniciador py. exe](https://docs.python.org/3/using/windows.html#launcher) selecionará automaticamente a versão mais recente do Python que você instalou. Você também pode usar comandos como `py -3.7` para selecionar uma versão específica ou `py --list` para ver quais versões podem ser usadas. **No entanto**, o iniciador py. exe só funcionará se você estiver usando uma versão do Python instalada em [Python.org](https://www.python.org/downloads/windows/). Quando você instala o Python do Microsoft Store, o `py` comando **não é incluído**. Para Linux, MacOS, WSL e a versão Microsoft Store do Python, você deve usar o `python3` comando ( `python3.7`ou).

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>Por que executar o Python. exe abre o Microsoft Store?

Para ajudar novos usuários a encontrar uma boa instalação do Python, adicionamos um atalho para o Windows que o levará diretamente para a versão mais recente do pacote da Comunidade publicada no Microsoft Store. Esse pacote pode ser instalado facilmente, sem permissões de administrador, e substituirá o `python` padrão `python3` e os comandos pelos verdadeiros.

A execução do executável de atalho com qualquer argumento de linha de comando retornará um código de erro para indicar que o Python não foi instalado. Isso é para impedir que arquivos em lotes e scripts abram o aplicativo da loja quando ele provavelmente não foi pretendido.

Se você instalar o Python usando os instaladores do [Python.org](https://www.python.org/downloads/windows/) e selecionar a opção "adicionar ao caminho", o `python` novo comando terá prioridade sobre o atalho. Observe que outros instaladores podem adicionar `python` em uma prioridade _mais baixa_ do que o atalho interno.

Você pode desabilitar os atalhos sem instalar o Python abrindo "gerenciar aliases de execução de aplicativo" em Iniciar, localizando as entradas do Python "instalador de aplicativo" e alternando-as para "desativado".

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>Por que os caminhos de arquivo não funcionam no Python quando eu os copio?

As strings do python usam "escapes" para caracteres especiais. Por exemplo, para inserir um novo caractere de linha em uma cadeia de caracteres, `\n`você digitaria. Como os caminhos de arquivo no Windows usam barras invertidas, algumas partes podem estar sendo convertidas em caracteres especiais.

Para colar um caminho como uma cadeia de caracteres no Python, `r` adicione o prefixo. Isso indica que se trata de `raw` uma cadeia de caracteres e nenhum caractere de escape será usado, exceto para \ "(talvez seja necessário remover a última barra invertida em seu caminho). Portanto, seu caminho pode ser semelhante a: r "C:\Users\MyName\Documents\Document.txt"

Ao trabalhar com caminhos no Python, é recomendável usar o módulo pathlib padrão. Isso permitirá que você converta a cadeia de caracteres em um objeto de caminho avançado que pode fazer manipulações de caminho de forma consistente, independentemente de usar barras invertidas ou barras invertidas, tornando seu código funcionando melhor em diferentes sistemas operacionais.

## <a name="what-is-pythonpath"></a>O que é o PYTHONPATH?

A variável de ambiente PYTHONPATH é usada pelo Python para especificar uma lista de diretórios dos quais os módulos podem ser importados. Ao executar, você pode inspecionar `sys.path` a variável para ver quais diretórios serão pesquisados quando você importar algo.

Para definir essa variável no prompt de comando, use: `set PYTHONPATH=list;of;paths`.

Para definir essa variável do PowerShell, use: `$env:PYTHONPATH=’list;of;paths’` logo antes de iniciar o Python.

**Não** é recomendável definir essa variável globalmente por meio das configurações de **variáveis de ambiente** , pois ela pode ser usada por qualquer versão do Python em vez da que você pretende usar.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>Onde posso encontrar ajuda com o empacotamento e a implantação?

[Docker](https://code.visualstudio.com/docs/azure/docker): A [extensão VSCode](https://code.visualstudio.com/docs/azure/docker) ajuda você a empacotar e implantar rapidamente com os modelos Dockerfile e Docker-Compose. yml (gerar os arquivos de Docker apropriados para seu projeto).

O [AKs (serviço kubernetes do Azure)](https://docs.microsoft.com/azure/aks/) permite implantar e gerenciar aplicativos em contêineres ao dimensionar recursos sob demanda.

## <a name="what-if-i-need-to-work-across-different-machines"></a>E se eu precisar trabalhar em máquinas diferentes?

A [sincronização de configurações](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) permite que você sincronize suas configurações de vs Code em diferentes instalações usando o github. Se você trabalha em computadores diferentes, isso ajuda a manter seu ambiente consistente entre eles.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>E se eu estiver acostumado a usar PyCharm, Atom, Sublime Text, Emacs ou vim?

A extensão do VSCode [keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) pode ajudar seu ambiente a se sentir bem em casa.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Como as teclas de atalho do Mac são mapeadas para as teclas de atalho do Windows?

Alguns dos botões de teclado e atalhos do sistema são um pouco diferentes entre um computador Windows e um Macintosh. Este [Guia de mapeamento de teclado](https://support.microsoft.com/help/970299/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh) da suporte da Microsoft aborda os conceitos básicos.
