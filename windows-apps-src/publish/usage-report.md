---
Description: O relatório Uso no painel do Centro de Desenvolvimento do Windows permite que você veja como os clientes estão usando seu aplicativo.
title: Relatório de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
---

# Relatório de uso


> **Importante** O relatório **Uso** fornecerá dados somente se você tiver ativado o [SDK do Application Insights do Visual Studio](http://go.microsoft.com/fwlink/?LinkId=615086) em seu aplicativo (ou habilitá-lo, marcando a caixa "Mostrar telemetria no Centro de Desenvolvimento do Windows" ao criar seu pacote). Você também deve enviar o aplicativo no novo painel unificado do Centro de Desenvolvimento para que os dados sejam mostrados nesse relatório. Além disso, você deve ter habilitado a telemetria de uso do aplicativo em suas Configurações de conta.

O relatório **Uso** no painel do Centro de Desenvolvimento do Windows permite que você veja como os clientes estão usando seu aplicativo. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline.

Esse relatório fornece uma visão de alto nível do uso do seu aplicativo. Se tiver associado uma assinatura do Azure com o SDK do Application Insights do Visual Studio para seu aplicativo, você poderá obter informações mais detalhadas de telemetria de uso do aplicativo no Portal do Azure. Links para relatórios do Portal do Azure do seu aplicativo serão fornecidos na parte superior do relatório.

Observe que a conta da Microsoft associada com sua conta de desenvolvedor deve estar associada com a assinatura do Azure usada no Application Insights. Se você não estiver usando a mesma conta da Microsoft em ambos os lugares, deverá fazer logon no Portal de Gerenciamento do Azure e, em seguida, adicionar a conta da Microsoft associada à sua conta de desenvolvedor como um administrador de serviços, coadministrador ou na função de proprietário dessa assinatura.

## Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados desta página pelo intervalo de datas e/ou grupo de produtos (versões relacionadas do sistema operacional).

-   **Data**: o filtro padrão é **Últimas 72 horas**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Grupos de produtos**: a configuração padrão é **Todos**. Se o seu aplicativo inclui mais de um grupo de produtos, você poderá escolher um específico aqui.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado em **Aplicar filtros**. Por padrão, isso incluirá dados de todos os grupos de produtos com suporte, a menos que você tenha usado a seção **Aplicar filtros** para filtrar apenas um.

## Total de sessões de usuário


O gráfico **Total de sessões de usuário** mostra o número de sessões de usuário diário do seu aplicativo durante o período de tempo selecionado.

Cada sessão de usuário representa um cliente iniciando e usando o seu aplicativo por um período de tempo. Este gráfico não rastreia usuários únicos (isto é, várias sessões de usuários mostradas aqui poderiam ser do mesmo cliente).

## Usuários ativos


O gráfico **Usuários ativos** mostra o número de clientes que usaram seu aplicativo em um dia específico durante o período de tempo selecionado.

Cada usuário ativo representa um cliente que usou seu aplicativo nesse dia. Este gráfico não rastreia sessões de usuário únicos (ou seja, um cliente é representado neste gráfico se eles usou seu aplicativo apenas uma vez ou várias vezes nesse dia).

## Média de tempo de sessão do usuário em segundos


O gráfico **Média de tempo de sessão do usuário em segundos** mostra a duração média de tempo em que um cliente usou seu aplicativo em um dia específico durante o período de tempo selecionado.

Você também pode clicar em **Média de tempo entre as sessões em segundos** para que esse gráfico exiba o tempo médio decorrido entre sessões separadas de uso de seu aplicativo durante o período de tempo selecionado.

## Eventos personalizados nos últimos 30 dias


O gráfico **Eventos personalizados nos últimos 30 dias** mostra o total de ocorrências de quaisquer eventos personalizados que você definiu para o seu aplicativo. Isso pode incluir várias ocorrências do mesmo cliente.

## Visualizações de página nos últimos 30 dias


O gráfico **Page views over last 30 days** mostra o número total de visualizações de páginas específicas em seu aplicativo. Isso pode incluir várias visualizações do mesmo cliente.

 

 






<!--HONumber=Mar16_HO1-->


