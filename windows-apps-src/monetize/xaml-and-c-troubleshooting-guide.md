---
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: Conheça as soluções para problemas comuns de desenvolvimento com as bibliotecas do Microsoft Advertising em aplicativos XAML.
title: Guia de solução de problemas de XAML e C#
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, AdControl, solução de problemas, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: ab8ba3930c13ffcb00d2cb9701a89cafe581b4ff
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506880"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>Guia de solução de problemas de XAML e C#

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Este tópico contém soluções para problemas comuns de desenvolvimento com as bibliotecas do Microsoft Advertising em aplicativos XAML.

* [XAML](#xaml)
  * [AdControl não aparecendo](#xaml-notappearing)
  * [Caixa preta pisca e desaparece](#xaml-blackboxblinksdisappears)
  * [Anúncios não atualizando](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [AdControl não aparecendo](#csharp-adcontrolnotappearing)
  * [Caixa preta pisca e desaparece](#csharp-blackboxblinksdisappears)
  * [Anúncios não atualizando](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl não aparece

1.  Certifique-se de que a funcionalidade **Internet (Client)** esteja selecionada em Package.appxmanifest.

2.  Verifique a ID do aplicativo e a ID da unidade de anúncio. Essas IDs devem corresponder à ID do aplicativo e à ID da unidade do AD que você obteve no Partner Center. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Verifique as propriedades **Height** e **Width**. Elas devem ser definidas como um dos [Tamanhos de anúncio com suporte para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Verifique a posição do elemento. O [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) deve estar dentro da área visível.

5.  Verifique a propriedade **Visibility**. A propriedade opcional **Visibility** não deve ser definida como recolhida ou oculta. Essa propriedade pode ser definida embutida (como mostrado abaixo) ou em uma folha de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Verifique o pai do **AdControl**. Se o elemento **AdControl** residir em um elemento pai, o pai deverá ser ativo e visível.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  Certifique-se de que o **AdControl** não esteja oculto no visor. O **AdControl** deve estar visível para que os anúncios sejam exibidos corretamente.

8.  Valores dinâmicos para **ApplicationId** e **AdUnitId** não devem ser testados no emulador. Para garantir que o **AdControl** está funcionando como esperado, use os [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para **ApplicationId** e **AdUnitId**.

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Caixa preta pisca e desaparece

1.  Verifique novamente todas as etapas na seção [AdControl não aparece](#xaml-notappearing) anterior.

2.  Manipule o evento **ErrorOccurred** e use a mensagem que é passada ao manipulador de eventos para determinar se ocorreu um erro e qual tipo de erro foi gerado. Consulte [Tratamento de erros no passo a passo do XAML/C#](error-handling-in-xamlc-walkthrough.md) para obter mais informações.

    Este exemplo demonstra um manipulador de eventos **ErrorOccurred**. O primeiro snippet é a marcação da interface do usuário XAML.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Este exemplo demonstra o código C# correspondente.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    O erro mais comum que causa uma caixa preta é “Nenhum anúncio disponível”. Esse erro significa que não há anúncios disponíveis para serem retornados da solicitação.

3.  O [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) está funcionando normalmente.

    Por padrão, o **AdControl** será recolhido quando não conseguir exibir um anúncio. Se outros elementos forem filhos do mesmo pai, eles poderão se mover para preencher a lacuna do **AdControl** recolhido e se expandir quando a próxima solicitação for feita.

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anúncios não são atualizados

1.  Verifique a propriedade [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled). Por padrão, essa propriedade opcional é definida como **True**. Quando definida como **False**, o método [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) deverá ser usado para recuperar outro anúncio.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Verifique as chamadas para o método **Refresh**. Ao usar a atualização automática, não é possível usar **Refresh** para recuperar outro anúncio. Ao usar a atualização manual, **Refresh** só deverá ser chamado depois de um intervalo mínimo de 30 a 60 segundos dependendo da conexão de dados atual do dispositivo.

    Os trechos de código a seguir mostram um exemplo de como usar o método **Refresh**. O primeiro snippet é a marcação da interface do usuário XAML.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Esse snippet mostra um exemplo do código C# por trás da marcação da interface do usuário.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

<span id="csharp"/>

## <a name="c"></a>\# de C #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl não aparece

1.  Certifique-se de que a funcionalidade **Internet (Client)** esteja selecionada em Package.appxmanifest.

2.  Verifique se o **AdControl** está instanciado. Se o **AdControl** não estiver instanciado, ele não estará disponível.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  Verifique a ID do aplicativo e a ID da unidade de anúncio. Essas IDs devem corresponder à ID do aplicativo e à ID da unidade do AD que você obteve no Partner Center. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Verifique os parâmetros **Height** e **Width**. Elas devem ser definidas como um dos [tamanhos de anúncio com suporte para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Certifique-se de que o **AdControl** seja adicionado a um elemento pai. Para ser exibido, o **AdControl** deve ser adicionado como um filho a um controle pai (por exemplo, um **StackPanel** ou **Grid**).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  Verifique o parâmetro **Margin**. O **AdControl** deve estar dentro da área visível.

7.  Verifique a propriedade **Visibility**. A propriedade opcional **Visibility** deve ser definida como **Visible**.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Verifique o pai do **AdControl**. O pai deve estar ativo e visível.

9. Valores dinâmicos para **ApplicationId** e **AdUnitId** não devem ser testados no emulador. Para garantir que o **AdControl** está funcionando como esperado, use os [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para **ApplicationId** e **AdUnitId**.

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Caixa preta pisca e desaparece

1.  Verifique novamente todas as etapas na seção [AdControl não aparece](#csharp-adcontrolnotappearing) acima.

2.  Manipule o evento **ErrorOccurred** e use a mensagem que é passada ao manipulador de eventos para determinar se ocorreu um erro e qual tipo de erro foi gerado. Consulte [Tratamento de erros no passo a passo do XAML/C#](error-handling-in-xamlc-walkthrough.md) para obter mais informações.

    Os exemplos a seguir mostram o código básico necessário para implementar uma chamada de erro. Esse código XAML define um **TextBlock** que é usado para exibir a mensagem de erro.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Esse código C# recupera a mensagem de erro e a exibe no **TextBlock**.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    O erro mais comum que causa uma caixa preta é “Nenhum anúncio disponível”. Esse erro significa que não há anúncios disponíveis para serem retornados da solicitação.

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anúncios não são atualizados

1.  Verifique se a propriedade [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) do **AdControl** está definida como false. Por padrão, essa propriedade opcional é definida como **true**. Quando definida como **false**, o método **Refresh** deve ser usado para recuperar outro anúncio.

2.  Verifique as chamadas para o método [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx). Ao usar a atualização automática (**IsAutoRefreshEnabled** definido como **true**), não é possível usar **Refresh** para recuperar outro anúncio. Ao usar a atualização manual (**IsAutoRefreshEnabled** definido como **false**), **Refresh** só deverá ser chamado depois de um intervalo mínimo de 30 a 60 segundos dependendo da conexão de dados atual do dispositivo.

    O exemplo a seguir demonstra como chamar o método **Refresh**.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

 

 
