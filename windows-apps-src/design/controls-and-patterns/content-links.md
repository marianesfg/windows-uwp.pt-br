---
Description: Use content links to embed rich data in your text controls.
title: Links de conteúdo em controles de texto
label: Content links
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: a984e30bbdc569522b04d328087775aa9e8ce2bc
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885219"
---
# <a name="content-links-in-text-controls"></a>Links de conteúdo em controles de texto

Links de conteúdo fornecem uma maneira de inserir dados avançados em seus controles de texto, o que permite que um usuário encontre e use mais informações sobre uma pessoa ou um local sem sair do contexto do seu aplicativo.

Quando o usuário prefixa uma entrada com um símbolo de e comercial (@) em uma RichEditBox, aparece uma lista de sugestões de pessoas e/ou locais que corresponde à entrada. Em seguida, por exemplo, quando o usuário seleciona um lugar, um ContentLink para esse local é inserido no texto. Quando o usuário invoca o link de conteúdo da RichEditBox, um submenu é mostrado com um mapa e informações adicionais sobre o lugar.

> **APIs importantes**: [classe ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink), [classe ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo), [classe RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> As APIs para links de conteúdo estão espalhadas pelos seguintes namespaces: Controls, Documents e UI.



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>Links de conteúdo em edição avançada versus controles de bloco de texto

Há duas maneiras distintas de usar links de conteúdo:

1. Em uma [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), o usuário pode abrir um seletor para adicionar um link de conteúdo por meio da prefixação de texto com um símbolo @. O link de conteúdo é armazenado como parte do conteúdo de texto avançado.
1. Em um [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) ou [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock), o link de conteúdo é um elemento de texto com uso e comportamento como um [hiperlink](/uwp/api/windows.ui.xaml.documents.hyperlink).

Veja a aparência de links de conteúdo por padrão em uma RichEditBox e em um TextBlock.

![link de conteúdo em caixa de edição avançada](images/content-link-default-richedit.png)
![link de conteúdo no bloco de texto](images/content-link-default-textblock.png)

Diferenças em uso, renderização e comportamento são abordadas em detalhes nas seções a seguir. Esta tabela dá uma rápida comparação das principais diferenças entre um link de conteúdo em uma RichEditBox e um bloco de texto.

| Recurso   | RichEditBox | bloco de texto |
| --------- | ----------- | ---------- |
| Uso | Instância ContentLinkInfo | Elemento de texto ContentLink |
| Cursor | Determinado pelo tipo de link de conteúdo, não pode ser alterado | Determinado pela propriedade Cursor, **null** por padrão |
| ToolTip | Não é renderizado | Mostra o texto secundário |

## <a name="enable-content-links-in-a-richeditbox"></a>Habilitar links de conteúdo em uma RichEditBox

O uso mais comum de um link de conteúdo é permitir que um usuário adicione informações rapidamente por meio da prefixação de um nome de pessoa ou local com um símbolo de e comercial (@) no texto. Quando habilitada em uma [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), isso abre um seletor e permite que o usuário insira uma pessoa de sua lista de contatos ou um local nas proximidades, dependendo do que você ativou.

O link de conteúdo pode ser salvo com o conteúdo de texto avançado, e você pode extrair para usar em outras partes do seu aplicativo. Por exemplo, em um aplicativo de email, você pode extrair as informações da pessoa e usá-las para preencher a caixa Para com um endereço de email.

> [!NOTE]
> O seletor de link de conteúdo é um aplicativo que faz parte do Windows, portanto, ele é executado em um processo separado do seu aplicativo.

### <a name="content-link-providers"></a>Provedores de link de conteúdo

Para habilitar links de conteúdo em uma RichEditBox, adicione um ou mais provedores de link de conteúdo à coleção [RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders). Há 2 provedores de link de conteúdo incorporados na estrutura XAML.

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider) – pesquisa contatos usando o aplicativo **Pessoas**.
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider) – pesquisa locais usando o aplicativo **Mapas**.

> [!IMPORTANT]
> O valor padrão da propriedade RichEditBox.ContentLinkProviders é **null**, não é uma coleção vazia. Você precisa criar explicitamente o [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection) antes de adicionar provedores de link de conteúdo.

Veja como adicionar os provedores de link de conteúdo em XAML.

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

Você também pode adicionar provedores de link de conteúdo em um estilo e aplicá-lo a várias RichEditBoxes desta forma.

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

Veja como adicionar os provedores de link de conteúdo em código.

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>Cores dos links de conteúdo

A aparência de um link de conteúdo é determinada pelo seu primeiro plano, plano de fundo e ícone. Em uma RichEditBox, você pode definir as propriedades [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) e [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) para alterar as cores padrão. 

Você não pode definir o cursor. O cursor é renderizado pela RichEditbox com base no tipo de link de conteúdo - um cursor [Pessoa](/uwp/api/windows.ui.core.corecursortype) para um link de pessoa, ou um cursor [Pin](/uwp/api/windows.ui.core.corecursortype) para um link de lugar.

### <a name="the-contentlinkinfo-object"></a>O objeto ContentLinkInfo

Quando o usuário faz uma seleção no seletor de pessoas ou locais, o sistema cria um objeto [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo) e o adiciona à propriedade **ContentLinkInfo** do atual [RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange).

O objeto ContentLinkInfo contém as informações usadas para exibir, invocar e gerenciar o link de conteúdo.

- **DisplayText** – Cadeia de caracteres que é mostrada quando o link de conteúdo é renderizado. Em uma RichEditBox, o usuário pode editar o texto de um link de conteúdo após ser criado, o que altera o valor dessa propriedade.
- **SecondaryText** – Essa cadeia de caracteres é mostrada na dica de ferramenta de um link de conteúdo renderizado.
  - Em um link de conteúdo de lugar criado pelo seletor, ele contém o endereço do local, se disponível.
- **URI** – O link para obter mais informações sobre o assunto do link de conteúdo. Esse Uri pode abrir um aplicativo instalado ou um site.
- **Id** - Contador somente leitura, por controle, criado pelo controle RichEditBox. Ele é usado para acompanhar esse ContentLinkInfo durante ações como excluir ou editar. Se ContentLinkInfo for recortar e colar no controle, ele receberá um novo ID. Os valores de Id são incrementais.
- **LinkContentKind** – Cadeia de caracteres que descreve o tipo de link de conteúdo. Os tipos de conteúdo internos são _Locais_ e _Contatos _. A valor diferencia maiúsculas de minúsculas.

#### <a name="link-content-kind"></a>Tipo de conteúdo de link

Existem várias situações em que o LinkContentKind é importante.

- Quando um usuário copia um link de conteúdo de RichEditBox e cola em outra RichEditBox, ambos os controles devem ter um ContentLinkProvider para esse tipo de conteúdo. Caso contrário, o link é colado como texto.
- Você pode usar LinkContentKind em um manipulador de eventos [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) para determinar o que fazer com um link de conteúdo quando você usá-lo em outras partes do seu aplicativo. Veja a seção Exemplo para mais informações.
- O LinkContentKind influencia como o sistema abre o Uri quando o link é invocado. Veremos isso na discussão de inicialização do Uri em seguida.

#### <a name="uri-launching"></a>Inicialização de Uri

A propriedade Uri funciona como a propriedade NavigateUri de um hiperlink. Quando um usuário clica nela, ela inicia o Uri no navegador padrão ou no aplicativo que está registrado para o protocolo especificado no valor do Uri.

O comportamento específico dos 2 tipos de conteúdo de link é descrito aqui.

##### <a name="places"></a>Lugares

O seletor de lugares cria um ContentLinkInfo com uma raiz de Uri de https://maps.windows.com/. Este link pode ser aberto de 3 modos:

- Se LinkContentKind = "Locais", ele abre um cartão de informações em um submenu. O submenu é semelhante ao submenu do seletor de link de conteúdo. Ele faz parte do Windows e é executado em um processo separado do seu aplicativo.
- Se LinkContentKind não for "Locais", ele tenta abrir o aplicativo **Mapas** no local especificado. Por exemplo, isso pode acontecer se você tiver modificado LinkContentKind no manipulador de eventos ContentLinkChanged.
- Se o Uri não puder ser aberto no aplicativo Mapas, o mapa será aberto no navegador padrão. Isso normalmente acontece quando as configurações _aplicativos para sites_ do usuário não permitirem abrir o Uri com o aplicativo **Mapas**.

##### <a name="people"></a>Pessoas

O seletor de pessoas cria um ContentLinkInfo com um Uri que usa o protocolo **ms-people**.

- Se LinkContentKind = "Pessoas", ele abre um cartão de informações em um submenu. O submenu é semelhante ao submenu do seletor de link de conteúdo. Ele faz parte do Windows e é executado em um processo separado do seu aplicativo.
- Se LinkContentKind não for "People", ele abre o aplicativo **Pessoas**. Por exemplo, isso pode acontecer se você tiver modificado LinkContentKind no manipulador de eventos ContentLinkChanged.

> [!TIP]
> Para obter mais informações sobre como abrir outros aplicativos e sites de seu aplicativo, consulte os tópicos em [Iniciar um aplicativo com um Uri](/windows/uwp/launch-resume/launch-app-with-uri).

#### <a name="invoked"></a>Invocado

Quando o usuário invoca um link de conteúdo, o evento [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked) é acionado antes que o comportamento padrão de iniciar o Uri ocorra. Você pode manipular esse evento para substituir ou cancelar o comportamento padrão.

Este exemplo mostra como você pode substituir o comportamento padrão de inicialização o de um link de conteúdo de Local. Em vez de abrir o mapa em um cartão de informações de lugar, no aplicativo Mapas ou um navegador da Web padrão, você pode marcar o evento como Handled e abrir o mapa em um controle interno de aplicativo [WebView](/uwp/api/windows.ui.xaml.controls.webview).

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

Você pode usar o evento [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) para ser notificado quando um link de conteúdo é adicionado, modificado ou removido. Isso permite que você extraia o link de conteúdo do texto e o use em outros lugares no seu aplicativo. Isto é exibido posteriormente na seção Exemplos.

As propriedades do [ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs) são:

- **ChangedKind** - Essa enumeração ContentLinkChangeKind é a ação dentro do ContentLink. Por exemplo, se o ContentLink é inserido, removido ou editado.
- **Info** - ContentLinkInfo que era o destino da ação.

Esse evento é gerado para cada ação ContentLinkInfo. Por exemplo, se o usuário copia e cola vários links de conteúdo em RichEditBox ao mesmo tempo, esse evento é gerado para cada item adicionado. Ou, se o usuário seleciona e exclui vários links de conteúdo ao mesmo tempo, esse evento é gerado para cada item excluído.

Esse evento é acionado em RichEditBox após o evento **TextChanging** e antes do evento **TextChanged**.

#### <a name="enumerate-content-links-in-a-richeditbox"></a>Enumerar links de conteúdo em uma RichEditBox

Você pode usar a propriedade RichEditTextRange.ContentLink para obter um link de conteúdo de um documento de edição. A enumeração TextRangeUnit tem o valor ContentLink para especificar o link de conteúdo como uma unidade para usar ao navegar em um intervalo de texto.

Este exemplo mostra como você pode enumerar todos os links de conteúdo em uma RichEditBox e extrair as pessoas em uma lista.

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>Elemento de texto ContentLink

Para usar um link de conteúdo em um controle TextBlock ou RichTextBlock, use o elemento de texto [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink) (no namespace Windows.UI.Xaml.Documents).

Fontes típicas para um ContentLink em um bloco de texto são:

- Um link de conteúdo criado por um seletor extraído de um controle RichTextBlock.
- Um link de conteúdo para um local que você criar no seu código.

Para outras situações, um elemento de texto de hiperlink é geralmente apropriado.

### <a name="contentlink-appearance"></a>Aparência do ContentLink

A aparência de um link de conteúdo é determinada pelo seu primeiro plano, plano de fundo e cursor. Em um bloco de texto, você pode definir as propriedades Foreground (no TextElement) e [Background](/uwp/api/windows.ui.xaml.documents.contentlink.background) para alterar as cores padrão.

Por padrão, o cursor [Mão](/uwp/api/windows.ui.core.corecursortype) é mostrado quando o usuário passa sobre o link de conteúdo. Ao contrário de RichEditBlock, controles de bloco de texto não mudam o cursor automaticamente com base no tipo de link. Você pode definir a propriedade [Cursor](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor) para alterar o cursor com base no tipo de link ou outros fatores.

Veja um exemplo de um ContentLink usado em um TextBlock. O ContentLinkInfo é criado no código e atribuído à propriedade Info do elemento de texto ContentLink que é criado em XAML.

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> Quando você usa um ContentLink em um controle de texto com outros elementos de texto no XAML, coloque o conteúdo em um contêiner [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) e aplicar o atributo `xml:space="preserve"` para manter o espaço em branco entre o ContentLink e os outros elementos.

## <a name="examples"></a>Exemplos

Neste exemplo, um usuário pode inserir um link de conteúdo de pessoa ou lugar em um RickTextBlock. Manipule o evento ContentLinkChanged para extrair os links de conteúdo e mantê-los atualizados em uma lista de pessoas ou lista de locais.

Nos modelos de item para as listas, você usa um TextBlock com um elemento de texto ContentLink para mostrar as informações de link de conteúdo. O ListView fornece sua própria tela de fundo para cada item, para que a tela de fundo ContentLink está definida como Transparent.

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```
