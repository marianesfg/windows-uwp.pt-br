---
author: jnHs
Description: O relatório Aquisições de IAP do painel do Centro de Desenvolvimento do Windows permite que você veja quantos IAPs você vendeu, além das informações demográficas e dos detalhes da plataforma.
title: Relatório de aquisições de IAP
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
---

# Relatório de aquisições de IAP


O relatório **Aquisições de IAP** do painel do Centro de Desenvolvimento do Windows permite que você veja quantos IAPs você vendeu, além das informações demográficas e dos detalhes da plataforma. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline. Como alternativa, você pode recuperar esses dados de forma programática usando a [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Nesse relatório, uma aquisição de IAP significa que um cliente comprou um IAP de você. Várias compras do mesmo IAP consumível pelo mesmo cliente são contadas como aquisições de IAP separadas.

> **Importante**  O relatório **Aquisições de IAP** não inclui dados sobre reembolsos, reversões, estornos, etc. Para calcular a receita do seu aplicativo, visite [Resumo de pagamento](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.

## Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados dessa página por intervalo de datas e/ou por tipo de dispositivo. Você também pode filtrar para mostrar somente os dados para um IAP específico.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **IAP**: o filtro padrão é **Todos os IAPs**. Se desejar mostrar dados de aquisição para apenas um de seus IAPs, você pode escolher um específico aqui.
-   **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Se desejar mostrar dados para aquisições de IAP de um determinado tipo de dispositivo apenas, você pode escolher um específico aqui.

As informações dos gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**. Por padrão, isso inclui dados para todos os tipos de dispositivos, a menos que você tenha usado **Aplicar filtros** para escolher apenas um.

## Aquisições IAP


O gráfico **Aquisições IAP** mostra o número de aquisições diárias ou semanais de suas IAPs durante o período de tempo selecionado. (Quando você usa **Aplicar filtros** para filtrar os dados por uma duração maior, os dados serão agrupados por semana).

Você também pode ver o número de aquisições do tempo de vida de suas IAPs. Isso mostra o total cumulativo de todas as aquisições, desde quando o seu aplicativo foi publicado pela primeira vez.

O gráfico também mostra o preço que um cliente pagou para adquirir a IAP.

Você pode filtrar os resultados por mercado e/ou por versão do sistema operacional.

## Principais IAPs


O gráfico **Principais IAPs** mostra o número total de aquisições de cada um dos seus IAPs durante o período de tempo selecionado por mercado. Por padrão, mostramos o IAP que tinha mais aquisições na parte superior e continuamos a partir daí. Você pode reverter essa ordem alternando a seta na coluna **Aquisições** deste gráfico.

## Mercados


O gráfico **Mercados** mostra o número total de aquisições de IAP durante o período de tempo selecionado por mercado. Por padrão, mostramos o mercado que tinha mais aquisições na parte superior e continuamos a partir daí. Você pode reverter essa ordem alternando a seta na coluna **Aquisições** deste gráfico.

## Demografia do cliente


O gráfico **Customer demographic** mostra informações demográficas sobre as pessoas que adquiriram seu aplicativo. Você pode ver quantas aquisições (ao longo do período de tempo selecionado) foram feitas por pessoas em uma determinada faixa etária e por sexo.

> **Observação**  Alguns clientes optam por não compartilhar essas informações. Se não for possível determinar a faixa etária ou o sexo, a aquisição será categorizada como **Desconhecido**.

## Versão do sistema operacional


O catálogo em destaque **Versão do sistema operacional** mostra o número total de aquisições com base no sistema operacional do cliente (ou através do [volume de aquisições por organizações](organizational-licensing.md)). Em alguns casos, talvez você não consiga determinar essas informações. Neste caso, a versão do sistema operacional aparecerá como **Desconhecida**.

 

 


<!--HONumber=May16_HO2-->


