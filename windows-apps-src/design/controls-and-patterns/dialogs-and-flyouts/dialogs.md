---
Description: Caixas de diálogo e submenus exibem elementos transitórios da interface do usuário que aparecem quando o usuário os solicita ou quando acontece algo que requer notificação ou aprovação.
title: Controles de caixa de diálogo
label: Dialogs
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1277d9089e900451ac4c537805079ff479f808fa
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66748452"
---
# <a name="dialog-controls"></a>Controles de caixa de diálogo

Controles de caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. Eles bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignorados. Elas muitas vezes solicitam algum tipo de ação do usuário.

![Exemplo de uma caixa de diálogo](../images/dialogs/dialog_RS2_delete_file.png)


> **APIs importantes**: [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use caixas de diálogo para notificar os usuários sobre informações importantes, ou para solicitar a confirmação ou informações adicionais antes de uma ação ser concluída.

Para ver recomendações sobre quando usar uma caixa de diálogo e quando usar um submenu (um controle semelhante), confira [Caixas de diálogo e submenus](index.md). 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
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
    -   Certifique-se de que a caixa de diálogo tenha pelo menos um botão correspondente para uma ação segura e não destrutiva como "Pronto!", "Fechar" ou "Cancelar". Use a API CloseButton para adicionar esse botão.
    -   Use respostas específicas para a instrução ou o conteúdo principal como texto do botão. Um exemplo é "Você permite que NomeDoAplicativo acesse o sua localização?", seguido pelos botões "Permitir" e "Bloquear". Respostas específicas podem ser entendidas mais rapidamente, resultando na tomada de decisão eficiente.
    - Verifique se o texto dos botões de ação é conciso. Cadeias de caracteres curtas permitem que o usuário faça uma escolha de modo rápido e seguro.
    - Além da ação segura e não destrutiva, você pode, opcionalmente, apresentar ao usuário um ou dois botões de ação relacionados à instrução principal. Esses botões de ação "faça isso" confirmam o ponto principal da caixa de diálogo. Use as APIs PrimaryButton e SecondaryButton para adicionar essas ações "faça isso".
    - Os botões de ação "faça isso" devem aparecer como os botões da extrema esquerda. A ação segura e não destrutiva deve aparecer como o botão da extrema direita.
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
Como caixas de diálogo bloqueiam a interação do usuário e como os botões são o mecanismo principal para os usuários ignorarem a caixa de diálogo, certifique-se de que a caixa de diálogo contenha pelo menos um botão de ação "segura" e não destrutiva, como "Fechar" ou "Pronto!". **Todas as caixas de diálogo devem conter pelo menos um botão de ação segura para fechar a caixa de diálogo.** Isso garante que o usuário possa fechar a caixa de diálogo com segurança sem executar uma ação.<br>![Uma caixa de diálogo de um botão](../images/dialogs/dialog_RS2_one_button.png)

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

Quando as caixas de diálogo são usadas para exibir uma pergunta de bloqueio, sua caixa de diálogo deve apresentar ao usuário botões de ação relacionados à pergunta. O botão de ação "segura" e não destrutiva pode ser acompanhado por um ou dois botões de ação "faça isso". Ao apresentar para o usuário várias opções, certifique-se de que os botões expliquem claramente as ações "faça isso" seguras/"não faça isso" relacionadas à pergunta proposta.

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

As caixas de diálogo de três botões são usadas quando você apresenta ao usuário duas ações do tipo "faça isso" e "não faça isso". As caixas de diálogo de três botões devem ser usadas com moderação com diferenças claras entre a ação secundária e a ação segura/fechar.

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
ContentDialog tem três tipos diferentes de botão que você pode usar para criar uma experiência de caixa de diálogo.

- **CloseButton** – obrigatório – representa a ação segura e não destrutiva que permite ao usuário sair da caixa de diálogo. Aparece como o botão à extrema direita.
- **PrimaryButton** – opcional – representa a primeira ação "faça isso". Aparece como o botão à extrema esquerda.
- **SecondaryButton** – opcional – representa a segunda ação "faça isso". Aparece como o botão do meio.

O uso de botões internos posicionará os botões adequadamente, garantirá que eles respondam corretamente aos eventos de teclado, garantirá que a área de comando permaneça visível quando o teclado virtual estiver ativo e tornará a caixa de diálogo consistente com outras caixas de diálogo.

### <a name="closebutton"></a>CloseButton
Cada caixa de diálogo deve conter um botão de ação segura e não destrutiva que permita ao usuário sair da caixa de diálogo com confiança.

Use a API ContentDialog.CloseButton para criar esse botão. Isso permite que você crie a experiência do usuário certa para todas as entradas, incluindo mouse, teclado, toque e gamepad. Essa experiência acontecerá quando:
<ol>
    <li>O usuário clicar ou tocar em CloseButton </li>
    <li>O usuário pressionar o botão Voltar do sistema </li>
    <li>O usuário pressionar o botão ESC no teclado </li>
    <li>O usuário pressionar o botão B do Gamepad </li>
</ol>

Quando o usuário clica em um botão de caixa de diálogo, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna um [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para informar em qual botão o usuário clica. Pressionar CloseButton retorna ContentDialogResult.None.

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton e SecondaryButton
Além de CloseButton, como opção, você pode apresentar ao usuário um ou dois botões de ação relacionados à instrução principal.
Aproveite PrimaryButton para a primeira ação "faça isso" e SecondaryButton para a segunda ação "faça isso". Em caixas de diálogo de três botões, o PrimaryButton geralmente representa a ação "faça isso" afirmativa, enquanto o SecondaryButton geralmente representa uma ação "faça isso" neutra ou secundária.
Por exemplo, um aplicativo pode solicitar que o usuário assine para receber um serviço. O PrimaryButton como ação "faça isso" afirmativa pode hospedar o texto Assinar, enquanto o SecondaryButton como a ação "faça isso" neutra pode hospedar o texto Experimentar. O CloseButton permitiria ao usuário cancelar sem executar qualquer ação.

Quando o usuário clica em PrimaryButton, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna ContentDialogResult.Primary.
Quando o usuário clica em SecondaryButton, o método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna ContentDialogResult.Secondary.

![Uma caixa de diálogo de três botões](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
Como opção, você pode optar por diferenciar um dos três botões como o botão padrão. A especificação do botão padrão resulta em:
- O botão recebe o tratamento visual de Botão de Destaque
- O botão responderá à tecla ENTER automaticamente
    - Quando o usuário pressiona a tecla ENTER no teclado, o manipulador de cliques associado ao Botão Padrão será acionado e o ContentDialogResult retornará o valor associado ao Botão Padrão
    - Se o usuário tiver colocado o Foco do Teclado em um controle que manipula ENTER, o Botão Padrão não responderá ao pressionamento de ENTER
- O botão receberá o foco automaticamente quando a caixa de diálogo for aberta, exceto quando o conteúdo da caixa de diálogo apresentar a interface do usuário focalizável

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

> Algumas plataformas colocam o botão de afirmação à direita em vez de à esquerda. Por que é recomendável colocá-lo no lado esquerdo?  Se você considerar que a maioria dos usuários é destra e eles seguram o telefone com essa mão, é realmente mais confortável pressionar o botão de afirmação quando ele está à esquerda, pois o botão tem mais chances de estar dentro do raio do polegar do usuário. Botões no lado direito da tela exigem que o usuário puxe o polegar para dentro até uma posição mais confortável.

## <a name="contentdialog-in-appwindow-or-xaml-islands"></a>ContentDialog em AppWindow ou Ilhas Xaml

> OBSERVAÇÃO: esta seção se aplica somente a aplicativos direcionados ao Windows 10, versão 1903 ou posterior. AppWindow e Ilhas XAML não estão disponíveis em versões anteriores. Para saber mais sobre controle de versão, consulte [Aplicativos adaptáveis à versão](../../../debug-test-perf/version-adaptive-apps.md).

Por padrão, as caixas de diálogo de conteúdo são exibidas modalmente em relação à [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) raiz. Quando você usa ContentDialog em [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) ou [Ilha XAML](/apps/desktop/modernize/xaml-islands), é preciso definir manualmente [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) na caixa de diálogo para a raiz do host do XAML.

Para fazer isso, defina a propriedade XamlRoot de ContentDialog para o mesmo XamlRoot como um elemento já em AppWindow ou Ilha XAML, como mostrado aqui.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        noWifiDialog.XamlRoot = elementAlreadyInMyAppWindow.XamlRoot;
    }

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

> [!WARNING]
> Só pode haver uma ContentDialog aberta por thread de cada vez. A tentativa de abrir duas ContentDialogs lançará uma exceção, mesmo que estejam tentando abrir em AppWindows separadas.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados
- [Dicas de ferramentas](../tooltips.md)
- [Menus e menu de contexto](../menus.md)
- [Classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
