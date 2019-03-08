---
title: Relatório de campanha publicitária
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: O relatório de campanha de anúncio no Partner Center permite ver como estão o desempenho de suas campanhas de anúncios de promoção de aplicativo.
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, promover, app, campanha, relatório, instalações
ms.localizationpriority: medium
ms.openlocfilehash: 7b798abf88805ef3c693149149be958ba0cc1de9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653541"
---
# <a name="ad-campaign-report"></a>Relatório de campanha publicitária

O **campanha publicitária** de relatórios no [Partner Center](https://partner.microsoft.com/dashboard) permite que você veja como promoção de seu aplicativo [campanhas publicitárias](create-an-ad-campaign-for-your-app.md) estão executando. Para ver o relatório, expanda **Attract** no menu de navegação à esquerda e selecione **campanhas publicitárias**.

## <a name="definitions"></a>Definições

Esse relatório mostra dados para os seguintes itens:

-   **Impressões**: O número de vezes que o ad foi concebido para os clientes.
-   **Clicar em**: O número de vezes que um cliente tiver clicado no ad.
-   **Conversões**: Se seu objetivo de campanha é aumentar a instalações do seu aplicativo, uma conversão é o número de vezes que alguém exibidos do ad e, em seguida, instalado seu aplicativo dentro de 24 horas. Se o seu objetivo de campanha é aumentar o envolvimento em seu app, uma conversão é o número de vezes em que alguém visualizou seu anúncio e abriu seu app em 24 horas. Veja a seguir para obter mais informações sobre o rastreamento de instalação e como conversões são medidas.
-   **Gastar**: A quantidade de dinheiro que você gastou em cada campanha.

## <a name="apply-filters"></a>Aplicar filtros

Na parte superior do relatório, você pode usar os **filtros de Seção** para ajustar o escopo dos dados mostrados no relatório:

-   **Data**: A seleção padrão é **últimos 30 dias**, mas você pode optar por mostrar dados para 3, 6 ou 12 meses.
-   **O objetivo de campanha**: Você pode mostrar todas as campanhas, ou limitar os dados para exibir apenas aqueles cujo objetivo é **aplicativo instala** ou **envolvimento no aplicativo**.
-   **Nome do aplicativo**: A seleção padrão é **todos os**, mas você pode optar por mostrar campanhas para apenas um aplicativo específico.
-   **Tipo de campanha**: Você pode mostrar todos os tipos de campanha ou limitar os dados para mostrar apenas pago, casa ou campanhas de anúncios de comunidade.
-   **Status**: A seleção padrão é **todos os**, mas você pode optar por mostrar dados somente de campanhas que se enquadram em um status específico (**Active**, **rascunho**, **pausado**, **Concluído**, ou **precisa de atenção**).


## <a name="ad-campaign-metrics"></a>Métricas de campanha publicitária

Na parte superior da página, você verá uma lista de suas campanhas publicitárias com o número de impressões, cliques e conversões para cada uma, juntamente com o gasto total e todas as ações disponíveis. Para editar uma campanha, é possível clicar no nome na lista.

Na parte inferior da página, você verá essas métricas de desempenho exibidas como linhas plotadas em um gráfico. Selecione os cabeçalhos da guia para exibir dados de **Impressões**, **Cliques**, **Conversões**, ou **Gastos** como descrito acima.

Você pode exibir dados de desempenho de até seis campanhas publicitárias diferentes de cada vez. Selecione **Mais campanhas** para escolher quais campanhas exibir. Você também pode selecionar o símbolo de subtração ao lado de uma campanha exibida para removê-la do gráfico.


## <a name="install-tracking"></a>Rastreamento de instalação

A execução de uma campanha publicitária de instalação por meio do Partner Center fornece a exposição imprescindíveis para anunciar seus aplicativos. As impressões de anúncio são mostradas para clientes que têm mais probabilidade de se interessar pelo aplicativo, e os clientes podem clicar no anúncio e instalar o aplicativo na Loja. Anteriormente, era difícil distinguir entre instalações que foram o resultado de uma campanha publicitária versus instalações que vieram de outras fontes.

Este relatório mostra quantas instalações que você obteve devido às campanhas publicitárias. Isso representa somente os downloads que são um resultado direto das campanhas publicitárias e não inclui downloads de outras fontes.

Monitorando instalações de suas campanhas publicitárias, você pode medir o verdadeiro retorno sobre o investimento de seu dinheiro gasto para promover seus apps. Isso também ajuda a comparar o custo da obtenção de um novo cliente com o valor de tempo de vida dos seus clientes.


## <a name="measuring-conversions"></a>Medição de conversões

As campanhas publicitárias oferecem impressões de anúncios em outros aplicativos. Os clientes que são expostos ao anúncio têm probabilidade de instalar o app em uma de duas maneiras: clicando no anúncio ou com base na exibição da impressão de anúncios.

Se for mostrado um anúncio para um cliente e se ele instalar o aplicativo em 24 horas ao clicar no anúncio ou indo diretamente na página de listagem da Loja do aplicativo, essa instalação será atribuída à campanha que proporcionou a impressão.

Uma instalação é rastreada na Loja pelo telefone, tablet, computador e outros dispositivos Windows 10 com base no cliente que instalou o aplicativo. O mecanismo de publicidade rastreia os clientes que viram o anúncio e usamos essas informações para correlacionar os clientes que visualizaram o anúncio com os clientes que instalaram o app. Para que a instalação do aplicativo seja contada, alguns requisitos devem ser atendidos:

1.  O cliente não rejeitou o direcionamento.
2.  O cliente está conectado em uma conta da Microsoft.
3.  O cliente atende aos requisitos de [COPPA](https://go.microsoft.com/fwlink?LinkId=536558) (clientes que não atendem aos requisitos de COPPA não podem ser rastreados).

Consequentemente, é possível que o rastreamento de instalação do aplicativo subnotifique o número real de instalações geradas por uma campanha publicitária. Observe que você pode exibir o número total de instalações para um aplicativo (por meio de campanhas e, caso contrário) na [aquisições](acquisitions-report.md) relatório no Partner Center.


## <a name="account-billing-history"></a>Histórico de cobrança da conta

Para ver todas as transações da campanha publicitária associadas à sua conta, selecione **Histórico de cobrança** no menu de navegação esquerdo.

Para cada transação, mostramos d **Data de transação**, o **Nome da campanha** apropriado, a **Forma de pagamento** cobrada, **ID de pagamento**, **Data de início da cobrança**, **Data de término de cobrança**, **Valor Total** da cobrança e **Status de pagamento**.

Você pode baixar seu histórico de cobrança de conta como um documento do Microsoft Word clicando no link **Baixar**.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar uma campanha publicitária para seu aplicativo](create-an-ad-campaign-for-your-app.md)

 

 
