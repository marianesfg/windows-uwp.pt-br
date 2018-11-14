---
author: TerryWarwick
title: Introdução ao Ponto de Serviço
description: Este artigo contém informações sobre a introdução às APIs de UWP de PointOfService.
ms.author: jken
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 46dd1f615e42f6e89ee9a92cb980299e9a0e5205
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6275739"
---
# <a name="getting-started-with-point-of-service"></a>Introdução ao Ponto de Serviço

## <a name="pointofservice-basics"></a>Noções básicas de PointOfService

Esta seção contém tópicos que são comuns em todas as categorias de dispositivos de Ponto de serviço.

|Tópico |Descrição |
|------|------------|
| [Declaração de funcionalidade](pos-basics-capability.md)      | Saiba como adicionar a funcionalidade **pointOfService** ao manifesto do aplicativo.  Essa funcionalidade é obrigatória para usar o namespace Windows.Devices.PointOfService.  |
| [Enumerando dispositivos](pos-basics-enumerating.md)        | Saiba como definir um seletor de dispositivo usado para consultar os dispositivos disponíveis para o sistema e usar esse seletor para enumerar dispositivos de Ponto de Serviço.  |
| [Criar um objeto de dispositivo](pos-basics-deviceobject.md)  | Saiba como criar um objeto de dispositivo de PointOfService que oferece acesso às propriedades somente leitura do periférico e solicitar o periférico para uso exclusivo. |
| [Declaração e habilitar ](pos-basics-claim.md)  | Saiba como reservar um periférico de PointOfService para uso exclusivo e habilitar para operações de e/s.  |
| [Compartilhar periféricos com outras pessoas](pos-basics-sharing.md) | Saiba como compartilhar periféricos Bluetooth conectado ou rede com outros computadores em um ambiente onde vários computadores contam com periféricos compartilhados em vez de dedicada periféricos conectados para cada computador.
| [PointOfService de ponta a ponta](pos-get-started.md)  | Este é um exemplo de ponta a ponta de como interagir com periféricos do PointOfService que utilizam os exemplos acima. |
|

## <a name="see-also"></a>Consulte também

| Tópico   | Descrição |
|:--------|:------------|
| [Distribuição de aplicativos](../publish/distribute-lob-apps-to-enterprises.md) | Saiba mais sobre as opções para distribuir seu aplicativo para clientes corporativos. |
| [Ciclo de vida do aplicativo](../launch-resume/app-lifecycle.md) | Saiba mais sobre o ciclo de vida de um aplicativo UWP e o que acontece quando o Windows inicia, suspende e retoma o seu aplicativo. |
| [Recursos de aplicativo](../app-resources/index.md) | Saiba como criar, empacotar e consumir recursos de arquivo, imagem e cadeia de caracteres do seu aplicativo. |
| [Associação de dados](../data-binding/index.md) | Saiba como usar a vinculação de dados para exibir dados na interface do usuário do seu aplicativo. |
| [Enumeração do dispositivo](enumerate-devices.md) | Saiba sobre técnicas de enumeração de uso avançada para localizar os periféricos.|
| [Aplicativos adaptáveis de versão](../debug-test-perf/version-adaptive-apps.md) | Saiba como projetar seu aplicativo para que ele seja executado em várias versões do Windows 10.|
|


## <a name="sample-code"></a>Código de exemplo
+ [Exemplo de scanner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemplo de caixa registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemplo de display de balcão](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemplo de leitor de tarjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemplo de impressora de Ponto de Serviço](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

