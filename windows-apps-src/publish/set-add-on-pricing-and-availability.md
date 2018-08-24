---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: Definir disponibilidade e preço de complemento
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, complemento, cra, preço
ms.localizationpriority: medium
ms.openlocfilehash: b5b7a6424fea3d62849e992f56b0b40ab72a55f5
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2836738"
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

Você deve selecionar um preço base para seu complemento (a menos que você selecionou a opção de **Parar aquisição** na seção de **visibilidade** ). A seleção padrão é **livre**, portanto, se você quiser são cobradas para o complemento, escolha uma das camadas preço disponíveis (começando em USD.99).

Você também pode agendar alterações de preço para indicar a data e a hora em que o preço do complemento deve ser alterado. Além disso, você tem a opção de personalizar essas alterações para mercados específicos. 

> [!TIP]
> Complementos de inscrição, você não pode elevar o preço depois de publicar o complemento, selecionando um preço de base superior em um envio posterior ou agendando uma mudança de preço que aumenta o preço. Você pode selecionar um preço menor usando qualquer um desses métodos, mas depois que o preço é reduzido não será possível que eleve o maior do que esse novo preço. Por isso, é especialmente importante para garantir que você selecionar a camada de preço apropriada complementos de assinatura. 

Para obter mais informações, consulte [Definir e agendar preço do aplicativo](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Preço de venda

Se você quiser oferecer seu complemento por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção. Para obter mais informações, consulte [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md).


