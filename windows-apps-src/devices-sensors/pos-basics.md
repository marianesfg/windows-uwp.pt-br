---
title: Introdução ao Ponto de Serviço
description: Este artigo contém informações sobre a introdução às APIs de UWP de PointOfService.
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 1b4ff9443c40cf44e171bf898b627de3e2819034
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656461"
---
# <a name="getting-started-with-point-of-service"></a>Introdução ao Ponto de Serviço

## <a name="pointofservice-basics"></a>Noções básicas de PointOfService

Esta seção contém tópicos que são comuns em todas as categorias de dispositivos de Ponto de serviço.

|Tópico |Descrição |
|------|------------|
| [Declaração de recurso](pos-basics-capability.md)      | Saiba como adicionar a funcionalidade **pointOfService** ao manifesto do aplicativo.  Essa funcionalidade é obrigatória para usar o namespace Windows.Devices.PointOfService.  |
| [Enumeração de dispositivos](pos-basics-enumerating.md)        | Saiba como definir um seletor de dispositivo usado para consultar os dispositivos disponíveis para o sistema e usar esse seletor para enumerar dispositivos de Ponto de Serviço.  |
| [Criando um objeto de dispositivo](pos-basics-deviceobject.md)  | Saiba como criar um objeto de dispositivo de PointOfService que oferece acesso às propriedades somente leitura do periférico e solicitar o periférico para uso exclusivo. |
| [Declaração e habilitar ](pos-basics-claim.md)  | Saiba como reservar um PointOfService periférico para uso exclusivo e habilitar para operações de e/s.  |
| [Periféricos de compartilhamento com outras pessoas](pos-basics-sharing.md) | Aprenda a compartilhar periféricos de Bluetooth conectado ou de rede com outros computadores em um ambiente onde vários PCs dependem de periféricos compartilhados em vez de periféricos dedicados anexados a cada computador.
| [PointOfService de ponta a ponta](pos-get-started.md)  | Este é um exemplo de ponta a ponta de como interagir com os periféricos PointOfService utilizando os exemplos acima. |
|

## <a name="see-also"></a>Consulte também

| Tópico   | Descrição |
|:--------|:------------|
| [Distribuição de aplicativos](../publish/distribute-lob-apps-to-enterprises.md) | Saiba mais sobre as opções para distribuir seu aplicativo para clientes corporativos. |
| [Ciclo de vida do aplicativo](../launch-resume/app-lifecycle.md) | Saiba mais sobre o ciclo de vida de um aplicativo UWP e o que acontece quando o Windows inicia, suspende e retoma seu aplicativo. |
| [Recursos do aplicativo](../app-resources/index.md) | Saiba como criar, pacote, e consumir a cadeia de caracteres, imagem e recursos de arquivo do seu aplicativo. |
| [Vinculação de dados](../data-binding/index.md) | Saiba como usar a ligação de dados para exibir dados na interface de usuário do seu aplicativo. |
| [Enumeração de dispositivo](enumerate-devices.md) | Aprenda técnicas de enumeração de uso avançado para localizar os periféricos.|
| [Aplicativos adaptáveis da versão](../debug-test-perf/version-adaptive-apps.md) | Saiba como projetar seu aplicativo para que ele seja executado em várias versões do Windows 10.|
|


## <a name="sample-code"></a>Código de exemplo
+ [Exemplo de scanner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemplo de gaveta de pagamento à vista]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemplo de exibição de linha](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Amostra de leitor magnética](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemplo de POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

