---
Description: Explica como definir um elemento ResourceDictionary e os recursos inseridos, e também como os recursos XAML se relacionam com outros recursos definidos como parte do aplicativo ou do pacote do aplicativo.
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referências de recursos de ResourceDictionary e XAML
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 31b4a02f3307909f325b71cdc0540d44054adf4c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73061979"
---
# <a name="resourcedictionary-and-xaml-resource-references"></a>Referências de recursos de ResourceDictionary e XAML

 

Você pode definir a interface do usuário ou recursos para seu aplicativo usando XAML. Os recursos geralmente são definições de algum objeto que você espera usar mais de uma vez. Para fazer referência a um recurso XAML posteriormente, você precisa especificar uma chave para um recurso XAML que aja como o nome do recurso. Você pode fazer referência a um recurso em uma aplicativo ou em qualquer página XAML dentro dele. É possível definir seus recursos usando um elemento [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) do XAML do Windows Runtime. Em seguida, você pode referenciar seus recursos usando uma extensão de marcação [StaticResource](../../xaml-platform/staticresource-markup-extension.md) ou [ThemeResource](../../xaml-platform/themeresource-markup-extension.md).

Os elementos XAML que podem ser declarados com mais frequência como recursos XAML incluem [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style), [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate), componentes de animação e subclasses [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush). Aqui, explicaremos como definir um elemento [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) e os recursos inseridos, e também como os recursos XAML se relacionam com outros recursos definidos como parte do aplicativo ou do pacote do aplicativo. Também explicaremos as características avançadas do dicionário de recursos como [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) e [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

**Pré-requisitos**

Presumimos que você entende a marcação XAML e leu a [Visão geral do XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview).

## <a name="define-and-use-xaml-resources"></a>Definir e usar recursos XAML

Recursos XAML são objetos referenciados na marcação mais de uma vez. Os recursos são definidos em um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), normalmente em um arquivo separado ou na parte superior da página de marcação, como esta.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

Neste exemplo:

-   `<Page.Resources>…</Page.Resources>` - Define o dicionário de recursos.
-   `<x:String>` - Define o recurso com a chave "greeting".
-   `{StaticResource greeting}` – pesquisa o recurso com a chave "greeting", que é atribuída à propriedade [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) de [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

> **Observação**&nbsp;&nbsp;Não confunda os conceitos relacionados a [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) com a ação de compilação **Resource**, arquivos de recursos (.resw) ou outros "recursos" que são abordados no contexto de estruturação do projeto de código que produz seu pacote do aplicativo.

Os recursos não precisam ser cadeias de caracteres; eles podem ser qualquer objeto compartilhável, como estilos, modelos, pincéis e cores. Entretanto, controles, formas e outros [FrameworkElements](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) não são compartilháveis, então não podem ser declarados como recursos reutilizáveis. Para saber mais sobre compartilhamento, consulte a seção [Recursos XAML devem ser compartilháveis](#xaml-resources-must-be-shareable) mais adiante neste tópico.

Aqui, um pincel e uma cadeia de caracteres são declarados como recursos e usados pelos controles em uma página.

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

Todos os recursos precisam ter uma chave. Normalmente, essa chave é uma cadeia de caracteres definida com `x:Key="myString"`. Entretanto, há outras maneiras de especificar uma chave:

-   [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) e [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) exigem um **TargetType** e usarão o **TargetType** como a chave se [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) não for especificado. Nesse caso, a chave é o objeto Type mesmo, não uma cadeia de caracteres. (Consulte exemplos abaixo.)
-   Os recursos [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) com um **TargetType** usarão o **TargetType** como a chave se [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) não for especificado. Nesse caso, a chave é o objeto Type mesmo, não uma cadeia de caracteres.
-   Pode ser usado [x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute) em vez de [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute). Entretanto, x:Name também gera um campo code-behind para o recurso. Como resultado, x:Name é menos eficiente do que x:Key, pois esse campo precisa ser inicializado quando a página é carregada.

A [extensão de marcação StaticResource](../../xaml-platform/staticresource-markup-extension.md) pode recuperar recursos apenas com um nome de cadeia de caracteres ([x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) ou [x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute)). Porém, a estrutura XAML também procura recursos de estilo implícito (aqueles que usam **TargetType** em vez de x:Key ou x:Name) quando ela decide o estilo e o modelo a serem usados para um controle que não definiu as propriedades [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.style) e [ContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) ou [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate).

Aqui, [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) tem uma chave implícita **typeof(Button)** e, desde que [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) na parte inferior da página não especifique uma propriedade [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.style), ele procura um estilo com a chave **typeof(Button)** :

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Background" Value="Red"/>
        </Style>
    </Page.Resources>
    <Grid>
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
    </Grid>
</Page>
```

Para saber mais sobre estilos implícitos e como eles funcionam, consulte [Aplicando estilos a controles](xaml-styles.md) e [Modelos de controle](control-templates.md).

## <a name="look-up-resources-in-code"></a>Pesquisar recursos no código

Você acessa membros do dicionário de recursos como qualquer outro dicionário.

> [!WARNING]
> Quando você executa uma pesquisa de recursos no código, somente os recursos no dicionário `Page.Resources` são examinados. Ao contrário da [extensão de marcação StaticResource](../../xaml-platform/staticresource-markup-extension.md), o código não fará fallback para o dicionário `Application.Resources` se os recursos não forem encontrados no primeiro dicionário.

 

Este exemplo mostra como recuperar o recurso `redButtonStyle` do dicionário de recursos de uma página:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```

Para pesquisar recursos em todo o aplicativo no código, use **Application.Current.Resources** para obter o dicionário de recursos do aplicativo, conforme é mostrado aqui.

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```

Você também pode adicionar um recurso do aplicativo no código.

Há dois coisas para ter em mente ao fazer isso:

-   Primeiro, você precisa adicionar os recursos antes que qualquer página tente usar o recurso.
-   Segundo, você não pode adicionar recursos ao construtor do aplicativo.

Você poderá evitar os dois problemas se adicionar o recurso no método [Application.OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched), desta forma.

```CSharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```

## <a name="every-frameworkelement-can-have-a-resourcedictionary"></a>Cada FrameworkElement pode ter um ResourceDictionary

[FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) é uma classe base da qual os controles herdam e tem uma propriedade [Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources). Portanto, você pode adicionar um dicionário de recursos local a qualquer **FrameworkElement**.

Aqui, tanto [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) quanto [Border](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) têm dicionários de recursos, e ambas têm um recurso chamado "greeting". O [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) chamado 'textBlock2' está dentro de **Border** e, portanto, sua pesquisa de recursos examina primeiro os recursos de **Border**, depois os recursos de **Page** e depois os recursos de [Application](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application). O **TextBlock** mostrará "Hola mundo".

Para acessar os recursos desse elemento no código, use a propriedade [Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) do elemento. O acesso aos recursos de um [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) no código, em vez de XAML, examinará apenas esse dicionário, não os dicionários do elemento pai.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>
    
    <StackPanel>
        <!-- Displays "Hello world" -->
        <TextBlock x:Name="textBlock1" Text="{StaticResource greeting}"/>

        <Border x:Name="border">
            <Border.Resources>
                <x:String x:Key="greeting">Hola mundo</x:String>
            </Border.Resources>
            <!-- Displays "Hola mundo" -->
            <TextBlock x:Name="textBlock2" Text="{StaticResource greeting}"/>
        </Border>

        <!-- Displays "Hola mundo", set in code. -->
        <TextBlock x:Name="textBlock3"/>
    </StackPanel>
</Page>

```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            textBlock3.Text = (string)border.Resources["greeting"];
        }
    }
```

## <a name="merged-resource-dictionaries"></a>Dicionários de recursos mesclados

Um *dicionário de recursos mesclados* combina um dicionário de recursos com outro, geralmente em outro arquivo.

> **Dica**&nbsp;&nbsp;Você pode criar um arquivo de dicionário de recursos no Microsoft Visual Studio usando a opção **Adicionar &gt; Novo Item…&gt; Dicionário de Recursos** do menu **Projeto**.

Aqui, você define um dicionário de recursos em um arquivo XAML separado chamado Dictionary1.xaml.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

Para usar esse dicionário, mescle-o com o dicionário de sua página:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

Aqui está o que acontece neste exemplo. Em `<Page.Resources>`, declare `<ResourceDictionary>`. A estrutura XAML cria implicitamente um dicionário de recursos quando você adiciona recursos a `<Page.Resources>`; porém, neste caso, você não quer simplesmente qualquer dicionário de recursos, mas um que contenha dicionários mesclados.

Portanto, você declara `<ResourceDictionary>` e depois adiciona elementos à sua coleção `<ResourceDictionary.MergedDictionaries>`. Cada uma dessas entradas assume a forma `<ResourceDictionary Source="Dictionary1.xaml"/>`. Para adicionar mais de um dicionário, basta adicionar uma entrada `<ResourceDictionary Source="Dictionary2.xaml"/>` após a primeira entrada.

Depois de `<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>`, você tem a opção de colocar recursos adicionais em seu dicionário principal. Você usa recursos de um dicionário mesclado da mesma forma que um dicionário comum. No exemplo acima, `{StaticResource brush}` encontra o recurso no dicionário filho/mesclado (Dictionary1.xaml), enquanto `{StaticResource greeting}` encontra seu recurso no dicionário da página principal.

Na sequência de pesquisa de recursos, um dicionário [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) só é verificado após a verificação de todos os recursos com chave do [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). Depois de pesquisar esse nível, a pesquisa acessa os dicionários mesclados, e cada item em **MergedDictionaries** é verificado. Se houver vários dicionários mesclados, eles serão verificados na ordem inversa em que foram declarados na propriedade **MergedDictionaries**. No exemplo seguinte, se Dictionary2.xaml e Dictionary1.xaml tiverem declarado a mesma chave, a chave de Dictionary2.xaml será usada primeiro porque é a última no conjunto **MergedDictionaries** .

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

No escopo de qualquer [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), o dicionário é verificado para confirmar a exclusividade da chave. Entretanto, esse escopo não se estende pelos vários itens em arquivos diferentes de [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries).

É possível usar a combinação da sequência de pesquisa e a ausência de imposição de chave exclusiva nos escopos de dicionários mesclados para criar uma sequência de valores de fallback dos recursos do [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). Por exemplo, você pode armazenar as preferências do usuário para uma determinada cor do pincel no último dicionário de recursos mesclado na sequência. Basta usar um dicionário de recursos que é sincronizado com o estado de seu aplicativo e dados de preferência do usuário. No entanto, se não houver nenhuma preferência de usuário, você poderá definir essa mesma cadeia de caracteres de chave para um recurso **ResourceDictionary** no arquivo [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) inicial e isso pode servir como o valor de fallback. Lembre-se de que qualquer valor fornecido em um dicionário de recursos principal é sempre verificado antes da verificação dos dicionários mesclados; portanto, se quiser usar a técnica de fallback, não defina esse recurso em um dicionário de recursos principal.

## <a name="theme-resources-and-theme-dictionaries"></a>Recursos de tema e dicionários de temas

Um [ThemeResource](../../xaml-platform/themeresource-markup-extension.md) é semelhante a um [StaticResource](../../xaml-platform/staticresource-markup-extension.md), mas a pesquisa de recursos é reavaliada quando o tema muda.

Neste exemplo, você define o primeiro plano de um [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) como um valor do tema atual.

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

Um dicionário de temas é um tipo especial de dicionário mesclado que mantém os recursos que variam conforme o tema atualmente usado pelo usuário em seu dispositivo. Por exemplo, o tema "claro" pode usar uma cor branca, enquanto o tema "escuro" pode usar um pincel de cor escura. O pincel muda o recurso para o qual ele é resolvido; caso contrário, a composição de um controle que usa o pincel como um recurso poderia ser a mesma. Para reproduzir o comportamento de alternância de temas em seus próprios modelos e estilos, em vez de usar [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) como a propriedade para mesclar os itens nos dicionários principais, use a propriedade [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

Cada elemento [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) dentro de [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) deve ter um valor [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute). Esse valor é uma cadeia de caracteres que nomeia o tema relevante, por exemplo, "Default", "Dark", "Light" ou "HighContrast". Em geral, `Dictionary1` e `Dictionary2` definirão recursos que tenham os mesmos nomes, mas valores diferentes.

Aqui, você usa texto em vermelho para o tema claro e texto em azul para o tema escuro.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!-- Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

Neste exemplo, você define o primeiro plano de um [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) como um valor do tema atual.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

Para dicionários de temas, o dicionário ativo que será usado para a pesquisa de recursos muda dinamicamente sempre que a [extensão de marcação ThemeResource](../../xaml-platform/themeresource-markup-extension.md) é usada para fazer a referência e o sistema detecta uma mudança de tema. O comportamento da pesquisa feita pelo sistema se baseia no mapeamento do tema ativo para o [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) de um dicionário de temas específico.

Pode ser útil examinar como os dicionários de temas são estruturados nos recursos de design de XAML padrão, que são correspondentes aos modelos que o Windows Runtime usa por padrão para seus controles. Abra os arquivos XAML em \\(Arquivos de Programas)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic usando um editor de texto ou seu IDE. Observe como os dicionários de temas são definidos primeiro em generic.xaml e como cada dicionário de temas define as mesmas chaves. Cada uma dessas chaves é então referenciada pelos elementos de composição nos vários elementos inseridos, os quais estão fora do dicionário de temas e foram definidos posteriormente na XAML. Há também um arquivo themeresources.xaml à parte para o design que contém apenas os recursos de tema e modelos extras, e não os modelos de controle padrão. As áreas de temas são duplicatas do que você veria em generic.xaml.

Quando você usa ferramentas de design de XAML para editar cópias de estilos e modelos, as ferramentas de design extraem seções dos dicionários de recursos de design de XAML e os colocam como cópias locais de elementos do dicionário XAML que fazem parte do seu aplicativo e do projeto.

Para saber mais e obter uma lista dos recursos do sistema e dos recursos específicos de temas que estão disponíveis para o seu aplicativo, consulte [Recursos de temas XAML](xaml-theme-resources.md).

## <a name="lookup-behavior-for-xaml-resource-references"></a>Comportamento de pesquisa para referências de recursos XAML

*Comportamento de pesquisa* é o termo que descreve como o sistema de recursos XAML tenta encontrar um recurso XAML. A pesquisa ocorre quando uma chave é referenciada como uma referência de recurso XAML de algum lugar na XAML do aplicativo. Primeiro, o sistema de recursos tem um comportamento previsível no que diz respeito a onde ele verificará a existência de um recurso com base na análise. Se um recurso não for encontrado na análise inicial, a análise será expandida. O comportamento de pesquisa continua em todas as localizações e escopos em que um recurso XAML poderia ser definido por um aplicativo ou pelo sistema. Se todas as tentativas de pesquisa de recursos possíveis falharem, em geral um erro será gerado. Normalmente, é possível eliminar esses erros durante o processo de desenvolvimento.

O comportamento de pesquisa para referências de recursos XAML começa com o objeto em que o uso real foi aplicado e sua respectiva propriedade [Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources). Se houver um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) nesse local, esse **ResourceDictionary** será verificado para obter um item que tenha a chave solicitada. Esse primeiro nível de pesquisa raramente é relevante porque, em geral, você não define e depois referencia um recurso no mesmo objeto. Na verdade, não existe aqui uma propriedade **Resources**. É possível fazer referências a recursos XAML praticamente de qualquer lugar do XAML; você não está limitado às propriedades de subclasses [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement).

A sequência de pesquisa verifica o próximo objeto pai na árvore de objetos do runtime do aplicativo. Se [FrameworkElement.Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) existir e contiver um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), o item de dicionário com a cadeia de caracteres da chave especificada será solicitado. Se o recurso for encontrado, a sequência de pesquisa será interrompida e o objeto será fornecido ao local onde a referência foi feita. Caso contrário, o comportamento de pesquisa avançará para o próximo nível pai, em direção da raiz da árvore de objetos. A pesquisa continuará recursivamente para cima até que o elemento raiz da XAML seja alcançado, esgotando a pesquisa de todos os possíveis locais de recursos imediatos.

> **Observação**&nbsp;&nbsp;É prática comum definir todos os recursos imediatos no nível da raiz de uma página, tanto para obter as vantagens desse comportamento de pesquisa de recursos como também como uma convenção do estilo de marcação XAML.

 

Se o recurso solicitado não for encontrado nos recursos imediatos, a próxima etapa de pesquisa será verificar a propriedade [Application.Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources). **Application.Resources** é o melhor lugar para colocar qualquer recurso específico do aplicativo que seja referenciado por várias páginas, na estrutura de navegação do aplicativo.

Os modelos de controles têm outro possível local na pesquisa de referência: os dicionários de temas. Um dicionário de temas é um único arquivo XAML que tem o elemento [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) como raiz. O dicionário de temas pode ser um dicionário mesclado de [Application.Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources). O dicionário de temas também pode ser o dicionário de temas específico de um controle modelo personalizado.

Por fim, há uma pesquisa baseada nos recursos de plataforma. Os recursos de plataforma incluem os modelos de controle definidos para cada um dos temas da interface do usuário do sistema e que definem a aparência padrão de todos os controles usados na interface do usuário de um aplicativo do Windows Runtime. Os recursos da plataforma também incluem um conjunto de recursos nomeados que se relacionam à aparência e aos temas em todo o sistema. Esses recursos são tecnicamente um item [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) e estão disponíveis para pesquisa de XAML ou código depois que o aplicativo é carregado. Por exemplo, os recursos de tema do sistema incluem um recurso chamado "SystemColorWindowTextColor" que oferece uma definição de [Color](https://docs.microsoft.com/uwp/api/Windows.UI.Color) para combinar a cor do texto do aplicativo com a cor do texto da janela do sistema que vem do sistema operacional e das preferências do usuário. Outros estilos XAML em seu aplicativo podem mencionar esse estilo, ou o seu código pode obter um valor da pesquisa de recurso (e convertê-lo em **Color** no caso do exemplo).

Para saber mais e obter uma lista dos recursos do sistema e dos recursos específicos de temas que estão disponíveis para um aplicativo UWP em XAML, confira [Recursos de temas XAML](xaml-theme-resources.md).

Se a chave solicitada ainda assim não for encontrada nesses locais, ocorrerá um erro/exceção de análise XAML. Em determinadas circunstâncias, a exceção de análise XAML pode ser uma exceção de tempo de execução que não é detectada nem pela ação de compilação de marcação XAML, nem pelo ambiente de design XAML.

Devido ao comportamento de pesquisa em níveis dos dicionários de recursos, é possível definir deliberadamente vários itens de recursos, cada um deles com o mesmo valor de cadeia de caracteres como a chave, desde que cada recurso seja definido em um nível diferente. Ou seja, embora as chaves devam ser exclusivas em qualquer [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) fornecido, a exigência de exclusividade não se estende à sequência do comportamento de pesquisa como um todo. Durante a pesquisa, apenas o primeiro objeto recuperado com êxito é usado para a referência de recurso XAML e, em seguida, a pesquisa para. Você poderá usar esse comportamento para solicitar o mesmo recurso XAML pela chave em várias posições dentro da XAML de seu aplicativo, mas obter recursos diferentes de volta, dependendo do escopo do qual a referência do recurso XAML foi feita e de como essa pesquisa específica se comporta.

##  <a name="forward-references-within-a-resourcedictionary"></a>Referências de encaminhamento em um ResourceDictionary


As referências de recursos XAML em um determinado dicionário de recursos deve fazer referência a um recurso já definido com uma chave, e esse recurso deve aparecer lexicalmente antes da referência de recurso. Referências de encaminhamento não podem ser resolvidas por uma referência de recurso XAML. Por isso, se usar referências de recursos estáticos XAML originadas em outro recurso, crie sua estrutura de dicionário de recursos para que os recursos que são utilizados por outros recursos sejam definidos pela primeira vez em um dicionário de recurso.

Os recursos definidos no nível do aplicativo não podem fazer referência a recursos imediatos. Isso equivale a uma tentativa de referência posterior, pois os recursos do aplicativo são efetivamente processados primeiro (quando o aplicativo é iniciado pela primeira vez, e antes do carregamento de qualquer conteúdo de página de navegação). Entretanto, recursos imediatos podem fazer referência a um recurso de aplicativo, e isso pode ser uma técnica bem útil para evitar situações de referência de encaminhamento.

## <a name="xaml-resources-must-be-shareable"></a>Recursos XAML devem ser compartilháveis


Para que um objeto exista em um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), esse objeto deve ser *compartilhável*.

A condição de compartilhável é uma exigência porque, quando a árvore de objetos de um aplicativo é construída e usada em tempo de execução, os objetos não podem existir em vários locais da árvore. Internamente, o sistema de recursos cria cópias de valores de recursos a serem utilizadas no gráfico de objeto de seu aplicativo quando cada recurso XAML é solicitado.

Um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) e a XAML do Windows Runtime em geral dão suporte a esses objetos para uso compartilhável:

-   Estilos e modelos ([Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) e classes derivadas de [FrameworkTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkTemplate))
-   Pincéis e cores (classes derivadas de valores [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) e [Color](https://docs.microsoft.com/uwp/api/Windows.UI.Color))
-   Tipos de animação, incluindo [Storyboard](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
-   Transformações (classes derivadas de [GeneralTransform](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GeneralTransform))
-   [Matrix](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) e [Matrix3D](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D)
-   Valores [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)
-   Determinadas estruturas relacionadas à interface do usuário, como [Thickness](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Thickness) e [CornerRadius](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.CornerRadius)
-   [Tipos de dados XAML intrínsecos](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-intrinsic-data-types)

Também é possível usar tipos personalizados como um recurso compartilhável, se você seguir os padrões de implementação necessários. Você define essas classes em seu código de suporte (ou em componentes de runtime que você inclui) e, depois, instancia essas classes na XAML como um recurso. Exemplos: fontes de dados de objeto e implementações [IValueConverter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) para vinculação de dados.

Tipos personalizados devem ter um construtor padrão, porque é isso que um analisador XAML usa para instanciar uma classe. Os tipos personalizados usados ​​como recursos não podem ter a classe [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) em sua herança, pois o **UIElement** nunca pode ser compartilhável (é sempre destinado a representar exatamente um elemento da interface do usuário que existe em uma posição no gráfico de objeto do aplicativo em runtime).

## <a name="usercontrol-usage-scope"></a>Escopo de uso de UserControl


Um elemento [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl) apresenta uma situação especial de comportamento de pesquisa de recursos, pois tem os conceitos inerentes ao escopo de definição e ao escopo de uso. Um **UserControl** que faça uma referência de recurso XAML no respectivo escopo de definição deve poder dar suporte à pesquisa desse recurso em sua própria sequência de pesquisa de escopo de definição, ou seja, ele não pode acessar recursos de aplicativo. Em um escopo de uso do **UserControl**, uma referência de recurso é tratada como se estivesse na sequência de pesquisa, em direção à respectiva raiz da página de uso (exatamente como qualquer outra referência de recurso feita em um objeto carregado em uma árvore de objetos), e ela pode acessar recursos de aplicativo.

## <a name="resourcedictionary-and-xamlreaderload"></a>ResourceDictionary e XamlReader.Load

O [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) pode ser usado na raiz ou como parte da entrada XAML do método [XamlReader.Load](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load). Também será possível incluir referências de recurso XAML nessa XAML se todas essas referências estiverem totalmente autocontidas na XAML enviada para carregamento. **XamlReader.Load** analisa a XAML em um contexto que não reconhece nenhum outro objeto de **ResourceDictionary**, nem mesmo [Application.Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources). Além disso, não use `{ThemeResource}` em uma XAML enviada para **XamlReader.Load**.

## <a name="using-a-resourcedictionary-from-code"></a>Usando um ResourceDictionary em código

A maior parte dos cenários de um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) é manipulada exclusivamente na XAML. Você declara o contêiner **ResourceDictionary** e os recursos contidos nele como um arquivo XAML ou conjunto de nós XAML em um arquivo de definição da interface do usuário. E, em seguida, você usa referências de recurso XAML para solicitar esses recursos de outras partes do XAML. Ainda assim, existem alguns cenários em que o seu aplicativo talvez precise ajustar o conteúdo de um **ResourceDictionary** usando o código que é executado enquanto o aplicativo está em execução, ou pelo menos consultar o conteúdo de um **ResourceDictionary** para ver se um recurso já está definido. Essas chamadas de código são feitas em uma instância de **ResourceDictionary**, portanto, recupere primeiro uma delas seja um **ResourceDictionary** imediato, em algum lugar da árvore de objetos obtendo [FrameworkElement.Resources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) ou `Application.Current.Resources`.

No código de C\# ou do Microsoft Visual Basic, você pode referenciar um recurso de um determinado [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) usando o indexador ([Item](https://docs.microsoft.com/dotnet/api/system.windows.resourcedictionary.item)). Um **ResourceDictionary** é um dicionário de cadeias de caracteres inseridas, portanto, o indexador usa a chave de cadeia de caracteres em vez de um número inteiro. No código de extensões de componentes Visual C++ (C++/CX), use [Lookup](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.lookup).

Ao usar um código para examinar ou mudar um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), o comportamento para APIs como [Lookup](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.lookup) ou [Item](https://docs.microsoft.com/dotnet/api/system.windows.resourcedictionary.item) não passa de recursos imediatos para recursos do aplicativo, o que é um comportamento do analisador de XAML que só acontece enquanto páginas XAML são carregadas. Em tempo de execução, o escopo para chaves é autossuficiente na instância de **ResourceDictionary** que você está usando no momento. Entretanto, esse escopo não se estende em [MergedDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries).

Além disso, se você solicitar uma chave não existente no [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), talvez não ocorra nenhum erro; o valor de retorno poderá simplesmente ser fornecido como **null**. Porém, você ainda poderá receber um erro se tentar usar o **null** retornado como um valor. O erro seria gerado pelo setter da propriedade, e não pela sua chamada **ResourceDictionary**. A única maneira de evitar um erro seria se a propriedade aceitasse **null** como um valor válido. Observe como esse comportamento contrasta com o comportamento de pesquisa XAML no tempo de análise da XAML; uma falha na resolução da chave fornecida pela XAML no tempo de análise resulta em um erro de análise de XAML, mesmo nos casos em que a propriedade poderia ter aceito **null**.

Dicionários de recursos mesclados são incluídos no escopo de índice do dicionário de recursos principal que referencia o dicionário mesclado em tempo de execução. Em outras palavras, você pode usar **Item** ou [Lookup](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.lookup) do dicionário principal para localizar qualquer objeto que na verdade foi definido no dicionário mesclado. Neste caso, o comportamento de pesquisa se parece com o comportamento de pesquisa XAML em tempo de análise: se houver vários objetos nos dicionários mesclados e cada um deles tiver a mesma chave, o objeto do último dicionário adicionado será retornado.

É possível adicionar itens a um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) existente chamando **Add** (C\# ou Visual Basic) ou [Insert](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.insert) (C++/CX). Você pode adicionar os itens a recursos imediatos ou recursos do aplicativo. Cada uma dessas chamadas de API exige uma chave, o que atende à exigência de que cada item em um **ResourceDictionary** deve ter uma chave. Entretanto, os itens adicionados a um **ResourceDictionary** em tempo de execução não são relevantes para referências a recursos XAML. A pesquisa necessária para referências de recurso XAML acontece quando essa XAML é analisada pela primeira vez quando o aplicativo é carregado (ou um tema alterado é detectado). Os recursos adicionados a coleções no tempo de execução não estavam disponíveis então, e alterar o **ResourceDictionary** não invalida um recurso já recuperado dele, mesmo que você altere o valor desse recurso.

Também é possível remover itens de um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) em tempo de execução, fazer cópias parcial ou total dos itens ou executar outras operações. A listagem de membros do **ResourceDictionary** indica quais APIs estão disponíveis. Observe que como o **ResourceDictionary** tem uma API projetada para dar suporte a suas interfaces de coleção adjacentes, as opções da sua API são diferentes, dependendo de você estar usando C\# ou Visual Basic, em vez de C++/CX.

## <a name="resourcedictionary-and-localization"></a>ResourceDictionary e localização


Um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) XAML pode, inicialmente, conter cadeias de caracteres que podem ser localizadas. Se assim for, armazene essas cadeias de caracteres como recursos de projeto, e não em um **ResourceDictionary**. Retire as cadeias de caracteres da XAML e dê ao elemento proprietário um valor da [diretiva x:Uid](https://docs.microsoft.com/windows/uwp/xaml-platform/x-uid-directive). Em seguida, defina um recurso em um arquivo de recursos. Forneça um nome de recurso no formato *XUIDValue*.*PropertyName* e um valor de recurso da cadeia de caracteres que deve ser localizada.

## <a name="custom-resource-lookup"></a>Pesquisa de recursos personalizada

Em cenários avançados, você pode implementar uma classe que pode ter um comportamento diferente do comportamento de pesquisa de referência de recursos XAML descrito neste tópico. Para fazer isso, implemente a classe [CustomXamlResourceLoader](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) para poder acessar esse comportamento usando a extensão de marcação [CustomResource markup extension](https://docs.microsoft.com/windows/uwp/xaml-platform/customresource-markup-extension) para referências de recursos, em vez de usar [StaticResource](../../xaml-platform/staticresource-markup-extension.md) ou [ThemeResource](../../xaml-platform/themeresource-markup-extension.md). A maioria dos aplicativos não terá cenários que requeiram isso. Para saber mais, consulte [CustomXamlResourceLoader](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

 
## <a name="related-topics"></a>Tópicos relacionados

* [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [Visão geral do XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview)
* [Extensão de marcação StaticResource](../../xaml-platform/staticresource-markup-extension.md)
* [Extensão de marcação ThemeResource](../../xaml-platform/themeresource-markup-extension.md)
* [Recursos de temas XAML](xaml-theme-resources.md)
* [Aplicando estilos a controles](xaml-styles.md)
* [Atributo x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute)

 

 



