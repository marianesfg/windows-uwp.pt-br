---
Description: Você também pode promover o seu aplicativo ou complemento na Microsoft Store colocando-o em promoção por um tempo limitado.
title: Colocar apps e complementos à venda
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ec71453cd03359dc6e1b72af2f6a43805bcb5ff
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788359"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Colocar apps e complementos à venda

Você também pode promover o seu aplicativo ou complemento na Microsoft Store colocando-o em promoção por um tempo limitado. Você pode optar por oferecer o produto em uma faixa de preços menor ou com um desconto baseado em porcentagem. E você pode optar por oferecer a venda para todas as pessoas ou torná-lo em uma oferta exclusiva para os clientes que possuem um dos seus outros produtos.

> [!NOTE]
> Não há suporte para o preço de venda para complementos de assinaturas.

Ao usar a seção **Preço de venda** da página **Preço e disponibilidade** de um envio para reduzir temporariamente o preço do seu aplicativo ou complemento, os clientes que acessarem a listagem da Loja verão um preço tachado indicando a redução (em vez de ter passado por uma [alteração de preço agendada](set-and-schedule-app-pricing.md#schedule-price-changes), o que pode reduzir ou elevar o preço sem exibi-lo como uma alteração na Loja). 

Durante o período em que o produto estiver em promoção, os clientes poderão comprá-lo a um preço menor durante o período de tempo selecionado. Se você baixar o preço para **Grátis**, eles poderão baixá-lo sem pagar durante o período de venda.

> [!IMPORTANT]
> Preço de venda é mostrada apenas para seus clientes em dispositivos Windows 10, incluindo Xbox One. Vendas oferecidos somente aos proprietários de um dos seus outros produtos são mostradas apenas para os clientes no Windows 10, versão 1607 ou posterior.
> 
> Em outros sistemas operacionais, os clientes verão o preço normal do aplicativo ou complemento e não poderão adquiri-los com o preço promocional. Você pode sempre alterar um preço escolhendo uma faixa de preço diferente em um novo envio, mas ele não será exibido como uma venda de tempo limitado.


## <a name="scheduling-a-sale"></a>Agendamento de uma promoção

Promoções agendadas como parte do envio de um aplicativo ou complemento. Se você quiser agendar uma promoção para um aplicativo ou complemento que já foi publicado, você precisará criar um novo envio, mesmo que essa seja a única alteração que você deseja fazer.

**Para agendar uma venda**

1. Na página **Preço e disponibilidade** de um envio de um aplicativo ou complemento em andamento, vá para a seção **Preço de venda**.
2. Selecione **Mostrar opções** e, em seguida, selecione **Nova venda** .
3. A janela pop-up **Seleção de mercado** será exibida, permitindo que você crie um *grupo de mercados* que especificará os mercados em que a venda deve ser oferecida. Você pode clicar em **Selecionar tudo** para oferecer a venda a todos os mercados em que seu aplicativo está disponível, selecionar um mercado individual ou selecionar vários mercados. Se desejar, você pode inserir um nome para seu grupo de mercados. Após fazer suas seleções, clique em **Criar** . (Para editar os mercados no grupo posteriormente, clique em seu nome).

   > [!NOTE]
   > As seleções de mercado que você faz na seção Preço de venda não afetarão os mercados nos quais o aplicativo é oferecido; essas seleções somente determinam se um preço de venda é oferecido e em quais mercados. Se você definir o preço de venda para um mercado em que seu aplicativo não está disponível, isso não fará com que o aplicativo se torne disponível no mercado.
4. Escolha uma das opções a seguir para especificar o tipo de desconto:
   - **Preço**: Use esta opção para selecionar um tipo de preço mais baixo no qual seu aplicativo será oferecido. Você pode alterar a lista suspensa para selecionar o preço em qualquer moeda de sua preferência. (O preço será convertido para a faixa correspondente de cada moeda. Para obter mais informações, consulte [Preços](set-app-pricing-and-availability.md).)
   - **Percentual**: Use esta opção para selecionar a porcentagem de desconto que será aplicada ao seu aplicativo. A mesma porcentagem de desconto é usada para todas as moedas.
5. Na linha **Oferecido para**, escolha uma das opções disponíveis, incluindo:
   - **Todas as pessoas**: Venda será oferecida para todos os clientes.
   - **Os proprietários de**: Venda será oferecida aos clientes que já têm um dos seus aplicativos. Você pode selecionar um dos seus aplicativos publicados na lista suspensa exibida. Você deve ter um ou mais aplicativos publicado para que essa opção fique disponível.

  > [!IMPORTANT]
  > Se você selecionar **Proprietários de**, a venda ficará visível somente para os clientes no Windows 10, versão 1607 ou posterior.

   - **Grupo de usuários conhecidos**: Venda será oferecida para as pessoas na [grupo de usuários conhecidos](create-known-user-groups.md) você selecionar. O grupo de usuários conhecido já deve estar criado para que a opção esteja disponível.
   - **Segmento**: Venda será oferecida para as pessoas no segmento de cliente que você selecionar. Você pode usar um [segmento já criado](create-customer-segments.md) aqui. Você também pode escolher **Novos contribuintes** para oferecer a venda somente a clientes que nunca compraram nada na Loja. Oferecemos esse segmento aqui porque descobrimos que, depois que um cliente faz sua primeira compra na Loja, ele geralmente continua a fazer mais compras; portanto, esse pode ser um grupo excelente para convencer com o preço de venda.
6. Insira a data e a hora para o início e o fim do período da promoção. Escolha uma das seguintes opções de fuso horário:
   - **UTC**: A hora em que você selecionar será o tempo de Horário Coordenado Universal (UTC), para que a venda ocorra ao mesmo tempo em todos os lugares.
   - **Local**: A hora em que você selecionar serão usados em cada fuso horário associado a um mercado. (Observe que, no caso de mercados que incluem mais de um fuso horário, apenas um fuso horário desse mercado será usado. Para os Estados Unidos, o fuso horário do Leste dos EUA é usado.
7. Para agendar uma venda adicional, selecione **Nova venda** . Do contrário, selecione **Salvar** na parte inferior da página **Preço e disponibilidade** e, em seguida, selecione **Enviar à Loja** na visão geral do envio.

> [!NOTE]
> É possível selecionar uma faixa de preços maior que o preço base do seu aplicativo. No entanto, o preço de venda só será mostrado aos clientes se o preço de venda for menor que o preço normal do aplicativo no mercado.
>
> Selecionar um preço que seja maior que o preço base do seu aplicativo pode ser apropriado para seu venda, se você já definiu preços personalizados em certos mercados que são maiores do que o preço base do seu aplicativo, e você deseja reduzir temporariamente o preço nesses mercados (mas o preço de venda ainda é maior do que o preço base do aplicativo). Se suas seleções resultariam num preço mais alto do aplicativo em um determinado mercado, não mostraremos esse preço (maior) para os clientes nesse mercado; eles continuarão a ver o aplicativo com seu preço anterior (inferior). Também mostraremos aos clientes o preço mais baixo disponível se você agendar promoções sobrepostas separadas com preços diferentes.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Alterando ou cancelando uma promoção agendada

Para revisar ou cancelar uma promoção agendada anteriormente para um aplicativo ou complemento, você precisará criar um novo envio e enviá-lo para a Loja.

**Para editar uma venda agendada**

1.  Na página **Preço e disponibilidade** de um envio de aplicativo ou complemento em andamento, vá para a seção **Preço de Venda**.
2.  Localize a venda que você deseja atualizar e faça suas alterações.
3.  Clique em **Salvar** na parte inferior da página **Preço e disponibilidade** e, em seguida, clique em **Enviar para a Loja** na visão geral do envio.

Depois que seu envio passar pelo processo de certificação, as alterações entrarão em vigor.

> [!IMPORTANT]
> Se a venda já tiver iniciado, você não poderá editar a data de início. Embora você possa editar a data de término, é recomendável editar uma venda para ser finalizada mais cedo do que a data de término original. Seus clientes potenciais podem ficar frustrados se você encerrar uma venda antes da data em que foi originalmente publicada (já que os clientes veem a data de término agendada ao visualizarem a listagem da Loja do seu aplicativo).

 **Para cancelar uma venda que ainda não foi iniciado**

1.  Na página **Preço e disponibilidade** de um envio de um aplicativo ou complemento em andamento, vá para a seção **Preço de venda**.
2.  Localize a venda que você deseja cancelar e clique em **Remover**.
3.  Clique em **Salvar** na parte inferior da página **Preço e disponibilidade** e, em seguida, clique em **Enviar para a Loja** na visão geral do envio. Contanto que a venda não seja iniciada no momento em que o novo envio conclui processo de certificação, a venda removida não será executada.




