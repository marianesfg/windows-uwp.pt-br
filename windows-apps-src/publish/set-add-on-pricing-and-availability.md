---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: "Definir disponibilidade e preço de complemento"
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, complemento, cra, preço"
ms.localizationpriority: high
ms.openlocfilehash: 8eb2321ec6d2bc8602438e2dc66ca1d96690a3f8
ms.sourcegitcommit: 446fe2861651f51a129baa80791f565f81b4f317
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2018
---
# <a name="set-add-on-pricing-and-availability"></a>Definir disponibilidade e preço de complemento


Ao enviar um complemento, as opções na página **Preço e disponibilidade** determinam quanto cobrar por seu complemento e como ele deve ser oferecido aos clientes.

## <a name="markets"></a>Mercados

Por padrão, seu complemento será listado em todos os mercados possíveis, incluindo quaisquer mercados futuros que poderemos adicionar mais tarde, com o preço base.

No entanto, assim como com um aplicativo, você tem a opção de escolher os mercados em que gostaria de oferecer seu complemento. Na maioria dos casos, você vai querer selecionar o mesmo conjunto de mercados que o aplicativo, mas terá flexibilidade para fazer alterações, conforme necessário. 

Para obter mais informações e obter uma lista completa dos mercados disponíveis, consulte [Definir seleção de mercado](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidade

Você pode determinar se seu complemento deve ser oferecido para compra aos clientes. 

A opção padrão é **Podem ser exibidos na listagem da Store do produto pai**. Deixe essa opção marcada para complementos que serão disponibilizados para qualquer cliente. 

Para complementos que você não quer disponibilizar por completo, selecione **Oculto na Store** e uma das seguintes opções:

-   **Disponível para compra somente no produto pai**: a seleção dessa opção permite que qualquer cliente adquira o complemento no aplicativo, mas o complemento não será exibido na listagem da Store do aplicativo. Use essa opção somente quando a oferta não for amplamente disponível, por exemplo, durante os períodos iniciais de testes internos.
-   **Parar a aquisição: qualquer cliente com um link direto pode ver a listagem da Store do produto, mas é possível baixá-lo somente se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10. Esse complemento não é exibido na listagem do produto pai**: a seleção dessa opção significa que o complemento não será exibido na listagem do aplicativo e nenhum cliente pode adquirir o complemento. No entanto, **essa opção não tem suporte para os clientes no Windows 8.1 ou versões anteriores**. Se o seu aplicativo estiver disponível no Windows 8.1 ou anterior, o complemento ainda estará disponível para compra aos clientes. Para interromper a oferta deste complemento aos clientes no Windows 8.1 ou anterior, você precisará atualizar seu aplicativo para remover o código que oferece o complemento e depois publicar um novo envio do aplicativo. Isso é recomendado mesmo se o seu aplicativo não for para o Windows 8.1 ou versões anteriores. Seus clientes terão uma experiência melhor se você nunca lhes oferecer um complemento que você optou por tornar indisponível.
    
 > [!NOTE] 
 > A seleção da opção **Parar aquisição** e/ou enviar uma atualização de aplicativo que remove o complemento do código do aplicativo não afeta clientes que já adquiriram o complemento, independentemente do sistema operacional.


## <a name="schedule"></a>Agenda

Por padrão (a menos que você tenha selecionado uma das opções **Ocultar na Store** na seção **Visibilidade**), o complemento estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção. 

Para obter mais informações, consulte [Configurar agendamento preciso do lançamento](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preço

É necessário selecionar um preço base para o complemento (a não ser que você tenha selecionado a opção de **Parar aquisição** na seção **Visibilidade**), escolhendo **Gratuito** ou uma das faixas de preço disponíveis (a partir de US$ 0,99).

Você também pode agendar alterações de preço para indicar a data e a hora em que o preço do complemento deve ser alterado. Além disso, você tem a opção de personalizar essas alterações para mercados específicos. 

Para obter mais informações, consulte [Definir e agendar preço do aplicativo](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Preço de venda

Se você quiser oferecer seu complemento por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção. Para obter mais informações, consulte [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md).


## <a name="publish-date"></a>Data de publicação

Por padrão, seu envio iniciará o processo de publicação assim que for aprovado na certificação, a menos que você tenha configurado as datas na seção [**Agendamento**](#schedule) descrita acima. 

Para controlar quando o complemento deve ser publicado na Store, use a seção **Agendamento**. Na maioria dos envios, você deve usar essa seção para agendar o lançamento do aplicativo e deixar a seção **Data de publicação** definida como a opção padrão, **Publicar o envio assim que for aprovado na certificação**. Isso impedirá que o aplicativo seja publicado antes das datas definidas na seção **Agendamento**. As datas que você selecionou na seção **Agendamento** determinam quando o complemento será disponibilizado para os clientes na Store.

Se você ainda não quiser definir uma data de lançamento e preferir que o envio permaneça não publicado até você decidir manualmente iniciar o processo de publicação, escolha **Publicar este aplicativo manualmente**. Escolher essa opção significa que a seleção só será publicada depois que você confirmar. Depois que o complemento for aprovado na certificação, publique-o selecionando **Publicar agora** na página de status da certificação ou selecionando uma data específica, conforme descrito a seguir.

Escolha **Não antes de \[data\]** para garantir que o envio não seja publicado até uma data específica. Com esta opção, seu envio será lançado assim que possível na data especificada ou depois dela A data deve ser pelo menos 24 horas no futuro. Com a data, você também pode especificar a hora em que o envio deve começar a ser publicado.
 
> [!NOTE]
> Atrasos durante a certificação ou publicação podem fazer com que a data real do lançamento seja posterior à data solicitada. A Microsoft Store não pode garantir que seu complemento (ou atualização) estará disponível em uma data específica.  



 




