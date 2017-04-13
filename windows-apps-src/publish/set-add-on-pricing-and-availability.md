---
author: jnHs
Description: "Ao enviar um complemento, as opções na página Preço e disponibilidade determinam quanto cobrar por seu complemento e como ele deve ser oferecido aos clientes."
title: "Definir disponibilidade e preço de complemento"
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2902b81e777662e0d71bf10553ed82087c5cee9a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-add-on-pricing-and-availability"></a>Definir disponibilidade e preço de complemento


Ao enviar um complemento, as opções na página **Preço e disponibilidade** determinam quanto cobrar por seu complemento e como ele deve ser oferecido aos clientes.

## <a name="base-price"></a>Preço base


Você deve selecionar um preço base para seu complemento. Essas faixas de preço são as mesmas dos aplicativos, começando em 0,99 USD. Você também tem a opção de oferecer seu complemento gratuitamente.

## <a name="markets-and-custom-prices"></a>Mercados e preços personalizados


Por padrão, seu complemento será listado em todos os mercados possíveis, incluindo quaisquer mercados futuros que poderemos adicionar mais tarde, com o preço base.

No entanto, assim como com um aplicativo, você tem a opção de escolher os mercados em que gostaria de oferecer seu complemento. Na maioria dos casos, você vai querer selecionar o mesmo conjunto de mercados que o aplicativo, mas terá flexibilidade para fazer alterações, conforme necessário. Você também pode definir preços personalizados para que possa cobrar preços diferentes para o complemento em diferentes mercados.

Para obter mais informações e obter uma lista completa dos mercados disponíveis, consulte [Definir preço e seleção de mercado](define-pricing-and-market-selection.md).

## <a name="sale-pricing"></a>Preço de venda


Se você quiser oferecer seu complemento por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção. Para obter mais informações, consulte [Colocar aplicativos e complementos em promoção](put-apps-and-add-ons-on-sale.md).

## <a name="distribution-and-visibility"></a>Distribuição e visibilidade


Você pode determinar se seu complemento deve ser oferecido para compra aos clientes. Escolha uma das seguintes opções:

-   **Disponível para compra. Podem ser exibidas na página de detalhes do seu aplicativo:** esta é a configuração padrão e é recomendada, a menos que você deseje restringir o acesso ao seu complemento. Deixe essa opção marcada para complementos que serão disponibilizados para qualquer cliente.
-   **Disponível para compra. Não exibido nos detalhes do seu aplicativo:** escolher essa opção permite que os clientes comprem o complemento de dentro de seu aplicativo, mas o complemento não será exibido na listagem da Loja do aplicativo. Use essa opção somente quando a oferta não for amplamente disponível, por exemplo, durante os períodos iniciais de testes internos.
-   **Não estão mais disponíveis para compra. Não é exibido nos detalhes do seu aplicativo.** Escolher essa opção significa que o complemento não será exibido nos detalhes de seu aplicativo e novos clientes não podem adquirir o complemento. No entanto, **essa opção não tem suporte para os clientes no Windows 8.1 ou versões anteriores**. Se o seu aplicativo estiver disponível no Windows 8.1 ou anterior, o complemento ainda estará disponível para compra aos clientes. Para interromper a oferta deste complemento aos clientes no Windows 8.1 ou anterior, você precisará atualizar seu aplicativo para remover o código que oferece o complemento e depois publicar um novo envio do aplicativo. Isso é recomendado mesmo se o seu aplicativo não for para o Windows 8.1 ou versões anteriores. Seus clientes terão uma experiência melhor se você nunca lhes oferecer um complemento que você optou por tornar indisponível.
    
 > **Observação**  Escolher essa configuração e/ou enviar uma atualização de aplicativo que remove o complemento do código do seu aplicativo não afeta clientes que já compraram o complemento, independentemente do sistema operacional.


## <a name="publish-date"></a>Data de publicação

Você pode indicar quando o envio do complemento deverá ser publicado, escolhendo uma opção na seção **Data de publicação**.

-   Escolha **Publicar meu complemento assim que ele for aprovado na certificação** para disponibilizá-lo na Loja assim que possível.
-   Escolha **Publicar manualmente este complemento** se você não quiser que seu aplicativo seja publicado até você indicar que ele deve ser. Você pode fazer isso na página de status de certificação, clicando em **Publicar agora**, ou selecionando uma data específica, conforme descrito a seguir.
-   Escolha **Não mais cedo do que \[data\]** para garantir que o envio não seja publicado até uma data específica. Com esta opção, seu envio será lançado assim que possível na data especificada ou depois dela A data deve ser pelo menos 24 horas no futuro. Com a data, você também pode especificar a hora em que o envio deve começar a ser publicado.

 > **Observação**  Atrasos durante a certificação ou publicação podem fazer com que a data real de lançamento seja posterior à data solicitada. A Windows Store não pode garantir que seu complemento (ou atualização) estará disponível em uma data específica.
 

 




