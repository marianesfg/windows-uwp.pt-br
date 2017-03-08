---
author: shawjohn
title: "Promova seu relatório de app - Desenvolva apps UWP"
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: "O relatório &quot;Promover seu app&quot; no painel do Centro de Desenvolvimento do Windows permite que você veja o desempenho de suas campanhas de anúncios de promoção de app."
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, promover, app, campanha, relatório, instalações"
translationtype: Human Translation
ms.sourcegitcommit: b01924366a0bc2afabe2f381e72e45862f0dd682
ms.openlocfilehash: af719bbb4cc9def4e9b202c8815320698c6148d0
ms.lasthandoff: 02/08/2017

---

# <a name="promote-your-app-report"></a>Relatório Promover seu app

O relatório **Promover seu app** no painel do Centro de Desenvolvimento do Windows permite que você veja o desempenho de suas campanhas de anúncios de promoção de app.

Para ver o relatório, selecione **Promoções** no menu de navegação superior. Na parte superior da página **Promover seu app**, você verá uma lista de suas campanhas de anúncios e métricas de desempenho em forma tabular.

Na parte inferior da página, você verá as métricas de desempenho exibidas como linhas plotadas em um gráfico, com tempo no eixo x e a métrica selecionada no eixo y. Selecione os cabeçalhos de guia para exibir essas métricas de desempenho:

-   **Impressões**: a quantidade de vezes em que seu anúncio foi exibido a clientes.
-   **Cliques**: a quantidade de vezes em que um cliente clicou em seu anúncio.
-   **Conversões**: se o seu objetivo de campanha é aumentar instalações do seu app, uma conversão é o número de vezes em que alguém visualizou seu anúncio e instalou seu app em 24 horas. Se o seu objetivo de campanha é aumentar o envolvimento em seu app, uma conversão é o número de vezes em que alguém visualizou seu anúncio e abriu seu app em 24 horas. Veja a seguir para obter mais informações sobre o rastreamento de instalação e como conversões são medidas.
-   **Gastar**: a quantidade de dinheiro que você gastou em cada campanha.

Você pode exibir dados de desempenho de até seis campanhas publicitárias diferentes de cada vez. Selecione **Mais campanhas** para escolher quais campanhas exibir. Você também pode selecionar o símbolo de sinal de subtração ao lado de uma campanha exibida para removê-la.

### <a name="what-is-install-tracking"></a>O que é rastreamento de instalação?

Executar uma campanha publicitária de instalação por meio de **Promover seu app** no Centro de Desenvolvimento oferece a exposição tão necessária para anunciar seus apps. Impressões de anúncio são mostradas para clientes que têm mais probabilidade de se interessar pelo app, e os clientes clicam no anúncio e instalam o app da Loja. Anteriormente, era difícil distinguir entre instalações que foram o resultado de uma campanha publicitária versus instalações que vieram de outras fontes.

O relatório **Promover seu app** mostra quantas instalações que você ganhou devido à sua campanha publicitária. Isso só representa downloads que são um resultado direto de sua campanha publicitária e não inclui downloads de outras fontes.

Monitorando instalações de suas campanhas publicitárias, você pode medir o verdadeiro retorno sobre o investimento de seu dinheiro gasto para promover seus apps. Isso também ajuda a comparar o custo da obtenção de um novo cliente com o valor de tempo de vida dos seus clientes.

### <a name="how-are-conversions-measured"></a>Como as conversões são medidas?

As campanhas publicitárias **Promover seu app** fornecem impressões de anúncios dentro de outros apps. Os clientes que são expostos ao anúncio têm probabilidade de instalar o app em uma de duas maneiras: clicando no anúncio ou com base na exibição da impressão de anúncios.

Se for mostrado um anúncio para um cliente e se ele instalar o app dentro de 24 horas (clicando no anúncio ou indo diretamente para a página do app na Loja), essa instalação será atribuída à campanha que proporcionou a impressão.

Uma instalação é rastreada na Loja (pelo telefone, tablet, computador e outros dispositivos Windows 10) com base no cliente que instalou o app. O mecanismo de publicidade rastreia os clientes que viram o anúncio e usamos essas informações para correlacionar os clientes que visualizaram o anúncio com os clientes que instalaram o app. Para que a instalação do app seja contada, alguns requisitos devem ser atendidos:

1.  O cliente não rejeitou o direcionamento.
2.  O cliente está conectado em uma conta da Microsoft.
3.  O cliente atende aos requisitos de [COPPA](http://go.microsoft.com/fwlink?LinkId=536558) (clientes que não atendem aos requisitos de COPPA não podem ser rastreados).

Consequentemente, é possível que o rastreamento de instalação do app *subnotifique* o número real de instalações geradas por uma campanha **Promover seu app**. Observe que o número total de downloads de um app fica visível no relatório **Aquisições** no Centro de Desenvolvimento.

## <a name="account-billing-history"></a>Histórico de cobrança da conta

Para ver todas as transações associadas à sua conta, selecione **Histórico de cobrança** no menu de navegação esquerdo.

Para cada transação, mostramos d **Data de transação**, o **Nome da campanha** apropriado, a **Forma de pagamento** cobrada, **ID de pagamento**, **Data de início da cobrança**, **Data de término de cobrança**, **Valor Total** da cobrança e **Status de pagamento**.

Você também pode baixar seu histórico de cobrança de conta como um documento do Microsoft Word clicando no link **Download**.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar uma campanha publicitária para seu app](create-an-ad-campaign-for-your-app.md)
* [Gerenciando sua campanha publicitária](managing-your-ad-campaign.md)
* [Sobre anúncios domésticos](about-house-ads.md)
* [Perguntas comuns](common-questions.md)
 

 

