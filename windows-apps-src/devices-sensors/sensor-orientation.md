---
author: DBirtolo
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: "Orientação do sensor"
description: "Os dados do sensor das classes Accelerometer, Gyrometer, Compass, Inclinometer e OrientationSensor são definidos por seus eixos de referência. Esses eixos são definidos pela orientação paisagem do dispositivo e giram com o dispositivo conforme o usuário o vira."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3da7957bf9b162f1ac1533ccff90c8764f34890a

---
# Orientação do sensor

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** APIs importantes **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Os dados do sensor das classes [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687), [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718), [**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705), [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) e [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) são definidos por seus eixos de referência. Esses eixos são definidos pela orientação paisagem do dispositivo e giram com o dispositivo conforme o usuário o vira. Caso seu aplicativo dê suporte à rotação automática e se reoriente para acomodar o dispositivo conforme o usuário o gira, você deve ajustar os dados do sensor para a rotação antes de usá-lo.

## Orientação de exibição x orientação do dispositivo

Para poder compreender os eixos de referência para os sensores, você precisa distinguir entre a orientação de exibição e a orientação do dispositivo. A orientação de exibição é a direção na qual textos e imagens são exibidas na tela, enquanto que a orientação do dispositivo é a posição física do dispositivo. Na imagem a seguir, ambas as orientações de exibição e do dispositivo estão em **Landscape**.

![Orientação de exibição e do dispositivo em Paisagem](images/accelerometer-axis-orientation-landscape-with-text.png)

A imagem a seguir mostra ambas as orientações de exibição e dispositivo em **LandscapeFlipped**.

![Orientação de exibição e do dispositivo em LandscapeFlipped](images/accelerometer-axis-orientation-landscape-180-with-text.png)

A próxima imagem exibe a orientação de exibição em Paisagem, enquanto que a orientação do dispositivo é LandscapeFlipped.

![Orientação de exibição em Paisagem, enquanto que a orientação do dispositivo é LandscapeFlipped.](images/accelerometer-axis-orientation-landscape-180-with-text-inverted.png)

Você pode consultar os valores de orientação por meio da classe [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258) usando o método [**GetForCurrentView**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx) com a propriedade [**CurrentOrientation**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx). Em seguida, você pode criar lógica comparando com a enumeração [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142). Lembre-se de que para cada orientação com suporte, é necessário dar suporte a uma conversão dos eixos de referência para essa orientação.

## Dispositivos de prioridade paisagem x prioridade retrato

Os fabricantes produzem dispositivos com ambos os formatos de prioridade paisagem e prioridade retrato. Quando o fabricante integra os componentes nos dispositivos, eles o fazem de uma maneira unificada e consistente para que todos os dispositivos operem dentro do mesmo enquadramento de referência. A tabela a seguir mostra os eixos do sensor para ambos os dispositivos prioridade paisagem e prioridade retrato.

| Orientação | Prioridade paisagem | Prioridade retrato |
|-------------|-----------------|----------------|
| **Paisagem** | ![Dispositivo de prioridade paisagem em orientação Paisagem](images/accelerometer-axis-orientation-landscape.png) | ![Dispositivo de prioridade retrato em orientação Paisagem](images/accelerometer-axis-orientation-portrait-270.png) |
| **Retrato** | ![Dispositivo de prioridade paisagem em orientação Retrato](images/accelerometer-axis-orientation-landscape-90.png) | ![Dispositivo de prioridade retrato em orientação Retrato](images/accelerometer-axis-orientation-portrait.png) |
| **LandscapeFlipped ** | ![Dispositivo de prioridade paisagem em orientação LandscapeFlipped](images/accelerometer-axis-orientation-landscape-180.png) | ![Dispositivo de prioridade retrato em orientação LandscapeFlipped](images/accelerometer-axis-orientation-portrait-90.png) | 
| **PortraitFlipped** | ![Dispositivo de prioridade paisagem em orientação PortraitFlipped](images/accelerometer-axis-orientation-landscape-270.png)| ![Dispositivo de prioridade retrato em orientação PortraitFlipped](images/accelerometer-axis-orientation-portrait-180.png) |

## Dispositivos com transmissão de exibição e dispositivos sem periféricos

Alguns dispositivos têm a capacidade de transmissão da exibição para outro dispositivo. Por exemplo, você pode pegar um tablet e transmitir a exibição para um projetor que estará na orientação paisagem. Nesse caso, é importante ter em mente que a orientação do dispositivo é baseada no dispositivo original e não naquele em que a tela é apresentada. Portanto, um acelerômetro informaria dados para o tablet.

Além disso, alguns dispositivos não têm uma exibição. Com esses dispositivos, a orientação padrão é retrato.

## Orientação de exibição e direcionamento da bússola


O direcionamento da bússola depende dos eixos de referência e, portanto, muda com a orientação do dispositivo. Você compensa com base nesta tabela (pressuponha que o usuário esteja voltado para o norte).

| Orientação de exibição | Eixo de referência para direcionamento da bússola | Direcionamento da bússola da API quando voltado para o norte | Compensação do direcionamento da bússola | 
|---------------------|------------------------------------|---------------------------------------|------------------------------|
| Paisagem           | -Z | 0   | Direcionamento               |
| Retrato            |  Y | 90  | (Direcionamento + 270) % 360 | 
| LandscapeFlipped    |  Z | 180 | (Direcionamento + 180) % 360 |
| PortraitFlipped     |  Y | 270 | (Direcionamento + 90) % 360  |

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

## Orientação de exibição com o acelerômetro e o girômetro

Esta tabela converte os dados do acelerômetro e do girômetro para orientação de exibição.

| Eixos de referência        |  X |  Y | Z |
|-----------------------|----|----|---|
| **Paisagem**         |  X |  Y | Z |
| **Retrato**          |  Y | -X | Z |
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

## Orientação de exibição e do dispositivo

Os dados de [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) devem ser alterados de maneira diferente. Pense nessas diferentes orientações como rotações no sentido anti-horário para o eixo Z, portanto, precisamos inverter a rotação para obter a orientação do usuário de volta. Para dados quatérnion, podemos usar a fórmula de Euler para definir uma rotação com um quatérnion de referência e também podemos usar uma matriz de rotação de referência.

![Fórmula de Euler](images/eulers-formula.png) Para obter a orientação relativa desejada, multiplique o objeto de referência pelo objeto absoluto. Observe que esta matemática não é comutativa.

![Multiplique o objeto de referência pelo objeto absoluto](images/orientation-formula.png) Na expressão anterior, o objeto absoluto é retornado pelos dados do sensor.

| Orientação de exibição  | Rotação no sentido anti-horário em torno de Z | Quatérnion de referência (rotação invertida) | Matriz de rotação de referência (rotação invertida) | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Paisagem**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Retrato**         | 90                                 | cos(-45⁰) + (i + j + k)*sen(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sen(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |




<!--HONumber=Jun16_HO4-->


