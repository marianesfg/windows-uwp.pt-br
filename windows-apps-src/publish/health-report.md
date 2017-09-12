---
author: jnHs
Description: "O relatório Integridade no painel Centro de Desenvolvimento do Windows permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta."
title: "Relatório de integridade"
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.author: wdg-dev-content
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 67f87405f7738dec591c71678ecfa5122e673502
ms.sourcegitcommit: ae93435e1f9c010a054f55ed7d6bd2f268223957
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2017
---
# <a name="health-report"></a>Relatório de integridade


O relatório **Integridade** no painel Centro de Desenvolvimento do Windows permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para visualizar offline. Onde for aplicável, você pode exibir rastreamentos de pilha e/ou arquivos CAB para depuração adicional.

Como alternativa, você pode recuperar esses dados no relatório ao usar a [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **72H** (72 horas), mas você pode escolher **30D** em vez disso, para mostrar os dados nos últimos 30 dias.

Também é possível expandir **Filtros** para filtrar os dados dessa página por versão do pacote, mercado e/ou por tipo de dispositivo.

-   **Versão do pacote**: a configuração padrão é **Tudo**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Mercado**: o filtro padrão é **Todos os mercados**, mas você pode limitar os dados a aquisições em um ou mais mercados.
-   **Tipo de dispositivo**: A configuração padrão é **Tudo**, mas é possível optar por mostrar dados apenas de um tipo de dispositivo específico.
-   **Versão do sistema operacional**: o padrão é **Todas as versões do sistema operacional**, mas é possível escolher uma versão do sistema operacional específica.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e quaisquer filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


## <a name="failure-hits"></a>Ocorrências de falha

O gráfico **Ocorrências de falhas** mostra a quantidade de falhas e eventos diários que os clientes observaram ao usar o aplicativo durante o período de tempo selecionado. Cada tipo de evento que o aplicativo experimentou é rastreado separadamente: falhas, travamentos, exceções de JavaScript e falhas de memória.


## <a name="failure-hits-by-market"></a>Ocorrências de falha por mercado

O gráfico **Ocorrências de falha por mercado** mostra o total de falhas e eventos durante o período de tempo selecionado por mercado.

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de sessões do usuário. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="package-version"></a>Versão do pacote

O gráfico **Versão do pacote** mostra o número total de falhas e eventos durante o período de tempo selecionado por versão do pacote. Por padrão, mostramos a versão do pacote que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico.

## <a name="failures"></a>Falhas

O gráfico **Falhas** mostra o número total de falhas e eventos durante o período de tempo selecionado por nome da falha. Por padrão, mostramos a falha que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico. Para cada falha, também mostramos a porcentagem do número total de falhas.

Para exibir o relatório **Detalhes da falha** de uma falha específica, selecione o nome da falha. Se você tiver incluído arquivos de símbolo PDB, o relatório **Detalhes da falha** inclui o número de ocorrências de falha ao longo do mês passado, bem como um log de falha que lista os detalhes da ocorrência (data, versão do pacote, tipo de dispositivo, modelo do dispositivo, build do sistema operacional) e um link para o rastreamento de pilha e/ou arquivo CAB, se disponível.

> [!TIP]
> Arquivos CAB estarão disponíveis somente quando a falha ocorrer em um computador usando uma compilação do Windows Insider, portanto, nem todas as falhas incluem a opção de baixar o CAB. Você pode clicar no cabeçalho **Links** no **Log de falha** para classificar os resultados para que as falhas que incluem arquivos CAB apareçam na parte superior da lista.

 

 
