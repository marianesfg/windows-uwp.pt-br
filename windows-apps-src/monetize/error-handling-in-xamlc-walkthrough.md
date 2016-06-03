---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: Saiba como detectar erros de AdControl em seu aplicativo.
title: Tratamento de erros no passo a passo do XAML/C#

---

# Tratamento de erros no passo a passo do XAML/C#


\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico demonstra como detectar erros de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em seu aplicativo.

Estes exemplos pressupõem que você tenha um aplicativo XAML/C# que contém um **AdControl**. Para obter instruções passo a passo que demonstram como adicionar um **AdControl** ao seu aplicativo, consulte [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md). Para um projeto de exemplo completo que demonstra como adicionar anúncios em faixa a um aplicativo XAML em C# e C++, consulte os [Exemplos de publicidade no GitHub](http://aka.ms/githubads).

1.  No arquivo MainPage.xaml, localize a definição do **AdControl**. Esse código fica assim.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300" />
    ```

2.   Após a propriedade **Width**, mas antes da marca de fechamento, atribua o nome de um manipulador de eventos de erro ao evento [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx). Neste guia passo a passo, o nome do manipulador de eventos de erro é **OnAdError**.

    ``` syntax
    ErrorOccurred="OnAdError"
    ```

    O código XAML completo que define o **AdControl** fica assim.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300"
       ErrorOccurred="OnAdError"/>
    ```

2.  Para gerar um erro em tempo de execução, crie um segundo **AdControl** com uma ID de aplicativo diferente. Como todos os objetos **AdControl** em um aplicativo devem usar a mesma ID de aplicativo, criar um **AdControl** adicional com uma id de aplicativo diferente causará um erro.

    Defina um segundo **AdControl** no MainPage.xaml logo após o primeiro **AdControl**, e defina a propriedade [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) como zero (“0”).

    ``` syntax
    <UI:AdControl
      ApplicationId="0"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,265,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError" />
    ```

3.  No MainPage.xaml.cs, adicione o manipulador de eventos **OnAdError** a seguir à classe **MainPage**. Esse manipulador de eventos grava as informações na janela **Saída** do Visual Studio.

    ``` syntax
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Compile e execute o projeto.

Depois que o aplicativo estiver em execução, você verá uma mensagem semelhante a que está abaixo na janela **Saída** do Visual Studio.

``` syntax
AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
```

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->

