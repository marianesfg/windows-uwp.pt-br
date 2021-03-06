---
Description: Direcione segmentos específicos dos seus clientes com conteúdo personalizado para aumentar o envolvimento, a retenção e a monetização.
title: Usar ofertas direcionadas para maximizar o envolvimento e as conversões
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, ofertas direcionadas, ofertas, notificações
ms.localizationpriority: medium
ms.openlocfilehash: fd4cf135e5129ed4f3dbdbcebfb2003e7573182a
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685105"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Usar ofertas direcionadas para maximizar o envolvimento e as conversões

Direcione segmentos específicos dos seus clientes com conteúdo atrativo e personalizado para aumentar o envolvimento, a retenção e a monetização.

> [!IMPORTANT]
> As ofertas direcionadas podem ser usadas somente com aplicativos UWP que incluem complementos.

## <a name="targeted-offer-overview"></a>Visão geral da oferta direcionada

Basicamente, você precisa tomar três medidas para usar ofertas direcionadas:

1. **Crie a oferta no [Partner Center](https://partner.microsoft.com/dashboard).** Navegue até a página **Interagir > Ofertas direcionadas** para criar ofertas. Veja mais informações sobre esse processo a seguir.
2. **Implemente a experiência de oferta no aplicativo.** Use a *API de ofertas de Microsoft Store direcionada* no código do aplicativo para recuperar as ofertas disponíveis para um determinado usuário. Você também precisará criar a experiência de aplicativo para a oferta direcionada. Para obter mais informações, consulte [Gerenciar ofertas direcionadas usando os serviços da Loja](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Envie seu aplicativo para a loja.** O aplicativo deve ser publicado com a experiência de oferta no aplicativo para que as ofertas sejam disponibilizadas aos clientes.

Após concluir essas etapas, os clientes que usam seu aplicativo visualizam as ofertas disponíveis para eles naquele momento, com base na sua participação nos segmentos associados às suas ofertas. Observe que, embora façamos todos os esforços para mostrar todas as ofertas disponíveis aos clientes, ocasionalmente podem ocorrer problemas que afetam a disponibilidade da oferta.


## <a name="to-create-and-send-a-targeted-offer"></a>Para criar e enviar uma oferta direcionada

1.  No [Partner Center](https://partner.microsoft.com/dashboard) **, expanda entrar no** menu de navegação à esquerda e selecione **ofertas direcionadas**.
2.  Na página **Ofertas direcionadas**, analise as ofertas disponíveis. Selecione **Criar nova oferta** para qualquer oferta que você deseja implementar.

    > [!NOTE]
    > As ofertas disponíveis exibidas podem variar com o tempo e com base nos critérios da conta.

3.  Na nova linha que aparece abaixo das ofertas disponíveis, escolha o produto (aplicativo) no qual a oferta estará disponível. Em seguida, selecione o complemento que deseja associar à oferta.
4.  Se quiser criar ofertas adicionais, repita as etapas 2 e 3. Você pode implementar o mesmo tipo de oferta mais de uma vez no mesmo aplicativo, desde que você selecione complementos diferentes para cada oferta. Além disso, você pode associar o mesmo complemento a mais de um tipo de oferta.
5.  Quando terminar de criar as ofertas, clique em **Salvar**.

Depois de implementar suas ofertas, você pode retornar à página de **ofertas direcionadas** no Partner Center para exibir as conversões totais de cada oferta.

Se você decidir não usar uma oferta (ou se você não quiser mais usá-la), clique em **Excluir**.

> [!IMPORTANT]
> Certifique-se de publicar o código a fim de recuperar as ofertas disponíveis de um determinado usuário e criar a experiência no aplicativo. Para obter mais informações, consulte [Gerenciar ofertas direcionadas usando os serviços da Loja](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Ao considerar o conteúdo das ofertas direcionadas, tenha em mente que, assim como acontece com todo o conteúdo de aplicativo, o conteúdo de suas ofertas deve estar em conformidade com as [Políticas de conteúdo](https://docs.microsoft.com/legal/windows/agreements/store-policies) da Loja.
>
> Lembre-se também que, se um cliente que usar seu aplicativo (e entrar usando a conta da Microsoft no momento em que a associação do segmento for determinada) deixar outra pessoa usar seu dispositivo, essa pessoa poderá ver as ofertas direcionadas para o cliente original.
