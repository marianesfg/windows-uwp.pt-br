---
author: jwmsft
title: atributo xLoad
description: xLoad permite a criação dinâmica e destruição de um elemento e seus filhos, diminuindo o uso de memória e tempo de inicialização.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f34ea43b981de65c81e9cce2b8a896c3577e595
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "610781"
---
# <a name="xload-attribute"></a>Atributo x:Load

Você pode usar **X: carregar** para otimizar a inicialização, criação de árvore visual e uso de memória do seu aplicativo XAML. Usar **X: carregar** tem um efeito visual semelhante a **visibilidade**, exceto que quando o elemento não é carregado, sua memória é liberada e um pequeno espaço reservado é usado internamente para marcar seu lugar na árvore visual.

O elemento de interface do usuário atribuído com x: carregar pode ser carregado e descarregado por meio de código ou utilizando uma expressão [X: Vincular](x-bind-markup-extension.md) . Isso é útil para reduzir os custos de elementos que são mostrados de forma pouco frequente ou condicional. Quando você usar x: carregar em um contêiner, como grade ou StackPanel, o contêiner e todos os seus filhos são carregados ou descarregados como um grupo.

O controle dos elementos adiados pelo framework XAML adiciona cerca de 600 bytes para o uso de memória para cada elemento atribuído com carga: x, para considerar o espaço reservado. Portanto, é possível uso excessivo este atributo na medida em que realmente diminui o desempenho. É recomendável que você apenas usá-lo em elementos que precisam ser ocultada. Se você usar x: carregar em um contêiner, a sobrecarga é pago apenas para o elemento com o atributo x: carregar.

> [!IMPORTANT]
> O atributo de x: carregar está disponível a partir do Windows 10, versão 1703 (criadores atualização). A versão mínima segmentada pelo seu projeto Visual Studio deve ser *Atualização do Windows 10 para Criadores (10.0, compilação 15063)* para usar x:Load.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Elementos de carregamento

Há várias maneiras diferentes para carregar os elementos:

- Use uma expressão [X: ligar](x-bind-markup-extension.md) para especificar o estado de carga. A expressão deve retornar **true** para carregar e **false** para descarregar o elemento.
- Chame [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) com o nome definido no elemento.
- Chame [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) com o nome definido no elemento.
- Em um [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), use uma animação [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou **Storyboard** que tem como destino o elemento x: carregar.
- O elemento descarregado em qualquer **Storyboard**de destino.

> OBSERVAÇÃO: Depois que a instanciação de um elemento for iniciada, ele será criado no thread da interface do usuário, de maneira que faça a interface do usuário ser dividida se várias forem criadas de uma só vez.

Depois que um elemento adiado for criado por um dos métodos listados acima, várias coisas acontecerão:

- O evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) no elemento será acionado.
- O campo para x: Name é definido.
- Qualquer ligações x: vincular no elemento são avaliadas.
- Se o aplicativo tiver se registrado para receber notificações de alteração feita na propriedade sobre a propriedade que contém os elementos adiados, a notificação será acionada.

## <a name="unloading-elements"></a>Elementos de descarregamento

Para descarregar um elemento:

- Use uma expressão x: ligar para especificar o estado de carga. A expressão deve retornar **true** para carregar e **false** para descarregar o elemento.
- Em uma página ou UserControl, chame **UnloadObject** e passe na referência de objeto
- Chamar **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** e passar na referência de objeto

Quando um objeto é descarregado, ele será substituído na árvore com um espaço reservado. A instância do objeto permanecerá na memória até que todas as referências foram liberadas. A API UnloadObject em um página/UserControl foi projetada para liberar as referências conduzidas por gerador de código: nome de x e x: vincular. Se você mantiver referências adicionais no código de aplicativo, que eles também precisará ser liberada.

Quando um elemento seja descarregado, todo o estado associado ao elemento serão descartados, portanto se usando x: carregar como uma versão otimizada de visibilidade, em seguida, certifique-se de que todos os de estado é aplicada por meio de vínculos ou novamente é aplicado pelo código quando o evento Loaded for acionado.

## <a name="restrictions"></a>Restrições

As restrições de uso **X: carregar** são:

- Você deve definir um [X:Names](x-name-attribute.md) para o elemento, uma vez que deve haver uma forma de encontrar o elemento depois.
- Você só pode usar x: carregar nos tipos de derivam de [**elementos de interface**](https://msdn.microsoft.com/library/windows/apps/br208911) ou [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- Você não pode usar x: carregar em elementos de raiz em uma [**página**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), um [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)ou um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
- Você não pode usar x: carregar em elementos em um [**dicionário de recurso**](https://msdn.microsoft.com/library/windows/apps/br208794).
- Você não pode usar x: carregar em XAML livre carregado com [**XamlReader**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Mover um elemento pai limpará check-out de todos os elementos que não foram carregados.

## <a name="remarks"></a>Comentários

Você pode usar x: carregar em elementos aninhados, no entanto, eles precisam ser concretizados do elemento mais externas no.  Se você tentar observar um elemento filho antes que o pai tenha sido percebido, uma exceção será gerada.

Normalmente, recomendamos que você adie elementos que não são visíveis no primeiro quadro. Uma boa orientação para encontrar os candidatos a serem diferidos é procurar elementos que estão sendo criados com [**Visibilidade**](https://msdn.microsoft.com/library/windows/apps/br208992) recolhida. Além disso, a UI que é desencadeada pela interação do usuário é um bom lugar para procurar elementos que você pode adiar.

Seja cauteloso ao adiar elementos em um [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), pois o tempo de inicialização será menor, mas também o desempenho do movimento panorâmico pode ser reduzido, dependendo do que você está criando. Se você estiver procurando melhorar o desempenho de movimento panorâmico, consulte a documentação sobre [extensão de marcação {x: Bind}](x-bind-markup-extension.md) e [atributo x:Phase](x-phase-attribute.md).

Se o [atributo x: fase](x-phase-attribute.md) for usado em conjunto com **X: carregar** , quando um elemento ou um elemento de árvore é concretizado, as ligações são aplicadas até e incluindo a fase atual. A fase especificada para **X: fase** afetar ou controlar o estado de carregamento do elemento. Quando um item de lista é reciclado como parte de panorâmica, concretizados elementos serão se comportam da mesma maneira como outros elementos ativos e ligações compiladas (**{X: Vincular}** ligações) são processadas usando as mesmas regras, incluindo a introdução gradual.

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

