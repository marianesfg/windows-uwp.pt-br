---
author: jnHs
Description: "O Resumo de pagamentos mostra detalhes do dinheiro que você ganhou com os seus aplicativos e complementos. Ele também permite saber quando você receberá os pagamentos e quanto você será pago."
title: Resumo do pagamento
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: be33881f015bfa1f91a545cfb571bb5f3da66c20
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="payout-summary"></a>Resumo do pagamento


O **Resumo de pagamentos** mostra detalhes do dinheiro que você ganhou com os seus aplicativos e complementos. Ele também permite saber quando você receberá os pagamentos e quanto você será pago.

Se você usa o Microsoft Advertising para ganhar dinheiro desde 1º de abril de 2016, também verá informações de pagamento sobre receita de publicidade no **Resumo de pagamento**. Mostraremos o aplicativo no qual essas receitas foram ganhas ou "não mapeado" para unidades utilizadas em vários aplicativos ou que não podem ser mapeadas para um aplicativo específico. 

Se você vender produtos no Azure Marketplace, também verá informações sobre pagamentos bem-sucedidos a partir 1º de novembro de 2015 e em **Resumo de pagamento**. Para saber mais sobre o pagamento do Azure Marketplace, consulte [Políticas de Participação do Microsoft Azure Marketplace](http://go.microsoft.com/fwlink/p/?LinkId=722436) e [Microsoft Azure Marketplace Publisher Agreement](http://go.microsoft.com/fwlink/p/?LinkID=699560 ). Mais informações sobre como visualizar informações de pagamento anteriores do Azure Marketplace podem ser encontradas [aqui](http://go.microsoft.com/fwlink/p/?LinkID=722439).

> **Observação**  Para ser qualificado para pagamento, a receita deve alcançar o limite de pagamento aplicável. Se a receita for menor que o limite de pagamento, permanecerá na categoria Reservado até que o limite seja atingido. Para obter mais detalhes sobre o limite de pagamento para receita do aplicativo, consulte o [Contrato de Desenvolvedor de Aplicativo](https://msdn.microsoft.com/library/windows/apps/hh694058). Para receita da Microsoft Advertising, o limite de pagamento é US$ 50 (ou o equivalente em moeda local). 
>
> Os pagamentos são feitos mensalmente (desde que tenha sido atingido um limite de pagamento aplicável). Normalmente, enviaremos qualquer pagamento devido em um determinado mês até o 15º dia do mês. Observe que os pagamentos geralmente levam entre 3 e 10 dias úteis adicionais para alcançar sua conta de pagamento. Para obter mais informações, consulte [Limites, métodos e prazos de pagamento](payment-thresholds-methods-and-timeframes.md).

## <a name="current-proceeds-and-payments"></a>Receita e pagamentos atuais


Próximo ao topo da página, você encontrará **Receita e pagamentos atuais**, que contém três seções: **Reservado**, **Pagamentos futuros** e **Pagamento mais recente**.

-   **Reservado** mostra a quantidade de dinheiro que sua conta acumulou, mas que ainda não foi agendada para pagamento, incluindo receita de publicidade. (A receita do Azure Marketplace não aparece na seção Reservado; se você apenas participar do Azure Marketplace, verá US$ 0,00 aqui). A receita do seu anúncio mais recente e as vendas de aplicativo permanecem em um estado pendente por cerca de 30 dias antes de se tornarem qualificadas para pagamento. Depois disso, a receita será programada para pagamento no mês seguinte (supondo que o limite de pagamento tenha sido atingido). Quando se tenta fazer um pagamento, o valor do pagamento é diminuído do o saldo reservado, e você vê o valor refletido em **Próximo pagamento**. Observe que o valor mostrado em **Reservado** é uma estimativa, porque as taxas de câmbio para vendas em outras moedas podem flutuar antes da geração do pagamento. Você pode notar que seu saldo reservado muda um pouco no início de cada mês. Seu saldo reservado é atualizado mensalmente para refletir as taxas de câmbio mensais, de modo que ele represente uma estimativa mais precisa. Você pode clicar em **Exibir detalhes** para ver informações adicionais ou no link **Baixar as transações reservadas** para exibir um arquivo .csv de todas as suas transações **Reservadas**.
-   **Pagamentos futuros** mostra o número de pagamentos futuros, o valor do(s) próximo(s) pagamento(s) e a(s) data(s) de criação do pagamento. Se a receita qualificada ainda não tiver alcançado o limite de pagamentos, nenhum pagamento futuro será mostrado aqui. Você pode clicar em **Exibir detalhes** para ver informações adicionais, incluindo os valores dos pagamentos e os respectivos códigos de receita. Quando um valor for mostrado na seção **Pagamento futuro**, você verá um link temporário para **Baixar transações**.  Se clicar no link, você poderá exibir um arquivo .csv de todas as transações que constituem os pagamentos futuros.  Observação: quando o valor **Pagamento futuro** passar para **Pagamento mais recente**, o link **Baixar transações** deixará de ser exibido.
-   **Pagamento mais recente** mostra o valor da última tentativa de pagamento. Se o pagamento foi bem-sucedido, o link **Exibir detalhes** estará azul, e você pode clicar nele para ver os detalhes de cada pagamento. Observe que se tentarmos fazer vários pagamentos e apenas um deles for bem-sucedido, apenas o valor do pagamento bem-sucedido será mostrado aqui. Se ocorrer falha em um ou mais pagamentos, o link **Exibir detalhes** estará vermelho e o número de pagamentos com falha serão exibidos. Você pode clicar em **Exibir detalhes** para ver mais detalhes sobre o problema a fim de corrigir a situação.

## <a name="proceeds-by-app-and-adjustments"></a>Receita por aplicativo e ajustes


Esta seção divide as informações do resumo para permitir que você veja detalhes específicos por aplicativo. Se você tiver recebido dinheiro por meio da Microsoft Advertising, a quantidade total da receita de publicidade será mostrada aqui como um item de linha único.

Ao examinar esta seção, você poderá determinar quais aplicativos geraram dinheiro que está atualmente nas categorias **Reservado** ou **Pagamento mais recente**. Você também poderá ver o valor total recebido por cada aplicativo. Caso seja necessário fazer [ajustes](#proceeds-by-app-and-adjustments) no saldo de sua conta, também é possível exibi-los aqui. (Os ajustes de receita do Microsoft Advertising não são mostrados aqui no momento.)

## <a name="payment-statements"></a>Demonstrativos de pagamento


Nesta seção, você pode exibir os demonstrativos de todos os pagamentos mensais bem-sucedidos e ver a quantidade total de dinheiro pago.

A seção **Total pago até a data** mostra o valor total pago por todas as suas vendas. Clique em **Exibir detalhes** para ver os valores oriundos de cada fonte de receita.

Abaixo da seção **Total pago até a data**, você verá seus três últimos demonstrativos por padrão. Para ver o demonstrativo completo (para pagamentos bem-sucedidos), clique em **Exibir**. Você pode acessar seus demonstrativos de pagamento históricos por meio da caixa de lista suspensa.

Na parte superior de cada demonstrativo, você verá o valor total de seu pagamento mensal. Logo abaixo, na área de **pagamentos efetuados**, você verá um resumo da forma como o valor dos pagamentos foi calculado.

Abaixo na seção **Divisão de receitas**, você pode ver detalhes sobre quanto dinheiro foi ganho por mercado e por fonte de receita (por exemplo, Loja do Windows Phone, Windows Store 8, Windows Store, etc.) por aplicativo. Você também verá detalhes sobre quaisquer [ajustes](#proceeds-by-app-and-adjustments) feitos, inclusive a data, o valor e o motivo do ajuste.

As seções mencionadas acima mostram apenas informações sobre sua receita (e ajustes) de vendas do aplicativo. Se ganhar dinheiro com publicidade, você verá uma seção separada da Microsoft Advertising com detalhes sobre os pagamentos e as conversões de moeda.

## <a name="adjustments"></a>Ajustes


| Categoria de ajuste     | Descrição                                                                                                |
|-------------------------|------------------------------------------------------------------------------------------------------------|
| Ajuste compensatório | Qualquer ajuste feito em seu saldo de pagamento que não pertença a outras categorias de ajuste listadas |
| Saldo histórico        | Saldos de pagamento de um sistema de pagamento histórico                                                             |
| Transferência de impostos              | Ajuste de imposto relacionado a vendas na Coreia                                                                   |

 

## <a name="downloading-payment-transactions"></a>Baixando transações de pagamento


Na parte superior de cada demonstrativo, você verá um link para **Baixar transações**. Clique neste link para obter um arquivo .csv com informações detalhadas sobre cada uma das transações incluídas em seu pagamento.

A tabela a seguir descreve os campos que aparecem no arquivo .csv. Observe que os campos exatos que você verá podem variar conforme o relatório for atualizado.

| Nome do campo              | Descrição                                                                                                                              |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Fonte de Receita          | A fonte de sua receita, com base no local onde ocorreu a transação (por exemplo, Windows Store, Loja do Windows Phone, Loja do Windows 8, Microsoft Advertising, etc.) |
| ID do Pedido          |  Identificador exclusivo do pedido. Essa ID permite identificar as transações de compra com suas respectivas transações de não compra (como reembolsos, estornos, etc.). Ambas terão a mesma ID de Pedido. Além disso, no caso de uma cobrança dividida, onde várias formas de pagamento são usadas em uma compra única, isso permitirá que você vincule as transações de compra.                                                                                                          |
| ID da transação          |       Identificador exclusivo da transação.  |
| Data/hora da transação   | A data e a hora em que a transação ocorreu (UTC).                                                                                        |
| ID do Produto (Product ID) pai       | Identificador exclusivo do produto pai. Observação: se não houver um produto pai para a transação, então ID do produto pai = ID do produto (Product ID). |
| ID do Produto (Product ID)              | Identificador exclusivo do produto.                                                                                                                |
| Nome do produto pai     | Nome do produto pai. Observação: se não houver um produto pai para a transação, então Nome do produto pai = Nome do produto.   |
| Nome do produto            | Nome do produto.                                                                                                                      |
| Tipo do Produto            | Tipo de produto (por exemplo, app, complemento, jogo, etc.)                                                                                        |
| Quantidade                | Quando a origem da receita é a Windows Store para Empresas, a quantidade representa o número de licenças adquiridas. Para todas as outras fontes de receita, a quantidade sempre será 1. Observação: mesmo quando uma única transação é dividida em dois itens de linha porque foram usados dois métodos de pagamento diferentes, cada item de linha mostrará uma quantidade de 1.                                                                     |
| Tipo de transação        | Tipo de transação (por exemplo, compra, reembolso, estorno, etc.)                                                               |
| Forma de pagamento          | A forma de pagamento do cliente usada na transação (por exemplo, cartão de crédito, conta de celular, PayPal, etc)                                        |
| País/região        | País/região onde ocorreu a transação.                                                                                            |
| Vendedor ou provedor local | Vendedor ou provedor local do registro.                                                                                                          |
| Moeda da transação    | Moeda da transação.                                                                                                              |
| Valor da transação      | Valor da transação.                                                                                                                |
| Imposto remetido            | Valor do imposto remetido (vendas, uso ou impostos IVA/GST).                                                                                    |
| Recebimentos líquidos            | Valor da transação menos imposto remetido.                                                                                                    |
| Taxa da Loja               | O percentual dos Recebimentos Líquidos retidos pela Microsoft como taxa pela disponibilização do aplicativo ou do complemento na Loja.                |
| Receita do aplicativo            | Recebimentos líquidos menos a taxa da Loja.                                                                                                         |
| Impostos retidos          | Valor do imposto de renda retido. (Não incluído no arquivo .csv **Reservado**.)                                                                                                           |
| Pagamento                 | Receita do aplicativo menos retenção de imposto de renda aplicável (valor mostrado na moeda da transação). (Não incluído no arquivo .csv **Reservado**.)                                           |
| Taxa de câmbio                 | Taxa de câmbio usada para converter a moeda da transação em moeda do pagamento.                                                           |
| Moeda do pagamento        | Moeda usada em seu pagamento.                                                                                                         |
| Pagamento convertido       | Valor do pagamento convertido na moeda do pagamento usando a taxa de câmbio.                                                                           |
| Modelo de remessa de imposto         | Parte responsável por remeter impostos (vendas, uso ou impostos IVA/GST).                                                                     |
| Data e hora da qualificação   | A data e a hora quando a receita da transação se qualificou para pagamento (UTC). Quando criado, um pagamento inclui a receita da transação com uma data e hora de qualificação anterior à data de criação do pagamento. (Incluído apenas no arquivo .csv **Reservado**.)                                                                     |
| Encargos                 | Mostra uma divisão de todos os detalhes de cobrança agregados na coluna Valor da Transação. (Incluído apenas para Azure Marketplace; não incluído no arquivo .csv **Reservado**.)          |

 

 

 




