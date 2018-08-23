---
author: jnHs
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: Definir a disponibilidade e o preço do aplicativo
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, preço, disponível, detectável, avaliação gratuita, avaliações, avaliação, apps, data de lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 20c52687b375f9bf33dd491eeb37d4142acace99
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2819518"
---
# <a name="set-app-pricing-and-availability"></a>Definir a disponibilidade e o preço do aplicativo


A página **Preço e disponibilidade** do [processo de envio de aplicativo](app-submissions.md) permite que você determine quanto seu aplicativo vai custar, se você oferecerá uma avaliação gratuita e como, quando e onde ele estará disponível para os clientes. Aqui, nós o guiaremos pelas opções desta página e o que você deve considerar ao inserir essas informações.


## <a name="markets"></a>Mercados

A Microsoft Store abrange clientes em mais de 200 países e regiões em todo o mundo. Por padrão, ofereceremos seu aplicativo em todos os mercados possíveis. Se você preferir, poderá escolher os mercados específicos nos quais deseja oferecer seu aplicativo. 

Para obter mais informações, consulte [Definir seleção de mercado](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Visibilidade

A seção **Visibilidade** permite que você defina restrições sobre como seu app pode ser descoberto e adquirido, inclusive se as pessoas podem encontrar seu app na Store ou ver sua listagem da Store.

Para saber mais, consulte [Escolher opções de visibilidade](choose-visibility-options.md).


## <a name="schedule"></a>Agendar

Por padrão (a menos que você tenha selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Store** na seção [Visibilidade](choose-visibility-options.md#discoverability)), o app estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção. 

Para obter mais informações, consulte [Configurar agendamento preciso do lançamento](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preço

É necessário selecionar um preço base para seu app (a não ser que você tenha selecionado a opção **Parar aquisição** em **Disponibilizar este produto mas não torná-lo detectável na Store** na seção [Visibilidade](choose-visibility-options.md#discoverability)), escolhendo **Gratuito** ou uma das faixas de preço disponíveis. Você também pode agendar alterações de preço para indicar a data e a hora em que o preço do aplicativo deve ser alterado. Além disso, você tem a opção de personalizar essas alterações para mercados específicos. 

Para obter mais informações, consulte [Definir e agendar preço do aplicativo](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Avaliação gratuita

Muitos desenvolvedores optam por permitir que os clientes experimentem seu aplicativo gratuitamente usando a funcionalidade de avaliação fornecida pela Loja. Por padrão, a opção **Nenhuma avaliação gratuita** é selecionada, e não haverá nenhuma avaliação para seu aplicativo. Se você quiser oferecer uma avaliação, poderá selecionar um valor na lista suspensa **Avaliação gratuita**.

Existem dois tipos de versão de avaliação que você pode escolher, e você tem a opção de configurar a data e a hora de início e término da oferta da versão de avaliação.

### <a name="time-limited"></a>Limitada ao tempo

Escolha **Limitada ao tempo** para permitir que os clientes testem o aplicativo gratuitamente por um determinado número de dias: **1 dia**, **7 dias**, **15 dias** ou **30 dias**. Você pode limitar os recursos adicionando código a [excluir ou limitar recursos na versão de avaliação](../monetize/in-app-purchases-and-trials.md), ou pode permitir que os clientes acessem a funcionalidade completa durante esse período. 
> [!NOTE]
> As versões de avaliação por tempo limitado não são mostradas para os clientes no Windows 10 build 10.0.10586 ou versões anteriores nem para os clientes no Windows Phone 8.1 e versões anteriores.

### <a name="unlimited"></a>Ilimitado

Escolha **Ilimitado** para permitir que os clientes acessem seu aplicativo gratuitamente, por tempo ilimitado. Você vai querer incentivá-los a comprar a versão completa. Portanto, certifique-se de adicionar código para [excluir ou limitar recursos na versão de avaliação](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Datas de início e término

Por padrão, a versão de avaliação será disponibilizada assim que o aplicativo for publicado e a oferta nunca será interrompida. Se quiser, você pode especificar a data e a hora em que sua versão de avaliação deve começar a ser oferecida e quando essa oferta deve ser interrompida. 

>[!NOTE]
> Essas datas se aplicam somente a clientes no Windows 10 (incluindo o Xbox). Se o app estiver disponível para os clientes nas versões anteriores do sistema operacional, a versão de avaliação será oferecida a esses clientes enquanto seu produto estiver disponível. 

Para definir as datas em que sua avaliação deve ser oferecida aos clientes no Windows 10, altere a lista suspensa **Começa em** e/ou **Termina em** para **em** e escolha a data e a hora. Se você fizer isso, poderá escolher **UTC** para que a hora selecionada seja no fuso horário Horário Coordenado Universal (UTC) ou escolha **Local** para que esses períodos sejam usados em cada fuso horário associado a um mercado. (Observe que, no caso de mercados que incluem mais de um fuso horário, apenas um fuso horário desse mercado será usado. Para os Estados Unidos, o fuso horário do Leste é usado.) Se você quiser definir datas diferentes para qualquer market(s), você pode selecionar **Personalizar para mercados específicos** .


## <a name="sale-pricing"></a>Preço de venda

Se você quiser oferecer seu aplicativo por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção.

Para obter mais informações, consulte [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Licenciamento para organizações

Por padrão, seu aplicativo pode ser oferecido para que as organizações comprem por volume. Você pode indicar se e como seu aplicativo pode ser oferecido nesta seção.

Para obter mais informações, consulte [Opções de licenciamento para organizações](organizational-licensing.md).


## <a name="publish-date"></a>Data de publicação

Anteriormente, a seção **Data de publicação** apareceu nesta página. Agora, essa funcionalidade pode ser encontrada na página [Opções de envio](manage-submission-options.md), na seção **Opções de bloqueio de publicação**. (Observe que, para controlar quando o app deverá ser publicado na Store, recomendamos usar a seção [Agendar](configure-precise-release-scheduling.md) da página **Preço e disponibilidade**).


