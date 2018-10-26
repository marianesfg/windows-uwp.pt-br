---
author: Jwmsft
Description: You create the UI for your app by using controls such as buttons, text boxes, and combo boxes to display data and get user input. Here, we show you how to add controls to your app.
title: Introdução a controles e padrões
ms.assetid: 64740BF2-CAA1-419E-85D1-42EE7E15F1A5
label: Intro to controls and patterns
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 290c06a7bac75217c8249821821f081da619c7f3
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5593450"
---
# <a name="intro-to-controls-and-patterns"></a>Introdução a controles e padrões

No desenvolvimento de aplicativos UWP, um *controle* é um elemento de interface do usuário que exibe conteúdo ou permite interação. Crie a interface do usuário do seu aplicativo usando controles como botões, caixas de texto e caixas de combinação para exibir dados e obter entrada do usuário.

> **APIs importantes**: [namespace Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)

Um *padrão* é a receita para modificar um controle ou combinar vários controles para criar algo novo. Por exemplo, o padrão de [mestre/detalhes](master-details.md) é uma maneira que você pode usar um controle [SplitView](split-view.md) para navegação do aplicativo. Da mesma forma, você pode personalizar o modelo de um controle [NavigationView](navigationview.md) para implementar o padrão de guia.

Em muitos casos, você pode usar um controle da forma como ele se apresenta. No entanto, os controles XAML separam a função da estrutura e aparência para que você possa fazer vários níveis de modificação para torná-los adequados às suas necessidades. Na seção [Estilo](../style/index.md), você pode aprender a usar [estilos XAML](xaml-styles.md) e [modelos de controle](control-templates.md) para modificar um controle.

Nesta seção, fornecemos diretrizes para cada um dos controles XAML que você pode usar para criar sua interface do usuário do aplicativo. Para começar, este artigo mostra como adicionar controles ao seu aplicativo. Há 3 etapas principais para usar controles em seu aplicativo:

- Adicione um controle à interface do usuário de seu aplicativo.
- Defina as propriedades no controle, como largura, altura e cor de primeiro plano.
- Adicione código aos manipuladores de evento do controle para que ele faça algo. 

## <a name="add-a-control"></a>Adiciona um controle
=Você pode adicionar um controle a um aplicativo de várias maneiras:
 
- Use uma ferramenta de design, como o Blend para Visual Studio ou o designer XAML (Extensible Application Markup Language) do Microsoft Visual Studio. 
- Adicione o controle à marcação XAML no editor XAML do Visual Studio. 
- Adicione o controle usando código. Os controles adicionados ao código ficam visíveis quando o aplicativo é executado, mas não no designer XAML do Visual Studio.

No Visual Studio, quando você adiciona e manipula controles no seu aplicativo, você pode usar muitos dos recursos do programa, incluindo a Caixa de Ferramentas, o designer XAML, o editor XAML e a janela Propriedades. 

A Caixa de Ferramentas do Visual Studio exibe muitos dos controles que você pode usar no seu aplicativo. Para adicionar um controle ao aplicativo, clique duas vezes nele na Caixa de Ferramentas. Por exemplo, quando você clica duas vezes no controle TextBox, a linguagem XAML é adicionada ao modo de exibição XAML. 

```xaml
<TextBox HorizontalAlignment="Left" Text="TextBox" VerticalAlignment="Top"/>
```

Você também pode arrastar o controle da Caixa de Ferramentas para o designer XAML.

## <a name="set-the-name-of-a-control"></a>Definir o nome de um controle

Para trabalhar com um controle usando código, defina o atributo [x:Name](../../xaml-platform/x-name-attribute.md) e faça referência a ele pelo nome em seu código. Você pode definir o nome na janela Propriedades do Visual Studio ou no XAML. Consulte como definir o nome do controle atualmente selecionado usando a caixa de texto Nome na parte superior da janela Propriedades.

Para nomear um controle
1. Selecione o elemento a ser nomeado.
2. No painel Propriedades, digite um nome na caixa de texto Nome.
3. Pressione Enter para confirmar o nome.

![Propriedade Nome no designer do Visual Studio](images/add-controls-control-name-designer.png)

Consulte como definir o nome de um controle no editor XAML adicionando o atributo x:Name.

```xaml
<Button x:Name="Button1" Content="Button"/>
```

## <a name="set-the-control-properties"></a>Definir as propriedades de controle 

Use propriedades para especificar a aparência, o conteúdo e outros atributos de controles. Quando você adiciona um controle usando a ferramenta de design, algumas propriedades que controlam o tamanho, a posição e o conteúdo podem ser definidas pelo Visual Studio. Você pode alterar algumas propriedades, como Width, Height ou Margin selecionando e manipulando o controle no modo Design. Esta ilustração mostra algumas das ferramentas de redimensionamento que estão disponíveis no modo Design. 

![Ferramentas de redimensionamento no designer do Visual Studio](images/add-controls-resizing-designer.png)

Talvez você queira que o controle seja dimensionado e posicionado automaticamente. Nesse caso, você pode redefinir as propriedades de tamanho e posição que o Visual Studio define.

Para redefinir uma propriedade
1. No painel Propriedades, clique no marcador de propriedade ao lado do valor da propriedade. O menu Propriedade é aberto.
2. No menu Propriedade, clique em Redefinir.

![Propriedade do Visual Studio para redefinir a opção de menu](images/add-controls-property-reset.png)

Você pode definir as propriedades de controles na janela Propriedades, em XAML ou em código. Por exemplo, para alterar a cor de primeiro plano de um Botão, defina a propriedade Foreground do controle. Esta ilustração mostra como definir a propriedade Foreground usando o seletor de cor na janela Propriedades. 

![Selecionador de cores no designer do Visual Studio](images/add-controls-foreground-designer.png)

Veja como definir a propriedade Foreground no editor XAML. Observe a janela do Visual Studio IntelliSense que é aberta para ajudá-lo com a sintaxe. 

![IntelliSense na parte 1 do XAML](images/add-controls-foreground-xaml.png)

![IntelliSense na parte 2 do XAML](images/add-controls-foreground-xaml-2.png)

Aqui está o XAML resultante depois que você define a propriedade Foreground. 

```xaml
<Button x:Name="Button1" Content="Button" 
        HorizontalAlignment="Left" VerticalAlignment="Top"
        Foreground="Beige"/>
```

Aqui está como definir a propriedade Foreground em código. 

```csharp
Button1.Foreground = new SolidColorBrush(Windows.UI.Colors.Beige);
```

## <a name="create-an-event-handler"></a>Criar um manipulador de eventos 

Todos os controles possuem eventos que permitem que você responda a ações de seu usuário ou a outras mudanças em seu aplicativo. Por exemplo, um controle de Botão fornece um evento Clicar que é gerado quando um usuário clica no Botão. Crie um método, chamado manipulador de eventos, para manipular o evento. Você pode associar o evento de um controle a um método de manipulador de eventos na janela Propriedades, em XAML ou em código. Para saber mais sobre eventos, veja [Visão geral de eventos e eventos roteados](../../xaml-platform/events-and-routed-events-overview.md).

Para criar um manipulador de eventos, selecione o controle e clique na guia Eventos, na parte superior da janela Propriedades. A janela Propriedades lista todos os eventos disponíveis para esse controle. Veja alguns dos eventos para um Botão.

![Lista de eventos do Visual Studio](images/add-controls-add-event-designer.png)

Para criar um manipulador de eventos com o nome padrão, clique duas vezes na caixa de texto ao lado do nome do evento na janela Propriedades. Para criar um manipulador de eventos com um nome personalizado, digite o nome de sua escolha na caixa de texto e pressione Enter. O manipulador de eventos é criado e o arquivo code-behind é aberto no editor de código. O método de manipulador de eventos tem dois parâmetros. O primeiro é `sender`, que é uma referência ao objeto ao qual o manipulador está anexado. O parâmetro `sender` é um tipo **Object**. Geralmente, você converte `sender` em um tipo mais preciso se espera verificar ou alterar o estado no próprio objeto `sender`. Com base no design do seu próprio aplicativo, você espera um tipo que seja seguro no qual converter o `sender`, com base no local onde o manipulador foi anexado. O segundo valor são dados de evento. Esse valor geralmente aparece em assinaturas como o parâmetro `e` ou `args`.

Aqui está o código que manipula o evento Clicar de um Botão nomeado `Button1`. Quando você clica no botão, a propriedade Foreground do Botão em que você clicou é definida para a cor a azul. 

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Button b = (Button)sender;
    b.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
}
```

Você também pode associar um manipulador de eventos em XAML. No editor XAML, digite o nome do evento que deseja manipular. O Visual Studio mostra uma janela IntelliSense quando você começa a digitar. Depois de especificar o evento, você pode clicar duas vezes em `<New Event Handler>` na janela IntelliSense para criar um novo manipulador de eventos com o nome padrão, ou selecionar um manipulador de eventos existente na lista. 

Aqui está a janela IntelliSense que é exibida. Ela ajuda você a criar um novo manipulador de eventos ou selecionar um manipulador de eventos existente.

![IntelliSense para o evento clicar](images/add-controls-add-event-xaml.png)

Este exemplo mostra como associar um evento Clicar a um manipulador de eventos nomeado Botão_Clicar em XAML. 

```xaml
<Button Name="Button1" Content="Button" Click="Button_Click"/>
```

Você também pode associar um evento ao manipulador de eventos no code-behind. Consulte como associar um manipulador de eventos no código.

```csharp
Button1.Click += new RoutedEventHandler(Button_Click);
```

## <a name="related-topics"></a>Tópicos relacionados

-   [Índice de controles por função](controls-by-function.md)
-   [Namespace Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)
-   [Layout](../layout/index.md)
-   [Estilo](../style/index.md)
-   [Usabilidade](../usability/index.md)
