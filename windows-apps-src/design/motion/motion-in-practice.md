---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: 'Movimento na prática: animação em aplicativos UWP'
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 87a17d3f73887c9b1b5029e2096c5b41c9444c4e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843709"
---
# <a name="bringing-it-together"></a>Reunião

> [!IMPORTANT]
> Este artigo descreve uma funcionalidade que ainda não foi lançada e pode ser modificada substancialmente antes de ser lançada comercialmente. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

O tempo, a suavização, a direção e a gravidade trabalham em conjunto para formar a base do movimento Fluente. Cada um deve ser considerado no contexto em relação ao outros e aplicado adequadamente no contexto do aplicativo.

Veja três maneiras de aplicar os conceitos básicos de movimento fluente em seu aplicativo.

:::linha::: :::coluna::: **Animação implícita**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::fim da linha:::

### **<a name="transition-example"></a>Exemplo de transição**

![animação funcional](images/pageRefresh.gif)

:::linha::: :::coluna::: <b>Direção para frente e para fora:</b><br>
        Fade out: 150 m; Suavização: aceleração padrão

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
      
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::fim da linha:::

 ### **<a name="object-example"></a>Exemplo de objeto**

 ![Movimento de 300 ms](images/control.gif)
 
:::linha::: :::coluna::: <b>Expansão da direção:</b><br>
        Aumentar: 300 ms; Suavização: Padrão :::fim da coluna::: :::coluna::: <b>Contrato de direção:</b><br>
        Aumentar: 150 ms; Suavização: Aceleração padrão :::fim da coluna::: :::fim da linha:::

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral do movimento](index.md)
- [Tempo e suavização](timing-and-easing.md)
- [Direção e gravidade](directionality-and-gravity.md)