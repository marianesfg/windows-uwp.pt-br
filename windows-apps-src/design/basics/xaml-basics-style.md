---
author: mijacobs
title: Criar estilos personalizados
description: Este artigo aborda as noções básicas da definição de estilo dos elementos de interface do usuário em XAML
keywords: XAML, UWP, Introdução
ms.author: mijacobs
ms.date: 08/31/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 11f279de206a84e61144789ba43a268f2b896fee
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6249894"
---
# <a name="tutorial-create-custom-styles"></a>Tutorial: Criar estilos personalizados

Este tutorial mostra como personalizar a interface do usuário do app XAML. Aviso: este tutorial pode envolver um unicórnio ou não. (Ele envolve!)  

## <a name="prerequisites"></a>Pré-requisitos
* [SDK do Visual Studio 2017 e do Windows 10 (10.0.15063.468 ou posterior)](https://developer.microsoft.com/windows/downloads)

## <a name="part-0-get-the-code"></a>Parte 0: Obter o código
O ponto de partida deste laboratório é o repositório de exemplos do PhotoLab, na pasta [xaml-basics-starting-points/style/ folder](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/style). Após ter clonado/baixado o repositório, você poderá editar o projeto abrindo o PhotoLab.sln com o Visual Studio 2017.

O app PhotoLab tem duas páginas principais:

**MainPage.xaml:** exibe o modo de exibição de galeria de fotos, juntamente com algumas informações sobre cada arquivo de imagem.
![MainPage](../basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** exibe uma única foto após ela ter sido selecionada. Um menu de edição de submenu permite que a foto seja alterada, renomeada e salva.
![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="part-1-create-a-fancy-slider-control"></a>Parte 1: Criar um controle deslizante sofisticado  

A Plataforma Universal do Windows (UWP) oferece inúmeras maneiras de personalizar a aparência do app. Você tem várias opções à sua disposição, desde fontes e configurações de tipografia a efeitos de cores, gradientes e desfoque. 

Na primeira parte do tutorial, vamos incrementar alguns controles de edição de fotos. 

<figure>
    <img src="../basics/images/xaml-basics/slider-start.png" />
    <figure>*Um controle deslizante simples com estilo padrão.*</figure>
</figure>

Esses controles deslizantes são úteis, fazem tudo o que devem fazer, mas não são muito sofisticados. Vamos corrigir isso. 

O controle deslizante de exposição ajusta a exposição da imagem: deslize-o para a esquerda e a imagem ficará mais escura; deslize-o para a direita e a imagem ficará mais clara. Vamos tornar nosso controle deslizante mais atrativo, conferindo a ele um plano de fundo que vai do preto para o branco. Isso tornará o controle deslizante mais atrativo, o que é ótimo, mas também dará uma dica visual sobre a funcionalidade proporcionada pelo controle deslizante.

### <a name="customize-a-slider-control"></a>Personalizar um controle deslizante

<!-- TODO: Update folder -->
1. Depois de baixar o repositório, abra **PhotoLab.sln** no xaml-Noções básicas-iniciando-pontos/estilo/pasta e defina sua plataforma de solução para x86 ou x64 (não ARM). 

    Pressione F5 para compilar e executar o app. A primeira tela mostra uma galeria de imagens. Clique em uma imagem para ir até a página de detalhes da imagem. Nessa página, clique no botão Editar para ver os controles de edição nos quais trabalharemos. Saia do app e retorne ao Visual Studio.  

2. No painel do Gerenciador de Soluções, clique duas vezes em **DetailPage.xaml** para abri-lo. 

    ![O arquivo DetailPage.xaml no gerenciador de soluções do Visual Studio 2017.](../basics/images/xaml-basics/style-detail-page-explorer.png)

3. Use um elemento Polygon para criar uma forma de plano de fundo para o controle deslizante de exposição.

    O [namespace do Windows.XAML.Ui.Shapes](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Shapes) oferece sete formas para você escolher. Há uma elipse, um retângulo e um elemento chamado Path, que pode criar qualquer tipo de forma; sim, até mesmo um unicórnio! 
    
    <!-- TODO reduce size --> ![Um unicórnio](../basics/images/xaml-basics/unicorn.png)
    
    > **Leia sobre isso.** O artigo sobre [formas de desenho](https://docs.microsoft.com/en-us/windows/uwp/graphics/drawing-shapes) explica tudo o que você precisa saber sobre as formas XAML. 
    
    Queremos criar um widget com aparência de triângulo, algo como a forma que você veria no controle de volume de um aparelho estéreo.
    
    ![Um controle deslizante de volume](../basics/images/xaml-basics/style-volume-slider.png)
    
    Parece ser uma tarefa para a forma Polígono! Para definir um polígono, especifique um conjunto de pontos e atribua a ele um preenchimento. Vamos criar um polígono de, aproximadamente, 200 pixels de largura e 20 pixels de altura, com preenchimento de gradiente.
    
    No DetailPage.xaml, localize o código do controle deslizante de exposição e crie um elemento Polygon antes dele: 

    * Defina **Grid.Row** como "2" para colocar o polígono na mesma linha do controle deslizante de exposição. 
    * Defina a propriedade **Points** como "0,20 200,20 200,0" para definir a forma de triângulo.
    * Defina a propriedade **Stretch** como "Fill" e a propriedade **HorizontalAlignment** como "Stretch".
    * Defina **Height** como "20" e **VerticalAlignment** como "Center". 
    * Atribua ao elemento **Polygon** um preenchimento de gradiente linear.     
    * No controle deslizante de exposição, defina a propriedade **Foreground** como "Transparent" para que você possa ver o polígono. 

    **Antes**
    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```
    **Depois**
    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure" 
        Grid.Row="2" 
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    Observações:
    * Se você analisar o XAML ao redor, verá que esses elementos estão em uma grade. Colocamos o polígono na mesma linha do controle deslizante de exposição (Grid.Row="2") para que eles apareçam no mesmo lugar. Colocamos o polígono antes do controle deslizante para que o controle deslizante seja renderizado na parte superior da forma.
    * Definimos Stretch="Fill" e HorizontalAlignment="Stretch" no polígono para que o triângulo seja ajustado para preencher o espaço disponível. Se a largura do controle deslizante for diminuída ou aumentada, o polígono também diminuirá ou aumentará. 

4. Compile e execute o app. A aparência do controle ficará incrível:

    ![Um controle deslizante de exposição sofisticado](../basics/images/xaml-basics/style-exposure-slider-done.png)

5. Vamos atualizar o próximo controle deslizante: o controle deslizante de temperatura. O controle deslizante de temperatura altera a temperatura da cor da imagem. Deslizar para a esquerda torna a imagem mais azulada e deslizar para a direita torna a imagem mais amarelada.

    Usaremos outro polígono nessa forma de plano de fundo com as mesmas dimensões da anterior, mas, dessa vez, faremos com que o preenchimento exiba um gradiente que vai do azul para o amarelo, e não do preto para o branco. 

    **Antes**
    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
                
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```
    **Depois**
    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />         
                
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

6. Compile e execute o app. Agora você terá dois controles deslizantes sofisticados.

    ![Dois controles deslizantes sofisticados](../basics/images/xaml-basics/style-2sliders-done.png)

7. **Crédito extra**

    Adicione uma forma de plano de fundo ao controle deslizante de tonalidade que tem um gradiente que vai do verde para o vermelho. 

    ![Três controles deslizantes sofisticados](../basics/images/xaml-basics/style-3sliders-done.png)


Parabéns, você concluiu a Parte 1! Se você não conseguiu mais avançar ou quer ver a solução final, localize o código completo em **UWP Academy\XAML\Styling\Part1\Finish**.

 
    
## <a name="part-2-create-basic-styles"></a>Parte 2: Criar estilos básicos

Uma das vantagens dos estilos XAML é que ele pode reduzir significativamente a quantidade de código que você precisa escrever e pode facilitar muito a atualização da aparência do seu app.

Para definir um estilo, adicione um elemento [Estilo](https://msdn.microsoft.com/library/windows/apps/br208849) à propriedade [Resources](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.Resources) de um elemento que contém o controle cujo estilo você deseja definir.  Se você adicionar o estilo à propriedade **Page. Resources** , seus estilos poderão ser acessados na página inteira. Se você adicionar o estilo à propriedade **Application.Resources** no arquivo App.xaml, o estilo poderá ser acessado em todo o app.

Você pode criar estilos gerais e estilos nomeados. Um estilo nomeado deve ser aplicado explicitamente a controles específicos; um estilo geral é aplicado a qualquer controle que corresponda ao **TargetType** especificado. 

Neste exemplo, o primeiro estilo tem o atributo **x:Key**, e seu tipo de destino é **Button**. A propriedade **Style** do primeiro botão é definida para essa chave, portanto, esse é um estilo nomeado e deve ser aplicado explicitamente. O segundo estilo é aplicado automaticamente ao segundo botão porque seu tipo de destino é **Button** e o estilo não tem um atributo **x:Key**.


```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

Vamos adicionar um estilo ao app. No DetailsPage.xaml, analise os blocos de texto que aparecem ao lado dos controles deslizantes de exposição, temperatura e tonalidade. Cada um desses blocos de texto exibe o valor de um controle deslizante. Este é o bloco de texto do controle deslizante de exposição. Observe que as propriedades **Margin**, **VerticalAlignment** e **Padding** estão definidas.

```XAML
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```
Analise os outros blocos de texto; observe que essas mesmas propriedades estão definidas para os mesmos valores. Parece ser um bom candidato para um estilo...

### <a name="create-a-value-text-block-style"></a>Criar um estilo de bloco de texto de valor

<!-- TODO: add second starting point -->
1. Abra DetailsPage.xaml.

2. Localize o controle **Grid** chamado **EditControlsGrid**. Ele contém nossos controles deslizantes e caixas de texto. Observe que a grade já define um estilo para nossos controles deslizantes. 

    ```XAML
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>    
    ```
3. Crie um estilo para um **TextBlock** que defina **Margin** como "10,8,0,0", **VerticalAlignment** como "Center" e **Padding** como "0".

    **Antes**
    ```XAML
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>                           
        </Grid.Resources>
    ```

    **Depois**
    ```XAML
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
        </Grid.Resources>
    ```    

4. Vamos torná-lo um estilo nomeado para que possamos especificar a quais controles **TextBlock** ele se aplica. Defina a propriedade **X:Key** do estilo como "ValueTextBox". 

    **Antes**
    ```XAML
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
    ```    

    **Depois**
    ```XAML
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBox">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
    ```    

5. Para cada **TextBlock**, remova as propriedades **Margin**, **VerticalAlignment** e **Padding**, e defina a propriedade **Style** como "{StaticResource ValueTextBox}".

    **Antes**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```

    **Depois**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBox}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```    

    Faça essa alteração nos seis controles TextBlock associados aos controles deslizantes.

6. Compile e execute o app. Ele ficará com a mesma aparência. Mas você terá aquela sensação maravilhosa de satisfação e realização por ter escrito um código eficiente e sustentável.

<!-- TODO add new start/end points -->
Parabéns, você concluiu a Parte 2!


## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>Parte 3: Usar um modelo de controle para criar um controle deslizante sofisticado

Você se lembra como, na Parte 1, adicionamos uma forma atrás do controle deslizante para torná-lo mais atrativo?

Bem, conseguimos concluir o trabalho, mas há uma forma melhor de obter o mesmo efeito: criando um modelo de controle. 

<!-- TODO add new starting points -->
1. No painel do Gerenciador de Soluções, clique duas vezes em **DetailPage.xaml**.

2. Em seguida, usaremos o modelo de controle padrão do controle deslizante como ponto de partida. Adicione esse XAML ao elemento **Page. Resources**. (O elemento **Page. Resources** está voltado para o início da página.)

    ```XAML
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider">
        <Grid Margin="{TemplateBinding Padding}">
            <Grid.Resources>
                <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                    <Setter Property="BorderThickness" Value="0" />
                    <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                    <Setter Property="Template">
                        <Setter.Value>
                            <ControlTemplate TargetType="Thumb">
                                <Border Background="{TemplateBinding Background}"
                                            BorderBrush="{TemplateBinding BorderBrush}"
                                            BorderThickness="{TemplateBinding BorderThickness}"
                                            CornerRadius="4" />
                            </ControlTemplate>
                        </Setter.Value>
                    </Setter>
                </Style>
            </Grid.Resources>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                    <VisualState x:Name="PointerOver">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                </VisualStateGroup>
                <VisualStateGroup x:Name="FocusEngagementStates">
                    <VisualState x:Name="FocusDisengaged" />
                    <VisualState x:Name="FocusEngagedHorizontal">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                    <VisualState x:Name="FocusEngagedVertical">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            <ContentPresenter x:Name="HeaderContentPresenter"
                        x:DeferLoadStrategy="Lazy"
                        Visibility="Collapsed"
                        Foreground="{ThemeResource SliderHeaderForeground}"
                        Margin="{ThemeResource SliderHeaderThemeMargin}"
                        Content="{TemplateBinding Header}"
                        ContentTemplate="{TemplateBinding HeaderTemplate}"
                        FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
                        TextWrapping="Wrap" />
            <Grid x:Name="SliderContainer"
                        Background="{ThemeResource SliderContainerBackground}"
                        Grid.Row="1"
                        Control.IsTemplateFocusTarget="True">
                <Grid x:Name="HorizontalTemplate" MinHeight="44">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="18" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="18" />
                    </Grid.RowDefinitions>
                    <Rectangle x:Name="HorizontalTrackRect"
                                Fill="{TemplateBinding Background}"
                                Height="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Row="1"
                                Grid.ColumnSpan="3" />
                    <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1" />
                    <TickBar x:Name="TopTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                VerticalAlignment="Bottom"
                                Margin="0,0,0,4"
                                Grid.ColumnSpan="3" />
                    <TickBar x:Name="HorizontalInlineTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderInlineTickBarFill}"
                                Height="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Row="1"
                                Grid.ColumnSpan="3" />
                    <TickBar x:Name="BottomTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                VerticalAlignment="Top"
                                Margin="0,4,0,0"
                                Grid.Row="2"
                                Grid.ColumnSpan="3" />
                    <Thumb x:Name="HorizontalThumb"
                                Style="{StaticResource SliderThumbStyle}"
                                DataContext="{TemplateBinding Value}"
                                Height="24"
                                Width="8"
                                Grid.Row="0"
                                Grid.RowSpan="3"
                                Grid.Column="1"
                                FocusVisualMargin="-14,-6,-14,-6"
                                AutomationProperties.AccessibilityView="Raw" />
                </Grid>
                <Grid x:Name="VerticalTemplate" MinWidth="44" Visibility="Collapsed">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="*" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="18" />
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="18" />
                    </Grid.ColumnDefinitions>
                    <Rectangle x:Name="VerticalTrackRect"
                                Fill="{TemplateBinding Background}"
                                Width="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Column="1"
                                Grid.RowSpan="3" />
                    <Rectangle x:Name="VerticalDecreaseRect"
                                Fill="{TemplateBinding Foreground}"
                                Grid.Column="1"
                                Grid.Row="2" />
                    <TickBar x:Name="LeftTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                HorizontalAlignment="Right"
                                Margin="0,0,4,0"
                                Grid.RowSpan="3" />
                    <TickBar x:Name="VerticalInlineTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderInlineTickBarFill}"
                                Width="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Column="1"
                                Grid.RowSpan="3" />
                    <TickBar x:Name="RightTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                HorizontalAlignment="Left"
                                Margin="4,0,0,0"
                                Grid.Column="2"
                                Grid.RowSpan="3" />
                    <Thumb x:Name="VerticalThumb"
                                Style="{StaticResource SliderThumbStyle}"
                                DataContext="{TemplateBinding Value}"
                                Width="24"
                                Height="8"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="3"
                                FocusVisualMargin="-6,-14,-6,-14"
                                AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </Grid>
        </Grid>
    </ControlTemplate>
    ```

    Nossa! É muito XAML! Os modelos de controle são um recurso poderoso, mas podem ser bastante complexos; por isso, geralmente é recomendável começar com o modelo padrão. 
    
3. No **ControlTemplate** que você acabou de adicionar, localize o controle de grade chamado **HorizontalTemplate**. Essa grade define a parte do modelo que queremos alterar.

    ```XAML
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

5.  Crie um polígono exatamente igual ao polígono que você criou para o controle deslizante de exposição na Parte 1. Adicione o polígono após o fechamento da marca **RowDefinitions**. Defina **Grid.Row** como "0", **Grid.RowSpan** como "3" e **Grid.ColumnSpan** como "3". 

    **Antes**
    ```XAML
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>        
    ```

    **Depois**
    ```XAML
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>           
    ```

6. Remova a configuração **Polygon.Fill**. Defina **Fill** como "{TemplateBinding Background}". Isso é feito para que a configuração da propriedade **Background** do controle deslizante defina a propriedade **Fill** do polígono. 

    **Antes**
    ```XAML
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>           
    ```
    
    **Depois**
    ```XAML
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" 
                    Fill="{TemplateBinding Background}">
        </Polygon>           
    ```    

7. Logo após o polígono adicionado, há um retângulo chamado **HorizontalTrackRect**. Remova a configuração da propriedade **Fill** do retângulo para que este não fique visível e não bloqueie nosso polígono. (Não queremos remover completamente o retângulo porque o modelo de controle também o utiliza em elementos visuais de interação, como hover.)

    **Antes**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />          
    ```
    
    **Depois**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Você concluiu o modelo! Agora precisamos aplicá-lo aos nossos controles deslizantes. 
    
8. Vamos atualizar o controle deslizante de exposição.

    * Defina a propriedade **Template** do controle deslizante para "{StaticResource FancySliderControlTemplate}".
    * Remover a configuração Background="Transparent" do controle deslizante. 
    * Defina o plano de fundo do controle deslizante para um gradiente linear que faça a transição de preto para branco.
    * Remova o polígono de plano de fundo que criamos na Parte 1.
        
    **Antes**
    ```XAML
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure" 
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}"/>    
    ```
    
    **Depois**
    ```XAML
    <Slider Header="Exposure" 
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```        
9. Faça as mesmas atualizações no controle deslizante de temperatura.

    **Antes**
    ```XAML
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```
    
    **Depois**
    ```XAML
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```    

10. Faça as mesmas atualizações no controle deslizante de tonalidade.

    **Antes**
    ```XAML
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```
    
    **Depois**
    ```XAML
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```        

11. Compile e execute o app. 

    ![Os melhores controles deslizantes do mundo](../basics/images/xaml-basics/style-sliders-templates.png)
    
    Como você pode ver, nossas atualizações aprimoraram o posicionamento do polígono; agora a parte inferior do polígono está alinhada à parte inferior da miniatura de controle deslizante.
    
<!-- TODO correct folder -->
Parabéns, você concluiu o tutorial! Se você não conseguiu mais avançar e quer ver a solução final, localize o exemplo completo no [repositório de exemplos do aplicativo UWP](https://github.com/Microsoft/Windows-universal-samples).