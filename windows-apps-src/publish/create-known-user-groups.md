---
author: JnHs
Description: "Saiba como criar grupos de usuários conhecidos para uso na liberação de versões de pré-lançamento e muito mais."
title: "Criar grupos de usuários conhecidos"
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, segmento, segmentos, grupo de destino, clientes
ms.openlocfilehash: fc3986520e55ae0c636eb2db731df065463002b5
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
# <a name="create-known-user-groups"></a>Criar grupos de usuários conhecidos

Grupos de usuários conhecidos permitem que você adicione pessoas específicas a um grupo usando o endereço de e-mail associado à sua conta da Microsoft. Esses grupos de usuários conhecidos são mais frequentemente usados com [pacotes de pré-lançamento](package-flights.md) para distribuir pacotes específicos a um grupo selecionado de pessoas. Eles podem ser usados para enviar [notificações direcionadas](send-push-notifications-to-your-apps-customers.md) ou [ofertas direcionadas](use-targeted-offers-to-maximize-engagement-and-conversions.md) para um grupo de clientes específicos como parte das campanhas de envolvimento.

Para ser considerado um membro do grupo, cada pessoa deve ser autenticada na Loja usando a conta da Microsoft associada ao endereço de e-mail fornecido. Para pacotes de pré-lançamento, eles devem estar usando [um dispositivo Windows 10 compatível com o pacote de pré-lançamento](package-flights.md) para baixar o aplicativo.


## <a name="to-create-a-known-user-group"></a>Para criar um grupo de usuários conhecido

1.  No painel do Centro de Desenvolvimento do Windows, expanda **Envolver** no menu de navegação esquerdo e, em seguida, selecione **Grupos de clientes**. 
2.  Na seção **Meus grupos de clientes**, selecione **Criar novo grupo**.
3.  Na próxima página, selecione o botão de opção **Grupo de usuários conhecido**.
4.  Na caixa **Nome do grupo**, insira um nome para o grupo de usuários conhecido.
5.  Insira os endereços de e-mail das pessoas que você gostaria de adicionar ao grupo. Você deve incluir pelo menos um endereço de e-mail, com limite máximo de 10.000. Você pode inserir endereços de e-mail diretamente no campo (separados por espaços, vírgulas, ponto-e-vírgula ou quebras de linha) ou clicar no link **Importar .csv** para criar o grupo de versões de pré-lançamento a partir de uma lista de endereços de e-mail em um arquivo .csv.
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

Observe que a implementação das alterações na associação do grupo de versão de pré-lançamento pode demorar até 30 minutos. Se você adicionar pessoas a um grupo de usuários conhecido depois de publicar um pacote de pré-lançamento para o grupo, os pacotes serão enviados para o novo pessoal automaticamente; você não precisa criar e publicar um novo envio para esse pacote de pré-lançamento. 






