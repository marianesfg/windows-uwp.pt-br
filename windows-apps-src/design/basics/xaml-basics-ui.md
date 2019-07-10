---
title: Criar um tutorial de interface do usuário
description: Este artigo aborda as noções básicas da criação de interfaces do usuário em XAML
keywords: XAML, UWP, Introdução
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1bae8455f1062b3ad62aeac3807c6c58ae274a1b
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63794743"
---
# <a name="tutorial-create-a-user-interface"></a>Tutorial: Criar uma interface do usuário

Neste tutorial, você aprenderá como criar uma interface do usuário básica para um programa de edição de imagens: 

+ Usando as ferramentas XAML do Visual Studio, como Designer XAML, Caixa de Ferramentas, editor XAML, painel Propriedades e Estrutura de Tópicos do Documento, para adicionar controles e conteúdo à interface do usuário
+ Usando alguns painéis de layout XAML mais comuns, como RelativePanel, Grid e StackPanel.

O programa de edição de imagem tem duas páginas/telas:

A **página principal**, que mostra uma exibição de galeria de fotos, juntamente com algumas informações sobre cada arquivo de imagem.

![MainPage](images/xaml-basics/mainpage.png)

A **página de detalhes**, que exibe uma única foto após ela ter sido selecionada. Um menu de edição de submenu permite que a foto seja alterada, renomeada e salva.

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Pré-requisitos

* Visual Studio 2017: [Baixar o Visual Studio 2017 Community (gratuito)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* SDK do Windows 10 (10.0.15063.468 ou posterior):  [Baixar o SDK mais recente do Windows (gratuito)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Parte 0: obter o código inicial do github

Para este tutorial, você iniciará com uma versão simplificada da amostra do PhotoLab. 

1. Vá para [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Isso levará você para a página do GitHub para ver a amostra. 
2. Em seguida, você precisará clonar ou baixar a amostra. Clique no botão **Clonar ou baixar**. Um submenu será exibido.
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>O menu <b>Clonar ou baixar</b> na página do GitHub da amostra do PhotoLab.</figcaption>
    </figure>

    **Se você não estiver familiarizado com o GitHub:**
    
    a. Clique em **Baixar ZIP** e salve o arquivo localmente. Isso baixa um arquivo .zip que contém todos os arquivos de projeto de que você precisa.
    b. Extraia o arquivo. Use o Explorador de Arquivos para navegar até o arquivo .zip que você acabou de baixar, clique com o botão direito do mouse e selecione **Extrair Tudo...** . c. Navegue até sua cópia local da amostra e acesse o diretório `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.    

    **Se você estiver familiarizado com o GitHub:**

    a. Clone a ramificação mestre do repositório localmente.
    b. Navegue até o diretório `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`.

3. Abra o projeto clicando em `Photolab.sln`.

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>Parte 1: Adicionar um TextBlock usando o Designer XAML

O Visual Studio oferece várias ferramentas para facilitar a criação da interface do usuário XAML. O Designer XAML permite arrastar controles na superfície de design e ver sua aparência antes de executar o aplicativo. O painel Propriedades permite exibir e definir todas as propriedades do controle que estão ativas no designer. A Estrutura de Tópicos do Documento mostra a estrutura pai-filho da árvore visual XAML da interface do usuário. O editor XAML permite inserir e modificar a marcação XAML diretamente.

Esta é a interface do usuário do Visual Studio com as ferramentas identificadas.

![Layout do Visual Studio](images/xaml-basics/visual-studio-tools.png)

Cada uma dessas ferramentas facilita a criação da interface do usuário. Por isso, usaremos todas elas neste tutorial. Você começará usando o Designer XAML para adicionar um controle. 

**Adicionar um controle usando o Designer XAML:**

1. Clique duas vezes em **MainPage.xaml** no Gerenciador de Soluções para abri-lo. A página principal do aplicativo será exibida sem elementos de interface do usuário adicionados.

2. Antes de prosseguir, é necessário fazer alguns ajustes no Visual Studio.

    - Verifique se a sua Plataforma de Solução está definida como x86 ou x64, não como ARM.
    - Defina a página principal Designer XAML para mostrar a visualização da área de trabalho de 13,3 pol.

    Você verá as duas configurações próximas à parte superior da janela, conforme mostrado aqui.

    ![Configurações do VS](images/xaml-basics/layout-vs-settings.png)

    Você pode executar o aplicativo agora, mas não verá muitos detalhes. Vamos adicionar alguns elementos de interface do usuário para tornar o aplicativo mais interessante.

3. Na Caixa de Ferramentas, expanda **Controles comuns do XAML** e localize o controle [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock). Arraste um TextBlock para a área de design perto do canto superior esquerdo da página.

    O TextBlock é adicionado à página e o designer define algumas propriedades com base em seu melhor palpite no layout desejado. Um realce azul é exibido em torno do TextBlock para indicar que ele é agora o objeto ativo. Observe as margens e outras configurações adicionadas pelo designer. O XAML terá a aparência a seguir. Não se preocupe se ele não tiver a formatação exata mostrada aqui. Nós abreviamos aqui para facilitar a leitura.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    Nas próximas etapas, você atualizará esses valores.

4. No painel Propriedades, altere o valor de nome do TextBlock de **textBlock** para **TitleTextBlock**. (Assegure que o TextBlock ainda é o objeto ativo.)

5. Em **Comum**, altere o valor de texto para **Collection**.

    ![Propriedades do TextBlock](images/xaml-basics/text-block-properties.png)

    No editor XAML, o XAML agora terá a aparência a seguir.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. Para posicionar o TextBlock, primeiro remova os valores de propriedade adicionados pelo Visual Studio. Em Estrutura de Tópicos do Documento, clique com o botão direito do mouse em **TitleTextBlock** e selecione **Layout > Redefinir Tudo**.

![Estrutura de Tópicos do Documento](images/xaml-basics/doc-outline-reset.png)

7. No painel Propriedades, insira **margin** na caixa de pesquisa para localizar facilmente a propriedade **Margin**. Defina as margens esquerda e inferior para 24.

    ![Margens do TextBlock](images/xaml-basics/margins.png)

    As margens fornecem o posicionamento mais básico de um elemento na página. Elas são úteis para ajuste do layout, mas o uso de valores de margem grandes, como os adicionados pelo Visual Studio, dificulta a adaptação da interface do usuário a vários tamanhos de tela e, portanto, deve ser evitado.

    Para obter mais informações, confira [Alinhamento, margens e preenchimento](../layout/alignment-margin-padding.md).

8. No painel da Estrutura de Tópicos do Documento, clique com o botão direito do mouse em **TitleTextBlock** e, em seguida, selecione **Editar Estilo > Aplicar Recurso > TitleTextBlockStyle**. Isso aplica um estilo definido pelo sistema ao texto do título.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. No painel Propriedades, insira **textwrapping** na caixa de pesquisa para localizar facilmente a propriedade **TextWrapping**. Clique no _marcador de propriedade_ da propriedade **TextWrapping** para abrir o respectivo menu. (O _marcador de propriedade_ é o símbolo de caixa pequena à direita de cada valor da propriedade. O _marcador de propriedade_ é preto para indicar que a propriedade é definida como um valor não padrão.) No menu **Propriedade**, selecione **Redefinir** para redefinir a propriedade TextWrapping.

    O Visual Studio adiciona essa propriedade, mas ela já está definida no estilo aplicado, portanto, você não precisa dela aqui.

Você adicionou a primeira parte da interface do usuário ao aplicativo. Execute o aplicativo agora para ver sua aparência.

Você deve ter observado que, no Designer XAML, o aplicativo exibiu texto branco em uma tela de fundo preta, mas quando você o executou, ele mostrou texto preto em uma tela de fundo branca. Isso ocorre porque o Windows tem um tema Escuro e um tema Claro e o tema padrão varia de acordo com o dispositivo. Em um computador, o tema padrão é Claro. Você pode clicar no ícone de engrenagem na parte superior do Designer XAML para abrir as Configurações de Visualização de Dispositivo e alterar o tema para Claro para que o aplicativo no Designer XAML tenha a mesma aparência exibida no computador.

> [!NOTE]
> Nesta parte do tutorial, você adicionou um controle usando o recurso de arrastar e soltar. Você também pode adicionar um controle clicando duas vezes nele na Caixa de Ferramentas. Faça um teste e veja as diferenças geradas pelo Visual Studio no XAML.

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>Parte 2: Adicionar um controle GridView usando o editor XAML

Na Parte 1, você viu um pouco do uso do Designer XAML e de algumas ferramentas fornecidas pelo Visual Studio. Aqui, você usará o editor XAML para trabalhar diretamente com a marcação XAML. À medida que você se familiarizar com o XAML, poderá chegar à conclusão de que essa é a maneira mais eficiente de trabalhar.

Primeiro, você substituirá o layout raiz [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) por [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). O RelativePanel facilita a reorganização dos blocos de interface do usuário referentes ao painel ou a outras partes da interface do usuário. Você verá sua utilidade no tutorial [Layout adaptável XAML](xaml-basics-adaptive-layout.md). 

Em seguida, você adicionará um controle [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) para exibir os dados.

**Adicionar um controle usando o editor XAML**

1. No editor XAML, altere a raiz **Grid** para **RelativePanel**.

    **Antes**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **Depois**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    Para obter mais informações sobre layout usando um **RelativePanel**, confira [Painéis de layout](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Abaixo do elemento **TextBlock**, adicione um **controle GridView** chamado 'ImageGridView'. Defina as _propriedades anexadas_ de **RelativePanel** para colocar o controle abaixo do texto do título e ampliá-lo ao longo de toda a largura da tela.

    **Adicionar este XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Depois do TextBlock**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    Para obter mais informações sobre as propriedades anexadas de painel, confira [Painéis de layout](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Para que **GridView** mostre tudo, você precisa atribuir a ele uma coleção de dados a ser exibida. Abra MainPage.xaml.cs e localize o método **GetItemsAsync**. Esse método popula uma coleção chamada Images, que é uma propriedade que adicionamos a MainPage.

    Após o loop **foreach** em **GetItemsAsync**, adicione esta linha de código.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    Isso definirá a propriedade **ItemsSource** do GridView para a coleção **Images** do aplicativo e atribuirá ao **GridView** algo para exibir.

Esse é um bom lugar para executar o aplicativo e ter certeza de que tudo está funcionando. Ele terá a aparência a seguir.

![Ponto de verificação 1 da interface do usuário do aplicativo](images/xaml-basics/layout-0.png)

Você observará que o aplicativo não está mostrando imagens ainda. Por padrão, ele mostra o valor ToString do tipo de dados que está na coleção. Em seguida, você criará um modelo de dados para definir como os dados serão mostrados.

> [!NOTE]
> Obtenha mais informações sobre o layout usando um **RelativePanel** no artigo [Painéis de layout](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Dê uma olhada e teste alguns layouts diferentes definindo as propriedades anexadas de RelativePanel no **TextBlock** e no **GridView**.

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>Parte 3: Adicionar um DataTemplate para exibir os dados

Agora, você criará um [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) que informa ao GridView como exibir os dados. Para obter uma explicação completa dos modelos de dados, confira [Contêineres e modelos de item](../controls-and-patterns/item-containers-templates.md).

Por enquanto, você só adicionará espaços reservados para ajudá-lo a criar o layout desejado. No tutorial [Vinculação de dados XAML](../../data-binding/xaml-basics-data-binding.md), você substituirá esses espaços reservados por dados reais da classe **ImageFileInfo**. Abra o arquivo ImageFileInfo.cs agora se quiser ver a aparência do objeto de dados.

**Adicionar um modelo de dados a uma exibição de grade**

1. Abra MainPage.xaml.

2. Para mostrar a classificação, use o controle **RadRating** do pacote NuGet [Telerik UI for UWP](https://github.com/telerik/UI-For-UWP). Adicione uma referência ao namespace XAML que especifica o namespace dos controles Telerik. Coloque isso na tag de abertura **Page**, logo após as outras entradas 'xmlns:'.

    **Adicionar este XAML**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Após a última entrada 'xmlns:'**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    Para obter mais informações sobre namespaces XAML, confira [Namespaces XAML e mapeamento de namespace](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-namespaces-and-namespace-mapping).

3. Na Estrutura de Tópicos do Documento, clique com o botão direito do mouse em **ImageGridView**. No menu de contexto, selecione **Editar Modelos Adicionais > Editar Itens Gerados (ItemTemplate) > Criar Vazio...** . A caixa de diálogo **Criar Recurso** abre.

4. Na caixa de diálogo, altere o valor de nome (chave) para **ImageGridView_DefaultItemTemplate** e clique em **OK**.

    Várias coisas acontecem quando você clica em **OK**.

    - Um **DataTemplate** é adicionado à seção Page.Resources do MainPage.xaml.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - O escopo da Estrutura de Tópicos do Documento é definido para esse **DataTemplate**.

        Ao terminar de criar o modelo de dados, você poderá clicar na seta para cima no canto superior esquerdo da Estrutura de Tópicos do Documento para retornar ao escopo da página.

    - A propriedade **ItemTemplate** do GridView é definida para o recurso **DataTemplate**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. No recurso **ImageGridView_DefaultItemTemplate**, atribua à **grade** raiz uma altura e uma largura igual a **300** e uma margem de **8**. Em seguida, adicione duas linhas e defina a altura da segunda linha como **Auto**.

    **Antes**
    ```xaml
    <Grid/>
    ```

    **Depois**
    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    Para obter mais informações sobre layouts de grade, confira [Painéis de layout](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid).

6. Adicione controles à grade.

    a. Adicione um controle **Image** à primeira linha de grade. É nesse local que a imagem aparecerá, mas, por enquanto, você usará o logotipo de loja do aplicativo como um espaço reservado.

    b. Adicione os controles **TextBlock** para exibir o nome, o tipo de arquivo e as dimensões da imagem. Para isso, use os controles **StackPanel** para organizar os blocos de texto.

    Para obter mais informações sobre o layout **StackPanel**, confira [Painéis de layout](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)

    c. Adicione o controle **RadRating** ao **StackPanel** externo (vertical). Coloque-o após o **StackPanel** interno (horizontal).

    **O modelo final**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <telerikInput:RadRating Value="3"
                                    IsReadOnly="True">
                <telerikInput:RadRating.FilledIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="SolidStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.FilledIconContentTemplate>
                <telerikInput:RadRating.EmptyIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="OutlineStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.EmptyIconContentTemplate>
            </telerikInput:RadRating>

        </StackPanel>
    </Grid>
    ```

Execute o aplicativo agora para ver o **GridView** com o modelo de item que você acabou de criar. No entanto, talvez você não veja o controle de classificação, porque ele tem estrelas brancas em uma tela de fundo branca. O próximo passo será alterar a cor da tela de fundo.

![Ponto de verificação 2 da interface do usuário do aplicativo](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Parte 4: Modificar o estilo de contêiner do item

O modelo de controle de um item contém os elementos visuais que exibem o estado, como seleção, posição do ponteiro e foco. Esses elementos visuais são renderizados acima ou abaixo do modelo de dados. Aqui, você modificará as propriedades **Background** e **Margin** do modelo de controle para atribuir aos itens de **GridView** uma tela de fundo cinza.

**Modificar o contêiner do item**

1. Na Estrutura de Tópicos do Documento, clique com o botão direito do mouse em **ImageGridView**. No menu de contexto, selecione **Editar Modelos Adicionais > Editar Contêiner de Itens Gerados (ItemContainerStyle) > Editar uma Cópia...** . A caixa de diálogo **Criar Recurso** abre.

2. Na caixa de diálogo, altere o valor de nome (chave) para **ImageGridView_DefaultItemContainerStyle** e clique em **OK**.

    Uma cópia do estilo padrão é adicionada à seção **Page.Resources** do XAML.

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="True"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="FocusVisualPrimaryBrush" Value="{ThemeResource GridViewItemFocusVisualPrimaryBrush}"/>
        <Setter Property="FocusVisualPrimaryThickness" Value="2"/>
        <Setter Property="FocusVisualSecondaryBrush" Value="{ThemeResource GridViewItemFocusVisualSecondaryBrush}"/>
        <Setter Property="FocusVisualSecondaryThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    O estilo padrão **GridViewItem** define muitas propriedades. Você sempre deve começar com uma cópia do estilo padrão e modificar apenas as propriedades necessárias. Caso contrário, os visuais poderão não aparecer conforme o esperado porque algumas propriedades não estarão definidas corretamente.

    Como na etapa anterior, a propriedade **ItemContainerStyle** do GridView é definida para o novo recurso **Style**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. Altere o valor da propriedade **Background** para **Gray**.

    **Antes**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **Depois**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. Altere o valor da propriedade **Margin** para **8**.

    **Antes**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **Depois**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

Execute o aplicativo e confira sua aparência agora. Redimensionar a janela do aplicativo. O **GridView** se encarrega da reorganização das imagens para você, mas, em algumas larguras, há muito espaço no lado direito da janela do aplicativo. Seria melhor se as imagens fossem centralizadas. Vamos cuidar disso a seguir.

![Ponto de verificação 3 da interface do usuário do aplicativo](images/xaml-basics/layout-2.png)

> [!Note]
> Se você quiser experimentar, tente definir as propriedades Background e Margin para valores diferentes e veja o efeito disso.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Parte 5: Aplicar alguns ajustes finais ao layout

Para centralizar as imagens na página, você precisa ajustar o alinhamento da grade na página. Ou você precisa ajustar o alinhamento das imagens no **GridView**? Isso é importante? Vamos ver.

Para obter mais informações sobre alinhamento, confira [Alinhamento, margens e preenchimento](../layout/alignment-margin-padding.md).

(Você pode tentar definir a propriedade **Background** do **GridView** para sua cor favorita nesta etapa. Assim, você conseguirá ver com mais clareza o que está acontecendo com o layout.)

**Modificar o alinhamento das imagens**

1. No **GridView**, defina a propriedade **HorizontalAlignment** para **Center**.

    **Antes**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **Depois**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}" 
                  HorizontalAlignment="Center"/>
    ```

2. Execute o aplicativo e redimensione a janela. Role a tela para baixo para ver mais imagens.

    As imagens serão centralizadas, o que proporcionará uma aparência melhor ao aplicativo. No entanto, a barra de rolagem está alinhada à borda de **GridView**, e não à borda da janela. Para corrigir isso, centralize as imagens no **GridView** em vez de centralizar o **GridView** na página. Dá um pouco mais de trabalho, mas a aparência ficará melhor no final.

3. Remova a configuração **HorizontalAlignment** da etapa anterior.

4. Na Estrutura de Tópicos do Documento, clique com o botão direito do mouse em **ImageGridView**. No menu de contexto, selecione **Editar Modelos Adicionais > Editar Layout dos Itens (ItemsPanel) > Editar uma Cópia...** . A caixa de diálogo **Criar Recurso** abre.

5. Na caixa de diálogo, altere o valor de nome (chave) para **ImageGridView_ItemsPanelTemplate** e clique em **OK**.

    Uma cópia do **ItemsPanelTemplate** padrão é adicionada à seção **Page.Resources** do XAML. (Como antes, o **GridView** é atualizado para fazer referência a esse recurso.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    Assim como você usou vários painéis para dispor o layout dos controles no aplicativo, o **GridView** tem um painel interno que gerencia o layout de seus itens. Agora que você tem acesso a esse painel (o **ItemsWrapGrid**), pode modificar suas propriedades para alterar o layout dos itens no **GridView**.

6. No **ItemsWrapGrid**, defina a propriedade **HorizontalAlignment** para **Center**.

    **Antes**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **Depois**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. Execute o aplicativo e redimensione a janela novamente. Role a tela para baixo para ver mais imagens.

![Ponto de verificação 4 da interface do usuário do aplicativo](images/xaml-basics/layout-3.png)

Agora, a barra de rolagem está alinhada à borda da janela. Bom trabalho! Você criou uma interface do usuário básica para seu aplicativo.

## <a name="going-further"></a>Aprofundamento

Agora que você criou a interface do usuário básica, confira estes outros tutoriais, também baseados na amostra do PhotoLab: 

* Adicionar imagens reais e dados no [tutorial de associação de dados XAML](../../data-binding/xaml-basics-data-binding.md).
* Faça com que a interface do usuário se adapte a diferentes tamanhos de tela no [tutorial de layout adaptável XAML](xaml-basics-adaptive-layout.md).


## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtenha a versão final da amostra do PhotoLab

Este tutorial não é compilado para o aplicativo de edição de fotos completo. Portanto, confira a [versão final](https://github.com/Microsoft/Windows-appsample-photo-lab) para ver outros recursos, como animações personalizadas e suporte telefônico.

