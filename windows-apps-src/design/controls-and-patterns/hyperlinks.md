---
Description: Os hiperlinks levam o usuário para outra parte do aplicativo, para outro aplicativo ou para iniciar um URI (Uniform Resource Identifier) específico usando um aplicativo de navegador separado.
title: Hiperlinks
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: fa4ff0cbc2bd361b241f660f9c6b28f03bc7c24c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80080887"
---
# <a name="hyperlinks"></a>Hiperlinks

Os hiperlinks levam o usuário para outra parte do aplicativo, para outro aplicativo ou para iniciar um URI (Uniform Resource Identifier) específico usando um aplicativo de navegador separado. Há duas formas de se adicionar um hiperlink a um aplicativo XAML: o elemento de texto **Hyperlink** e o controle **HyperlinkButton**.

> **APIs da plataforma**: [elemento de texto de Hiperlink](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Hyperlink), [controle HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)

![Um botão de hiperlink](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um hiperlink quando você precisar de texto que responda quando selecionado e o usuário navegar para obter mais informações sobre o texto que foi selecionado.

Escolha o tipo correto de hiperlink com base em suas necessidades:

-   Use um elemento de texto **Hyperlink** embutido dentro de um controle de texto. Um elemento Hyperlink flui com outros elementos de texto e você pode usá-lo em qualquer InlineCollection. Use um hiperlink de texto se quiser uma quebra de texto automática e não precisar necessariamente de um destino de toque grande. O texto do hiperlink pode ser pequeno e ser difícil de direcionar, especialmente para toque.
-   Use um **HyperlinkButton** para hiperlinks autônomos. Um HyperlinkButton é um controle de botão especializado que você pode usar em qualquer lugar onde usaria um botão.
-   Use um **HyperlinkButton** com uma [(Imagem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) como seu conteúdo para criar uma imagem clicável.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/HyperlinkButton">abrir o aplicativo e ver o HyperlinkButton em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>Criar um elemento de texto de hiperlink

Este exemplo mostra como usar um elemento de texto de hiperlink dentro de um [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock).

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
O hiperlink aparece embutido e flui com o texto ao redor:

![Exemplo de um hiperlink como um elemento de texto](images/controls_hyperlink-element.png) 

> **Dica**&nbsp;&nbsp;Quando você usar um hiperlink em um controle de texto com outros elementos de texto no XAML, coloque o conteúdo em um contêiner [Span](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.span) e aplique o atributo `xml:space="preserve"` ao Span para manter o espaço em branco entre o hiperlink e os outros elementos.

## <a name="create-a-hyperlinkbutton"></a>Criar um HyperlinkButton

Abaixo estão as instruções de como usar um HyperlinkButton com texto e com uma imagem.

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```

Os botões de hiperlink com conteúdo de texto aparecem como texto marcado. A imagem do logotipo Contoso também é um hiperlink clicável:

![Exemplo de um hiperlink como um controle de botão](images/controls_hyperlink-button-image.png)

Este exemplo mostra como criar um HyperlinkButton no código.

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>Manipulas a navegação

Para os dois tipos de hiperlinks, você manipula a navegação da mesma maneira. Você pode definir a propriedade **NavigateUri** ou manipular o evento **Click**.

**Navegar até um URI**

Para usar o hiperlink para navegar para um URI, defina a propriedade NavigateUri. Quando um usuário clica ou toca no hiperlink, o URI especificado é aberto no navegador padrão. O navegador padrão é executado em um processo à parte do aplicativo.

> [!NOTE]
> Um URI é representado pela classe [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri). Ao programar com .NET, essa classe fica oculta e você terá que usar a classe [System.Uri](https://docs.microsoft.com/dotnet/api/system.uri). Para obter mais informações, confira as páginas de referência para essas classes.

Você não precisa usar esquemas **http:** nem **https:** . Poderá usar esquemas como **ms-appx:** , **ms-appdata:** ou **ms-resources:** se houver conteúdo de recurso nesses locais que seja apropriado carregar em um navegador. No entanto, o esquema **file:** é especificamente bloqueado. Para obter mais informações, consulte [Esquemas de URI](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)).

Quando um usuário clica no hiperlink, o valor da propriedade NavigateUri é passado para um manipulador do sistema para esquemas e tipos de URI. Em seguida, o sistema inicia o aplicativo que está registrado para o esquema do URI fornecido para NavigateUri.

Se você não quiser que o hiperlink carregue conteúdo em um navegador da Web padrão (e não quiser que um navegador apareça), não defina um valor para NavigateUri. Em vez disso, manipule o evento Click e escreva código que faça o que você quer.


**Manipular o evento Click**

Use o evento Click para ações que não sejam iniciar um URI em um navegador, como navegação dentro do aplicativo. Por exemplo, se você quiser carregar uma nova página de aplicativo, em vez de abrir um navegador, chame um método [Frame.Navigate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) dentro de seu manipulador de eventos Click para navegar para a nova página do aplicativo. Se você quiser que um URI absoluto externo seja carregado dentro de um controle de [WebView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview) que também existe em seu aplicativo, chame [WebView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigate) como parte da lógica de seu manipulador de cliques.

Você normalmente não manipula o evento Click tão bem quanto especifica um valor de NavigateUri, uma vez que esses representam duas maneiras diferentes de usar o elemento hyperlink. Se sua intenção for abrir o URI no navegador padrão, e você tiver especificado um valor para NavigateUri, não manipule o evento Click. Por outro lado, se você manipular o evento Click, não especifique um NavigateUri.

Não há nada que você pode fazer no manipulador de eventos Click para impedir que o navegador padrão carregue qualquer destino válido especificado para NavigateUri. Essa ação ocorre automaticamente (de forma assíncrona) quando o hiperlink é ativado e não pode ser cancelada de dentro do manipulador de eventos Click. 

## <a name="hyperlink-underlines"></a>Sublinhados de hiperlink
Por padrão, os hiperlinks são sublinhados. Esse sublinhado é importante porque ele ajuda a atender a requisitos de acessibilidade. Usuários daltônicos usam o sublinhado para distinguir entre os hiperlinks e outros textos. Se você desabilitar os sublinhados, deverá considerar a adição de algum outro tipo de formatação diferente para distinguir hiperlinks de outros textos, como FontWeight ou FontStyle.

**Elementos de texto de hiperlink**

Você pode definir a propriedade [UnderlineStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.hyperlink.underlinestyle) para desabilitar o sublinhado. Se o fizer, considere o uso de [FontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement.fontweight) ou de [FontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement.fontstyle) para diferenciar o texto do link.

**HyperlinkButton** 

Por padrão, o HyperlinkButton aparece como texto sublinhado quando você configura uma cadeia de caracteres como o valor da propriedade [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content).

O texto não aparece sublinhado nos seguintes casos:
- Você define um [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) como o valor da propriedade Content e define a propriedade [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) no TextBlock.
- Você remodela o HyperlinkButton e altera o nome da parte do modelo [ContentPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentpresenter).

Se você precisar de um botão que apareça como texto sem sublinhado, considere usar um controle de botão padrão e aplicar o recurso interno do sistema `TextBlockButtonStyle` para sua propriedade Style.

## <a name="notes-for-hyperlink-text-element"></a>Observações para o elemento de texto de hiperlink

Esta seção aplica-se apenas ao elemento de texto de hiperlink, não para o controle HyperlinkButton.

**Eventos de entrada**

Como um hiperlink não é um [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), ele não tem o conjunto de eventos de entrada de elemento da interface do usuário, como Tapped, PointerPressed e assim por diante. Em vez disso, um hiperlink tem seu próprio evento Click, além do comportamento implícito do sistema que carrega qualquer URI especificado como o NavigateUri. O sistema manipula todas as ações de entrada que devem invocar as ações de hiperlink e aciona o evento Click em resposta.

**Conteúdo**

O hiperlink tem restrições sobre o conteúdo que podem existir na sua coleção [Inlines](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.span.inlines). Especificamente, um hiperlink só permite [Run](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.run) e outros tipos de [Span](/uwp/api/windows.ui.xaml.documents.span) que não são outro hiperlink. [InlineUIContainer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.inlineuicontainer) não pode estar na coleção Inlines de um hiperlink. Tentar adicionar conteúdo restrito gera uma exceção de argumento inválido ou uma exceção de análise XAML.

**Comportamento de hiperlink e tema/estilo**

O hiperlink não herda [Control](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control), portanto, ele não tem uma propriedade [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.style) ou um [Template](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template). Você pode editar as propriedades que são herdadas de [TextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement), como Foreground ou FontFamily, para alterar a aparência de um hiperlink, mas você não pode usar um estilo ou modelo comum para aplicar as alterações. Em vez de usar um modelo, considere usar recursos comuns para valores de propriedades de hiperlink para fornecer consistência. Algumas propriedades de hiperlink usam padrões de um valor de extensão de marcação {ThemeResource} fornecidos pelo sistema. Isso permite que a aparência do hiperlink seja alternada de maneiras apropriadas quando o usuário altera o tema do sistema em tempo de execução.

A cor padrão do hiperlink é a cor de destaque do sistema. Você pode definir a propriedade [Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement.foreground) para substituir isso.

## <a name="recommendations"></a>Recomendações

-   Só use hiperlinks para navegação; não os utilize para outras ações.
-   Use o estilo Body da rampa de tipos para hiperlinks baseados em texto. Leia mais sobre as [fontes e a rampa de tipos do Windows 10](../style/typography.md).
-   Mantenha hiperlinks diferentes distantes o suficiente para que o usuário possa diferenciar entre eles e tenha facilidade ao selecionar cada uma deles.
-   Adicione dicas de ferramentas aos hiperlinks que indicam para onde o usuário será direcionado. Se o usuário for direcionado para um site externo, inclua o nome do domínio de nível superior na dica de ferramenta e defina o estilo do texto com uma cor de fonte secundária.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Controles de texto](text-controls.md)
- [Diretrizes de dicas de ferramenta](tooltips.md)

**Para desenvolvedores (XAML)**
- [Classe Windows.UI.Xaml.Documents.Hyperlink](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Hyperlink)
- [Classe Windows.UI.Xaml.Controls.HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)
