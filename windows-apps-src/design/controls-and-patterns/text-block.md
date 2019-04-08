---
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
Description: Bloco de texto é o controle principal para exibir texto somente leitura em aplicativos.
title: Bloco de texto
label: Text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 692b8837f3bd74dfc5f74bee02786213c9a898f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599381"
---
# <a name="text-block"></a>Bloco de texto

 

 Bloco de texto é o controle principal para exibir texto somente leitura em aplicativos. Você pode usá-lo para exibir texto de linha única ou de várias linhas, hiperlinks embutidos e texto com formatação, como negrito, itálico ou sublinhado.
 
 > **APIs importantes**: [Classe TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [propriedade Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx), [da propriedade Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Um bloco de texto geralmente é mais fácil de usar e fornece melhor desempenho na renderização de texto do que um bloco Rich Text, por isso é o preferido para a maioria do texto da interface do usuário de aplicativo. Você pode facilmente acessar e usar o texto de um bloco de texto em seu aplicativo, obtendo o valor da propriedade [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx). Também fornece muitas das mesmas opções de formatação para personalizar como o texto será renderizado.

Embora você possa colocar quebras de linha no texto, o bloco de texto é projetado para exibir um único parágrafo e não oferece suporte a recuo de texto. Use um **RichTextBlock** quando precisar de suporte para vários parágrafos, texto com várias colunas ou outros layouts de texto complexos ou elementos de interface do usuário embutidos como imagens.

Para obter mais informações sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TextBlock">abrir o aplicativo e ver o TextBlock em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-text-block"></a>Criar um bloco de texto

Consulte aqui como definir um controle TextBlock simples e configurar sua propriedade Text para uma cadeia de caracteres.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### <a name="content-model"></a>Modelo de conteúdo

Há duas propriedades que você pode usar para adicionar conteúdo a um TextBlock: [Texto](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) e [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx).

A maneira mais comum de exibir texto é definir a propriedade Text como um valor da cadeia de caracteres, conforme mostrado no exemplo anterior.

Você também pode adicionar conteúdo ao colocar elementos de conteúdo de fluxo embutidos na propriedade TextBox.Inlines, dessa forma.
```xaml
<TextBlock>Text can be <Bold>bold</Bold>, <Underline>underlined</Underline>, 
    <Italic>italic</Italic>, or a <Bold><Italic>combination</Italic></Bold>.</TextBlock>
```

Elementos derivados da classe Inline, como Bold, Italic, Run, Span e LineBreak, permitem formatações diferentes para diferentes partes do texto. Para obter mais informações, consulte a seção [Formatação do texto](#formatting-text). O elemento Hyperlink embutido permite adicionar um hiperlink ao seu texto. No entanto, usar Inlines também desabilita a renderização de texto de caminho rápido, que será discutida na próxima seção.


## <a name="performance-considerations"></a>Considerações sobre desempenho

Sempre que possível, o XAML usa um caminho de código mais eficiente para texto de layout. Esse caminho rápido reduz o uso de memória em geral e reduz consideravelmente o tempo da CPU para medir e organizar o texto. Esse caminho rápido aplica-se somente ao TextBlock. Portanto, sempre que possível, ele deve ter preferência sobre RichTextBlock.

Determinadas condições exigem que o TextBlock faça fallback em caminho de código com mais recursos e uso intenso de CPU para renderização de texto. Para manter a renderização de texto no caminho rápido, siga estas diretrizes para configurar as propriedades listadas aqui.
- [Texto](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): A condição mais importante é que o caminho rápido é usado somente quando você define o texto definindo explicitamente a propriedade de texto, em XAML ou no código (conforme mostrado nos exemplos anteriores). Definir o texto pela coleção Inlines do TextBlock (como `<TextBlock>Inline text</TextBlock>`) desabilitará o caminho rápido, devido à complexidade potencial de vários formatos.
- [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): Somente o valor padrão de 0 é o caminho rápido.
- [TextTrimming](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): Somente o **None**, **CharacterEllipsis**, e **WordEllipsis** valores são o caminho rápido. O valor **Clip** desabilita o caminho rápido.

> **Observação**&nbsp;&nbsp;Antes do Windows 10, versão 1607, as propriedades adicionais também afetam o caminho rápido. Se o aplicativo for executado em uma versão anterior do Windows, essas condições também farão o texto ser renderizado no caminho lento. Para saber mais sobre as versões, consulte Código adaptável de versão.
- [Tipografia](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): Somente os valores padrão para as várias propriedades de tipografia são caminho rápido.
- [LineStackingStrategy](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): Se [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) é não em 0, o **BaselineToBaseline** e **MaxHeight** valores desabilitar o caminho rápido.
- [IsTextSelectionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): Somente **falsos** é o caminho rápido. Configurar essa propriedade como **true** desabilita o caminho rápido.

Você pode configurar a propriedade [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) como **true** durante a depuração para determinar se o texto está usando a renderização de caminho rápido. Quando essa propriedade é configurada como true, o texto que está no caminho rápido aparece na cor verde-claro.

>**Dica**&nbsp;&nbsp;esse recurso é explicado em detalhes nesta sessão da Build 2015 - [desempenho XAML: Técnicas para maximizar a experiências de aplicativo Universal do Windows criados com XAML](https://channel9.msdn.com/Events/Build/2015/3-698).



Você geralmente define as configurações de depuração na substituição do método [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) na página code-behind, desta forma.
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

Neste exemplo, o primeiro TextBlock é renderizado usando o caminho rápido, enquanto o segundo não é.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

Quando você executa esse XAML no modo de depuração com IsTextPerformanceVisualizationEnabled configurado como true, o resultado é semelhante a este.

![Texto renderizado no modo de depuração](images/text-block-rendering-performance.png)

>**Cuidado**&nbsp;&nbsp;A cor do texto que não está no caminho rápido não é alterada. Se você tiver texto em seu aplicativo com a cor especificada como verde brilhante, ele ainda será exibido em verde brilhante quando estiver no caminho de renderização mais lento. Tome cuidado para não confundir o texto definido como verde no aplicativo com o texto que está no caminho rápido e verde devido às configurações de depuração.

## <a name="formatting-text"></a>Formatação do texto

Embora a propriedade Text armazene texto sem formatação, você pode aplicar várias opções de formatação no controle TextBlock para personalizar como o texto será renderizado em seu aplicativo. Você pode definir propriedades de controle padrão como FontFamily, FontSize, FontStyle, Foreground e CharacterSpacing para alterar a aparência do texto. Você também pode usar os elementos de texto embutidos e propriedades anexadas Typography para formatar seu texto. Essas opções afetam apenas como TextBlock exibe o texto localmente, portanto, se você copiar e colar o texto em um controle rich text, por exemplo, nenhuma formatação será aplicada.

>**Observação**&nbsp;&nbsp;Lembre-se de que, conforme observado na seção anterior, elementos de texto embutidos e valores de tipografia não padrão não são renderizados no caminho rápido.


### <a name="inline-elements"></a>Elementos embutidos

O namespace [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) fornece uma variedade de elementos de texto embutidos que você pode usar para formatar o texto, como Bold, Italic, Run, Span e LineBreak.

Você pode exibir uma série de cadeias de caracteres em um TextBlock, em que cada cadeia tem formatação diferente. É possível fazer isso usando um elemento Run para exibir cada cadeia com a sua formatação e separando cada elemento Run com um elemento LineBreak.

Consulte como definir algumas cadeias de caracteres de texto formatadas diferentemente em um TextBlock usando Run objetos separados com um LineBreak.
```xaml
<TextBlock FontFamily="Segoe UI" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Segoe UI Light" FontSize="24">
        Segoe UI Light 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Georgia" FontSize="18" FontStyle="Italic">
        Georgia Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="Black" FontFamily="Arial" FontSize="14" FontWeight="Bold">
        Arial Bold 14
    </Run>
</TextBlock>
```

Consulte o resultado.

![Texto formatado com elementos de execução](images/text-block-run-examples.png)

### <a name="typography"></a>Tipografia

As propriedades anexadas da classe [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) fornecem acesso a um conjunto de propriedades de tipografia Microsoft OpenType. Você pode definir essas propriedades anexadas em TextBlock ou nos elementos de texto embutidos individuais. Estes exemplos mostram ambos.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Controles de texto](text-controls.md)
- [Diretrizes para verificação ortográfica](text-controls.md)
- [Adicionando uma pesquisa](search.md)
- [Diretrizes para a entrada de texto](text-controls.md)
- [Classe de caixa de texto](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe PasswordBox Windows](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriedade String. Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
