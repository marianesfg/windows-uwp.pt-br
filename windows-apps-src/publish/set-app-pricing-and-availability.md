---
author: jnHs
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: Definir a disponibilidade e o preço do aplicativo
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: high
ms.openlocfilehash: 3e94aadefb418aa7cefa90b8f274868c80f3e480
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2018
---
# <a name="set-app-pricing-and-availability"></a>Definir a disponibilidade e o preço do aplicativo


A página **Preço e disponibilidade** do [processo de envio de aplicativo](app-submissions.md) permite que você determine quanto seu aplicativo vai custar, se você oferecerá uma avaliação gratuita e como, quando e onde ele estará disponível para os clientes. Aqui, nós o guiaremos pelas opções desta página e o que você deve considerar ao inserir essas informações.


## <a name="markets"></a>Mercados

A Microsoft Store abrange clientes em mais de 200 países e regiões em todo o mundo. Por padrão, ofereceremos seu aplicativo em todos os mercados possíveis. Se você preferir, poderá escolher os mercados específicos nos quais deseja oferecer seu aplicativo. 

Para obter mais informações, consulte [Definir seleção de mercado](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Visibilidade

A seção **Visibilidade** permite que você defina restrições sobre como seu aplicativo pode ser descoberto e adquirido.

A configuração padrão é **Make this product available and discoverable in the Store**. Isso significa que seu aplicativo será listado na Loja para os clientes encontrarem via link direto do aplicativo e/ou por outros métodos, como pesquisa, navegação e inclusão em listas comissionadas. 

Se você quiser ocultar seu aplicativo na Loja, mas ainda quiser torná-lo disponível para algumas pessoas, selecione **Mostrar opções** para expandir a seção, selecione **Disponibilizar este produto mas não torná-lo detectável na Loja**. Isso significa que nenhum cliente poderá encontrar seu aplicativo na Loja por meio de pesquisa ou navegação, independentemente da versão do sistema operacional. Você também deve escolher uma das seguintes versões para determinar como seu aplicativo pode ser adquirido.

>[!IMPORTANT]
> Cada uma dessas opções limita as versões de sistema operacional no qual os clientes podem adquirir o aplicativo. Leia as descrições atentamente para garantir que você sabe quais versões do sistema operacional são compatíveis. Observe que os clientes do Windows 8 e Windows 8.1 não conseguirão baixar o aplicativo se você escolher qualquer uma das opções em **Disponibilizar este produto mas não torná-lo detectável na Loja**. 

- **Somente link direto: qualquer cliente com um link direto para a listagem do produto pode baixá-lo, exceto no Windows 8.x.** Qualquer cliente que obtém a listagem do seu aplicativo por meio de um link direto pode baixá-lo em dispositivos que executam o Windows 10 ou o Windows Phone 8.1 e versões anteriores. Os clientes no Windows 8. x não podem obter o aplicativo com essa opção.
- **Parar a aquisição: os clientes com um link direto poderão ver a listagem da Loja do produto, mas só poderão baixá-lo se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10.** Mesmo que um cliente tenha um link direto, não é possível baixar o aplicativo, a menos que ele tenha um [código promocional](generate-promotional-codes.md) e esteja usando um dispositivo Windows 10. Ao fornecer um código para um cliente, ele poderá usar o link e o código para acessar seu aplicativo gratuitamente (no Windows 10 somente), mesmo que você não esteja oferecendo o aplicativo a outros clientes. Não há nenhuma outra forma de obter seu aplicativo a não ser através de um código promocional.
- **Somente para pessoas no Windows Phone 8.x: somente as pessoas que você especificar abaixo podem baixar este produto em um dispositivo Windows Phone 8.x. Qualquer pessoa com um link direto e um código promocional podem baixar o produto em um dispositivo Windows 10.** Essa opção pode não aparecer em todos os envios. Ele só se aplicará se você tiver pacotes que podem ser executados no Windows Phone 8. x. Somente os clientes cujos endereços de email (associados às suas contas da Microsoft) você inserir na caixa (separados por ponto-e-vírgulas) poderão baixar seu aplicativo no Windows Phone 8.x usando o link direto para sua listagem. Você também pode gerar códigos promocionais para realizar a distribuição a pessoas específicas no Windows 10, conforme descrito acima. 

> [!TIP]
> Se você desejar parar completamente de oferecer um aplicativo para quaisquer novos clientes, é necessário selecionar **Tornar aplicativo indisponível** na página de visão geral. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Microsoft Store, e os novos clientes não poderão obtê-lo (exceto com um [código promocional](generate-promotional-codes.md) e se estiver no dispositivo Windows 10). Esta ação substituirá as seleções de **Visibilidade** no seu envio. Para disponibilizar o aplicativo para novos clientes novamente (de acordo com seleções de **Visibilidade**), você pode a qualquer momento clicar em **Tornar aplicativo disponível** na página de visão geral do aplicativo. Para obter mais informações, consulte [Removendo um aplicativo da Loja](guidance-for-app-package-management.md#removing-an-app-from-the-store).


## <a name="schedule"></a>Agenda

Por padrão (a menos que você tenha selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção **Visibilidade**), o aplicativo estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção. 

Para obter mais informações, consulte [Configurar agendamento preciso do lançamento](configure-precise-release-scheduling.md).


## <a name="display-release-date"></a>Exibir data de lançamento

Por padrão, a data de lançamento do aplicativo será a data em que ele aparece na Loja. Se você quiser substituir isso e fornecer uma data de lançamento personalizada, marque a caixa nesta seção e insira a data de lançamento. Observe que a data de lançamento nem sempre é exibida nas listagens da Loja.


## <a name="pricing"></a>Preço

É necessário selecionar um preço base para seu aplicativo (a não ser que você tenha selecionado uma das opções de **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](#visibility) seção), escolhendo **Gratuito** ou uma das faixas de preço disponíveis. Você também pode agendar alterações de preço para indicar a data e a hora em que o preço do aplicativo deve ser alterado. Além disso, você tem a opção de personalizar essas alterações para mercados específicos. 

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

Para definir as datas em que sua avaliação deve ser oferecida aos clientes no Windows 10, altere a lista suspensa **Começa em** e/ou **Termina em** para **em** e escolha a data e a hora. Se você fizer isso, poderá escolher **UTC** para que a hora selecionada seja no fuso horário Horário Coordenado Universal (UTC) ou escolha **Local** para que esses períodos sejam usados em cada fuso horário associado a um mercado. (Observe que, no caso de mercados que incluem mais de um fuso horário, apenas um fuso horário desse mercado será usado. Para os Estados Unidos, o fuso horário do Leste dos EUA é usado. 

>[!NOTE]
> Diferente da seção [Agendamento](configure-precise-release-scheduling.md), as datas que você selecionar para a **Avaliação gratuita** não poderão ser personalizadas para mercados específicos. 



## <a name="sale-pricing"></a>Preço de venda

Se você quiser oferecer seu aplicativo por um preço reduzido por um período limitado de tempo, será possível criar e agendar uma promoção.

Para obter mais informações, consulte [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Licenciamento para organizações

Por padrão, seu aplicativo pode ser oferecido para que as organizações comprem por volume. Você pode indicar se e como seu aplicativo pode ser oferecido nesta seção.

Para obter mais informações, consulte [Opções de licenciamento para organizações](organizational-licensing.md).


## <a name="publish-date"></a>Data de publicação

Por padrão, seu envio iniciará o processo de publicação assim que for aprovado na certificação, a menos que você tenha configurado as datas na seção [**Agendamento**](#schedule) descrita acima. 

Para controlar quando o aplicativo deve ser publicado na Loja, use a seção **Agendamento**. Na maioria dos envios, você deve usar essa seção para agendar o lançamento do seu aplicativo e deixar a seção **Data de publicação** definida para a opção padrão, **Publicar meu aplicativo assim que ele for aprovado na certificação**. Isso impedirá que o aplicativo seja publicado antes das datas definidas na seção **Agendamento**. As datas que você selecionou na seção **Agendamento** determinarão quando seu aplicativo será disponibilizado para os clientes na Loja.

Se você ainda não quiser definir uma data de lançamento e preferir que o envio permaneça não publicado até você decidir manualmente iniciar o processo de publicação, escolha **Publicar este aplicativo manualmente**. Escolher essa opção significa que a seleção só será publicada depois que você confirmar. Depois que seu aplicativo for aprovado na certificação, publique-o selecionando **Publicar agora** na página de status da certificação ou selecionando uma data específica, conforme descrito a seguir.

Escolha **Não antes de \[data\]** para garantir que o envio não seja publicado até uma data específica. Com esta opção, seu envio será lançado assim que possível na data especificada ou depois dela A data deve ser pelo menos 24 horas no futuro. Com a data, você também pode especificar a hora em que o envio deve começar a ser publicado.
 
> [!NOTE]
> Atrasos durante a certificação ou publicação podem fazer com que a data real do lançamento seja posterior à data solicitada. A Microsoft Store não pode garantir que seu aplicativo (ou atualização) estará disponível em uma data específica.  

Você também pode mudar a data de lançamento após enviar o aplicativo, desde que ele ainda não tenha entrado na etapa Publicação. 

