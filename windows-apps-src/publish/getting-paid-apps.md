---
Description: Saiba mais sobre como receber pagamentos para seus aplicativos, Complementos (produtos no aplicativo) e ganhos de publicidade.
title: Ser pago
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: windows 10, uwp, pagamentos, vendas de aplicativos, receita do aplicativo, pagamento, taxa da store, pagamento em espera, porcentagem
ms.localizationpriority: medium
ms.openlocfilehash: 0d42677aeda694e2fc8924cee1832b62d98b15e5
ms.sourcegitcommit: a937963ce63a14c254420926661b9b68be28a8ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84746766"
---
# <a name="getting-paid"></a>Ser pago
Aqui estão algumas informações importantes sobre o recebimento de pagamentos para seus aplicativos, Complementos e anúncios de publicidade.

> [!IMPORTANT]
> Antes de poder receber dinheiro das vendas de aplicativos na Microsoft Store, você precisa [configurar sua conta de pagamento e preencher os formulários de imposto necessários](setting-up-your-payout-account-and-tax-forms.md).

> [!NOTE]
> Se você estiver procurando suporte em relação a pagamentos, incluindo a configuração de contas de pagamento, pagamentos ausentes, colocação de pagamentos em espera ou qualquer outra coisa, entre em contato com o suporte [aqui](https://developer.microsoft.com/windows/support).

## <a name="store-fee"></a>Taxa da loja

Ao [se inscrever para uma conta de desenvolvedor](https://developer.microsoft.com/store/register), você aceita o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Esse contrato explica o relacionamento entre você e a Microsoft em relação à venda de apps na Microsoft Store, incluindo a taxa da Store cobrada pela Microsoft por cada venda feita.

As taxas estão oficialmente definidas no [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Sempre que tiver alguma dúvida, leia este documento.

A taxa da Store é aplicada a todas as vendas de aplicativos recolhidas pela Microsoft Store, incluindo complementos.


## <a name="price-tiers"></a>Faixas de preço

A faixa de preço selecionada define o [preço de venda](set-and-schedule-app-pricing.md#base-price) em todos os países/regiões em que você decidir distribuir seu aplicativo. Você também pode usar recursos adicionais de preços, como [escolher preços diferentes para diferentes mercados](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) ou [colocar seu aplicativo em promoção](put-apps-and-add-ons-on-sale.md).

Você pode oferecer seu aplicativo gratuitamente ou pode especificar o preço que os clientes deverão pagar para comprá-lo. As faixas de preço começam em US$ 0,99, com incrementos adicionais (US$ 1,29, US$ 1,49, US$ 1,99 e assim por diante). Os incrementos entre as faixas de preço aumentam conforme o preço fica mais alto.

> [!NOTE] 
> Essas faixas de preço também se aplicam a qualquer complemento que você ofereça no aplicativo.

Cada faixa de preço tem um valor correspondente em cada uma das moedas oferecidas pela Loja. Usamos esses valores para ajudar você a vender seus aplicativos por um preço proporcional em todo o mundo. No entanto, devido a alterações em taxas cambiais, o valor exato das vendas pode sofrer uma pequena variação de uma moeda para a outra. As taxas de câmbio são calculadas mensalmente. Com base em quando sua transação ocorreu, a taxa de câmbio apropriada é aplicada. A taxa de câmbio e o intervalo de datas para o qual ele estava em vigor são indicados em seu relatório de pagamento nas colunas exdisqueteiraname e exchangeRateDate, respectivamente.

Você também tem a opção de inserir um preço livre de sua escolha na moeda local de um mercado específico. Ao fazer isso, o preço não será ajustado (mesmo que as taxas de conversão mudem), a não ser que você envie uma atualização com um novo preço. 

Lembre-se de que o preço selecionado pode incluir impostos sobre vendas ou valor agregado que seus clientes devem pagar. Para saber mais, consulte [Detalhes fiscais de aplicativos pagos](tax-details-for-paid-apps.md).


## <a name="payout-reporting"></a>Relatório de pagamento

Você pode acessar detalhes sobre suas informações de pagamento e baixar relatórios no **Resumo de pagamento** do [Partner Center](https://partner.microsoft.com/dashboard). Para saber mais sobre as informações mostradas aqui, e como categorizar o dinheiro que você ganhar, consulte [Resumo de pagamento](payout-summary.md).


## <a name="payout-timeframe"></a>Período de pagamento

Os pagamentos são feitos mensalmente (desde que o limite de pagamento aplicável tenha sido atingido e você ainda não tenha colocado seu pagamento em espera conforme descrito abaixo). Normalmente, enviaremos qualquer pagamento pendente até o 15º dia do dito mês. Observe que os pagamentos geralmente levam entre 3 e 10 dias úteis adicionais para alcançar sua conta de pagamento. Para obter mais informações, consulte [Limites, métodos e períodos de pagamento](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>Status de pagamento em espera

Por padrão, enviaremos pagamentos mensalmente, conforme descrito anteriormente. No entanto, você tem a opção de colocar os pagamentos de um programa em espera, o que nos impedirá de enviar os pagamento para sua conta. Se você optar por colocar seus pagamentos em espera, continuaremos registrando qualquer receita obtida e forneceremos os detalhes em seu **Resumo de pagamento**. Porém, não enviaremos nenhum pagamento à sua conta até que você remova a espera.

Para posicionar seus pagamentos em espera, vá para **configurações do desenvolvedor**. Em **pagamento e imposto**, na seção **atribuição de perfil de pagamento e imposto** , localize o programa para o qual deseja que os pagamentos sejam mantidos. Clique na caixa de seleção **manter meu pagamento** para manter os pagamentos deste programa. Você pode alterar o status de pagamento em espera a qualquer momento, mas lembre-se de que sua decisão afetará o próximo pagamento mensal. Por exemplo, se você quiser manter o pagamento de abril, certifique-se de definir seu status de espera de pagamento como **ativado** antes do fim de março.

Depois de definir seu status de espera de pagamento como **ativado**, todos os pagamentos desse programa estarão em espera até que você alterne o controle deslizante de volta para **desativado**. Quando fizer isso, você estará incluído no próximo ciclo de pagamento mensal (desde que tenham sido atingidos os limites de pagamento aplicáveis). Por exemplo, se você tiver seus pagamentos em espera, mas quiser ter um pagamento gerado em junho, certifique-se de alternar o status de espera de pagamento para **desativado** antes do final de maio.

> [!NOTE]
> O **status de retenção de pagamento** se aplica a cada programa individualmente (Microsoft Store, publicidade, Azure Marketplace, etc.). Se desejar manter os pagamentos em todos os seus programas, você deverá manter os pagamentos em cada programa individualmente.


 

 




