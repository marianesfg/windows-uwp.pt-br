---
Description: Ao enviar um complemento, as opções na página Preço e disponibilidade determinam quanto cobrar por seu complemento e como ele deve ser oferecido aos clientes.
title: Definir disponibilidade e preço de complemento
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complemento, cra, preço
ms.localizationpriority: medium
ms.openlocfilehash: 062337c82d2567d15b0eff1767ab157618da257e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597801"
---
# <a name="set-add-on-pricing-and-availability"></a>Definir disponibilidade e preço de complemento

Ao enviar um complemento na [Partner Center](https://partner.microsoft.com/dashboard), as opções a **preços e disponibilidade** página determinar o quanto a cobrar os clientes para seu complemento e como ele deve ser oferecido aos clientes.

## <a name="markets"></a>Mercados

Por padrão, seu complemento será listado em todos os mercados possíveis, incluindo quaisquer mercados futuros que poderemos adicionar mais tarde, com o preço base.

No entanto, assim como com um aplicativo, você tem a opção de escolher os mercados em que gostaria de oferecer seu complemento. Na maioria dos casos, você vai querer selecionar o mesmo conjunto de mercados que o aplicativo, mas terá flexibilidade para fazer alterações, conforme necessário. 

Para obter mais informações e obter uma lista completa dos mercados disponíveis, consulte [Definir seleção de mercado](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidade

Você pode determinar se seu complemento deve ser oferecido para compra aos clientes. 

A opção padrão é **Podem ser exibidos na listagem da Store do produto pai**. Deixe essa opção marcada para complementos que serão disponibilizados para qualquer cliente. 

Para complementos que você não quer disponibilizar por completo, selecione **Oculto na Store** e uma das seguintes opções:

-   **Disponível para compra dentro do produto pai somente**: Esta opção permite que qualquer cliente comprar o complemento de dentro de seu aplicativo, mas o complemento não será exibido na listagem de Store do seu aplicativo ou detectável na Store. Use essa opção somente quando a oferta não for amplamente disponível, por exemplo, durante os períodos iniciais de testes internos.
-   **Pare a aquisição de: Qualquer cliente com um link direto pode ver a listagem de Store do produto, mas eles podem baixá-lo somente se o produto antes, de propriedade ou tiver um código promocional e estiver usando um dispositivo Windows 10. Este complemento não é exibido na listagem do produto pai**: Escolher essa opção significa que o complemento não será exibido nos detalhes de seu aplicativo e novos clientes não podem adquirir o complemento. No entanto, **essa opção não tem suporte para clientes Windows 8.1 ou anterior**. Se seu aplicativo publicado anteriormente está disponível no Windows 8.1 ou anterior, o complemento ainda estará disponível para compra aos clientes. Para interromper a oferta de complemento aos clientes no Windows 8.1 ou anterior, você precisará atualizar seu aplicativo para remover o código que oferece o complemento e, em seguida, publicar um envio de novo para o aplicativo. Isso é recomendado, mesmo se seu aplicativo não tem como meta Windows 8.1 ou anterior; é uma melhor experiência para seus clientes se você nunca oferecê-las um complemento que você tenha optado por tornar indisponível.
    
 > [!NOTE] 
 > A seleção da opção **Parar aquisição** e/ou enviar uma atualização de aplicativo que remove o complemento do código do aplicativo não afeta clientes que já adquiriram o complemento, independentemente do sistema operacional.


## <a name="schedule"></a>Agendamento

Por padrão (a menos que você tenha selecionado uma das opções **Ocultar na Store** na seção **Visibilidade**), o complemento estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção. 

Para obter mais informações, consulte [Configurar agendamento preciso do lançamento](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preço

Você deve selecionar um preço base para seu complemento (a menos que você selecionou a **parar a aquisição** opção a **visibilidade** seção). A seleção padrão é **gratuito**, portanto, se você quiser cobrar dinheiro para o complemento, certifique-se de escolher uma das camadas de preço disponíveis (começando em USD.99).

Você também pode agendar alterações de preço para indicar a data e a hora em que o preço do complemento deve ser alterado. Além disso, você tem a opção de personalizar essas alterações para mercados específicos. 

> [!TIP]
> Para complementos de assinatura, você não pode aumentar o preço depois de publicar o complemento, selecionando um preço mais alto de base em um envio posterior ou com o agendamento de uma alteração de preço que aumenta o preço. Você pode selecionar um preço mais baixo usando qualquer um desses métodos, mas depois que o preço é reduzido não será capaz de gerá-lo maior do que esse novo preço. Por isso, é especialmente importante para garantir que você selecionar a camada de preços apropriada para complementos de assinaturas. 

Para obter mais informações, consulte [Definir e agendar preço do aplicativo](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Preço promocional

Se você quiser oferecer seu complemento por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção. Para obter mais informações, consulte [Colocar aplicativos e complementos em promoção](put-apps-and-add-ons-on-sale.md).



