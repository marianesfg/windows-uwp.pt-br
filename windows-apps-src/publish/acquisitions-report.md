---
Description: O relatório de aquisições no Partner Center permite que você veja quem tenha adquirido e instalaram o seu aplicativo, junto com dados demográficos e detalhes de plataforma.
title: Relatório de aquisições
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, aquisições, vendas de aplicativos, downloads de aplicativos, instalações, funil, aquisição, conversões, canal, exibições de página de aplicativos
ms.localizationpriority: medium
ms.openlocfilehash: af8fce64f96626e2842c3b99622af21bd286c2e4
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63770343"
---
# <a name="acquisitions-report"></a>Relatório de aquisições


O **aquisições** de relatórios no [Partner Center](https://partner.microsoft.com/dashboard) permite que você veja quem tenha adquirido e instalaram o seu aplicativo, junto com dados demográficos e detalhes de plataforma e mostra informações sobre como os clientes no Windows 10 (incluindo Xbox) chegou a listagem do seu aplicativo. Você também pode exibir dados em tempo real de aquisição de curto para o último período ou setenta e duas horas. 

Você pode exibir esses dados no Partner Center, ou [baixar o relatório](download-analytic-reports.md) exibir offline. Como alternativa, você pode recuperar esses dados de forma programática usando nossa [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

Neste relatório, uma **aquisição** significa que um novo cliente obteve uma licença para seu aplicativo (independentemente se você cobrou ou ofereceu gratuitamente). Uma **instalação** se refere ao aplicativo sendo instalado em um dispositivo Windows 10.

> [!IMPORTANT]
> O **aquisições** relatório não inclui dados sobre reembolsos, reversões, estornos, etc. Para estimar sua receita do aplicativo, visite [resumo de pagamento](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.
>
> Com exceção dos dados de modo de exibição de página (como descrito abaixo), esse relatório não inclui dados relacionados aos clientes que adquirem um aplicativo sem estar conectado a uma conta da Microsoft.


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar. Quase em tempo real, os dados serão mostrados para todas as opções (exceto nas **aplicativo cumulativo** dados). O **1h** e **H 72** períodos de tempo se aplicam somente ao **aplicativo diariamente** guia do **aquisições** gráfico e o  **Aquisições** guia do **mercados** gráfico. 

Você também pode expandir **Filtros** para filtrar os dados dessa página por mercado e/ou por tipo de dispositivo.

-   **Mercado**: O filtro padrão é **todos os mercados**, mas você pode limitar os dados a serem aquisições nos mercados de um ou mais.
-   **Tipo de dispositivo**: A configuração padrão é **todos os dispositivos**. Se você deseja mostrar dados de aquisições apenas de um determinado tipo de dispositivo (por exemplo, computador, console ou tablet), pode escolher um dispositivo específico aqui.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e todos os filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


## <a name="acquisitions"></a>Aquisições

O gráfico **Aquisições** mostra a quantidade de aquisições diárias ou semanais (um novo cliente que obtém uma licença do aplicativo) durante o período selecionado. (Quando você usa **Aplicar filtros** para exibir dados por uma duração maior, os dados de aquisição serão agrupados por semana). Somente aquisições feitas por clientes que são assinados com uma conta válida da Microsoft estão incluídas neste gráfico. 

Por padrão, vamos mostrar o **aplicativo diariamente** exibição, que inclui quase em tempo real de dados. Você também pode ver a quantidade de aquisições do tempo de vida do aplicativo ao selecionar **Cumulativo do aplicativo**. Isso mostra o total cumulativo de todas as aquisições, desde quando o seu aplicativo foi publicado pela primeira vez.

**Vendas brutas** para seu aplicativo (a partir de outubro de 2016 - presente) também estão disponíveis neste gráfico, que mostra a quantidade total acumulada de vendas de aplicativo (em USD). Observe que essa quantidade não leva em conta para quaisquer reembolsos, reversões, estorno etc.

Você pode filtrar os resultados se a aquisição foi originada do cliente ou da loja Web e/ou por versão do sistema operacional.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter aquisições de aplicativo](../monetize/get-app-acquisitions.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

No **aplicativo diariamente** exibição, quando o **1!d 30** período de tempo é selecionado, você poderá ver os marcadores de círculo. Eles representam um aumento significativo ou diminuir em um determinado valor, acreditamos que você vai querer saber sobre. A data em que o círculo aparece representa o fim da semana em que foi detectado um aumento significativo ou diminuição em comparação comparada a semana anterior que. Para ver mais detalhes sobre o que mudou, passe o mouse sobre o círculo.  

> [!TIP]
> Você pode exibir mais insights relacionados a alterações significativas nos últimos 30 dias na [relatório de Insights](insights-report.md).

## <a name="installs"></a>Instalações

O gráfico **Instalações** mostra quantas vezes detectamos que os clientes instalaram seu app com êxito em dispositivos Windows 10 (incluindo consoles Xbox One) durante o período selecionado. O total é mostrado, juntamente com um gráfico mostrando as instalações por dia ou semana (dependendo da duração selecionada). Você pode filtrar os resultados por uma versão específica do pacote.

O total de instalação inclui:
-   **Instala em vários dispositivos Windows 10.** Por exemplo, se o mesmo cliente instalar o app em dois computadores Windows 10 e um console Xbox One, isso será considerado como três instalações.
-   **Reinstala.** Por exemplo, se um cliente instalar o app hoje, desinstalar seu app amanhã e, em seguida, reinstalar o app próximo mês, isso contará como duas instalações.

O total de instalação não inclui ou reflete:
-   **Seja instalado nos dispositivos Windows 10.** Caso seu aplicativo ofereça suporte a versões anteriores do sistema operacional, como o Windows 8. x ou Windows Phone 8. x, nós não consideramos qualquer instalação nesses dispositivos.
-   **Desinstala.** Quando um cliente desinstala o aplicativo no dispositivo, nós não subtraímos da quantidade total de instalações.
-   **Atualizações.** Por exemplo, se o usuário instalar o aplicativo hoje e instalar uma atualização uma semana depois, isso será considerado como apenas uma instalação.
-   **Pré-instala.** Se um cliente comprar um dispositivo com seu aplicativo pré-instalado, nós não consideramos isso como uma instalação.
-   **Instalações iniciadas pelo sistema.** Se o Windows instalar o aplicativo automaticamente por algum motivo, nós não consideramos isso como uma instalação.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter instalações de aplicativo](../monetize/get-app-installs.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Funil de aquisição

O **funil aquisição** mostra quantos clientes concluíram cada etapa do funil, da visualização da página da Loja ao uso do aplicativo, juntamente com a taxa de conversão. Esses dados podem ajudar você a identificar áreas em que convém investir mais para aumentar as aquisições, as instalações ou o uso.

> [!IMPORTANT]
> O **Funil de aquisição** mostra os dados somente sobre cliente com o Windows 10 (incluindo o Xbox) dos últimos 90 dias.

As etapas no funil são:

- **Exibições de página**: Esse número representa o total de exibições de lista de Store do seu aplicativo, inclusive pessoas que não são assinadas com uma conta da Microsoft. Isso não inclui dados dos clientes que optaram por não fornecer essas informações para a Microsoft.
- **Aquisições**: O número de novos clientes que obtido uma licença para seu aplicativo (quando conectado com sua conta da Microsoft) dentro de 48 horas de exibição de sua listagem de Store.
- **Instala**: O número de clientes que instalaram o aplicativo depois de adquirir a ele.
- **uso**: O número de clientes que usaram o aplicativo depois da instalação.

Como alternativa, você pode filtrar os resultados por gênero e/ou faixa etária, bem como por ID de campanha personalizada.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter dados de funil de aquisições](../monetize/get-acquisition-funnel-data.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Mercados

O gráfico **Mercados** mostra a quantidade total de aquisições ou instalações durante o período de tempo selecionado para cada mercado em que o aplicativo está disponível. Você pode optar por exibir dados de **Aquisições** ou **Instalações**.

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de aquisições ou instalações. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="customer-demographic"></a>Demografia do cliente

O gráfico **Customer demographic** mostra informações demográficas sobre as pessoas que adquiriram seu aplicativo. Você pode ver quantas aquisições (ao longo do período de tempo selecionado) foram feitas por pessoas em uma determinada faixa etária e por sexo.

> [!NOTE]
> Alguns clientes optaram por não para compartilhar essa informação. Se não for possível determinar a faixa etária ou o sexo, a aquisição será categorizada como **Desconhecido**.

 

## <a name="app-page-views-and-conversions-by-channel"></a>Exibições e conversões de página de aplicativo por canal

O **exibições de página de aplicativo e conversões pelo canal** gráfico permite que você veja como os clientes no Windows 10 chegaram a listagem do seu aplicativo ao longo do período de tempo selecionado.

Nesse gráfico, um *canal* se refere ao método em que um cliente chegou à página de listagem do aplicativo (por exemplo, navegação e pesquisa na Loja, um link de um site externo, um link de uma das suas campanhas personalizadas etc.). Os seguintes tipos de canal estão incluídos:

-   **Tráfego de Store:** O cliente foi navegação ou pesquisa dentro do Store quando eles exibiram listagem do seu aplicativo.
-   **Campanha personalizada:** O cliente seguiu um link que é usado um [ID da campanha personalizado](create-a-custom-app-promotion-campaign.md).
-   **Outros:** O cliente seguido um link externo (sem ID campanha personalizada) de um site da Web a listagem do seu aplicativo ou o cliente seguiu um link de um mecanismo de pesquisa para a listagem do seu aplicativo.

Um *exibição de página* significa que um cliente exibido a página de listagem de Store do seu aplicativo, por meio de Store baseado na web ou de dentro do aplicativo da Store no Windows 10. Isso inclui modos de exibição por pessoas que não estão conectadas com uma conta da Microsoft. Alguns clientes optaram por não fornecer essas informações para a Microsoft.

Uma *conversão* significa que um cliente (conectado com uma conta da Microsoft) obteve recentemente uma licença para seu aplicativo (seja paga ou gratuita).

Os números de visualização de página e conversão não são contagens de clientes exclusivos. Para obter as informações da taxa de conversão, consulte o gráfico do [Funil de aquisição](#acquisition-funnel).

> [!NOTE]
> Os clientes podem chegar à listagem do aplicativo ao clicar em uma campanha personalizada que não foi criada por você. Marcamos cada modo de exibição de página em uma sessão com a ID da campanha a partir da qual o cliente entrou na Loja pela primeira vez. Em seguida, nós atribuímos conversões a essa ID de campanha para todas as aquisições nas últimas 24 horas. Por isso, você pode observar um número maior de conversões totais do que o total de conversões para suas IDs de campanha, e você pode ter conversões ou conversões de complemento com nenhuma exibição de página.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter conversões de aplicativo por canal](../monetize/get-app-conversions-by-channel.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Exibições e conversões de página de aplicativo por ID de campanha

O gráfico **Exibições de página e conversões de aplicativo por ID de campanha** permite que você acompanhe as conversões e as exibições de página, como descrito acima, para cada uma das suas [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md). As principais IDs de campanha são mostradas e você pode usar os filtros para excluir ou incluir IDs de campanha específicas.

## <a name="total-campaign-conversions"></a>Total de conversões da campanha

O gráfico **Total de conversões da campanha** mostra o total de conversões de aplicativo e complemento de todas as campanhas personalizadas durante o período selecionado.





 

 
