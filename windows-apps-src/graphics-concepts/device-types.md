---
title: Tipos de dispositivos
description: Os tipos de dispositivos Direct3D incluem dispositivos de camada de abstração de hardware (hal) e o rasterizador de referência.
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- Tipos de dispositivos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ddb1dc0e42f88cf65464841388b9addfb4b5748
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332293"
---
# <a name="device-types"></a>Tipos de dispositivos


Os tipos de dispositivos Direct3D incluem dispositivos de camada de abstração de hardware (hal) e o rasterizador de referência.

## <a name="span-idhaldevicespanspan-idhaldevicespanspan-idhaldevicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>Dispositivo HAL


O tipo de dispositivo principal é o dispositivo hal, que dá suporte à rasterização acelerada por hardware e ao processamento de vértice de hardware e software. Se o computador no qual seu aplicativo está sendo executado é equipado com um adaptador de vídeo compatível com o Direct3D, seu aplicativo deve usá-lo para as operações do Direct3D. Os dispositivos hal com Direct3D implementam todos ou parte dos módulos de rasterização, iluminação e transformação no hardware.

Os aplicativos não acessam os adaptadores gráficos diretamente. Eles chamam métodos e funções do Direct3D. O Direct3D acessa o hardware por meio do hal. Se o computador que seu aplicativo está sendo executado oferece suporte a hal, ele obterá o melhor desempenho usando um dispositivo hal.

## <a name="span-idreferencedevicespanspan-idreferencedevicespanspan-idreferencedevicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>Dispositivo de referência


O Direct3D dá suporte a um tipo de dispositivo adicional chamado um rasterizador de referência ou dispositivo de referência. Ao contrário de um dispositivo de software, o rasterizador de referência dá suporte a todos os recursos do Direct3D. Este dispositivo se destina a ser usado para fins de depuração e, portanto, só está disponível em computadores em que o SDK do DirectX tiver sido instalado. Como esses recursos são implementados para precisão em vez de velocidade e são implementados no software, os resultados não são muito rápidos. O rasterizador de referência faz uso de instruções especiais da CPU sempre que possível, mas ele não foi desenvolvido para aplicativos de varejo. Use o rasterizador de referência apenas para fins de teste ou demonstração do recurso.

## <a name="span-idhalvsrefspanspan-idhalvsrefspanspan-idhalvsrefspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>Dispositivos HAL vs. REF


Os dispositivos HAL (camada de abstração de Hardware) e REF (rasterizador de referência) são os dois tipos principais de dispositivo Direct3D; o primeiro baseia-se no suporte de hardware e é muito rápido, mas talvez não dê suporte a tudo; enquanto o segundo usa a aceleração de hardware, portanto é muito lento, mas garante suporte a todo o conjunto de recursos do Direct3D, da forma correta. Em geral, você só precisará usar dispositivos HAL, mas se você estiver usando algum recurso avançado que não dá suporte à placa gráfica, então talvez seja necessário voltar ao REF.

Uma outra ocasião onde convém usar o REF é se o dispositivo HAL estiver produzindo resultados estranhos, ou seja, você tem certeza que seu código está correto, mas o resultado não é o que você espera. É garantido que o dispositivo REF irá se comportar corretamente, portanto, convém testar seu aplicativo no dispositivo REF e ver se o comportamento estranho continua. Se isso não acontecer, isso significa que o seu aplicativo está pressupondo que a placa gráfica oferece suporte há algo não suportado por ela, ou (b) é um bug de driver. Se ainda não funcionar com o dispositivo REF, trata-se de um bug de aplicativo.

## <a name="span-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>Processamento de vértice de software versus hardware


O processamento de vértice de hardware versus software se aplica somente aos dispositivos HAL. Quando você transmite os vértices por push através do pipeline, eles precisam ser transformadas (pelas matrizes de mundo, modo de exibição e projeção) e iluminados (pelas internas luzes do D3D) - esse estágio de processamento é conhecido como T&L (para transformação e iluminação). O processamento de vértice de hardware significa que isso é feito no hardware, se o hardware oferecer suporte a ele; logo, o processamento de vértice de software é feito no software. A prática geral é a criação de um dispositivo de Hardware T&L primeiro, se isso falhar, tente Misto e se falhar novamente tente Software. (Se o software falhar, não continue insistindo e encerre com um erro).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Dispositivos](devices.md)

 

 




