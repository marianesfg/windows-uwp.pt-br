---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: Obter informações de bateria
description: Saiba como obter informações de bateria detalhadas usando APIs no namespace Windows.Devices.Power.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ea9be48da57e260cdb3d5d1c9a9a0b564c1f4386
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370040"
---
# <a name="get-battery-information"></a>Obter informações de bateria


** APIs importantes **

-   [**Windows.Devices.Power**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power)
-   [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)

Saiba como obter informações de bateria detalhadas usando APIs no namespace [**Windows.Devices.Power**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power). Um *Relatório da bateria* ([**BatteryReport**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.BatteryReport)) descreve a carga, a capacidade e o status de uma bateria ou um agregado de baterias. Este tópico demonstra como seu aplicativo pode obter relatórios de bateria e ser notificado sobre alterações. Os exemplos de código são do aplicativo básico de bateria listado no final deste tópico.

## <a name="get-aggregate-battery-report"></a>Obter relatório de baterias agregadas


Alguns dispositivos têm mais de uma bateria e nem sempre é óbvio como cada uma contribui com a capacidade de energia geral do dispositivo. É aí que a classe [**AggregateBattery**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.aggregatebattery) entra. A *bateria agregada* representa todos os controladores de bateria conectados ao dispositivo e pode fornecer um único objeto geral [**BatteryReport**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.BatteryReport).

**Observação**  um [ **bateria** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.Battery) classe, na verdade, corresponde a um controlador de bateria. Dependendo do dispositivo, às vezes, o controlador é conectado à bateria física e, às vezes, ao compartimento do dispositivo. Assim, é possível criar um objeto de bateria, mesmo quando não há baterias presentes. Outras vezes, o objeto de bateria pode ser **nulo**.

Quando você tiver um objeto de bateria agregada, chame [**GetReport**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.getreport) para o [**BatteryReport**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.BatteryReport)correspondente.

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## <a name="get-individual-battery-reports"></a>Obter relatórios de bateria individual

Você também pode criar um objeto [**BatteryReport**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.BatteryReport) para baterias individuais. Use [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.getdeviceselector) com o método [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) para obter uma coleção de objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) que representam qualquer controlador de bateria conectado ao dispositivo. Em seguida, usando a propriedade **Id** do objeto **DeviceInformation** desejado, crie um [**Battery**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.Battery) correspondente ao método [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.fromidasync). Por fim, chame [**GetReport**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.getreport) para obter o relatório de bateria individual.

Este exemplo mostra como criar um relatório de bateria para todas as baterias conectadas ao dispositivo.

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## <a name="access-report-details"></a>Acessar detalhes do relatório

O objeto [**BatteryReport**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.BatteryReport) fornece muitas informações de bateria. Para obter mais informações, consulte a referência de API para suas propriedades: **Status** (uma [ **BatteryStatus** ](https://docs.microsoft.com/previous-versions/windows/dn818458(v=win.10)) enumeração), [ **ChargeRateInMilliwatts**](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.chargerateinmilliwatts), [ **DesignCapacityInMilliwattHours**](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.designcapacityinmilliwatthours), [ **FullChargeCapacityInMilliwattHours**](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours), e [  **RemainingCapacityInMilliwattHours**](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours). Este exemplo mostra algumas das propriedades de relatório de bateria usadas no aplicativo básico de bateria, que é fornecido no final deste tópico.

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## <a name="request-report-updates"></a>Solicitar atualizações de relatório

O objeto [**Battery**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.Battery) aciona o evento [**ReportUpdated**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.reportupdated) quando a carga, a capacidade ou o status da bateria muda. Isso geralmente ocorre imediatamente para alterações de status e periodicamente para todas as outras alterações. Este exemplo mostra como registrar-se para atualizações de relatório de bateria.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## <a name="handle-report-updates"></a>Manipular atualizações de relatório

Quando ocorre uma atualização de bateria, o evento [**ReportUpdated**](https://docs.microsoft.com/uwp/api/windows.devices.power.battery.reportupdated) passa o objeto [**Battery**](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.Battery) correspondente para o método de manipulador de evento. Entretanto, esse manipulador de evento não é chamado a partir do thread de interface do usuário. Será necessário usar o objeto [**Dispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) para invocar qualquer mudança de IU, como mostrado neste exemplo.

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## <a name="example-basic-battery-app"></a>Exemplo: aplicativo básico de bateria

Teste essas APIs criando o aplicativo básico de bateria a seguir no Microsoft Visual Studio. Na página inicial do Visual Studio, clique em **Novo Projeto** e nos modelos de **Visual C# &gt; Windows &gt; Universal**, crie um novo aplicativo usando o modelo **Aplicativo em Branco**.

Em seguida, abra o arquivo **MainPage.xaml** e copie o XML a seguir nesse arquivo (substituindo seu conteúdo original).

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

Se o nome do seu aplicativo não for **App1**, será necessário substituir a primeira parte do nome da classe no trecho anterior pelo namespace do seu aplicativo. Por exemplo, se você criou um projeto chamado **BasicBatteryApp**, substitua `x:Class="App1.MainPage"` por `x:Class="BasicBatteryApp.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:BasicBatteryApp"`.

Em seguida, abra o arquivo **MainPage.xaml.cs** do seu projeto e substitua o código existente pelo seguinte.

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar & labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

Se o nome do seu aplicativo não for **App1**, será necessário renomear o namespace no exemplo anterior para o nome que você deu ao seu projeto. Por exemplo, se você criou um projeto chamado **BasicBatteryApp**, substitua o namespace `App1` pelo namespace `BasicBatteryApp`.

Por fim, para executar este aplicativo básico de bateria: no menu **Depurar**, clique em **Iniciar Depuração** para testar a solução.

**Dica**  receber valores numéricos da [ **BatteryReport** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Power.BatteryReport) objeto, depurar seu aplicativo no **computador Local** ou externo **Dispositivo** (por exemplo, um Windows Phone). Ao fazer a depuração em um emulador de dispositivo, o objeto **BatteryReport** devolve **nulo** às propriedade de capacidade e taxa.

 

