---
Description: O Resumo de pagamentos mostra detalhes do dinheiro que você ganhou com os seus aplicativos e complementos. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.
title: Resumo do pagamento
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, resumo de pagamentos, extrato, pagamentos, lucros, pagamentos, pagamento, receita
ms.localizationpriority: medium
ms.openlocfilehash: 777ee4201b435f17cdc4fc3650a2d33645ff56b9
ms.sourcegitcommit: 769ec7811aaaa79fe521e3e984a2e1a2a9671caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70057825"
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

- Quando você acessa essa página na página de histórico de transações ou de pagamentos, seus filtros não são transferidos. Você precisará refazê-los na página Exportar dados.
- A página Exportar dados não é atualizada por conta própria. Talvez seja necessário atualizar a página manualmente para ver os dados mais recentes.
- O filtro pode resultar em um erro de nenhum dado disponível. Isso provavelmente significa que você saiu do período de tempo padrão selecionado em três meses e, em seguida, selecionou uma ID de pagamento de uma conquista que está fora desse período. Expanda seu período de tempo e tente novamente.

## <a name="payment-download-export"></a>Exportação de download de pagamento

Essa opção fornece um download dos pagamentos recebidos em seu banco para um determinado programa, o imposto associado e a quantidade de conquista agregada. Esse relatório é usado para vários programas do Partner Center, portanto, algumas colunas podem ser inaplicáveis ao seu relatório. Essas colunas são marcadas abaixo.

| Nome da coluna              | Descrição                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participante da            | A principal identidade do parceiro ganhando sob o programa                                                                           |
| participantIDType        | Normalmente a ID do programa para programas de incentivo e ID do vendedor para programas da loja                                                              |
| participantename          | Nome do parceiro de conquista                                                                                                             |
| programName              | Nome do programa de incentivo/loja                                                                                                            |
| obtidos                   | Valor obtido na moeda de pagamento para o programa/participanteid                                                                     |
| earnedUSD                | Valor obtido para a ID do programa/participante, em USD                                                                                    |
| withheldTax              | Quantidade de imposto retido na moeda de pagamento para o programa/participanteid                                                             |
| salesTax                 | Valor total do imposto sobre vendas na moeda de pagamento para o programa/participanteid                                                          |
| totalPayment             | Pagamento total na moeda local, excluindo a retenção de imposto e incluindo o imposto sobre vendas (se aplicável) para o programa/participanteid |
| currencyCode             | Pagar para código de moeda                                                                                                                    |
| paymentMethod            | O método usado para pagar o parceiro (transferência bancária eletrônica, nota de crédito)                                                              |
| pagamento de                | Identificador exclusivo para o pagamento. Esse número geralmente é visível em seu extrato bancário.                                               |
| paymentStatus            | Status do pagamento                                                                                                                          |
| paymentStatusDescription | Descrição amigável do status de pagamento                                                                                                  |
| paymentDate              | A data de pagamento foi enviada da Microsoft                                                                                                    |

## <a name="transaction-history-download-export"></a>Exportação de download de histórico de transações

Essa opção fornece um download de cada item de linha de produção que você vê na página Histórico de transações, tipo de conquista, data, valor da transação associada, cliente, produto e outros detalhes transacionais aplicáveis aos seus programas.

| Nome da coluna                    | Descrição                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| ganhandoid                      | Identificador exclusivo para cada conquista                                                                                                       |
| participante da                  | A principal identidade do parceiro ganhando sob o programa                                                                            |
| participantIdType              | ID do vendedor                                                                                                                                |
| participantename                | Nome do parceiro de conquista                                                                                                              |
| partnerCountryCode             | Localização/país do parceiro de conquista                                                                                                  |
| programName                    | Nome do programa de incentivo/loja                                                                                                             |
| transactionId                  | Identificador exclusivo para a transação                                                                                                    |
| transactionCurrency            | Moeda na qual a transação original do cliente ocorreu                                                                             |
| transactionDate                | Data da transação. Útil para programas em que muitas transações contribuem para uma conquista                                           |
| transactionExchangeRate        | Data da taxa de câmbio usada para mostrar o valor USD correspondente                                                                             |
| transactionAmount              | Valor da transação na moeda da transação original com base na qual a conquista é gerada                                              |
| transactionAmountUSD           | Valor da transação em USD                                                                                                                |
| alavanca                          | Indica a regra de negócio para a conquista                                                                                                  |
| earningRate                    | Taxa de incentivo aplicada ao valor da transação para gerar uma conquista                                                                      |
| quantity                       | Varia de acordo com o programa. Indica a quantidade cobrada para programas transacionais                                                            |
| ganhandotype                    | Indica se é Tarifa, desconto, Coop, venda etc.                                                                                          |
| earningAmount                  | Conquistando valor na moeda da transação original                                                                                      |
| earningAmountUSD               | Conquistando valor em USD                                                                                                                    |
| earningDate                    | Data da conquista                                                                                                                      |
| calculationDate                | Data em que a conquista foi calculada no sistema                                                                                            |
| earningExchangeRate            | Taxa de câmbio usada para mostrar o valor USD correspondente                                                                                  |
| exchangeRateDate               | Data da taxa de câmbio usada para calcular EarningAmount USD                                                                                   |
| claimId                        | Estará sempre em branco                                                                                                                     |
| pagamento de                      | Identificador exclusivo para o pagamento. Esse número geralmente é visível em seu extrato bancário                                                 |
| paymentStatus                  | Status do pagamento                                                                                                                           |
| paymentStatusDescription       | Descrição amigável do status de pagamento                                                                                                   |
| customerId                     | Estará sempre em branco                                                                                                                     |
| Customer                   | Estará sempre em branco                                                                                                                     |
| partNumber                     | Estará sempre em branco                                                                                                                     |
| productName                    | Nome do produto vinculado à transação                                                                                                       |
| productId                      | Identificador exclusivo do produto                                                                                                                |
| parentProductId                | Identificador exclusivo do produto pai. Observação: se não houver um produto pai para a transação, então ID do produto pai = ID do produto (Product ID). |
| parentProductName              | Nome do produto pai. Observação: se não houver um produto pai para a transação, então Nome do produto pai = Nome do produto.   |
| productType                    | Tipo de produto (por exemplo, app, complemento, jogo, etc.)                                                                                        |
| invoiceNumber                  | Estará sempre em branco                                                                                                                     |
| subscriptionId                 | Estará sempre em branco                                                                                                                     |
| subscriptionStartDate          | Estará sempre em branco                                                                                                                     |
| subscriptionEndDate            | Estará sempre em branco                                                                                                                     |
| revendedorid                     | Estará sempre em branco                                                                                                                     |
| revendedorname                   | Estará sempre em branco                                                                                                                     |
| distribuidorid                  | Estará sempre em branco                                                                                                                     |
| distribuidorname                | Estará sempre em branco                                                                                                                     |
| agreementNumber                | Estará sempre em branco                                                                                                                     |
| agreementStartDate             | Estará sempre em branco                                                                                                                     |
| agreementEndDate               | Estará sempre em branco                                                                                                                     |
| pico                       | Estará sempre em branco                                                                                                                     |
| transactionType                | Tipo de transação (por exemplo, compra, reembolso, estorno, etc.)                                                               |
| localProviderSeller            | Vendedor ou provedor local do registro                                                                                                          |
| taxRemitted                    | Valor do imposto remetido (vendas, uso ou impostos IVA/GST).                                                                                   |
| taxRemitModel                  | Parte responsável por remeter impostos (vendas, uso ou impostos IVA/GST).                                                                    |
| storeFee                       | O valor retido pela Microsoft como uma taxa para tornar o aplicativo ou complemento disponível na loja.                                           |
| transactionPaymentMethod       | A forma de pagamento do cliente usada na transação (por exemplo, cartão de crédito, conta de celular, PayPal, etc)                                |
| tpan                           | Indica a rede de terceiros do AD                                                                                                     |
| purchaseTypeCode               | Estará sempre em branco                                                                                                                     |
| purchaseOrderType              | Estará sempre em branco                                                                                                                     |
| purchaseOrderCoverageStartDate | Estará sempre em branco                                                                                                                     |
| purchaseOrderCoverageEndDate   | Estará sempre em branco                                                                                                                     |
| externalReferenceId            | Estará sempre em branco                                                                                                                     |
| externalReferenceIdLabel       | Estará sempre em branco                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Exportação de download da instrução de pagamento (herdada)

Por um tempo limitado na página de resumo do pagamento antigo, as instruções de pagamento estarão disponíveis para download. Esse relatório contém os campos a seguir.

> [!NOTE]
> O histórico de transações herdadas tem uma coluna chamada "reservado" que corresponde à coluna "ganhos" no histórico moderno, exceto pelo fato de que ela exclui todos os ganhos com status = "pagamento enviado".

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
