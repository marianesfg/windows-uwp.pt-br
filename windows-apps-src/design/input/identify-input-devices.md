---
author: Karl-Bridge-Microsoft
Description: Identify the input devices connected to a Universal Windows Platform (UWP) device and identify their capabilities and attributes.
title: Identificar dispositivos de entrada
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: dispositivo, digitalizador, entrada, interação
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 77d2a914147fb2cd20f8043e0b52a824ddf3710d
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7169212"
---
# <a name="identify-input-devices"></a>Identificar dispositivos de entrada


Identifique os dispositivos de entrada conectados a um dispositivo da Plataforma Universal do Windows (UWP) e os seus recursos e atributos.

> **APIs importantes**: [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

## <a name="retrieve-mouse-properties"></a>Recuperar as propriedades do mouse


O namespace [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contém a classe [**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626) usada para recuperar as propriedades expostas por um ou mais mouses conectados. Crie um novo objeto **MouseCapabilities** e obtenha as propriedades que interessam a você.

**Observação**os valores retornados pelas propriedades abordadas aqui são baseados em todos os mouses detectados: as propriedades Boolianas retornam não zero se pelo menos um mouse der suporte a um recurso específico e as propriedades numéricas retornarem o valor máximo exposto por um mouse.

 

O código a seguir usa uma série de elementos [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para exibir as propriedades e valores do mouse específico.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <a name="retrieve-keyboard-properties"></a>Recuperar propriedades do teclado


O namespace [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contém a classe [**KeyboardCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225623) usada para recuperar se um teclado estiver conectado. Basta criar um novo objeto **KeyboardCapabilities** e obter a propriedade [**KeyboardPresent**](https://msdn.microsoft.com/library/windows/apps/br225625).

O código a seguir usa um elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para exibir o valor e a propriedade do teclado.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>Recuperar as propriedades de toque


O namespace [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contém a classe [**TouchCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225644) usada para recuperar se um digitalizador de toque estiver conectado. Basta criar um novo objeto **TouchCapabilities** e obter as propriedades que interessam a você.

**Observação**os valores retornados pelas propriedades abordadas aqui são baseados em todos os digitalizadores detectados: as propriedades Boolianas retornam não zero se pelo menos um digitalizador der suporte a um recurso específico e as propriedades numéricas retornarem o valor máximo exposto por um digitalizador.

 

O código a seguir usa uma série de elementos [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para exibir as propriedades de toque e valores.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>Recuperar as propriedades de ponteiro


O namespace [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contém a classe [**PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633) usada para recuperar se um dispositivo detectado der suporte à entrada de ponteiro (toque, touchpad, mouse ou caneta). Basta criar um novo objeto **PointerDevice** e obter as propriedades que interessam a você.

**Observação**os valores retornados pelas propriedades abordadas aqui são baseados em todos os dispositivos de ponteiro detectados: as propriedades Boolianas retornam não zero se pelo menos um dispositivo dá suporte a um recurso específico e as propriedades numéricas retornarem o valor máximo exposto por qualquer dispositivo de ponteiro.

O código a seguir usa uma tabela para exibir as propriedades e valores de cada dispositivo de ponteiro.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <a name="related-articles"></a>Artigos relacionados


**Exemplos**
* [Amostra de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemplo do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)

**Exemplos de arquivo-morto**
* [Entrada: exemplo de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
 

 




