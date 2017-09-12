---
author: jnHs
Description: "O relatório de aquisições no painel do Centro de Desenvolvimento do Windows permite que você veja quem adquiriu e instalou seu aplicativo, juntamente com detalhes demográficos e de plataforma."
title: "Relatório de aquisições"
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.author: wdg-dev-content
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d6540db7a3d0a045fa6a2c8fec617f045f4a1bf4
ms.sourcegitcommit: 2b436dc5e5681b8884e0531ee303f851a3e3ccf2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2017
---
# <a name="acquisitions-report"></a>Relatório de aquisições


O relatório de **Aquisições** no painel do Centro de Desenvolvimento do Windows permite que você veja quem adquiriu e instalou seu aplicativo, juntamente com detalhes demográficos e de plataforma. Ele também permite que você obtenha informações sobre como os clientes no Windows 10 chegaram à listagem do aplicativo.

Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibição offline. Como alternativa, você pode recuperar esses dados de forma programática usando nossa [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

Neste relatório, uma **aquisição** significa que um novo cliente obteve uma licença para seu aplicativo (independentemente se você cobrou ou ofereceu gratuitamente). Uma **instalação** se refere ao aplicativo sendo instalado em um dispositivo Windows 10.

> [!IMPORTANT]
> O relatório **Aquisições** não inclui dados sobre reembolsos, reversões, estornos etc. Para estimar a receita do aplicativo, visite [Resumo de pagamentos](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.
>
> Com exceção dos dados de modo de exibição de página (como descrito abaixo), esse relatório não inclui dados relacionados aos clientes que adquirem um aplicativo sem estar conectado a uma conta da Microsoft.


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

Você também pode expandir **Filtros** para filtrar os dados dessa página por mercado e/ou por tipo de dispositivo.

-   **Mercado**: o filtro padrão é **Todos os mercados**, mas você pode limitar os dados a aquisições em um ou mais mercados.
-   **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Se desejar mostrar dados para aquisições de um determinado tipo de dispositivo apenas, você pode escolher um específico aqui.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e quaisquer filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


## <a name="acquisitions"></a>Aquisições

O gráfico **Aquisições** mostra a quantidade de aquisições diárias ou semanais (um novo cliente que obtém uma licença do aplicativo) durante o período selecionado. (Quando você usa **Aplicar filtros** para mostrar dados de um período maior, os dados de aquisição serão agrupados por semana.) Somente as aquisições efetuadas por clientes conectados com uma conta da Microsoft válida são incluídas neste gráfico.

Você também pode ver a quantidade de aquisições do tempo de vida do aplicativo ao selecionar **Cumulativo do aplicativo**. Isso mostra o total cumulativo de todas as aquisições, desde quando o seu aplicativo foi publicado pela primeira vez.

Você pode filtrar os resultados se a aquisição foi originada do cliente ou da loja Web e/ou por versão do sistema operacional.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter aquisições de aplicativo](../monetize/get-app-acquisitions.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="installs"></a>Instalações

O gráfico de **Instalações** mostra quantas vezes detectamos que os clientes instalaram com sucesso seu aplicativo em dispositivos Windows 10 durante o período selecionado. A quantidade total é mostrada, juntamente com um gráfico mostrando as instalações por dia ou semana (dependendo da duração selecionada). Você pode filtrar os resultados por uma versão específica do pacote.

O total de instalação inclui:
-   **Instalações em vários dispositivos Windows 10.** Por exemplo, se o mesmo cliente instalar o aplicativo em dois computadores com Windows 10 e um telefone Windows 10, isso será considerado como três instalações.
-   **Reinstalações.** Por exemplo, se um cliente instalar o app hoje, desinstalar seu app amanhã e, em seguida, reinstalar o app próximo mês, isso contará como duas instalações.

O total de instalação não inclui ou reflete:
-   **Instalações em dispositivos não Windows 10.** Caso seu aplicativo ofereça suporte a versões anteriores do sistema operacional, como o Windows 8. x ou Windows Phone 8. x, nós não consideramos qualquer instalação nesses dispositivos.
-   **Desinstalações.** Quando um cliente desinstala o aplicativo no dispositivo, nós não subtraímos da quantidade total de instalações.
-   **Atualizações.** Por exemplo, se o usuário instalar o aplicativo hoje e instalar uma atualização uma semana depois, isso será considerado como apenas uma instalação.
-   **Pré-instalações.** Se um cliente comprar um dispositivo com seu aplicativo pré-instalado, nós não consideramos isso como uma instalação.
-   **Instalações iniciadas pelo sistema.** Se o Windows instalar o aplicativo automaticamente por algum motivo, nós não consideramos isso como uma instalação.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter instalações de aplicativo](../monetize/get-app-installs.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Funil de aquisição

O **funil aquisição** mostra quantos clientes concluíram cada etapa do funil, da visualização da página da Loja ao uso do aplicativo, juntamente com a taxa de conversão. Esses dados podem ajudar você a identificar áreas em que convém investir mais para aumentar as aquisições, as instalações ou o uso.

> [!IMPORTANT]
> O **funil de aquisição** mostra dados somente para os cliente no Windows 10 nos últimos 90 dias.

As etapas no funil são:

- **Modos de exibição de página**: esse número representa o total dos modos de exibição da listagem da Loja do seu aplicativo, incluindo pessoas que não estão conectadas em uma conta da Microsoft. Isso não inclui dados dos clientes que optaram por não fornecer essas informações para a Microsoft.
- **Aquisições**: a quantidade de novos clientes que obtiveram uma licença para seu aplicativo (quando conectados com uma conta da Microsoft) dentro de 48 horas a partir da visualização da listagem da loja.
- **Instalações**: a quantidade de clientes que instalaram o aplicativo depois de adquiri-lo.
- **Uso**: a quantidade de clientes que usaram o aplicativo depois de instalá-lo.

Como alternativa, você pode filtrar os resultados por gênero e/ou faixa etária, bem como por ID de campanha personalizada.

> [!NOTE]
> Você também pode recuperar de forma programática esses dados usando o método [obter dados de funil de aquisições](../monetize/get-acquisition-funnel-data.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Mercados

O gráfico **Mercados** mostra a quantidade total de aquisições ou instalações durante o período de tempo selecionado para cada mercado em que o aplicativo está disponível. Você pode optar por exibir dados de **Aquisições** ou **Instalações**.

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de aquisições ou instalações. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="customer-demographic"></a>Demografia do cliente

O gráfico **Customer demographic** mostra informações demográficas sobre as pessoas que adquiriram seu aplicativo. Você pode ver quantas aquisições (ao longo do período de tempo selecionado) foram feitas por pessoas em uma determinada faixa etária e por sexo.

> [!NOTE]
> Alguns clientes optaram por não para compartilhar essa informação. Se não for possível determinar a faixa etária ou o sexo, a aquisição será categorizada como **Desconhecida**.

 

## <a name="app-page-views-and-conversions-by-channel"></a>Exibições e conversões de página de aplicativo por canal

O gráfico de **Visualizações de página e conversões por canal do aplicativo** permite saber como os clientes no Windows 10 chegaram à listagem do aplicativo em um período específico.

Nesse gráfico, um *canal* se refere ao método em que um cliente chegou à página de listagem do aplicativo (por exemplo, navegação e pesquisa na Loja, um link de um site externo, um link de uma das suas campanhas personalizadas etc.). Os seguintes tipos de canal estão incluídos:

-   **Tráfego da Loja:** o cliente estava navegando ou pesquisando a Loja quando visualizou os detalhes do seu app.
-   **Campanha personalizada:** o cliente seguiu um link que usava uma [ID da campanha personalizada](create-a-custom-app-promotion-campaign.md).
-   **Outros:** o cliente seguiu um link externo (sem qualquer ID de campanha personalizada) de um site para a listagem do seu app ou o cliente seguiu um link de um mecanismo de pesquisa para a listagem do seu app.

Uma *exibição de página* significa que um cliente viu a página de listagem da Loja do seu aplicativo, por meio da Loja baseada na Web ou dentro do aplicativo Loja no Windows 10. Isso inclui modos de exibição por pessoas que não estão conectadas com uma conta da Microsoft. Alguns clientes optaram por não fornecer essas informações para a Microsoft.

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





 

 
