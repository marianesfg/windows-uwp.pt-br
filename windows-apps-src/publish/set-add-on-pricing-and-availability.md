---
Description: Ao enviar um complemento, as opções na página Preço e disponibilidade determinam quanto cobrar por seu complemento e como ele deve ser oferecido aos clientes.
title: Definir disponibilidade e preço de complemento
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complemento, cra, preço
ms.localizationpriority: medium
ms.openlocfilehash: 803164c395602313bcb84331e30376efd6832731
ms.sourcegitcommit: 912146681b1befc43e6db6e06d1e3317e5987592
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79295719"
---
# <a name="set-add-on-pricing-and-availability"></a>Definir disponibilidade e preço de complemento

Ao enviar um complemento no [Partner Center](https://partner.microsoft.com/dashboard), as opções na página **preços e disponibilidade** determinam quanto cobrar os clientes pelo seu complemento e como eles devem ser oferecidos aos clientes.

## <a name="markets"></a>Mercados

Por padrão, seu complemento será listado em todos os mercados possíveis, incluindo quaisquer mercados futuros que poderemos adicionar mais tarde, com o preço base.

No entanto, assim como com um aplicativo, você tem a opção de escolher os mercados em que gostaria de oferecer seu complemento. Na maioria dos casos, você vai querer selecionar o mesmo conjunto de mercados que o aplicativo, mas terá flexibilidade para fazer alterações, conforme necessário. 

Para obter mais informações e obter uma lista completa dos mercados disponíveis, consulte [Definir seleção de mercado](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidade

Você pode determinar se seu complemento deve ser oferecido para compra aos clientes. 

A opção padrão é **Podem ser exibidos na listagem da Store do produto pai**. Deixe essa opção marcada para complementos que serão disponibilizados para qualquer cliente. 

Para complementos que você não quer disponibilizar por completo, selecione **Oculto na Store** e uma das seguintes opções:

-   **Disponível para compra somente de dentro do produto pai**: a escolha dessa opção permite que qualquer cliente adquira o complemento de dentro de seu aplicativo, mas o complemento não será exibido na listagem de armazenamento do aplicativo ou detectável na loja. Use essa opção somente quando a oferta não for amplamente disponível, por exemplo, durante os períodos iniciais de testes internos.
-   **Parar a aquisição: qualquer cliente com um link direto pode ver a listagem da Store do produto, mas é possível baixá-lo somente se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10. Esse complemento não é exibido na listagem do produto pai**: a seleção dessa opção significa que o complemento não será exibido na listagem do aplicativo e nenhum cliente pode adquirir o complemento. No entanto, **essa opção não tem suporte para clientes no Windows 8.1 ou anterior**. Se o aplicativo publicado anteriormente estiver disponível no Windows 8.1 ou anterior, o complemento ainda estará disponível para compra para esses clientes. Para parar de oferecer o complemento aos clientes no Windows 8.1 ou anterior, você precisará atualizar seu aplicativo para remover o código que oferece o complemento e, em seguida, publicar um novo envio para o aplicativo. Isso é recomendado mesmo que seu aplicativo não direcione Windows 8.1 ou anterior; é uma experiência melhor para seus clientes se você nunca oferecer a eles um complemento que você tenha optado por tornar-se indisponível.
    
 > [!NOTE] 
 > Escolher a opção **parar a aquisição** e/ou enviar uma atualização de aplicativo que remove o complemento do seu aplicativo não impedirá que os clientes usem o complemento se já o tiverem comprado. As assinaturas existentes não serão renovadas e subsequentemente serão canceladas depois que o termo atual terminar.


## <a name="schedule"></a>Agendamento

Por padrão (a menos que você tenha selecionado uma das opções **Ocultar na Store** na seção **Visibilidade**), o complemento estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção. 

Para obter mais informações, consulte [Configurar agendamento preciso do lançamento](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preços

Você deve selecionar um preço base para seu complemento (a menos que tenha selecionado a opção **parar aquisição** na seção **visibilidade** ). A seleção padrão é **gratuita**, portanto, se você quiser cobrar dinheiro pelo complemento, certifique-se de escolher um dos tipos de preço disponíveis (começando em 99 USD).

Você também pode agendar alterações de preço para indicar a data e a hora em que o preço do complemento deve ser alterado. Além disso, você tem a opção de personalizar essas alterações para mercados específicos. 

> [!TIP]
> Para Complementos de assinatura, você não pode aumentar o preço depois de publicar o complemento, seja selecionando um preço base mais alto em um envio posterior ou agendando uma alteração de preço que aumente o preço. Você pode selecionar um preço menor usando qualquer um desses métodos, mas depois que o preço for reduzido, você não poderá aumentar esse preço para um valor maior. Por isso, é especialmente importante ter certeza de que você selecionou o tipo de preço apropriado para Complementos de assinatura. 

Para obter mais informações, confira [Definir e agendar preço do aplicativo](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Preço promocional

Se você quiser oferecer seu complemento por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção. Para obter mais informações, consulte [Colocar aplicativos e complementos em promoção](put-apps-and-add-ons-on-sale.md).



