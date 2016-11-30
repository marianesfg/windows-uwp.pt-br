---
author: jnHs
Description: "O relatório Classificações no painel do Centro de Desenvolvimento do Windows permite que você consulte a distribuição de como os clientes classificam seu aplicativo na Windows Store."
title: "Relatório de classificações"
ms.assetid: CAFEC20B-04FB-48C8-B663-1238C0B85ECD
translationtype: Human Translation
ms.sourcegitcommit: 7b73682ea36574f8b675193a174d6e4b4ef85841
ms.openlocfilehash: 45d22b46a750655cc723658b476ba40d18b4f745

---

# Relatório de classificações


O relatório **Classificações** no painel do Centro de Desenvolvimento do Windows permite que você consulte a distribuição de como os clientes classificam seu aplicativo na Windows Store. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline. Você também pode recuperar programaticamente esses dados usando o método [obter classificações de aplicativo](../monetize/get-app-ratings.md) na [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Nesse relatório, uma classificação significa o número de estrelas (de 1 a 5) que um cliente deu ao seu aplicativo quando o classificou na Loja. O relatório **Classificações** não inclui informações sobre todos os comentários individuais deixados como análises. Esses comentários estão disponíveis no [Relatório de avaliações](reviews-report.md).

## Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados nessa página por intervalo de datas e/ou por mercado.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Mercado**: o filtro padrão é **Todos os mercados**. Você pode escolher um mercado específico, se quiser que esta página mostre somente as classificações de clientes desse mercado.
-   **Tipo do dispositivo**: o filtro padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente classificações deixadas por clientes que estão usando esse dispositivo.

As informações em todos os gráficos listados a seguir refletirão o período de tempo selecionado na seção **Aplicar filtros** e refletirá todos os outros filtros que você tenha escolhido aqui.

## Classificação média


O **Classificação média** mostra a classificação média do seu aplicativo durante o período de tempo selecionado.

## Número de classificações


O gráfico **Número de classificações** mostra o número total de classificações do seu aplicativo durante o período de tempo selecionado.

## Classificações novas e revisadas


O gráfico **Classificações novas e revisadas** mostra o número de classificações de cada tipo de classificação (nova ou revisada) durante o período de tempo selecionado.

-   **Classificações de novas** são classificações que os clientes enviaram, mas que não foram alteradas.
-   **Classificações revisadas** são classificações que foram alteradas pelo cliente.

>**Observação**  Uma classificação aparecerá aqui como revisada mesmo se o cliente tiver apenas alterado ou adicionado o texto ou o título de sua análise e deixado a classificação em si inalterada.

## Classificação média ao longo do tempo


O gráfico **Classificação média ao longo do tempo** mostra como a classificação média do aplicativo foi alterada durante o período de tempo selecionado.

Em vez de calcular a média de todas as classificações feitas durante o período de tempo selecionado (como no gráfico **Classificação média**), o gráfico **Classificação média ao longo do tempo** mostra como os clientes classificaram o aplicativo em um determinado dia ou semana durante o período. Isso ajuda a identificar tendências ou determinar se classificações foram afetadas por atualizações ou outros fatores.

Se você filtrou as informações por **Últimos 30 dias** ou **Últimos três meses**, o gráfico exibe a classificação média por dia. Se você filtrou por **Últimos 6 meses** ou **Últimos 12 meses**, o gráfico exibe a classificação média por semana (com uma nova semana considerada para iniciar na segunda-feira; a média de classificação mostrada é da semana anterior).

## Mercados


O gráfico **Mercados** mostra a classificação média e o número de classificações durante o período de tempo selecionado por mercado.

> **Observação**  Se tiver usado os **Filtros de página** para especificar um mercado específico, você não verá este gráfico no relatório **Classificações**. Para ver este gráfico, altere os **Filtros de página** para mostrar todos os mercados.

Por padrão, podemos mostrar o mercado que tinha mais críticas e continuar para baixo, mas você pode reverter essa ordem, alternando a seta na coluna **Número de classificações** deste gráfico. Você também pode classificar os dados por **Média de classificação** ou **Mercado**, clicando nessas colunas.

> **Observação** É provável que você consulte um número diferente de classificações ao comparar o relatório **Classificações** no Centro de Desenvolvimento do Windows com o relatório Críticas no aplicativo móvel mais antigo do Centro de Desenvolvimento. Isso ocorre porque o aplicativo mostra apenas os dados de avaliações de clientes no Windows Phone 8.1 e versões anteriores. Isso também pode ser resultado do trabalho da Microsoft de remover avaliações da Windows Store que foram identificadas como spam, inadequadas, ofensivas ou que violam a política de outra forma. Esperamos que essa ação resulte em uma melhor experiência para o cliente.

 

 



<!--HONumber=Nov16_HO1-->


