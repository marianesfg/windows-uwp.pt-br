---
author: DBirtolo
ms.assetid: 15BAB25C-DA8C-4F13-9B8F-EA9E4270BCE9
title: Usar o sensor de luz
description: Aprenda a usar o sensor de luz ambiente para detectar alterações na iluminação.
---
# Usar o sensor de luz

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** APIs importantes **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**LightSensor**](https://msdn.microsoft.com/library/windows/apps/BR225790)

Aprenda a usar o sensor de luz ambiente para detectar alterações na iluminação.

Um sensor de luz ambiente é um dos vários tipos de sensores de ambiente que permitem aos aplicativos reagir a alterações no ambiente do usuário.

## Pré-requisitos

Você deve estar familiarizado com a linguagem XAML, o Microsoft Visual C# e eventos.

O dispositivo ou emulador que você estiver usando deve ter um sensor de luz ambiente.

## Criar um aplicativo simples de sensor de luz

Esta seção está dividida em duas subseções. A primeira subseção guiará você pelas etapas necessárias para criar um aplicativo simples de sensor de luz do zero. A subseção seguintes explica o aplicativo que você acabou de criar.

###  Instruções

-   Crie um novo projeto. Escolha um **Aplicativo (Universal do Windows) em Branco** nos modelos de projetos do **Visual C#**.

-   Abra o arquivo BlankPage.xaml.cs do projeto e substitua o código existente pelo exemplo abaixo.

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
    using Windows.Devices.Sensors; // Required to access the sensor platform and the ALS

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class BlankPage : Page
        {
            private LightSensor _lightsensor; // Our app' s lightsensor object
           
            // This event handler writes the current light-sensor reading to 
            // the textbox named "txtLUX" on the app' s main page.

            private void ReadingChanged(object sender, LightSensorReadingChangedEventArgs e)
            {
                Dispatcher.RunAsync(CoreDispatcherPriority.Normal, (s, a) =>
                {
                    LightSensorReading reading = (a.Context as LightSensorReadingChangedEventArgs).Reading;
                    txtLuxValue.Text = String.Format("{0,5:0.00}", reading.IlluminanceInLux);
                });
            }

            public BlankPage()
            {
                InitializeComponent();
                _lightsensor = LightSensor.GetDefault(); // Get the default light sensor object

                // Assign an event handler for the ALS reading-changed event
                if (_lightsensor != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _lightsensor.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _lightsensor.ReportInterval = reportInterval;

                    // Establish the even thandler
                    _lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, LightSensorReadingChangedEventArgs>(ReadingChanged);
                }

            }

        }
    }
```

Você precisará substituir o nome do namespace no trecho anterior pelo nome que você deu a seu projeto. Por exemplo, se você criou um projeto denominado **LightingCS**, pode substituir `namespace App1` por `namespace LightingCS`.

-   Abra o arquivo MainPage.xaml e substitua o conteúdo original pelo XML abaixo.

```xml
    <Page
        x:Class="App1.BlankPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
            <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>


        </Grid>

    </Page>
```

Você precisará substituir a primeira parte do nome da classe no trecho anterior pelo namespace de seu aplicativo. Por exemplo, se você criou um projeto denominado **LightingCS**, poderá substituir `x:Class="App1.MainPage"` por `x:Class="LightingCS.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:LightingCS"`.

-   Pressione F5 ou selecione **Depurar** > **Iniciar Depuração** para criar, implantar e executar o aplicativo.

Quando o aplicativo estiver em execução, você poderá alterar os valores do sensor de luz. Altere a luz disponível para o sensor ou use as ferramentas do emulador.

-   Pare o aplicativo. Basta retornar ao Visual Studio e pressionar Shift + F5 ou selecionar **Depurar** > **Parar Depuração** para parar o aplicativo.

###  Explicação

O exemplo anterior demonstra a codificação mínima que será necessária gravar para integrar a entrada do sensor de luz ao aplicativo.

O aplicativo estabelece uma conexão com o sensor padrão no método **BlankPage**.

```csharp
_lightsensor = LightSensor.GetDefault(); // Get the default light sensor object
```

O aplicativo estabelece o intervalo de relatório dentro do método **BlankPage**. Esse código recupera o intervalo mínimo com suporte do dispositivo e o compara com um intervalo solicitado de 16 milissegundos (aproximadamente a uma taxa de atualização de 60 Hz). Se o intervalo mínimo suportado for maior que o intervalo solicitado, o código definirá valor mínimo. Caso contrário, ele definirá o valor para o intervalo solicitado.

```csharp
uint minReportInterval = _lightsensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_lightsensor.ReportInterval = reportInterval;
```
Os novos dados do sensor de luz são capturados no método **ReadingChanged**. Toda vez que o driver do sensor recebe novos dados do sensor, ele transmite o valor para seu aplicativo usando este manipulador de eventos. O aplicativo registra este manipulador de eventos na seguinte linha.

```csharp
_lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, 
LightSensorReadingChangedEventArgs>(ReadingChanged);
```

Os novos valores são gravados em um TextBlock encontrado no XAML do projeto.

```xml
<TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
 <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>
```

## Tópicos relacionados

* [Exemplo de LightSensor](http://go.microsoft.com/fwlink/p/?linkid=241381)
 



<!--HONumber=May16_HO2-->


