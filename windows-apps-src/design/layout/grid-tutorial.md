---
author: muhsinking
Description: This tutorial walks through how to create a basic application user interface. It explains and demonstrates the use of Grid and StackPanel, two of the most common XAML elements.
title: Use Grid e StackPanel para criar um aplicativo de clima simples.
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9794a04d-e67f-472c-8ba8-8ebe442f6ef2
ms.localizationpriority: medium
ms.openlocfilehash: 0327437c809455cf191dcfc572e4a5145b73eb49
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986846"
---
# <a name="tutorial-use-grid-and-stackpanel-to-create-a-simple-weather-app"></a>Tutorial: Use Grid e StackPanel para criar um aplicativo de clima simples

Use XAML para criar o layout de um aplicativo de clima simples usando os elementos **Grid** e **StackPanel**. Com essas ferramentas que você pode criar apps com um visual excelente que funcionam em qualquer dispositivo que tenha o Windows 10 instalado. Este tutorial dura de 10 a 20 minutos.

> **APIs importantes**: [classe Grid](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid), [classe StackPanel](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.stackpanel)

## <a name="prerequisites"></a>Pré-requisitos
- Windows 10 e Microsoft Visual Studio 2015 ou posterior. (Mais recente do Visual Studio para desenvolvimento atual e segurança atualizações recomendadas) [Clique aqui para aprender a preparar-se com o Visual Studio](../../get-started/get-set-up.md).
- Conhecimento de como criar um app "Hello World" básico usando XAML e C#. Se você ainda não o tem, [clique aqui para saber como criar um aplicativo "Hello World"](https://msdn.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="step-1-create-a-blank-app"></a>Etapa 1: Criar um aplicativo em branco
1. No menu Visual Studio, selecione **Arquivo** > **Novo Projeto**.
2. No painel esquerdo da caixa de diálogo **Novo Projeto**, selecione **Visual C#** > **Windows** > **Universal** ou **Visual C++** > **Windows** > **Universal**.
3. No painel central, selecione **Aplicativo em Branco**.
4. Na caixa **Nome**, insira **WeatherPanel** e selecione **OK**.
5. Para executar o programa, selecione **Depurar** > **Iniciar Depuração** no menu ou pressione F5.

## <a name="step-2-define-a-grid"></a>Etapa 2: Definir uma grade
Em XAML, um elemento **Grid** é composto de uma série de linhas e colunas. Ao especificar a linha e a coluna de um elemento dentro de um **Grid**, você pode posicionar e espaçar outros elementos dentro de uma interface do usuário. As linhas e colunas são definidas com os elementos **RowDefinition** e **ColumnDefinition**.

Para começar a criar um layout, abra **MainPage. XAML** usando o **Gerenciador de Soluções** e substitua o elemento **Grid** gerado automaticamente por este código.

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="3*"/>
        <ColumnDefinition Width="5*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="2*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
</Grid>
```

O novo **Grid** cria um conjunto de duas linhas e colunas, que define o layout da interface do app. A primeira coluna tem o valor **Width** de "3\*", enquanto a segunda tem "5\*", dividindo o espaço horizontal entre as duas colunas em uma proporção de 3:5. Da mesma forma, as duas linhas têm o valor **Height** de "2\*" e "\*" respectivamente, assim o elemento **Grid** aloca duas vezes mais espaço para a primeira linha do que para a segunda ("\*" é o mesmo que "1\*"). Essas proporções serão mantidas mesmo que a janela seja redimensionada ou o dispositivo seja alterado.

Para saber mais sobre outros métodos de dimensionamento de linhas e colunas, consulte [Definir layouts com XAML](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml#layout-properties).

Se você executar o app agora, não verá nada além de uma página em branco, pois nenhuma das áreas de **Grid** tem conteúdo. Para mostrar o elemento **Grid** vamos dar a ele uma cor.

## <a name="step-3-color-the-grid"></a>Etapa 3: Colorir o elemento Grid
Para colorir o elemento **Grid**, adicionamos três elementos **Border**, cada um com uma cor de plano de fundo diferente. Cada um deles também é atribuído a uma linha e uma coluna no elemento **Grid** pai usando os atributos **Grid.Row** e **Grid.Column**. Os valores desses atributos têm o valor padrão 0, então você não precisa atribuí-los ao primeiro elemento **Border**. Adicione o código a seguir ao elemento **Grid** após as definições de linha e coluna.

```xml
<Border Background="#2f5cb6"/>
<Border Grid.Column ="1" Background="#1f3d7a"/>
<Border Grid.Row="1" Grid.ColumnSpan="2" Background="#152951"/>
```

Observe que, para o terceiro elemento **Border**, usamos um atributo adicional, **ColumnSpan**, que faz com que **Border** estenda ambas as colunas na linha inferior. Você pode usar **RowSpan** da mesma forma e, juntos, esses atributos permitem que você estenda um elemento sobre qualquer número de linhas e colunas. O canto superior esquerdo desse tipo de extensão é sempre o **Grid.Column** e o **Grid.Row** especificados nos atributos do elemento.

Se você executar o app, o resultado será semelhante a isto.

![Colorindo o elemento Grid](images/grid-weather-1.png)

## <a name="step-4-organize-content-by-using-stackpanel-elements"></a>Etapa 4: Organizar o conteúdo usando elementos StackPanel
**StackPanel** é o segundo elemento de interface do usuário que usaremos para criar nosso app de clima. O **StackPanel** é uma parte fundamental de muitos layouts de apps básicos, permitindo que você empilhe elementos na vertical ou na horizontal.

No código a seguir, criamos dois elementos **StackPanel** e preenchemos cada um com três **TextBlocks**. Adicione esses elementos **StackPanel** ao **Grid** abaixo dos elementos **Border** da etapa 3. Isso faz com que os elementos **TextBlock** sejam renderizados por cima do elemento **Grid** colorido que criamos anteriormente.

```xml
<StackPanel Grid.Column="1" Margin="40,0,0,0" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="Today - 64° F"/>
    <TextBlock Foreground="White" FontSize="25" Text="Partially Cloudy"/>
    <TextBlock Foreground="White" FontSize="25" Text="Precipitation: 25%"/>
</StackPanel>
<StackPanel Grid.Row="1" Grid.ColumnSpan="2" Orientation="Horizontal"
            HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="High: 66°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Low: 43°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Feels like: 63°"/>
</StackPanel>
```

No primeiro **Stackpanel**, cada **TextBlock** é empilhado verticalmente abaixo do próximo. Esse é o comportamento padrão de um StackPanel; portanto, não precisamos definir o atributo **Orientation**. No segundo StackPanel, queremos que os elementos filho sejam empilhados horizontalmente da esquerda para a direita, então definimos o atributo **Orientation** como "Horizontal". Devemos também definir o atributo **ColumnSpan** como "2", para que o texto seja centralizado sobre o elemento **Border** inferior.

Se você executar o app agora, verá algo parecido com isto.

![Adicionando StackPanels](images/grid-weather-2.png)

## <a name="step-5-add-an-image-icon"></a>Etapa 5: Adicionar um ícone de imagem

Por fim, vamos preencher a seção vazia em nosso elemento **Grid** com uma imagem que represente o clima de hoje — algo dizendo que está "parcialmente nublado".

Baixe a imagem abaixo e salve-a como um PNG denominado "parcialmente-nublado".

![Parcialmente nublado](images/partially-cloudy.PNG)

No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta **Ativos** e selecione **Adicionar** -> **Item existente...** Encontre parcialmente-nublado.png no navegador que é aberto, selecione-o e clique em **Adicionar**.

Em seguida, em **MainPage.xaml**, adicione o elemento **Image** seguinte abaixo dos StackPanels da etapa 4.

```xml
<Image Margin="20" Source="Assets/partially-cloudy.png"/>
```

Como queremos a imagem na primeira linha e coluna, não precisamos definir seus atributos **Row** ou **Column**, permitindo que eles assumam o padrão "0".

Pronto! Você criou com êxito o layout de um app de clima simples. Se você executar o app pressionando **F5**, deverá ver algo parecido com isto:

![Exemplo de painel de clima](images/grid-weather-3.PNG)

Se desejar, experimente o layout acima e explore formas diferentes de representar dados do clima.

## <a name="related-articles"></a>Artigos relacionados
Para obter uma introdução ao design de layouts de aplicativos UWP, consulte [Introdução ao design de aplicativos UWP](https://msdn.microsoft.com/windows/uwp/layout/design-and-ui-intro)

Para saber mais sobre como criar layouts dinâmicos que se adaptem a diferentes tamanhos de tela, consulte [Definir layouts de página com XAML](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml)
