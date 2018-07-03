---
author: TerryWarwick
title: Introdução ao Ponto de Serviço
description: Este artigo contém informações sobre a introdução às APIs de UWP de PointOfService.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: a0583adbcef9e45dfe0b2e56e03ce7e0451ac5bb
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983539"
---
# <a name="getting-started-with-point-of-service"></a>Introdução ao Ponto de Serviço

Esta seção contém tópicos que são comuns em todas as categorias de dispositivos de Ponto de serviço.

|Tópico |Descrição |
|------|------------|
| [Declaração de funcionalidade](pos-basics-capability.md)      | Saiba como adicionar a funcionalidade **pointOfService** ao manifesto do aplicativo.  Essa funcionalidade é obrigatória para usar o namespace Windows.Devices.PointOfService.  |
| [Enumerando dispositivos](pos-basics-enumerating.md)        | Saiba como definir um seletor de dispositivo usado para consultar os dispositivos disponíveis para o sistema e usar esse seletor para enumerar dispositivos de Ponto de Serviço.  |
| [Criar um objeto de dispositivo](pos-basics-deviceobject.md)  | Saiba como criar um objeto de dispositivo de PointOfService que oferece acesso às propriedades somente leitura do periférico e solicitar o periférico para uso exclusivo. |
| [Solicitar um dispositivo para uso exclusivo ](pos-basics-claim.md)  | Saiba como reservar um periférico de PointOfService para uso exclusivo com o modelo de solicitação de PointOfService, além de permitir que outros aplicativos no mesmo computador acessem o periférico do PointOfService quando precisarem de uso exclusivo.  |
|

## <a name="see-also"></a>Veja também
[Introdução ao Windows.Devices.PointOfService](pos-get-started.md)


## <a name="sample-code"></a>Código de exemplo
+ [Exemplo de scanner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemplo de caixa registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemplo de display de balcão](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemplo de leitor de tarjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemplo de impressora de Ponto de Serviço](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

