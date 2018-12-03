---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: Emparelhar dispositivos
description: Alguns dispositivos precisam ser emparelhados antes de serem usados. O namespace Windows.Devices.Enumeration oferece suporte a três maneiras diferentes de emparelhar dispositivos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ccff9d892fbedc62cf1b54e374a0071877805731
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458437"
---
# <a name="pair-devices"></a>Emparelhar dispositivos



**APIs importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Alguns dispositivos precisam ser emparelhados antes de serem usados. O namespace [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) oferece suporte a três maneiras diferentes de emparelhar dispositivos.

-   Emparelhamento automático
-   Emparelhamento básico
-   Emparelhamento personalizado

**Dica**alguns dispositivos não precisam ser emparelhados para serem usados. Isso é abordado na seção sobre emparelhamento automático.

 

## <a name="automatic-pairing"></a>Emparelhamento automático


Às vezes você quer usar um dispositivo em seu aplicativo, mas não se preocupar com o fato de o dispositivo estar emparelhado ou não. Quer simplesmente poder usar a funcionalidade associada a um dispositivo. Por exemplo, se o seu aplicativo quer simplesmente capturar uma imagem de uma webcam, você não está necessariamente interessado no dispositivo em si, apenas a captura de imagem. Se houver APIs de dispositivos disponíveis para o dispositivo em que você está interessado, esse cenário se enquadrará no emparelhamento automático.

Nesse caso, você simplesmente usa as APIs associadas ao dispositivo, fazendo as chamadas conforme necessário e confiando no sistema para lidar com qualquer emparelhamento que possa ser necessário. Alguns dispositivos não precisam ser emparelhados para que você possa usar suas funcionalidades. Se o dispositivo realmente precisar ser emparelhado, as APIs do dispositivo manipularão a ação de emparelhamento em segundo plano, para que você não precise integrar essa funcionalidade ao seu aplicativo. Seu aplicativo não terá nenhum conhecimento sobre um determinado dispositivo estar emparelhado ou não, ou se precisa estar, mas você ainda poderá acessar o dispositivo e usar sua funcionalidade.

## <a name="basic-pairing"></a>Emparelhamento básico


Emparelhamento básico é quando seu aplicativo usa as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) para tentar emparelhar o dispositivo. Nessa situação, você permite que Windows tente realizar e manipule o processo de emparelhamento. Se for necessário alguma interação do usuário, ela será realizada pelo Windows. Você usaria o emparelhamento básico se precisasse emparelhar com um dispositivo e não houvesse uma API de dispositivo relevante que tentasse o emparelhamento automático. Você quer apenas poder usar o dispositivo e precisa emparelhá-lo primeiro.

Antes de tentar realizar o emparelhamento básico, você precisa obter o objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) para o dispositivo em que você está interessado. Assim que receber esse objeto, você vai interagir com a propriedade [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx), que é um objeto [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx). Para tentar emparelhar, basta chamar [**DeviceInformationPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/mt608800). Você precisará **esperar** o resultado para dar ao seu aplicativo tempo para tentar completar a ação de emparelhamento. O resultado da ação de emparelhamento será retornado e, desde que nenhum erro seja retornado, o dispositivo será emparelhado.

Se estiver usando o emparelhamento básico, você também tem acesso a informações adicionais sobre o status de emparelhamento do dispositivo. Por exemplo, você sabe o status de emparelhamento ([**IsPaired**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) e se o dispositivo pode emparelhar ([**CanPair**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair)). Ambas são propriedades do objeto [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx). Se estiver usando o emparelhamento automático, você talvez não tenha acesso a essas informações a menos que obtenha os objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) relevantes.

## <a name="custom-pairing"></a>Emparelhamento personalizado


O emparelhamento personalizado permite que seu aplicativo participe do processo de emparelhamento. Isso permite que seu aplicativo especifique os [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) que têm suporte para o processo de emparelhamento. Você também será responsável pela criação de sua própria interface do usuário para interagir com o usuário conforme a necessidade. Use o emparelhamento personalizado quando quiser que o aplicativo tenha um pouco mais de influência na maneira como o processo de emparelhamento ocorre ou para exibir sua própria interface de usuário de emparelhamento.

Para implementar o emparelhamento personalizado, você precisará obter o objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) para o dispositivo em que está interessado, exatamente como no emparelhamento básico. Contudo, a propriedade específica em que você está interessado é [**DeviceInformation.Pairing.Custom**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.custom.aspx). Isso lhe dará um objeto [**DeviceInformationCustomPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.aspx). Todos os métodos [**DeviceInformationCustomPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairasync.aspx) exigem que você inclua um parâmetro [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808). Isso indica as ações que o usuário precisará tomar para tentar emparelhar o dispositivo. Consulte a página de referência **DevicePairingKinds** para obter mais informações sobre os diferentes tipos e quais ações o usuário precisará tomar. Assim como com o emparelhamento básico, você precisará **esperar** o resultado para dar a seu aplicativo tempo para tentar completar a ação de emparelhamento. O resultado da ação de emparelhamento será retornado e, desde que nenhum erro seja retornado, o dispositivo será emparelhado.

Para dar suporte ao emparelhamento personalizado, você precisará criar um manipulador para o evento [**PairingRequested**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested.aspx). É preciso garantir que esse manipulador será levado em consideração para todos os diferentes [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) que podem ser usados em uma situação de emparelhamento personalizado. A ação adequada a ser tomada dependerá do **DevicePairingKinds** fornecido como parte dos argumentos de evento.

É importante estar ciente de que o emparelhamento personalizado é sempre uma operação a nível de sistema. Por isso, quando você estiver operando em um desktop ou Windows Phone, um diálogo de sistema sempre será exibido para o usuário quando o emparelhamento for acontecer. Isso ocorre porque ambas as plataformas possuem uma experiência de usuário que requer o consentimento do usuário. Como esse diálogo é gerado automaticamente, você não precisará criar seu próprio diálogo quando for optar por um [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) de **ConfirmOnly** quando estiver operando nessas plataformas. Para os outros **DevicePairingKinds**, você precisará realizar algumas interações especiais, dependendo do valor **DevicePairingKinds** específico. Veja os exemplos de amostra de como manipular o emparelhamento personalizado para valores **DevicePairingKinds**.

## <a name="unpairing"></a>Desemparelhando


Desemparelhar um dispositivo só é relevante nas situações de emparelhamento básico ou personalizado descritas acima. Se você estiver usando o emparelhamento automático, o aplicativo permanecerá indiferente ao status de emparelhamento do dispositivo e não será necessário desemparelhá-lo. Se você optar por desemparelhar um dispositivo, o processo será idêntico se você implementar o emparelhamento básico ou personalizado. Isso porque não é preciso fornecer informações adicionais ou interagir no processo de desemparelhamento.

O primeiro passo para desemparelhar um dispositivo é obter o objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) para o dispositivo que você quer desemparelhar. Em seguida, você precisa recuperar a propriedade [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) e chamar [**DeviceInformationPairing.UnpairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.unpairasync). Assim como com o emparelhamento, é bom **esperar** o resultado. O resultado da ação de desemparelhamento será retornado e, desde que nenhum erro seja retornado, o dispositivo será desemparelhado.

## <a name="sample"></a>Amostra


Para baixar uma amostra de como usar as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), clique [aqui](http://go.microsoft.com/fwlink/?LinkID=620536).

 

 
