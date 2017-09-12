---
author: jnHs
Description: "Selecione o preço base de um aplicativo e agende alterações de preço. Você também pode personalizar essas opções para mercados específicos."
title: "Definir e agendar o preço do aplicativo"
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 65d2fa64e4d442da42b8c546693c61eda817aa18
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2017
---
# <a name="set-and-schedule-app-pricing"></a>Definir e agendar o preço do aplicativo

A seção **Preço** da página [Preço e disponibilidade](set-app-pricing-and-availability.md) permite que você selecione o preço base de um aplicativo. Você também pode [agendar alterações de preço](#schedule-price-changes) para indicar a data e a hora em que o preço do aplicativo deve ser alterado. Além disso, você tem a opção de [personalizar essas alterações para mercados específicos](#customize-pricing-for-specific-markets). 

> [!NOTE]
> Embora este tópico se refira aos aplicativos, a seleção de preço para envios de complemento usa o mesmo processo.

## <a name="base-price"></a>Preço de base

Quando você selecionar o **Preço base** do seu aplicativo, esse preço será usado em todos os mercados em que o aplicativo é vendido, a menos que você especifique um preço personalizado para um mercado específico.

Você pode definir o **Preço base** para **Gratuito** ou escolher uma faixa de preços disponível, que defina o preço de venda em todos os países em que você opte por distribuir seu aplicativo. As faixas de preço começam em USD 0,99, com níveis adicionais disponíveis em incrementos crescentes (USD 1,09, USD 1,19 e assim por diante). Esses incrementos geralmente aumentam à medida que o preço também aumenta. 

> [!NOTE]
> Essas faixas de preço também se aplicam aos complementos. 

Cada faixa de preço tem um valor correspondente em cada uma das mais de 60 moedas oferecidas pela Loja. Usamos esses valores para ajudar você a vender seus aplicativos com um preço proporcional em todo o mundo. Você pode selecionar seu preço base em qualquer moeda, e nós usaremos automaticamente o valor correspondente para diferentes mercados.

Na seção **Preços**, clique em **exibir tabela** para ver os preços correspondentes em todas as moedas. Isso também exibe um número de ID associado a cada faixa de preços, que será necessário se você estiver usando a [API de envio da Windows Store](../monetize/manage-app-submissions.md#price-tiers) para inserir preços. Você pode clicar em **Download** para baixar uma cópia da tabela de faixa de preços como um arquivo. csv.

Lembre-se de que a faixa de preços selecionada pode incluir impostos sobre vendas ou valor agregado que seus clientes devem pagar. Para saber mais sobre as implicações fiscais do seu aplicativo nos mercados selecionados, consulte [Detalhes fiscais para aplicativos pagos](tax-details-for-paid-apps.md). Você também deve analisar as [considerações de preço para mercados específicos](define-pricing-and-market-selection.md#price-considerations-for-specific-markets).

## <a name="schedule-price-changes"></a>Agendar alterações de preço

Opcionalmente, você pode agendar uma ou mais alterações de preço se quiser que o preço base do seu aplicativo seja alterado para uma data e hora específicas. 

> [!IMPORTANT]
> As alterações de preço são mostradas apenas para clientes no Windows 10. Se seu aplicativo oferecer suporte a versões anteriores do sistema operacional, as alterações de preço não serão aplicadas. 
>
> - Para clientes no Windows 8, o aplicativo sempre será oferecido em seu **Preço base** (e não em qualquer preço específico de mercado), mesmo se você agendar alterações de preço adicionais. 
> - Para clientes no Windows 8.1 e no Windows Phone 8.1 e versões anteriores, o aplicativo sempre será oferecido a um preço inicial para o mercado do cliente, mesmo se você agendar alterações de preço adicionais nesse mercado.
> 
> Tenha isso em mente ao agendar alterações de preço. Por exemplo, se você inicialmente lançar seu aplicativo a um preço mais baixo e, depois, agendar uma data em que o preço aumentará, os clientes nas versões anteriores do sistema operacional que compraram o aplicativo pagarão um preço menor (original).

Clique em **Agendar uma alteração de preço** para ver as opções de alteração de preço. Escolha a faixa de preços que você deseja usar e selecione a data, a hora e o fuso horário.

Você pode clicar em **Schedule a price change again** para agendar quantas alterações desejar.

> [!NOTE]
> As alterações de preço agendadas funcionam de forma diferente no [Preço de venda](put-apps-and-add-ons-on-sale.md). Quando você coloca um aplicativo à venda, os clientes que estão visualizando a listagem da Loja verão que o preço foi reduzido e poderão comprar o aplicativo a um preço menor durante o período selecionado por você. Após o período de venda, ele retornará ao preço base.
>
> Com uma alteração de preço agendada, você pode ajustar o preço para ser maior ou menor. A alteração será realizada na data em que você especificar, mas ela não será exibida como uma venda na Loja; o aplicativo terá apenas um novo preço base. 

## <a name="customize-pricing-for-specific-markets"></a>Personalizar o preço para mercados específicos

Por padrão, as opções que você selecionar acima serão aplicadas a todos os mercados em que o aplicativo for oferecido. Para personalizar o preço para mercados específicos, selecione **Personalizar para mercados específicos**. A janela pop-up **Seleção de mercado** será exibida, listando todos os mercados em que você optou disponibilizar o aplicativo. Se você excluiu algum mercado na seção Mercados, esses mercados não serão mostrados aqui. 

> [!IMPORTANT]
> Os clientes no Windows 8 sempre verão o aplicativo em seu **Preço base**, mesmo se você selecionar um preço diferente para seu mercado.

Para adicionar a personalização de preços para um mercado, selecione-a e clique em **Salvar**. Depois, você verá as mesmas opções **Preço base** e **Agendar uma alteração de preço** como descrito acima, mas as seleções feitas serão específicas desse mercado.

Para adicionar a personalização de preço para vários mercados, crie um *grupo de mercados*. Para fazer isso, selecione os mercados que você deseja incluir e insira um nome para o grupo. (Esse nome é somente para referência e não ficará visível para todos os clientes.) Quando terminar, clique em **Salvar**. Depois, você verá as mesmas opções **Preço base** e **Agendar uma alteração de preço** como descrito acima, mas as seleções feitas serão específicas desse grupo de mercados.

> [!NOTE]
> Um mercado não pode pertencer a mais de um dos grupos de mercados usados na seção Preços.

Para adicionar uma personalização de preços para um mercado adicional ou um grupo de mercados adicional, Selecione **Personalizar para mercados específicos** e repita essas etapas. Para alterar os mercados incluídos em um grupo de mercados, selecione seu nome. Para remover os preços de um grupo de mercados (ou um mercado individual), clique em **Remover**.



