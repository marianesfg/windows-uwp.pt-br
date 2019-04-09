---
title: Tamanhos e espaçamento
description: Novos estilos de controle padrão Fluent e Compact garantir uma experiência de usuário está confortável, independentemente do método de entrada e de dispositivo.
keywords: UWP, Windows 10, controles, tamanho, densidade, standard, compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249448"
---
# <a name="control-size-and-density"></a>Controle de tamanho e densidade

Use uma combinação do tamanho do controle e a densidade para otimizar o aplicativo de plataforma Universal do Windows (UWP) e fornecer uma experiência de usuário que é mais apropriada para os requisitos de funcionalidade e a interação do seu aplicativo.

Por padrão, os aplicativos UWP são processados com uma baixa densidade (ou `Standard`) layout. No entanto, começando com o WinUI 2.1, uma alta densidade (ou `Compact`) opção de layout para informações de interface de usuário rica e semelhantes cenários especializados, também tem suporte. Isso pode ser especificado por meio de um recurso de estilo básico (veja os exemplos abaixo).

Durante a funcionalidade e o comportamento não foi alterado e permanece consistente entre as duas opções de tamanho e densidade, o tamanho de fonte de corpo padrão foi atualizado para 14px para todos os controles dar suporte a essas opções de densidade de dois. Esse tamanho de fonte funciona em regiões e dispositivos e garante que seu aplicativo permaneça balanceada e confortável para os usuários.

## <a name="fluent-standard-sizing"></a>Dimensionamento padrão Fluent

*Dimensionamento padrão Fluent* foi criado para fornecer um equilíbrio entre o conforto de densidade e o usuário de informações. Na verdade, todos os itens na tela alinham a um destino efetivo de 40 x 40 pixels (epx), que permite que elementos de interface do usuário se alinham com uma grade e dimensionada de forma adequada com base em dimensionamento do nível do sistema.

**Dimensionamento padrão foi projetado para acomodar o toque e o ponteiro de entrada.**

> [!NOTE]
>Para obter mais informações sobre pixels relevantes e dimensionamento, consulte [Introdução ao design de aplicativos UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Para obter mais informações sobre o dimensionamento de nível de sistema, consulte [alinhamento, margem, preenchimento](../layout/alignment-margin-padding.md).

Para o Windows 10 de outubro de 2018 Update (versão 1809), o padrão, o tamanho padrão para todos os controles UWP foi reduzido para aumentar a usabilidade em todos os cenários de uso.

A imagem a seguir mostra alguns do controle de alterações no layout que foram introduzidas com o Windows Update 10 de outubro de 2018. Especificamente, a margem entre um cabeçalho e a parte superior de um controle foi reduzida em relação a 8epx para 4epx e a grade de 44epx foi alterada para uma grade 40epx.

![Exemplo de layout de controle padrão](images/standarddensity.png)

*Exemplo de layout de controle padrão*

Essa imagem a seguir mostra as alterações feitas em tamanhos de controle para o Windows Update 10 de outubro de 2018. Especificamente, alinhamento à grade 40epx.

![Exemplo de comando padrão](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Dimensionamento Compact Fluent

Dimensionamento Compact habilita densos, ricos em informações de grupos de controles e pode ajudar com o seguinte:

- Navegando em grandes volumes de conteúdo.
- Maximizando o conteúdo visível em uma página.
- Navegar e interagir com controles e conteúdo

**Dimensionamento Compact é projetado principalmente para acomodar o ponteiro de entrada.**

### <a name="examples"></a>Exemplos

Dimensionamento Compact é implementado por meio de um dicionário de recursos especiais que pode ser especificado em seu aplicativo no nível de página ou em um layout específico. O dicionário de recursos está disponível na [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) pacote do Nuget.

Os exemplos a seguir mostram como o o `Compact` estilo pode ser aplicado para a página e um controle de grade individual.

#### <a name="page-level"></a>Nível de página

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>Nível de grade

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>Artigos relacionados

- [Diretrizes para destinos de toque](../input/guidelines-for-targeting.md)
- [Referências de recursos de ResourceDictionary e XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Dicionário de recursos](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [Estilos de XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
