---
author: jwmsft
title: atributo xLoad
description: xLoad permite dinâmica criação e destruição de um elemento e seus filhos, diminuindo o uso de memória e o tempo de inicialização. 
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d9659d183c020c579aa0a21fe179a69c1d9997c5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5701650"
---
# <a name="xload-attribute"></a>Atributo x:Load

Você pode usar **X:Load** para otimizar a inicialização, a criação de árvore visual e o uso de memória de seu aplicativo XAML. Usar **X:Load** tem um efeito visual semelhante a **visibilidade**, exceto que quando o elemento não é carregado, sua memória é lançada e internamente um pequeno espaço reservado é usado para marcar seu lugar na árvore visual.

O elemento de interface do usuário atribuído com x: carregar pode ser carregado e descarregado por meio de código, ou usando uma expressão [x: Bind](x-bind-markup-extension.md) . Isso é útil para reduzir os custos de elementos que são mostrados de forma pouco frequente ou condicional. Ao usar X:Load em um contêiner como Grid ou StackPanel, o contêiner e todos os seus filhos são carregados ou descarregados como um grupo.

O rastreamento de elementos diferidos pela estrutura XAML adiciona cerca de 600 bytes ao uso da memória para cada elemento atribuído com x: carregar, para levar em conta o espaço reservado. Portanto, é possível esse atributo em excesso na medida em que, na verdade, diminui o desempenho. É recomendável que você apenas usá-lo nos elementos que precisam estar oculta. Se você usar X:Load em um contêiner, a sobrecarga é pago apenas para o elemento com o atributo X:Load.

> [!IMPORTANT]
> O atributo X:Load está disponível a partir do Windows 10, versão 1703 (atualização para criadores). A versão mínima segmentada pelo seu projeto Visual Studio deve ser *Atualização do Windows 10 para Criadores (10.0, compilação 15063)* para usar x:Load.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Elementos de carregamento

Há várias maneiras diferentes para carregar os elementos:

- Use uma expressão [x: Bind](x-bind-markup-extension.md) para especificar o estado de carga. A expressão deve retornar **true** para carregar e **false** para descarregar o elemento.
- Chame [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) com o nome definido no elemento.
- Chame [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) com o nome definido no elemento.
- Em um [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), use uma animação [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou **Storyboard** que segmenta o elemento X:Load.
- Segmente o elemento descarregado em qualquer **Storyboard**.

> OBSERVAÇÃO: Depois que a instanciação de um elemento for iniciada, ele será criado no thread da interface do usuário, de maneira que faça a interface do usuário ser dividida se várias forem criadas de uma só vez.

Depois que um elemento adiado for criado por um dos métodos listados acima, várias coisas acontecerão:

- O evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) no elemento será acionado.
- O campo para X:Name é definido.
- Todas as associações de x: Bind no elemento serão avaliadas.
- Se o aplicativo tiver se registrado para receber notificações de alteração feita na propriedade sobre a propriedade que contém os elementos adiados, a notificação será acionada.

## <a name="unloading-elements"></a>Elementos descarregando

Descarregar um elemento:

- Use uma expressão x: Bind para especificar o estado de carga. A expressão deve retornar **true** para carregar e **false** para descarregar o elemento.
- Em uma página ou um UserControl, chame **UnloadObject** e passe a referência de objeto
- Chame **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** e passe a referência de objeto

Quando um objeto é descarregado, ele será substituído na árvore com um espaço reservado. A instância do objeto permanecerá na memória até que todas as referências foram lançadas. A API UnloadObject em um página/UserControl foi projetada para liberar as referências mantidas pelo gerador de código para X:Name e x: Bind. Se você mantiver referências adicionais no código de aplicativo, que eles também precisarão ser liberado.

Quando um elemento é descarregado, todo o estado associado ao elemento será descartada, dessa forma, se usar X:Load como uma versão otimizada de visibilidade, em seguida, certifique-se de que todos os estado é aplicado por meio de associações, ou novamente é aplicado pelo código quando o evento Loaded é acionado.

## <a name="restrictions"></a>Restrições

As restrições para usar **X:Load** são:

- Você deve definir um [X:Name](x-name-attribute.md)para o elemento, como daí precisa ser uma maneira de encontrar o elemento depois.
- Você pode usar X:Load nos tipos que derivam de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) ou [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- Você não pode usar X:Load nos elementos raiz em uma [**página**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), um [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)ou um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
- Você não pode usar X:Load nos elementos em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
- Você não pode usar X:Load em XAML flexível carregado com [**XamlReader. Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Mover um elemento pai apagará todos os elementos que não tiverem sido carregados.

## <a name="remarks"></a>Comentários

Você pode usar X:Load em elementos aninhados, no entanto eles precisam ser efetivados a partir do elemento mais externo. Se você tentar observar um elemento filho antes que o pai tenha sido percebido, uma exceção será gerada.

Normalmente, recomendamos que você adie elementos que não são visíveis no primeiro quadro.Uma boa orientação para encontrar os candidatos a serem diferidos é procurar elementos que estão sendo criados com [**Visibilidade**](https://msdn.microsoft.com/library/windows/apps/br208992) recolhida. Além disso, a UI que é desencadeada pela interação do usuário é um bom lugar para procurar elementos que você pode adiar.

Seja cauteloso ao adiar elementos em um [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), pois o tempo de inicialização será menor, mas também o desempenho do movimento panorâmico pode ser reduzido, dependendo do que você está criando. Se você estiver procurando melhorar o desempenho de movimento panorâmico, consulte a documentação sobre [extensão de marcação {x: Bind}](x-bind-markup-extension.md) e [atributo x:Phase](x-phase-attribute.md).

Se o [atributo X:Phase](x-phase-attribute.md) é usado em conjunto com **X:Load** em seguida, quando um elemento ou uma árvore de elementos for realizada, as associações serão aplicadas até e incluindo a fase atual. A fase especificada para **X:Phase** afeta ou controla o estado de carregamento do elemento. Quando um item de lista é reciclado como parte de movimento panorâmico, os realizados elementos irá se comportar da mesma maneira como outros elementos ativos e associações compiladas (associações **{x: Bind}** ) serão processadas usando as mesmas regras, incluindo fases.

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

