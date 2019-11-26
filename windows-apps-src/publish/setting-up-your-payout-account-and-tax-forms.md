---
Description: Para receber dinheiro das vendas de aplicativos no Microsoft Store, você precisa configurar sua conta de pagamento e preencher os formulários de imposto necessários.
title: Configurar a conta de pagamento e formulários de imposto
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a56630a0a2f0acdc71241ac0234cad463e45ace
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259907"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>Configurar a conta de pagamento e formulários de imposto

Para receber dinheiro das vendas de aplicativos na Microsoft Store, você precisa configurar sua conta de pagamento e preencher os formulários de impostos necessários no [Partner Center](https://partner.microsoft.com/dashboard).

Se você pretende listar apenas os aplicativos gratuitos (e não planeja oferecer compras no aplicativo ou use o Microsoft Advertising), não precisa configurar uma conta de pagamento nem preencher formulários tributários. Se você mudar de ideia posteriormente e decidir que deseja vender aplicativos (ou complementos), poderá configurar sua conta de pagamento e preencher os formulários de impostos nesse momento. Você não poderá enviar aplicativos pagos ou complementos até que sua conta de pagamento e perfil fiscal tenham sido concluídos.

> [!NOTE]
> Em [alguns mercados](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), os desenvolvedores podem enviar somente aplicativos gratuitos. Sua conta precisa estar registrada em um desses mercados para que você tenha a opção de configurar uma conta de pagamento.

Depois de [configurar sua conta de desenvolvedor](opening-a-developer-account.md), há duas coisas que você precisa fazer para poder vender aplicativos (ou complementos) no Microsoft Store:

- [Preencha seus formulários de imposto](#tax-forms)
- [Configurar sua conta de pagamento](#payout-account)

> [!NOTE]
> Para saber detalhes de como e quando você receberá o dinheiro obtido com os aplicativos, veja [Sendo pago](getting-paid-apps.md).

## <a name="tax-forms"></a>Formulários fiscais

### <a name="filling-out-your-tax-forms"></a>Preenchendo seus formulários de imposto

Primeiro, você precisará criar um perfil de imposto e atribuí-lo aos programas que participa. Você pode criar seu *perfil de imposto* para o Microsoft Store concluindo as seguintes etapas:

- Especificar seu país/região de residência e cidadania.
- Preencher os formulários fiscais apropriados.

Você pode concluir e enviar seus formulários de impostos eletronicamente no Partner Center; na maioria dos casos, você não precisa imprimir e enviar nenhum formulário.

> [!IMPORTANT]
> Os diversos países e regiões têm requisitos fiscais diferentes. O valor exato dos impostos que você precisa pagar depende dos países e das regiões onde seus aplicativos são vendidos. Veja o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) para saber em quais países/regiões a Microsoft paga os impostos sobre vendas e uso em seu nome. Em outros países/regiões, dependendo de onde você está registrado, talvez seja necessário remeter os impostos sobre vendas e uso referentes às vendas de seu aplicativo diretamente para a autoridade fiscal local. Além disso, as receitas de vendas do aplicativo que você recebe podem ser taxadas como renda. É altamente recomendável que você entre em contato com a autoridade relevante para seu país ou região que possa ajudá-lo a determinar as informações de impostos corretas para suas atividades Microsoft Store Developer.

1. No [Partner Center](https://partner.microsoft.com/dashboard), selecione o ícone **configurações de conta** no canto superior direito e, em seguida, selecione configurações do **desenvolvedor**.
2. No menu de navegação à esquerda, selecione **pagamento e imposto**e, em seguida, selecione **pagamento e atribuições de impostos**.

    ![Atribuição de perfil de pagamento e imposto](images/payout-tax-profile-assignment.png)

3. Selecione a combinação programa e ID do vendedor para a qual você deseja configurar as informações do imposto.

    ![Pagamento selecionar ID do vendedor](images/payout-select-seller-id.png)

4. Se você quiser usar um perfil de imposto existente, selecione-o na lista suspensa. Caso contrário, selecione **criar novo perfil** e pressione **Enviar**. Você será levado para a página de perfis de impostos.
5. Clique no botão **Editar** para editar suas informações de imposto.
6. Selecione o botão de opção apropriado e selecione seu país, se solicitado. Esta etapa determina a entidade de negócios da Microsoft que será usada para fazer pagamentos em sua conta.

    ![Selecionar pagamento do país do imposto](images/payout-select-tax-country.png)

7. Dependendo de suas seleções na etapa 6, você será solicitado a fornecer informações de impostos necessárias para seu país.

> [!NOTE]
> Independentemente do seu país de residência ou cidadania, você deve preencher Estados Unidos formulários de impostos para vender quaisquer aplicativos ou complementos por meio do Microsoft Store. Os desenvolvedores que atendem a determinados requisitos de residência nos Estados Unidos precisam preencher um formulário IRS W-9. Outros desenvolvedores fora dos Estados Unidos precisam preencher um formulário IRS W-8. Você pode preencher esses formulários online enquanto preenche seu perfil fiscal.

### <a name="withholding-rates"></a>Retenção de impostos

As informações que você envia em seus formulários fiscais determinam a taxa de retenção de impostos apropriada. A retenção de impostos aplica-se somente a vendas realizadas nos Estados Unidos; as vendas feitas em locais fora dos EUA não estão sujeitas à retenção. As taxas de retenção podem variar, mas para a maioria dos desenvolvedores registrados fora dos Estados Unidos, a taxa padrão é de 30%. Você tem a opção de reduzir essa taxa, se o seu país/região tiver firmado um tratado de imposto de renda com os Estados Unidos.

### <a name="tax-treaty-benefits"></a>Benefícios de tratados fiscais

Se você está fora dos Estados Unidos, talvez possa tirar proveito de benefícios de tratados fiscais. Esses benefícios variam de país para país e podem permitir que você reduza a quantidade de impostos que a Microsoft Store retenção. Você pode reivindicar benefícios de tratados fiscais preenchendo a Parte II do formulário W-8BEN. Recomendamos que você entre em contato com os recursos apropriados em seu país ou sua região para determinar se esses benefícios se aplicam a você.

> [!NOTE]
> Não é necessário ter um ITIN (Número de Identificação de Contribuinte Individual) dos Estados Unidos para receber pagamentos da Microsoft ou reivindicar benefícios de tratados fiscais.

## <a name="payout-account"></a>Conta de pagamento

A conta de pagamento é a conta bancária para a qual enviamos a receita de suas vendas. Você pode exibir todas as contas de pagamento inseridas na página perfil.

> [!NOTE]
> Em alguns mercados, o PayPal pode ser usado para sua conta de pagamento. Confira [limites de pagamento, métodos e períodos de tempo](payment-thresholds-methods-and-timeframes.md) para descobrir se o Paypal tem suporte para um mercado específico e leia as [informações de PayPal](#paypal-info) abaixo para obter mais detalhes.

### <a name="create-a-payment-profile"></a>Criar um perfil de pagamento

1. No [Partner Center](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem **configurações** no canto superior direito e, em seguida, selecione **configurações do desenvolvedor**.
2. Abaixo do *pagamento e* do cabeçalho do imposto, selecione **pagamento e atribuição de perfil de imposto**.

    > [!NOTE]
    > Como se tratam de informações confidenciais, talvez você seja solicitado a entrar novamente.

3. Selecione o método de pagamento que você deseja configurar.

    ![Seleção do tipo de conta de pagamento](images/payout-account-type-selection.png)

4. Selecione um perfil de pagamento existente ou clique em **criar um novo perfil de pagamento** para criar um novo perfil para o método de pagamento escolhido.

> [!NOTE]
> Se, por algum motivo, sua conta não estiver pronta para receber fundos da Microsoft, você poderá marcar a caixa de seleção **manter meu pagamento** . Você continuará a ganhar suas vendas, mas os pagamentos não serão distribuídos até que você desabilite **o meu pagamento.**

### <a name="create-a-bank-based-payment-profile"></a>Criar um perfil de pagamento baseado no banco

Se você optou por usar uma conta bancária para receber pagamentos, você concluirá o processo a seguir para configurar sua conta bancária.

1. Na página *perfil bancário* , forneça as informações necessárias sobre seu banco.
2. Forneça os detalhes da sua conta bancária.

    > [!NOTE]
    > Os campos que você usa para inserir as informações da conta aceitam apenas caracteres alfanuméricos.

    ![Informações bancárias de pagamento](images/payout-bank-info.png)

3. Forneça os detalhes do beneficiário.
4. De volta à página *atribuição de perfil* , selecione a moeda que você gostaria de usar ao emitir seus pagamentos.

    > [!WARNING]
    > Verifique se seu banco aceita a moeda de pagamento que você selecionou.

5. Você precisará selecionar um perfil de pagamento para cada programa no qual você participa, embora possa usar o mesmo perfil para vários programas.

    ![Perfil bancário de uso de pagamento](images/payout-use-bank-profile.png)

6. Clique em enviar para salvar as alterações.

> [!NOTE]
> A Microsoft pode levar até 48 horas para validar as informações em seu perfil. Quando esse processo estiver concluído, o *status de verificação* mostrará **concluído**

Para garantir que seu pagamento seja bem-sucedido, também tenha em mente o seguinte:

- O **nome do titular da conta** inserido para sua conta de pagamento no Partner Center deve ter exatamente o mesmo nome associado à sua conta bancária. Por exemplo, se seu nome da conta bancária contiver um nome do meio, adicione-o ao **Nome do titular da conta**.
- Os pagamentos são transferidos diretamente da Microsoft para sua conta bancária na moeda USD.
- As informações bancárias inseridas no Partner Center em caracteres latinos são convertidas em caracteres cirílico.

### <a name="editing-existing-payment-profiles"></a>Editando perfis de pagamento existentes

Você pode editar os perfis de pagamento existentes se precisar fazer alterações ou corrigir informações incorretas.

1. No [Partner Center](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem **configurações** no canto superior direito e, em seguida, selecione **configurações do desenvolvedor**.
2. Abaixo do *pagamento e* do cabeçalho do imposto, selecione **pagamento e perfis de impostos**.
3. Seus perfis de pagamento serão listados junto com seu status. Localize o perfil que você deseja editar e clique em **Editar** na extrema direita

> [!IMPORTANT]
> A modificação dessa conta pode atrasar seus pagamentos em até um ciclo. Esse atraso ocorre porque nós temos que verificar a mudança na conta, da mesma forma que fazemos quando você configura a conta de pagamento pela primeira vez. Você receberá o valor total após a verificação da conta; todos os pagamentos vencidos no ciclo de pagamento atual serão adicionados ao próximo ciclo. Consulte [Obtendo pagamento](getting-paid-apps.md) para saber mais.

### <a name="paypal-info"></a>Informações do PayPal

Em alguns países e regiões, você pode criar uma conta de pagamento inserindo suas informações do PayPal. No entanto, antes de escolher o PayPal como opção de conta de pagamento:

- Verifique os [limites de pagamento, os métodos e os períodos de tempo](payment-thresholds-methods-and-timeframes.md) para confirmar se o Paypal é um método de pagamento com suporte em seu país ou região.
- Leia as perguntas frequentes a seguir. Dependendo da situação, o PayPal pode não ser a melhor opção de conta de pagamento para você e uma conta bancária pode ser preferencial.

Perguntas comuns sobre como usar o PayPal como forma de pagamento:

- **Quais configurações do PayPal preciso ter para receber pagamentos?** Você deve garantir que sua conta do PayPal não bloqueie pagamentos via eCheck. Essa configuração é gerenciada na página Preferências de Recebimento de Pagamento do PayPal. Consulte a [página de configuração da conta do PayPal](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/) para saber mais.
- **Há suporte para meu país/região?** Confira [limites de pagamento, métodos e períodos de tempo](payment-thresholds-methods-and-timeframes.md) para descobrir onde o Paypal é um método de pagamento com suporte.
- **Minha conta do PayPal precisa ser registrada no mesmo país/região que minha conta do centro de parceiros?** Não. Quando você configura uma conta do PayPal, é possível aceitar a configuração padrão. Você não deve ter problemas com outros países/regiões e moedas, a menos que tenha pagamento bloqueado em algumas moedas. Essa configuração é gerenciada na página Preferências de Recebimento de Pagamento do PayPal.
- **É necessário aceitar os pagamentos do PayPal manualmente?** Não. As contas do PayPal são configuradas por padrão para exigir que os usuários aceitem pagamentos manualmente, o que significa que se você não aceitar o pagamento dentro de 30 dias, ele é devolvido. Você pode alterar essa configuração desativando “Perguntar-me” na página Mais Configurações do PayPal.
- **A quais moedas o PayPal dá suporte?** Consulte a [página de suporte do PayPal](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) para a lista atual

### <a name="specific-requirements-for-certain-countriesregions"></a>Requisitos específicos para determinados países/regiões

Em alguns países e regiões, requisitos adicionais para contas de pagamento devem ser seguidos. Se você é residente do Paquistão, Rússia e da Ucrânia, observe os seguintes requisitos.

#### <a name="pakistan"></a>Paquistão

Formulário-R é uma exigência regulamentar bancária do Paquistão. Ele é usado para indicar a finalidade e a razão para o recebimento de fundos do exterior. Portanto, a qualquer momento que você tiver direito a um pagamento mensal da Microsoft, precisará enviar um formulário-R ao seu banco para que o pagamento seja liberado em sua conta. Entre em contato com sua filial bancária local para obter instruções sobre como obter uma cópia do formulário-R.

Você precisará enviar um formulário-R ao seu banco a cada mês que você tiver direito a um pagamento. Por exemplo, se você espera receber um pagamento todos os meses do ano, precisará enviar um formulário-R 12 vezes (uma vez por mês).

Uma vez enviado o pagamento ao seu banco, você tem 30 dias para enviar um formulário-R. Se não for enviado no prazo de 30 dias, os fundos serão devolvidos para a Microsoft.

#### <a name="russia"></a>Rússia

Se você for um desenvolvedor que vive na Rússia, talvez precise fornecer uma documentação ao seu banco antes que ele deposite fundos em sua conta. Quando você estiver qualificado para o pagamento, iremos fornecer-lhe a seguinte documentação em uma mensagem por email:

1. Certificado de Aceitação (AC) – contém o montante do pagamento transferido a sua conta.
2. Contrato do Desenvolvedor de Aplicativo (ADA) – uma cópia assinada do contrato de desenvolvedor que precisa ser contra-assinado.

Para garantir que seu pagamento seja bem-sucedido, também tenha em mente o seguinte:

- O **nome do titular da conta** inserido para sua conta de pagamento no Partner Center deve ter exatamente o mesmo nome associado à sua conta bancária. Por exemplo, se seu nome da conta bancária contiver um nome do meio, adicione-o ao **Nome do titular da conta**.
- Os pagamentos são transferidos diretamente da Microsoft para sua conta bancária na moeda rublo (RUB).
- As informações bancárias inseridas no Partner Center em caracteres latinos são convertidas em caracteres cirílico.
- Os pagamentos devem ser efetuados em uma conta bancária e não em um cartão bancário.

#### <a name="ukraine"></a>Ucrânia

Se você for um desenvolvedor que vive na Ucrânia, talvez precise fornecer uma documentação ao seu banco antes que ele deposite fundos em sua conta. Quando você estiver qualificado para o pagamento, iremos fornecer-lhe a seguinte documentação em uma mensagem por email:

1. Certificado de Aceitação (AC) – contém o montante do pagamento transferido a sua conta.
2. Contrato do Desenvolvedor de Aplicativo (ADA) – uma cópia assinada do contrato de desenvolvedor que precisa ser contra-assinado.
3. Acordo de Alteração (AA) - este documento pode ser usado por seu banco para ajudar a identificar os seus fundos de pagamento.

A Microsoft fornece os três documentos quando é feita a tentativa de efetuar seu primeiro pagamento. Para todos os pagamentos subsequentes, você só receberá o documento AC. Guarde os documentos ADA e AA no caso de precisar deles para receber pagamentos futuros do seu banco.

### <a name="create-a-paypal-payment-profile"></a>Criar um perfil de pagamento do PayPal

Se você optou por usar uma conta bancária para receber pagamentos, você concluirá o processo a seguir para configurar sua conta bancária.

1. Na página *paypal* , forneça as informações necessárias sobre sua conta do PayPal.
2. Forneça os detalhes da sua conta do PayPal.

    > [!NOTE]
    > Os campos que você usa para inserir as informações da conta aceitam apenas caracteres alfanuméricos.

    ![Informações de pagamento do PayPal](images/payout-paypal-info.png)

3. Forneça os detalhes do beneficiário.
4. De volta à página *atribuição de perfil* , selecione a moeda que você gostaria de usar ao emitir seus pagamentos.
5. Você precisará selecionar um perfil de pagamento para cada programa no qual você participa, embora possa usar o mesmo perfil para vários programas.
6. Clique em enviar para salvar as alterações.