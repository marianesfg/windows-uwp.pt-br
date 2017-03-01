---
author: jnHs
Description: "O relatório de Aquisições de complemento do painel do Centro de Desenvolvimento do Windows permite que você veja quantos complementos você vendeu, além das informações demográficas e dos detalhes da plataforma."
title: "Relatório de aquisições de complemento"
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 189b15728fd5013a1f976803ffb0e55f57e95142
ms.lasthandoff: 02/07/2017

---

# <a name="add-on-acquisitions-report"></a>Relatório de aquisições de complemento


O relatório de **Aquisições de complemento** do painel do Centro de Desenvolvimento do Windows permite que você veja quantos complementos você vendeu, além das informações demográficas e dos detalhes da plataforma. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline. Como alternativa, você pode recuperar de forma programática esses dados usando o método [obter aquisições de complemento](../monetize/get-in-app-acquisitions.md) na [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Nesse relatório, uma aquisição de complemento significa que um cliente comprou um complemento de você. Várias compras do mesmo complemento consumível pelo mesmo cliente são contadas como aquisições de complementos separadas.

> **Importante**  O relatório de **aquisições de Complemento** não inclui dados sobre reembolsos, reversões, estornos etc. Para estimar a receita do seu app, visite [Resumo de pagamento](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.

## <a name="apply-filters"></a>Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados dessa página por intervalo de datas e/ou por tipo de dispositivo. Você também pode filtrar para mostrar somente os dados para um complemento específico.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Complemento**: o filtro padrão é **Todos os complementos**. Se desejar mostrar dados de aquisição para apenas um de seus complementos, você pode escolher um específico aqui.
-   **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Se desejar mostrar dados para aquisições de complementos de um determinado tipo de dispositivo apenas, você pode escolher um específico aqui.

As informações dos gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**. Por padrão, isso inclui dados para todos os tipos de dispositivos, a menos que você tenha usado **Aplicar filtros** para escolher apenas um.

## <a name="add-on-acquisitions"></a>Aquisições de complemento


O gráfico **Aquisições de Complemento** mostra o número de aquisições diárias ou semanais de seus complementos durante o período de tempo selecionado. (Quando você usa **Aplicar filtros** para filtrar os dados por uma duração maior, os dados serão agrupados por semana).

Você também pode ver o número de aquisições do tempo de vida dos seus complementos. Isso mostra o total cumulativo de todas as aquisições, desde quando o seu app foi publicado pela primeira vez.

O gráfico também mostra o preço que um cliente pagou para adquirir o complemento.

Você pode filtrar os resultados por mercado e/ou por versão do sistema operacional.

## <a name="top-add-ons"></a>Complementos principais

O gráfico **Complementos principais** mostra o número total de aquisições de cada um dos seus complementos durante o período de tempo selecionado por mercado. Por padrão, mostramos o complemento que tinha mais aquisições na parte superior e continuamos a partir daí. Você pode reverter essa ordem alternando a seta na coluna **Aquisições** deste gráfico.

## <a name="markets"></a>Mercados

O gráfico **Mercados** mostra o número total de aquisições de complementos durante o período de tempo selecionado por mercado. Por padrão, mostramos o mercado que tinha mais aquisições na parte superior e continuamos a partir daí. Você pode reverter essa ordem alternando a seta na coluna **Aquisições** deste gráfico.

## <a name="customer-demographic"></a>Demografia do cliente

O gráfico **Customer demographic** mostra informações demográficas sobre as pessoas que adquiriram seu complemento. Você pode ver quantas aquisições (ao longo do período de tempo selecionado) foram feitas por pessoas em uma determinada faixa etária e por sexo.

> **Observação**  Alguns clientes optam por não compartilhar essas informações. Se não for possível determinar a faixa etária ou o sexo, a aquisição será categorizada como **Desconhecido**.

## <a name="os-version"></a>Versão do sistema operacional

O catálogo em destaque **Versão do sistema operacional** mostra o número total de aquisições com base no sistema operacional do cliente (ou através do [volume de aquisições por organizações](organizational-licensing.md)). Em alguns casos, talvez você não consiga determinar essas informações. Neste caso, a versão do sistema operacional aparecerá como **Desconhecida**.

 

 

