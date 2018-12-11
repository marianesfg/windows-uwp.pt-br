---
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3d71a7995bc56a87e6a9b0fc53ff35fdebe82596
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8897584"
---
# <a name="rich-text-block"></a>Bloco de rich text

 

Blocos Rich Text fornecem vários recursos para layout de texto avançado que você pode usar quando precisa de suporte para parágrafos, elementos de interface do usuário embutidos ou layouts de texto complexos.

> **APIs importantes**: [classe RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [classe RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx), [classe Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx), [classe Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use um **RichTextBlock** quando precisar de suporte para vários parágrafos, várias colunas ou outros layouts de texto complexos ou elementos de interface do usuário embutidos, como imagens.

Use um **TextBlock** para exibir mais texto somente leitura em seu aplicativo. Você pode usá-lo para exibir texto de linha única ou de várias linhas, hiperlinks embutidos e texto com formatação, como negrito, itálico ou sublinhado. TextBlock fornece um modelo de conteúdo mais simples e, portanto, é mais fácil de usar e pode fornecer melhor desempenho na renderização de texto do que RichTextBlock. Esse é o controle preferencial para a maioria dos textos de interface do usuário de aplicativos. Embora você possa colocar quebras de linha no texto, TextBlock é projetado para exibir um único parágrafo e não oferece suporte a recuo de texto.

Para obter mais informações sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RichTextBlock">abrir o aplicativo e ver o RichTextBlock em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>Criar um bloco Rich Text

A propriedade de conteúdo de RichTextBlock é a propriedade [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), que oferece suporte a texto baseado em parágrafos por meio do elemento [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Ele não tem uma propriedade **Text** que você pode usar para acessar facilmente o conteúdo de texto do controle em seu aplicativo. No entanto, RichTextBlock fornece vários recursos exclusivos que TextBlock não fornece. 

RichTextBlock oferece suporte a:
- Vários parágrafos. Defina o recuo de parágrafos definindo a propriedade [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx).
- Elementos de interface do usuário embutidos. Use [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) para exibir elementos de interface do usuário, como imagens, embutidos no seu texto.
- Contêineres de excedente. Use elementos [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) para criar layouts de texto de várias colunas.

### <a name="paragraphs"></a>Parágrafos

Você usa os elementos [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) para definir os blocos de texto a serem exibidos dentro de um controle RichTextBlock. Cada RichTextBlock deve incluir pelo menos um Paragraph. 

Você pode definir o valor de recuo para todos os parágrafos em RichTextBlock definindo a propriedade [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx). Você pode substituir essa configuração para parágrafos específicos em RichTextBlock definindo a propriedade [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) como um valor diferente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>Elementos de interface do usuário embutidos

A classe [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) permite inserir qualquer UIElement embutido no seu texto. Um cenário comum é colocar uma Imagem embutida no texto, mas você também pode usar elementos interativos, como Button ou CheckBox.

Se quiser inserir mais de um elemento embutido na mesma posição, considere usar um painel como o único InlineUIContainer filho e, em seguida, coloque os vários elementos dentro desse painel.

Este exemplo mostra como usar InlineUIContainer para inserir uma imagem em RichTextBlock. 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>Contêineres de excedente

Você pode usar RichTextBlock com elementos [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) para criar layouts de várias colunas ou outros layouts de página avançados. O conteúdo de um elemento RichTextBlockOverflow sempre vem de um elemento RichTextBlock. Vincule elementos RichTextBlockOverflow definindo-os como OverflowContentTarget de RichTextBlock ou outro RichTextBlockOverflow.

Consulte um exemplo simples que cria um layout de duas colunas. Veja a seção Exemplos para obter um exemplo mais complexo.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>Formatação do texto

Embora RichTextBlock armazene texto sem formatação, você pode aplicar várias opções de formatação para personalizar como o texto será renderizado no aplicativo. Você pode definir propriedades de controle padrão como FontFamily, FontSize, FontStyle, Foreground e CharacterSpacing para alterar a aparência do texto. Você também pode usar os elementos de texto embutidos e propriedades anexadas Typography para formatar seu texto. Essas opções afetam apenas como RichTextBlock exibe o texto localmente, portanto, se você copiar e colar o texto em um controle Rich Text, por exemplo, nenhuma formatação será aplicada.

### <a name="inline-elements"></a>Elementos embutidos

O namespace [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) fornece uma variedade de elementos de texto embutidos que você pode usar para formatar o texto, como Bold, Italic, Run, Span e LineBreak. Uma maneira comum de aplicar formatação a seções de texto é colocar o texto em um elemento Run ou Span e, em seguida, definir propriedades nesse elemento.

Veja a seguir um Parágrafo com a primeira frase em texto em negrito, azul, 16 pt.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Typography

As propriedades anexadas da classe [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) fornecem acesso a um conjunto de propriedades de tipografia Microsoft OpenType. Você pode definir essas propriedades anexadas em RichTextBlock ou nos elementos de texto embutidos individuais, conforme mostrado aqui.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>Recomendações

Consulte Tipografia e Diretrizes para fontes.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

[Controles de texto](text-controls.md)

**Para designers**
- [Diretrizes para verificação ortográfica](text-controls.md)
- [Adicionando pesquisa](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Diretrizes para entrada de texto](text-controls.md)

**Para desenvolvedores (XAML)**
- [Classe TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe Windows.UI.Xaml.Controls PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)


**Para desenvolvedores (outros)**
- [Propriedade String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
