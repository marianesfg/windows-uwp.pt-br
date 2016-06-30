---
author: mcleanbyron
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: "Conheça as soluções para problemas comuns de desenvolvimento com as bibliotecas do Microsoft Advertising em aplicativos XAML."
title: "Guia de solução de problemas de XAML e C#"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: ef9ad8f8056b17793d7ad8230e410e014edf2c95

---

# Guia de solução de problemas de XAML e C#

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico contém soluções para problemas comuns de desenvolvimento com as bibliotecas do Microsoft Advertising em aplicativos XAML.

-   [XAML](#xaml)

    -   [AdControl não aparece](#xaml-notappearing)

    -   [Caixa preta pisca e desaparece](#xaml-blackboxblinksdisappears)

    -   [Anúncios não são atualizados](#xaml-adsnotrefreshing)

-   [C#](#csharp)

    -   [AdControl não aparece](#csharp-adcontrolnotappearing)

    -   [Caixa preta pisca e desaparece](#csharp-blackboxblinksdisappears)

    -   [Anúncios não são atualizados](#csharp-adsnotrefreshing)

<span id="xaml"/>
## XAML

<span id="xaml-notappearing"/>
### AdControl não aparece

1.  Certifique-se de que a funcionalidade **Internet (Client)** esteja selecionada em Package.appxmanifest.

2.  Verifique a ID do aplicativo e a ID da unidade de anúncio. Essas IDs devem coincidir com a ID do aplicativo e a ID da unidade de anúncio que você obteve no Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Verifique as propriedades **Height** e **Width**. Elas devem ser definidas como um dos [Tamanhos de anúncio com suporte para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Verifique a posição do elemento. O [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) deve estar dentro da área visível.

5.  Verifique a propriedade **Visibility**. A propriedade opcional **Visibility** não deve ser definida como recolhida ou oculta. Essa propriedade pode ser definida embutida (como mostrado abaixo) ou em uma folha de estilos externa.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Verifique a propriedade **IsEnabled**. A propriedade opcional `IsEnabled` deve ser definida como `True`.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  IsEnabled="True"
                  Width="728" Height="90" />
    ```

7.  Verifique o pai do **AdControl**. Se o elemento **AdControl** residir em um elemento pai, o pai deverá ser ativo e visível.

    ``` syntax
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

8.  Certifique-se de que o **AdControl** não esteja oculto no visor. O **AdControl** deve estar visível para que os anúncios sejam exibidos corretamente.

9.  Valores dinâmicos para **ApplicationId** e **AdUnitId** não devem ser testados no emulador. Para garantir que o **AdControl** esteja funcionando conforme o esperado, use as IDs de teste para **ApplicationId** e **AdUnitId** encontradas em [Valores de modo de teste](test-mode-values.md).

<span id="xaml-blackboxblinksdisappears"/>
### Caixa preta pisca e desaparece

1.  Verifique novamente todas as etapas na seção [AdControl não aparece](#xaml-notappearing) anterior.

2.  Manipule o evento **ErrorOccurred** e use a mensagem que é passada ao manipulador de eventos para determinar se ocorreu um erro e qual tipo de erro foi gerado. Consulte [Tratamento de erros no passo a passo do XAML/C#](error-handling-in-xamlc-walkthrough.md) para obter mais informações.

    Este exemplo demonstra um manipulador de eventos **ErrorOccurred**. O primeiro trecho é a marcação da interface do usuário XAML.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />

    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Este exemplo demonstra o código correspondente.

    ``` syntax
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.Error.Message;
    }
    ```

    O erro mais comum que causa uma caixa preta é “Nenhum anúncio disponível”. Esse erro significa que não há anúncios disponíveis para serem retornados da solicitação.

3.  O [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) está funcionando normalmente.

    Por padrão, o **AdControl** será recolhido quando não conseguir exibir um anúncio. Se outros elementos forem filhos do mesmo pai, eles poderão se mover para preencher a lacuna do **AdControl** recolhido e se expandir quando a próxima solicitação for feita.

<span id="xaml-adsnotrefreshing"/>
### Anúncios não são atualizados

1.  Verifique a propriedade [IsAutoRefreshEnabled](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx). Por padrão, essa propriedade opcional é definida como **True**. Quando definida como **False**, o método [Refresh](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) deverá ser usado para recuperar outro anúncio.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Verifique as chamadas para o método **Refresh**. Ao usar a atualização automática, não é possível usar **Refresh** para recuperar outro anúncio. Ao usar a atualização manual, **Refresh** só deverá ser chamado depois de um intervalo mínimo de 30 a 60 segundos dependendo da conexão de dados atual do dispositivo.

    Os trechos de código a seguir mostram um exemplo de como usar o método **Refresh**. O primeiro trecho é a marcação da interface do usuário XAML.

    ``` syntax
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Esse trecho de código mostra um exemplo do código C# por trás da marcação da interface do usuário.

    ``` syntax
    public Ads()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

<span id="csharp"/>
## C\# #

<span id="csharp-adcontrolnotappearing"/>
### AdControl não aparece

1.  Certifique-se de que a funcionalidade **Internet (Client)** esteja selecionada em Package.appxmanifest.

2.  Verifique se o **AdControl** está instanciado. Se o **AdControl** não estiver instanciado, ele não estará disponível.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public sealed partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl()
                {
                    ApplicationId = "{ApplicationID}",
                    AdUnitId = "{AdUnitID}",
                    Height = 90,
                    Width = 728
                };
            }
        }
    }
    ```

3.  Verifique a ID do aplicativo e a ID da unidade de anúncio. Essas IDs devem coincidir com a ID do aplicativo e a ID da unidade de anúncio que você obteve no Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Verifique os parâmetros **Height** e **Width**. Elas devem ser definidas como um dos [tamanhos de anúncio com suporte para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Certifique-se de que o **AdControl** seja adicionado a um elemento pai. Para ser exibido, o **AdControl** deve ser adicionado como um filho a um controle pai (por exemplo, um **StackPanel** ou **Grid**).

    ``` syntax
    ContentPanel.Children.Add(adControl);
    ```

6.  Verifique o parâmetro **Margin**. O **AdControl** deve estar dentro da área visível.

7.  Verifique a propriedade **Visibility**. A propriedade opcional **Visibility** deve ser definida como **Visible**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Verifique a propriedade **IsEnabled**. A propriedade opcional **IsEnabled** deve ser definida como **True**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsEnabled = True;
    ```

9.  Verifique o pai do **AdControl**. O pai deve estar ativo e visível.

10. Valores dinâmicos para **ApplicationId** e **AdUnitId** não devem ser testados no emulador. Para garantir que o **AdControl** esteja funcionando conforme o esperado, use as IDs de teste para **ApplicationId** e **AdUnitId** encontradas em [Valores de modo de teste](test-mode-values.md).

<span id="csharp-blackboxblinksdisappears"/>
### Caixa preta pisca e desaparece

1.  Verifique novamente todas as etapas na seção [AdControl não aparece](#csharp-adcontrolnotappearing) acima.

2.  Manipule o evento **ErrorOccurred** e use a mensagem que é passada ao manipulador de eventos para determinar se ocorreu um erro e qual tipo de erro foi gerado. Consulte [Tratamento de erros no passo a passo do XAML/C#](error-handling-in-xamlc-walkthrough.md) para obter mais informações.

    Os exemplos a seguir mostram o código básico necessário para implementar uma chamada de erro. Esse código XAML define um **TextBlock** que é usado para exibir a mensagem de erro.

    ``` syntax
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Esse código C# recupera a mensagem de erro e a exibe no **TextBlock**.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl();
                adControl.ApplicationId = "{ApplicationID}";
                adControl.AdUnitId = "{AdUnitID}";
                adControl.Height = 90;
                adControl.Width = 728;
                adControl.ErrorOccurred += (s,e) =>
                {
                    TextBlock1.Text = e.Error.Message;
                };
            }
        }
    }
    ```

    O erro mais comum que causa uma caixa preta é “Nenhum anúncio disponível”. Esse erro significa que não há anúncios disponíveis para serem retornados da solicitação.

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

<span id="csharp-adsnotrefreshing"/>
### Anúncios não são atualizados

1.  Verifique a propriedade **IsAutoRefreshEnabled**. Por padrão, essa propriedade opcional é definida como **True**. Quando definida como **False**, o método **Refresh** deverá ser usado para recuperar outro anúncio.

    O exemplo a seguir demonstra como usar a propriedade **IsAutoRefreshEnabled**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsAutoRefreshEnabled = true;
    ```

2.  Verifique as chamadas para o método **Refresh**. Ao usar a atualização automática, não é possível usar **Refresh** para recuperar outro anúncio. Ao usar a atualização manual, **Refresh** só deverá ser chamado depois de um intervalo mínimo de 30 a 60 segundos dependendo da conexão de dados atual do dispositivo.

    O exemplo a seguir demonstra como chamar o método **Refresh**.

    ``` syntax
    public MainPage()
    {
        InitializeComponent();

        adControl = new AdControl();
        adControl.ApplicationId = "{ApplicationID}";
        adControl.AdUnitId = "{AdUnitID}";
        adControl.Height = 90;
        adControl.Width = 728;
        adControl.IsAutoRefreshEnabled = false;

        ContentPanel.Children.Add(adControl);

        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl.Refresh();
        timer.Start();
    }
    ```

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

 

 



<!--HONumber=Jun16_HO4-->


