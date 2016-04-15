---
Description: Você pode incentivar os clientes a deixar comentários iniciando o Hub de Feedback do seu aplicativo.
title: Iniciar o Hub de Feedback do seu aplicativo
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
---

# Iniciar o Hub de Feedback do seu aplicativo

Você pode incentivar os clientes a deixar comentários adicionando um controle (como um botão) ao seu aplicativo da Plataforma Universal do Windows (UWP) que inicia o Hub de Feedbacks. Hub de Feedback é um aplicativo pré-instalado que oferece um local único para coletar feedback sobre o Windows e os aplicativos instalados. Todo o feedback do cliente que é enviado para seu aplicativo por meio do Hub de Feedback é coletado e apresentado no [Relatório de feedback](../publish/feedback-report.md) no painel do Centro de Desenvolvimento do Windows. Assim, você pode ver os problemas, as sugestões e atualizações que seus clientes enviaram em um relatório.

Para iniciar o Hub de Feedback do seu aplicativo, use uma API que seja fornecida pelo [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Recomendamos que você use essa API para iniciar o Hub de Feedback de um elemento de interface do usuário em seu aplicativo que siga nossas diretrizes de design.

>**Observação** O Hub de Feedback está disponível apenas em dispositivos que executam o Windows 10 versão 10.0.14271 ou posterior. Recomendamos que você mostre um controle de feedback no seu aplicativo apenas se o Hub de Feedback estiver disponível no dispositivo do usuário. O código neste tópico demonstra como fazer isso.

## Como iniciar o Hub de Feedback do seu aplicativo

Para iniciar o Hub de Feedback do seu aplicativo:

1. Instale o [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Além de API para iniciar o Hub de Feedback, esse SDK também fornece APIs para outros recursos como execução de experiências em seus aplicativos com testes A/B e exibição de anúncios. Para obter mais informações sobre esse SDK, consulte [Monetizar seu aplicativo e envolver os clientes com o SDK de Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.
4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
5. Na lista de SDKs, clique na caixa de seleção ao lado de **SDK de Microsoft Store Engagement** e clique em **OK**.
6. No seu projeto, adicione o controle que você deseja mostrar aos usuários para iniciar o Hub de Feedback, como um botão. Recomendamos que você configure o controle da seguinte maneira:
  * Defina a fonte do conteúdo exibido no controle como **Segoe MDL2 Assets**.
  * Defina o texto no controle como o código de caractere Unicode hexadecimal E939. Este é o código de caractere do ícone de feedback recomendado na fonte **Segoe MDL2 Assets**.
  * Defina a visibilidade do controle como oculto.

    > **Observação** O Hub de Feedback está disponível apenas em dispositivos que executam o Windows 10 versão 10.0.14271 ou posterior. Recomendamos que você oculte o controle de feedback por padrão e mostre-o no seu código de inicialização somente se o Hub de Feedback estiver disponível no dispositivo do usuário. A próxima etapa demonstra como fazer isso.

  O código a seguir demonstra a definição XAML de um [Botão](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) que é configurado conforme descrito acima.
  ```
  <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
  ```
7. Em seu código de inicialização para a página do aplicativo que hospeda o controle de feedback, use a propriedade [IsSupported](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.issupported.aspx) da classe [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) para determinar se o Hub de Feedback está disponível no dispositivo do usuário. Se essa propriedade retornar **true**, deixe o controle visível. O código a seguir demonstra como fazer isso para um [Botão](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx).
```
if (Microsoft.Services.Store.Engagement.Feedback.IsSupported)
{
        this.feedbackButton.Visibility = Visibility.Visible;
}
```
8. No manipulador de eventos que é executado quando o usuário clica no controle, chame o método estático [LaunchFeedbackAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.launchfeedbackasync.aspx) da classe [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) para iniciar o aplicativo do Hub de Feedback. Há duas sobrecargas para esse método: uma sem parâmetros e outra que aceita um dicionário de pares de chave e valor que contém os metadados que você deseja associar ao feedback. O exemplo a seguir demonstra como iniciar o Hub de Feedback no manipulador de eventos [Clique](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) para um [Botão](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx).
```
private async void feedbackButton_Click(object sender, RoutedEventArgs e)
{
        await Microsoft.Services.Store.Engagement.Feedback.LaunchFeedbackAsync();
}
```

## Recomendações de design para a interface do usuário de feedback

Para iniciar o Hub de Feedback, recomendamos que você adicione um elemento de interface do usuário em seu aplicativo (por exemplo, um botão) que exiba o seguinte ícone de feedback padrão da fonte Segoe MDL2 Assets e o código de caractere E939.

![]Feedback icon](images/feedback_icon.png)

Também recomendamos que você use uma ou mais das seguintes opções de posicionamento para vinculação ao Hub de Feedback em seu aplicativo.
* **Diretamente na barra de aplicativos**. Dependendo de sua implementação, convém usar apenas o ícone ou adicionar texto (como mostrado abaixo).

  ![]Feedback icon](images/feedback_appbar_placement.png)

* **Nas configurações do seu aplicativo**. Essa é uma maneira mais sutil de fornecer acesso ao Hub de Feedback. No exemplo a seguir, o link Feedback aparece como um dos links em Aplicativo.

  ![]Feedback icon](images/feedback_settings_placement.png)

* **Em um submenu acionado por eventos**. Isso é útil quando você deseja consultar os clientes sobre uma pergunta específica antes de iniciar o Hub do Windows Feedback. Por exemplo, depois que seu aplicativo usar um determinado recurso, você pode fazer ao cliente uma pergunta específica sobre sua satisfação com esse recurso. Se o cliente optar por responder, seu aplicativo iniciará o Hub de Feedback.


## Tópicos relacionados

* [Relatório de feedback](../publish/feedback-report.md)


<!--HONumber=Mar16_HO5-->


