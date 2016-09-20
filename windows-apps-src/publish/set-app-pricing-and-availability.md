---
author: jnHs
Description: "A página Preço e disponibilidade do processo de envio de aplicativo permite que você determine quanto seu aplicativo vai custar, se você oferecerá uma avaliação gratuita e como, quando e onde ele estará disponível para os clientes."
title: "Definir a disponibilidade e o preço do aplicativo"
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 509ef3b8b9ec06907fccc2dbbe29fa3c72cb8e5d

---

# Definir a disponibilidade e o preço do aplicativo


A página **Preço e disponibilidade** do [processo de envio de aplicativo](app-submissions.md) permite que você determine quanto seu aplicativo vai custar, se você oferecerá uma avaliação gratuita e como, quando e onde ele estará disponível para os clientes. Aqui, nós o guiaremos pelas opções desta página e o que você deve considerar ao inserir essas informações.

## Preço base


O primeiro item desta página permite que você selecione um preço base para o seu aplicativo. Você pode optar por oferecê-lo gratuitamente, ou pode selecionar um das faixas de preço disponíveis. Especificar um preço base é necessário para enviar seu aplicativo.

Para saber mais, consulte [Definir preço e seleção de mercado](define-pricing-and-market-selection.md).

## Avaliação gratuita


Muitos desenvolvedores optam por permitir que os clientes experimentem seu aplicativo gratuitamente usando a funcionalidade de avaliação fornecida pela loja. Por padrão, um aplicativo não estará disponível como uma avaliação gratuita, mas se você gostaria de oferecer uma, selecione um valor na lista suspensa **Avaliação gratuita**.

Escolha **Avaliação nunca expira** para permitir que os clientes acessem seu aplicativo gratuitamente indefinidamente. Você vai querer incentivá-los a comprar a versão completa. Portanto, certifique-se de adicionar código para [excluir ou limitar recursos na versão de avaliação](https://msdn.microsoft.com/library/windows/apps/mt219685).

Você também tem a opção de selecionar uma avaliação por tempo limitado de **1 dia**, **7 dias**, **15 dias** ou **30 dias**. Você ainda pode limitar os recursos durante o período de avaliação, ou pode permitir que os clientes tenham acesso à funcionalidade completa durante esse período de tempo.

> 
            **Observação**  Avaliações por tempo limitado não são mostradas para os clientes no Windows Phone 8.1 e versões anteriores.

## Mercados e preços personalizados


Por padrão, seu aplicativo será listado em todos os mercados possíveis com seu preço base. Você pode alterar essas configurações para incluir ou excluir mercados específicos, e alterar o preço do aplicativo em qualquer mercado em que você o ofereça, na seção **Mercados e preços personalizados**. Para saber mais, consulte [Definir preço e seleção de mercado](define-pricing-and-market-selection.md).

## Preço de venda


Se você quiser oferecer seu aplicativo por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção. Para saber mais, consulte [Colocar aplicativos e IAPs em promoção](put-apps-and-iaps-on-sale.md).

## Distribuição e visibilidade


A seção **Distribuição e visibilidade** permite que você defina restrições sobre como seu aplicativo pode ser descoberto e adquirido.

A configuração padrão é **Tornam este aplicativo disponível na Loja**. Isso significa que seu aplicativo será listado na Loja para os clientes encontrarem via link direto do aplicativo e/ou por outros métodos, como pesquisa, navegação e inclusão em listas comissionadas.

Se você quiser ocultar seu aplicativo na Loja, mas ainda quiser torná-lo disponível para algumas pessoas, selecione uma das opções a seguir para limitar a disponibilidade do aplicativo. Observe que os clientes do Windows 8 e do Windows 8.1 não conseguirão baixar o aplicativo se você escolher qualquer uma dessas opções.

-   
            **Ocultar esse aplicativo e evitar a aquisição. Os clientes com um código promocional ainda podem baixá-lo em dispositivos Windows 10**: nenhum cliente poderá encontrar seu aplicativo na Loja por meio de pesquisas ou navegação, mas você pode [gerar códigos promocionais](generate-promotional-codes.md) para distribuição a pessoas específicas no Windows 10. Eles podem usar o link e o código para acessar seu aplicativo gratuitamente, mesmo que você não o esteja oferecendo para quaisquer outros clientes
-   
            **Ocultar este aplicativo na Loja. Os clientes com um link direto para o detalhe do aplicativo ainda podem baixá-lo, exceto no Windows 8 e no Windows 8.1**: nenhum cliente poderá localizar seu aplicativo na Loja por meio de pesquisa ou navegação, mas qualquer cliente com o link direto para a listagem do aplicativo pode baixá-lo em dispositivos Windows 10 ou Windows Phone 8.1 e versões anteriores.
-   
            **Ocultar esse aplicativo e disponibilizá-lo somente para as pessoas que você especificar abaixo, que podem baixá-lo em dispositivos Windows Phone 8.x. Um código promocional pode ser usado para baixar o aplicativo em dispositivos Windows 10**: nenhum cliente pode encontrar seu aplicativo na Loja por meio de pesquisas ou navegação, e somente os clientes do Windows Phone 8.x cujos endereços de email (associados às respectivas contas da Microsoft) que você inserir na caixa (separados por ponto-e-vírgula) poderão baixar o aplicativo usando o link direto para sua listagem. Você também pode [gerar códigos promocionais](generate-promotional-codes.md) para distribuição a pessoas específicas no Windows 10. Essa opção é frequentemente usada para [teste beta](beta-testing-and-targeted-distribution.md) no Windows Phone 8.1 e versões anteriores. Observe que essa opção só pode ser selecionada se você nunca tiver publicado o aplicativo com a opção **Distribuição e visibilidade** definida como **Qualquer pessoa pode encontrar seu aplicativo na Loja**.

> 
            **Observação**  Para parar completamente de oferecer um aplicativo para novos clientes, clique em **Tornar aplicativo indisponível** na página de visão geral do aplicativo. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Loja, e os novos clientes não poderão obtê-lo por meio de qualquer método. Esta ação substituirá qualquer uma das opções que você escolheu aqui: ele não estará disponível para novos clientes. Para disponibilizá-lo para novos clientes novamente, você pode a qualquer momento clicar em **Tornar aplicativo disponível** na página de visão geral do aplicativo. Para saber mais, consulte [Removendo um aplicativo da Loja](guidance-for-app-package-management.md#removing-an-app-from-the-store).

## Famílias de dispositivos Windows 10

Esta seção permite que você indique quais tipos de dispositivos Windows 10 os clientes podem usar para comprar seu aplicativo. (Se o pacote não será executado em um determinado tipo de dispositivo, não o ofereceremos para download para esse tipo de dispositivo.)

> 
            **Importante**  Para evitar completamente que uma determinada família de dispositivos do Windows 10 obtenha seu aplicativo, você precisa atualizar o elemento [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) em seu manifesto appx para direcionar somente à família de dispositivos a qual você deseja dar suporte (isto é, **Windows.Mobile** ou **Windows.Desktop**), em vez de deixá-lo como o valor **Windows.Universal** (para a família de dispositivos universal) que o Microsoft Visual Studio inclui no manifesto appx por padrão.

Por padrão, as caixas para **Móvel** e **Desktop** estarão marcadas. Recomendamos deixar essas caixas marcadas, a menos que você tenha um motivo específico para limitar os tipos de dispositivos do Windows 10 que podem adquirir o seu aplicativo. Por exemplo, você pode ter criado pacotes universais do Windows, mas sabe que ainda precisa testar alguns problemas com o aplicativo em dispositivos móveis. Para impedir que novos clientes baixem o aplicativo em dispositivos móveis do Windows 10, você pode desmarcar a caixa de seleção **Móvel** aqui. Em seguida, se você decidir mais tarde que está pronto para oferecê-lo aos clientes em dispositivos móveis do Windows 10, crie um novo envio com a caixa **Móvel** marcada.

Se você testou o aplicativo para garantir que ele seja executado corretamente no Microsoft HoloLens, também poderá marcar a caixa **Holográfico** para oferecer o aplicativo aos clientes do HoloLens. Para saber mais sobre como criar, testar e publicar aplicativos holográficos, consulte a [Visão geral de Desenvolvimento Holográfico do Windows](http://dev.windows.com/holographic/development_overview).

Observe que as seleções feitas nesta seção serão aplicadas a todos os pacotes do aplicativo, independentemente da versão do sistema operacional a que eles se destinam (Windows 10, Windows 8.x, Windows Phone 8.x etc.). No entanto, elas afetam a disponibilidade apenas para os clientes que estão usando dispositivos Windows 10 (e não dispositivos Windows 8.x ou Windows Phone 8.x).

É importante também estar ciente de que as seleções feitas aqui serão aplicadas apenas a novas aquisições. Quem já tem o seu aplicativo pode continuar a usá-lo e receberá as atualizações enviadas, mesmo se você remover essa família de dispositivos aqui. Isso se aplica inclusive aos clientes que compraram o aplicativo antes de atualizar para o Windows 10. Por exemplo, se você tiver um aplicativo publicado com pacotes do Windows Phone 8.1, e mais tarde adicionar um pacote do Windows 10 (UWP) ao mesmo aplicativo direcionado à família de dispositivos universal, será oferecido para os clientes móveis do Windows 10 que tinham o pacote do Windows Phone 8.1 uma atualização para esse pacote do Windows 10 (UWP), mesmo se você tiver desmarcado a caixa **Móvel** (já que essa não é uma nova aquisição mas uma atualização). No entanto, se você não fornecer qualquer pacote do Windows 10 (UWP) destinado à família de dispositivos universal ou móveis, os clientes móveis do Windows 10 permanecerão com o pacote do Windows Phone 8.1.

Para saber mais sobre famílias de dispositivos, veja [Guia para aplicativos UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/windows/apps/dn894631) e [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

> 
            **Observação**  Você também verá uma caixa de seleção na qual pode indicar se deseja permitir que a Microsoft disponibilize o aplicativo para famílias de dispositivos Windows 10 futuras. É recomendável manter essa caixa marcada para que seu aplicativo possa estar disponível para clientes em potencial, conforme novas famílias de dispositivos forem introduzidas.

## Licenciamento para organizações


Por padrão, seu aplicativo pode ser oferecido para que as organizações comprem por volume. Você pode indicar se e como seu aplicativo pode ser oferecido nesta seção.

Para saber mais, consulte [Opções de licenciamento para organizações](organizational-licensing.md).

## Data de publicação


Você pode indicar quando seu aplicativo (ou atualização) será publicado, escolhendo uma opção na seção **Data de publicação**.

-   Escolha **Publicar este envio assim que ele for aprovado na certificação** para disponibilizá-lo na Loja assim que possível.
-   Escolha **Publicar manualmente este envio** se você não quiser que seu aplicativo seja publicado até você indicar que ele deve ser. Você pode fazer isso na página de status de certificação, clicando em **Publicar agora**, ou selecionando uma data específica, conforme descrito a seguir.
-   Escolha **Não mais cedo do que \[data\]** para garantir que o envio não seja publicado até uma data específica. Com esta opção, seu envio será lançado assim que possível na data especificada ou depois dela A data deve ser pelo menos 24 horas no futuro. Com a data, você também pode especificar a hora em que o envio deve começar a ser publicado.
    > 
            **Observação**  Atrasos durante a certificação ou publicação podem fazer com que a data real de lançamento seja posterior à data solicitada. A Windows Store não pode garantir que seu aplicativo (ou atualização) estará disponível em uma data específica.

Você também pode mudar a data de lançamento após enviar o aplicativo, desde que ele ainda não tenha entrado na etapa **Publicação**.
 

 







<!--HONumber=Jun16_HO4-->


