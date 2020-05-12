---
title: Espaçamento e tamanhos
description: Os novos estilos de controle Fluent Padrão e Compacto garantem uma experiência de usuário confortável, independentemente do dispositivo e do método de entrada.
keywords: UWP, Windows 10, controles, tamanho, densidade, padrão, compacto
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 5bcc7d45646651cdb60228a3c08123378eedb960
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970611"
---
# <a name="control-size-and-density"></a>Controle de tamanho e densidade

Use uma combinação de tamanho e densidade do controle para otimizar o aplicativo do Windows e fornecer uma experiência de usuário que é mais apropriada aos requisitos de funcionalidade e interação do seu aplicativo.

Por padrão, os aplicativos UWP são processados com um layout de baixa densidade (ou `Standard`). No entanto, a partir do WinUI 2.1, também é compatível com uma opção de layout de alta densidade (ou `Compact`), para oferecer uma interface de usuário rica em informações e cenários especializados semelhantes. Isso pode ser especificado por meio de um recurso de estilo básico (veja os exemplos abaixo).

Embora a funcionalidade e o comportamento não tenham mudado e permaneçam consistentes nas duas opções de tamanho e densidade, o tamanho de fonte de corpo padrão foi atualizado para 14 px para todos os controles a fim de suportar as duas opções de densidade. Esse tamanho de fonte funciona em vários dispositivos e regiões, e garante que seu aplicativo permaneça equilibrado e confortável para os usuários.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Compact Sizing">abri-lo e ver o dimensionamento compacto em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Dimensionamento Fluent padrão

O *Dimensionamento Fluent padrão* foi criado para fornecer um equilíbrio entre densidade de informações e conforto do usuário. Na verdade, todos os itens na tela se alinham a um alvo de 40 px x 40 px efetivos (epx), o que permite que os elementos da interface do usuário se alinhem com uma grade e sejam dimensionados de forma adequada com base no nível do sistema.

**O dimensionamento padrão foi projetado para acomodar a entrada com ponteiro e toque.**

> [!NOTE]
>Para saber mais sobre dimensionamento e pixels efetivos, confira [Introdução ao design de aplicativos do Windows](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Para saber mais sobre o dimensionamento no nível de sistema, confira [Alinhamento, margem, preenchimento](../layout/alignment-margin-padding.md).

Para a Atualização de outubro de 2018 do Windows 10 (versão 1809), o tamanho padrão para todos os controles UWP foi reduzido para aumentar a usabilidade em todos os cenários de uso.

A imagem a seguir mostra algumas das alterações no layout de controle que foram introduzidas com a Atualização de outubro de 2018 do Windows 10. Especificamente, a margem entre um cabeçalho e a parte superior de um controle foi reduzida de 8 epx para 4 epx, e a grade de 44 epx foi alterada para 40 epx.

![Exemplo de layout de controle padrão](images/standarddensity.png)

*Exemplo de layout de controle padrão*

A imagem a seguir mostra as alterações feitas nos tamanhos de controle para a Atualização de outubro de 2018 do Windows 10. Especificamente, alinhamento à grade de 40 epx.

![Exemplo de execução de comando padrão](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Dimensionamento Fluent compacto

O dimensionamento compacto permite grupos de controles densos e ricos em informações e pode ajudar com o seguinte:

- Navegar em grandes volumes de conteúdo.
- Maximizar o conteúdo visível em uma página.
- Navegar e interagir com controles e conteúdo

**O dimensionamento compacto é projetado principalmente para acomodar a entrada com ponteiro.**

### <a name="examples"></a>Exemplos

O dimensionamento compacto é implementado por meio de um dicionário de recursos especiais que pode ser especificado em seu aplicativo no nível da página ou em um layout específico. O dicionário de recursos está disponível no pacote Nuget [WinUI](https://docs.microsoft.com/uwp/toolkits/winui/).

Os exemplos a seguir mostram como o estilo `Compact` pode ser aplicado à página e a um controle de grade individual.

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

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Diretrizes para destinos de toque](../input/guidelines-for-targeting.md)
- [Referências de ResourceDictionary e recursos XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Dicionário de recursos](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary)
- [Estilos de XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) 
