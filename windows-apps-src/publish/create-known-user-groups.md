---
author: JnHs
Description: Learn how to create known user groups to use for package flighting and more.
title: Criar grupos de usuários conhecidos
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, grupo direcionado, clientes, grupo de versão de pré-lançamento, grupos de usuários, usuários conhecidos
ms.localizationpriority: medium
ms.openlocfilehash: e15b5a4a2f76cbfc33db593c3110ac6f2d054b5b
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4743703"
---
# <a name="create-known-user-groups"></a>Criar grupos de usuários conhecidos

Grupos de usuários conhecidos permitem que você adicione pessoas específicas a um grupo usando o endereço de e-mail associado à sua conta da Microsoft. Esses grupos de usuários conhecidos são frequentemente utilizados para distribuir pacotes específicos para um seleto grupo de pessoas com [pacotes de pré-lançamento](package-flights.md) ou para a distribuição de um envio para um [público particular](choose-visibility-options.md#audience). Eles podem ser usados para campanhas de envolvimento, como o envio de [notificações direcionadas](send-push-notifications-to-your-apps-customers.md) ou de [ofertas direcionadas](use-targeted-offers-to-maximize-engagement-and-conversions.md) para um grupo de clientes específicos.

Para ser considerado um membro do grupo, cada pessoa deve ser autenticada na Loja usando a conta da Microsoft associada ao endereço de e-mail fornecido. Para baixar o app por meio de pacotes de pré-lançamento, os membros do grupo deverão usar uma versão do Windows 10 que dê suporte a pacotes de pré-lançamento (Windows. Desktop build 10586 ou posterior; Windows.Mobile build 10586.63 ou posterior; ou Xbox One). Com os envios para audiência particular, os membros do grupo deverão usar o Windows 10, versão 1607 ou posterior (incluindo o Xbox One).

## <a name="to-create-a-known-user-group"></a>Para criar um grupo de usuários conhecido

1. No painel do Centro de Desenvolvimento do Windows, expanda **Envolver** no menu de navegação esquerdo e, em seguida, selecione **Grupos de clientes**. 
2. Na seção **Meus grupos de clientes**, selecione **Criar novo grupo**.
3. Na próxima página, insira um nome para seu grupo na caixa **Nome do grupo**.
4. Verifique se o botão de opção **Grupo de usuários conhecido** está selecionado.
5. Insira os endereços de e-mail das pessoas que você gostaria de adicionar ao grupo. Você deve incluir pelo menos um endereço de e-mail, com limite máximo de 10.000. Você pode inserir endereços de e-mail diretamente no campo (separados por espaços, vírgulas, ponto-e-vírgula ou quebras de linha) ou clicar no link **Importar .csv** para criar o grupo de versões de pré-lançamento a partir de uma lista de endereços de e-mail em um arquivo .csv.
6. Selecione **Salvar**.

O grupo agora estará disponível para uso.

Você também pode criar um grupo de usuários conhecidos ao selecionar **Criar um grupo de versão de pré-lançamento** na página de criação do [pacote de pré-lançamento](package-flights.md). Observe que você deve inserir novamente as informações já fornecidas na página de criação do pacote de pré-lançamento se você fizer isso.

> [!IMPORTANT]
> Ao usar grupos de usuários conhecidos com pacote de pré-lançamento, certifique-se de que você obteve o consentimento necessário das pessoas que acrescentar ao grupo de envio de versão de pré-lançamento e se elas entenderam que receberão pacotes diferentes do seu envio de versão completa. 

## <a name="to-edit-a-known-user-group"></a>Para editar um grupo de usuários conhecido

Não é possível remover um grupo de usuários conhecidos do painel (ou alterar seu nome) após a criação, mas você pode editar sua participação a qualquer momento.

Para revisar e editar os grupos de usuários conhecidos, expanda o menu **Envolver** no menu de navegação esquerdo e selecione **Grupos de clientes**. Em **Meus grupos de clientes**, selecione o nome do grupo que você deseja editar. Também é possível editar um grupo de usuários conhecido na página de criação do pacote de pré-lançamento ao selecionar **Exibir e gerenciar grupos existentes** ao criar uma nova versão de pré-lançamento ou selecionar o nome do grupo na página de visão geral de um pacote de pré-lançamento. 

Depois de selecionar o grupo que você deseja editar, é possível adicionar ou remover endereços de e-mail diretamente no campo.

Para alterações maiores, selecione **Exportar .csv** para salvar as informações de associação do grupo em um arquivo .csv. Faça suas alterações nesse arquivo, clique em **Importar .csv** para usar a nova versão para atualizar a associação do grupo.

Observe que a implementação das alterações na associação do grupo de versão de pré-lançamento pode demorar até 30 minutos. Você não precisa publicar um novo envio para que os novos membros do grupo poderão acessar seu envio por meio de pacotes de pré-lançamento ou de público particular; eles terão acesso assim que as alterações forem implementadas. 






