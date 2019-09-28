---
Description: Caixa de senha é uma caixa de entrada de texto que oculta os caracteres digitados nela, para fins de privacidade.
title: Diretrizes para caixas de senha
ms.assetid: 332B04D6-4FFE-42A4-8B3D-ABE8266C7C18
dev.assetid: 4BFDECC6-9BC5-4FF5-8C63-BB36F6DDF2EF
label: Password box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: dfa010d3e50208df31dad9d838486e72f38787c2
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340241"
---
# <a name="password-box"></a>Caixa de senha

 

Caixa de senha é uma caixa de entrada de texto que oculta os caracteres digitados nela, para fins de privacidade. Uma caixa de senha se parece com uma caixa de texto, exceto que ela renderiza caracteres de espaço reservado no lugar do texto que foi inserido. Você pode configurar o caractere de espaço reservado.

> **APIs importantes**: [Classe PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox), [Propriedade Password](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.password), [Propriedade PasswordChar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchar), [Propriedade PasswordRevealMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode), [Evento PasswordChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchanged)

Por padrão, a caixa de senha oferece uma maneira para o usuário visualizar a senha pressionando um botão de revelar. Você pode desabilitar o botão de revelação ou fornecer um mecanismo alternativo para revelar a senha, como uma caixa de seleção.

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use o controle **PasswordBox** para receber uma senha ou outros dados particulares, como um número de CPF.

Para obter mais informações sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/PasswordBox">abri-lo e ver o PasswordBox em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

A caixa de senha tem vários estados, incluindo estes notáveis.

Uma caixa de senha em repouso pode mostrar um texto de dica para que o usuário saiba sua finalidade:

![Caixa de senha em estado de repouso com texto de dica](images/passwordbox-rest-hinttext.png)

Quando o usuário digita em uma caixa de senha, o comportamento padrão é exibir marcadores que escondem o texto sendo inserido:

![Foco da caixa de senha no estado de digitação texto](images/passwordbox-focus-typing.png)

Pressionar o botão de "revelar" à direita permite ver o texto da senha sendo inserida:

![Texto da caixa de senha revelado](images/passwordbox-text-reveal.png)

## <a name="create-a-password-box"></a>Criar uma caixa de senha

Use a propriedade [Password](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.password) para obter ou configurar o conteúdo da PasswordBox. Você pode fazer isso no manipulador para o evento [PasswordChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchanged), para realizar a validação enquanto o usuário insere a senha. Ou então, você pode usar outro evento, como um [clique](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) em um botão, para realizar a validação após o usuários completar a entrada de texto.

Aqui está o XAML para um controle de caixa de senha que demonstra a aparência padrão da PasswordBox. Quando o usuário insere uma senha, você verifica se é o valor literal: "Senha". Se for, você exibe uma mensagem para o usuário.

```xaml
<StackPanel>  
  <PasswordBox x:Name="passwordBox" Width="200" MaxLength="16"
             PasswordChanged="passwordBox_PasswordChanged"/>

  <TextBlock x:Name="statusText" Margin="10" HorizontalAlignment="Center" />
</StackPanel>   
```

```csharp
private void passwordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    if (passwordBox.Password == "Password")
    {
        statusText.Text = "'Password' is not allowed as a password.";
    }
    else
    {
        statusText.Text = string.Empty;
    }
}
```
Aqui está o resultado quando esse código é executado é o usuário insere "Senha".

![Caixa de senha com uma mensagem de validação](images/passwordbox-revealed-validation.png)

### <a name="password-character"></a>Caractere de senha

Você pode alterar o caractere usado para mascarar a senha, definindo a propriedade [PasswordChar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchar). Aqui, o marcador padrão é substituído por um asterisco.

```xaml
<PasswordBox x:Name="passwordBox" Width="200" PasswordChar="*"/>
```

O resultado tem a seguinte aparência.

![Caixa de senha com um caractere personalizado](images/passwordbox-custom-char.png)

### <a name="headers-and-placeholder-text"></a>Cabeçalhos e texto de espaço reservado

Você pode usar as propriedades [Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.header) e [PlaceholderText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.placeholdertext) para fornecer contexto para a PasswordBox. Isso é especialmente útil quando você tem várias caixas, como em um formulário para alterar uma senha.

```xaml
<PasswordBox x:Name="passwordBox" Width="200" Header="Password" PlaceholderText="Enter your password"/>
```

![Caixa de senha em estado de repouso com texto de dica](images/passwordbox-rest-hinttext.png)

### <a name="maximum-length"></a>Comprimento máximo

Especifique o número máximo de caracteres que o usuário pode inserir configurando a propriedade [MaxLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.maxlength). Não há nenhuma propriedade para especificar um comprimento mínimo, mas você pode verificar o comprimento da senha, e realizar outras validações, no código do aplicativo.

## <a name="password-reveal-mode"></a>Modo de revelação de senha

A PasswordBox tem um botão interno que o usuário pode pressionar para exibir o texto da senha. Aqui está o resultado da ação do usuário. Quando usuário o libera, a senha volta a ser oculta automaticamente.

![Texto da caixa de senha revelado](images/passwordbox-text-reveal.png)

### <a name="peek-mode"></a>Modo espiada

Por padrão, o botão de revelação de senha (ou botão de "espiada") é exibido. O usuário deve pressionar continuamente o botão para exibir a senha, para que um alto nível de segurança seja mantido.

O valor da propriedade [PasswordRevealMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) não é único fator que determina se um botão de revelar a senha é visível para o usuário. Outros fatores incluem se o controle é exibido acima de uma largura mínima, se a PasswordBox tem foco e se o campo de entrada de texto contém pelo menos um caractere. O botão de revelar a senha é exibido apenas quando a PasswordBox recebe foco pela primeira vez e um caractere é inserido. Se a PasswordBox perde foco e o retoma em seguida, o botão de revelar não é exibido novamente, a menos que a senha seja apagada e a entrada de caracteres recomece.

> **Cuidado**&nbsp;&nbsp;Antes do Windows 10, o botão de revelar a senha não era exibido por padrão. Se a segurança de seu aplicativo exigir que a senha esteja sempre oculta, certifique-se de definir PasswordRevealMode como Hidden.

### <a name="hidden-and-visible-modes"></a>Modos ocultos e visíveis

Os outros valores de enumeração [PasswordRevealMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordRevealMode), **Hidden** e **Visible**, ocultam o botão de revelar senha e permitem que você gerencie programaticamente se a senha é ocultada.

Para sempre ocultar a senha, defina PasswordRevealMode como Hidden. A menos que precise que a senha seja sempre oculta, você pode fornecer uma interface do usuário personalizada para permitir que o usuário alterne o PasswordRevealMode entre Oculto e Visível.

Em versões anteriores do Windows Phone, a PasswordBox usava uma caixa de seleção para alternar entre senha oculta ou não. Você pode criar uma interface do usuário semelhante para seu aplicativo, conforme mostrado no exemplo a seguir. Você também pode usar outros controles, como [ToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton), para deixar os usuários alternarem os modos.

Este exemplo mostra como uma [CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) para permitir que um usuário alterne o modo de revelação de uma PasswordBox.

```xaml
<StackPanel Width="200">
    <PasswordBox Name="passwordBox1"
                 PasswordRevealMode="Hidden"/>
    <CheckBox Name="revealModeCheckBox" Content="Show password"
              IsChecked="False"
              Checked="CheckBox_Changed" Unchecked="CheckBox_Changed"/>
</StackPanel>
```

```csharp
private void CheckBox_Changed(object sender, RoutedEventArgs e)
{
    if (revealModeCheckBox.IsChecked == true)
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Visible;
    }
    else
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Hidden;
    }
}
```

Esta PasswordBox tem esta aparência.

![Caixa de senha com um botão de revelação personalizado](images/passwordbox-custom-reveal.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Escolha o teclado correto para o controle de texto

Para ajudar os usuários a inserir dados usando o teclado virtual ou SIP (Soft Input Panel), você pode configurar o escopo de entrada do controle de texto para corresponder ao tipo de dado que se espera que o usuário insira. A PasswordBox oferece suporte apenas ao valores de escopo de entrada **Password** e **NumericPin**. Qualquer outro valor é ignorado.

Para obter mais informações sobre como usar escopos de entrada, consulte [Usar o escopo de entrada para alterar o teclado virtual](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard).

## <a name="recommendations"></a>Recomendações

-   Se o propósito da caixa de senha não for claro, use um rótulo ou um texto de espaço reservado. Um rótulo fica visível, independentemente da caixa de entrada de texto ter ou não um valor. O texto de espaço reservado é exibido dentro da caixa de entrada de texto e desaparece uma vez que um valor tiver sido inserido.
-   Dê à caixa de senha uma largura adequada para o intervalo de valores que pode ser inserido. O comprimento da palavra varia entre os idiomas, então leve em conta a localização se quiser que seu aplicativo esteja preparado para uso internacional.
-   Não coloque outro controle bem próximo de uma caixa de entrada de senha. A caixa de senha tem um botão de revelar senha para que os usuários verifiquem as senhas que já digitaram, sendo que ter outro controle muito próximo pode fazer com que os usuários revelem acidentalmente suas senhas quando tentarem interagir com o outro controle. Para evitar que isso aconteça, coloque um espaço entre a caixa de entrada de senha e o outro controle, ou o coloque na próxima linha.
-   Considere a possibilidade de apresentar duas caixas de senha para a criação de conta: uma para a nova senha e outra para confirmação dela.
-   Mostre uma caixa de senha apenas para logons.
-   Quando uma caixa de senha é usada para inserir um PIN, considere fornecer uma resposta instantânea assim que o último número for inserido, em vez de usar um botão de confirmação.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

[Controles de texto](text-controls.md)

- [Diretrizes para verificação ortográfica](text-controls.md)
- [Como adicionar pesquisa](https://docs.microsoft.com/previous-versions/windows/apps/hh465231(v=win.10))
- [Diretrizes para entrada de texto](text-controls.md)
- [Classe TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Classe Windows.UI.Xaml.Controls PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propriedade String.Length](https://docs.microsoft.com/dotnet/api/system.string.length)
