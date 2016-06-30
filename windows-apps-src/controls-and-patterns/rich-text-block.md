---
author: Jwmsft
Description: "Use RichTextBlock com elementos RichTextBlockOverflow para criar layouts de texto avançados."
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 28c78b39bad4c66457ec5aba8cf0b4ce0de4f00a

---
# Bloco Rich Text
Blocos Rich Text fornecem vários recursos para layout de texto avançado que você pode usar quando precisa de suporte para parágrafos, elementos de interface do usuário embutidos ou layouts de texto complexos.


<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [**Classe RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)
-   [**Classe RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)
-   [**Classe Paragraph**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)
-   [**Classe Typography**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## Esse é o controle correto?

Use um **RichTextBlock** quando precisar de suporte para vários parágrafos, várias colunas ou outros layouts de texto complexos ou elementos de interface do usuário embutidos, como imagens.

Use um **TextBlock** para exibir mais texto somente leitura em seu aplicativo. Você pode usá-lo para exibir texto de linha única ou de várias linhas, hiperlinks embutidos e texto com formatação, como negrito, itálico ou sublinhado. TextBlock fornece um modelo de conteúdo mais simples e, portanto, é mais fácil de usar e pode fornecer melhor desempenho na renderização de texto do que RichTextBlock. Esse é o controle preferencial para a maioria dos textos de interface do usuário de aplicativos. Embora você possa colocar quebras de linha no texto, TextBlock é projetado para exibir um único parágrafo e não oferece suporte a recuo de texto.

Para saber mais sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## Exemplos


## Criar um bloco Rich Text

A propriedade de conteúdo de RichTextBlock é a propriedade [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), que oferece suporte a texto baseado em parágrafos por meio do elemento [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Ele não tem uma propriedade **Text** que você pode usar para acessar facilmente o conteúdo de texto do controle em seu aplicativo. No entanto, RichTextBlock fornece vários recursos exclusivos que TextBlock não fornece. 

RichTextBlock oferece suporte a:
- Vários parágrafos. Defina o recuo de parágrafos definindo a propriedade [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx).
- Elementos de interface do usuário embutidos. Use [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) para exibir elementos de interface do usuário, como imagens, embutidos no seu texto.
- Contêineres de excedente. Use elementos [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) para criar layouts de texto de várias colunas.

### Parágrafos

Use elementos [**Paragraph**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) para definir os blocos de texto a serem exibidos dentro de um controle RichTextBlock. Cada RichTextBlock deve incluir pelo menos um Paragraph. 

Você pode definir o valor de recuo para todos os parágrafos em RichTextBlock definindo a propriedade [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx). Você pode substituir essa configuração para parágrafos específicos em RichTextBlock definindo a propriedade [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) como um valor diferente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### Elementos de interface do usuário embutidos

A classe [**InlineUIContainer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) permite inserir qualquer UIElement embutido no seu texto. Um cenário comum é colocar uma Imagem embutida no texto, mas você também pode usar elementos interativos, como Button ou CheckBox.

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

## Contêineres de excedente

Você pode usar RichTextBlock com elementos [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) para criar layouts de várias colunas ou outros layouts de página avançados. O conteúdo de um elemento RichTextBlockOverflow sempre vem de um elemento RichTextBlock. Vincule elementos RichTextBlockOverflow definindo-os como OverflowContentTarget de RichTextBlock ou outro RichTextBlockOverflow.

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

## Formatação do texto

Embora RichTextBlock armazene texto sem formatação, você pode aplicar várias opções de formatação para personalizar como o texto será renderizado no aplicativo. Você pode definir propriedades de controle padrão como FontFamily, FontSize, FontStyle, Foreground e CharacterSpacing para alterar a aparência do texto. Você também pode usar os elementos de texto embutidos e propriedades anexadas Typography para formatar seu texto. Essas opções afetam apenas como RichTextBlock exibe o texto localmente, portanto, se você copiar e colar o texto em um controle Rich Text, por exemplo, nenhuma formatação será aplicada.

### Elementos embutidos

O namespace [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) fornece uma variedade de elementos de texto embutidos que você pode usar para formatar o texto, como Bold, Italic, Run, Span e LineBreak. Uma maneira comum de aplicar formatação a seções de texto é colocar o texto em um elemento Run ou Span e, em seguida, definir propriedades nesse elemento.

Veja a seguir um Parágrafo com a primeira frase em texto em negrito, azul, 16 pt.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### Typography

As propriedades anexadas da classe [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) fornecem acesso a um conjunto de propriedades de tipografia Microsoft OpenType. Você pode definir essas propriedades anexadas em RichTextBlock ou nos elementos de texto embutidos individuais, conforme mostrado aqui.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## Recomendações

Consulte Tipografia e Diretrizes para fontes.



## Artigos relacionados

[Controles de texto](text-controls.md)

**Para designers**
- [Diretrizes para verificação ortográfica](spell-checking-and-prediction.md)
- [Adicionando pesquisa](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Diretrizes para entrada de texto](text-controls.md)

**Para desenvolvedores (XAML)**
- [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Classe Windows.UI.Xaml.Controls PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519)


**Para desenvolvedores (outros)**
- [Propriedade String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)



<!--HONumber=Jun16_HO4-->


