---
title: Atributo xPhase
description: Use xPhase com a extensão de marcação xBind para renderizar os itens ListView e GridView de maneira incremental e melhorar a experiência de movimento panorâmico.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6def088b3e7f6410f12d1b2e411bcb547c90a09a
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918557"
---
# <a name="xphase-attribute"></a>atributo x:Phase


Use **x:Phase** com a [extensão de marcação {x:Bind}](x-bind-markup-extension.md) para renderizar os itens [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) de maneira incremental e melhorar a experiência de movimento panorâmico. **x:Phase** oferece uma maneira declarativa de conseguir o mesmo efeito que o uso do evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) para controlar manualmente a renderização de itens de lista. Consulte também [Atualizar os itens GridView e ListView de forma incremental](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally).

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>Valores XAML


| Termo | Descrição |
|------|-------------|
| PhaseValue | Um número que indica a fase em que o elemento será processado. O valor padrão é 0. | 

## <a name="remarks"></a>Comentários

Se uma lista é girada rapidamente com toque ou usando a roda do mouse, então, dependendo da complexidade do modelo de dados, a lista pode não ser capaz de renderizar itens suficientemente rápido para se manter atualizada com a velocidade de rolagem. Isso é especialmente verdadeiro para um dispositivo portátil com uma CPU de baixo consumo de energia, como um telefone ou um tablet.

O atributo Phase permite renderização incremental do modelo de dados, de modo que os conteúdos possam ser priorizados e os elementos mais importantes processados primeiro. Isso permite que a lista mostre algum conteúdo para cada item se o movimento panorâmico for rápido, e renderizará mais elementos de cada modelo se houver tempo suficiente.

## <a name="example"></a>Exemplo

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

O modelo de dados descreve 4 fases:

1.  Apresenta o bloco de texto DisplayName. Todos os controles sem uma fase especificada serão considerados implicitamente como parte da fase 0.
2.  Mostra o bloco de texto prettyDate.
3.  Mostra os blocos de texto prettyFileSize e prettyImageSize.
4.  Mostra a imagem.

O atributo Phase é um recurso do [{x: Bind}](x-bind-markup-extension.md) que funciona com controles derivados de [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) e que processa incrementalmente o modelo de item para vinculação de dados. Durante a renderização de itens da lista, **ListViewBase** renderiza uma única fase para todos os itens no modo de exibição, antes de passar para a próxima fase. O trabalho de renderização é realizado em lotes de subconjuntos de tempo para que, conforme a lista é rolada, o trabalho necessário possa ser avaliado novamente, e não realizado para itens que não estão mais visíveis.

O atributo **x:Phase** pode ser especificado em qualquer elemento num modelo de dados que usa [{x:Bind}](x-bind-markup-extension.md). Quando um elemento tem uma fase diferente de 0, o elemento ficará oculto no modo de exibição (via **Opacity**, não **Visibility**) até que essa fase seja processada e as associações atualizadas. Quando um controle derivado [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) é rolado, haverá reciclagem dos modelos de item, a partir de itens que não estão mais na tela para renderizar os itens visíveis recentemente. Os elementos de interface do usuário dentro do modelo manterão seus valores antigos até que sejam associados a dados novamente. O atributo Phase faz com que essa etapa de vinculação de dados seja atrasada e, portanto, Phase precisa ocultar os elementos da interface do usuário, caso estejam obsoletos.

Cada elemento de interface do usuário pode ter apenas uma fase especificada. Em caso afirmativo, isso se aplicará a todas as associações no elemento. Se uma fase não for especificada, é considerada fase 0.

Números de fase não precisam ser contíguos e são os mesmos do valor de [**ContainerContentChangingEventArgs.Phase**](https://msdn.microsoft.com/library/windows/apps/dn298493). O evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) será gerado para cada fase antes de as associações **x:Phase** serem processadas.

O atributo Phase afeta apenas associações [{x: Bind}](x-bind-markup-extension.md), não associações [{Binding}](binding-markup-extension.md).

O atributo Phase será aplicado somente quando o modelo de item for renderizado, usando um controle que está ciente do atributo Phase. Para Windows 10, o que significa o [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). O atributo Phase não será aplicado aos modelos de dados usados em outros controles de item, ou para outros cenários, como as seções [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/br209369) ou [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843), nesses casos, todos os elementos de interface do usuário serão dados associados ao mesmo tempo.

