---
author: jnHs
ms.assetid: 284EBA1F-BFB4-4CDA-9F05-4927CDACDAA7
title: Abrindo uma conta de desenvolvedor
description: Esta visão geral sobre como registrar-se para uma conta de desenvolvedor para a Microsoft Store e outros programas da Microsoft ajudará você entender o processo de configuração de sua conta.
ms.author: wdg-dev-content
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b707479d11cc7aef62385b476720bff8477ed401
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4389800"
---
# <a name="opening-a-developer-account"></a>Abrindo uma conta de desenvolvedor

Esta visão geral sobre como registrar-se para uma conta de desenvolvedor para a Microsoft Store e outros programas da Microsoft ajudará você entender o processo de configuração de sua conta.

> [!NOTE]
> Quando você se inscreve em uma conta de desenvolvedor, o endereço de email fornecido nas informações de contato é usado para enviar comunicações por email relacionadas à sua conta. Às vezes, isso incluir emails informativos sobre nossos programas. Mesmo que você opte por não receber esses emails informativos, [recusando-os](http://go.microsoft.com/fwlink/p/?LinkId=533280), ainda enviaremos emails transacionais (por exemplo, para que você saiba que seu aplicativo foi aprovado para certificação ou que um pagamento está sendo feito). Esses emails são uma parte fundamental da sua conta e, a menos que você feche sua conta, continuará a receber esses emails transacionais.

## <a name="the-account-signup-process"></a>O processo de inscrição de conta

> [!NOTE]
> Em alguns casos, as telas e os campos que você vir ao se registrar para obter uma conta de desenvolvedor podem variar um pouco em relação ao que é destacado a seguir. As informações básicas e o processo serão os mesmos.

1.  Acesse a [página de registro](http://go.microsoft.com/fwlink/p/?LinkId=615100) e clique em **Inscrever-se**.
2.  Se você ainda não estiver conectado com uma conta da Microsoft, entre agora ou crie uma nova conta da Microsoft. A conta da Microsoft usada aqui será a que você utilizará para entrar na conta de desenvolvedor.
3.  Selecione o [país/região](account-types-locations-and-fees.md#developer-account-and-app-submission-markets) em que você vive ou onde sua empresa está localizada. Não será possível alterar isso posteriormente.
4.  Selecione seu [tipo de conta de desenvolvedor](account-types-locations-and-fees.md) (individual ou empresa). Você não poderá alterar isso depois, por isso, esteja atento para escolher o tipo de conta correto.
5.  Informe o **nome de exibição do editor** que você deseja usar (50 caracteres ou menos). Selecione isso com cuidado, pois os clientes verão esse nome quando navegarem e reconhecerão seus aplicativos por esse nome. Para contas de empresa, certifique-se de usar o nome comercial ou registrado da sua organização. Observe que, se você inserir um nome já selecionado por outra pessoa ou se aparentemente alguém já tiver os direitos de usar esse nome, nós não permitiremos que você use esse nome. 

   > [!NOTE]
   > Certifique-se de ter os direitos de uso do nome inserido aqui. Se outra pessoa tiver registrado a marca ou detiver os direitos autorais sobre o nome escolhido, sua conta poderá ser fechada. Consulte o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) para saber mais. Se alguém estiver usando um nome de exibição do editor do qual você detenha a marca ou outros direitos jurídicos, [contate a Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777).    

6.  Insira as informações de contato que deseja usar para sua conta de desenvolvedor.

   > [!NOTE]
   > Usaremos essas informações para entrar em contato com você sobre assuntos relacionados à conta. Por exemplo, você receberá uma mensagem de confirmação por email após concluir o registro. Depois disso, nós enviaremos mensagens quando formos pagar você ou se você precisar corrigir alguma coisa com a sua conta. Também podemos enviar emails informativos conforme descrito acima, a menos que você recuse o recebimento de emails não transacionais.

   Se você estiver se registrando como empresa, também precisará inserir o nome, o endereço de email e o telefone da pessoa que aprovará a conta da sua empresa.

7.  Clique em **Avançar** para acessar a seção **Pagamento**.

8.  Insira suas informações de pagamento para a taxa única de registro. Se você tiver um código promocional que abranja o custo de registro, insira-o aqui. Caso contrário, forneça suas informações de cartão de crédito (ou PayPal em mercados com suporte). Observe que cartões de crédito pré-pagos não pode ser usados para essa compra. Quando tiver terminado, clique em **Avançar** para ir para a seção **Análise**.

9.  Analise suas informações de conta e confirme se tudo estiver correto. Leia e aceite os termos e as condições do [Contrato de Desenvolvedor de Aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Marque a caixa para indicar que você leu e aceitou os termos.

10.  Clique em **Concluir** para confirmar o registro. Seu pagamento será processado e enviaremos uma mensagem de confirmação para seu endereço de email.

Depois de se inscrever, sua conta passará pelo processo de verificação. Para contas individuais, verificamos se outra empresa já não está usando seu nome de exibição do editor. Para contas de empresa, o processo demora um pouco mais, pois também precisamos confirmar se você está autorizado a configurar a conta para sua empresa. Essa verificação pode levar desde alguns dias a até várias semanas e normalmente inclui um telefonema para sua empresa. Você pode verificar o status da verificação na página **Configurações da conta**.


## <a name="additional-guidelines-for-company-accounts"></a>Diretrizes adicionais para contas da empresa

> [!IMPORTANT]
> Para permitir que vários usuários acessem sua conta do Centro de desenvolvimento, recomendamos usar o Azure Active Directory para atribuir funções a usuários individuais (em vez de acesso de compartilhamento para a conta da Microsoft). Cada usuário pode acessar a conta do Centro de Desenvolvimento fazendo logon com as credenciais individuais do Azure AD. Para obter mais informações, consulte [Gerenciar usuários de contas](manage-account-users.md).

Ao criar uma conta da empresa, estas diretrizes podem ajudar se mais de uma pessoa precisar acessar a conta ao entrar com a conta da Microsoft que abriu a conta (em vez de como os usuários individuais adicionados à conta do Centro de desenvolvimento).

-   Crie sua conta da Microsoft usando um endereço de email que ainda não pertença a você ou outra pessoa, como MyCompany_DevCenter@outlook.com. Não use um endereço de email no domínio da sua empresa, especialmente se sua empresa já usa o Azure AD. (Como observado acima, você pode adicionar mais usuários do Azure da sua empresa AD posteriormente.)
-   Limite o acesso a essa conta da Microsoft ao menor número possível de usuários.
-   Configurar uma lista de distribuição de emails corporativos que inclua todos aqueles que precisam acessar a conta de desenvolvedor e adicione este endereço de email para o [informações de segurança associadas com a conta da Microsoft [(https://account.microsoft.com/security). Isso permite que todos os funcionários da lista recebam os códigos de segurança enviados para esse alias. Se a configuração de uma lista de distribuição não é possível, você pode adicionar o endereço de email de uma pessoa para suas informações de segurança, mas o proprietário desse endereço de email será o único que pode acessar e compartilhar o código de segurança quando solicitado (como quando novas informações de segurança são adicionadas à t Ele conta ou quando ele é acessado em um novo dispositivo).
-   Adicione um número de telefone da empresa às informações de segurança da conta da Microsoft. Tente usar um número que não exige uma extensão e seja acessível para os principais membros da equipe.
-   Em geral, solicite que os desenvolvedores usem [dispositivos confiáveis](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device) para fazer logon na conta de desenvolvedor da sua empresa. Todos os membros principais da equipe devem ter acesso a esses dispositivos confiáveis. Isso reduzirá a necessidade do envio de códigos de segurança durante o acesso à conta. Existe um limite quanto ao número de códigos que podem ser gerados por conta a cada semana.
-   Se você precisar permitir acesso à conta a partir de um computador não confiável, limite esse acesso a no máximo cinco desenvolvedores. Idealmente, esses desenvolvedores devem acessar a conta de computadores que compartilham o mesmo local geográfico e de rede.
-   Analise frequentemente as informações de segurança da empresa em https://account.microsoft.com/security para garantir que elas estejam atualizadas.


## <a name="microsoft-account-security"></a>Segurança de conta da Microsoft

Usamos as informações de segurança que você forneceu para aumentar o nível de segurança da sua conta da Microsoft ao associá-la a várias formas de identificação. Isso dificulta bastante o acesso não autorizado à conta da Microsoft (e à conta de desenvolvedor). Além disso, se você se esquecer da senha ou se alguém tentar acessar a sua conta, poderemos entrar em contato para confirmar se você é o proprietário e/ou restabelecer o controle adequado da conta.

Você deve ter no mínimo dois endereços de email ou números de telefone na sua conta da Microsoft. Recomendamos a adição do máximo possível. Lembre-se de que algumas informações de segurança precisam ser confirmadas antes de se tornarem válidas. Também revise suas informações de segurança com frequência e assegure que estejam atualizadas. Você pode gerenciar suas informações de segurança acessando https://account.microsoft.com/security e entrar usando sua conta da Microsoft. Para obter mais informações, consulte [as informações de segurança e códigos de segurança](https://support.microsoft.com/help/12428/microsoft-account-security-info-and-security-codes) .

Quando você entra no seu painel do Centro de desenvolvimento do Windows usando sua conta da Microsoft, o sistema pode solicitar que você verifique sua identidade enviando um código de segurança que você deve fornecer para concluir o processo de entrada. Recomendamos designar os computadores que você usa com frequência como *dispositivos confiáveis*. Quando você entrar em um dispositivo confiável, você geralmente não será solicitado um código, embora você possa ocasionalmente ser solicitado em situações específicas ou se você ainda não entrou nesse dispositivo há muito tempo. Consulte [Adicionar um dispositivo confiável para sua conta da Microsoft](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device) para obter mais informações.


## <a name="closing-your-account"></a>Fechando sua conta

As contas dos desenvolvedores não expiram, então, não há necessidade de renovar sua conta a fim de mantê-la aberto. Se você decidir fechar sua conta completamente, pode fazê-lo entrando em contato com o suporte.

Quando você fechar sua conta, é importante entender o que acontece com qualquer app que você tenha publicado na Microsoft Store:

-   Os clientes atuais do seu aplicativo ainda poderão usar o app. Entretanto, eles não poderão fazer compras no aplicativo.
-   Mesmo que o aplicativo continue disponível para os clientes que já o compraram, os detalhes do aplicativo serão removidos da Loja. Novos clientes não poderão comprar seu aplicativo.
-   O nome de seu aplicativo será liberado para possível uso de outro desenvolvedor.
-   Se houver algum saldo resultante de vendas anteriores do aplicativo, você pode solicitar o pagamento dessa quantia mesmo que o valor devido não obedeça o limite usual de pagamento.
