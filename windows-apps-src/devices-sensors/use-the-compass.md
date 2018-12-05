---
ms.assetid: 5B30E32F-27E0-4656-A834-391A559AC8BC
title: Usar a bússola
description: Saiba como usar a bússola para determinar a direção atual.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8c1cc6e17d95f55cc97af7695c12b374edcaaa8
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8700428"
---
# <a name="use-the-compass"></a>Usar a bússola


**APIs Importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Bússola**](https://msdn.microsoft.com/library/windows/apps/BR225705)

**Exemplo**

-   Para obter uma implementação completa, consulte o [exemplo de bússola](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass).

Saiba como usar a bússola para determinar a direção atual.

Um aplicativo pode recuperar a direção atual com relação ao norte magnético ou verdadeiro. Os aplicativos de navegação usam a bússola para determinar a direção para qual um dispositivo está voltado e orientar o mapa de acordo.

## <a name="prerequisites"></a>Pré-requisitos

Você deve estar familiarizado com Extensible Application Markup Language (XAML), Microsoft VisualC c# e eventos.

O dispositivo ou emulador que você está usando deve ter suporte para uma bússola.

## <a name="create-a-simple-compass-app"></a>Criar um aplicativo simples de bússola

Esta seção está dividida em duas subseções. A primeira subseção guiará você pelas etapas necessárias para criar um aplicativo simples de bússola do zero. A subseção seguintes explica o aplicativo que você acabou de criar.

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

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the compass


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Compass _compass; // Our app' s compass object

            // This event handler writes the current compass reading to
            // the textblocks on the app' s main page.

            private async void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
            {
               await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    CompassReading reading = e.Reading;
                    txtMagnetic.Text = String.Format("{0,5:0.00}", reading.HeadingMagneticNorth);
                    if (reading.HeadingTrueNorth.HasValue)
                        txtNorth.Text = String.Format("{0,5:0.00}", reading.HeadingTrueNorth);
                    else
                        txtNorth.Text = "No reading.";
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
               _compass = Compass.GetDefault(); // Get the default compass object

                // Assign an event handler for the compass reading-changed event
                if (_compass != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _compass.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _compass.ReportInterval = reportInterval;
                    _compass.ReadingChanged += new TypedEventHandler<Compass, CompassReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
    ```

You'll need to rename the namespace in the previous snippet with the name you gave your project. For example, if you created a project named **CompassCS**, you'd replace `namespace App1` with `namespace CompassCS`.

-   Open the file MainPage.xaml and replace the original contents with the following XML.

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
            <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
            <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
            <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>

         </Grid>
    </Page>
```

Você precisará substituir a primeira parte do nome da classe no trecho anterior pelo namespace de seu aplicativo. Por exemplo, se você criou um projeto denominado **CompassCS**, pode substituir `x:Class="App1.MainPage"` por `x:Class="CompassCS.MainPage"`. Você também deve substituir `xmlns:local="using:App1"` por `xmlns:local="using:CompassCS"`.

-   Pressione F5 ou selecione **Depurar** > **Iniciar Depuração** para criar, implantar e executar o aplicativo.

Quando o aplicativo estiver em execução, você poderá alterar os valores da bússola. Basta mover o dispositivo ou usar as ferramentas do emulador.

-   Pare o aplicativo. Basta retornar ao Visual Studio e pressionar Shift + F5 ou selecionar **Depurar** > **Parar Depuração** para parar o aplicativo.

### <a name="explanation"></a>Explicação

O exemplo anterior comprova que você precisará escrever pouco código para integrar a entrada da bússola ao seu aplicativo.

O aplicativo estabelece uma conexão com a bússola padrão no método **MainPage**.

```csharp
_compass = Compass.GetDefault(); // Get the default compass object
```

O aplicativo estabelece o intervalo de relatório **MainPage**. Esse código recupera o intervalo mínimo com suporte do dispositivo e o compara com um intervalo solicitado de 16 milissegundos (aproximadamente a uma taxa de atualização de 60 Hz). Se o intervalo mínimo suportado for maior que o intervalo solicitado, o código definirá valor mínimo. Caso contrário, ele definirá o valor para o intervalo solicitado.

```csharp
uint minReportInterval = _compass.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_compass.ReportInterval = reportInterval;
```

Os novos dados da bússola são capturados no método **ReadingChanged**. Toda vez que o driver do sensor recebe novos dados do sensor, ele transmite o valor para seu aplicativo usando este manipulador de eventos. O aplicativo registra este manipulador de eventos na seguinte linha.

```csharp
_compass.ReadingChanged += new TypedEventHandler<Compass,
CompassReadingChangedEventArgs>(ReadingChanged);
```

Esses novos valores são gravados nos TextBlocks encontrados no XAML do projeto.

```xml
 <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
 <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
 <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>
```
 

 
