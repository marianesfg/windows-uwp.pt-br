---
title: Configurar políticas de acesso no Partner Center
description: Descreve como você pode configurar políticas de acesso no Partner Center para permitir que outros aplicativos, jogos e serviços para acessar as configurações do Xbox Live.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, udc, Central de desenvolvedores universal
ms.openlocfilehash: ae26c18abdac30ff988e90ee5c56f178bf14b74a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658211"
---
# <a name="configure-access-policies-in-partner-center"></a>Configurar políticas de acesso no Partner Center

Você pode usar [Partner Center](https://partner.microsoft.com/dashboard) para permitir que outros serviços, jogos e aplicativos acessem dados e configurações do Xbox Live seu título. Por exemplo, convém um serviço da web para exibir o placar de líderes em seu site, ou você pode ter um aplicativo complementar que pode acessar o armazenamento de título do jogo para exibir ou modificar dados de jogos salvos.

Por padrão, somente o título em si pode acessar as configurações e dados armazenados no serviço Xbox Live. Você pode alterar isso configurando políticas de acesso no Partner Center.

> [!NOTE]
> Este tópico não se aplica a títulos do programa de criadores do Xbox Live.

Adicione configuração fazendo o seguinte:

1. Depois de selecionar o título na [Partner Center](https://partner.microsoft.com/dashboard), navegue até **Services** > **Xbox Live**.

2. Clique no link para **políticas de acesso**.

3. Clique na configuração que você deseja conceder acesso a e clique no botão Adicionar aplicativo/serviço. Isso adicionará uma nova linha na parte inferior da lista de aplicativos e serviços configurados para acessar essa configuração.

4. Selecione o tipo de aplicativo ou serviço na caixa suspensa e preencha a caixa de detalhes para indicar o aplicativo, id de título ou id de serviço do aplicativo ou serviço que acessará os dados.

5. Selecione se o aplicativo ou serviço pode apenas ler os dados, ou se ele tem acesso completo aos dados.

6. Repita para cada configuração e para cada aplicativo ou serviço que precisa acessar essas configurações. Você pode clicar em **excluir** para remover uma entrada.

7. Quando tiver terminado, clique no **salvar** botão para salvar suas alterações.

![Políticas de acesso adicionam tela de aplicativo ou serviço](../../images/dev-center/data-sharing-2.png)
