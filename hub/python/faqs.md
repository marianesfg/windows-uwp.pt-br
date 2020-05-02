---
title: Perguntas frequentes sobre como usar o Python no Windows
description: Perguntas frequentes sobre como usar o Python no Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, pip, py.exe, caminhos de arquivos, PYTHONPATH, implantação do Python, empacotamento do Python
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 6dbf86e0f9435e44140159ebb2bcbc3d67928999
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74663553"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Perguntas frequentes sobre como usar o Python no Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>Por que eu não posso realizar a "instalação PIP" de um determinado pacote?

Há uma série de motivos pelos quais uma instalação pode falhar – na maioria dos casos, a solução certa é entrar em contato com o desenvolvedor do pacote.

A causa mais comum de problemas é tentar instalar em um local que você não tem permissão para modificar. Por exemplo, o local de instalação padrão pode exigir privilégios de administrador, mas, por padrão, o Python não tem esses privilégios. A melhor solução é criar um ambiente virtual e instalar ali.

Alguns pacotes incluem código nativo que exige um compilador C ou C++ para instalação. Em geral, os desenvolvedores de pacotes costumam publicar versões pré-compiladas. Porém, algumas vezes eles não o fazem. Alguns desses pacotes poderão funcionar se você [instalar Ferramentas de Build para o Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) e selecionar a opção C++. No entanto, na maioria dos casos, será necessário entrar em contato com o desenvolvedor do pacote.

[Acompanhe as discussões no StackOverflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>O que é py.exe?

Você pode acabar com várias versões do Python instaladas em seu computador, já que está trabalhando em diferentes tipos de projetos do Python. Como todas usam o comando `python`, talvez não seja tão óbvio qual versão do Python você está usando. Como padrão, é recomendável usar o comando `python3` (ou `python3.7` para selecionar uma versão específica).

O [inicializador py.exe](https://docs.python.org/3/using/windows.html#launcher) selecionará automaticamente a versão mais recente do Python que você instalou. Você também pode usar comandos como `py -3.7` para selecionar uma versão específica ou `py --list` para ver quais versões podem ser usadas. **NO ENTANTO**, o inicializador py.exe só funcionará se você estiver usando uma versão do Python instalada de [python.org](https://www.python.org/downloads/windows/). Quando você instala o Python da Microsoft Store, o comando `py`**não está incluído**. Para Linux, macOS, WSL e a versão Microsoft Store do Python você deve usar o comando `python3` (ou `python3.7`).

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>Por que executar o python.exe abre a Microsoft Store?

Para ajudar novos usuários a encontrar uma boa instalação do Python, adicionamos um atalho para o Windows que o levará diretamente para a versão mais recente do pacote da comunidade publicado no Microsoft Store. Esse pacote pode ser instalado com facilidade – sem permissões de administrador – e substituirá os comandos padrão `python` e `python3` pelos verdadeiros.

A execução do atalho executável com qualquer argumento de linha de comando retornará um código de erro para indicar que o Python não foi instalado. Isso é para impedir que arquivos em lotes e scripts abram o aplicativo do Microsoft Store quando provavelmente não foi pretendido.

Se você instalar o Python usando os instaladores de [python.org](https://www.python.org/downloads/windows/) e selecionar a opção "adicionar ao CAMINHO", o novo comando `python` terá prioridade sobre o atalho. Observe que outros instaladores podem adicionar `python` a uma prioridade _mais baixa_ do que o atalho interno.

Você pode desabilitar os atalhos sem instalar o Python abrindo "Gerenciar aliases de execução de aplicativo", no menu Iniciar, localizando as entradas do Python do "Instalador de aplicativo" e alternando-as para "Desativado".

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>Por que os caminhos de arquivo não funcionam no Python quando eu os copio e colo?

As strings do python usam "escapes" para caracteres especiais. Por exemplo, para inserir um caractere de nova linha em uma cadeia de caracteres, você deve digitar `\n`. Como os caminhos de arquivo no Windows usam barras invertidas, algumas partes podem estar sendo convertidas em caracteres especiais.

Para colar um caminho como uma cadeia de caracteres no Python, adicione o prefixo `r`. Isso indica que se trata de uma cadeia de caracteres `raw` e nenhum caractere de escape será usado, exceto a \" (talvez seja necessário remover a última barra invertida do caminho). Portanto, seu caminho pode ter a seguinte aparência: `r"C:\Users\MyName\Documents\Document.txt"`

Ao trabalhar com caminhos no Python, é recomendável usar o módulo pathlib padrão. Isso permitirá que você converta a cadeia de caracteres em um objeto de caminho avançado, que pode fazer manipulações de caminho de forma consistente, independentemente de usar barras normais ou invertidas, fazendo com que o seu código funcione melhor em diferentes sistemas operacionais.

## <a name="what-is-pythonpath"></a>O que é o PYTHONPATH?

A variável de ambiente PYTHONPATH é usada pelo Python para especificar uma lista de diretórios dos quais os módulos podem ser importados. Durante a execução, você poderá inspecionar a variável `sys.path` para ver quais diretórios serão pesquisados quando você importar algo.

Para definir essa variável no prompt de comando, use: `set PYTHONPATH=list;of;paths`.

Para definir essa variável no PowerShell, use: `$env:PYTHONPATH=’list;of;paths’` logo antes de inicializar o Python.

Definir essa variável globalmente por meio das configurações de **Variáveis de ambiente** **não** é recomendável, pois ela pode ser usada por qualquer versão do Python em vez daquela que você pretende usar.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>Onde posso encontrar ajuda com o empacotamento e a implantação?

[Docker](https://code.visualstudio.com/docs/azure/docker): [A extensão VSCode](https://code.visualstudio.com/docs/azure/docker) ajuda você a empacotar e implantar rapidamente com os modelos Dockerfile e docker-compose.yml (gerar os arquivos de Docker apropriados para o seu projeto).

[O AKS (Serviço de Kubernetes do Azure)](https://docs.microsoft.com/azure/aks/) permite implantar e gerenciar aplicativos em contêineres ao mesmo tempo em que dimensiona recursos sob demanda.

## <a name="what-if-i-need-to-work-across-different-machines"></a>E se eu precisar trabalhar em diferentes computadores?

As [Configurações de sincronização](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) permitem que você sincronize suas configurações de VS Code em diferentes instalações usando o GitHub. Se você trabalha em diferentes computadores, isso ajuda a manter seu ambiente consistente entre eles.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>E se eu estiver acostumado a usar PyCharm, Atom, Sublime Text, Emacs ou Vim?

A extensão VSCode [keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) pode ajudar seu ambiente a se sentir em casa.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Qual é a correspondência entre as teclas de atalho do Mac e as teclas de atalho do Windows?

Alguns botões do teclado e atalhos do sistema são um pouco diferentes entre um computador Windows e um Macintosh. Este [Guia de transição do Mac para o Windows](../dev-environment/mac-to-windows.md) aborda os conceitos básicos.
