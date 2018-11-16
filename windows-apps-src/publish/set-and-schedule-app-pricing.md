---
author: jnHs
Description: Select the base price for an app and schedule price changes. You can also customize these options for specific markets.
title: Definir e agendar o preço do aplicativo
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, preços, preços de aplicativos, preço do aplicativo, vender aplicativos, alteração de preço, preço personalizado, preço, custo, substituir preço base, preço livre, livre
ms.localizationpriority: medium
ms.openlocfilehash: 372abfdb0de5567b7c7d262b298d264b086fe339
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6988631"
---
# <a name="set-and-schedule-app-pricing"></a>Definir e agendar o preço do aplicativo

A seção **Preço** da página [Preço e disponibilidade](set-app-pricing-and-availability.md) permite que você selecione o preço base de um aplicativo. Você também pode [agendar alterações de preço](#schedule-price-changes) para indicar a data e a hora em que o preço do app deve ser alterado. Além disso, você tem a opção de [substituir o preço base para mercados específicos](#override-base-price-for-specific-markets), selecionando uma nova faixa de preço ou inserindo um preço livre na moeda local do mercado.

> [!NOTE]
> Embora este tópico refira-se a apps, a seleção de preço para envios de complementos usa o mesmo processo. Observe que para [complementos de assinatura](../monetize/enable-subscription-add-ons-for-your-app.md), o preço base que você selecionar não pode nunca ser aumentado (se alterando o preço base ou agendar uma alteração de preço), embora ele pode ser reduzido.

## <a name="base-price"></a>Preço de base

Ao selecionar o **Preço base** do seu app, esse preço será usado em todos os mercados em que o app for vendido, a menos que você substitua o preço base em algum mercado.

Você pode definir o **Preço base** como **Gratuito** ou pode escolher uma faixa de preço disponível, que define o preço em todos os países em que você optar por distribuir seu app. As faixas de preço começam em USD 0,99, com faixas adicionais disponíveis em incrementos crescentes (USD 1,09, USD 1,19 e assim por diante). Esses incrementos geralmente aumentam à medida que o preço também aumenta. 

> [!NOTE]
> Essas faixas de preço também se aplicam aos complementos. 

Cada faixa de preço tem um valor correspondente em cada uma das mais de 60 moedas oferecidas pela Loja. Usamos esses valores para ajudar você a vender seus aplicativos com um preço proporcional em todo o mundo. Você pode selecionar seu preço base em qualquer moeda, e usaremos automaticamente o valor correspondente para diferentes mercados. Observe que, às vezes, podemos ajustar o valor correspondente em um determinado mercado para compensar as alterações nas taxas de conversão de moeda.

Na seção **Preços**, clique em **exibir tabela de conversão** para ver os preços correspondentes em todas as moedas. Isso também exibe um número de ID associado a cada faixa de preço, que será necessário se você estiver usando a [API de envio da Microsoft Store](../monetize/manage-app-submissions.md#price-tiers) para inserir preços. Você pode clicar em **Baixar** para baixar uma cópia da tabela de faixa de preço como um arquivo .csv.

Lembre-se de que a faixa de preços selecionada pode incluir impostos sobre vendas ou valor agregado que seus clientes devem pagar. Para saber mais sobre as implicações fiscais do seu aplicativo nos mercados selecionados, consulte [Detalhes fiscais para aplicativos pagos](tax-details-for-paid-apps.md). Você também deve analisar as [considerações de preço para mercados específicos](define-pricing-and-market-selection.md#price-considerations-for-specific-markets).

> [!NOTE]
> Se você escolher a opção de **Parar a aquisição** em **disponibilizar este produto mas não detectável na loja** na seção [visibilidade](choose-visibility-options.md#discoverability) ), não será capaz de definir o preço para seu envio (já que ninguém será capaz de adquirir o aplicativo a menos que usarem um código promocional para acessar o aplicativo gratuitamente).

## <a name="schedule-price-changes"></a>Agendar alterações de preço

Opcionalmente, você pode agendar uma ou mais alterações de preço se quiser que o preço base do seu aplicativo seja alterado para uma data e hora específicas. 

> [!IMPORTANT]
> As alterações de preço são mostradas apenas para clientes em dispositivos Windows 10 (incluindo o Xbox). Se seu aplicativo publicado anteriormente é compatível com versões anteriores, as alterações de preço não serão aplicadas a esses clientes. Para clientes no Windows 8, o aplicativo sempre será oferecido em seu **Preço base** (e não em qualquer preço específico de mercado), mesmo se você agendar alterações de preço adicionais. Para clientes no Windows 8.1 e no Windows Phone 8.1 e versões anteriores, o aplicativo sempre será oferecido a primeira faixa de preço para o mercado do cliente.

Clique em **Agendar uma alteração de preço** para ver as opções de alteração de preço. Escolha a faixa de preço que você deseja usar (ou insira um preço livre para substituições de preço base para um único mercado) e selecione a data, a hora e o fuso horário.

Você pode clicar **Agendar uma alteração de preço** novamente para agendar quantas alterações quiser.

> [!NOTE]
> As alterações de preço agendadas funcionam de forma diferente do [preço de venda](put-apps-and-add-ons-on-sale.md). Ao colocar um app à venda, o preço é mostrado com um tachado na Store, e os clientes poderão comprar o app ao preço de venda durante o período selecionado. Após o período de venda, o preço de venda não será mais aplicável, e o app estará disponível ao seu preço base (ou um preço diferente que você especificou para esse mercado, se aplicável).
>
> Com uma alteração de preço agendada, você pode ajustar o preço para ser mais alto ou mais baixo. A alteração será realizada na data que você especificar, mas ela não será exibida como uma venda na Store e não terá nenhuma formatação especial aplicada. O app apenas terá um novo preço base. 


## <a name="override-base-price-for-specific-markets"></a>Substituir o preço base para mercados específicos

Por padrão, as opções selecionadas acima serão aplicadas a todos os mercados em que o aplicativo for oferecido. Você pode optar por alterar o preço de um ou mais mercados, escolhendo uma faixa de preço diferente ou inserindo um preço livre na moeda local do mercado.

> [!IMPORTANT]
> Se seu aplicativo publicado anteriormente dá suporte ao Windows 8, os clientes sempre verão o app ao seu **preço de Base**, mesmo se você selecionar um preço diferente para seu mercado.

Para alterar o preço para mercados específicos, clique em **Escolha os mercados para substituir o preço**. A janela pop-up **Seleção de mercado** será exibida, listando todos os mercados em que você optou por disponibilizar o app. (Se você excluir algum mercado na seção **Mercados**, ele não estará disponível.) 

Você pode substituir o preço base para um mercado de cada vez ou para um grupo de mercados juntos. Após fazer isso, você pode substituir o preço base para um mercado adicional (ou um grupo de mercados adicionais), selecionando **Escolha os mercados para substituir o preço** novamente e repetindo o processo descrito abaixo. Para remover a substituição de preço especificada para um mercado (ou grupo de mercados), clique em **Remover**.


### <a name="override-the-base-price-for-a-single-market"></a>Substituir o preço base para um único mercado

Para alterar o preço para apenas um mercado, selecione-o e clique em **Criar**. Depois, você verá as mesmas opções **Preço base** e **Agendar uma alteração de preço** como descrito acima, mas as seleções feitas serão específicas desse mercado. Como você está substituindo o preço base para apenas um mercado, as faixas de preço serão mostradas na moeda local desse mercado. Clique em **exibir tabela de conversão** para ver os preços correspondentes em todas as moedas. 

A substituição do preço base para um único mercado também oferece a opção de inserir um preço livre de sua preferência na moeda local do mercado. Você pode inserir o preço que quiser (dentro de uma faixa mínima e máxima), mesmo que não corresponda a uma das faixas de preço padrão. Esse preço será usado apenas para clientes no Windows 10 (incluindo o Xbox) no mercado selecionado. 

> [!IMPORTANT]
> Se você inserir um preço livre, esse preço não será ajustado (mesmo que as taxas de conversão mudem), a não ser que envie uma atualização com um novo preço. 

### <a name="override-the-base-price-for-a-market-group"></a>Substituir o preço base para um grupo de mercados

Para substituir o preço base para vários mercados, crie um *grupo de mercados*. Para fazer isso, selecione os mercados que você deseja incluir. Em seguida, você pode optar por inserir um nome para o grupo. (Esse nome é somente para sua referência e não ficará visível para nenhum cliente.) Quando terminar, clique em **Criar**. Depois, você verá as mesmas opções **Preço base** e **Agendar uma alteração de preço** como descrito acima, mas as seleções feitas serão específicas desse grupo de mercados. Observe que os preços livres não podem ser usados com grupos de mercados. Você precisará selecionar uma faixa de preço disponível.

Para alterar os mercados incluídos em um grupo de mercados, clique no nome do grupo de mercados, adicione ou remova os mercados que desejar e clique em **OK** para salvar as alterações. 

> [!NOTE]
> Um mercado não pode pertencer a vários grupos de mercados na seção **Preços**.





