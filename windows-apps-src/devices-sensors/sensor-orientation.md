---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Orientação do sensor
description: Os dados do sensor das classes Accelerometer, Gyrometer, Compass, Inclinometer e OrientationSensor são definidos por seus eixos de referência. Esses eixos são definidos pela orientação paisagem do dispositivo e giram com o dispositivo conforme o usuário o vira.
ms.date: 05/24/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bfd84cd2f2255138b738ecb6dd7f6dab824d7ec4
ms.sourcegitcommit: d1ef530ef4dfa34db7bc429ab5a0c19fc405885f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71247449"
---
# <a name="sensor-orientation"></a>Orientação do sensor

Os dados do sensor das classes [**Accelerometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer), [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer), [**Compass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Compass), [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) e [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) são definidos por seus eixos de referência. Esses eixos são definidos pelo quadro de referência do dispositivo e giram com o dispositivo conforme o usuário o vira. Caso seu aplicativo dê suporte à rotação automática e se reoriente para acomodar o dispositivo conforme o usuário o gira, você deve ajustar os dados do sensor para a rotação antes de usá-lo.

### <a name="important-apis"></a>APIs Importantes

- [**Windows. Devices. sensores**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
- [**Windows. Devices. sensores. Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>Orientação de exibição x orientação do dispositivo

Para poder compreender os eixos de referência para os sensores, você precisa distinguir entre a orientação de exibição e a orientação do dispositivo. A orientação de exibição é a direção na qual textos e imagens são exibidas na tela, enquanto que a orientação do dispositivo é a posição física do dispositivo.

Nos diagramas a seguir, a orientação de exibição e o dispositivo estão em [paisagem](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) (os eixos de sensor mostrados são específicos para a orientação paisagem), com o eixo z positivo estendido do dispositivo.

Esse diagrama mostra a orientação de exibição e de dispositivo em [paisagem](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations).

![Orientação de exibição e do dispositivo em Paisagem](images/sensor-orientation-a.PNG)

Este próximo diagrama mostra a exibição e a orientação do dispositivo no [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations).

![Orientação de exibição e do dispositivo em LandscapeFlipped](images/sensor-orientation-b.PNG)

Este último diagrama mostra a orientação de exibição em paisagem enquanto a orientação do dispositivo é [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations).

![Orientação de exibição em Paisagem, enquanto que a orientação do dispositivo é LandscapeFlipped.](images/sensor-orientation-c.PNG)

Você pode consultar os valores de orientação por meio da classe [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) usando o método [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) com a propriedade [**CurrentOrientation**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.currentorientation). Em seguida, você pode criar lógica comparando com a enumeração [**DisplayOrientations**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations). Lembre-se de que para cada orientação com suporte, é necessário dar suporte a uma conversão dos eixos de referência para essa orientação.

## <a name="landscape-first-vs-portrait-first-devices"></a>Dispositivos de prioridade paisagem x prioridade retrato

Os fabricantes produzem dispositivos com ambos os formatos de prioridade paisagem e prioridade retrato. O quadro de referência varia entre dispositivos de prioridade paisagem (como desktops e notebooks) e dispositivos de prioridade retrato (por exemplo, alguns telefones e tablets). A tabela a seguir mostra os eixos do sensor para ambos os dispositivos prioridade paisagem e prioridade retrato.

| Orientação | Prioridade paisagem | Prioridade retrato |
|-------------|-----------------|----------------|
| **Ambiente** | ![Dispositivo de prioridade paisagem em orientação Paisagem](images/sensor-orientation-0.PNG) | ![Dispositivo de prioridade retrato em orientação Paisagem](images/sensor-orientation-1.PNG) |
| **Orientações** | ![Dispositivo de prioridade paisagem em orientação Retrato](images/sensor-orientation-2.PNG) | ![Dispositivo de prioridade retrato em orientação Retrato](images/sensor-orientation-3.PNG) |
| **LandscapeFlipped** | ![Dispositivo de prioridade paisagem em orientação LandscapeFlipped](images/sensor-orientation-4.PNG) | ![Dispositivo de prioridade retrato em orientação LandscapeFlipped](images/sensor-orientation-5.PNG) | 
| **PortraitFlipped** | ![Dispositivo de prioridade paisagem em orientação PortraitFlipped](images/sensor-orientation-6.PNG)| ![Dispositivo de prioridade retrato em orientação PortraitFlipped](images/sensor-orientation-7.PNG) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>Dispositivos com transmissão de exibição e dispositivos sem periféricos

Alguns dispositivos têm a capacidade de transmissão da exibição para outro dispositivo. Por exemplo, você pode pegar um tablet e transmitir a exibição para um projetor que estará na orientação paisagem. Nesse caso, é importante ter em mente que a orientação do dispositivo é baseada no dispositivo original e não naquele em que a tela é apresentada. Portanto, um acelerômetro informaria dados para o tablet.

Além disso, alguns dispositivos não têm uma exibição. Com esses dispositivos, a orientação padrão é retrato.

## <a name="display-orientation-and-compass-heading"></a>Orientação de exibição e direcionamento da bússola

O direcionamento da bússola depende dos eixos de referência e, portanto, muda com a orientação do dispositivo. Você compensa com base nesta tabela (pressuponha que o usuário esteja voltado para o norte).

| Orientação de exibição | Eixo de referência para direcionamento da bússola | Direcionamento da bússola da API quando voltado para o norte (paisagem primeiro) | Direcionamento da bússola da API quando voltado para o norte (retrato primeiro) |Compensação do direcionamento da bússola (paisagem primeiro) | Compensação do direcionamento da bússola (retrato primeiro) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Paisagem           | -Z | 0   | 270 | Direcionamento               | (Direcionamento + 90) % 360  |
| Retrato            |  S | 90  | 0   | (Direcionamento + 270) % 360 |  Direcionamento              |
| LandscapeFlipped    |  Z | 180 | 90  | (Direcionamento + 180) % 360 | (Direcionamento + 270) % 360 |
| PortraitFlipped     |  S | 270 | 180 | (Direcionamento + 90) % 360  | (Direcionamento + 180) % 360 |

Modifique o direcionamento da bússola conforme mostrado na tabela a fim de exibir o direcionamento corretamente. O trecho de código a seguir demonstra como fazer isso.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>Orientação de exibição com o acelerômetro e o girômetro

Esta tabela converte os dados do acelerômetro e do girômetro para orientação de exibição.

| Eixos de referência        |  X |  S | Z |
|-----------------------|----|----|---|
| **Ambiente**         |  X |  S | Z |
| **Orientações**          |  S | -X | Z |
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

O exemplo de código a seguir aplica essas conversões ao girômetro.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>Orientação de exibição e do dispositivo

Os dados de [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) devem ser alterados de maneira diferente. Imagine essas diferentes orientações como rotações no sentido anti-horário para o eixo Z, portanto, precisamos reverter a rotação para obter a orientação do usuário. Para dados quatérnion, podemos usar a fórmula de Euler para definir uma rotação com um quatérnion de referência e também podemos usar uma matriz de rotação de referência.

![Fórmula de Euler](images/eulers-formula.png)

Para obter a orientação relativa desejada, multiplique o objeto de referência pelo objeto absoluto. Observe que esta matemática não é comutativa.

![Multiplique o objeto de referência pelo objeto absoluto.](images/orientation-formula.png)

Na expressão anterior, o objeto absoluto é retornado pelos dados do sensor.

| Orientação de exibição  | Rotação no sentido anti-horário em torno de Z | Quatérnion de referência (rotação invertida) | Matriz de rotação de referência (rotação invertida) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Ambiente**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Orientações**         | 90                                 | cos(-45⁰) + (i + j + k)*sen(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sen(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |
