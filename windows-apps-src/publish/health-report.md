---
Description: The Health report in Partner Center lets you get data related to the performance and quality of your app, including crashes and unresponsive events.
title: Relatório de integridade
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, integridade, falhas, eventos sem resposta, integridade de aplicativo, dados de integridade, rastreamento de pilha, arquivo cab, falha, falhas, pdb, símbolos
ms.localizationpriority: medium
ms.openlocfilehash: c547cc933247e69fd208e8d3c297572815a5f2ea
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8486925"
---
# <a name="health-report"></a>Relatório de integridade

O relatório de **integridade** no [Partner Center](https://partner.microsoft.com/dashboard) permite que você obtenha dados relacionados ao desempenho e à qualidade de seu aplicativo, incluindo falhas e eventos sem resposta. Você pode exibir esses dados no Partner Center, ou [baixar o relatório](download-analytic-reports.md) para exibição offline. Onde for aplicável, você pode exibir rastreamentos de pilha e/ou arquivos CAB para depuração adicional.

Como alternativa, você pode recuperar os dados desse relatório de forma programática usando a [API REST de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **72H** (72 horas), mas você pode escolher **30D** em vez disso, para mostrar os dados nos últimos 30 dias. Observe que os dados são mostrados em seu fuso horário local para a exibição **72h** e em UTC para a exibição **30d** .

Também é possível expandir **Filtros** para filtrar os dados dessa página por versão do pacote, mercado e/ou por tipo de dispositivo.

-   **Versão do pacote**: a configuração padrão é **Tudo**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Mercado**: o filtro padrão é **Todos os mercados**, mas você pode limitar os dados a um ou mais mercados.
-   **Tipo de dispositivo**: a configuração padrão é **Todos**, mas você pode optar por mostrar dados apenas de um tipo específico de dispositivo. Observe que a categoria **Outros** inclui dispositivos em que a marca/modelo é reconhecido, mas não podemos incluí-lo em uma das categorias predefinidas mostradas neste filtro. Para esses dispositivos, o modelo de dispositivo pode ser visualizado na seção **Log de falhas** do relatório **Detalhes da falha**.  
-   **Versão do sistema operacional**: o padrão é **Todas as versões do sistema operacional**, mas você pode optar por uma versão específica do sistema operacional.
-   **Versão de lançamento do SO**: o padrão é **Todas as versões de lançamento do sistema operacional**, mas você pode optar por uma versão de lançamento específica da **Versão do sistema operacional** selecionada.
-   **Área restrita**: o padrão é **Varejo**, mas para os produtos que usam várias áreas restritas de desenvolvimento (como jogos que se integram ao Xbox Live), você pode optar por uma área específica aqui. (Se o seu produto não usar áreas restritas, esse filtro mostrará apenas **Varejo** e não será aplicável.)
-   **Arquitetura**: o padrão é **Todas as arquiteturas**, mas você pode optar por um tipo específico de arquitetura do sistema. Esse filtro só está disponível quando **30D** está selecionado.
-   **PRAID**: a configuração padrão é **Todos**, mas, se você definiu vários IDs do aplicativo relacionado ao pacote (PRAIDs) ao criar o pacote do aplicativo, pode optar por mostrar somente os dados relacionados a um PRAID. Esse filtro não será exibido se você não tiver definido vários PRAIDs.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e todos os filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


## <a name="failure-hits"></a>Ocorrências de falha

O gráfico **Ocorrências de falhas** mostra a quantidade de falhas e eventos diários que os clientes observaram ao usar o aplicativo durante o período de tempo selecionado. Cada tipo de evento que o aplicativo experimentou é rastreado separadamente: falhas, travamentos, exceções de JavaScript e falhas de memória.

Quando o **30d** período é selecionado, você poderá ver marcadores em círculo. Eles representam um aumento significativo ou diminuir em um determinado valor acreditamos que você vai querer saber sobre. A data em que o círculo é exibido representa o final da semana em que detectamos uma significativo aumento ou redução em comparação comparada a semana anterior. Para ver mais detalhes sobre o que mudou, passe o mouse sobre o círculo.  

> [!TIP]
> Você pode exibir mais informações relacionadas a alterações significativas nos últimos 30 dias no [relatório de ideias](insights-report.md).

## <a name="failure-hits-by-market"></a>Ocorrências de falha por mercado

O gráfico **Ocorrências de falha por mercado** mostra o total de falhas e eventos durante o período de tempo selecionado por mercado.

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de sessões do usuário. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="package-version"></a>Versão do pacote

O gráfico **Versão do pacote** mostra o número total de falhas e eventos durante o período de tempo selecionado por versão do pacote. Por padrão, mostramos a versão do pacote que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico.

## <a name="failures"></a>Falhas

O gráfico **Falhas** mostra o número total de falhas e eventos durante o período selecionado por nome da falha. Cada nome de falha é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem/driver em que ocorreu a falha e o nome da função associada. Por padrão, mostramos a falha com mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico. Para cada falha, também mostramos a porcentagem do número total de falhas.

> [!TIP]
> Às vezes, você pode ver uma entrada para **Desconhecido** nesta seção. Isso ocorre quando, apesar dos nossos esforços, não foi possível coletar detalhes completos de uma ou mais falhas, que serão todas agrupadas em **Desconhecido**. Em geral, isso ocorre por conta das restrições de armazenamento, mas também pode ser resultado das configurações de privacidade de um dispositivo, problemas de conexão de rede, despejos de memória parciais/inválidos e outros fatores.
>
> Se você vir **!unknown** como parte de um nome de falha, isso significa que símbolos não estavam presentes e, por isso, não foi possível identificar o nome da falha. Certifique-se de incluir símbolos no seu pacote para obter a análise detalhada da falha. Consulte [Configurar um pacote do aplicativo](../packaging/packaging-uwp-apps.md#configure-an-app-package). Por outro lado, nomes de falha que incluem **! unknown_error_in_** e **! unknown_function** significam que não foi possível coletar detalhes completos por conta de vários outros motivos.

Para exibir o relatório **Detalhes da falha** de uma falha específica, selecione o nome da falha. Se você tiver incluído arquivos de símbolo, o relatório **Detalhes da falha** incluirá o número de ocorrências de falhas ao longo do mês anterior, bem como um log de falhas que lista os detalhes das ocorrências (data, versão do pacote, tipo de dispositivo, modelo de dispositivo, compilação do sistema operacional) e um link para o rastreamento de pilha e/ou arquivo CAB, se disponível.

> [!TIP]
> Arquivos CAB estarão disponíveis somente quando a falha ocorrer em um computador usando uma compilação do Windows Insider, portanto, nem todas as falhas incluem a opção de baixar o CAB. Para mostrar apenas as falhas que têm arquivos CAB, selecione **falhas com downloads** no filtro de seção. Você também pode clicar no cabeçalho **Links** o **log de falhas** para classificar os resultados para que as falhas que incluem arquivos CAB apareçam na parte superior da lista.

Na página de **detalhes da falha** , você também verá o gráfico **prevalência de pilha** , que mostra a parte superior de pilhas que contribuíram para a falha, ordenados por percentual e o gráfico de **configuração do dispositivo (30d)** , que fornece detalhes sobre o configuração dos dispositivos que ocorreu a falha. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sessões livre de falhas e dispositivos (30d)

O gráfico de **dispositivos e sessões livre de pane** mostra a porcentagem de dispositivos ou sessões de usuário que não experimentou uma falha nos últimos 30 dias. Essas informações ajudam você a entender a amplitude seus falhas estão afetando seus usuários. Por exemplo, um aplicativo poderia ter 10.000 falhas em um dia. Se 90% dos seus dispositivos são afetados, em seguida, você seria provavelmente classificar que como crítica e agir para corrigi-lo imediatamente. No entanto, se apenas que representa 5% dos dispositivos usando seu aplicativo, a prioridade pode ser menor.

Este gráfico tem duas guias:
- **Dispositivos livre de falha**: mostra o percentual de dispositivos exclusivos que não experimentou uma falha em cada dia (durante os últimos 30 dias).
- **Sessões livre de falha**: mostra a porcentagem de sessões de usuário exclusivos que não experimentou uma falha em cada dia (durante os últimos 30 dias).


 

 
