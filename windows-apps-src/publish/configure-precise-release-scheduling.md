---
author: jnHs
Description: "Você pode definir a data e a hora exatas em que o aplicativo será disponibilizado na Loja, o que lhe conferirá maior flexibilidade e a capacidade de personalizar datas para diferentes mercados."
title: "Configurar o agendamento preciso do lançamento"
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 77e954e3d8b85c0cf517a154447957a2d81d5bcf
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2017
---
# <a name="configure-precise-release-scheduling"></a>Configurar o agendamento preciso do lançamento

A seção **Agendamento** na página [Preço e disponibilidade](set-app-pricing-and-availability.md) permite que você defina a data e a hora exatas em que o aplicativo deve ser disponibilizado na Loja, conferindo a você maior flexibilidade e a capacidade de personalizar datas para diferentes mercados.

> [!NOTE]
> Embora este tópico se refira aos aplicativos, o agendamento de versão para envios de complemento usa o mesmo processo.

Além disso, você pode optar por definir uma data em que o produto não estará mais disponível na Loja. Observe que isso significa que o produto não pode mais ser encontrado na Loja por meio de pesquisa ou navegação, mas qualquer cliente com um link direto pode ver a listagem da Loja do produto. Ele pode baixá-lo somente se ele já tiver o produto ou um [código promocional](generate-promotional-codes.md) e estiver usando um dispositivo Windows 10.

Por padrão (a menos que você tenha selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](set-app-pricing-and-availability.md#visibility)), o aplicativo estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção.

Observe que você não conseguirá configurar as datas na seção **Agendamento** se tiver selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](set-app-pricing-and-availability.md#visibility), porque o aplicativo não será lançado para os clientes; por isso, não há uma data de lançamento a ser configurada.

> [!IMPORTANT]
> As datas especificadas na seção Agendamento só se aplicam aos clientes no Windows 10.
>
>Se o aplicativo oferecer suporte a versões anteriores do sistema operacional, os clientes que usam essas versões verão a listagem do aplicativo assim que ele for aprovado na certificação e conclui o processo de publicação, mesmo se você tiver selecionado uma data de lançamento mais recente. Qualquer data para **Parar aquisição** selecionada não se aplicará a esses clientes; eles ainda poderão adquirir o aplicativo (a menos que você envie uma atualização com uma nova seleção na seção [Visibilidade](set-app-pricing-and-availability.md#visibility) ou selecione **Make app unavailable** na página **Visão geral do aplicativo**).


## <a name="base-schedule"></a>Agendamento base

As seleções feitas para o agendamento base se aplicarão a todos os mercados em que o aplicativo está disponível, a menos que você adicione posteriormente datas para mercados específicos (ou grupos de mercados), selecionando [Personalizar para mercados específicos](#customize-the-schedule-for-specific-markets).

Você verá duas opções aqui: **Lançamento** e **Parar aquisição**. 

### <a name="release"></a>Lançamento

Na lista suspensa **Lançamento**, você pode definir quando deseja que o aplicativo seja disponibilizado na Loja. Isso significa que o aplicativo pode ser descoberto na Loja por meio de pesquisa ou navegação, e que os clientes podem ver a listagem da Loja e adquirir o aplicativo.

>[!NOTE]
> Depois que o aplicativo for publicado e disponibilizado na Loja, você não conseguirá selecionar uma data de **Lançamento** (a não ser que o aplicativo já tenha sido lançado).

Estas são as opções que você pode configurar para o agendamento do **Lançamento** de um produto:
- **assim que possível**: o produto será lançado assim que ele for certificado e publicado. Essa é a opção padrão.
- **em**: o produto será lançado na data e hora que você selecionar. Você tem duas opções adicionais:
   - **UTC**: a hora que você selecionar será UTC (Horário Coordenado Universal), para que o aplicativo seja lançado ao mesmo tempo em todos os lugares.
   - **Local**: a hora que você selecionar será o valor usado em cada fuso horário associado a um mercado. (Observe que, no caso de mercados que incluem mais de um fuso horário, apenas um fuso horário desse mercado será usado. Para os Estados Unidos, o fuso horário do Leste dos EUA é usado.
- **não agendado**: o aplicativo não estará disponível na Loja. Se você escolher essa opção, poderá disponibilizar o aplicativo na Loja mais tarde, criando um novo envio e escolhendo uma das outras opções.

> [!TIP]
> Se desejar, você também pode [inserir uma data de lançamento diferente](set-app-pricing-and-availability.md#display-release-date) para que apareça na Loja do aplicativo. 

### <a name="stop-acquisition"></a>Parar aquisição

Na lista suspensa **Parar aquisição**, você pode definir uma data e hora em que os novos clientes não possam mais adquirir o aplicativo na Loja ou descobri-lo na listagem. Isso pode ser útil se você quiser controlar com precisão quando um aplicativo não será oferecido mais para novos clientes; por exemplo, quando você estiver coordenando a disponibilidade de mais de um aplicativo.

Por padrão, a opção **Parar aquisição** é definida como nunca. Para alterar essa configuração, selecione **em** na lista suspensa e especifique uma data e hora, conforme descrito acima. Na data e hora que você selecionar, os clientes não poderão mais adquirir o aplicativo.

É importante entender que essa opção tem o mesmo impacto que selecionar **Make this app discoverable but not available** na seção [Visibilidade](set-app-pricing-and-availability.md#visibility) e escolher **Parar a aquisição: os clientes com um link direto poderão ver a listagem da Loja do produto, mas só poderão baixá-lo se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10**. Para parar completamente de oferecer um aplicativo para novos clientes, clique em **Make app unavailable** na página de visão geral do aplicativo. Para obter mais informações, consulte [Removendo um aplicativo da Loja](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Se você selecionar uma data para **Parar aquisição** e depois decidir disponibilizar o aplicativo novamente, poderá criar um novo envio e alterar **Parar aquisição** para **Nunca**. O aplicativo ficará disponível novamente depois que o envio atualizado for publicado.

### <a name="customize-the-schedule-for-specific-markets"></a>Personalizar o agendamento para mercados específicos 

Por padrão, as opções que você selecionar acima serão aplicadas a todos os mercados em que o aplicativo for oferecido. Para personalizar o preço para mercados específicos, clique em **Personalizar para mercados específicos**. A janela pop-up **Seleção de mercado** será exibida, listando todos os mercados em que você optou disponibilizar o aplicativo. Se você excluiu algum mercado na seção [Mercados]((define-pricing-and-market-selection.md), eles não serão mostrados. 

Para adicionar a agenda de um mercado, selecione-a e clique em **Salvar**. Depois, você verá as mesmas opções **Lançamento** e **Parar aquisição** descritas acima, mas as seleções feitas serão aplicadas somente a esse mercado.

Para adicionar um agendamento que se aplicará a vários mercados, crie um *grupo de mercados*. Para fazer isso, selecione os mercados que você deseja incluir e insira um nome para o grupo. (Esse nome é somente para referência e não ficará visível para todos os clientes.) Por exemplo, se você quiser criar um grupo de mercado para a América do Norte, selecione **Canadá**, **México** e **Estados Unidos**, e nomeie-o como **América do Norte** ou outro nome de sua preferência. Quando você terminar de criar o grupo de mercados, clique em **Salvar**. Depois, você verá as mesmas opções **Lançamento** e **Parar aquisição** descritas acima, mas as seleções feitas serão aplicadas somente a esse grupo de mercados.

Para adicionar uma agenda personalizada para um mercado adicional ou um grupo de mercados adicional, basta clicar novamente em **Personalizar para mercados específicos** e repetir essas etapas. Para alterar os mercados incluídos em um grupo de mercados, selecione seu nome. Para remover o agendamento personalizado de um grupo de mercados (ou um mercado individual), clique em **Remover**.

> [!NOTE]
> Um mercado não pode pertencer a mais de um dos grupos de mercado usados na seção **Agendamento**. 









