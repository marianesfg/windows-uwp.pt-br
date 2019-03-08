---
title: Use a exemplo de placar de líderes de cena no Unity
description: Mostra as etapas para configurar corretamente a cena do placar de líderes de Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity, placares de líderes
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589741"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>A cena do exemplo de placar de líderes no Unity

[O plug-in do Unity](https://github.com/Microsoft/xbox-live-unity-plugin) hospeda um número de cenas de exemplo para demonstrar os serviços Xbox Live disponíveis para desenvolvedores do Unity. Uma cena tal é a cena do exemplo de placar de líderes. Captura de tela abaixo, você verá que a cena do placar de líderes exemplo simplesmente exibe um painel de entrada do usuário do Xbox Live e um painel para conter o placar de líderes. Se você pressionar reproduzir no momento sem adicionar a esta cena, você encontrará o painel de entrada é preenchida com dados de usuário falso, mas o placar de líderes não carrega nenhuma informação. Para obter essa cena de exemplo para carregar um placar de líderes real, que você terá que fazer algumas adições.

![Captura de tela de cena do placar de líderes](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>Pré-requisitos

Placar de líderes no Xbox Live é baseados nas estatísticas no serviço de estatísticas do Xbox Live. Antes de você pode preencher um placar de líderes com dados, que você precisará ter algumas estatísticas associadas com suas contas de teste. Se você ainda não adicionou as estatísticas para o título, você pode aprender como fazer isso lendo [adicionar estatísticas de player e placares de líderes ao seu projeto do Unity](add-stats-and-leaderboards-in-unity.md). Depois de executar as ações na seção de estatísticas de artigo volte aqui para exibir uma estatística da cena do placar de líderes de exemplo.

## <a name="the-leaderboard-inspector"></a>O Inspetor de placar de líderes

Placar de líderes pré-fabricado tem um número de configurações que podem ser alteradas na seção de inspetor para componente de script do placar de líderes, como a interface do usuário *tema*, o player associado o placar de líderes, configurações do controlador do xbox, e outras configurações de placar de líderes. Você pode ver que as configurações de placar de líderes são divididas em diferentes seções no Inspetor de abaixo.

![Captura de tela de Inspetor de placar de líderes](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>Configurações de tema e exibição

Esta seção tem uma configuração chamada *tema*. Isso é uma simple operação de soltar para baixo que permite que você use um tema claro ou escuro para o placar de líderes de pré-fabricado. Isso alterará o plano de fundo, fonte e as cores da imagem de um pré-fabricado. O efeito é visto facilmente durante a reprodução de cena no player do unity.

![Tema claro](../images/unity/leaderboard_light_theme.JPG) ![Tema escuro](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>Configuração STAT

Esta seção permite que você determinar que tipo de dados será recuperado quando o placar de líderes é preenchido com linhas.

- **Número de Player** -Isso determina qual player está associado com o placar de líderes.
- **STAT** -a estatística usada para popular os dados de placar de líderes. Isso é necessário para o placar de líderes pré-fabricado carregar dados.
- **Tipo de placar de líderes** -esse menu suspenso aplica um filtro aos dados placar de líderes carregados. Se *Global* está selecionado o placar de líderes será não filtrado e mostrará cada jogador com um valor para a estatística selecionada. Se *amigos* está selecionado o placar de líderes será filtrada para apenas Mostrar jogadores no placar de líderes que também estão na lista de amigos. Se *favorito* está selecionado o placar de líderes será filtrada para apenas Mostrar jogadores no placar de líderes que também estão na sua lista de favoritos.
- **Contagem de entrada** -um controle deslizante com um intervalo de 1 a 100 que determina quantas linhas de placar de líderes serão retornadas por vez. O número definido aqui determina o número de linhas de placar de líderes mostrados por página.

### <a name="controller-configuration"></a>Configuração do controlador

Placar de líderes pré-fabricado permite aos desenvolvedores configurar facilmente o uso de controlador do Xbox. A seção de configuração do controlador do pré-fabricado permite que você habilite e escolha os botões que controlam o placar de líderes pré-fabricado.

- **Habilitar o controlador de entrada** -uma alternância de botão de rádio simples. Se essa opção estiver marcada, em seguida, você pode usar um controlador do Xbox para interagir com pré-fabricado. Necessário para suporte ao controlador.
- **Número de joystick** -designa qual controlador pode interagir com esse placar de líderes de pré-fabricado.
- **Botão de controlador de próxima página** -menu suspenso de quais controles de botão que carrega a próxima página de placar de líderes de linhas.
- **Botão de controlador de página anterior** -menu suspenso de quais controles de botão que carrega a página anterior do placar de líderes de linhas.
- **Botão próximo do controlador de exibição** – alterna o tipo de exibição entre **todas as** e **mais próximo ME**.
- **Botão de controlador de exibição anterior** – alterna o tipo de exibição entre **todas as** e **mais próximo ME**.
- **Eixo de entrada de rolagem vertical** -cadeia de caracteres que designa o eixo do controlador está associado com a rolagem.
- **Role o multiplicador de velocidade** -determina a velocidade de rolagem do controlador.

> [!NOTE]
> Alterar os botões usados para alterar a página ou exibição de placar de líderes também alterará a imagem associada com o botão usado para cada função de placar de líderes. Você não precisa alterar a seção de referências de interface do usuário manualmente para corresponder a imagem para o botão.

### <a name="ui-references"></a>Referências de interface do usuário

Essa seção controla a composição geral da pré-fabricado placar de líderes e imagens. Esta seção não precisa ser alterado para o uso bem-sucedido do placar de líderes pré-fabricado. No entanto, você talvez precise fazer ajustes para esta seção para personalizar a aparência de um pré-fabricado para suas próprias finalidades.

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>Preenchendo o placar de líderes de Player do Unity (com dados falsos)

Para popular o placar de líderes de Player do Unity com dados, você precisará adicionar uma estatística para o placar de líderes pré-fabricado. Exibir o placar de líderes pré-fabricado no Inspetor de revelará que pode levar um objeto do tipo `Stat Base` como um parâmetro público no seu script. Você pode arrastar qualquer um os `State Base` digite pré-fabricados `IntegerStat`, `DoubleStat`, ou `StringStat` na pasta pré-fabricados de plug-in do Xbox Live Unity e colocá-lo neste local no placar de líderes pré-fabricado.

![Arraste e solte pré-fabricado Stat](../images/unity/stat-to-leaderbaord-drag.gif)

Executar agora a cena no Unity e você descobrirá que o placar de líderes é preenchido com dados fictícios, como abaixo.

![Captura de tela de placar de líderes de dados falsos preenchidos](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>Populando um criado o projeto com dados reais do Visual Studio

Para popular um placar de líderes com dados reais para seu título, você precisará criar seu jogo para executar localmente em seu computador. Você precisará de uma compilação local porque o editor do Unity não tem acesso ao Xbox Live. Além de criar seu projeto deve ser executado localmente, você terá que configurar o stat no seu placar de líderes para um estado que é inicializado e tem valores para o título. Para associar um estado para o placar de líderes, você precisará modificar a ID e o nome de exibição do objeto stat no placar de líderes pré-fabricado. A identificação precisa corresponder ao de um stat configurado no [Partner Center](https://partner.microsoft.com/dashboard). Após você ter feito isso, compile o projeto conforme descrito na [compilar a seção Configurar Xbox Live no artigo do Unity](configure-xbox-live-in-unity.md#build-and-test-the-project). Executar este projeto como x64 direcionamento no computador Local de compilação deve permitir que você entrar com um nome de jogador real e preencher o placar de líderes com dados reais.