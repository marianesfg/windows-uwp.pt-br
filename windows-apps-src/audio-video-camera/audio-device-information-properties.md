---
author: drewbatgit
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: Este artigo lista as propriedades de DeviceInformation relacionadas a dispositivos de áudio
title: Propriedades de informações do dispositivo de áudio
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c221e3d77419ca02b46e8be227f3b943fe8dc241
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
ms.locfileid: "1639006"
---
# <a name="audio-device-information-properties"></a>Propriedades de informações do dispositivo de áudio

Este artigo lista as propriedades de informações de dispositivo relacionadas a dispositivos de áudio. No Windows, cada dispositivo de hardware tem propriedades [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) associados que fornecem informações detalhadas sobre um dispositivo que você pode usar quando precisa de informações específicas sobre o dispositivo ou quando está criando um seletor de dispositivo. Para obter informações gerais sobre a enumeração de dispositivos no Windows, veja [Enumerar dispositivos](../devices-sensors/enumerate-devices.md) e [Propriedades de informações do dispositivo](../devices-sensors/device-information-properties.md).


|Nome|Tipo|Descrição|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Especifica a sensibilidade do microfone em decibéis em relação a unidades de escala completa (dBFS).|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|Double|Especifica o sinal de microfone para ruído (SNR) medido em unidades de decibéis (dB).|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Booleano|Indica se o dispositivo de áudio dá suporte para processamento de fala.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Booleano|Indica se o dispositivo de áudio dá suporte para processamento bruto.|
|**System.Devices.MicrophoneArray.Geometry**|caractere sem sinal[]|Dados de geometria para uma matriz de microfones.|

## <a name="related-topics"></a>Tópicos relacionados

* [Enumerar dispositivos](../devices-sensors/enumerate-devices.md)
* [Propriedades de informações do dispositivo](../devices-sensors/device-information-properties.md)
* [Criar um seletor de dispositivo](../devices-sensors/build-a-device-selector.md)
* [Reprodução de mídia](media-playback.md)




