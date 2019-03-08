---
Description: Você pode definir a data e a hora exatas em que o aplicativo será disponibilizado na Loja, o que lhe conferirá maior flexibilidade e a capacidade de personalizar datas para diferentes mercados.
title: Configurar o agendamento preciso do lançamento
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, agenda, data de lançamento, datas, lançamento
ms.localizationpriority: medium
ms.openlocfilehash: a1477a426a9cdf240e694efb19bd7521fcd734cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597201"
---
# <a name="configure-precise-release-scheduling"></a>Configurar o agendamento preciso do lançamento

A seção **Agendamento** na página [Preço e disponibilidade](set-app-pricing-and-availability.md) permite que você defina a data e a hora exatas em que o aplicativo deve ser disponibilizado na Loja, conferindo a você maior flexibilidade e a capacidade de personalizar datas para diferentes mercados.

> [!NOTE]
> Embora este tópico se refira aos aplicativos, o agendamento de versão para envios de complemento usa o mesmo processo.

Além disso, você pode optar por definir uma data em que o produto não estará mais disponível na Loja. Observe que isso significa que o produto não pode mais ser encontrado na Loja por meio de pesquisa ou navegação, mas qualquer cliente com um link direto pode ver a listagem da Loja do produto. Ele pode baixá-lo somente se ele já tiver o produto ou um [código promocional](generate-promotional-codes.md) e estiver usando um dispositivo Windows 10.

Por padrão (a menos que você tenha selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](choose-visibility-options.md#discoverability)), o aplicativo estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção.

Observe que você não conseguirá configurar as datas na seção **Agendamento** se tiver selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](choose-visibility-options.md#discoverability), porque o aplicativo não será lançado para os clientes; por isso, não há uma data de lançamento a ser configurada.

> [!IMPORTANT]
> As datas especificadas na seção Agendamento só se aplicam aos clientes no Windows 10.
>
>Se seu aplicativo publicado anteriormente dá suporte a versões anteriores do sistema operacional, qualquer **parar a aquisição** data selecionada não se aplicará aos clientes; eles ainda poderão adquirir o aplicativo (a menos que você enviar uma atualização com uma nova seleção no [Visibilidade](choose-visibility-options.md#discoverability) seção, ou se você selecionar **Verifique aplicativo indisponível** do **visão geral do aplicativo** página).


## <a name="base-schedule"></a>Agendamento base

As seleções feitas para o agendamento base se aplicarão a todos os mercados em que o aplicativo está disponível, a menos que você adicione posteriormente datas para mercados específicos (ou grupos de mercados), selecionando [Personalizar para mercados específicos](#customize-the-schedule-for-specific-markets).

Você verá duas opções aqui: **Versão** e **parar aquisição**. 

## <a name="release"></a>Versão

Na lista suspensa **Lançamento**, você pode definir quando deseja que o aplicativo seja disponibilizado na Loja. Isso significa que o aplicativo pode ser descoberto na Loja por meio de pesquisa ou navegação, e que os clientes podem ver a listagem da Loja e adquirir o aplicativo.

>[!NOTE]
> Depois que o aplicativo for publicado e disponibilizado na Loja, você não conseguirá selecionar uma data de **Lançamento** (a não ser que o aplicativo já tenha sido lançado).

Estas são as opções que você pode configurar para o agendamento do **Lançamento** de um produto:
- **Assim que possível**: O produto liberará assim que ele é certificado e publicado. Essa é a opção padrão.
- **at**: O produto liberará na data e hora em que você selecionar. Você tem duas opções adicionais:
   - **UTC**: A hora em que você selecionar será o tempo de Horário Coordenado Universal (UTC), para que as versões do aplicativo ao mesmo tempo em todos os lugares.
   - **Local**: A hora em que você selecionar serão usados em cada fuso horário associado a um mercado. (Observe que, no caso de mercados que incluem mais de um fuso horário, apenas um fuso horário desse mercado será usado. Para os Estados Unidos, o fuso horário do Leste dos EUA é usado.
- **não agendado**: O aplicativo não estará disponível na Store. Se você escolher essa opção, poderá disponibilizar o aplicativo na Loja mais tarde, criando um novo envio e escolhendo uma das outras opções.


## <a name="stop-acquisition"></a>Parar aquisição

Na lista suspensa **Parar aquisição**, você pode definir uma data e hora em que os novos clientes não possam mais adquirir o aplicativo na Loja ou descobri-lo na listagem. Isso pode ser útil se você quiser controlar com precisão quando um aplicativo não será oferecido mais para novos clientes; por exemplo, quando você estiver coordenando a disponibilidade de mais de um aplicativo.

Por padrão, a opção **Parar aquisição** é definida como nunca. Para alterar essa configuração, selecione **em** na lista suspensa e especifique uma data e hora, conforme descrito acima. Na data e hora que você selecionar, os clientes não poderão mais adquirir o aplicativo.

É importante entender que essa opção tem o mesmo impacto que a seleção **tornar este aplicativo localizável, mas não está disponível** na [visibilidade](choose-visibility-options.md#discoverability) seção e escolhendo **parar aquisição: Qualquer cliente com um link direto pode ver a listagem de Store do produto, mas eles podem baixá-lo somente se o produto antes, de propriedade ou tiver um código promocional e estiver usando um dispositivo Windows 10.** Para parar completamente de oferecer um aplicativo para novos clientes, clique em **Make app unavailable** na página de visão geral do aplicativo. Para obter mais informações, consulte [Removendo um aplicativo da Loja](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Se você selecionar uma data para **Parar aquisição** e depois decidir disponibilizar o aplicativo novamente, poderá criar um novo envio e alterar **Parar aquisição** para **Nunca**. O aplicativo ficará disponível novamente depois que o envio atualizado for publicado.

## <a name="customize-the-schedule-for-specific-markets"></a>Personalizar o agendamento para mercados específicos 

Por padrão, as opções selecionadas acima serão aplicadas a todos os mercados em que o aplicativo for oferecido. Para personalizar o preço para mercados específicos, clique em **Personalizar para mercados específicos**. A janela pop-up **Seleção de mercado** será exibida, listando todos os mercados em que você optou por disponibilizar o app. Se você tiver excluído algum mercado na seção [Mercados](define-pricing-and-market-selection.md), esses mercados não serão mostrados aqui. 

Para adicionar a agenda de um mercado, selecione-a e clique em **Salvar**. Depois, você verá as mesmas opções **Lançamento** e **Parar aquisição** descritas acima, mas as seleções feitas serão aplicadas somente a esse mercado.

Para adicionar um agendamento que se aplicará a vários mercados, crie um *grupo de mercados*. Para fazer isso, selecione os mercados que você deseja incluir e insira um nome para o grupo. (Esse nome é somente para referência e não ficará visível para todos os clientes) Por exemplo, se você quiser criar um grupo de mercado para América do Norte, você pode selecionar **Canadá**, **México**, e **dos Estados Unidos**e nomeie- **América do Norte**  ou outro nome que você escolher. Quando você terminar de criar o grupo de mercados, clique em **Salvar**. Depois, você verá as mesmas opções **Lançamento** e **Parar aquisição** descritas acima, mas as seleções feitas serão aplicadas somente a esse grupo de mercados.

Para adicionar uma agenda personalizada para um mercado adicional ou um grupo de mercados adicional, basta clicar novamente em **Personalizar para mercados específicos** e repetir essas etapas. Para alterar os mercados incluídos em um grupo de mercados, selecione seu nome. Para remover o agendamento personalizado de um grupo de mercados (ou um mercado individual), clique em **Remover**.

> [!NOTE]
> Um mercado não pode pertencer a mais de um dos grupos de mercado usados na seção **Agendamento**. 










