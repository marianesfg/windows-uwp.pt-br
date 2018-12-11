---
ms.assetid: 1889AC3A-A472-4294-89B8-A642668A8A6E
title: Usar sensor de orientação
description: Saiba como usar os sensores de orientação para determinar a orientação do dispositivo.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 16d1ea6186cc8ccabacd1751db61e752a97930f7
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874404"
---
# <a name="use-the-orientation-sensor"></a>Usar o sensor de orientação


**APIs Importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371)
-   [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)

**Exemplos**

-   [Exemplo de sensor de orientação](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)
-   [Exemplo de sensor de orientação simples](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)

Saiba como usar os sensores de orientação para determinar a orientação do dispositivo.

Existem dois tipos diferentes de APIs de sensor de orientação no namespace [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408): [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) e [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399). Embora os dois sensores sejam sensores de orientação, esse termo é superestimado e eles são usados para fins bem diferentes. No entanto, como os dois são sensores de orientação, eles são abordados neste artigo.

A API do [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) é usada para que aplicativos 3D obtenham um quatérnion e uma matriz de rotação. Um quatérnion pode ser mais fácil de compreender do que um ponto de rotação \[x,y,z\] sobre um eixo arbitrário (em comparação com uma matriz de rotação que representa rotações em torno de três eixos). A matemática oculta nos quatérnions é um pouco exótica, já que envolve as propriedades geométricas de números complexos e as propriedades matemáticas de números imaginários, mas trabalhar com elas é simples e estruturas como DirectX têm suporte para elas. Um aplicativo 3-D complexo pode usar o Sensor de orientação para ajustar a perspectiva do usuário. Esse sensor combina a entrada do acelerômetro, do girômetro e da bússola.

A API do [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399) é usada para determinar a orientação do dispositivo atual em termos de definições como retrato para cima, retrato para baixo, paisagem à esquerda e paisagem à direita. Ela também pode detectar se um dispositivo está voltado para cima ou para baixo. Em vez de retornar propriedades como "portrait-up" ou "landscape left", este sensor retorna um valor de rotação: "Not rotated", "Rotated90DegreesCounterclockwise", etc. A tabela a seguir mapeia as propriedades comuns de orientação à leitura correspondente do sensor.

| Orientação     | Leitura correspondente do sensor      |
|-----------------|-----------------------------------|
| Portrait Up     | NotRotated                        |
| Landscape Left  | Rotated90DegreesCounterclockwise  |
| Portrait Down   | Rotated180DegreesCounterclockwise  |
| Landscape Right | Rotated270DegreesCounterclockwise  |

## <a name="prerequisites"></a>Pré-requisitos

Você deve estar familiarizado com Extensible Application Markup Language (XAML), Microsoft VisualC c# e eventos.

O dispositivo ou emulador que você estiver usando deve ser compatível com um sensor de orientação.

## <a name="create-an-orientationsensor-app"></a>Criar um aplicativo OrientationSensor

Esta seção está dividida em duas subseções. A primeira subseção levará você pelas etapas necessárias para criar um aplicativo de orientação do zero. A subseção seguintes explica o aplicativo que você acabou de criar.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private OrientationSensor _sensor;

            private async void ReadingChanged(object sender, OrientationSensorReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    OrientationSensorReading reading = e.Reading;

                    // Quaternion values
                    txtQuaternionX.Text = String.Format("{0,8:0.00000}", reading.Quaternion.X);
                    txtQuaternionY.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Y);
                    txtQuaternionZ.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Z);
                    txtQuaternionW.Text = String.Format("{0,8:0.00000}", reading.Quaternion.W);

                    // Rotation Matrix values
                    txtM11.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M11);
                    txtM12.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M12);
                    txtM13.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M13);
                    txtM21.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M21);
                    txtM22.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M22);
                    txtM23.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M23);
                    txtM31.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M31);
                    txtM32.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M32);
                    txtM33.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M33);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _sensor = OrientationSensor.GetDefault();

                // Establish the report interval for all scenarios
                uint minReportInterval = _sensor.MinimumReportInterval;
                uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                _sensor.ReportInterval = reportInterval;

                // Establish event handler
                _sensor.ReadingChanged += new TypedEventHandler<OrientationSensor, OrientationSensorReadingChangedEventArgs>(ReadingChanged);
            }
        }
    }
```

Você precisará substituir o nome do namespace no trecho anterior pelo nome que você deu a seu projeto. Por exemplo, se você criou um projeto denominado **OrientationSensorCS**, poderá substituir `namespace App1` por `namespace OrientationSensorCS`.

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

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,4,0,0" TextWrapping="Wrap" Text="M11:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,36,0,0" TextWrapping="Wrap" Text="M12:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,72,0,0" TextWrapping="Wrap" Text="M13:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="31" Margin="4,118,0,0" TextWrapping="Wrap" Text="M21:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,160,0,0" TextWrapping="Wrap" Text="M22:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,201,0,0" TextWrapping="Wrap" Text="M23:" VerticalAlignment="Top" Width="35"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,234,0,0" TextWrapping="Wrap" Text="M31:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,274,0,0" TextWrapping="Wrap" Text="M32:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="4,322,0,0" TextWrapping="Wrap" Text="M33:" VerticalAlignment="Top" Width="39"/>
            <TextBlock x:Name="txtM11" HorizontalAlignment="Left" Height="19" Margin="43,4,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM12" HorizontalAlignment="Left" Height="23" Margin="43,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM13" HorizontalAlignment="Left" Height="15" Margin="43,72,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM21" HorizontalAlignment="Left" Height="20" Margin="43,114,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM22" HorizontalAlignment="Left" Height="19" Margin="43,156,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM23" HorizontalAlignment="Left" Height="16" Margin="43,197,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM31" HorizontalAlignment="Left" Height="17" Margin="43,230,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM32" HorizontalAlignment="Left" Height="19" Margin="43,270,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM33" HorizontalAlignment="Left" Height="21" Margin="43,322,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,8,0,0" TextWrapping="Wrap" Text="Quaternion X:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="194,36,0,0" TextWrapping="Wrap" Text="Quaternion Y:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,72,0,0" TextWrapping="Wrap" Text="Quaternion Z:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionX" HorizontalAlignment="Left" Height="15" Margin="279,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="104"/>
            <TextBlock x:Name="txtQuaternionY" HorizontalAlignment="Left" Height="12" Margin="275,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="108"/>
            <TextBlock x:Name="txtQuaternionZ" HorizontalAlignment="Left" Height="19" Margin="275,68,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="89"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="194,96,0,0" TextWrapping="Wrap" Text="Quaternion W:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionW" HorizontalAlignment="Left" Height="12" Margin="279,96,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="72"/>

        </Grid>
    </Page>
```

Você precisará substituir a primeira parte do nome da classe no trecho anterior pelo namespace de seu aplicativo. Por exemplo, se você criou um projeto denominado **OrientationSensorCS**, poderá substituir `x:Class="App1.MainPage"` por `x:Class="OrientationSensorCS.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:OrientationSensorCS"`.

-   Pressione F5 ou selecione **Depurar** > **Iniciar Depuração** para criar, implantar e executar o aplicativo.

Quando o aplicativo estiver em execução, você poderá alterar a orientação. Basta mover o dispositivo ou usar as ferramentas do emulador.

-   Pare o aplicativo. Basta retornar ao Visual Studio e pressionar Shift + F5 ou selecionar **Depurar** > **Parar Depuração** para parar o aplicativo.

###  <a name="explanation"></a>Explicação

O exemplo anterior comprova que você precisará escrever pouco código para integrar a entrada do sensor de orientação ao seu aplicativo.

O aplicativo estabelece conexão com o sensor de orientação padrão no método **MainPage**.

```csharp
_sensor = OrientationSensor.GetDefault();
```

O aplicativo estabelece o intervalo de relatório **MainPage**. Esse código recupera o intervalo mínimo com suporte do dispositivo e o compara com um intervalo solicitado de 16 milissegundos (aproximadamente a uma taxa de atualização de 60 Hz). Se o intervalo mínimo suportado for maior que o intervalo solicitado, o código definirá valor mínimo. Do contrário, ele definirá o valor para o intervalo solicitado.

```csharp
uint minReportInterval = _sensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_sensor.ReportInterval = reportInterval;
```

Os novos dados do sensor são capturados no método **ReadingChanged**. Toda vez que o driver do sensor recebe novos dados do sensor, ele transmite o valor para seu aplicativo usando este manipulador de eventos. O aplicativo registra este manipulador de eventos na seguinte linha.

```csharp
_sensor.ReadingChanged += new TypedEventHandler<OrientationSensor,
OrientationSensorReadingChangedEventArgs>(ReadingChanged);
```

Esses novos valores são gravados nos TextBlocks encontrados no XAML do projeto.

## <a name="create-a-simpleorientation-app"></a>Criar um aplicativo SimpleOrientation

Esta seção está dividida em duas subseções. A primeira subseção guiará você pelas etapas necessárias para criar um aplicativo simples de orientação do zero. A subseção seguintes explica o aplicativo que você acabou de criar.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;
    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private SimpleOrientationSensor _simpleorientation;

            // This event handler writes the current sensor reading to
            // a text block on the app' s main page.

            private async void OrientationChanged(object sender, SimpleOrientationSensorOrientationChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    SimpleOrientation orientation = e.Orientation;
                    switch (orientation)
                    {
                        case SimpleOrientation.NotRotated:
                            txtOrientation.Text = "Not Rotated";
                            break;
                        case SimpleOrientation.Rotated90DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 90 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated180DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 180 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated270DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 270 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Faceup:
                            txtOrientation.Text = "Faceup";
                            break;
                        case SimpleOrientation.Facedown:
                            txtOrientation.Text = "Facedown";
                            break;
                        default:
                            txtOrientation.Text = "Unknown orientation";
                            break;
                    }
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _simpleorientation = SimpleOrientationSensor.GetDefault();

                // Assign an event handler for the sensor orientation-changed event
                if (_simpleorientation != null)
                {
                    _simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor, SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
                }
            }
        }
    }
```

Você precisará substituir o nome do namespace no trecho anterior pelo nome que você deu a seu projeto. Por exemplo, se você criou um projeto denominado **SimpleOrientationCS**, poderá substituir `namespace App1` por `namespace SimpleOrientationCS`.

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
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
            <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>

        </Grid>
    </Page>
```

Você precisará substituir a primeira parte do nome da classe no trecho anterior pelo namespace de seu aplicativo. Por exemplo, se você criou um projeto denominado **SimpleOrientationCS**, poderá substituir `x:Class="App1.MainPage"` por `x:Class="SimpleOrientationCS.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:SimpleOrientationCS"`.

-   Pressione F5 ou selecione **Depurar** > **Iniciar Depuração** para criar, implantar e executar o aplicativo.

Quando o aplicativo estiver em execução, você poderá alterar a orientação. Basta mover o dispositivo ou usar as ferramentas do emulador.

-   Pare o aplicativo. Basta retornar ao Visual Studio e pressionar Shift + F5 ou selecionar **Depurar** > **Parar Depuração** para parar o aplicativo.

### <a name="explanation"></a>Explicação

O exemplo anterior comprova que você precisará escrever pouco código para integrar a entrada do sensor simples de orientação ao seu aplicativo.

O aplicativo estabelece conexão com o sensor padrão no método **MainPage**.

```csharp
_simpleorientation = SimpleOrientationSensor.GetDefault();
```

Os novos dados do sensor são capturados no método **OrientationChanged**. Toda vez que o driver do sensor recebe novos dados do sensor, ele transmite o valor para seu aplicativo usando este manipulador de eventos. O aplicativo registra este manipulador de eventos na seguinte linha.

```csharp
_simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor,
SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
```

Os novos valores são gravados em um TextBlock encontrado no XAML do projeto.

```csharp
<TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
 <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>
```