---
author: jnHs
Description: "Você também pode promover o seu aplicativo ou IAP (produto no aplicativo) na Windows Store colocando-o em promoção por um tempo limitado."
title: "Colocar aplicativos e IAPs em promoção"
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b53d8d4ddcf3f75ffe039203377d12fd02e52f07

---

# Colocar aplicativos e IAPs em promoção


Você também pode promover o seu aplicativo ou IAP (produto no aplicativo) na Windows Store colocando-o em promoção por um tempo limitado.

Quando você agenda uma promoção para reduzir temporariamente o preço do seu aplicativo ou IAP, os clientes que estão visualizando os detalhes na Loja verão que o preço foi reduzido, e eles poderão comprar o aplicativo a um preço menor durante o período de tempo selecionado por você. Se você baixar o preço para **Grátis**, eles poderão baixá-lo sem pagar durante o período de venda.

> 
            **Observação**  O preço de venda é mostrado somente para os clientes no Windows 10. Em outros sistemas operacionais, os clientes verão o preço normal do seu aplicativo ou IAP. Você pode sempre alterar um preço escolhendo uma faixa de preço diferente em um novo envio, mas ele não será exibido como uma venda de tempo limitado.

## Agendamento de uma promoção


Promoções agendadas como parte do envio de um aplicativo ou IAP. Se você quiser agendar uma promoção para um aplicativo ou IAP que já foi publicado, você precisará criar um novo envio, mesmo que essa seja a única alteração que você deseja fazer.

**Para agendar uma promoção**

1.  Na página **Preço e disponibilidade** de um envio de um aplicativo ou IAP em andamento, vá para a seção **Preço de venda**.
2.  Clique em **Nova venda**.
3.  Insira a data e a hora para o início e o fim do período da promoção. As horas são mostradas em UTC.

   > 
            **Observação**  Para vendas de IAP, você não pode agendar vendas sobrepostas.

4.  Escolha o preço da promoção na lista suspensa. Você pode escolher qualquer preço, incluindo **Grátis**.
5.  Se você quiser inserir preços personalizados para essa promoção, clique em **Mostrar opções personalizadas de preços de mercado**. Você pode definir preços de venda personalizado por mercado (ou excluir mercados específicos da venda) aqui. Para saber mais, consulte [Definir preço e seleção de mercado](define-pricing-and-market-selection.md).

    > 
            **Observação**  As seleções de mercado que você faz na seção **Preço de venda** não afetarão os mercados nos quais o aplicativo é oferecido; essas seleções apenas determinam se um preço de venda é oferecido e em quais mercados. Se você definir o preço de venda para um mercado em que seu aplicativo não está disponível, isso não fará com que o aplicativo se torne disponível no mercado.

6.  Clique em **Concluído** para salvar a promoção agendada.
7.  Clique em **Salvar** na parte inferior da página **Preço e disponibilidade** e, em seguida, clique em **Enviar para a Loja** na visão geral do envio.

> 
            **Observação**  É possível selecionar uma faixa de preços que seja maior que o preço base do seu aplicativo. No entanto, o preço de venda só será mostrado aos clientes se o preço de venda for menor que o preço normal do aplicativo no mercado. Selecionar um preço que seja maior que o preço base do seu aplicativo pode ser apropriado para seu venda, se você já definiu preços personalizados em certos mercados que são maiores do que o preço base do seu aplicativo, e você deseja reduzir temporariamente o preço nesses mercados (mas o preço de venda ainda é maior do que o preço base do aplicativo). Se suas seleções resultariam num preço mais alto do aplicativo em um determinado mercado, não mostraremos esse preço (maior) para os clientes nesse mercado; eles continuarão a ver o aplicativo com seu preço anterior (inferior). Também mostraremos aos clientes o preço mais baixo disponível se você agendar promoções sobrepostas separadas com preços diferentes.

## Alterando ou cancelando uma promoção agendada


Para revisar ou cancelar uma promoção agendada anteriormente para um aplicativo ou IAP, você precisará criar um novo envio e enviá-lo para a Loja.

**Para editar uma promoção agendada**

1.  Na página **Preço e disponibilidade** de um envio de um aplicativo ou IAP em andamento, vá para a seção **Preço de venda**.
2.  Encontre a venda que você deseja atualizar e clique no preço para editar a venda.
3.  Faça suas alterações e depois clique em **Concluído**.
4.  Clique em **Salvar** na parte inferior da página **Preço e disponibilidade** e, em seguida, clique em **Enviar para a Loja** na visão geral do envio.

Depois que seu envio passa pelo processo de certificação, as alterações terão efeito (mesmo que a venda já tenha sido iniciada).

> 
            **Dica**  Você pode reutilizar uma venda concluída em um novo envio editando suas datas de início e fim. Isso será especialmente útil se você tiver configurado uma venda com o preço de mercado personalizado complicado.
 
**Para cancelar uma promoção agendada**

1.  Na página **Preço e disponibilidade** de um envio de um aplicativo ou IAP em andamento, vá para a seção **Preço de venda**.
2.  Encontre a venda que você deseja cancelar e clique em **Excluir** para removê-lo.
3.  Clique em **Salvar** na parte inferior da página **Preço e disponibilidade** e, em seguida, clique em **Enviar para a Loja** na visão geral do envio.

Desde que a promoção não tenha sido iniciada no momento em que o processo de certificação do envio tenha sido concluído, a promoção excluída não será executada. Se você excluir uma promoção que já foi finalizada, a promoção simplesmente será removida da sua página **Preço e disponibilidade**.

> 
            **Importante**   Já que os clientes podem ver a data de término agendada ao visualizar os detalhes do seu aplicativo na Loja, não recomendamos a exclusão de uma promoção depois que ela for iniciada. Se você excluir uma promoção que já está em andamento, a promoção terminará quando o processo de certificação do envio for concluído, o que pode ser frustrante para seus clientes potenciais.




<!--HONumber=Jun16_HO4-->


