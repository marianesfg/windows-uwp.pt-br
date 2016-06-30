---
author: mcleanbyron
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: "Depois de instalar o SDK do Microsoft Store Engagement and Monetization, siga as instruções neste tópico para usar controle do Ad Mediator no seu aplicativo."
title: Adicione e use o controle do Ad Mediator
translationtype: Human Translation
ms.sourcegitcommit: 8c3f1997427a7c3d4f4b4b7acc876a2a091e4553
ms.openlocfilehash: a0d73b50207d251c079714265845a816f4ac23da

---

# Adicione e use o controle do Ad Mediator


\[Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Depois de [instalar o SDK do Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk), siga as instruções neste tópico para usar controle do Ad Mediator no seu aplicativo. Para obter uma lista de redes de publicidade e tipos de projeto atualmente com suporte pelo controle de anúncios, consulte [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md).

## Adicionar o controle de anúncios ao seu projeto


Para adicionar uma instância do controle de anúncios ao seu projeto:

1.  No Visual Studio, abra seu projeto.
2.  Se você estiver controle de anúncios a um aplicativo que já esteja lucrando com as [redes de publicidades](select-and-manage-your-ad-networks.md) suportadas pelo controle de anúncios, remova a implementação do anúncio existente e todas as duas referências antes de continuar.
3.  No **Gerenciador de Soluções**, localize a página em seu aplicativo em que você deseja mostrar anúncios e clique duas vezes na página para abri-la no designer.
4.  Na **Caixa de Ferramentas**, arraste um novo **AdMediatorControl** para o designer (certifique-se de arrastar o controle para o designer, e não para o seu código XAML). Posicione o controle no local onde você deseja que seus anúncios sejam exibidos. Se você quiser exibir anúncios em mais de uma área de seu aplicativo, poderá adicionar vários controles.

    O **AdMediatorControl** está localizado nos seguintes locais da **Toolbox**:

    -   Em um projeto da Plataforma Universal do Windows (UWP), use o **AdMediatorControl** na seção **AdMediator Universal**.
    -   Em um projeto do Windows 8.1 ou Windows Phone 8.1 que usa C# ou Visual Basic com XAML, use o **AdMediatorControl** na seção **AdMediator**.
    -   Em um projeto do Windows Phone Silverlight, use o **AdMediatorControl** na seção **Todos os controles do Windows Phone**.

    > **Observação**  Quando você arrasta o controle **AdMediatorControl** para o designer pela primeira vez em um UWP, o projeto do Windows 8.1 ou Windows Phone 8.1 em C# ou Visual Basic XAML, o Visual Studio adiciona a referência de assembly do Ad Mediator exigida para o seu projeto, mas o controle ainda não está adicionado ao designer. Para adicionar o controle, clique em OK na mensagem exibida pelo Visual Studio, aguarde alguns segundos pela atualização do designer e arraste o controle de volta para o designer. Se você ainda não conseguir adicionar o controle ao designer, verifique se o projeto se destina à arquitetura do processador pertinente a seu aplicativo (por exemplo, **x86**) em vez de **Qualquer CPU**. O controle não pode ser adicionado ao designer se o projeto se destinar a **Qualquer CPU** da plataforma de compilação.

5.  O Visual Studio adiciona uma referência de assembly do controle de anúncios ao seu projeto e insere o XAML do controle de anúncios na página atual, incluindo uma ID exclusiva e um nome para o controle. A referência de assembly e o XAML variam de acordo com a plataforma de destino. Por exemplo, para um aplicativo da Plataforma Universal do Windows (UWP), o nome do assembly é **Microsoft.AdMediator.Universal** e o XAML gerado é semelhante ao exemplo seguinte.

    ```xml
    // Code that gets added to the XAML page header
    xmlns:Universal="using:Microsoft.AdMediator.Universal"

    // Code that gets added for the ad mediator control
    <Universal:AdMediatorControl x:Name="AdMediator_3D4884"
      Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250"
      Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0"
      VerticalAlignment="Top" Width="300"/>
    ```

    O elemento **Name** ajuda a identificar o controle específico no seu aplicativo quando você configura o seu controle de anúncios. Você pode alterar para o que quiser, mas cuidado para não alterar ou duplicar o elemento **Id**. Esse **Id** deve ser único para cada controle dentro do seu aplicativo.

6.  Ajuste o tamanho e a posição do controle conforme necessário. Para obter mais informações, consulte [Ajustar o tamanho e a posição](#adjust-size-and-position).

## Configurar redes de publicidade

Depois de adicionar todos os controles que quiser, você estará pronto para configurar as redes de publicidade pelos Serviços Conectados.

> **Importante**  Se você inserir um AdMediatorControl adicional depois, precisará configurá-lo pelos Serviços Conectados novamente. Caso contrário, o novo controle não será capaz de usar o controle de anúncios.

Para configurar as redes de publicidade:

1.  Clique com o botão direito do mouse no nome do projeto no Gerenciador de Soluções, clique em **Adicionar** e, em seguida, clique em **Serviço Conectado...** para abrir a janela **Adicionar Serviço Conectado** (Visual Studio 2015) ou **Gerenciador de Serviços** (Visual Studio 2013).
2.  Se você estive usando o Visual Studio 2015, clique em **Controle de Anúncios** e em **Configurar** para abrir a janela **Controle de Anúncios**. Se você estiver usando o Visual Studio 2013, basta clicar em **Controle de Anúncios** no painel esquerdo do **Gerenciador de Serviços**.

    O arquivo AdMediator.config é adicionado ao seu projeto. É neste arquivo que as configurações iniciais da rede de publicidade são salvas localmente no projeto.

3.  Na janela do **Ad Mediator** (Visual Studio 2015) ou do **Gerenciador de Serviços** (Visual Studio 2013), clique em **Selecionar redes de publicidade**, selecione a rede de publicidade que você quer usar e clique em **OK** na janela **Selecionar redes de publicidade**.

    > **Dica**  É bom adicionar todas as redes em que você tem conta, mesmo se não pretende usar todas no seu aplicativo de imediato. Depois que o aplicativo for publicado, você poderá configurar a frequência com que cada rede será usada no Centro de Desenvolvimento (ou começar a usar uma rede que ainda não tinha usado) sem precisar fazer alterações de código e reenviar o aplicativo.

    O Visual Studio busca os assemblies necessários para as redes de publicidade selecionadas e adiciona essas referências de assembly ao seu projeto. Após a conclusão desse processo, clique em **OK** na caixa de diálogo **Obtendo Status**.

4.  Na janela **Controle de Anúncios** (Visual Studio 2015) ou **Gerenciador de Serviços** (Visual Studio 2013), selecione opcionalmente cada rede e clique em **Configurar** a fim de inserir as informações de configuração para cada rede para utilização durante teste do aplicativo. Essas informações são salvas no arquivo AdMediator.config no projeto. Você poderá modificar essas informações quando configurar o comportamento da rede de publicidade no painel do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Submit your app and configure ad mediation](submit-your-app-and-configure-ad-mediation.md).
    > **Observação**  Se você não inserir informações de configuração durante esta etapa, o controle de anúncios testará os valores de configuração quando você executar o aplicativo no seu computador de desenvolvimento (para aplicativos UWP e XAML do Windows 8.1) ou no emulador ou dispositivo (para aplicativos do Windows Phone).

5.  Na janela do **Ad Mediator** (Visual Studio 2015) ou do **Gerenciador de Serviços** (Visual Studio 2013), confirme que cada rede de publicidade que você selecionou mostra **Obtido**. Clique em **OK** para enviar as alterações ao seu projeto.

> **Observação**   Se você atualizar posteriormente para uma nova versão do SDK do Microsoft Store Engagement and Monetization, precisará abrir os **Serviços Conectados** novamente para assegurar que todas as DLLs de redes de publicidade obtidas estão atualizadas corretamente.

### Declarar os recursos necessários

Cada rede de publicidade pode exigir determinados recursos do aplicativo. Esses itens são mostrados por cada provedor na janela do **Ad Mediator** (Visual Studio 2015) ou **Gerenciador de Serviços** (Visual Studio 2013). Declare todos os recursos necessários no manifesto do seu aplicativo de forma que os anúncios sejam exibidos corretamente.

A captura de tela a seguir mostra as capacidades necessárias para várias redes de publicidade para um aplicativo XAML do Windows 8.1 ou Windows Phone 8.1.

![gerenciador de serviços exibindo todas as referências obtidas](images/ad-med-8.jpg)

A captura de tela a seguir mostra as capacidades necessárias para várias redes de publicidade em um aplicativo Silverlight para Windows Phone 8.1.

![gerenciador de serviços exibindo todas as referências obtidas](images/ad-med-6.jpg)
### Obter DLLs de rede de publicidade manualmente

Em alguns casos, talvez você veja que determinadas DLLs não foram buscadas. Neste caso, você precisará adicioná-las manualmente. Para obter links de download de assemblies individuais, consulte [Select and manage your ad networks](select-and-manage-your-ad-networks.md).

> **Observação**  Ao adicionar DLLs manualmente, pode aparecer uma mensagem de erro que diz "Não é possível adicionar uma referência a uma DLL de versão superior ou incompatível ao projeto". Para resolver esse erro, clique com o botão na DLL no Gerenciador e selecione **Propriedades**. Na seção Segurança, clique em **Desbloquear**.

![botão desbloquear para resolver mensagem de erro](images/ad-med-4.png)
## Ajuste tamanho e posição

Você pode configurar o tamanho e a posição do controle de anúncios no designer ou em seu código XAML. Verifique se esse tamanho será suficiente para ajustar todos os anúncios que serão exibidos de suas redes de publicidade. Algumas redes de publicidade podem não servir um anúncio se detectarem que o tamanho da tela não é suficiente para a exibição inteira do anúncio. Se qualquer uma das suas unidades de anúncio for maior do que o tamanho padrão, você poderá ajustar o tamanho da tela para acomodar o anúncio maior.

Quando você arrastar o controle para o designer, os tamanhos padrão do controle são:

-   XAML do UWP e Windows 8.1: 300 de largura x 250 de altura.
-   Windows Phone 8.1 XAML: 400 de largura x 67 de altura.
-   Windows Phone 8 e Windows Phone 8.1 Silverlight: 480 de largura x 80 de altura.

Você pode substituir os tamanhos padrão de anúncio usando parâmetros opcionais **Width** e **Height**, como mostrado abaixo.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

Você também pode especificar como cada controle será posicionado para acomodar uma variedade de tamanhos e de posições, dependendo das necessidades do seu aplicativo. Para algumas redes de publicidade, você pode usar parâmetros opcionais para fazer ajustes. Por exemplo, para alinhar anúncios do Microsoft Advertising ao canto inferior esquerdo:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Tamanhos de anúncios aceitos no Microsoft Advertising

O Microsoft Advertising aceita apenas anúncios nos seguintes tamanhos padrão recomendados pelo Interactive Advertising Bureau (IAB) para aplicativos executados nas plataformas a seguir.

-   Windows 10 e Windows 8.1:
    -   160 x 600
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 Mobile, Windows Phone 8.1 e Windows Phone 8:
    -   300 x 50
    -   320 x 50
    -   480 x 80 (só há suporte para esse tamanho no Windows Phone Silverlight)
    -   640 x 100

Pode ser que você queira especificar um tamanho do controle do Ad Mediator que não corresponda a um dos tamanhos de anúncios suportados pelo Microsoft Advertising (por exemplo, pode ser que você queira fazer isso caso um tamanho diferente seja melhor para a interface do usuário do seu aplicativo ou se você também tiver em mente outras redes de publicidade que dão suporte a outros tamanhos de anúncios). Para fazer isso, especifique o tamanho do controle exato desejado no designer ou em seu código XAML e, em seguida, atribua os parâmetros opcionais **Width** e **Height** do Microsoft Advertising para o tamanho mais próximo aceito que caberá nos limites do controle. O controle será exibido com o tamanho exato que você especificar no designer, mas o Microsoft Advertising veiculará os anúncios que corresponderem ao tamanho que você especificar usando os parâmetros opcionais **Width** e **Height**.

Por exemplo, se você tiver um aplicativo UWP e quiser que o controle de anúncios seja exibido em 300 x 300, defina o controle como 300 x 300 no designer ou em seu código XAML. Em seguida, atribua o parâmetro opcional **Width** a 300 e o parâmetro opcional **Height** a 250 para o Microsoft Advertising, conforme mostrado no código a seguir.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

Para verificar se as configurações de tamanho são compatíveis com o Microsoft Advertising, [teste sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md) e certifique-se de que os anúncios de teste do Microsoft Advertising sejam exibidos.

## Pausar, retomar e desabilitar o controle de anúncios

Se você quiser pausar o controle de anúncios por um tempo determinado durante o tempo de execução do aplicativo, use o método AdMediatorControl.Pause(). Observe que quando você fizer isso, o anúncio mais recente continuará sendo mostrado até você retomar a mediação ao chamar o método AdMediatorControl.Resume().

Para desabilitar completamente o mediador de anúncios, use o método AdMediatorControl.Disable(). Isso removerá todos os anúncios mostrados e minimizará o volume de memória do mediador. Você pode chamar AdMediatorControl.Resume() para retomar mediação, mas observe que o tempo de inicialização será mais lento do que o normal após a desabilitação do controle de anúncios.

## Definir tempo limite

Você pode especificar o número de segundos (de 2 a 60) pelos quais o controle de anúncios deverá aguardar depois de solicitar um anúncio da rede de publicidade antes de abandonar a solicitação e fazer uma solicitação para outra rede. Por padrão, o tempo limite para todas as redes de publicidade é de 15 segundos.

O código abaixo mostra como especificar uma duração de tempo limite para o Microsoft Advertising. Você pode modificar as durações e a redes conforme necessário.

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10);
```

> **Observação**  De forma alternativa, você pode definir o valor do tempo limite na página **Monetizar com anúncios** no painel do Centro de Desenvolvimento. Se você definir o tempo limite no código e no painel, o valor definido no código substituirá o valor do painel.

## Manipulação de eventos

Adicionar código para registrar eventos e capturar erros de controle de anúncios pode ajudar na solução de problemas. O exemplo de código abaixo adiciona manipuladores de eventos para eventos específicos de um controle.

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## Lidar com exceções sem tratamento de redes de publicidade

> **Observação**  Como parte do seu teste, identificamos um número de exceções sem tratamento de redes de publicidade específicas que devem ser manipuladas dentro do aplicativo para evitar falhas no aplicativo relacionadas a elas. É altamente recomendável que você copie e cole o código de amostra abaixo em seu arquivo App.xaml.cs.

Código a ser usado em um aplicativo UWP, ou do Windows 8.1 ou do Windows Phone usando C# e XAML.

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

Código a ser usado em um aplicativo Silverlight do Windows Phone

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) && exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }

   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## Tópicos relacionados

* [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md)
* [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md)
* [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de controle de anúncios](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


