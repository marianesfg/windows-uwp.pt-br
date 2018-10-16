---
author: muhsinking
ms.assetid: F90686F5-641A-42D9-BC44-EC6CA11B8A42
title: Usar o acelerômetro
description: Saiba como usar o acelerômetro para responder ao movimento do usuário.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2848b9a7326bdac084120ec9b75f067718f14853
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4689325"
---
# <a name="use-the-accelerometer"></a>Usar o acelerômetro


**APIs Importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Acelerômetro**](https://msdn.microsoft.com/library/windows/apps/BR225687)

**Amostra**

-   Para obter uma implementação completa, consulte o [exemplo de acelerômetro](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer).

Saiba como usar o acelerômetro para responder ao movimento do usuário.

Um aplicativo de jogos simples conta com um sensor único, o acelerômetro, como um dispositivo de entrada. Esses aplicativos geralmente usam apenas um ou dois eixos para entrada, mas também podem usar o evento shake como outra fonte de entrada.

## <a name="prerequisites"></a>Pré-requisitos

Você deve estar familiarizado com a linguagem XAML, o Microsoft Visual C# e eventos.

O dispositivo ou emulador que você estiver usando deve ter suporte para um acelerômetro.

## <a name="create-a-simple-accelerometer-app"></a>Criar um aplicativo simples de acelerômetro

Esta seção está dividida em duas subseções. A primeira subseção guiará você pelas etapas necessárias para criar um aplicativo simples de acelerômetro do zero. A subseção seguintes explica o aplicativo que você acabou de criar.

### <a name="instructions"></a>Instruções

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

    // Required to support the core dispatcher and the accelerometer

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    namespace App1
    {

        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private Accelerometer _accelerometer;

            // This event handler writes the current accelerometer reading to
            // the three acceleration text blocks on the app' s main page.

            private async void ReadingChanged(object sender, AccelerometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    AccelerometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationZ);

                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _accelerometer = Accelerometer.GetDefault();

                if (_accelerometer != null)
                {
                    // Establish the report interval
                    uint minReportInterval = _accelerometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _accelerometer.ReportInterval = reportInterval;

                    // Assign an event handler for the reading-changed event
                    _accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer, AccelerometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Você precisará substituir o nome do namespace no trecho anterior pelo nome que você deu a seu projeto. Por exemplo, se você criou um projeto denominado **AccelerometerCS**, pode substituir `namespace App1` por `namespace AccelerometerCS`.

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
            <TextBlock HorizontalAlignment="Left" Height="25" Margin="8,20,0,0" TextWrapping="Wrap" Text="X-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFEDE6E6"/>
            <TextBlock HorizontalAlignment="Left" Height="27" Margin="8,49,0,0" TextWrapping="Wrap" Text="Y-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF5F2F2"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,80,0,0" TextWrapping="Wrap" Text="Z-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF6F0F0"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>

        </Grid>
    </Page>
```

Você precisará substituir a primeira parte do nome da classe no trecho anterior pelo namespace de seu aplicativo. Por exemplo, se você criou um projeto denominado **AccelerometerCS**, pode substituir `x:Class="App1.MainPage"` por `x:Class="AccelerometerCS.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:AccelerometerCS"`.

-   Pressione F5 ou selecione **Depurar** &gt; **Iniciar Depuração** para criar, implantar e executar o aplicativo.

Quando o aplicativo estiver em execução, você poderá alterar os valores do acelerômetro. Basta mover o dispositivo ou usar as ferramentas do emulador.

-   Pare o aplicativo. Basta retornar ao Visual Studio e pressionar Shift + F5 ou selecionar **Depurar** &gt; **Parar Depuração** para parar o aplicativo.

### <a name="explanation"></a>Explicação

O exemplo anterior demonstra a codificação mínima que será necessário gravar para integrar a entrada do acelerômetro ao aplicativo.

O aplicativo estabelece uma conexão com o acelerômetro padrão no método **MainPage**.

```csharp
_accelerometer = Accelerometer.GetDefault();
```

O aplicativo estabelece o intervalo de relatório **MainPage**. Esse código recupera o intervalo mínimo com suporte do dispositivo e o compara com um intervalo solicitado de 16 milissegundos (aproximadamente a uma taxa de atualização de 60 Hz). Se o intervalo mínimo suportado for maior que o intervalo solicitado, o código definirá valor mínimo. Caso contrário, ele definirá o valor para o intervalo solicitado.

```csharp
uint minReportInterval = _accelerometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_accelerometer.ReportInterval = reportInterval;
```

Os novos dados do acelerômetro são capturados no método **ReadingChanged**. Toda vez que o driver do sensor recebe novos dados do sensor, ele transmite o valor para seu aplicativo usando este manipulador de eventos. O aplicativo registra este manipulador de eventos na seguinte linha.

```csharp
_accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer,
AccelerometerReadingChangedEventArgs>(ReadingChanged);
```

Esses novos valores são gravados nos TextBlocks encontrados no XAML do projeto.

```xml
<TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
 <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
 <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>
```
