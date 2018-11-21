---
author: jwmsft
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: Otimizar seu layout XAML
description: O layout pode ser uma parte cara de um aplicativo XAML&\#8212;, tanto no uso de CPU quanto na sobrecarga de memória. Aqui estão algumas etapas simples que você pode seguir para melhorar o desempenho de layout do seu app XAML.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0dcb3e49bb35902a17f829c0222c570265be8b5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7576910"
---
# <a name="optimize-your-xaml-layout"></a>Otimizar o layout XAML


**APIs importantes**

-   [**Painel**](https://msdn.microsoft.com/library/windows/apps/BR227511)

O layout é o processo de definição da estrutura visual para sua interface do usuário. O mecanismo principal para descrever o layout em XAML é por meio de painéis, que são objetos de contêiner que permitem posicionar e organizar os elementos de interface do usuário dentro deles. O layout pode ser uma parte cara de um aplicativo XAML, tanto no uso de CPU quanto na sobrecarga de memória. Aqui estão algumas etapas simples que você pode seguir para melhorar o desempenho de layout do seu aplicativo XAML.

## <a name="reduce-layout-structure"></a>Reduzir a estrutura de layout

O maior ganho no desempenho de layout vem de simplificar a estrutura hierárquica da árvore de elementos de interface do usuário. Os painéis existem na árvore visual, mas são elementos estruturais, não *elementos produtores de pixels*, como um [**Botão**](https://msdn.microsoft.com/library/windows/apps/BR209265) ou um [**Retângulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Simplificar a árvore reduzindo a quantidade de elementos não produtores de pixels costuma provocar uma grande melhora no desempenho.

Muitas interfaces do usuário são implementadas aninhando-se painéis, o que resulta em árvores profundas e complexas de painéis e elementos. É prático aninhar painéis, mas em muitos casos, a mesma interface do usuário pode ser obtida com um painel único mais complexo. Usar um único painel proporciona melhor desempenho.

### <a name="when-to-reduce-layout-structure"></a>Quando reduzir a estrutura do layout

Reduzir a estrutura do layout de maneira trivial, por exemplo, reduzindo um painel aninhado de sua página de nível superior, não possui efeitos notáveis.

Os maiores ganhos de desempenho vêm da redução de estrutura de layout que é repetida na interface do usuário, como em [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705). Esses elementos [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) usam um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), que define uma subárvore de elementos de interface do usuário que é instanciada muitas vezes. Quando a mesma subárvore está sendo duplicada muitas vezes em seu aplicativo, qualquer melhoria no desempenho dessa subárvore tem um efeito multiplicativo sobre o desempenho geral do seu aplicativo.

### <a name="examples"></a>Exemplos

Considere a interface do usuário a seguir.

![Exemplo de layout de formulário](images/layout-perf-ex1.png)

Estes exemplos mostram três formas de implementar a mesma interface do usuário. Cada opção de implementação resulta em pixels quase idênticos na tela, mas difere substancialmente nos detalhes de implementação.

Opção 1: Elementos [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) aninhados

Embora esse seja o modelo mais simples, ele usa 5 elementos do painel e resulta em uma sobrecarga significativa.

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

Opção 2: um único [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)

O [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) adiciona certa complexidade, mas usa apenas um único elemento do painel.

```xml
  <Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

Opção 3: um único [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546):

Esse painel único também é um pouco mais complexo do que a utilização de painéis aninhados, mas pode ser mais fácil de entender e manter do que um [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704).

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

Como esses exemplos mostram, há muitas maneiras de atingir a mesma interface do usuário. Você deve escolher com cuidado considerando todas as vantagens e desvantagens, inclusive o desempenho, a legibilidade e a capacidade de manutenção.

## <a name="use-single-cell-grids-for-overlapping-ui"></a>Usar grades de célula única na sobreposição de interface do usuário

Um requisito da interface do usuário comum é ter um layout em que elementos se sobrepõem uns aos outros. Geralmente, preenchimento, margens, alinhamentos e transformações são usados para posicionar os elementos dessa maneira. O controle [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) XAML é otimizado para melhorar o desempenho do layout para elementos que se sobrepõem.

**Importante**para ver a melhora, use uma única célula de [**grade**](https://msdn.microsoft.com/library/windows/apps/BR242704). Não defina [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.rowdefinitions) ou [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.columndefinitions).

### <a name="examples"></a>Exemplos

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![Texto sobreposto em um círculo](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![Dois blocos de texto em uma grade](images/layout-perf-ex3.png)

## <a name="use-a-panels-built-in-border-properties"></a>Use as propriedades de borda internas do painel

Os controles [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546) e [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378) têm propriedades de borda internas que permitem que você desenhe uma borda em torno deles sem acrescentar um elemento [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) ao XAML. As novas propriedades que oferecem suporte à borda interna são: **BorderBrush**, **BorderThickness**, **CornerRadius** e **preenchimento**. Cada um desses é um [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362), então você pode usá-los com ligações e animações. Eles são feitos para serem uma substituição total para um elemento **Border** separado.

Se a sua interface do usuário tem elementos [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) em torno desses painéis, use a borda interna ao invés, o que economiza um elemento extra na estrutura do layout de seu aplicativo. Como mencionado anteriormente, isso pode ser uma economia significativa, principalmente no caso de interface do usuário repetida.

### <a name="examples"></a>Exemplos

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## <a name="use-sizechanged-events-to-respond-to-layout-changes"></a>Use eventos **SizeChanged** para responder a mudanças de layout

A classe [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) expõe dois eventos similares para responder a alterações de layout: [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.layoutupdated) e [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.sizechanged). Você pode estar usando um desses eventos para receber notificações quando um elemento é redimensionado durante o layout. A semântica dos dois eventos é diferente e existem considerações de desempenho importantes na escolha entre eles.

Para um bom desempenho, [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.sizechanged) é quase sempre a escolha certa. **SizeChanged** tem semântica intuitiva. É gerado durante o layout quando o tamanho do [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) foi atualizado.

[**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.layoutupdated) também é gerado durante o layout, mas tem uma semântica global: é gerado em cada elemento sempre que qualquer elemento for atualizado. É comum fazer apenas processamento local no manipulador de evento. Nesse caso, o código é executado com mais frequência que o necessário. Use **LayoutUpdated** apenas se você precisar saber quando um elemento é reposicionado sem alteração de tamanho (o que é incomum).

## <a name="choosing-between-panels"></a>Escolhendo painéis

Desempenho não costuma ser uma consideração quando se escolhe entre painéis individuais. Essa escolha costuma ser feita levando em consideração qual painel fornece o comportamento de layout mais próximo à interface do usuário que você está implementando. Por exemplo, se estiver escolhendo entre [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) e [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546), você deve optar pelo painel que ofereça o mapeamento mais próximo de seu modelo mental da implementação.

Todo painel XAML é otimizado para obter bom desempenho, e todos os painéis oferecem um desempenho semelhante à interface do usuário semelhante.

