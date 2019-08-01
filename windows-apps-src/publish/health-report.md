---
Description: O relatório de integridade no Partner Center permite que você obtenha dados relacionados ao desempenho e à qualidade do seu aplicativo, incluindo falhas e eventos sem resposta.
title: Relatório de integridade
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, integridade, falhas, eventos sem resposta, integridade de aplicativo, dados de integridade, rastreamento de pilha, arquivo cab, falha, falhas, pdb, símbolos
ms.localizationpriority: medium
ms.openlocfilehash: 9b6795673959510d0e4f5452a68ffced6c43ced1
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682497"
---
# <a name="health-report"></a>Relatório de integridade

O relatório de **integridade** no [Partner Center](https://partner.microsoft.com/dashboard) permite que você obtenha dados relacionados ao desempenho e à qualidade do seu aplicativo, incluindo falhas e eventos sem resposta. Você pode exibir esses dados no Partner Center ou [baixar o relatório](download-analytic-reports.md) para exibir offline. Onde for aplicável, você pode exibir rastreamentos de pilha e/ou arquivos CAB para depuração adicional.

Como alternativa, você pode recuperar os dados desse relatório de forma programática usando a [API REST de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **72H** (72 horas), mas você pode escolher **30D** em vez disso, para mostrar os dados nos últimos 30 dias. Observe que os dados são mostrados em seu fuso horário local para a exibição **72h** e em UTC para a exibição **30D** .

Também é possível expandir **Filtros** para filtrar os dados dessa página por versão do pacote, mercado e/ou por tipo de dispositivo.

-   **Versão do pacote**: A configuração padrão é **All**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Mercado**: O filtro padrão é **todos os mercados**, mas você pode limitar os dados a um ou mais mercados.
-   **Tipo de dispositivo**: A configuração padrão é **All**, mas você pode optar por mostrar dados somente para um tipo de dispositivo específico. Observe que a categoria **Outros** inclui dispositivos em que a marca/modelo é reconhecido, mas não podemos incluí-lo em uma das categorias predefinidas mostradas neste filtro. Para esses dispositivos, o modelo de dispositivo pode ser visualizado na seção **Log de falhas** do relatório **Detalhes da falha**.  
-   **Versão do so**: O padrão é **todas as versões do sistema operacional**, mas você pode escolher uma versão específica do sistema operacional.
-   **Versão de lançamento do so**: O padrão é **todas as versões de lançamento do sistema operacional**, mas você pode escolher uma versão de lançamento específica da **versão do sistema operacional**selecionada.
-   **Área restrita**: O padrão é **varejo**, mas para produtos que usam várias áreas restritas de desenvolvimento (como jogos que se integram com o Xbox Live), você pode escolher um específico aqui. (Se o seu produto não usar áreas restritas, esse filtro mostrará apenas **Varejo** e não será aplicável.)
-   **Arquitetura**: O padrão é **todas as arquiteturas**, mas você pode escolher um tipo de arquitetura de sistema específico. Esse filtro só está disponível quando **30D** está selecionado.
-   **PRAID**: A configuração padrão é **All**, mas se você definiu várias IDs de aplicativo relativas ao pacote (PRAIDs) ao criar seu pacote de aplicativo, você pode optar por mostrar somente os dados relacionados a um PRAID. Esse filtro não será exibido se você não tiver definido vários PRAIDs.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e todos os filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


## <a name="failure-hits"></a>Ocorrências de falha

O gráfico **Ocorrências de falhas** mostra a quantidade de falhas e eventos diários que os clientes observaram ao usar o aplicativo durante o período de tempo selecionado. Cada tipo de evento que o aplicativo experimentou é rastreado separadamente: falhas, travamentos, exceções de JavaScript e falhas de memória.

Quando o período de tempo de **30D** for selecionado, você poderá ver marcadores de círculo. Elas representam um aumento ou uma diminuição significativa em um determinado valor que achamos que você desejará conhecer. A data na qual o círculo aparece representa o fim da semana em que detectamos um aumento significativo ou uma diminuição em comparação com a semana antes disso. Para ver mais detalhes sobre o que mudou, passe o mouse sobre o círculo.  

> [!TIP]
> Você pode exibir mais informações relacionadas a alterações significativas nos últimos 30 dias no [relatório](insights-report.md)do insights.

## <a name="failure-hits-by-market"></a>Ocorrências de falha por mercado

O gráfico **Ocorrências de falha por mercado** mostra o total de falhas e eventos durante o período de tempo selecionado por mercado.

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de sessões do usuário. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="package-version"></a>Versão do pacote

O gráfico **Versão do pacote** mostra o número total de falhas e eventos durante o período de tempo selecionado por versão do pacote. Por padrão, mostramos a versão do pacote que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico.

## <a name="failures"></a>Falhas

O gráfico **Falhas** mostra o número total de falhas e eventos durante o período de tempo selecionado por nome da falha. Cada nome de falha é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem/driver em que ocorreu a falha e o nome da função associada. Por padrão, mostramos a falha que tinha mais ocorrências na parte superior e continuamos a partir daí. Você pode reverter essa ordem, alternando a seta na coluna **Ocorrências** deste gráfico. Para cada falha, também mostramos a porcentagem do número total de falhas.

> [!TIP]
> Às vezes, você pode ver uma entrada para **Desconhecido** nesta seção. Isso ocorre quando, apesar dos nossos esforços, não foi possível coletar detalhes completos de uma ou mais falhas, que serão todas agrupadas em **Desconhecido**. Em geral, isso ocorre por conta das restrições de armazenamento, mas também pode ser resultado das configurações de privacidade de um dispositivo, problemas de conexão de rede, despejos de memória parciais/inválidos e outros fatores.
>
> Se você vir **!unknown** como parte de um nome de falha, isso significa que símbolos não estavam presentes e, por isso, não foi possível identificar o nome da falha. Certifique-se de incluir símbolos no seu pacote para obter a análise detalhada da falha. Consulte [Configurar um pacote do aplicativo](/windows/msix/package/packaging-uwp-apps#configure-an-app-package). Por outro lado, nomes de falha que incluem **! unknown_error_in_** e **! unknown_function** significam que não foi possível coletar detalhes completos por conta de vários outros motivos.

Para exibir o relatório **Detalhes da falha** de uma falha específica, selecione o nome da falha. Se você tiver incluído arquivos de símbolo, o relatório **Detalhes da falha** incluirá o número de ocorrências de falhas ao longo do mês anterior, bem como um log de falhas que lista os detalhes das ocorrências (data, versão do pacote, tipo de dispositivo, modelo de dispositivo, compilação do sistema operacional) e um link para o rastreamento de pilha e/ou arquivo CAB, se disponível.

> [!TIP]
> Arquivos CAB estarão disponíveis somente quando a falha ocorrer em um computador usando uma compilação do Windows Insider, portanto, nem todas as falhas incluem a opção de baixar o CAB. Para mostrar apenas as falhas que têm arquivos CAB, selecione **falhas com downloads** na seção filtro. Você também pode clicar no cabeçalho **links** no **log de falhas** para classificar os resultados para que as falhas que incluem arquivos CAB apareçam na parte superior da lista.

Na página **detalhes da falha** , você também verá o gráfico de prevalência de **pilha** , que mostra as pilhas superiores que contribuíram para a falha, ordenada por porcentagem e o gráfico de **configuração do dispositivo (30D)** , que fornece detalhes sobre o configuração de dispositivos que tiveram a falha. 


## <a name="crash-free-sessions-and-devices-30d"></a>Dispositivos e sessões sem falhas (30D)

O gráfico de **sessões e dispositivos sem falhas** mostra a porcentagem de dispositivos ou sessões de usuário que não experimentaram uma falha nos últimos 30 dias. Essas informações ajudam a entender a amplitude de que suas falhas estão afetando os usuários. Por exemplo, um aplicativo pode ter 10.000 falhas em um dia. Se 90% dos seus dispositivos forem afetados, você provavelmente classificará isso como crítico e agirá para corrigi-lo imediatamente. No entanto, se isso representar apenas 5% dos dispositivos que usam seu aplicativo, a prioridade poderá ser menor.

Este gráfico tem duas guias:
- **Dispositivos sem falhas**: Mostra a porcentagem de dispositivos exclusivos que não apresentaram uma falha em cada dia (durante os últimos 30 dias).
- **Sessões sem falhas**: Mostra a porcentagem de sessões de usuário exclusivas que não sofreram uma falha em cada dia (durante os últimos 30 dias).


 

 
