---
title: Configuração de armazenamento no Partner Center do título
description: Saiba como configurar o armazenamento de título no Partner Center
ms.date: 04/24/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jogos, uwp, windows 10, Xbox um, o armazenamento de título, o Partner Center
ms.openlocfilehash: d32f9f2f4e003db50ad560acc513511a850d43b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612091"
---
# <a name="configure-storage-for-you-title-in-partner-center"></a>Configurar o armazenamento para você título no Partner Center

Xbox Live permite que você salve os dados associados ao seu jogo na nuvem por meio do serviço de armazenamento do título. A página de configuração de armazenamento do título permite que você determine quais tipos de serviços de armazenamento de nuvem seu jogo será permitir, bem como carregar arquivos a ser usado para armazenamento Global.

Você pode encontrar a página de configuração de armazenamento de título do Xbox Live, acessando [parceiro](https://partner.microsoft.com/dashboard), escolhendo seu aplicativo da **visão geral** ou **produtos**, abrindo o  **Os serviços** soltar para baixo e selecionando **Xbox Live**. Os desenvolvedores no programa de criadores serão necessário clicar **Mostrar opções** na **salva e o armazenamento em nuvem** seção da sua página de configuração para ver as opções de configuração de armazenamento do título. Aqueles com o conjunto completo de recursos do Xbox Live disponíveis precisará localizar o **título armazenamento** link para navegar até a página de configuração de armazenamento do título.

Configuração de armazenamento do título tem duas seções principais. A seção de configurações de armazenamento do título e a seção de gerenciamento de arquivo de armazenamento global.

## <a name="section-1-title-storage-settings"></a>Seção 1: Configurações de armazenamento do título

![Captura de tela de configurações de armazenamento do título](../../images/dev-center/title-storage/title-storage-settings.JPG)

Nesta seção, você pode habilitar qualquer um dos quatro tipos de armazenamento, marcando a caixa **Active** coluna. Depois de escolher você título 's tipos de armazenamento você pode escolher se os dados de leitura serão restritos para o jogador que ele pertence, clicando na linha de tipo de armazenamento **proprietário** somente o botão de opção ou compartilhados publicamente, clicando na **Todas as pessoas** botão de opção. Se você selecionar **proprietário só** para um determinado **tipo de armazenamento** e em seguida, os dados de armazenamento do título desse tipo só poderá ser lidas pelo player que gerou esses dados. Se você selecionar **Everyone** para um determinado **tipo de armazenamento** e em seguida, os dados de armazenamento do título desse tipo serão legíveis para todos os jogadores. Gravar ou modificar os dados salvos só está disponível para o usuário que o gerou em todos os casos.

## <a name="storage-types"></a>Tipos de armazenamento

Há quatro tipos de armazenamento que podem ser ativados na página de configuração do armazenamento do título. Você pode encontrar uma descrição de cada tipo de armazenamento, passando o mouse sobre o ícone de informações ao lado do nome de cada tipo de armazenamento ou lendo a tabela a seguir.

|Tipo de Armazenamento |Descrição |Exemplo de uso  |
|---------|---------|---------|
|Global             |Dados carregados Partner Center que podem ser lidos por qualquer dispositivo e é acessível a todos os usuários. Só podem ser gravados pelos carregamentos de desenvolvedor Partner Center. | Anuncie atualizações para todos os usuários por meio do feed de notícias do jogo.     |
|Armazenamento Conectado  |Permite que o plano de fundo a sincronização de dados de jogo em XboxOne e jogos do Windows 10. Um jogo de tolerante a falhas robusto salvar o serviço. Podem ser lidos por qualquer dispositivo, podem ser gravados em, dispositivos Xbox One e Windows 10    | Salve arquivos para um usuário individual permitir a reprodução em um console separado.         |
|Universal          |Rede de armazenamento de blob acessível que fornece acesso de leitura/gravação a qualquer dispositivo que não é um Xbox 360 ou o Windows Phone. Pode ser lido por dispositivos Android e IOS.      | Salve a reprodução ou outras estatísticas possam ser acessadas de vários dispositivos do Windows.        |
|confiável            |Armazenamento de BLOBs acessível de rede que pode ser gravado apenas por Xbox One, Xbox 360 e Windows Phone. Podem ser lidos por qualquer dispositivo. Pode ser lido pelo Android e IOS.     | armazene a classificação de um jogador com vários participantes.        |

## <a name="section-2-global-storage-file-management"></a>Seção 2: Gerenciamento de arquivos de armazenamento global

![captura de tela de gerenciamento de arquivo global de armazenamento](../../images/dev-center/title-storage/global-storage-file-management.JPG)

Para ver opções de gerenciamento de arquivo completo do armazenamento Global, você deve clicar o **Mostrar opções** lista suspensa. Nesta seção, você pode adicionar arquivos e pastas que poderá ser acessadas se o **Global** tipo de armazenamento é definido como ativo nas configurações de armazenamento do título. Seu jogo precisará ser publicado para testes para adicionar arquivos nesta seção. Se seu jogo não for publicado adequadamente para teste, você poderá ver um aviso na parte superior da página de configuração de armazenamento do título.

## <a name="manage-global-storage-files"></a>Gerenciar arquivos de armazenamento Global

Gerenciamento de arquivos de armazenamento global permite que você carregar e baixar arquivos a ser usado para armazenamento global. Esses arquivos potencialmente podem ser acessados por qualquer pessoa que possui o título e devem ser compartilhados entre todos os jogadores que seu jogo. Para ver opções de arquivo de armazenamento global, você deve clicar o **Mostrar opções** lista suspensa ao lado do título seções. Para adicionar, clique em seu primeiro arquivo a **+ novo...**  link. Você, em seguida, terá a opção de adicionar um novo arquivo ou pasta para os arquivos de armazenamento global.

### <a name="new-folders"></a>Novas pastas

Quando simplesmente adicionando uma nova pasta em seu armazenamento global arquivos que você precisa para o nome da pasta e clique no **criar** botão. A nova pasta aparecerá na tabela do Explorador de arquivos.

![Adicionar caixa de diálogo de pasta](../../images/dev-center/title-storage/add-folder-global-storage-filled.JPG)

Para adicionar arquivos à pasta você deve carregá-los para a pasta diretamente por push as pastas **ações** botão: "**...** "e selecionando **carregar arquivos**. Você não pode arrastar e soltar arquivos em pastas dentro da tabela do Explorador de arquivos. Usando o **criar pasta** ação na **ações** menu de uma pasta criará uma pasta filho. Escolhendo a **exclua** ação na **ações** menu de uma pasta excluirá a pasta e todo seu conteúdo.

### <a name="new-files"></a>Novos arquivos

Ao adicionar um novo arquivo no armazenamento global você será solicitado que você carregue um arquivo do Explorador de arquivos do seu computador e, em seguida, é solicitado a escolher de um dos tipos de arquivo de três, binário, configuração e JSON. Além de poder carregar um arquivo com o **+ novo...**  botão, você também pode arrastar e soltar arquivos do computador para a tabela do Explorador de arquivos.

> [!WARNING]
> Você não pode arrastar as pastas na tabela do Explorador de arquivos, tentar esta ação resultará na pasta que está sendo tratada como um arquivo e não funcionará conforme o esperado.

Ações de gerenciamento de arquivos: ![arquivo gif de gerenciamento](../../images/dev-center/title-storage/global-storage-management.gif)

#### <a name="file-types"></a>Tipos de arquivo

* Binário - tipo binário deve ser usado para imagens, sons e dados personalizados. Esses dados devem ser HTTP amigável.
* Config - arquivos de configuração mantêm informações sobre seu jogo e podem ter consulta dinâmica com base em alguma entrada de valores de retorno.
* JSON - arquivos. JSON. Esses arquivos contêm algumas informações sobre um aspecto do seu jogo, semelhante a um arquivo de configuração.

## <a name="further-reading"></a>Leitura adicional

Para saber mais sobre a plataforma de armazenamento do Xbox Live, leia as [documentação do armazenamento de título](../../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) que fornecerá mais detalhes sobre Universal, confiáveis, armazenamento globais e tipos de arquivo de armazenamento e o [armazenamento conectado documentação](../../storage-platform/connected-storage/connected-storage-overview.md), que irá ensiná-lo mais sobre como salvar o progresso de jogo na nuvem para que você possa continuar seu jogo entre dispositivos.