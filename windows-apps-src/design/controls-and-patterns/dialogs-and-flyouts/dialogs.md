---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Controles de caixa de diálogo
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9ba4bfcd38acba2bcd7c8399b8b17184edacc15a
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6264430"
---
## <a name="dialog-controls"></a>Controles de caixa de diálogo

Controles de caixa de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. Eles bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas. Elas muitas vezes solicitam algum tipo de ação do usuário.

![Exemplo de uma caixa de diálogo](../images/dialogs/dialog_RS2_delete_file.png)


> **APIs importantes**: [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use caixas de diálogo para notificar os usuários sobre informações importantes ou para solicitar a confirmação ou informações adicionais antes de uma ação ser concluída.

Para obter recomendações sobre quando usar uma caixa de diálogo VS quando usar um submenu (um controle semelhante), consulte [as caixas de diálogo e submenus](index.md). 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>Diretrizes gerais

-   Identifique claramente o problema ou o objetivo do usuário na primeira linha do texto da caixa de diálogo.
-   O título da caixa de diálogo é a principal instrução e é opcional.
    -   Use um título curto para explicar o que as pessoas precisam fazer na caixa de diálogo.
    -   Se você está usando a caixa de diálogo para passar uma mensagem simples, de erro ou fazer uma pergunta, pode omitir o título. Utilize o texto do conteúdo para passar as informações principais.
    -   O título deve estar diretamente relacionado às opções de botão.
-   O conteúdo da caixa de diálogo contém o texto descritivo e é obrigatório.
    -   Apresente a mensagem, o erro ou a pergunta de bloqueio da maneira mais simples possível.
    -   Se um título da caixa de diálogo for usado, use a área de conteúdo para fornecer mais detalhes ou definir a terminologia. Não repita o título com palavras ligeiramente diferentes.
-   Pelo menos um botão da caixa de diálogo deve aparecer.
    -   Certifique-se de que a caixa de diálogo tem pelo menos um botão correspondente para uma ação segura e não destrutiva como "Acertou!", "Fechar" ou "Cancelar". Use a API CloseButton para adicionar esse botão.
    -   Use respostas específicas para a instrução ou o conteúdo principal como texto do botão. Um exemplo é "Você permite que AppName acesse sua localização?", seguido pelos botões "Permitir" e "Bloquear". Respostas específicas podem ser entendidas mais rapidamente, resultando na tomada de decisão eficiente.
    - Verifique se o texto dos botões de ação é conciso. Cadeias de caracteres curtas permitem que o usuário faça uma escolha de modo rápido e seguro.
    - Além da ação segura e não destrutiva, você pode, opcionalmente, apresentar ao usuário um ou dois botões de ação relacionados à instrução principal. Esses botões de confirmam o ponto principal da caixa de diálogo. Use as APIs PrimaryButton e SecondaryButton para adicionar essas ações.
    - Os botões de ação devem aparecer como os botões mais à esquerda. A ação segura e não destrutiva deve aparecer como o botão mais à direita.
    - Como opção, você pode optar por diferenciar um dos três botões como botão padrão da caixa de diálogo. Use a API DefaultButton para diferenciar um dos botões.  
-   Não use caixas de diálogo para erros que são contextuais a um local específico da página, como erros de validação (em campos de senha, por exemplo). Use a própria tela do aplicativo para mostrar erros embutidos.
- Use a [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) para criar sua experiência de caixa de diálogo. Não use a API MessageDialog preterida.

## <a name="how-to-create-a-dialog"></a>Como criar uma caixa de diálogo
Para criar uma caixa de diálogo, use a [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog). Você pode criar uma caixa de diálogo no código ou na marcação. Embora seja geralmente mais fácil definir os elementos de interface do usuário em XAML, no caso de uma caixa de diálogo simples, é realmente mais fácil usar apenas o código. Este exemplo cria uma caixa de diálogo para notificar o usuário que não há nenhuma conexão Wi-Fi e, em seguida, usa o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) para exibi-la.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Quando o usuário clica em um botão de caixa de diálogo, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna um [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para informar em qual botão o usuário clica.

A caixa de diálogo neste exemplo faz uma pergunta e usa o [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) retornado para determinar a resposta do usuário.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="provide-a-safe-action"></a>Fornecer uma ação segura
Como caixas de diálogo bloqueiam a interação do usuário e como os botões são o mecanismo principal para os usuários ignorarem a caixa de diálogo, certifique-se de que a caixa de diálogo contém pelo menos um botão de ação "segura" e não destrutiva, como "Fechar" ou "Acertou!". **Todas as caixas de diálogo devem conter pelo menos um botão de ação segura para fechar a caixa de diálogo.** Isso garante que o usuário possa fechar a caixa de diálogo sem executar uma ação.<br>![Uma caixa de diálogo de um botão](../images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Quando as caixas de diálogo são usadas para exibir uma pergunta de bloqueio, sua caixa de diálogo deve apresentar ao usuário botões de ação relacionados à pergunta. O botão de ação "segura" e não destrutiva pode ser acompanhado por um ou dois botões de ação. Ao apresentar para o usuário várias opções, certifique-se de que os botões expliquem claramente as ações de "fazer" seguras/"não fazer" relacionadas à pergunta proposta.

![Uma caixa de diálogo de dois botões](../images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

As caixas de diálogo de três botões são usadas quando você apresenta ao usuário duas ações do tipo "fazer" e "não fazer isso". As caixas de diálogo de três botões devem ser usadas com moderação com diferenças claras entre a ação secundária e a ação segura/fechar.

![Uma caixa de diálogo de três botões](../images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="the-three-dialog-buttons"></a>Os três botões da caixa de diálogo
ContentDialog tem três tipos diferentes de botões que você pode usar para criar uma experiência de caixa de diálogo.

- **CloseButton** - Obrigatório - Representa a ação segura e não destrutiva que permite ao usuário sair da caixa de diálogo. Aparece como o botão mais à direita.
- **PrimaryButton** - Opcional – Representa a primeira ação de "fazer". Aparece como o botão mais à esquerda.
- **SecondaryButton** - Opcional – Representa a segunda ação de "fazer". Aparece como o botão do meio.

Ao usar os botões internos eles são posicionados adequadamente, portanto, certifique-se de que eles respondam corretamente a eventos do teclado, verifique se a área de comando permanece visível mesmo quando o teclado virtual aparece e deixa a caixa de diálogo com aparência consistente a de outras caixas de diálogo.

### <a name="closebutton"></a>CloseButton
Cada caixa de diálogo deve conter um botão de ação segura e não destrutiva que permite ao usuário sair da caixa de diálogo com confiança.

Use a API ContentDialog.CloseButton para criar esse botão. Isso permite que você crie a experiência do usuário adequado para todas as entradas incluindo mouse, teclado, toque e gamepad. Essa experiência acontecerá quando:
<ol>
    <li>O usuário clica ou toca em CloseButton </li>
    <li>O usuário pressiona o botão de voltar do sistema. </li>
    <li>O usuário pressiona o botão ESC no teclado </li>
    <li>O usuário pressiona o Gamepad B </li>
</ol>

Quando o usuário clica em um botão de caixa de diálogo, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna um [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para informar em qual botão o usuário clica. Pressionar em CloseButton retorna ContentDialogResult.None.

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton e SecondaryButton
Além de CloseButton, como opção, você pode apresentar ao usuário um ou dois botões de ação relacionados à instrução principal.
Aproveite PrimaryButton como a primeira ação de "fazer" e SecondaryButton como a segunda ação de "fazer". Em caixas de diálogo de três botões, o PrimaryButton geralmente representa a ação de "fazer" afirmativa, enquanto o SecondaryButton geralmente representa uma ação de "fazer" neutra ou secundária.
Por exemplo, um aplicativo pode solicitar ao usuário para assinar um serviço. O PrimaryButton como ação de "fazer" afirmativa pode hospedar o texto Assinar, enquanto o SecondaryButton como a ação de "fazer" neutra pode hospedar o texto Tentar. O CloseButton permitiria ao usuário cancelar sem executar qualquer ação.

Quando o usuário clica em PrimaryButton, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna ContentDialogResult.Primary.
Quando o usuário clica em SecondaryButton, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna ContentDialogResult.Secondary.

![Uma caixa de diálogo de três botões](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
Como opção, você pode optar por diferenciar um dos três botões como botão padrão. A especificação do botão padrão faz resulta em:
- O botão recebe o tratamento visual de Botão de destaque
- O botão responderá à tecla ENTER automaticamente
    - Quando o usuário pressiona a tecla ENTER no teclado, o manipulador de cliques associado ao botão padrão será acionado e o ContentDialogResult retornará o valor associado ao Botão padrão
    - Se o usuário tiver colocado o Foco do teclado em um controle que manipula ENTER, o Botão padrão não responderá ao pressionar ENTER
- O botão receberá o foco automaticamente quando a Caixa de diálogo é aberta, exceto quando o conteúdo da caixa de diálogo contém a interface do usuário focalizável

Use a propriedade ContentDialog.DefaultButton para indicar o botão padrão. Por padrão, nenhum botão padrão está definido.

![Uma caixa de diálogo de três botões com um botão padrão](../images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="confirmation-dialogs-okcancel"></a>Caixas de diálogo de confirmação (OK/Cancelar)
Uma caixa de diálogo de confirmação proporciona aos usuários a oportunidade de confirmar que desejam executar uma ação. Ele podem afirmar a ação, ou optar por cancelar.  
Uma caixa de diálogo de confirmação típica tem dois botões: um botão de afirmação ("OK") e um botão Cancelar.  

<ul>
    <li>
        <p>Em geral, o botão de afirmação fica à esquerda (o botão primário) e o botão Cancelar (o botão secundário) fica à direita.</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>Conforme observado na seção de recomendações gerais, use botões com texto que identifiquem respostas específicas para a instrução ou o conteúdo principal.
    </li>
</ul>

> Algumas plataformas colocam o botão de afirmação à direita em vez de à esquerda. Por que é recomendável colocá-lo no lado esquerdo?  Se você considerar que a maioria dos usuários estão à direita e eles segurar o telefone com a mão, é realmente mais confortável pressionar o botão de afirmação quando ele está à esquerda, como o botão está mais chances de serem dentro thumb-arco do usuário. Botões no lado direito da tela exigem que o usuário receber seu polegar para dentro para uma posição mais confortável.





## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados
- [Dicas de ferramenta](../tooltips.md)
- [Menus e menu de contexto](../menus.md)
- [Classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
