---
Description: O Resumo de pagamentos mostra detalhes do dinheiro que você ganhou com os seus aplicativos e complementos. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.
title: Resumo do pagamento
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, resumo de pagamentos, extrato, pagamentos, lucros, pagamentos, pagamento, receita
ms.localizationpriority: medium
ms.openlocfilehash: 90238360ecc48beb974546dc5b49ac09c01407eb
ms.sourcegitcommit: 35a511c2b29ae3d5008612a5fc13d3eb6370d2d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67495731"
---
# <a name="payout-summary"></a>Resumo do pagamento

O **resumo de pagamento** mostra detalhes sobre o dinheiro ganhou com a Microsoft. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.

Se você vender produtos no Azure Marketplace, também verá informações sobre pagamentos bem-sucedidos em **Resumo de pagamento**. Para saber mais sobre o pagamento do Azure Marketplace, consulte [Políticas de Participação do Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) e [Microsoft Azure Marketplace Publisher Agreement](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Para se qualificar para o pagamento, sua receita deve atingir o [limite de pagamento](payment-thresholds-methods-and-timeframes.md) de US $50. Para obter detalhes sobre o limite de pagamento, consulte esta página e examine o contrato de desenvolvedor do aplicativo.

## <a name="access-the-payout-summary-pages"></a>Acessar as páginas de resumo de pagamento

Para abrir uma das páginas de resumo de pagamento:

1. Selecione o ícone de dinheiro no canto superior direito.
2. Selecione os pagamentos, histórico de transações, ou exportar dados.

## <a name="payments-page"></a>Página de pagamentos

Os totais nesta página representam todos os programas que você participa. Você pode filtrar por ID de participante, o programa, o ID de pagamento e o tipo de Earning. Valores são fornecidos em dólares americanos. O valor pago também é exibido no pagamento em moeda.

| Área                   | Descrição                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total pago neste ano   | O total combinado paga para você neste ano, em dólares americanos, para todos os seus programas.       |
| Próximo pagamento estimado | O único pagamento próxima chegando (mesmo se houver outras pessoas em breve), em dólares americanos. |
| Último pagamento           | O valor (em dólares americanos), o nome do programa e o programa do pagamento mais recente.           |
| Pagamentos por origem     | Quantidade de pagamentos em dólares americanos, representado pelo programa nos últimos 12 meses.           |
| Pagamentos               | Selecione pago ou pendente e, em seguida, classificar como desejar. Para obter detalhes adicionais de um pagamento específico, selecione modo de exibição. Para baixar uma cópia da instrução de remessa de pagamento, selecione o Download. Observe que os dados de histórico de transação podem levar até 24 horas para serem exibidas, portanto, você não poderá ver os ganhos associados imediatamente. |

Para exportar os dados nessa página, selecione Exportar e, em seguida, siga instruções na página de dados de exportação.

## <a name="transaction-history-page"></a>Página de histórico de transação

Esta página exibe todos os seus ganhos individuais, incluindo a data, tipo e a obtenção de cada um. Você pode selecionar um período de tempo, e você também pode filtrar por ID de registro, programa, ID de pagamento, tipo Earning, alavanca e Status. Dados estão disponíveis para o ano fiscal atual (1º de julho – 30 de junho) e os dois anos fiscais anteriores.

Para ver mais detalhes sobre um ganhar, selecione a seta para baixo no lado direito da página. Isso exibirá a alavanca, o valor de receita e o produto. Se por alguma razão, qualquer um dos dados não estão disponíveis, mas você precisa ter acesso a ele, entre em contato com [suporte](https://developer.microsoft.com/en-us/windows/support)]. Se a ganhar é o resultado de um ajuste e não uma transação, os campos de produto não serão exibidos.

Para exportar os dados de transação nesta página, selecione Exportar e, em seguida, siga instruções na página de dados de exportação. Os arquivos exportados da página de histórico de transações mostram dados na moeda da transação, ganhos na moeda da transação e dólares americanos, e o valor pago em pagar para moeda.

## <a name="payment-status"></a>Status do pagamento

| A obtenção de status           | Reason                                                                                                                                      | Ação de parceiro necessária?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Não processado              | A ganhar é elegível para pagamento. Ela permanece nesse estado por um período de resfriamento conforme definido na guia de programação para o programa de incentivo. | Não                                                         |
| Futuros                 | Ordem de pagamento gerado pendentes revisões internas antes que o pagamento é processado.                                                               | Não                                                         |
| Nota fiscal de imposto pendente      | Sua fatura de imposto está incompleta ou inválida.                                                                                                  | Você precisa atualizar sua fatura de imposto antes que você pode ser pago |
| Rejeitada durante a revisão   | O pagamento foi rejeitado durante a revisão.                                                                                                     | Entre em contato com [o suporte da Microsoft](https://developer.microsoft.com/en-us/windows/support) para obter detalhes                      |
| Failed (Falha)                   | O pagamento falhou devido a um erro de sistema do Microsoft.                                                                                         | Entre em contato com [o suporte da Microsoft](https://developer.microsoft.com/en-us/windows/support) para obter detalhes                      |
| Em andamento              | O pagamento está em andamento.                                                                                                                 | Não                                                         |
| Pagamento incorreto        | O pagamento recouping está em andamento.                                                                                                       | Não                                                         |
| enviado                     | O pagamento foi enviado ao seu banco.                                                                                                     | Não                                                         |
| Reprocessamento             | O pagamento encontrou um erro de sistema do Microsoft e está sendo reprocessado.                                                                  | Não                                                         |
| Inversão                 | O pagamento foi revertido pelo banco e será enviado novamente no próximo ciclo de pagamento.                                                     | Não                                                         |
| Nota fiscal de impostos rejeitada     | Sua fatura de imposto foi rejeitada durante a revisão. Todos os pagamentos pendentes serão em espera até que a revisão de nota fiscal de imposto seja concluída.                 | Entre em contato com [o suporte da Microsoft](https://developer.microsoft.com/en-us/windows/support) para obter detalhes                      |
| Nota fiscal de impostos sob revisão | Sua fatura de imposto está sendo revisada. O pagamento será liberado após aprovação de nota fiscal de imposto.                                   | Não                                                         |
| Rejeitado                 | O pagamento foi rejeitado pelo banco.                                                                                                      | Entre em contato com seu banco para obter detalhes.                             |

## <a name="export-data-page"></a>Página de dados de exportação

Siga as instruções nesta página para exportar os dados desejados.

Observações:

- Quando você acessa esta página de ambos os pagamentos ou a página de histórico de transações, não carregam seus filtros por meio do. Você precisará refazê-los na página de dados de exportação.
- A página de dados de exportação não atualiza por conta própria. Talvez você precise atualizar a página manualmente para ver os dados mais recentes.
- O filtro pode resultar em erro de não dados disponível. Isso significa que provavelmente você Mantive o padrão de período de tempo selecionado em três meses e, em seguida, selecionado uma ID de pagamento de ganhos fora esse período. Expanda seu período de tempo e tente novamente.

## <a name="payment-download-export"></a>Exportação de download de pagamento

Essa opção fornece um download dos pagamentos recebidas em seu banco para determinado programa, o imposto associado e agregados quantidade ganhando. Esse relatório é usado para muitos programas do Partner Center, portanto, algumas colunas podem ser não aplicáveis ao seu relatório. Essas colunas são marcadas abaixo.

| Nome da coluna              | Descrição                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participantID            | A identidade primária do parceiro ganhar sob o programa                                                                           |
| participantIDType        | Normalmente, id do programa para programas de incentivo e ID do vendedor para programas de Store                                                              |
| participantName          | Nome do parceiro de rendimentos                                                                                                             |
| programName              | Nome do programa de incentivo/armazenamento                                                                                                            |
| acumulado                   | Valor acumulado na moeda de pagamento para esse programa/participantID                                                                     |
| earnedUSD                | Valor acumulado para a ID do participante do programa, em dólares americanos                                                                                    |
| withheldTax              | Valor do imposto retido na moeda de pagamento para o programa/participantID                                                             |
| salesTax                 | Quantidade total de imposto sobre vendas a pagar para moeda para o programa/participantID                                                          |
| totalPayment             | Pagamento total na moeda local, exceto a retenção de imposto e incluindo o imposto sobre vendas (se aplicável) para o programa/participantID |
| currencyCode             | Pagar para o código de moeda                                                                                                                    |
| paymentMethod            | O método usado para pagar o parceiro (transferência eletrônica bancária, nota de crédito)                                                              |
| paymentID                | Identificador exclusivo para o pagamento. Esse número é normalmente visível no seu extrato bancário.                                               |
| paymentStatus            | Status do pagamento                                                                                                                          |
| paymentStatusDescription | Descrição amigável do status de pagamento                                                                                                  |
| paymentDate              | Data de pagamento foi enviado da Microsoft                                                                                                    |

## <a name="transaction-history-download-export"></a>Exportação de download do histórico de transação

Essa opção fornece um download de cada item de linha de rendimentos que você vê na página de histórico de transação, a obtenção de tipo, data, o valor da transação associada, cliente, produto e outros detalhes transacionais aplicáveis aos seus programas.

| Nome da coluna                    | Descrição                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| earningId                      | Identificador exclusivo para cada ganhar                                                                                                       |
| participantId                  | A identidade primária do parceiro ganhar sob o programa                                                                            |
| participantIdType              | ID do vendedor                                                                                                                                |
| participantName                | Nome do parceiro de rendimentos                                                                                                              |
| partnerCountryCode             | Local/país do parceiro rendimentos                                                                                                  |
| programName                    | Nome do programa de incentivo/armazenamento                                                                                                             |
| transactionId                  | Identificador exclusivo da transação                                                                                                    |
| transactionCurrency            | Moeda na qual ocorreu a transação original do cliente                                                                             |
| transactionDate                | Data da transação. Útil para programas onde muitas transações contribuem para a obtenção de um                                           |
| transactionExchangeRate        | Data de taxa de câmbio usada para mostrar a quantidade USD correspondente                                                                             |
| transactionAmount              | Valor da transação na moeda da transação original, com base na qual a obtenção de é gerada.                                              |
| transactionAmountUSD           | Quantidade de transações em USD                                                                                                                |
| Alavanca                          | Indica a regra de negócio para a ganhar                                                                                                  |
| earningRate                    | Taxa de incentivo aplicada na quantidade de transações para gerar ganhos                                                                      |
| quantity                       | Varia com base no programa. Indica a quantidade de cobrado para programas transacionais                                                            |
| earningType                    | Indica se é taxas, descontos, coop, sell etc.                                                                                          |
| earningAmount                  | A obtenção de valor na moeda da transação original                                                                                      |
| earningAmountUSD               | A obtenção de valor em dólares americanos                                                                                                                    |
| earningDate                    | Data da ganhar                                                                                                                      |
| calculationDate                | Data que de ganhar foi calculada no sistema                                                                                            |
| earningExchangeRate            | Taxa de câmbio usada para mostrar a quantidade USD correspondente                                                                                  |
| exchangeRateDate               | Data de taxa de câmbio usada para calcular EarningAmount USD                                                                                   |
| claimId                        | Sempre será em branco                                                                                                                     |
| paymentId                      | Identificador exclusivo para o pagamento. Esse número é normalmente visível no seu extrato bancário                                                 |
| paymentStatus                  | Status do pagamento                                                                                                                           |
| paymentStatusDescription       | Descrição amigável do status de pagamento                                                                                                   |
| customerId                     | Sempre será em branco                                                                                                                     |
| customerName                   | Sempre será em branco                                                                                                                     |
| partNumber                     | Sempre será em branco                                                                                                                     |
| productName                    | Nome do produto vinculado à transação                                                                                                       |
| productId                      | Identificador exclusivo do produto                                                                                                                |
| parentProductId                | Identificador exclusivo do produto pai. Observação: se não houver um produto pai para a transação, então ID do produto pai = ID do produto (Product ID). |
| parentProductName              | Nome do produto pai. Observação: se não houver um produto pai para a transação, então Nome do produto pai = Nome do produto.   |
| productType                    | Tipo de produto (por exemplo, app, complemento, jogo, etc.)                                                                                        |
| invoiceNumber                  | Sempre será em branco                                                                                                                     |
| subscriptionId                 | Sempre será em branco                                                                                                                     |
| subscriptionStartDate          | Sempre será em branco                                                                                                                     |
| subscriptionEndDate            | Sempre será em branco                                                                                                                     |
| resellerId                     | Sempre será em branco                                                                                                                     |
| resellerName                   | Sempre será em branco                                                                                                                     |
| distributorId                  | Sempre será em branco                                                                                                                     |
| distributorName                | Sempre será em branco                                                                                                                     |
| agreementNumber                | Sempre será em branco                                                                                                                     |
| agreementStartDate             | Sempre será em branco                                                                                                                     |
| agreementEndDate               | Sempre será em branco                                                                                                                     |
| Carga de trabalho                       | Sempre será em branco                                                                                                                     |
| transactionType                | Tipo de transação (por exemplo, compra, reembolso, estorno, etc.)                                                               |
| localProviderSeller            | Vendedor ou provedor local do registro                                                                                                          |
| taxRemitted                    | Valor do imposto remetido (vendas, uso ou impostos IVA/GST).                                                                                   |
| taxRemitModel                  | Parte responsável por remeter impostos (vendas, uso ou impostos IVA/GST).                                                                    |
| storeFee                       | O valor mantido pela Microsoft como uma taxa por disponibilizar o aplicativo ou o complemento da Store.                                           |
| transactionPaymentMethod       | A forma de pagamento do cliente usada na transação (por exemplo, cartão de crédito, conta de celular, PayPal, etc)                                |
| tpan                           | Indica a rede de anúncios de produtos de terceiros                                                                                                     |
| purchaseTypeCode               | Sempre será em branco                                                                                                                     |
| purchaseOrderType              | Sempre será em branco                                                                                                                     |
| purchaseOrderCoverageStartDate | Sempre será em branco                                                                                                                     |
| purchaseOrderCoverageEndDate   | Sempre será em branco                                                                                                                     |
| externalReferenceId            | Sempre será em branco                                                                                                                     |
| externalReferenceIdLabel       | Sempre será em branco                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Exportação de download de instrução de pagamento (herdado)

Por um período limitado na página de resumo de pagamento antigo, instruções de pagamento serão disponibilizado para download. Esse relatório contém os campos a seguir.

> [!NOTE]
> Histórico de transações herdadas tem uma coluna chamada "Reservados", que corresponde à coluna "Ganhos" no histórico de moderno, exceto que ele exclui todos os ganhos com status = "Pagamento enviado".

| Nome do campo              | Descrição                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fonte de Receita          | A fonte de sua receita, com base no local onde ocorreu a transação (por exemplo, Microsoft Store, Loja do Windows Phone, Windows Store 8, anúncio etc.)                  |
| ID do Pedido                | Identificador exclusivo do pedido. Essa ID permite identificar as transações de compra com suas respectivas transações de não compra (como reembolsos, estornos, etc.). Ambas terão a mesma ID de Pedido. Além disso, no caso de uma cobrança dividida, onde várias formas de pagamento são usadas em uma compra única, isso permitirá que você vincule as transações de compra. |
| ID da transação          | Identificador exclusivo da transação.                                                                                                                                          |
| Data/hora da transação   | A data e a hora em que a transação ocorreu (UTC).                                                                                                                       |
| ID do Produto (Product ID) pai       | Identificador exclusivo do produto pai. Observação: se não houver um produto pai para a transação, então ID do produto pai = ID do produto (Product ID).                                |
| ID do Produto              | Identificador exclusivo do produto.                                                                                                                                              |
| Nome do produto pai     | Nome do produto pai. Observação: se não houver um produto pai para a transação, então Nome do produto pai = Nome do produto.                                  |
| Nome do Produto            | Nome do produto.                                                                                                                                                    |
| Tipo do Produto            | Tipo de produto (por exemplo, app, complemento, jogo, etc.)                                                                                                                       |
| Quantidade                | Quando a fonte de receita é a Microsoft Store para Empresas, a quantidade representa o número de licenças adquiridas. Para todas as outras fontes de receita, a quantidade sempre será 1. Observação: mesmo quando uma única transação é dividida em dois itens de linha porque foram usados dois métodos de pagamento diferentes, cada item de linha mostrará uma quantidade de 1. |
| Tipo de transação        | Tipo de transação (por exemplo, compra, reembolso, estorno, etc.)                                                                                              |
| Forma de pagamento          | A forma de pagamento do cliente usada na transação (por exemplo, cartão de crédito, conta de celular, PayPal, etc)                                                               |
| País/região        | País/região onde ocorreu a transação.                                                                                                                          |
| Vendedor ou provedor local | Vendedor ou provedor local do registro.                                                                                                                                        |
| Moeda da transação    | Moeda da transação.                                                                                                                                            |
| Valor da transação      | Valor da transação.                                                                                                                                              |
| Imposto remetido            | Valor do imposto remetido (vendas, uso ou impostos IVA/GST).                                                                                                                  |
| Recebimentos líquidos            | Valor da transação menos imposto remetido.                                                                                                                                   |
| Taxa da Loja               | O percentual dos Recebimentos Líquidos retidos pela Microsoft como taxa pela disponibilização do aplicativo ou do complemento na Loja.                                                      |
| Receita do aplicativo            | Recebimentos líquidos menos a taxa da Loja.                                                                                                                                       |
| Impostos retidos          | Valor do imposto de renda retido. (Não incluído no arquivo .csv **Reservado**.)                                                                                                |
| Pagamento                 | Receita do aplicativo menos retenção de imposto de renda aplicável (valor mostrado na moeda da transação). (Não incluído no arquivo .csv **Reservado**.)                               |
| Taxa de câmbio                 | Taxa de câmbio usada para converter a moeda da transação em moeda do pagamento.                                                                                         |
| Moeda do pagamento        | Moeda usada em seu pagamento.                                                                                                                                       |
| Pagamento convertido       | Valor do pagamento convertido na moeda do pagamento usando a taxa de câmbio.                                                                                                         |
| Modelo de remessa de imposto         | Parte responsável por remeter impostos (vendas, uso ou impostos IVA/GST).                                                                                                   |
| Data e hora da qualificação   | A data e a hora quando a receita da transação se qualificou para pagamento (UTC). Quando criado, um pagamento inclui a receita da transação com uma data e hora de qualificação anterior à data de criação do pagamento. (Incluído apenas no arquivo .csv **Reservado**.) |
| Encargos                 | Mostra uma divisão de todos os detalhes de cobrança agregados na coluna Valor da Transação. (Incluído apenas para Azure Marketplace; não incluído no arquivo .csv **Reservado**.) |
