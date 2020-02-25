---
description: Saiba mais sobre as maneiras de melhorar a visualização de suas unidades publicitárias.
title: Otimizar a visualização das unidades de anúncio
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, diretrizes, visualização
ms.localizationpriority: medium
ms.openlocfilehash: 06d870fbc631611bcfc453698cd9aa4fc844da5d
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506801"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Otimizar a visualização das unidades de anúncio

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

O [relatório de desempenho de publicidade](../publish/advertising-performance-report.md) inclui métricas de visualização para suas unidades publicitárias. A visualização é uma métrica importante porque o setor de publicidade valoriza as impressões visíveis em vez de apenas impressões entregues. Os anunciantes fazer ofertas por impressões visíveis, pois seus anúncios têm uma probabilidade maior de serem vistos por usuários.  

Alinhado com as diretrizes de visualização do IAB, uma impressão de anúncio em faixa é contabilizada como visível se atender aos seguintes critérios:

* Requisito de pixel: 50% ou mais dos pixels no anúncio estavam no espaço visível do aplicativo.
* Requisito de tempo: o tempo em que o requisito de pixel foi atendido foi maior ou igual a um segundo contínuo após a renderização do anúncio.

Uma impressão de anúncio em vídeo é contabilizada como visível se atender aos seguintes critérios:

* Requisito de pixel: 50% ou mais dos pixels no anúncio estavam na parte visível do aplicativo.
* Requisito de tempo: o vídeo atendeu ao requisito de pixel e foi reproduzido por dois segundos contínuos após a renderização do anúncio.

A visualização é calculada usando a seguinte fórmula:

**Visualização = [impressões exibidas] * 100/[total de impressões do AD]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Diretrizes para melhorar a visualização da unidade publicitária

Siga essas diretrizes para otimizar as impressões visíveis das unidades publicitárias.

1. **Avaliar desempenho**&nbsp;&nbsp;Use o [Relatório de desempenho publicitário](../publish/advertising-performance-report.md) para analisar as métricas de visualização de cada unidade publicitária.
2.  **Posicionar o controle de anúncio adequadamente**&nbsp;&nbsp;Em geral, a posição mais visualizada de um anúncio é acima da divisão (ou seja, na parte inferior da parte visível da página que os usuários podem ver sem precisar rolar). Os anúncios exibidos na parte superior da página não são tão visíveis. Considere a inspeção de cada elemento da página para ver como você pode otimizar o espaço visível no seu aplicativo. Certifique-se de que os controles de publicidade não estão em segundo plano no aplicativo.
3.  **Gerenciar comprimento da página**&nbsp;&nbsp;O conteúdo de página mais curto resulta em mais visualizações. Implemente a rolagem infinita se as páginas do aplicativo precisam ser maiores.
4.  **Corrigir a posição do anúncio**&nbsp;&nbsp;Verifique se os anúncio estão em um posição fixa mesmo quando o usuário usa a rolagem. Isso ajudará a obter um tempo de visualização maior e aumentará a taxa de visualização.
5.  **Definir opacidade**&nbsp;&nbsp;Certifique-se de que a opacidade do contêiner com o controle de anúncio é de 100%.
6.  **Implementar o carregamento lento**&nbsp;&nbsp;Não carregue seus anúncios até que os usuários rolem para exibir o local com o controle de anúncios. Isso garante que o anúncio é exibido por mais tempo para o usuário.
7.  **Testar anúncios de diversos tamanhos**&nbsp;&nbsp;Selecione alguns tamanhos de anúncio e avalie a visualização de cada um usando o [Relatório de desempenho publicitário](../publish/advertising-performance-report.md). Selecione a melhor opção para você.
8.  **Revisitar o design do aplicativo**&nbsp;&nbsp;Faça melhoria no design da página do aplicativo para reduzir o tempo de carregamento da página. Os usuários tendem a ficar mais tempo nas páginas que carregam mais rápido.
