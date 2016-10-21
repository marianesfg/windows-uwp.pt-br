---
author: drewbatgit
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: "Este artigo lista as propriedades de DeviceInformation relacionadas a dispositivos de áudio"
title: "Propriedades de informações do dispositivo de áudio"
translationtype: Human Translation
ms.sourcegitcommit: 0745e96715ba49582ab762d4b25f1b8e681116f5
ms.openlocfilehash: 08ebb37679d1dd93458a3ffe846d8bd33574635d

---

# Propriedades de informações do dispositivo de áudio

Este artigo lista as propriedades de informações de dispositivo relacionadas a dispositivos de áudio. No Windows, cada dispositivo de hardware tem propriedades [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) associados que fornecem informações detalhadas sobre um dispositivo que você pode usar quando precisa de informações específicas sobre o dispositivo ou quando está criando um seletor de dispositivo. Para obter informações gerais sobre a enumeração de dispositivos no Windows, veja [Enumerar dispositivos](../devices-sensors/enumerate-devices.md) e [Propriedades de informações do dispositivo](../devices-sensors/device-information-properties.md).


|Nome|Tipo|Descrição|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Especifica a sensibilidade do microfone em decibéis em relação a unidades de escala completa (dBFS).|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRationInDb**|Double|Especifica o sinal de microfone para ruído (SNR) medido em unidades de decibéis (dB).|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Booleano|Indica se o dispositivo de áudio dá suporte para processamento de fala.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Booleano|Indica se o dispositivo de áudio dá suporte para processamento bruto.|
|**System.Devices.MicrophoneArray.Geometry**|caractere sem sinal[]|Dados de geometria para uma matriz de microfones.|

## Tópicos relacionados

* [Enumerar dispositivos](../devices-sensors/enumerate-devices.md)
* [Propriedades de informações do dispositivo](../devices-sensors/device-information-properties.md)
* [Criar um seletor de dispositivo](../devices-sensors/build-a-device-selector.md)
* [Reprodução de mídia](media-playback.md)







<!--HONumber=Aug16_HO3-->


