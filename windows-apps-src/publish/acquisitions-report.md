---
author: jnHs
Description: "O relatório Aquisições no painel do Centro de Desenvolvimento do Windows permite que você veja quem adquiriu o seu app, juntamente com detalhes demográficos e de plataforma."
title: "Relatório de aquisições"
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a668c3d03c11ac4c6c27cddeefafeb3c42caf1e3
ms.lasthandoff: 02/07/2017

---

# <a name="acquisitions-report"></a>Relatório de aquisições


O relatório **Aquisições** no painel do Centro de Desenvolvimento do Windows permite que você veja quem adquiriu o seu app, juntamente com detalhes demográficos e de plataforma. Você pode visualizar esses dados em seu painel ou [baixar o relatório](download-analytic-reports.md) para vê-lo offline. Como alternativa, você pode recuperar de forma programática esses dados usando o método [obter aquisições de app](../monetize/get-app-acquisitions.md) na [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Neste relatório, uma aquisição significa que um novo cliente obteve uma licença para o seu app (tendo você cobrado dinheiro ou oferecido gratuitamente).

> **Importante**  O Relatório de **Aquisições** não inclui dados sobre reembolsos, reversões, estornos, etc. Para estimar a receita do seu app, visite [Resumo de pagamento](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.



## <a name="apply-filters"></a>Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados dessa página por intervalo de datas e/ou por tipo de dispositivo.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Se desejar mostrar dados para aquisições de um determinado tipo de dispositivo apenas, você pode escolher um específico aqui.

As informações dos gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**. Por padrão, isso inclui dados para todos os tipos de dispositivos, a menos que você tenha usado **Aplicar filtros** para escolher apenas um.

## <a name="acquisitions"></a>Aquisições


O gráfico **Aquisições** mostra o número de aquisições diárias ou semanais de seu app durante o período de tempo selecionado. (Quando você usa **Aplicar filtros** para filtrar os dados por uma duração maior, os dados serão agrupados por semana).

Você também pode ver o número de aquisições do tempo de vida do seu app. Isso mostra o total cumulativo de todas as aquisições, desde quando o seu app foi publicado pela primeira vez.

Você pode filtrar os resultados por mercado e/ou por versão do sistema operacional.

## <a name="customer-demographic"></a>Demografia do cliente


O gráfico **Customer demographic** mostra informações demográficas sobre as pessoas que adquiriram seu app. Você pode ver quantas aquisições (ao longo do período de tempo selecionado) foram feitas por pessoas em uma determinada faixa etária e por sexo.

> **Observação**  Alguns clientes optam por não compartilhar essas informações. Se não for possível determinar a faixa etária ou sexo, a aquisição será categorizada como **Desconhecido**.

 

## <a name="markets"></a>Mercados


O gráfico **Mercados** mostra o número total de aquisições durante o período de tempo selecionado por mercado. Por padrão, mostramos o mercado que tinha mais aquisições na parte superior e continuamos a partir daí. Você pode reverter essa ordem alternando a seta na coluna **Aquisições** deste gráfico.

## <a name="os-version"></a>Versão do sistema operacional


O catálogo em destaque **OS version** mostra o número total de aquisições com base no sistema operacional do cliente (ou através do [volume de aquisições por organizações](organizational-licensing.md)). Em alguns casos, talvez você não consiga determinar essas informações. Neste caso, a versão do sistema operacional aparecerá como **Desconhecida**.



 

 

