---
title: Tutorial Criar layouts adaptáveis
description: Este artigo aborda as noções básicas do layout adaptável no XAML
keywords: XAML, UWP, Introdução
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7b444a11ab032034976d2f1b269bd10a89bf339e
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7695085"
---
# <a name="tutorial-create-adaptive-layouts"></a>Tutorial: Criar layouts adaptáveis

Este tutorial aborda as noções básicas do uso de recursos de layout adaptável e personalizado do XAML, que permitem a você criar apps que se adaptem a qualquer dispositivo. Você aprenderá como criar um novo DataTemplate, adicionar pontos de ajuste de janela e personalizar o layout do app usando os elementos VisualStateManager e AdaptiveTrigger. Usaremos essas ferramentas para otimizar um programa de edição de imagens para telas de dispositivo menores. 

A imagem do programa de edição em que você trabalhará tem duas páginas/telas:

A **página principal**, que oferece um modo de exibição de galeria de fotos, juntamente com algumas informações sobre cada arquivo de imagem.

![MainPage](../basics/images/xaml-basics/mainpage.png)

A **página de detalhes**, exibe uma única foto após ela ter sido selecionada. Um menu de edição de submenu permite que a foto seja alterada, renomeada e salva.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Pré-requisitos

* Visual Studio 2017: [Baixar o Visual Studio 2017 Community (gratuito)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* SDK do Windows 10 (10.0.15063.468 ou posterior): [Baixe o mais recente SDK do Windows (gratuito)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Emulador do Windows em celulares: [Baixar o emulador do Windows 10 mobile (gratuito)](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>Parte 0: Obter o código inicial do github

Para este tutorial, você iniciará com uma versão simplificada do exemplo do PhotoLab. 

1. Vá para [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Isso levará você para a página do GitHub para ver o exemplo. 
2. Em seguida, você precisará clonar ou baixar a amostra. Clique no botão **Clonar ou download**. Um submenu aparece.
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>O menu <b>Clonar ou baixar</b> na página do GitHub do exemplo do Photo lab.</figcaption>
    </figure>

    **Se você não estiver familiarizado com o GitHub:**
    
    a. Clique em **Baixar ZIP** e salve o arquivo localmente. Isso baixa um arquivo .zip que contém todos os arquivos de projeto de que você precisa.
    b. Extraia o arquivo. Use o Explorador de Arquivos para navegar até o arquivo .zip que você acabou de baixar, clique com o botão direito e selecione **Extrair Tudo...** c. Navegue até sua cópia local do exemplo e acesse o diretório `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.    

    **Se você estiver familiarizado com o GitHub:**

    a. Clone a ramificação mestre do repositório localmente.
    b. Navegue até o diretório `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`.

3. Abra o projeto clicando em `Photolab.sln`.

## <a name="part-1-run-the-mobile-emulator"></a>Parte 1: Executar o emulador móvel

Na barra de ferramentas do Visual Studio, verifique se a Plataforma de Solução foi definida para x86 ou x64, e não para ARM. Em seguida, altere o dispositivo de destino da máquina local para um dos emuladores móveis que você instalou (por exemplo, emulador móvel 10.0.15063 WVGA 5 polegadas 1 GB). Tente executar o app Galeria de Fotos no emulador móvel que você selecionou pressionando **F5**.

Assim que o app for iniciado, você provavelmente perceberá que, embora o app funcione, sua aparência não é ideal em um visor tão pequeno. O elemento Grid flexível tenta se acomodar no espaço limitado na tela, reduzindo o número de colunas exibidas, mas ficamos à mercê de um layout que parece não ter sido pensado nem adaptado para um visor pequeno.

![Layout para dispositivos móveis: depois](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>Parte 2: Compilar um layout para dispositivos móveis adaptado
Para que esse app tenha uma boa aparência em dispositivos menores, vamos criar um conjunto separado de estilos em nossa página XAML que será usado somente se for detectado um dispositivo móvel.

### <a name="create-a-new-datatemplate"></a>Criar um novo DataTemplate
Vamos para adaptar o modo de exibição de galeria do app criando um novo DataTemplate para as imagens. Abra MainPage.xaml no Gerenciador de Soluções e adicione o seguinte código às marcas **Page.Resources**.

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Esse modelo de galeria economiza espaço na tela, eliminando a borda em torno das imagens e livrando-se dos metadados de imagem (nome de arquivo, classificações etc.) abaixo de cada miniatura. Em vez disso, mostramos cada miniatura como um quadrado simples.

### <a name="add-metadata-to-a-tooltip"></a>Adicionar metadados a uma dica de ferramenta
Queremos ainda que o usuário seja capaz de acessar os metadados de cada imagem, portanto, adicionaremos uma dica de ferramenta para cada item de imagem. Adicione o seguinte código às marcas **Image** do DataTemplate que você acabou de criar.

```XAML
<Image ...>

    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
</Image>
```

Isso mostrará o título, o tipo de arquivo e as dimensões de uma imagem quando você passar o mouse sobre a miniatura (ou mantiver pressionado o cursor em uma tela sensível ao toque).

### <a name="add-a-visualstatemanager-and-statetrigger"></a>Adicionar um VisualStateManager e um StateTrigger

Agora criamos um novo layout para nossos dados, mas o app não tem como saber quando usar esse layout sobre os estilos padrão. Para corrigir isso, será necessário adicionar um **VisualStateManager**. Adicione o código a seguir ao elemento raiz da página **RelativePanel**.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Isso adicionará um novo **VisualState** e **StateTrigger**, que será acionado quando o app detectar que ele está sendo executado em um dispositivo móvel (a lógica dessa operação pode ser encontrada em MobileScreenTrigger.cs, que é fornecido para você no diretório do PhotoLab). Quando o **StateTrigger** for iniciado, o app usará qualquer atributo de layout designado a esse **VisualState**.

### <a name="add-visualstate-setters"></a>Adicionar setters VisualState
Em seguida, usaremos os setters **VisualState** para informar ao **VisualStateManager** quais atributos serão aplicados quando o estado for acionado. Cada setter destina-se a uma propriedade de um elemento XAML e a define como o valor fornecido. Adicione este código ao **VisualState** móvel que você acabou de criar, abaixo do elemento **VisualState.StateTriggers**. 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

Esses setters definem o **ItemTemplate** da galeria de imagens para o novo **DataTemplate** que criamos na primeira parte e alinham a barra de comandos e o controle deslizante de zoom à parte inferior da tela, para que seja mais fácil de acessá-los com seu dedo polegar na tela de um telefone celular.

### <a name="run-the-app"></a>Executar o app
Agora, tente executar o app usando um emulador móvel. O novo layout é exibido com êxito? Você verá uma grade de miniaturas conforme mostrado abaixo. Se o layout antigo ainda estiver aparecendo, talvez haja algum erro de digitação no código do **VisualStateManager**.

![Layout para dispositivos móveis: depois](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>Parte 3: Adaptar-se a vários tamanhos de janela em um único dispositivo
A criação de um novo layout adaptado elimina a complexidade do design responsivo para os dispositivos móveis, mas e os desktops e os tablets? O app pode ter uma boa aparência em tela inteira, mas se o usuário diminui a janela, a interface pode ficar estranha. Podemos garantir que o usuário final sempre terá um boa experiência usando o **VisualStateManager** para que haja a adaptação aos vários tamanhos de janela em um único dispositivo.

![Janela pequena: antes](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>Adicionar pontos de ajuste de janela
A primeira etapa é definir os "pontos de ajuste" nos quais os diferentes **VisualStates** serão acionados. Abra App.xaml no Gerenciador de Soluções e adicione o código a seguir entre as marcas **Application**.

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

Isso nos oferece três pontos de ajuste, que nos permitem criar novos **VisualStates** para três intervalos de tamanhos de janela:
+ Pequeno (0 a 640 pixels de largura)
+ Médio (641 a 1007 pixels de largura)
+ Grande (superior a 1007 pixels de largura)

### <a name="create-new-visualstates-and-statetriggers"></a>Criar novos VisualStates e StateTriggers
Em seguida, criamos o **VisualStates** e o **StateTriggers** correspondentes a cada ponto de ajuste. No MainPage.xaml.cpp, adicione o código a seguir ao **VisualStateManager** criado na Parte 2.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>Adicionar setters
Por fim, adicione esses setters ao estado **SmallWindow**.

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>

</VisualState>

```

Esses setters aplicarão o **DataTemplate** móvel e os estilos ao app de desktop sempre que o visor tiver menos de 641 pixels de largura. Eles também ajustam o controle deslizante de zoom para que se adaptem da melhor maneira à tela pequena.

### <a name="run-the-app"></a>Executar o app

Na barra de ferramentas do Visual Studio, defina o dispositivo de destino para **Computador Local** e executar o app. Quando o app for carregado, tente alterar o tamanho da janela. Quando você reduzir a janela, verá o app alternar para o layout de dispositivo móvel criado na Parte 2.

![Janela pequena: depois](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Aprofundamento

Agora que você concluiu este laboratório, tem conhecimento suficiente sobre layout adaptável para fazer novas experiências por conta própria. Tente adicionar um controle de classificação à dica de ferramenta somente para o dispositivo móvel adicionada anteriormente. Ou, se quiser um desafio maior, tente otimizar o layout das telas maiores (considere as telas de TV ou o Surface Studio)

Se você não conseguir mais avançar, busque mais orientações nestas seções de [Definir layouts de página com o XAML](../layout/layouts-with-xaml.md).

+ [Estados visuais e gatilhos de estado](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [Layouts personalizados](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#tailored-layouts)

Como alternativa, se você quiser saber mais sobre como o app inicial de edição de fotos foi criado, confira estes tutoriais sobre [interfaces do usuário](../basics/xaml-basics-ui.md) XAML e [vinculação de dados](../../data-binding/xaml-basics-data-binding.md).

## <a name="get-the-final-version-of-the-photolab-sample"></a>Baixe a versão final do exemplo PhotoLab

Este tutorial não é compilado para o app de edição de fotos completo; portanto, confira a [versão final](https://github.com/Microsoft/Windows-appsample-photo-lab) para ver outros recursos, como animações personalizadas e suporte por telefone.