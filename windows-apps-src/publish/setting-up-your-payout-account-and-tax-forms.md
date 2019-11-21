---
Description: In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms.
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

In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms in [Partner Center](https://partner.microsoft.com/dashboard).

Se você pretende listar apenas os aplicativos gratuitos (e não planeja oferecer compras no aplicativo ou use o Microsoft Advertising), não precisa configurar uma conta de pagamento nem preencher formulários tributários. If you change your mind later and decide you do want to sell apps (or add-ons), you can set up your payout account and fill out tax forms at that time. Você não poderá enviar aplicativos pagos ou complementos até que sua conta de pagamento e perfil fiscal tenham sido concluídos.

> [!NOTE]
> Em [alguns mercados](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), os desenvolvedores podem enviar somente aplicativos gratuitos. Sua conta precisa estar registrada em um desses mercados para que você tenha a opção de configurar uma conta de pagamento.

After you have [set up your developer account](opening-a-developer-account.md), there are two things you need to do before you can sell apps (or add-ons) in the Microsoft Store:

- [Fill out your tax forms](#tax-forms)
- [Set up your payout account](#payout-account)

> [!NOTE]
> Para saber detalhes de como e quando você receberá o dinheiro obtido com os aplicativos, veja [Sendo pago](getting-paid-apps.md).

## <a name="tax-forms"></a>Formulários fiscais

### <a name="filling-out-your-tax-forms"></a>Filling out your tax forms

First, you'll need to create a tax profile and assign it to the programs you participate in. You can create your *tax profile* for the Microsoft Store by completing the following steps:

- Especificar seu país/região de residência e cidadania.
- Preencher os formulários fiscais apropriados.

You can complete and submit your tax forms electronically in Partner Center; in most cases, you don't need to print and mail any forms.

> [!IMPORTANT]
> Os diversos países e regiões têm requisitos fiscais diferentes. O valor exato dos impostos que você precisa pagar depende dos países e das regiões onde seus aplicativos são vendidos. Veja o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) para saber em quais países/regiões a Microsoft paga os impostos sobre vendas e uso em seu nome. Em outros países/regiões, dependendo de onde você está registrado, talvez seja necessário remeter os impostos sobre vendas e uso referentes às vendas de seu aplicativo diretamente para a autoridade fiscal local. Além disso, as receitas de vendas do aplicativo que você recebe podem ser taxadas como renda. We strongly encourage you to contact the relevant authority for your country or region that can best help you determine the right tax info for your Microsoft Store developer activities.

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Account settings** icon in the top right corner, then select **Developer settings**.
2. In the left navigation menu, select **Payout and tax**, then select **Payout and tax assignments**.

    ![Payout and tax profile assignment](images/payout-tax-profile-assignment.png)

3. Select the program and seller id combination for which you want to configure tax information.

    ![Payout select seller id](images/payout-select-seller-id.png)

4. If you would like to use an existing tax profile, select it from the dropdown. Otherwise, select **Create new profile** and press **Submit**. You will be taken to the tax profiles page.
5. Click the **Edit** button to edit your tax information.
6. Select the appropriate radio button, and select your country if prompted. This step determines the Microsoft business entity that will be used to make payouts on your account.

    ![Payout select tax country](images/payout-select-tax-country.png)

7. Depending on your selections in step 6, you will be prompted to provide tax information required for your country.

> [!NOTE]
> Regardless of your country of residence or citizenship, you must fill out United States tax forms to sell any apps or add-ons through the Microsoft Store. Os desenvolvedores que atendem a determinados requisitos de residência nos Estados Unidos precisam preencher um formulário IRS W-9. Outros desenvolvedores fora dos Estados Unidos precisam preencher um formulário IRS W-8. Você pode preencher esses formulários online enquanto preenche seu perfil fiscal.

### <a name="withholding-rates"></a>Retenção de impostos

As informações que você envia em seus formulários fiscais determinam a taxa de retenção de impostos apropriada. A retenção de impostos aplica-se somente a vendas realizadas nos Estados Unidos; as vendas feitas em locais fora dos EUA não estão sujeitas à retenção. As taxas de retenção podem variar, mas para a maioria dos desenvolvedores registrados fora dos Estados Unidos, a taxa padrão é de 30%. Você tem a opção de reduzir essa taxa, se o seu país/região tiver firmado um tratado de imposto de renda com os Estados Unidos.

### <a name="tax-treaty-benefits"></a>Benefícios de tratados fiscais

Se você está fora dos Estados Unidos, talvez possa tirar proveito de benefícios de tratados fiscais. These benefits vary from country to country, and may allow you to reduce the amount of taxes that the Microsoft Store withholds. Você pode reivindicar benefícios de tratados fiscais preenchendo a Parte II do formulário W-8BEN. Recomendamos que você entre em contato com os recursos apropriados em seu país ou sua região para determinar se esses benefícios se aplicam a você.

> [!NOTE]
> Não é necessário ter um ITIN (Número de Identificação de Contribuinte Individual) dos Estados Unidos para receber pagamentos da Microsoft ou reivindicar benefícios de tratados fiscais.

## <a name="payout-account"></a>Conta de pagamento

A conta de pagamento é a conta bancária para a qual enviamos a receita de suas vendas. You can view all payment accounts that you have entered on the Profile page.

> [!NOTE]
> Em alguns mercados, o PayPal pode ser usado para sua conta de pagamento. See [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to find out if PayPal is supported for a specific market, and read the [PayPal info](#paypal-info) below for more details.

### <a name="create-a-payment-profile"></a>Create a payment profile

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Settings** gear icon in the top right corner, then select **Developer settings**.
2. Underneath the *Payout and tax* heading, select **Payout and tax profile assignment**.

    > [!NOTE]
    > Como se tratam de informações confidenciais, talvez você seja solicitado a entrar novamente.

3. Select the payment method you would like to configure.

    ![Payout account type selection](images/payout-account-type-selection.png)

4. Select an existing payment profile, or click **Create a new payment profile** to create a new profile for the chosen payment method.

> [!NOTE]
> If, for some reason, your account is not ready to receive funds from Microsoft, you may check the **Hold my payment** checkbox. You will continue to earn proceeds from your sales, but payments will not be distributed until you disable **Hold my payment.**

### <a name="create-a-bank-based-payment-profile"></a>Create a bank-based payment profile

If you elected to use a bank account to receive payouts, you'll complete the following process to configure your bank account.

1. On the *Bank Profile* page, provide the required information about your bank.
2. Provide your bank account details.

    > [!NOTE]
    > Os campos que você usa para inserir as informações da conta aceitam apenas caracteres alfanuméricos.

    ![Payout bank info](images/payout-bank-info.png)

3. Provide beneficiary details.
4. Back on the *Profile assignment* page, select the currency you would like us to use when we issue your payouts.

    > [!WARNING]
    > Make sure your bank accepts the payout currency you select.

5. You will need to select a payment profile for each program you participate in, though you can use the same profile for multiple programs.

    ![Payout use bank profile](images/payout-use-bank-profile.png)

6. Click submit to save your changes.

> [!NOTE]
> Microsoft may take up to 48 hours to validate the information in your profile. When this process is complete *verification status* will show **Complete**

Para garantir que seu pagamento seja bem-sucedido, também tenha em mente o seguinte:

- The **Account holder name** entered for your payout account in Partner Center must be the exact same name associated with your bank account. Por exemplo, se seu nome da conta bancária contiver um nome do meio, adicione-o ao **Nome do titular da conta**.
- Os pagamentos são transferidos diretamente da Microsoft para sua conta bancária na moeda USD.
- Bank information entered in Partner Center in Latin characters is translated to Cyrillic characters.

### <a name="editing-existing-payment-profiles"></a>Editing existing payment profiles

You can edit existing payment profiles if you need to make changes or correct any incorrect information.

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Settings** gear icon in the top right corner, then select **Developer settings**.
2. Underneath the *Payout and tax* heading, select **Payout and tax profiles**.
3. Your payment profiles will be listed along with their status. Find the profile you wish to edit and click **Edit** at the far right

> [!IMPORTANT]
> A modificação dessa conta pode atrasar seus pagamentos em até um ciclo. Esse atraso ocorre porque nós temos que verificar a mudança na conta, da mesma forma que fazemos quando você configura a conta de pagamento pela primeira vez. Você receberá o valor total após a verificação da conta; todos os pagamentos vencidos no ciclo de pagamento atual serão adicionados ao próximo ciclo. Consulte [Obtendo pagamento](getting-paid-apps.md) para saber mais.

### <a name="paypal-info"></a>Informações do PayPal

Em alguns países e regiões, você pode criar uma conta de pagamento inserindo suas informações do PayPal. No entanto, antes de escolher o PayPal como opção de conta de pagamento:

- Check [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to confirm whether PayPal is a supported payment method in your country or region.
- Leia as perguntas frequentes a seguir. Dependendo da situação, o PayPal pode não ser a melhor opção de conta de pagamento para você e uma conta bancária pode ser preferencial.

Perguntas comuns sobre como usar o PayPal como forma de pagamento:

- **What PayPal settings do I need to have in order to receive payments?** Você deve garantir que sua conta do PayPal não bloqueie pagamentos via eCheck. Essa configuração é gerenciada na página Preferências de Recebimento de Pagamento do PayPal. Consulte a [página de configuração da conta do PayPal](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/) para saber mais.
- **Is my country/region supported?** See [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to find out where PayPal is a supported payment method.
- **Does my PayPal account have to be registered in the same country/region as my Partner Center account?** Não. Quando você configura uma conta do PayPal, é possível aceitar a configuração padrão. Você não deve ter problemas com outros países/regiões e moedas, a menos que tenha pagamento bloqueado em algumas moedas. Essa configuração é gerenciada na página Preferências de Recebimento de Pagamento do PayPal.
- **Do I have to accept PayPal payments manually?** Não. As contas do PayPal são configuradas por padrão para exigir que os usuários aceitem pagamentos manualmente, o que significa que se você não aceitar o pagamento dentro de 30 dias, ele é devolvido. Você pode alterar essa configuração desativando “Perguntar-me” na página Mais Configurações do PayPal.
- **What currencies does PayPal support?** Please see [PayPal's support page](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) for the current list

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

- The **Account holder name** entered for your payout account in Partner Center must be the exact same name associated with your bank account. Por exemplo, se seu nome da conta bancária contiver um nome do meio, adicione-o ao **Nome do titular da conta**.
- Os pagamentos são transferidos diretamente da Microsoft para sua conta bancária na moeda rublo (RUB).
- Bank information entered in Partner Center in Latin characters is translated to Cyrillic characters.
- Os pagamentos devem ser efetuados em uma conta bancária e não em um cartão bancário.

#### <a name="ukraine"></a>Ucrânia

Se você for um desenvolvedor que vive na Ucrânia, talvez precise fornecer uma documentação ao seu banco antes que ele deposite fundos em sua conta. Quando você estiver qualificado para o pagamento, iremos fornecer-lhe a seguinte documentação em uma mensagem por email:

1. Certificado de Aceitação (AC) – contém o montante do pagamento transferido a sua conta.
2. Contrato do Desenvolvedor de Aplicativo (ADA) – uma cópia assinada do contrato de desenvolvedor que precisa ser contra-assinado.
3. Acordo de Alteração (AA) - este documento pode ser usado por seu banco para ajudar a identificar os seus fundos de pagamento.

A Microsoft fornece os três documentos quando é feita a tentativa de efetuar seu primeiro pagamento. Para todos os pagamentos subsequentes, você só receberá o documento AC. Guarde os documentos ADA e AA no caso de precisar deles para receber pagamentos futuros do seu banco.

### <a name="create-a-paypal-payment-profile"></a>Create a PayPal payment profile

If you elected to use a bank account to receive payouts, you'll complete the following process to configure your bank account.

1. On the *PayPal* page, provide the required information about your PayPal account.
2. Provide your paypal account details.

    > [!NOTE]
    > Os campos que você usa para inserir as informações da conta aceitam apenas caracteres alfanuméricos.

    ![Payout paypal info](images/payout-paypal-info.png)

3. Provide beneficiary details.
4. Back on the *Profile assignment* page, select the currency you would like us to use when we issue your payouts.
5. You will need to select a payment profile for each program you participate in, though you can use the same profile for multiple programs.
6. Click submit to save your changes.