---
Description: O relatório Críticas do painel do Centro de Desenvolvimento do Windows permite que você consulte os comentários que os clientes inseriram na classificação do seu aplicativo na Loja.
title: Relatório Críticas
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
---

# Relatório Críticas


O relatório **Críticas** do painel do Centro de Desenvolvimento do Windows permite que você consulte os comentários que os clientes inseriram na classificação do seu aplicativo na Loja. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline. Como alternativa, você pode recuperar esses dados de forma programática usando a [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

> **Observação** Você também pode [responder às críticas dos clientes](respond-to-customer-reviews.md) nessa página.

Esse relatório mostra o número de estrelas que um cliente concedeu ao seu aplicativo ao fazer uma crítica, mas não analisa as classificações por estrelas em seu aplicativo. Para obter estatísticas sobre suas classificações, consulte o [Relatório de classificações](ratings-report.md).

Observe que os clientes podem deixar uma classificação para o seu aplicativo sem adicionar qualquer comentário, para que você veja normalmente menos críticas do que classificações. Por padrão, esta página também mostra as classificações que não incluem revisar o conteúdo, mas você pode usar os **Filtros de página** para mostrar apenas as classificações que incluem críticas, conforme descrito a seguir.

Cada opinião do cliente contém:

-   O título e o texto da opinião fornecidos pelo cliente. (Críticas escritas por clientes no Windows Phone 8.1 e versões anteriores não terão um título).
-   A data da revisão.
-   O nome do revisor como ele aparece na Windows Store.
-   País/região do revisor.
-   A versão do pacote do aplicativo no dispositivo do cliente no momento em que a crítica foi deixada. (Essas informações não estão disponíveis para críticas enviadas online ou por clientes no Windows 8.1 e versões anteriores.)
-   A versão do sistema operacional do dispositivo que o cliente estava usando quando a crítica foi feita.
-   O nome do dispositivo que o cliente estava usando quando a crítica foi feita. (Essas informações não estão disponíveis para críticas enviadas online ou por clientes no Windows 8.1 e versões anteriores.)
-   A "contagem de utilidade" da crítica, conforme classificada por outros clientes ao ler a crítica. Esses itens são mostrados como uma série de dois números: o primeiro número mostra quantos clientes a classificaram como útil, e o segundo número é o número total de clientes que classificaram a crítica. Por exemplo, uma contagem de utilidade de 4/10 significa que, dos 10 classificadores, 4 consideraram a opinião útil e seis, não. (Se não houver votos de utilidade de uma crítica, nenhuma contagem de utilidade será exibida).

## Aplicar filtros


Perto da parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados dessa página.

>**Dica** Se você não visualizar críticas na página, verifique se os seus filtros não excluíram todas as críticas. Por exemplo, se você filtrar por um sistema operacional de destino que não dá suporte ao seu aplicativo, você não verá quaisquer críticas

-   **Classificação**: por padrão, todas as classificações por estrelas estão marcadas, mas você pode marcar e desmarcar as classificações (de 1 a 5 estrelas) se quiser ver apenas as críticas associadas a uma classificação por estrelas específica.
-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Conteúdo da crítica**: a configuração padrão é **Todos**, o que inclui classificações sem texto de crítica adicionado. Você pode selecionar **Classificações com conteúdo de crítica** para mostrar apenas as classificações que incluem conteúdo de crítica escrito.
-   **Sistema operacional de destino**: a configuração padrão é **Todos**. Você pode escolher um sistema operacional de destino específico, se quiser que essa página mostre apenas as classificações dos clientes que usando pacotes destinados a esse sistema operacional.
-   **Respostas**: a configuração padrão é **Todos**. Você pode optar por filtrar as análises para mostrar apenas as críticas em que você [respondeu aos clientes](respond-to-customer-reviews.md), ou apenas aquelas em que você ainda não respondeu.
-   **Atualizações**: a configuração padrão é **Todas**. Você pode optar por filtrar as análises para mostrar apenas as críticas que foram atualizadas pelo cliente desde que você [respondeu a uma crítica](respond-to-customer-reviews.md), ou apenas aquelas que ainda não foram atualizadas pelo cliente.
-   **Mercado**: a configuração padrão é **Todos os mercados**. Você pode escolher um mercado específico, se quiser que esta página mostre somente as críticas de clientes desse mercado.
-   **Tipo do dispositivo**: o filtro padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente análises deixadas por clientes que estão usando esse dispositivo.
-   **Versão do pacote**: o filtro padrão é **Todos os pacotes**. Se você quiser que essa página mostre apenas análises deixadas por clientes que tinham esse pacote quando analisaram seu aplicativo, você poderá escolher um pacote específico.

As informações em todos os gráficos listados a seguir refletirão o período de tempo selecionado na seção **Aplicar filtros** e refletirá todos os outros filtros que você tenha escolhido aqui.

> **Observação** A classificação média que um cliente vê na Loja leva em consideração o mercado e o tipo de dispositivo do cliente, além das classificações no ano anterior; portanto, ela pode ser diferente do que você vê nesse relatório. Para ver como a classificação média será exibida na Loja para um determinado cliente, você precisará aplicar filtros para selecionar um mercado e um tipo de dispositivo específicos e definir a **Data** como **Últimos 12 meses**.

## Traduzindo críticas


Por padrão, críticas que não foram escritas em seu idioma preferencial são traduzidas para você. Se você preferir, a tradução das críticas pode ser desabilitada, desmarcando-se a caixa de seleção **Traduzir análises** na parte superior direita, acima da lista de críticas.

Observe que críticas são traduzidas por um sistema de tradução automática, e a tradução resultante pode não ser precisa. O texto original será fornecido, se você quiser compará-lo com a tradução, ou traduzi-lo por meio algum outro meio.

## Revisões de classificação


Você pode classificar as criticas na página por data e/ou por classificação, em ordem crescente ou decrescente. Clique no link **Classificar por** para exibir opções de classificação por data e/ou classificação. Quando você clicar em um botão de opção na seção Data ou Classificação, os critérios de classificação serão aplicados, e você verá o rótulo de classificação mostrado ao lado do título **Classificar por**. Você pode remover todos os critérios de classificação clicando no **X** exibido em cada rótulo.

## Respondendo às críticas dos clientes


Você pode usar o painel do Centro de Desenvolvimento da Windows Store para enviar respostas a muitas das críticas dos clientes. Para saber mais, consulte [Responder às críticas dos clientes](respond-to-customer-reviews.md)

Veja algumas ações adicionais que você pode realizar com base nas classificações e críticas observadas.

-   Caso veja muitas críticas sugerindo um recurso novo ou alterado ou com reclamações sobre um problema, pense em lançar uma nova versão que atenda especificamente a esses comentários (Não se esqueça de atualizar a [descrição](create-app-descriptions.md) do aplicativo para indicar que o problema foi corrigido).
-   Caso a classificação média seja alta, mas o número de downloads seja baixo, convém procurar formas de [expor seu aplicativo a mais pessoas](app-promotion-and-customer-engagement.md), já que ele foi é bem-recebido pelos usuários que o testaram.

> **Observação** É provável que você consulte um número diferente de críticas ao comparar o relatório **Críticas** no Centro de Desenvolvimento do Windows com o relatório Críticas no aplicativo móvel mais antigo do Centro de Desenvolvimento. Isso ocorre porque o aplicativo mostra apenas os dados de avaliações de clientes no Windows Phone 8.1 e versões anteriores. Isso também pode ser resultado do trabalho da Microsoft de remover avaliações da Windows Store que foram identificadas como spam, inadequadas, ofensivas ou que violam a política de outra forma. Esperamos que essa ação resulte em uma melhor experiência para o cliente.

 

 

 


<!--HONumber=Mar16_HO1-->


