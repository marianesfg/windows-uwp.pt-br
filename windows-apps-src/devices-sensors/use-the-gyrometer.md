---
author: muhsinking
ms.assetid: 454953E1-DD8F-44B7-A614-7BAD8C683536
title: Usar o girômetro
description: Saiba como usar o girômetro para detectar mudanças no movimento do usuário.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25be2cfab15378f14aed61dcaae1e7e85159f36e
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4747794"
---
# <a name="use-the-gyrometer"></a>Usar o girômetro


**APIs Importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Girômetro**](https://msdn.microsoft.com/library/windows/apps/BR225718)

**Exemplo**

-   Para obter uma implementação completa, consulte o [exemplo de girômetro](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/gyrometer).

Saiba como usar o girômetro para detectar mudanças no movimento do usuário.

Os girômetros complementam os acelerômetros como controladores de jogos. O acelerômetro pode medir movimento linear enquanto o girômetro mede a velocidade angular ou movimento rotacional.

## <a name="prerequisites"></a>Pré-requisitos

Você deve estar familiarizado com a linguagem XAML, o Microsoft Visual C# e eventos.

O dispositivo ou emulador que você está usando deve ter suporte para um girômetro.

## <a name="create-a-simple-gyrometer-app"></a>Criar um aplicativo simples de girômetro

Esta seção está dividida em duas subseções. A primeira subseção guiará você pelas etapas necessárias para criar um aplicativo simples de girômetro do zero. A subseção seguintes explica o aplicativo que você acabou de criar.

###  <a name="instructions"></a>Instruções

-   Crie um novo projeto. Escolha um **Aplicativo (Universal do Windows) em Branco** nos modelos de projetos do **Visual C#**.

-   Abra o arquivo MainPage.xaml.cs do projeto e substitua o código existente pelo exemplo abaixo.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the gyrometer


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Gyrometer _gyrometer; // Our app' s gyrometer object

            // This event handler writes the current gyrometer reading to
            // the three textblocks on the app' s main page.

            private async void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    GyrometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityZ);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object

                if (_gyrometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _gyrometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _gyrometer.ReportInterval = reportInterval;

                    // Assign an event handler for the gyrometer reading-changed event
                    _gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, GyrometerReadingChangedEventArgs>(ReadingChanged);
                }

            }
        }
    }
```

Você precisará substituir o nome do namespace no trecho anterior pelo nome que você deu a seu projeto. Por exemplo, se você criou um projeto denominado **GyrometerCS**, pode substituir `namespace App1` por `namespace GyrometerCS`.

-   Abra o arquivo MainPage.xaml e substitua o conteúdo original pelo XML abaixo.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
            <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>

        </Grid>
    </Page>
```

Você precisará substituir a primeira parte do nome da classe no trecho anterior pelo namespace de seu aplicativo. Por exemplo, se você criou um projeto denominado **GyrometerCS**, pode substituir `x:Class="App1.MainPage"` por `x:Class="GyrometerCS.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:GyrometerCS"`.

-   Pressione F5 ou selecione **Depurar** > **Iniciar Depuração** para criar, implantar e executar o aplicativo.

Quando o aplicativo estiver em execução, você poderá alterar os valores do girômetro. Basta mover o dispositivo ou usar as ferramentas do emulador.

-   Pare o aplicativo. Basta retornar ao Visual Studio e pressionar Shift + F5 ou selecionar **Depurar** > **Parar Depuração** para parar o aplicativo.

###  <a name="explanation"></a>Explicação

O exemplo anterior comprova que você precisará escrever pouco código para integrar a entrada do girômetro ao seu aplicativo.

O aplicativo estabelece uma conexão com o girômetro padrão no método **MainPage**.

```csharp
_gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
```

O aplicativo estabelece o intervalo de relatório **MainPage**. Esse código recupera o intervalo mínimo com suporte do dispositivo e o compara com um intervalo solicitado de 16 milissegundos (aproximadamente a uma taxa de atualização de 60 Hz). Se o intervalo mínimo suportado for maior que o intervalo solicitado, o código definirá valor mínimo. Caso contrário, ele definirá o valor para o intervalo solicitado.

```csharp
uint minReportInterval = _gyrometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_gyrometer.ReportInterval = reportInterval;
```

Os novos dados do girômetro são capturados no método **ReadingChanged**. Toda vez que o driver do sensor recebe novos dados do sensor, ele transmite o valor para seu aplicativo usando este manipulador de eventos. O aplicativo registra este manipulador de eventos na seguinte linha.

```csharp
_gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer,
GyrometerReadingChangedEventArgs>(ReadingChanged);
```

Esses novos valores são gravados nos TextBlocks encontrados no XAML do projeto.

```xml
        <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
        <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
        <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
        <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
        <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
        <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>
```

 ## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo de girômetro](http://go.microsoft.com/fwlink/p/?linkid=241379)
