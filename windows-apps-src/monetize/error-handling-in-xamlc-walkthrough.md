---
author: Xansky
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: Saiba como detectar erros de AdControl em seu aplicativo.
title: Tratamento de erros no passo a passo do XAML/C#
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, tratamento de erros, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: be101f5ec189d822bc9704b435f4a098b61f57ac
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6036776"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>Tratamento de erros no passo a passo do XAML/C#

Este guia passo a passo demonstra como detectar erros relacionados a anúncios em seu aplicativo. O passo a passo usa um [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para exibir um anúncio em faixa, mas os conceitos gerais nele também se aplicam a anúncios intersticiais e anúncios nativos.

Estes exemplos pressupõem que você tenha um aplicativo XAML/C# que contém um **AdControl**. Para obter instruções passo a passo que demonstram como adicionar um **AdControl** ao seu aplicativo, consulte [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md). 

1.  No arquivo MainPage.xaml, localize a definição do **AdControl**. Esse código fica assim.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   Após a propriedade **Width**, mas antes da marca de fechamento, atribua o nome de um manipulador de eventos de erro ao evento [ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred). Neste guia passo a passo, o nome do manipulador de eventos de erro é **OnAdError**.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  Para gerar um erro em tempo de execução, crie um segundo **AdControl** com uma ID de aplicativo diferente. Como todos os objetos **AdControl** em um aplicativo devem usar a mesma ID de aplicativo, criar um **AdControl** adicional com uma id de aplicativo diferente causará um erro.

    Defina um segundo **AdControl** no MainPage.xaml logo após o primeiro **AdControl**, e defina a propriedade [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) como zero (“0”).
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  No MainPage.xaml.cs, adicione o manipulador de eventos **OnAdError** a seguir à classe **MainPage**. Esse manipulador de eventos grava as informações na janela **Saída** do Visual Studio.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Compile e execute o projeto. Depois que o aplicativo estiver em execução, você verá uma mensagem semelhante a que está abaixo na janela **Saída** do Visual Studio.
    ```
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
