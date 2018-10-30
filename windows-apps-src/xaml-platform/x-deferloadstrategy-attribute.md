---
author: jwmsft
title: Atributo xDeferLoadStrategy
description: xDeferLoadStrategy atrasa a criação de um elemento e seus filhos, o que reduz o tempo de inicialização, mas aumenta um pouco o uso da memória.Cada elemento afetado adiciona cerca de 600 bytes para o uso da memória.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cd958ba5f9025430be2736329c5a909233461039
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5761098"
---
# <a name="xdeferloadstrategy-attribute"></a>Atributo x:DeferLoadStrategy

> [!IMPORTANT]
> A partir do Windows 10, versão 1703 (Atualização para criadores), **x:DeferLoadStrategy** foi substituído pelo [**atributo x:Load**](x-load-attribute.md). Usar `x:Load="False"` é equivalente a `x:DeferLoadStrategy="Lazy"`, mas fornece a capacidade de descarregar a interface do usuário, se necessário. Consulte o atributo [x:Load](x-load-attribute.md) para mais informações.

Você pode usar **x:DeferLoadStrategy="Lazy"** para otimizar o desempenho de inicialização ou criação de árvore do seu aplicativo XAML. Quando você usa **x:DeferLoadStrategy="Lazy"**, a criação de um elemento e seus filhos está atrasada, o que diminui o tempo de inicialização e os custos de memória. Isso é útil para reduzir os custos de elementos que são mostrados de forma pouco frequente ou condicional. O elemento será realizado quando chamado a partir do código ou VisualStateManager.

No entanto, o rastreamento de elementos diferidos pela estrutura XAML adiciona cerca de 600 bytes ao uso de memória para cada elemento afetado. Quanto maior a árvore de elementos que você adiar, mais tempo de inicialização você economizará, mas ao custo de uma pegada de memória maior. Portanto, é possível usar esse atributo em excesso até o limite de redução do seu desempenho.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Comentários

As restrições para usar **x: DeferLoadStrategy** são:

- Você deve definir um [X:Name](x-name-attribute.md)para o elemento, como daí precisa ser uma maneira de encontrar o elemento depois.
- Você pode adiar somente tipos derivados de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) ou [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- Você não pode adiar elementos raiz em uma [**Página**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), um [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol), ou um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
- Você não pode adiar elementos em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
- Você não pode adiar o XAML solto carregado com [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Mover um elemento-pai apagará todos os elementos que não tenham sido realizados.

Existem várias maneiras de perceber os elementos diferidos:

- Chame [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) com o nome definido no elemento.
- Chame [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) com o nome definido no elemento.
- Em um [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), use uma animação [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou **Storyboard** que segmenta o elemento adiado.
- Segmente o elemento adiado em qualquer **Storyboard**.
- Use uma associação que esteja segmentando o elemento adiado.

> OBSERVAÇÃO: Depois que a instanciação de um elemento for iniciada, ele será criado no thread da interface do usuário, de maneira que faça a interface do usuário ser dividida se várias forem criadas de uma só vez.

Depois que um elemento adiado for criado por um dos métodos listados acima, várias coisas acontecerão:

- O evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) no elemento será acionado.
- Todas as associações no elemento serão avaliadas.
- Se o aplicativo tiver se registrado para receber notificações de alteração feita na propriedade sobre a propriedade que contém os elementos adiados, a notificação será acionada.

Você pode aniquilar elementos diferidos, no entanto, eles devem ser realizados a partir do elemento mais externo em. Se você tentar observar um elemento filho antes que o pai tenha sido percebido, uma exceção será gerada.

Normalmente, recomendamos que você adie elementos que não são visíveis no primeiro quadro.Uma boa orientação para encontrar os candidatos a serem diferidos é procurar elementos que estão sendo criados com [**Visibilidade**](https://msdn.microsoft.com/library/windows/apps/br208992) recolhida. Além disso, a UI que é desencadeada pela interação do usuário é um bom lugar para procurar elementos que você pode adiar.

Seja cauteloso ao adiar elementos em um [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), pois o tempo de inicialização será menor, mas também o desempenho do movimento panorâmico pode ser reduzido, dependendo do que você está criando. Se você estiver procurando melhorar o desempenho de movimento panorâmico, consulte a documentação sobre [extensão de marcação {x: Bind}](x-bind-markup-extension.md) e [atributo x:Phase](x-phase-attribute.md).

Se o [atributo x:Phase](x-phase-attribute.md) for usado em conjunto com **x:DeferLoadStrategy** quando um elemento ou uma árvore de elementos for realizado, as associações serão aplicadas até e incluindo a fase atual. A fase especificada para **x:Phase** não afeta ou controla o adiamento do elemento. Quando um item de lista é reciclado como parte de movimento panorâmico, os elementos realizados da mesma forma que outros elementos ativos e as associações compiladas (associações **{x:Bind}**) serão processados usando as mesmas regras, incluindo fases.

Uma diretriz geral é avaliar o desempenho do aplicativo antes e depois, para garantir que você obtenha o desempenho que deseja.

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
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```
