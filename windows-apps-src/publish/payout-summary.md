---
Description: Os relatórios de pagamento mostram detalhes sobre o dinheiro que você ganhou com seus aplicativos e Complementos. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.
title: Relatórios de pagamento
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, resumo de pagamentos, extrato, pagamentos, lucros, pagamentos, pagamento, receita
ms.localizationpriority: medium
ms.openlocfilehash: f4d8727a48cd68b304d515fe34082b4c4f632b4b
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210522"
---
# <a name="payout-reports"></a>Relatórios de pagamento

O **Resumo do pagamento** mostra detalhes sobre o dinheiro que você ganhou com a Microsoft. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.

Se você vender produtos no Azure Marketplace, também verá informações sobre pagamentos bem-sucedidos em **Resumo de pagamento**. Para saber mais sobre o pagamento do Azure Marketplace, consulte [Políticas de Participação do Microsoft Azure Marketplace](https://docs.microsoft.com/legal/marketplace/participation-policy) e [Microsoft Azure Marketplace Publisher Agreement](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Para ser elegível para pagamento, seus prosseguimentos devem alcançar o [limite de pagamento](payment-thresholds-methods-and-timeframes.md) de $50. Para obter detalhes sobre o limite de pagamento, consulte esta página e examine o contrato de desenvolvedor do aplicativo.

> [!NOTE]
> Se você estiver procurando suporte em relação a pagamentos, incluindo a configuração de contas de pagamento, pagamentos ausentes, colocação de pagamentos em espera ou qualquer outra coisa, entre em contato com o suporte [aqui](https://developer.microsoft.com/windows/support).

## <a name="access-the-payout-summary-pages"></a>Acessar as páginas de resumo do pagamento

Para abrir uma das páginas de resumo do pagamento:

1. Selecione o ícone de pagamento no canto superior direito.
2. Selecione histórico de transações, pagamentos ou exportar dados.

## <a name="transaction-history-page"></a>Página Histórico de transações

Esta página exibe todos os seus ganhos individuais, incluindo a data, o tipo e a conquista de cada um. Você pode selecionar um período de tempo para exibir e também pode filtrar por ID de registro, programa, ID de pagamento, tipo de conquista, alavanca e status. Os dados estão disponíveis para o ano fiscal atual (1º de julho a 30 de junho) e os dois anos fiscais anteriores.

Para ver mais detalhes sobre uma conquista, selecione a seta para baixo no lado direito da página. Isso exibirá a alavanca, o valor da receita e o produto. Se, por algum motivo, qualquer um desses dados estiver indisponível, mas você precisar acessá-lo, contate o [suporte](https://developer.microsoft.com/windows/support)]. Se a conquista for o resultado de um ajuste, e não de uma transação, os campos de produto não serão exibidos.

Para exportar qualquer um dos dados de transação nesta página, use a página **exportar dados** .

## <a name="payments-page"></a>Página de pagamentos

Os totais nessa página representam todos os programas que você participa. Você pode filtrar por ID de participante, programa, ID de pagamento e tipo de conquista. Os valores são fornecidos em dólares americanos. O valor pago também é exibido em pagar para moeda.

| Área                   | Descrição                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total pago neste ano   | O total combinado pago a você neste ano, em dólares americanos, para todos os seus programas.       |
| Próximo pagamento estimado | O próximo pagamento seguinte chegando a você (mesmo se houver outros em breve) em dólares americanos. |
| Último pagamento           | O valor (em dólares americanos), nome do programa e programa do seu pagamento mais recente.           |
| Pagamentos por origem     | Quantidade de pagamentos, em dólares americanos, representados por programa nos últimos 12 meses.           |
| Pagamentos               | Selecione pago ou pendente e, em seguida, classifique como desejar. Para obter detalhes adicionais de um pagamento específico, selecione Exibir. Para baixar uma cópia da instrução de remessa de pagamento, selecione baixar. Observe que os dados do histórico de transações podem levar até 24 horas para serem exibidos, portanto, você pode não ver os ganhos associados imediatamente. |

Para exportar qualquer um dos dados nessa página, selecione exportar e, em seguida, siga as instruções na página Exportar dados.

## <a name="transaction-history-page"></a>Página Histórico de transações

Esta página exibe todos os seus ganhos individuais, incluindo a data, o tipo e a conquista de cada um. Você pode selecionar um período de tempo para exibir e também pode filtrar por ID de registro, programa, ID de pagamento, tipo de conquista, alavanca e status. Os dados estão disponíveis para o ano fiscal atual (1º de julho a 30 de junho) e os dois anos fiscais anteriores.

Para ver mais detalhes sobre uma conquista, selecione a seta para baixo no lado direito da página. Isso exibirá a alavanca, o valor da receita e o produto. Se, por algum motivo, qualquer um desses dados estiver indisponível, mas você precisar acessá-lo, contate o [suporte](https://developer.microsoft.com/windows/support)]. Se a conquista for o resultado de um ajuste, e não de uma transação, os campos de produto não serão exibidos.

Para exportar qualquer um dos dados de transação nessa página, selecione exportar e, em seguida, siga as instruções na página Exportar dados. Os arquivos exportados da página Histórico de transações mostram os dados na moeda da transação, os ganhos na moeda da transação e nos dólares dos EUA e o valor pago em pagar para a moeda.

## <a name="payment-status"></a>Status do pagamento

| Ganhando status           | Motivo                                                                                                                                      | Ação do parceiro necessária?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Não processados              | A conquista está qualificada para pagamento. Ele permanece nesse estado por um período de resfriamento, conforme definido no guia do programa do programa de incentivo. | Não                                                         |
| Cerimônia                 | A ordem de pagamento gerou revisões internas pendentes antes de o pagamento ser processado.                                                               | Não                                                         |
| Fatura de imposto pendente      | Sua fatura de imposto está incompleta ou inválida.                                                                                                  | Você precisa atualizar sua fatura de imposto antes de poder ser pago |
| Rejeitado durante a revisão   | O pagamento foi rejeitado durante a revisão.                                                                                                     | Contate [o suporte da Microsoft](https://developer.microsoft.com/windows/support) para obter detalhes                      |
| Failed (Falha)                   | O pagamento falhou devido a um erro do sistema da Microsoft.                                                                                         | Contate [o suporte da Microsoft](https://developer.microsoft.com/windows/support) para obter detalhes                      |
| Em andamento              | O pagamento está em andamento.                                                                                                                 | Não                                                         |
| Pagamento incorreto        | A revitóriação de pagamento está em andamento.                                                                                                       | Não                                                         |
| Enviados                     | O pagamento foi enviado ao seu banco.                                                                                                     | Não                                                         |
| Reprocessamento             | O pagamento encontrou um erro de sistema da Microsoft e está sendo reprocessado.                                                                  | Não                                                         |
| Inversão                 | O pagamento foi revertido pelo seu banco e será enviado novamente no próximo ciclo de pagamento.                                                     | Não                                                         |
| Fatura de imposto rejeitada     | A fatura do imposto foi rejeitada durante a revisão. Todos os pagamentos pendentes estarão em espera até que a revisão da fatura fiscal seja concluída.                 | Contate [o suporte da Microsoft](https://developer.microsoft.com/windows/support) para obter detalhes                      |
| Fatura de imposto em revisão | Sua fatura de imposto está sendo revisada. Seu pagamento será liberado depois que a nota fiscal do imposto tiver sido aprovada.                                   | Não                                                         |
| Rejeitado                 | O pagamento foi rejeitado pelo seu banco.                                                                                                      | Entre em contato com seu banco para obter detalhes.                             |

## <a name="export-data-page"></a>Exportar página de dados

Siga as instruções nesta página para exportar os dados desejados.

Observações:

- A página Exportar dados não é atualizada por conta própria. Talvez seja necessário atualizar a página manualmente para ver os dados mais recentes.
- O filtro pode resultar em um erro de nenhum dado disponível. Isso provavelmente significa que você saiu do período de tempo padrão selecionado em três meses e, em seguida, selecionou uma ID de pagamento de uma conquista que está fora desse período. Expanda seu período de tempo e tente novamente.

## <a name="payments"></a>Pagamentos

![Exportar pagamentos](images/pc-export-payments.png)

Essa opção fornece um download dos pagamentos recebidos em seu banco para um determinado programa, o imposto associado e a quantidade de conquista agregada. Esse relatório é usado para vários programas do Partner Center, portanto, algumas colunas podem ser inaplicáveis ao seu relatório. Essas colunas são marcadas abaixo.

| Nome da coluna              | Descrição                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participante da            | A principal identidade do parceiro ganhando sob o programa                                                                             |
| participantIDType        | Normalmente a ID do programa para programas de incentivo e ID do vendedor para programas da loja                                                                |
| participantename          | Nome do parceiro de conquista                                                                                                               |
| programName              | Nome do programa de incentivo/loja                                                                                                              |
| obtidos                   | Valor obtido na moeda de pagamento para o programa/participanteid                                                                       |
| earnedUSD                | Valor obtido para a ID do programa/participante, em USD                                                                                      |
| withheldTax              | Quantidade de imposto retido na moeda de pagamento para o programa/participanteid                                                               |
| salesTax                 | Valor total do imposto sobre vendas na moeda de pagamento para o programa/participanteid (aplicável somente a programas de incentivo)                   |
| serviceFeeTax            | Quantidade total de serviceFeeTax em pagamento a moeda para o programa/participanteid (aplicável somente a programas da loja e do Azure Marketplace) |
| totalPayment             | Pagamento total na moeda local, excluindo a retenção de imposto e incluindo o imposto sobre vendas (se aplicável) para o programa/participanteid   |
| currencyCode             | Pagar para código de moeda                                                                                                                      |
| paymentMethod            | O método usado para pagar o parceiro por exemplo, transferência bancária eletrônica, nota de crédito                                                     |
| pagamento de                | Identificador exclusivo para o pagamento. Esse número geralmente é visível em seu extrato bancário. (aplicável somente a pagamentos SAP)              |
| paymentStatus            | Status do pagamento                                                                                                                            |
| paymentStatusDescription | Descrição amigável do status de pagamento                                                                                                    |
| paymentDate              | A data de pagamento foi enviada da Microsoft                                                                                                      |

## <a name="transaction-history"></a>Histórico de transações

![Exportar histórico de transações](images/pc-export-transaction.png)

Essa opção fornece um download de cada item de linha de produção que você vê na página Histórico de transações, tipo de conquista, data, valor da transação associada, cliente, produto e outros detalhes transacionais aplicáveis aos seus programas.

| Nome da coluna                    | Descrição                                                                                                                              | Aplicabilidade para incentivos/loja/Azure Marketplace           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| ganhandoid                      | Identificador exclusivo para cada conquista                                                                                                       | Todas                                                            |
| participante da                  | A principal identidade do parceiro ganhando sob o programa                                                                            | Todas                                                            |
| participantIdType              | Principalmente a ID do programa para programas de incentivo e vendedor se for para os programas da loja e o Azure Marketplace                                          | Todas                                                            |
| participantename                | Nome do parceiro de conquista                                                                                                              | Todas                                                            |
| partnerCountryCode             | Localização/país do parceiro de conquista                                                                                                  | Todas                                                            |
| programName                    | Nome do programa de incentivo/loja                                                                                                             | Todas                                                            |
| transactionId                  | Identificador exclusivo para a transação                                                                                                    | Todas                                                            |
| transactionCurrency            | Moeda na qual a transação original do cliente ocorreu (não é a moeda do local do parceiro)                                     | Todas                                                            |
| transactionDate                | Data da transação. Útil para programas em que muitas transações contribuem para uma conquista                                           | Todas                                                            |
| transactionExchangeRate        | Data da taxa de câmbio usada para mostrar a transação correspondente valor USD                                                                 | Todas                                                            |
| transactionAmount              | Valor da transação na moeda da transação original com base na qual a conquista é gerada                                              | Todas                                                            |
| transactionAmountUSD           | Valor da transação em USD                                                                                                                | Todas                                                            |
| alavanca                          | Indica a regra de negócio para a conquista                                                                                                  | Todas                                                            |
| earningRate                    | Taxa de incentivo aplicada ao valor da transação para gerar uma conquista                                                                      | Todas                                                            |
| quantity                       | Varia de acordo com o programa. Indica a quantidade cobrada para programas transacionais                                                            | Todas                                                            |
| quanttype                   | Indica o tipo de quantidade, por exemplo, quantidade cobrada, MAU                                                                             | Todas                                                            |
| ganhandotype                    | Indica se é Tarifa, desconto, Coop, venda etc.                                                                                          | Todas                                                            |
| earningAmount                  | Conquistando valor na moeda da transação original                                                                                      | Todas                                                            |
| earningAmountUSD               | Conquistando valor em USD                                                                                                                    | Todas                                                            |
| earningDate                    | Data da conquista                                                                                                                      | Todas                                                            |
| calculationDate                | Data em que a conquista foi calculada no sistema                                                                                            | Todas                                                            |
| earningExchangeRate            | Taxa de câmbio usada para mostrar o valor USD correspondente                                                                                  | Todas                                                            |
| exchangeRateDate               | Data da taxa de câmbio usada para calcular EarningAmount USD                                                                                   | Todas                                                            |
| paymentAmountWOTax             | Valor de conquista (sem imposto) em pagar para moeda somente para pagamentos "enviados"                                                                 | Todas                                                            |
| paymentCurrency                | Pague para moeda escolhida pelo parceiro no perfil de pagamento. Mostrado somente para pagamentos enviados                                                   | Todas                                                            |
| paymentExchangeRate            | Taxa de câmbio usada para calcular paymentAmountWOTax na moeda de pagamento usando ExchangeRateDate                                            | Todas                                                            |
| claimId                        | Identificador exclusivo para declaração                                                                                                              | Incentivos-apenas alguns programas                                |
| planId                         | Identificador exclusivo do plano                                                                                                               | Incentivos-apenas alguns programas                                |
| pagamento de                      | Identificador exclusivo para o pagamento. Esse número geralmente é visível em seu extrato bancário                                                 | Somente pagamentos do SAP                                              |
| paymentStatus                  | Status do pagamento                                                                                                                           | Todas                                                            |
| paymentStatusDescription       | Descrição amigável do status de pagamento                                                                                                   | Todas                                                            |
| customerId                     | Estará sempre em branco                                                                                                                     | Apenas programas de incentivo (exceção: OEM) e Azure Marketplace |
| customerName                   | Estará sempre em branco                                                                                                                     | Apenas programas de incentivo (exceção: OEM) e Azure Marketplace |
| partNumber                     | Estará sempre em branco                                                                                                                     | Alguns programas de incentivo e de loja e o Azure Marketplace        |
| productName                    | Nome do produto vinculado à transação                                                                                                       | Todas                                                            |
| productId                      | Identificador exclusivo do produto                                                                                                                | Store e Azure Marketplace                                    |
| parentProductId                | Identificador exclusivo do produto pai. Observação: se não houver um produto pai para a transação, então ID do produto pai = ID do produto (Product ID). | Store e Azure Marketplace                                    |
| parentProductName              | Nome do produto pai. Observação: se não houver um produto pai para a transação, então Nome do produto pai = Nome do produto.   | Store e Azure Marketplace                                    |
| productType                    | Tipo de produto (por exemplo, app, complemento, jogo, etc.)                                                                                        | Store e Azure Marketplace                                    |
| invoiceNumber                  | Número da nota fiscal (aplicável apenas ao EA)                                                                                                  | Incentivo e Azure Marketplace-apenas alguns programas           |
| subscriptionId                 | Identificador de assinatura associado ao cliente                                                                                         | Incentivo-apenas alguns programas                                 |
| subscriptionStartDate          | Data de início da assinatura                                                                                                                  | Incentivo-apenas alguns programas                                 |
| subscriptionEndDate            | Data de término da assinatura                                                                                                                    | Incentivo-apenas alguns programas                                 |
| revendedorid                     | Identificador do revendedor                                                                                                                      | Incentivo-apenas alguns programas                                 |
| revendedorname                   | Nome do revendedor                                                                                                                            | Incentivo-apenas alguns programas                                 |
| distribuidorid                  | Identificador do distribuidor                                                                                                                   | Incentivo-apenas alguns programas                                 |
| distribuidorname                | Nome do distribuidor                                                                                                                         | Incentivo-apenas alguns programas                                 |
| agreementNumber                | Número de contrato                                                                                                                         | Incentivo-apenas alguns programas                                 |
| agreementStartDate             | Data de início do contrato                                                                                                                     | Incentivo-apenas alguns programas                                 |
| agreementEndDate               | Data de término do contrato                                                                                                                       | Incentivo-apenas alguns programas                                 |
| pico                       | Carga de trabalho                                                                                                                                 | Incentivo-apenas alguns programas                                 |
| transactionType                | Tipo de transação (por exemplo, compra, reembolso, estorno, etc.)                                                               | Store e Azure Marketplace                                    |
| localProviderSeller            | Vendedor ou provedor local do registro                                                                                                          | Somente armazenar                                                     |
| taxRemitted                    | Valor do imposto remetido (vendas, uso ou impostos IVA/GST).                                                                                   | Store e Azure Marketplace                                    |
| taxRemitModel                  | Parte responsável por remeter impostos (vendas, uso ou impostos IVA/GST).                                                                    | Somente armazenar                                                     |
| storeFee                       | O valor retido pela Microsoft como uma taxa para tornar o aplicativo ou complemento disponível na loja.                                           | Somente armazenar                                                     |
| transactionPaymentMethod       | A forma de pagamento do cliente usada na transação (por exemplo, cartão de crédito, conta de celular, PayPal, etc)                                | Store e Azure Marketplace                                    |
| tpan                           | Indica a rede de terceiros do AD                                                                                                     | Armazenar somente anúncios                                               |
| CustomerCountryComparer                | País do cliente                                                                                                                         | Store e Azure Marketplace                                    |
| customerCity                   | Cidade do cliente                                                                                                                            | Store e Azure Marketplace                                    |
| customerstate                  | Estado do cliente                                                                                                                           | Store e Azure Marketplace                                    |
| customerZip                    | CEP do cliente/CEP                                                                                                                 | Store e Azure Marketplace                                    |
| purchaseTypeCode               | Estará sempre em branco                                                                                                                     | Programa de incentivo – CRI                                        |
| purchaseOrderType              | Estará sempre em branco                                                                                                                     | Programa de incentivo – CRI                                        |
| purchaseOrderCoverageStartDate | Estará sempre em branco                                                                                                                     | Programa de incentivo – CRI                                        |
| purchaseOrderCoverageEndDate   | Estará sempre em branco                                                                                                                     | Programa de incentivo – CRI                                        |
| programOfferingLevel           |                                                                                                                                          | Programa de incentivo – CRI                                        |
| TenantID                       |                                                                                                                                          | Programas de incentivo                                             |
| externalReferenceId            | Identificador exclusivo do programa                                                                                                        | Programas de pagamento direto (incentivo e loja)                      |
| externalReferenceIdLabel       | Rótulo de identificador exclusivo                                                                                                                  | Programas de pagamento direto (incentivo e loja)                      |

## <a name="historical-statements"></a>Instruções históricas

![Exportar instruções históricas](images/pc-export-statements.png)

O histórico de transações de até julho de 1 2019 é tratado separadamente. As instruções usarão os campos a seguir em vez dos atuais.

> [!NOTE]
> O histórico de transações herdadas tem uma coluna chamada "reservado" que corresponde à coluna "ganhos" no histórico moderno, exceto pelo fato de que ela exclui todos os ganhos com status = "pagamento enviado".

> [!NOTE]
> Os filtros (3M, 6 minutos, 12M, etc.) não serão aplicados à seção de **instruções históricas** .

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