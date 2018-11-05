---
author: drewbatgit
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: Este artigo mostra como acessar e usar a lâmpada do dispositivo, se houver uma. A funcionalidade da lâmpada é gerenciada separadamente da câmera e do flash da câmera do dispositivo.
title: Lanterna independente da câmera
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 913faf70531509a604cde52bb71886c128edae46
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6034272"
---
# <a name="camera-independent-flashlight"></a>Lanterna independente da câmera



Este artigo mostra como acessar e usar a lâmpada do dispositivo, se houver uma. A funcionalidade da lâmpada é gerenciada separadamente da câmera e do flash da câmera do dispositivo. Além de oferecer uma referência para a lâmpada e o ajuste de suas configurações, este artigo também mostra como liberar o recurso da lâmpada corretamente quando ela não estiver em uso e como detectar quando a disponibilidade da lâmpada muda caso esteja sendo usada por outro aplicativo.

## <a name="get-the-devices-default-lamp"></a>Obter a lâmpada padrão do dispositivo

Para obter o dispositivo de lâmpada padrão do dispositivo, chame [**Lamp.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/dn894327). As APIs da lâmpada são encontradas no namespace [**Windows.Devices.Lights**](https://msdn.microsoft.com/library/windows/apps/dn894331). Certifique-se de adicionar uma diretiva de uso para esse namespace antes de tentar acessar essas APIs.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

Se o objeto retornado for **nulo**, a API da **Lâmpada** não terá suporte no dispositivo. Alguns dispositivos podem não oferecer suporte à API da **Lâmpada** mesmo se houver uma lâmpada fisicamente presente no dispositivo.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>Obtenha uma lâmpada específica usando a cadeia de caracteres do seletor de lâmpada

Alguns dispositivos podem ter mais de uma lâmpada. Para obter uma lista de lâmpadas disponíveis no dispositivo, obtenha a cadeia de caracteres do seletor de dispositivo chamando [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894328). Essa cadeia de caracteres do seletor pode então ser passada para [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432). Esse método é usado para enumerar os diversos tipos de dispositivos, e a cadeia de caracteres do seletor informa ao método para retornar apenas os dispositivos de lâmpada. O objeto [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) retornado de **FindAllAsync** é uma coleção de objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) que representam as lâmpadas disponíveis no dispositivo. Selecione um dos objetos na lista e passe a propriedade [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) para [**Lamp.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894326) a fim de obter uma referência para a lâmpada solicitada. Este exemplo usa o método de extensão **GetFirstOrDefault** do namespace **System.Linq** para selecionar o objeto **DeviceInformation** em que a propriedade [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) tem um valor **Back**, que seleciona uma lâmpada que está na parte posterior do compartimento do dispostivo, caso haja uma.

Observe que as APIs [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) são encontradas no namespace [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459).

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>Ajustar as configurações da lâmpada

Quando você tiver uma instância da classe [**Lamp**](https://msdn.microsoft.com/library/windows/apps/dn894310), ative a lâmpada definindo a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) como **true**.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

Desativar a lâmpada definindo a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) como **false**.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

Alguns dispositivos têm lâmpadas que dão suporte a valores de cor. Verifique se uma lâmpada dá suporte a cor verificando a propriedade [**IsColorSettable**](https://msdn.microsoft.com/library/windows/apps/dn894329). Se esse valor for **true**, você poderá definir a cor da lâmpada com a propriedade [**Color**](https://msdn.microsoft.com/library/windows/apps/dn894322).

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>Registre-se para ser notificado se houver mudança na disponibilidade da lâmpada

O acesso à lâmpada é concedido ao aplicativo mais recente a solicitar acesso. Portanto, se outro aplicativo for iniciado e solicitar um recurso de lâmpada que seu aplicativo esteja usando no momento, seu aplicativo não poderá mais controlar a lâmpada até que o outro aplicativo tenha liberado o recurso. Para receber uma notificação quando a disponibilidade da lâmpada mudar, registre um manipulador para o evento [**Lamp.AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317).

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

No manipulador para o evento, verifique a propriedade [**LampAvailabilityChanged.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/dn894315) para determinar se a lâmpada está disponível. Neste exemplo, um botão de alternância para ativar e desativar a lâmpada está habilitado ou desabilitado com base na disponibilidade da lâmpada.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>Descartar corretamente o recurso da lâmpada quando não estiver em uso

Quando você não estiver mais usando a lâmpada, deverá desativá-la e chamar [**Lamp.Close**](https://msdn.microsoft.com/library/windows/apps/dn894320) para liberar o recurso e permitir que outros aplicativos acessem a lâmpada. Essa propriedade será mapeada para o método **Descarte** se você estiver usando C#. Se você se registrou para o [**AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317), deverá cancelar o registro do manipulador quando descartar o recurso de lâmpada. O lugar certo em seu código para descartar o recurso de lâmpada depende do seu aplicativo. Para analisar o acesso da lâmpada a uma única página, libere o recurso no evento [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509).

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>Tópicos relacionados
- [Reprodução de mídia](media-playback.md)

 




