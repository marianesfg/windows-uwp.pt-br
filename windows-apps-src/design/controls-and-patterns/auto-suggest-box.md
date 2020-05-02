---
Description: Uma caixa de entrada de texto que fornece sugestões à medida que o usuário digita.
title: Diretrizes para caixas de sugestão automática
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e8a4c347bd2baa51115ecd9315f923e205133a6e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081267"
---
# <a name="auto-suggest-box"></a>Caixa de sugestão automática

Use uma AutoSuggestBox para fornecer uma lista de sugestões para um usuário selecionar conforme digita.

![Uma caixa de sugestão automática](images/controls/auto-suggest-box-open.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | A Biblioteca de interface do usuário do Windows 2.2 ou posterior inclui um novo modelo para esse controle que usa cantos arredondados. Para obter mais informações, confira [Raio de canto](/windows/uwp/design/style/rounded-corner). WinUI é um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da plataforma**: [Classe AutoSuggestBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox), [evento TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged), [evento SuggestionChose](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen), [evento QuerySubmitted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Se você quer um controle simples e personalizável que permita a pesquisa de texto com uma lista de sugestões, então escolha uma caixa de sugestão automática.

Para obter mais informações sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/AutoSuggestBox">abrir o aplicativo e ver o AutoSuggestBox em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Um caixa de sugestão automática no aplicativo Groove Música.

![Um caixa de sugestão automática no aplicativo Groove Música](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>Anatomia
O ponto de entrada para a caixa de sugestão automática consiste de um cabeçalho opcional e uma caixa de texto com texto de dica opcional:

![Exemplo do ponto de entrada para o controle de sugestão automática](images/controls_autosuggest_entrypoint.png)

A lista de resultados da sugestão automática é preenchida automaticamente depois que o usuário começa a inserir texto. A lista de resultados pode aparecer acima ou abaixo da caixa de entrada de texto. Um botão "limpar tudo" aparece:

![Exemplo do controle de sugestão automática expandido](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>Criar uma caixa de sugestão automática

Para usar uma AutoSuggestBox, você precisa responder a três ações de usuário.

- Texto alterado - quando o usuário insere texto, atualiza a lista de sugestões.
- Sugestão escolhida – quando o usuário selecionar uma sugestão da lista, atualize a caixa de texto.
- Consulta enviada - quando o usuário envia uma consulta, mostra os resultados da consulta.

### <a name="text-changed"></a>Texto alterado

O evento [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged) ocorre sempre que o conteúdo da caixa de texto é atualizado. Use a propriedade [Reason](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason) de argumentos de evento para determinar se a alteração foi causada por entrada de usuário. Se a razão da alteração for **UserInput**, filtre seus dados de acordo com a entrada. Depois, configure os dados filtrados como o [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) da AutoSuggestBox, para atualizar a lista de sugestões.

Para controlar como os itens são exibidos na lista de sugestões, você pode usar [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) ou [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate).

- Para exibir o texto de uma única propriedade de seu item de dados, configure a propriedade DisplayMemberPath para escolher qual propriedade de seu objeto exibir na lista de sugestões.
- Para definir uma aparência personalizada para cada item na lista, use a propriedade ItemTemplate.

### <a name="suggestion-chosen"></a>Sugestão escolhida

Quando um usuário navega pela lista de sugestões usando o teclado, você precisa atualizar o texto na caixa de texto para corresponder.

Você pode configurar a propriedade [TextMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textmemberpath) para escolher qual propriedade de seu objeto de dados exibir na caixa de texto. Se você especificar um TextMemberPath, a caixa de texto será atualizada automaticamente. Geralmente, você deve especificar o mesmo valor para DisplayMemberPath e TextMemberPath, para que o texto seja o mesmo na lista de sugestões e na caixa de texto.

Se você precisa mostrar mais do que uma propriedade simples, manipule o evento [SuggestionChosen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen) para preencher a caixa de texto com texto personalizado com base no item selecionado.

### <a name="query-submitted"></a>Consulta enviada

Manipule o evento [QuerySubmitted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted) para executar uma ação de consulta adequada para seu aplicativo e mostre o resultado para o usuário.

O evento QuerySubmitted ocorre quando um usuário confirma uma cadeia de caracteres de consulta. O usuário pode confirmar uma consulta de uma das seguintes maneiras:
- Com o foco na caixa de texto, pressione Enter ou clique no ícone de consulta. A propriedade [ChosenSuggestion](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion) de argumentos de evento é **null**.
- Com o foco na lista de sugestões, pressione Enter, clique ou toque em um item. A propriedade ChosenSuggestion de argumentos de evento contém o item que foi selecionado da lista.

Em todos os casos, a propriedade [QueryText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext) de argumentos de evento contém o texto da caixa de texto.

Aqui está uma AutoSuggestBox simples com os manipuladores de eventos exigidos.

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>Use a AutoSuggestBox para pesquisa

Use uma AutoSuggestBox para fornecer uma lista de sugestões para um usuário selecionar conforme digita.

Por padrão, a caixa de entrada de texto não tem um botão de consulta aparente. Você pode configurar a propriedade [QueryIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.queryicon) para adicionar um botão com o ícone especificado no lado direito da caixa de texto. Por exemplo, para fazer com que a AutoSuggestBox se pareça com uma caixa de pesquisa típica, adicione um ícone de 'pesquisar' como este.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

Aqui está uma AutoSuggestBox com um ícone de 'pesquisar'.

![Exemplo do ponto de entrada para o controle de sugestão automática](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   Quando a caixa de sugestão automática for usada para realizar pesquisas e não houver nenhum resultado para o texto inserido, exiba a mensagem de uma linha "Nenhum resultado" para que os usuários saibam que a solicitação de pesquisa foi executada:

    ![Exemplo de uma caixa de sugestão automática com nenhum resultado de pesquisa](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.
- [Exemplo de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Artigos relacionados

- [Controles de texto](text-controls.md)
- [Verificação ortográfica](text-controls.md)
- [Pesquisa](search.md)
- [Classe TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Classe Windows.UI.Xaml.Controls PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propriedade String.Length](https://docs.microsoft.com/dotnet/api/system.string.length)
