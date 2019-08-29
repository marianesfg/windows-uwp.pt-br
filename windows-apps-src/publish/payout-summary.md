---
Description: O Resumo de pagamentos mostra detalhes do dinheiro que você ganhou com os seus aplicativos e complementos. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.
title: Resumo do pagamento
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, resumo de pagamentos, extrato, pagamentos, lucros, pagamentos, pagamento, receita
ms.localizationpriority: medium
ms.openlocfilehash: 68a7de0692d05ffe8d1b489e75a58c16b3c826df
ms.sourcegitcommit: 9779be4a1075e924dca7585808722d95cda99aff
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118064"
---
# <a name="payout-summary"></a>Resumo do pagamento

O **Resumo do pagamento** mostra detalhes sobre o dinheiro que você ganhou com a Microsoft. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.

Se você vender produtos no Azure Marketplace, também verá informações sobre pagamentos bem-sucedidos em **Resumo de pagamento**. Para saber mais sobre o pagamento do Azure Marketplace, consulte [Políticas de Participação do Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) e [Microsoft Azure Marketplace Publisher Agreement](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Para ser elegível para pagamento, seus prosseguimentos devem alcançar o [limite de pagamento](payment-thresholds-methods-and-timeframes.md) de $50. Para obter detalhes sobre o limite de pagamento, consulte esta página e examine o contrato de desenvolvedor do aplicativo.

## <a name="access-the-payout-summary-pages"></a>Acessar as páginas de resumo do pagamento

Para abrir uma das páginas de resumo do pagamento:

1. Selecione o ícone de dinheiro no canto superior direito.
2. Selecione pagamentos, histórico de transações ou exportar dados.

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

Para ver mais detalhes sobre uma conquista, selecione a seta para baixo no lado direito da página. Isso exibirá a alavanca, o valor da receita e o produto. Se, por algum motivo, qualquer um desses dados estiver indisponível, mas você precisar acessá-lo, contate o [suporte](https://developer.microsoft.com/en-us/windows/support)]. Se a conquista for o resultado de um ajuste, e não de uma transação, os campos de produto não serão exibidos.

Para exportar qualquer um dos dados de transação nessa página, selecione exportar e, em seguida, siga as instruções na página Exportar dados. Os arquivos exportados da página Histórico de transações mostram os dados na moeda da transação, os ganhos na moeda da transação e nos dólares dos EUA e o valor pago em pagar para a moeda.

## <a name="payment-status"></a>Status do pagamento

| Ganhando status           | Reason                                                                                                                                      | Ação do parceiro necessária?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Não processados              | A conquista está qualificada para pagamento. Ele permanece nesse estado por um período de resfriamento, conforme definido no guia do programa do programa de incentivo. | Não                                                         |
| Cerimônia                 | A ordem de pagamento gerou revisões internas pendentes antes de o pagamento ser processado.                                                               | Não                                                         |
| Fatura de imposto pendente      | Sua fatura de imposto está incompleta ou inválida.                                                                                                  | Você precisa atualizar sua fatura de imposto antes de poder ser pago |
| Rejeitado durante a revisão   | O pagamento foi rejeitado durante a revisão.                                                                                                     | Contate [o suporte da Microsoft](https://developer.microsoft.com/en-us/windows/support) para obter detalhes                      |
| Failed (Falha)                   | O pagamento falhou devido a um erro do sistema da Microsoft.                                                                                         | Contate [o suporte da Microsoft](https://developer.microsoft.com/en-us/windows/support) para obter detalhes                      |
| Em andamento              | O pagamento está em andamento.                                                                                                                 | Não                                                         |
| Pagamento incorreto        | A revitóriação de pagamento está em andamento.                                                                                                       | Não                                                         |
| Enviados                     | O pagamento foi enviado ao seu banco.                                                                                                     | Não                                                         |
| Reprocessamento             | O pagamento encontrou um erro de sistema da Microsoft e está sendo reprocessado.                                                                  | Não                                                         |
| Inversão                 | O pagamento foi revertido pelo seu banco e será enviado novamente no próximo ciclo de pagamento.                                                     | Não                                                         |
| Fatura de imposto rejeitada     | A fatura do imposto foi rejeitada durante a revisão. Todos os pagamentos pendentes estarão em espera até que a revisão da fatura fiscal seja concluída.                 | Contate [o suporte da Microsoft](https://developer.microsoft.com/en-us/windows/support) para obter detalhes                      |
| Fatura de imposto em revisão | Sua fatura de imposto está sendo revisada. Seu pagamento será liberado depois que a nota fiscal do imposto tiver sido aprovada.                                   | Não                                                         |
| Recusa                 | O pagamento foi rejeitado pelo seu banco.                                                                                                      | Entre em contato com seu banco para obter detalhes.                             |

## <a name="export-data-page"></a>Exportar página de dados

Siga as instruções nesta página para exportar os dados desejados.

Notas:

- A página Exportar dados não é atualizada por conta própria. Talvez seja necessário atualizar a página manualmente para ver os dados mais recentes.
- O filtro pode resultar em um erro de nenhum dado disponível. Isso provavelmente significa que você saiu do período de tempo padrão selecionado em três meses e, em seguida, selecionou uma ID de pagamento de uma conquista que está fora desse período. Expanda seu período de tempo e tente novamente.

## <a name="payment-download-export"></a>Exportação de download de pagamento

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
| paymentMethod            | O método usado para pagar o parceiro, por exemplo, transferência bancária eletrônica, nota de crédito                                                             |
| pagamento de                | Identificador exclusivo para o pagamento. Esse número geralmente é visível em seu extrato bancário. (aplicável somente a pagamentos SAP)              |
| paymentStatus            | Status do pagamento                                                                                                                            |
| paymentStatusDescription | Descrição amigável do status de pagamento                                                                                                    |
| paymentDate              | A data de pagamento foi enviada da Microsoft                                                                                                      |

## <a name="transaction-history-download-export"></a>Exportação de download de histórico de transações

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
| quanttype                   | Indica o tipo de quantidade, por exemplo, a quantidade cobrada, MAU                                                                                     | Todas                                                            |
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
| customerId                     | Estará sempre em branco                                                                                                                     | Somente programas de incentivo (exceção: OEM) e Azure Marketplace |
| Customer                   | Estará sempre em branco                                                                                                                     | Somente programas de incentivo (exceção: OEM) e Azure Marketplace |
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
| agreementNumber                | Número do contrato                                                                                                                         | Incentivo-apenas alguns programas                                 |
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
