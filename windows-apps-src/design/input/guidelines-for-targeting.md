---
Description: Este tópico descreve o uso da geometria de contato por área de toque e fornece as práticas recomendadas de direcionamento em aplicativos do Windows Runtime.
title: Configurando destinos
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 13737e9aeb906e0430b2a18dd75ae1fce3a75956
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234582"
---
# <a name="guidelines-for-touch-targets"></a>Diretrizes para destinos de toque

Todos os elementos interativos da interface do usuário no seu aplicativo do Windows devem ser grandes o suficiente para que os usuários acessem e usem com precisão, independentemente do tipo de dispositivo ou do método de entrada.

Dar suporte à entrada por toque (e a natureza relativamente precisa da área de contato de toque) requer mais otimização em relação ao tamanho de destino e ao layout de controle, pois o conjunto maior, mais complexo de dados de entrada relatados pelo Touch digitalizador é usado para determinar o destino pretendido (ou mais provável) do usuário.

Todos os controles UWP foram projetados com tamanhos e layouts de destino por toque padrão que permitem que você crie aplicativos visualmente equilibrados e atraentes que são confortáveis, fáceis de usar e inspira confiança.

Neste tópico, descrevemos esses comportamentos padrão para que você possa projetar seu aplicativo para a usabilidade máxima usando controles de plataforma e controles personalizados (caso seu aplicativo precise deles).

> **APIs importantes**: [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Dimensionamento Fluent padrão

O *Dimensionamento Fluent padrão* foi criado para fornecer um equilíbrio entre densidade de informações e conforto do usuário. Na verdade, todos os itens na tela se alinham a um alvo de 40 px x 40 px efetivos (epx), o que permite que os elementos da interface do usuário se alinhem com uma grade e sejam dimensionados de forma adequada com base no nível do sistema.

> [!NOTE]
> Para saber mais sobre dimensionamento e pixels efetivos, confira [Introdução ao design de aplicativos do Windows](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Para saber mais sobre o dimensionamento no nível de sistema, confira [Alinhamento, margem, preenchimento](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Dimensionamento Fluent compacto

Os aplicativos podem exibir um nível mais alto de densidade de informações com *dimensionamento compacto fluente*. O dimensionamento compacto alinha os elementos da interface do usuário com um destino EPX de 32x32, que permite que os elementos da interface do usuário se alinhem a uma grade mais rígida e dimensionam adequadamente com base no dimensionamento no nível do sistema

### <a name="examples"></a>Exemplos

O dimensionamento compacto pode ser aplicado no nível da página ou da grade.

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

Em geral, defina seu tamanho de destino de toque como o intervalo quadrado de 7,5 mm (40x40 pixels em uma exibição de 135 PPI em um limite de dimensionamento 1.0 x). Normalmente, os controles UWP se alinham com o destino de toque de 7,5 mm (isso pode variar com base no controle específico e em qualquer padrão de uso comum). Consulte [tamanho e densidade do controle](../style/spacing.md) para obter mais detalhes.

Essas recomendações de tamanho de destino podem ser ajustadas de acordo com determinado cenário. Estas são algumas coisas que você deve considerar:

- Frequência de toques – considere a possibilidade de tornar os destinos que são pressionados repetidamente ou frequentemente maiores que o tamanho mínimo.
- Conseqüência de erros-destinos que têm consequências graves se tocadas em erro devem ter um preenchimento maior e serem colocados além da borda da área de conteúdo. Isso vale principalmente para destinos que são tocados com frequência.
- Posição na área de conteúdo.
- Fator de forma e tamanho da tela.
- Postura de dedo.
- Visualizações de toque.

## <a name="related-articles"></a>Artigos relacionados

- [Introdução ao design de aplicativos do Windows](../basics/design-and-ui-intro.md)
- [Controle de tamanho e densidade](../style/spacing.md)
- [Alinhamento, margem, preenchimento](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Amostras

- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Amostra de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemplos de arquivo-morto

- [Entrada: amostra de eventos de entrada do usuário XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: amostra de funcionalidades do dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: amostra de teste de hit de toque](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemplo de rolagem, panorâmica e zoom do XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: amostra de tinta simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: amostra de gestos no Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: exemplo de manipulações e gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Amostra de entrada por toque do DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
