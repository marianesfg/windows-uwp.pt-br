---
Description: Este tópico descreve o uso da geometria de contato por área de toque e fornece as práticas recomendadas de direcionamento em aplicativos do Windows Runtime.
title: Direcionamento
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5c05b6686d31606a9510b1433339dc8829a52893
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59247174"
---
# <a name="guidelines-for-touch-targets"></a>Diretrizes para destinos de toque

Todos os elementos de interface do usuário interativos em seu aplicativo da plataforma Universal do Windows (UWP) devem ser grandes o suficiente para os usuários acessem com precisão e usar, independentemente do método de entrada ou de tipo de dispositivo.

Suporte a entrada de toque (e a natureza relativamente imprecisa da área de contato de toque) requer a otimização adicional em relação ao layout de tamanho e o controle de destino como o conjunto de maior e mais complexo de relatado pelo digitalizador do toque de dados de entrada é usado para determinar o destino do usuário pretendida (ou mais provável).

Todos os controles UWP foram projetados com tamanhos padrão de destino de toque e layouts que permitem criar aplicativos visualmente equilibrados e atraentes que são fáceis de usar, confortável e transmitir confiança.

Neste tópico, descreveremos esses comportamentos padrão, portanto, você pode projetar seu aplicativo para facilitar o uso máximo usando controles de plataforma e controles personalizados (deve seu aplicativo exigi-los).

> **APIs importantes**: [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="fluent-standard-sizing"></a>Dimensionamento padrão Fluent

*Dimensionamento padrão Fluent* foi criado para fornecer um equilíbrio entre o conforto de densidade e o usuário de informações. Na verdade, todos os itens na tela alinham a um destino efetivo de 40 x 40 pixels (epx), que permite que elementos de interface do usuário se alinham com uma grade e dimensionada de forma adequada com base em dimensionamento do nível do sistema.

> [!NOTE]
>Para obter mais informações sobre pixels relevantes e dimensionamento, consulte [Introdução ao design de aplicativos UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Para obter mais informações sobre o dimensionamento de nível de sistema, consulte [alinhamento, margem, preenchimento](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Dimensionamento Compact Fluent

Aplicativos podem exibir um nível mais alto de densidade de informações com *dimensionamento Fluent Compact*. Dimensionamento Compact alinha os elementos de interface do usuário para um destino de 32 x 32 epx, que permite que os elementos de interface do usuário para alinhar-se para uma grade mais rigorosa e uma escala adequadamente com base na escala de nível de sistema.

### <a name="examples"></a>Exemplos

Compact dimensionamento pode ser aplicado no nível de página ou grade.

### <a name="page-level"></a>Nível de página

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>Nível de grade

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>Tamanho de destino

Em geral, defina seu tamanho de destino de toque ao intervalo de quadrados de 7,5 mm (40 x 40 pixels em uma exibição PPI 135 em um 1.0 x dimensionamento sammamish). Normalmente, os controles UWP alinham com o destino de toque de 7,5 mm (Isso pode variar com base no controle específico e padrões comuns de uso). Ver [controlar o tamanho e densidade](../style/spacing.md) para obter mais detalhes.

Essas recomendações de tamanho de destino podem ser ajustadas de acordo com determinado cenário. Aqui estão algumas coisas a considerar:

- Frequência de toques - Considere tornar destinos que são frequentemente ou repetidamente pressionados maiores do que o tamanho mínimo.
- Erro consequência - destinos que têm sérias consequências se tocadas em erro deve ter o preenchimento maior e ser colocada mais longe da borda da área de conteúdo. Isso vale principalmente para destinos que são tocados com frequência.
- Posição na área de conteúdo.
- Tamanho da tela e fatores de formulário.
- Postura de dedo.
- Visualizações de toque.

## <a name="related-articles"></a>Artigos relacionados

- [Introdução ao design de aplicativos UWP](../basics/design-and-ui-intro.md)
- [Controle de tamanho e densidade](../style/spacing.md)
- [Alinhamento, margem, preenchimento](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Exemplos

- [Exemplo de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [Exemplo de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [Exemplo do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [Amostra de elementos visuais do foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>Exemplos de arquivo

- [Entrada: Exemplo de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [Entrada: Exemplo de recursos do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [Entrada: Exemplo de teste de hit de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [Entrada: Exemplo simplificado de tinta](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [Entrada: Exemplo de gestos do Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [Entrada: Manipulações e exemplo de gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [Amostra de entrada por toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
