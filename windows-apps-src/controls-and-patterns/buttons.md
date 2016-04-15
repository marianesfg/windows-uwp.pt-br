---
label: Buttons
template: detail.hbs
---
# Botões
Um botão dá ao usuário uma forma de acionar uma ação imediata.

![Exemplo de botões](images/controls/button.png)


<span class="sidebar_heading" style="font-weight: bold;">APIs Importantes</span>

-   [**Classe Button**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
-   [**Classe RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)
-   [**Evento Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

## Este é o controle correto?

Um botão permite que o usuário inicie uma ação imediata, como enviar um formulário.

Não use um botão quando a ação é de navegar para outra página, use um link. Para saber mais, consulte [Hiperlinks](hyperlinks.md).
    
> Exceção: na navegação do assistente, use botões rotulados como "Voltar" e "Próximo". Para outros tipos de navegação regressiva ou para um nível superior, use o botão Voltar.

## Exemplo

Este exemplo usa dois botões, Fechar tudo e Cancelar, em uma caixa de diálogo no navegador Microsoft Edge. 

![Exemplo de botões, usados em uma caixa de diálogo](images/control-examples/buttons-edge.png)

## Criar um botão

Este exemplo mostra um botão que responde a um clique. 

Crie o botão em XAML.

```xaml
<Button Content="Submit" Click="SubmitButton_Click"/>
```

Ou crie o botão em código.

```csharp
Button submitButton = new Button();
submitButton.Content = "Submit";
submitButton.Click += SubmitButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(submitButton);
```

Manipule o evento Click.

```csharp
private async void SubmitButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to submit form. For example:
    // form.Submit();
    Windows.UI.Popups.MessageDialog messageDialog = 
        new Windows.UI.Popups.MessageDialog("Thank you for your submission.");
    await messageDialog.ShowAsync();
}
```

### Interação de botão

Quando você toca em um botão com um dedo ou uma caneta, ou pressiona o botão esquerdo do mouse enquanto o ponteiro está sobre ele, o botão gera o evento [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Se um botão tem foco do teclado, pressionar a tecla Enter ou a barra de espaço também aciona o evento Click.

Geralmente, não se pode manipular eventos de baixo nível [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) em um botão porque, em vez disso, ele tem o comportamento Click. Para saber mais, consulte [Events and routed events overview](https://msdn.microsoft.com/en-us/library/windows/apps/mt185584.aspx).

Você pode alterar a forma como um botão aciona o evento Click, alterando a propriedade [**ClickMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.clickmode.aspx). O valor ClickMode padrão é **Release**. Se ClickMode for **Hover**, o evento Click não poderá ser chamado com o teclado ou o toque. 


### Conteúdo do botão

Botão é um [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Sua propriedade de conteúdo XAML é [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita uma sintaxe assim para XAML: `<Button>A button's content</Button>`. Você pode definir qualquer objeto como conteúdo do botão. Se o conteúdo for um [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), ele é renderizado no botão. Se o conteúdo for outro tipo de objeto, a representação da cadeia de caracteres é mostrada no botão.

Aqui, um **StackPanel** que contém a imagem de uma banana e o texto está definido como conteúdo de um botão.

```xaml
<Button Click="Button_Click" 
        Background="#FF0D6AA3" 
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Slices.png" Height="62"/>
        <TextBlock Text="Orange"  Foreground="White"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

O botão fica assim.

![Um botão com conteúdo de imagem e texto](images/button-orange.png)

## Criar um botão de repetição

Um [**RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) é um botão que gera eventos [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) repetidas vezes a partir do momento em que é pressionado até ser liberado. Defina a propriedade [**Delay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) para especificar o tempo que o RepeatButton aguarda após ser pressionado antes de começar a repetir a ação de clique. Defina a propriedade [**Interval**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) para especificar o tempo entre as repetições da ação de clique. O tempo para as duas propriedades são especificados em milissegundos.

O exemplo a seguir mostra dois controles RepeatButton cujos respectivos eventos Click são usados para aumentar e diminuir o valor mostrado em um bloco de texto.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## Recomendações

-   Assegure que a finalidade e o estado de um botão estejam claros para o usuário.
-   Utilize um texto conciso, específico e autoexplicativo que descreva claramente a ação executada pelo botão. Geralmente, o conteúdo de texto do botão é uma única palavra, um verbo.
-   Se o conteúdo em texto de um botão for dinâmico, por exemplo, estiver localizado, considere como o botão redimensionará e o que acontecerá para controles em volta dele.
-   Para botões de comando com conteúdo de texto, use a largura mínima do botão.
-   Não use botões estreitos, baixos ou altos com conteúdos em texto.
-   Use a fonte padrão a menos que suas diretrizes de marca digam para usar algo diferente.
-   Para uma ação que precise ser avaliada em múltiplas páginas em seu aplicativo, em vez de duplicar um botão em múltiplas páginas, considere usar uma [barra inferior do aplicativo](app-bars.md).
-   Exponha apenas um ou dois botões de cada vez para o usuário, por exemplo, Aceitar e Cancelar. Se precisar expor mais ações para o usuário, considere usar [checkboxes](checkbox.md) or [radio buttons](radio-button.md), de onde o usuário pode selecionar ações com apenas um botão de comando para acionar tais ações.
-   Use o botão de comando padrão para indicar a ação mais comum ou recomendada.
-   Considere personalizar seus botões. O formato de um botão é retangular por padrão, mas você pode customizar os recursos visuais que criam a aparência do botão. O conteúdo de um botão costuma ser texto—por exemplo, Aceitar ou Cancelar—mas você pode substituir o texto por um ícone ou usar ícone e texto.
-   Certifique-se de que conforme o usuário interaja com o botão, o botão mude de estado e aparência para fornecer um comentário ao usuário. Normal, pressionado e desativado são exemplos de estados do botão.
-   Acione os botões quando o usuário tocar ou pressionar o botão. Geralmente, a ação é disparada quando o usuário libera o botão, mas você também pode definir a ação de um botão para disparar assim que um dedo o pressiona.
-   Não use um botão de comando para definir um estado.
-   Não altere o texto do botão enquanto o aplicativo estiver em execução. Por exemplo, não altere o texto de um botão "Avançar" para "Continuar".
-   Não altere os estilos padrão de enviar, redefinir e botão.
-   Não coloque conteúdo em excesso dentro de um botão. Deixe o conteúdo conciso e fácil de entender (nada mais que uma imagem e um pouco de texto).

## Botões Voltar
O botão Voltar é uma funcionalidade de interface do usuário fornecida pelo sistema que permite a navegação regressiva através da pilha Voltar ou do histórico de navegação de um usuário.

O escopo do histórico de navegação (no aplicativo ou global) depende do dispositivo e do modo de dispositivo.

## <span id="examples"></span><span id="EXAMPLES"></span>Exemplo


A interface do usuário para o botão Voltar do sistema é otimizada para cada tipo de dispositivo e entrada, mas a experiência de navegação é global e consistente entre dispositivos e aplicativos da Plataforma Universal do Windows (UWP). Essas diferentes experiências incluem:

Dispositivos
Telefone
![sistema Voltar em um telefone](images/nav-back-phone.png)
-   Sempre presente.
-   Um botão de hardware ou software na parte inferior do dispositivo.
-   Navegação regressiva global no aplicativo e entre aplicativos.

<span id="Tablet"></span><span id="tablet"></span><span id="TABLET"></span>Tablet
![sistema Voltar em um tablet (no modo tablet)](images/nav-back-tablet.png)
-   Sempre presente no modo Tablet.

    Não está disponível no modo de área de trabalho. Em vez disso, o botão Voltar da barra de título pode ser habilitado. Consulte [Computador, laptop, tablet](#PC).

    Usuários podem alterar a execução entre modo Tablet e modo Área de trabalho indo para **Configurações &gt; Sistema &gt; Modo tablet** e definindo **Deixar o Windows mais fácil para o toque ao usar o seu dispositivo como tablet**.

-   Um botão de software na barra de navegação na parte inferior do dispositivo.
-   Navegação regressiva global no aplicativo e entre aplicativos.

<span id="PC"></span><span id="pc"></span>Computador, notebook, tablet
![sistema Voltar em um computador ou notebook](images/nav-back-pc.png)
-   Opcional no modo Área de trabalho.

    Não está disponível no modo Tablet. Consulte [Tablet](#Tablet).

    Desabilitado por padrão. Deve aceitar para habilitá-lo.

    Usuários podem alterar a execução entre modo Tablet e modo Área de trabalho indo para **Configurações &gt; Sistema &gt; Modo tablet** e definindo **Deixar o Windows mais fácil para o toque ao usar o seu dispositivo como tablet**.

-   Um botão de software na barra de título do aplicativo.
-   Navegação regressiva somente no aplicativo. Não oferece suporte à navegação entre aplicativos.

Surface Hub
![sistema Voltar em um Surface Hub](images/nav-back-surfacehub.png)
-   Sempre presente.
-   Um botão de software na parte inferior do dispositivo.
-   Navegação regressiva dentro do aplicativo e entre aplicativos.

 

## O que fazer e o que não fazer


-   Habilite a navegação regressiva.

    Se a navegação regressiva não estiver habilitada, seu aplicativo será incluído na pilha Voltar global, mas o histórico de navegação no aplicativo não será mantido.

-   Habilite o botão Voltar da barra de título no modo de área de trabalho.

    Histórico de navegação de página no aplicativo é mantido; não há suporte para navegação regressiva entre aplicativos.

    **Observação**  No modo Tablet, a barra do título é exibida quando um usuário desliza para a baixo, a partir da parte superior do dispositivo, ou move o ponteiro do mouse próximo à parte superior do dispositivo. Para evitar duplicação e confusão, o botão Voltar na barra de título não é exibido no modo Tablet.

     

-   Oculte ou desabilite o botão Voltar no modo de área de trabalho na barra de título quando o histórico de navegação no aplicativo está esgotado ou indisponível.

    Fornece uma indicação clara para o usuário de que ele voltou até onde foi possível.

-   Cada comando Voltar deve voltar uma página na pilha ou, caso não esteja no modo de área de trabalho, para o aplicativo imediatamente anterior.

    Os usuários podem ficar confusos se a navegação regressiva não for intuitiva, consistente e previsível.

## Artigos relacionados

- [Botões de rádio](radio-button.md)
- [Switches de alternância](toggles.md)
- [Caixas de seleção](checkbox.md)

**Para desenvolvedores (XAML)**
- [**Classe Button**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)




<!--HONumber=Mar16_HO4-->


