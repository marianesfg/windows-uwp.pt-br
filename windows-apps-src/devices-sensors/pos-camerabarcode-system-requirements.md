---
title: Requisitos de sistema do scanner de código de barras da câmera
description: Este artigo lista os requisitos para usar o scanner de código de barras da câmera de um aplicativo UWP.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243277"
---
# <a name="camera-barcode-scanner-system-requirements"></a>Requisitos de sistema do scanner de código de barras da câmera
A partir do Windows 10, versão 1803, você pode ler códigos de barras por meio de uma lente de câmera padrão de um aplicativo universal do Windows.

## <a name="supported-windows-editions"></a>Edições do Windows compatíveis
- Windows 10 Professional no modo S
- Windows 10 Professional
- Windows 10 Enterprise
- Windows 10 IoT Core


## <a name="webcam-requirements"></a>Requisitos da webcam
| Category      | Recomendação           | Comentários |
| ------------- | ------------------------ | -------- |
| Foco         | Foco automático               | Foco fixo não é recomendado. |
| Resolução    | 1920 x 1440 ou mais    | Tivemos a melhor experiência com câmeras que possuem resolução 1920 x 1440 ou superior.  Algumas câmeras de resolução inferior podem ler códigos de barras padrão se o código de barras for impresso de forma grande o suficiente. Os códigos de barras com elementos mais finos podem exigir câmeras com maior resolução. |
|

## <a name="see-also"></a>Consulte também

### <a name="samples"></a>Exemplos

- [Exemplo de scanner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
