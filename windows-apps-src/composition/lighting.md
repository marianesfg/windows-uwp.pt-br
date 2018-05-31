---
author: jebishop
ms.assetid: fb8ae71d-5c88-4c85-9257-a9607d5179b1
title: iluminação
description: Os objetos de luz são usados em conjunto com SceneLightingEffect para simular iluminação e refletividade dinâmicas.
ms.author: jimwalk
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 316b79fa9fc9a700d606f0881a89a299523330b1
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673703"
---
# <a name="lighting"></a>Iluminação

Os objetos [**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) são usados em conjunto com [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) para simular iluminação e refletividade dinâmicas.

Você pode aplicar luzes a [**Visuals**](https://msdn.microsoft.com/library/windows/apps/Dn706858) e [**UIElements**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) XAML.

## <a name="applying-lights-to-xaml-uielements"></a>Aplicação de luzes a UIElements XAML

Os objetos [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) são usados para aplicar [**CompositionLights**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar dinamicamente UIElements XAML. XamlLight fornece métodos para direcionar UIElements ou outros Pincéis XAML, aplicando luzes a árvores de UIElements e ajudando a gerenciar a vida útil de recursos CompositionLight, estando eles em uso ou não.

* Se você tiver um **Brush** como destino de uma XamlLight, então as partes de quaisquer UIElements que usem esse Brush serão iluminadas.
* Se você tiver um **UIElement** como destino de uma XamlLight, então o UIElement inteiro e seus UIElements filhos serão iluminados.

## <a name="creating-and-using-a-xamllight"></a>Criação e uso de uma XamlLight

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) é uma classe base que pode ser usada para criar luzes personalizadas.

Aqui está um exemplo de uma XamlLight personalizada que usa uma propriedade anexada para ter como destino elementos específicos:

```csharp
public sealed class FancyOrangeSpotLight : XamlLight
{
    // Register an attached proeprty that enables apps to set a UIElement or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(Boolean),
        typeof(FancyOrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );
    public static void SetIsTarget(DependencyObject target, Boolean value)
    {
        target.SetValue(IsTargetProperty, value);
    }
    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (Boolean) target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj,
                                            DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (Boolean)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        var spotLight = Window.Current.Compositor.CreateSpotLight();
        spotLight.InnerConeColor = Colors.Orange;
        spotLight.OuterConeColor = Colors.Yellow;
        spotLight.InnerConeAngleInDegrees = 30;
        spotLight.OuterConeAngleInDegrees = 45;
        CompositionLight = spotLight;
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen. The CompositionLight should be disposed when no longer required.
        CompositionLight.Dispose();
        CompositionLight = null;
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return typeof(FancyPointerTrackerLight).FullName;
    }
}
```

E aqui está um exemplo que mostra possíveis usos diferentes da luz personalizada definida acima:

```xml
<Page>
    <Page.Lights>
        <local:SimpleOrangeSpotLight />
    </Page.Lights>

    <StackPanel>
        <!-- this border will be lit by a FancyOrangeSpotLight, but not its children -->
        <Border BorderThickness="1">
            <Border.BorderBrush>
                <SolidColorBrush Color="Orange" local:FancyOrangeSpotLight.IsTarget="true" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border and its children will be lit by FancyOrangeSpotLight -->
        <Border BorderThickness="2" local:FancyOrangeSpotLight.IsTarget="true">
            <Border.BorderBrush>
                <SolidColorBrush Color="Purple" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border will not be lit -->
        <Border BorderThickness="2">
            <Border.BorderBrush>
                <SolidColorBrush Color="Green" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>
    </StackPanel>
<Page>
```

> [!Important]
> A configuração de UIElement.Lights em marcação, como mostrado no exemplo acima, só tem suporte para apps com uma Versão Mínima igual à Atualização do Windows 10 para Criadores. Para os apps que têm como destino as versões anteriores, as luzes devem ser criadas em code-behind.

## <a name="additional-resources"></a>Recursos adicionais

* Exemplos de Interface do usuário avançada e composição no [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs).
