---
title: Configuração de presença avançada no Partner Center
description: Saiba como configurar cadeias de caracteres de presença avançada no Partner Center
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jogos, uwp, windows 10, Xbox um, cadeias de caracteres de presença avançada, Partner Center
ms.openlocfilehash: 5c2add99c6fc6ad1c7f085eb35dc4fdba9e0688d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624121"
---
# <a name="configure-rich-presence-in-partner-center"></a>Configura presença avançada no Partner Center

Cadeias de caracteres de presença avançada exibem a atividade de jogos do usuário. Eles são exibidos sob o nome de jogador de um player na **amigos & paus** lista, bem como em seu perfil do usuário Xbox Live. Cadeias de caracteres de presença avançada configuradas são acrescentadas ao nome do jogo que está sendo reproduzido. Se você cria um jogo chamado BubblePop e configurar a cadeia de caracteres de presença avançada "Popping bolhas com amigos", sua cadeia de caracteres configurada produziria "BubblePop - bolhas de Popping com amigos" como um status. Abaixo você pode ver como uma cadeia de caracteres de presença avançada aparecerá no contexto.

Na seguinte captura de tela Xbox Live users **Roar última** e **Lucha Uno** estiver jogando jogos usando cadeias de caracteres de presença avançada.

![Exemplo de lista de amigos](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

Na seguinte captura de tela, você pode ver **de Lucha Uno** completa a cadeia de caracteres de presença avançada em seu perfil.

![Exemplo de perfil](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> Cadeias de caracteres de presença avançada não estão disponíveis para os títulos do programa de criadores do Xbox Live e, portanto, não são configuráveis para esses títulos. O conteúdo deste artigo é para ID@Xbox e títulos de parceiros gerenciadas.

## <a name="requirements"></a>Requisitos

Antes de configurar cadeias de caracteres de presença avançada você e seu título devem atender aos seguintes critérios:

- Você deve ter uma conta de desenvolvimento do Windows.
- Sua conta de desenvolvimento deve ser registrada no ID@Xbox programa ou como uma conta de desenvolvedor do parceiro gerenciado.
- O título deve ser registrado no Partner Center e ser Xbox Live habilitado.

Antes de usar cadeias de caracteres de presença avançada que você deve configurá-los no Partner Center.

## <a name="rich-presence-configuration-page"></a>Página de configuração de presença avançada

Cadeias de caracteres de presença avançada são configuradas como parte do serviço Xbox Live para o título na [Partner Center](https://partner.microsoft.com/dashboard).

Navegue até a página de configuração de presença avançada com as instruções a seguir:

1. Vá para [Partner Center](https://partner.microsoft.com/dashboard) em developer.microsoft.com.
2. Entrar com sua conta de desenvolvedor do Windows registrada se a entrada for solicitada.
3. Escolha seu título do Xbox Live habilitada ou o aplicativo do **visão geral** página. Não selecione um título do programa de criadores, pois ele não será habilitado para a configuração de cadeia de caracteres de presença avançada.
4. Clique no **Services** lista suspensa e selecione o Xbox Live.
5. Role para baixo até a **presença avançada** vincular e clique nele.

A página de presença avançada exibe uma breve descrição do serviço, um botão para criar uma nova cadeia de caracteres de presença avançada e uma lista de pesquisa cadeias de caracteres configurada anteriormente. Nessa página, você pode configurar novas cadeias de caracteres, bem como editar e revisar suas cadeias de caracteres configuradas.

![Exemplo de cadeia de presença avançada Config Page](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> Cadeias de caracteres como "Jogo Net executor - combate mortal com vários participantes na Base de LUA com acaba 10 com." não estarão disponíveis para os desenvolvedores a partir da atualização de 2017 da plataforma de dados. Plataforma de dados 2013 *variáveis* não estão disponíveis na plataforma de dados 2017. Nesse caso, a variável é que o número de elimina "10". A cadeia de caracteres equivalente após a atualização de dados plataforma 2017 seriam "Jogo Net executor - combate mortal com vários participantes na Base de LUA." "Reprodução Net executor - nos menus" ainda é uma cadeia de caracteres válida de presença avançada.

## <a name="create-a-new-rich-presence-string"></a>Criar uma nova cadeia de caracteres de presença avançada

Para criar uma cadeia de caracteres de presença avançada, clique no botão rotulado **cadeia de caracteres de presença do novo Rich**. Você verá a interface do usuário para preencher a **detalhes de presença** que incluem o **ID exclusiva de presença avançada** , bem como a **Exibir cadeia de caracteres** para sua nova cadeia de caracteres de presença avançada.

![nova cadeia de caracteres de presença avançada da interface do usuário](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**ID exclusiva de presença avançada** -a ID exclusiva de presença avançada é uma cadeia de caracteres usada para identificar sua cadeia de caracteres de presença avançada. Essa cadeia de caracteres será usada para definir o status de jogadores em seu jogo e está associada com a cadeia de caracteres específica que você deseja exibir. Sua ID pode ser um máximo de 50 caracteres.

**Exibir cadeia de caracteres** -exibir a cadeia de caracteres é a cadeia de caracteres que você deseja exibir acrescentado ao status de alguns jogadores o jogo. Isso é onde você especificará na cadeia de caracteres de presença avançada que deseja exibir para gerar interesse em seu jogo. A exibição pode ser um máximo de 100 caracteres, mas haverá ocasiões em que somente os primeiros 40 caracteres serão mostrados.

Para criar sua nova cadeia de caracteres de presença avançada, preencha os campos e pressione a **salvar** botão.
Depois de clicar em Salvar, que você será direcionado para a página de configuração de presença avançada onde você verá sua nova cadeia de caracteres de presença avançada adicionada à lista de cadeias de caracteres configuradas.

## <a name="review-edit-and-delete-strings"></a>Revisar, editar e excluir cadeias de caracteres

Aqui você pode ver uma página de configuração de presença avançada com algumas cadeias de caracteres configuradas.
![Página de presença avançada configurado de exemplo](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

Para analisar cadeias de caracteres criadas anteriormente, simplesmente procure a lista na página de configuração de presença avançada. Lá você encontrará a ID exclusiva de presença avançada tanto Exibir cadeia de caracteres juntos. Isso será útil quando você precisa usar a Id exclusiva de presença avançada no código do seu título para especificar uma cadeia de caracteres de presença avançada.

Para editar uma presença avançada cadeia de caracteres, basta clicar na **identificação exclusiva de presença avançada** link para a cadeia de caracteres que você deseja editar. Você será levado para a mesma interface do usuário usada para criar uma nova cadeia de caracteres de presença avançada com as configurações atuais de cadeia de caracteres preenchidas para edição. Depois de fazer edições de clique a **salvar** botão para atualizar a cadeia de caracteres configurada com suas alterações.

Para excluir um clique de cadeia de caracteres de presença avançada configurado a **excluir** link na página de configuração de presença avançada na mesma linha como a cadeia de caracteres de presença avançada que você deseja excluir. Você será solicitado a confirmar a exclusão.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações conceituais detalhadas sobre os recursos de cadeia de caracteres de presença avançada e como implementá-los, leia as [documentação de presença avançada](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview).