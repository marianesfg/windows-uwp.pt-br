---
author: jnHs
Description: Here’s some important info you’ll need to ensure that you receive payment for your apps, in-app products (IAPs), and advertising earnings.
title: Recebimento de pagamentos
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.author: wdg-dev-content
ms.date: 02/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, pagamentos, vendas de aplicativos, receita do aplicativo, pagamento, taxa da store, pagamento em espera, porcentagem
ms.localizationpriority: medium
ms.openlocfilehash: 0c128bedd1c889f4c2dcf0565c7c10575eb75013
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2830322"
---
# <a name="getting-paid"></a>Recebimento de pagamentos
Veja a seguir algumas informações importantes que serão necessárias para que você garanta o recebimento de pagamentos de ganhos com seus apps, complementos e anúncios.

> [!IMPORTANT]
> Antes de receber dinheiro pela venda de aplicativos na Microsoft Store, você deve [configurar sua conta de pagamento e preencher os formulários de impostos necessários](setting-up-your-payout-account-and-tax-forms.md).

## <a name="store-fee"></a>Taxa da loja

Ao [se inscrever para uma conta de desenvolvedor](http://go.microsoft.com/fwlink/p/?LinkID=615100), você aceita o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Esse contrato explica o relacionamento entre você e a Microsoft em relação à venda de apps na Microsoft Store, incluindo a taxa da Store cobrada pela Microsoft por cada venda feita.

Em muitos casos, a taxa da Loja é 30%. As taxas estão oficialmente definidas no [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Sempre que tiver alguma dúvida, leia este documento.

A taxa da Store é aplicada a todas as vendas de aplicativos recolhidas pela Microsoft Store, incluindo complementos.


## <a name="price-tiers"></a>Faixas de preço

A faixa de preço selecionada define o [preço de venda](set-and-schedule-app-pricing.md#base-price) em todos os países/regiões em que você decidir distribuir seu aplicativo. Você também pode usar recursos adicionais de preços, como [escolher preços diferentes para diferentes mercados](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) ou [colocar seu aplicativo em promoção](put-apps-and-add-ons-on-sale.md).

Você pode oferecer seu aplicativo gratuitamente ou pode especificar o preço que os clientes deverão pagar para comprá-lo. As faixas de preço começam em US$ 0,99, com incrementos adicionais (US$ 1,29, US$ 1,49, US$ 1,99 e assim por diante). Os incrementos entre as faixas de preço aumentam conforme o preço fica mais alto.

> [!NOTE] 
> Essas faixas de preço também se aplicam a qualquer complemento que você ofereça no aplicativo.

Cada faixa de preço tem um valor correspondente em cada uma das moedas oferecidas pela Loja. Usamos esses valores para ajudar você a vender seus aplicativos por um preço proporcional em todo o mundo. No entanto, devido a alterações em taxas cambiais, o valor exato das vendas pode sofrer uma pequena variação de uma moeda para a outra.

Você também tem a opção de inserir um preço livre de sua escolha na moeda local de um mercado específico. Ao fazer isso, o preço não será ajustado (mesmo que as taxas de conversão mudem), a não ser que você envie uma atualização com um novo preço. 

Lembre-se de que o preço selecionado pode incluir impostos sobre vendas ou valor agregado que seus clientes devem pagar. Para saber mais, consulte [Detalhes fiscais de aplicativos pagos](tax-details-for-paid-apps.md).


## <a name="payout-reporting"></a>Relatório de pagamento

Você pode acessar detalhes sobre suas informações de pagamento e baixar relatórios no **Resumo de pagamento** do painel Centro de Desenvolvimento do Windows. Para saber mais sobre as informações mostradas aqui, e como categorizar o dinheiro que você ganhar, consulte [Resumo de pagamento](payout-summary.md).


## <a name="payout-timeframe"></a>Período de pagamento

Os pagamentos são feitos mensalmente (desde que o limite de pagamento aplicável tenha sido atingido e você ainda não tenha colocado seu pagamento em espera conforme descrito abaixo). Normalmente, enviaremos qualquer pagamento pendente até o 15º dia do dito mês. Observe que os pagamentos geralmente levam entre 3 e 10 dias úteis adicionais para alcançar sua conta de pagamento. Para obter mais informações, consulte [Limites, métodos e prazos de pagamento](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>Status de espera de pagamento

Por padrão, enviaremos pagamentos mensalmente, conforme descrito anteriormente. No entanto, você tem a opção de colocar seus pagamentos em espera, o que evitará que enviemos pagamentos à sua conta. Se optar por colocar seus pagamentos em espera, continuaremos a registrar a receita que você ganha e fornecer os detalhes em seu **Resumo de pagamentos**. Porém, não enviaremos nenhum pagamento à sua conta até que você remova a espera. 

Para colocar seus pagamentos em espera, acesse **Configurações da Conta**. Em **Detalhes financeiros**, na seção **Status de espera de pagamento**, alterne para **Ativado**. Você pode alterar seu status de espera de pagamento a qualquer momento, mas lembre-se de que sua decisão afetará o próximo pagamento mensal. Por exemplo, se você quiser colocar o pagamento de abril em espera, defina seu status de espera de pagamento para **Ativado** antes do final de março.

Depois de definir o status de espera de pagamento para **Ativado**, todos os pagamentos ficarão em espera até que você alterne novamente para **Desativado**. Quando fizer isso, você estará incluído no próximo ciclo de pagamento mensal (desde que tenham sido atingidos os limites de pagamento aplicáveis). Por exemplo, se você tem seus pagamentos em espera, mas gostaria de ter um pagamento gerado em junho, alterne o status de espera de pagamento para **Desativado** antes do final de maio.

> [!NOTE]
> A seleção de **Status de pagamento em espera** se aplica a **todas** as fontes de receita pagas por meio do Centro de Desenvolvimento do Windows (Microsoft Store, anúncios, Azure Marketplace etc.) Você não pode selecionar um status de pagamento em espera diferente para cada fonte de receita.


 

 




