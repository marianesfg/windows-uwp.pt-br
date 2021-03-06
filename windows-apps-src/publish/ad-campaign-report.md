---
title: Relatório de campanha publicitária
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: O relatório de campanha do AD no Partner Center permite que você veja como as campanhas de anúncios de promoção de aplicativos estão sendo executadas.
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, promover, app, campanha, relatório, instalações
ms.localizationpriority: medium
ms.openlocfilehash: fef1c148db8a1c53be6c7ffd15cd8cd2992868ee
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260070"
---
# <a name="ad-campaign-report"></a>Relatório de campanha publicitária

O relatório de **campanha do AD** no [Partner Center](https://partner.microsoft.com/dashboard) permite que você veja como as [campanhas de anúncios](create-an-ad-campaign-for-your-app.md) de promoção de aplicativos estão sendo executadas. Para ver o relatório, expanda **atrair** no menu de navegação à esquerda e selecione **campanhas do AD**.

## <a name="definitions"></a>Definições

Esse relatório mostra dados para os seguintes itens:

-   **Impressões**: a quantidade de vezes em que seu anúncio foi exibido a clientes.
-   **Cliques**: a quantidade de vezes em que um cliente clicou em seu anúncio.
-   **Conversões**: se o seu objetivo de campanha é aumentar instalações do seu app, uma conversão é o número de vezes em que alguém visualizou seu anúncio e instalou seu app em 24 horas. Se o seu objetivo de campanha é aumentar o envolvimento em seu app, uma conversão é o número de vezes em que alguém visualizou seu anúncio e abriu seu app em 24 horas. Veja a seguir para obter mais informações sobre o rastreamento de instalação e como conversões são medidas.
-   **Gastar**: a quantidade de dinheiro que você gastou em cada campanha.

## <a name="apply-filters"></a>Aplicar filtros

Na parte superior do relatório, você pode usar os **filtros de Seção** para ajustar o escopo dos dados mostrados no relatório:

-   **Data**: a seleção padrão é **Últimos 30 dias**, mas você pode optar por mostrar dados de 30 dias, 3, 6 ou 12 meses.
-   **Objetivo de campanha**: você pode mostrar todas as campanhas ou limitar os dados para mostrar apenas aqueles cujo objetivo é **Instalações de aplicativo** ou **Envolvimento do aplicativo**.
-   **Nome do aplicativo**: a seleção padrão é **Tudo**, mas é possível optar por mostrar campanhas apenas de um tipo de dispositivo de aplicativo.
-   **Tipo de campanha**: você pode mostrar todos os tipos de campanha ou limitar os dados para mostrar apenas campanhas publicitárias pagas, internas ou da comunidade.
-   **Status**: a seleção padrão é **Todos**, mas você pode optar por mostrar dados apenas para campanhas que se enquadram em um determinado status (**Ativa**, **Rascunho**, **Pausada**, **Concluída**, ou **Requer atenção**).


## <a name="ad-campaign-metrics"></a>Métricas de campanha publicitária

Na parte superior da página, você verá uma lista de suas campanhas publicitárias com o número de impressões, cliques e conversões para cada uma, juntamente com o gasto total e todas as ações disponíveis. Para editar uma campanha, é possível clicar no nome na lista.

Na parte inferior da página, você verá essas métricas de desempenho exibidas como linhas plotadas em um gráfico. Selecione os cabeçalhos da guia para exibir dados de **Impressões**, **Cliques**, **Conversões**, ou **Gastos** como descrito acima.

Você pode exibir dados de desempenho de até seis campanhas publicitárias diferentes de cada vez. Selecione **Mais campanhas** para escolher quais campanhas exibir. Você também pode selecionar o símbolo de subtração ao lado de uma campanha exibida para removê-la do gráfico.


## <a name="install-tracking"></a>Rastreamento de instalação

A execução de uma campanha de instalação do AD por meio do Partner Center fornece uma exposição muito necessária para anunciar seus aplicativos. As impressões de anúncio são mostradas para clientes que têm mais probabilidade de se interessar pelo aplicativo, e os clientes podem clicar no anúncio e instalar o aplicativo na Loja. Anteriormente, era difícil distinguir entre instalações que foram o resultado de uma campanha publicitária versus instalações que vieram de outras fontes.

Este relatório mostra quantas instalações que você obteve devido às campanhas publicitárias. Isso representa somente os downloads que são um resultado direto das campanhas publicitárias e não inclui downloads de outras fontes.

Monitorando instalações de suas campanhas publicitárias, você pode medir o verdadeiro retorno sobre o investimento de seu dinheiro gasto para promover seus apps. Isso também ajuda a comparar o custo da obtenção de um novo cliente com o valor de tempo de vida dos seus clientes.


## <a name="measuring-conversions"></a>Medição de conversões

As campanhas publicitárias oferecem impressões de anúncios em outros aplicativos. Os clientes que são expostos ao anúncio têm probabilidade de instalar o app em uma de duas maneiras: clicando no anúncio ou com base na exibição da impressão de anúncios.

Se for mostrado um anúncio para um cliente e se ele instalar o aplicativo em 24 horas ao clicar no anúncio ou indo diretamente na página de listagem da Loja do aplicativo, essa instalação será atribuída à campanha que proporcionou a impressão.

Uma instalação é rastreada na Loja pelo telefone, tablet, computador e outros dispositivos Windows 10 com base no cliente que instalou o aplicativo. O mecanismo de publicidade rastreia os clientes que viram o anúncio e usamos essas informações para correlacionar os clientes que visualizaram o anúncio com os clientes que instalaram o app. Para que a instalação do app seja contada, alguns requisitos devem ser atendidos:

1.  O cliente não rejeitou o direcionamento.
2.  O cliente está conectado em uma conta da Microsoft.
3.  O cliente atende aos requisitos de [COPPA](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule) (clientes que não atendem aos requisitos de COPPA não podem ser rastreados).

Consequentemente, é possível que o rastreamento de instalação do aplicativo subnotifique o número real de instalações geradas por uma campanha publicitária. Observe que você pode exibir o número total de instalações de um aplicativo (por meio de campanhas e de outra forma) no relatório de [aquisições](acquisitions-report.md) no Partner Center.


## <a name="account-billing-history"></a>Histórico de cobrança da conta

Para ver todas as transações da campanha publicitária associadas à sua conta, selecione **Histórico de cobrança** no menu de navegação esquerdo.

Para cada transação, mostramos d **Data de transação**, o **Nome da campanha** apropriado, a **Forma de pagamento** cobrada, **ID de pagamento**, **Data de início da cobrança**, **Data de término de cobrança**, **Valor Total** da cobrança e **Status de pagamento**.

Você pode baixar seu histórico de cobrança de conta como um documento do Microsoft Word clicando no link **Baixar**.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar uma campanha do AD para seu aplicativo](create-an-ad-campaign-for-your-app.md)

 

 
