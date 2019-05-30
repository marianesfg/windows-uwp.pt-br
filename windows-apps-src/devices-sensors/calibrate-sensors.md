---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Calibrar sensores
description: Sensores em um dispositivo baseado no magnetômetro (bússola, inclinômetro e sensor de orientação) podem precisar de calibragem devido a fatores ambientais.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 21e902daac01d8ed2645625320dec27bf7805fba
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370309"
---
# <a name="calibrate-sensors"></a>Calibrar sensores


**APIs importantes**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**Windows.Devices.Sensors.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

Sensores em um dispositivo baseado no magnetômetro (bússola, inclinômetro e sensor de orientação) podem precisar de calibragem devido a fatores ambientais. A enumeração [**MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) pode ajudar a determinar um curso de ação quando seu dispositivo precisar de calibração.

## <a name="when-to-calibrate-the-magnetometer"></a>Quando calibrar o magnetômetro

A enumeração [**MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) tem quatro valores que ajudam a determinar se o dispositivo no qual o seu aplicativo está em execução precisa ser calibrado. Se um dispositivo precisar ser calibrado, você deve informar ao usuário que uma calibragem é necessária. No entanto, você não deve enviar prompts de calibragem com muita frequência para o usuário. Recomendamos não mais do que uma vez a cada 10 minutos.

| Valor           | Descrição    |
| ----------------- | ------------------- |
| **Desconhecido**     | O driver do sensor não pôde relatar a precisão atual. Isso não significa necessariamente que o dispositivo esteja descalibrado. Cabe ao seu aplicativo decidir o melhor curso de ação se o valor **Desconhcido** for retornado. Se o seu aplicativo depender de uma leitura de sensor precisa, convém enviar um prompt para o usuário calibrar o dispositivo. |
| **Não confiável**  | Atualmente, há um alto grau de imprecisão no magnetômetro. Os aplicativos sempre devem solicitar uma calibragem ao usuário da primeira vez em que esse valor é retornado. |
| **Aproximar** | Os dados são precisos o suficiente para alguns aplicativos. Um aplicativo de realidade virtual, que só precisa saber se o usuário movimentou o dispositivo para cima/baixo ou para a esquerda/direita, pode continuar sem calibragem. Aplicativos que exigem uma posição absoluta, como um aplicativo de navegação que precisa saber em que direção você está indo para poder fornecer instruções, precisam solicitar uma calibragem. |
| **Alta**        | Os dados são precisos. Nenhuma calibragem é necessária, mesmo para aplicativos que precisam conhecer uma posição absoluta, como aplicativos de navegação ou realidade ampliada. |