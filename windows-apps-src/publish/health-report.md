---
author: jnHs
Description: "O relatório Integridade no painel Centro de Desenvolvimento do Windows permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta."
title: "Relatório de integridade"
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
translationtype: Human Translation
ms.sourcegitcommit: 2481343772a6cf8af0667dc0796ed455dba1eaee
ms.openlocfilehash: a92a2ab8f11c46217d1d7b63f86c5bf68757d3fb

---

# Relatório de integridade


O relatório **Integridade** no painel Centro de Desenvolvimento do Windows permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta. É possível exibir esses dados no painel (**Análise** > **Integridade**) ou [baixar o relatório](download-analytic-reports.md) a fim de exibi-lo offline. Onde for aplicável, você pode exibir rastreamentos de pilha para depuração adicional. Como alternativa, você pode recuperar esses dados de forma programática usando a [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).


> **Observação**  Caso você já tenha publicado aplicativos anteriormente e exibido dados de desempenho, talvez perceba um número maior de falhas e eventos relatados aqui. Isso ocorre porque podemos incluir mais dados neste relatório para oferecer a você uma visão mais completa.

## Aplicar filtros


Na parte superior da página, você pode expandir os **Aplicar filtros** para filtrar todos os dados dessa página por intervalo de datas, versão do pacote, tipo de dispositivo e/ou versão do sistema operacional.

-   **Data**: o filtro padrão é **Últimas 72 horas**, mas você pode expandi-lo até **Últimos 30 dias**.
-   **Versão do pacote**: a configuração padrão é **Todas as versões**. Se o seu aplicativo inclui mais de uma versão do pacote, você pode escolher uma específica.
-   **Tipo de dispositivo**: o padrão é **Todos os dispositivos**, mas é possível escolher um dispositivo específico (**Computador**, **Telefone**, **Console**, **IoT**, **Holográfico** ou **Desconhecido**).
-   **Versão do sistema operacional**: o padrão é **Todas as versões do sistema operacional**, mas é possível escolher uma versão do sistema operacional específica.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**. Por padrão, isso incluirá dados de todas as versões do pacote, a menos que você tenha usado **Aplicar filtros** para escolher apenas um.

## Total de falhas e eventos


O gráfico **Failure hits** (anteriormente conhecido como **Total de falhas e eventos**) mostra o número de falhas e eventos diários que os clientes observaram ao usar seu aplicativo durante o período de tempo selecionado. Cada tipo de evento que seu aplicativo experimentou é rastreado separadamente: falhas, travamentos, exceções de JavaScript ou falhas de memória.

Você pode filtrar os resultados por mercado e/ou por versão do sistema operacional.

## Mercados


O gráfico **Mercados** mostra o número total de falhas e eventos durante o período de tempo selecionado por mercado. Por padrão, mostramos o mercado que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico.

## Versão do pacote


O gráfico **Versão do pacote** mostra o número total de falhas e eventos durante o período de tempo selecionado por versão do pacote. Por padrão, mostramos a versão do pacote que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico.

## Falhas


O gráfico **Falhas** mostra o número total de falhas e eventos durante o período de tempo selecionado por nome da falha. Por padrão, mostramos a falha que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico. Para cada falha, também mostramos a porcentagem do número total de falhas.

Para exibir o relatório **Detalhes da falha** de uma falha específica, selecione o nome da falha. Se você tiver incluído arquivos de símbolo PDB, o relatório **Detalhes da falha** inclui o número de ocorrências de falha ao longo do mês passado, bem como um log de falha que lista os detalhes da ocorrência (data, versão do pacote, tipo de dispositivo, modelo do dispositivo, build do sistema operacional) e um link para o rastreamento de pilha.

 

 



<!--HONumber=Nov16_HO1-->


