---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: Este artigo mostra como acessar e usar a lâmpada do dispositivo, se houver uma. A funcionalidade da lâmpada é gerenciada separadamente da funcionalidade da câmera e do flash da câmera do dispositivo.
title: Lanterna independente da câmera
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ddc7ad87a883c3512c719167975428b6c7746031
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359056"
---
# <a name="camera-independent-flashlight"></a>Lanterna independente da câmera



Este artigo mostra como acessar e usar a lâmpada do dispositivo, se houver uma. A funcionalidade da lâmpada é gerenciada separadamente da funcionalidade da câmera e do flash da câmera do dispositivo. Além de oferecer uma referência para a lâmpada e o ajuste de suas configurações, este artigo também mostra como liberar o recurso da lâmpada corretamente quando ela não estiver em uso e como detectar quando a disponibilidade da lâmpada muda caso esteja sendo usada por outro aplicativo.

## <a name="get-the-devices-default-lamp"></a>Obter a lâmpada padrão do dispositivo

Para obter o dispositivo de lâmpada padrão do dispositivo, chame [**Lamp.GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.getdefaultasync). As APIs da lâmpada são encontradas no namespace [**Windows.Devices.Lights**](https://docs.microsoft.com/uwp/api/Windows.Devices.Lights). Certifique-se de adicionar uma diretiva de uso para esse namespace antes de tentar acessar essas APIs.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

Se o objeto retornado for **nulo**, a API da **Lâmpada** não terá suporte no dispositivo. Alguns dispositivos podem não oferecer suporte à API da **Lâmpada** mesmo se houver uma lâmpada fisicamente presente no dispositivo.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>Obtenha uma lâmpada específica usando a cadeia de caracteres do seletor de lâmpada

Alguns dispositivos podem ter mais de uma lâmpada. Para obter uma lista de lâmpadas disponíveis no dispositivo, obtenha a cadeia de caracteres do seletor de dispositivo chamando [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.getdeviceselector). Essa cadeia de caracteres do seletor pode então ser passada para [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync). Esse método é usado para enumerar os diversos tipos de dispositivos, e a cadeia de caracteres do seletor informa ao método para retornar apenas os dispositivos de lâmpada. O objeto [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) retornado de **FindAllAsync** é uma coleção de objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) que representam as lâmpadas disponíveis no dispositivo. Selecione um dos objetos na lista e passe a propriedade [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) para [**Lamp.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.fromidasync) a fim de obter uma referência para a lâmpada solicitada. Este exemplo usa o método de extensão **GetFirstOrDefault** do namespace **System.Linq** para selecionar o objeto **DeviceInformation** em que a propriedade [**EnclosureLocation.Panel**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.panel) tem um valor **Back**, que seleciona uma lâmpada que está na parte posterior do compartimento do dispostivo, caso haja uma.

Observe que as APIs [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) são encontradas no namespace [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration).

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>Ajustar as configurações da lâmpada

Quando você tiver uma instância da classe [**Lamp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Lights.Lamp), ative a lâmpada definindo a propriedade [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.isenabled) como **true**.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

Desativar a lâmpada definindo a propriedade [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.isenabled) como **false**.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

Alguns dispositivos têm lâmpadas que dão suporte a valores de cor. Verifique se uma lâmpada dá suporte a cor verificando a propriedade [**IsColorSettable**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.iscolorsettable). Se esse valor for **true**, você poderá definir a cor da lâmpada com a propriedade [**Color**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.color).

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>Registre-se para ser notificado se houver mudança na disponibilidade da lâmpada

O acesso à lâmpada é concedido ao aplicativo mais recente a solicitar acesso. Portanto, se outro aplicativo for iniciado e solicitar um recurso de lâmpada que seu aplicativo esteja usando no momento, seu aplicativo não poderá mais controlar a lâmpada até que o outro aplicativo tenha liberado o recurso. Para receber uma notificação quando a disponibilidade da lâmpada mudar, registre um manipulador para o evento [**Lamp.AvailabilityChanged**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.availabilitychanged).

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

No manipulador para o evento, verifique a propriedade [**LampAvailabilityChanged.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable) para determinar se a lâmpada está disponível. Neste exemplo, um botão de alternância para ativar e desativar a lâmpada está habilitado ou desabilitado com base na disponibilidade da lâmpada.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>Descartar corretamente o recurso da lâmpada quando não estiver em uso

Quando você não estiver mais usando a lâmpada, deverá desativá-la e chamar [**Lamp.Close**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.close) para liberar o recurso e permitir que outros aplicativos acessem a lâmpada. Essa propriedade será mapeada para o método **Descarte** se você estiver usando C#. Se você se registrou para o [**AvailabilityChanged**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.availabilitychanged), deverá cancelar o registro do manipulador quando descartar o recurso de lâmpada. O lugar certo em seu código para descartar o recurso de lâmpada depende do seu aplicativo. Para analisar o acesso da lâmpada a uma única página, libere o recurso no evento [**OnNavigatingFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom).

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>Tópicos relacionados
- [Reprodução de mídia](media-playback.md)

 




