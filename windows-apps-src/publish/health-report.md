---
Description: O relatório Integridade no painel Centro de Desenvolvimento do Windows permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta.
title: Relatório de integridade
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
---

# Relatório de integridade


O relatório **Integridade** no painel Centro de Desenvolvimento do Windows permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline. Onde for aplicável, você pode exibir rastreamentos de pilha para depuração adicional. Como alternativa, você pode recuperar esses dados de forma programática usando a [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

> **Observação**  Caso você já tenha publicado aplicativos anteriormente e exibido dados de desempenho nos painéis anteriores, talvez perceba um número maior de falhas e eventos relatados aqui. Isso ocorre porque podemos incluir mais dados neste relatório para oferecer a você uma visão mais completa.

## Aplicar filtros


Na parte superior da página, você pode expandir os **Aplicar filtros** para filtrar todos os dados dessa página por intervalo de datas e/ou por versão de pacote.

-   **Data**: o filtro padrão é **Últimas 72 horas**, mas você pode expandi-lo até **Últimos 6 meses**.
-   **Versão do pacote**: a configuração padrão é **Todas as versões**. Se o seu aplicativo inclui mais de uma versão do pacote, você pode escolher uma específica.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado na seção **Aplicar filtros**. Por padrão, isso incluirá dados de todas as versões do pacote, a menos que você tenha usado **Aplicar filtros** para escolher apenas um.

## Total de falhas e eventos


O gráfico **Total de falhas e eventos** mostra o número de falhas e eventos diários que os clientes observaram ao usar seu aplicativo durante o período de tempo selecionado. Cada tipo de evento que seu aplicativo experimentou é rastreado separadamente: falhas, travamentos, exceções de JavaScript ou falhas de memória.

Você pode filtrar os resultados por mercado e/ou por versão do sistema operacional.

## Detalhamento de falhas e eventos


O gráfico **Detalhamento de falhas e eventos** permite que você veja gráficos que acompanham detalhes específicos relacionados às configurações dos clientes quando um evento sem resposta ou a falha ocorreu. Clique nos títulos de seção para ver detalhes sobre:

-   Versão do sistema operacional
-   Tipo de dispositivo
-   Memória (MB)
-   Armazenamento em massa (GB)
-   Velocidade da CPU (GHz)

Opcionalmente, você pode usar o **Filtro de falha**: para filtrar os resultados por tipo de erro: falhas, travamentos, exceções de JavaScript ou falhas de memória. (A configuração padrão é mostrar todos os tipos de falha).

## Mercado


O gráfico **Mercado** mostra o número total de falhas e eventos durante o período de tempo selecionado por mercado. Por padrão, mostramos o mercado que tinha mais aquisições na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Falhas** deste gráfico.

## Log de falha


Se você tiver incluído arquivos de símbolos PDB, o gráfico **Log de falha** mostrará detalhes relacionados a ocorrências de símbolos específicos, incluindo o número total de falhas e média diária de falhas por símbolo.

Essas informações se baseiam em uma porcentagem do total de eventos. Na parte superior do gráfico, indicaremos de que percentual de eventos foi feita a amostragem para fornecer esses dados.

 

 


<!--HONumber=Mar16_HO1-->


