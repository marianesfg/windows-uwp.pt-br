---
author: jwmsft
title: Atributo xDeferLoadStrategy
description: "xDeferLoadStrategy atrasa a criação de um elemento e seus filhos, o que reduz o tempo de inicialização, mas aumenta um pouco o uso da memória. Cada elemento afetado adiciona cerca de 600 bytes para o uso da memória."
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4ccc18920a98b3c2258b4965e96fa063124d0546
ms.lasthandoff: 02/07/2017

---

# <a name="xdeferloadstrategy-attribute"></a>Atributo x:DeferLoadStrategy

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**x:DeferLoadStrategy="Lazy"** é um recurso que pode ser usado para otimizar o desempenho dos cenários de inicialização ou criação de árvore de um aplicativo XAML. O uso de **x:DeferLoadStrategy="Lazy"** atrasa a criação de um elemento e seus filhos, diminuindo o tempo de inicialização e os custos de memória sem precisar criar os elementos. Isso é útil para reduzir os custos de elementos que não costumam ser ou condicionalmente necessários. O elemento será realizado quando chamado a partir do código ou VisualStateManager.

No entanto, a manutenção de adiamento adiciona cerca de 600 bytes ao uso da memória para cada elemento afetado. Quanto maior for a árvore de elementos que você adiar, mais tempo de inicialização você vai economizar, mas ao custo de um volume de memória maior. Portanto, é possível usar esse atributo em excesso até o limite de redução do seu desempenho.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Comentários

As restrições para usar **x: DeferLoadStrategy** são:

-   Exige um [x:Name](x-name-attribute.md) definido, porque deve existir uma maneira de encontrar o elemento depois.
-   Apenas um [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) pode ser marcado como adiado, com exceção dos tipos derivados de [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
-   Elementos raiz não podem ser adiados em um [**Page**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page), [**UserControls**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.usercontrol) ou [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
-   Elementos em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) não podem ser adiados.
-   Não funciona com XAML flexível carregado com [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
-   Mover um elemento pai apagará todos os elementos não obtidos.

Existem várias formas diferentes de obter os elementos adiados:

-   Chame [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) com o nome definido no elemento.
-   Chame [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) com o nome definido no elemento.
-   Em um [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), use uma animação [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou **Storyboard** que segmenta o elemento adiado.
-   Segmente o elemento adiado em qualquer **Storyboard**.
-   Use uma associação que esteja segmentando o elemento adiado.
-   OBSERVAÇÃO: Depois que a instanciação de um elemento for iniciada, ele será criado no thread da interface do usuário, de maneira que faça a interface do usuário ser dividida se várias forem criadas de uma só vez.

Depois que um elemento adiado for criado por um dos métodos listados acima, várias coisas acontecerão:

-   O evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) no elemento será acionado.
-   Todas as associações no elemento serão avaliadas.
-   Se o aplicativo tiver se registrado para receber notificações de alteração feita na propriedade sobre a propriedade que contém os elementos adiados, a notificação será acionada.

Você pode aninhar elementos adiados, no entanto eles precisam ser efetivados a partir do elemento mais externo.  Se você tentar observar um elemento filho antes que o pai tenha sido percebido, uma exceção será gerada.

Em geral, a recomendação é adiar coisas que não estiverem visíveis no primeiro quadro.  Uma boa diretriz para encontrar candidatos a serem adiados é procurar elementos que estejam sendo criados com recolhida [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992).  Também interface do usuário incidental (ou seja, interface do usuário que é acionada por interação do usuário) é um bom lugar para procurar elementos de adiamento.  

Seja cauteloso ao adiar elementos num cenário [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), pois o tempo de inicialização será menor, mas também o desempenho do movimento panorâmico pode ser reduzido, dependendo do que você está criando.  Se você estiver procurando melhorar o desempenho de movimento panorâmico, consulte a documentação sobre [extensão de marcação {x: Bind}](x-bind-markup-extension.md) e [atributo x:Phase](x-phase-attribute.md).

Se o [atributo x:Phase](x-phase-attribute.md) for usado em conjunto com **x:DeferLoadStrategy** quando um elemento ou uma árvore de elementos for realizado, as associações serão aplicadas até e incluindo a fase atual. A fase especificada para **x:Phase** não afeta ou controla o adiamento do elemento. Quando um item de lista é reciclado como parte de movimento panorâmico, os elementos realizados da mesma forma que outros elementos ativos e as associações compiladas (associações **{x:Bind}**) serão processados usando as mesmas regras, incluindo fases.

Uma diretriz geral é avaliar o aplicativo antes e depois, para garantir que você obtenha o desempenho que deseja.

## <a name="example"></a>Exemplo

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```


