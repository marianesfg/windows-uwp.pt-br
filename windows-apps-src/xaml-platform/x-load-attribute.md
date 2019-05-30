---
title: Atributo xLoad
description: xLoad permite a criação e destruição dinâmicas de um elemento e seus filhos, diminuindo o uso de memória e o tempo de inicialização.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d85051aabdb7631c5bdb84e08d6d10a0f70d6ede
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372290"
---
# <a name="xload-attribute"></a>Atributo x:Load

Você pode usar **x:Load** para otimizar a inicialização, a criação de árvore visual e o uso da memória do seu aplicativo XAML. Usar **x:Load** tem um efeito visual semelhante à **Visibilidade**, exceto que quando o elemento não é carregado, sua memória é lançada e, internamente, um pequeno espaço reservado é usado para marcar seu lugar na árvore visual.

O elemento de interface do usuário atribuído com x:Load pode ser carregado e descarregado por meio de código ou usando uma expressão [x: Bind](x-bind-markup-extension.md). Isso é útil para reduzir os custos de elementos que são mostrados de forma pouco frequente ou condicional. Quando você usa x:Load em um recipiente, como Grid ou StackPanel, o contêiner e todos os seus filhos são carregados ou descarregados como um grupo.

O rastreamento de elementos diferidos pela estrutura XAML adiciona cerca de 600 bytes ao uso de memória para cada elemento atribuído com x:Load, para contabilizar o espaço reservado. Portanto, é possível usar esse atributo em excesso até o limite atual de redução do seu desempenho. Recomendamos que você apenas use em elementos que precisam ser escondidos. Se você usar x:Load em um recipiente, a sobrecarga é paga apenas pelo elemento com o atributo x:Load.

> [!IMPORTANT]
> O atributo de x: carga está disponível a partir do Windows 10, versão 1703 (Creators Update). A versão mínima segmentada pelo seu projeto Visual Studio deve ser *Atualização do Windows 10 para Criadores (10.0, compilação 15063)* para usar x:Load.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Elementos de carregamento

Existem várias maneiras de carregar os elementos:

- Use uma expressão [x: Bind](x-bind-markup-extension.md) para especificar o estado de carregamento. A expressão deve retornar **true** para carregar e **falso** para descarregar o elemento.
- Chame [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) com o nome definido no elemento.
- Chame [**GetTemplateChild**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) com o nome definido no elemento.
- Em um [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState), use uma animação [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) ou **Storyboard** que segmenta o elemento x:Load.
- Segmente o elemento descarregado em qualquer **Storyboard**.

> OBSERVAÇÃO: Depois que a instanciação de um elemento for iniciado, ele será criado no thread da interface do usuário, portanto, pode fazer com que a interface do usuário reamostragem se for muito que grande parte é criado ao mesmo tempo.

Depois que um elemento adiado for criado por um dos métodos listados acima, várias coisas acontecerão:

- O evento [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) no elemento será acionado.
- O campo para X:Name é definido.
- Todas as associações x:Bind no elemento serão avaliadas.
- Se o aplicativo tiver se registrado para receber notificações de alteração feita na propriedade sobre a propriedade que contém os elementos adiados, a notificação será acionada.

## <a name="unloading-elements"></a>elementos de carregamento

Para descarregar um elemento:

- Use uma expressão x: Bind para especificar o estado de carregamento. A expressão deve retornar **true** para carregar e **falso** para descarregar o elemento.
- Em uma página ou UserControl, chame **UnloadObject** e passe na referência de objeto
- Chame **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** e passe na referência de objeto

Quando um objeto for descarregado, ele será substituído na árvore com um espaço reservado. A instância do objeto permanecerá na memória até que todas as referências tenham sido liberadas. A API do UnloadObject em uma página / UserControl foi projetada para liberar as referências mantidas pelo codegen para x:Name e x:Bind. Se você tiver referências adicionais no código do aplicativo, elas também precisarão ser lançadas.

Quando um elemento é descarregado, todo o estado associado ao elemento será descartado, então, se usar x:Load como uma versão otimizada de Visibilidade, assegure-se de que todo o estado seja aplicado através de ligações ou seja reaplicado por código quando o evento Loaded for acionado.

## <a name="restrictions"></a>Restrições

As restrições para usar **x:Load** são:

- Você deve definir um [X:Name](x-name-attribute.md) para o elemento, pois não há precisa ser uma maneira de localizar o elemento mais tarde.
- Você só pode usar x:Load em tipos derivados de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) ou [**FlyoutBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- Você não pode usar x:Load nos elementos raiz em uma [**Página**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), um [**UserControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol), ou um [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate).
- Você não pode usar x:Load em elementos em um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- Você não pode usar x:Load em XAML flexível carregado com [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Mover um elemento-pai apagará todos os elementos que não tenham sido carregados.

## <a name="remarks"></a>Comentários

Você pode usar x:Load em elementos aninhados, no entanto, eles devem ser realizados a partir do elemento mais externo.  Se você tentar observar um elemento filho antes que o pai tenha sido percebido, uma exceção será gerada.

Normalmente, recomendamos que você adie elementos que não são visíveis no primeiro quadro. Uma boa diretriz para encontrar candidatos a serem adiados é procurar elementos que estejam sendo criados com recolhida [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility). Além disso, a UI que é desencadeada pela interação do usuário é um bom lugar para procurar elementos que você pode adiar.

Seja cauteloso ao adiar elementos em um [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), pois o tempo de inicialização será menor, mas também o desempenho do movimento panorâmico pode ser reduzido, dependendo do que você está criando. Se você estiver procurando melhorar o desempenho de movimento panorâmico, consulte a documentação sobre [extensão de marcação {x: Bind}](x-bind-markup-extension.md) e [atributo x:Phase](x-phase-attribute.md).

Se o [atributo x:Phase](x-phase-attribute.md) for usado em conjunto com **x:Load** quando um elemento ou uma árvore de elementos for realizado, as associações serão aplicadas até e incluindo a fase atual. A fase especificada para **x:Phase** não afeta ou controla o estado de carga do elemento. Quando um item de lista é reciclado como parte de movimento panorâmico, os elementos realizados da mesma forma que outros elementos ativos e as associações compiladas (associações **{x:Bind}** ) serão processados usando as mesmas regras, incluindo fases.

Uma diretriz geral é avaliar o desempenho do aplicativo antes e depois, para garantir que você obtenha o desempenho que deseja.

## <a name="example"></a>Exemplo

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```

