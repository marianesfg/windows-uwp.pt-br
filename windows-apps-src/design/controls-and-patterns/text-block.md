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
ms.openlocfilehash: 4f609f7ec989cf334d6b21a32ee8bde0e43203f0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081495"
---
# <a name="text-block"></a>Bloco de texto

 

 Bloco de texto é o controle principal para exibir texto somente leitura em aplicativos. Você pode usá-lo para exibir texto de linha única ou de várias linhas, hiperlinks embutidos e texto com formatação, como negrito, itálico ou sublinhado.
 
 > **APIs da plataforma**: [classe TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [propriedade Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text), [propriedade Inlines](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.inlines)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Um bloco de texto geralmente é mais fácil de usar e fornece melhor desempenho na renderização de texto do que um bloco Rich Text, por isso é o preferido para a maioria do texto da interface do usuário de aplicativo. Você pode facilmente acessar e usar o texto de um bloco de texto em seu aplicativo, obtendo o valor da propriedade [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text). Também fornece muitas das mesmas opções de formatação para personalizar como o texto será renderizado.

Embora você possa colocar quebras de linha no texto, o bloco de texto é projetado para exibir um único parágrafo e não dá suporte a recuo de texto. Use um **RichTextBlock** quando precisar de suporte para vários parágrafos, texto com várias colunas ou outros layouts de texto complexos ou elementos de interface do usuário embutidos como imagens.

Para obter mais informações sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TextBlock">abri-lo e ver o TextBlock em ação</a>.</p>
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

Há duas propriedades que você pode usar para adicionar conteúdo a um TextBlock: [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) e [Inlines](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.inlines).

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
- [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text): a condição mais importante é que o caminho rápido seja usado somente quando você definir o texto configurando explicitamente a propriedade Text, em XAML ou em código (como mostrado nos exemplos anteriores). Definir o texto pela coleção Inlines do TextBlock (como `<TextBlock>Inline text</TextBlock>`) desabilitará o caminho rápido, devido à complexidade potencial de vários formatos.
- [CharacterSpacing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.characterspacing): somente o valor padrão de 0 é o caminho rápido.
- [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming): somente os valores **None**, **CharacterEllipsis** e **WordEllipsis** são o caminho rápido. O valor **Clip** desabilita o caminho rápido.

> **Observação**&nbsp;&nbsp;Antes do Windows 10, versão 1607, as propriedades adicionais também afetam o caminho rápido. Se o aplicativo for executado em uma versão anterior do Windows, essas condições também farão o texto ser renderizado no caminho lento. Para saber mais sobre as versões, consulte Código adaptável de versão.
- [Typography](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Typography): somente os valores padrão das diversas propriedades Typography são o caminho rápido.
- [LineStackingStrategy](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy): se [LineHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) não for 0, os valores **BaselineToBaseline** e **MaxHeight** desabilitarão o caminho rápido.
- [IsTextSelectionEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextselectionenabled): somente **false** é o caminho rápido. Configurar essa propriedade como **true** desabilita o caminho rápido.

Você pode configurar a propriedade [DebugSettings.IsTextPerformanceVisualizationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled) como **true** durante a depuração para determinar se o texto está usando a renderização de caminho rápido. Quando essa propriedade é configurada como true, o texto que está no caminho rápido aparece na cor verde-claro.

>**Dica**&nbsp;&nbsp;Esse recurso é explicado em detalhes nesta sessão do Build 2015 - [Desempenho do XAML: técnicas para maximizar experiências dos Aplicativos Universais do Windows criados com XAML](https://channel9.msdn.com/Events/Build/2015/3-698).



Você geralmente define as configurações de depuração na substituição do método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) na página code-behind, desta forma.
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

O namespace [Windows.UI.Xaml.Documents](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) fornece uma variedade de elementos de texto embutidos que você pode usar para formatar o texto, como Bold, Italic, Run, Span e LineBreak.

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

As propriedades anexadas da classe [Typography](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Typography) fornecem acesso a um conjunto de propriedades de tipografia Microsoft OpenType. Você pode definir essas propriedades anexadas em TextBlock ou nos elementos de texto embutidos individuais. Estes exemplos mostram ambos.
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

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Controles de texto](text-controls.md)
- [Diretrizes para verificação ortográfica](text-controls.md)
- [Como adicionar pesquisa](search.md)
- [Diretrizes para entrada de texto](text-controls.md)
- [Classe TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Classe Windows.UI.Xaml.Controls PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propriedade String.Length](https://docs.microsoft.com/dotnet/api/system.string.length)
