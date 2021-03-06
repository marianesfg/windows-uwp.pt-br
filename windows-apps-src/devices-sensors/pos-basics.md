---
title: Noções básicas de ponto de serviço
description: Este artigo contém informações sobre como começar a usar as APIs de Windows Runtime do PointOfService.
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: cb8acf5f7dce8e81d72850a4dbd097083b240864
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730042"
---
# <a name="point-of-service-basics"></a>Noções básicas de ponto de serviço

Esta seção contém tópicos que são comuns em todas as categorias de dispositivos de Ponto de serviço.

|Tópico |Descrição |
|------|------------|
| [Declaração de funcionalidade](pos-basics-capability.md)      | Saiba como adicionar a funcionalidade **pointOfService** ao manifesto do aplicativo.  Essa funcionalidade é obrigatória para usar o namespace Windows.Devices.PointOfService.  |
| [Enumerando dispositivos](pos-basics-enumerating.md)        | Saiba como definir um seletor de dispositivo usado para consultar os dispositivos disponíveis para o sistema e usar esse seletor para enumerar dispositivos de Ponto de Serviço.  |
| [Criar um objeto de dispositivo](pos-basics-deviceobject.md)  | Saiba como criar um objeto de dispositivo de PointOfService que oferece acesso às propriedades somente leitura do periférico e solicitar o periférico para uso exclusivo. |
| [Declarar e habilitar](pos-basics-claim.md)  | Saiba como reservar um periférico PointOfService para uso exclusivo e habilitar operações de e/s.  |
| [Compartilhar periféricos com outras pessoas](pos-basics-sharing.md) | Saiba como compartilhar redes ou periféricos conectados por Bluetooth com outros computadores em um ambiente em que vários PCs dependem de periféricos compartilhados em vez de periféricos dedicados anexados a cada computador.
| [PointOfService de ponta a ponta](pos-get-started.md)  | Este é um exemplo de ponta a ponta de como interagir com periféricos PointOfService utilizando os exemplos acima. |
|

## <a name="see-also"></a>Confira também

| Tópico   | Descrição |
|:--------|:------------|
| [Distribuição de aplicativos](../publish/distribute-lob-apps-to-enterprises.md) | Saiba mais sobre as opções para distribuir seu aplicativo para clientes corporativos. |
| [Ciclo de vida do aplicativo](../launch-resume/app-lifecycle.md) | Saiba mais sobre o ciclo de vida de um aplicativo UWP e o que acontece quando o Windows inicia, suspende e retoma seu aplicativo. |
| [Recursos do aplicativo](../app-resources/index.md) | Saiba como criar, empacotar e consumir a cadeia de caracteres, a imagem e os recursos de arquivo do seu aplicativo. |
| [Associação de dados](../data-binding/index.md) | Saiba como usar a vinculação de dados para exibir dados na interface do usuário do seu aplicativo. |
| [Enumeração de dispositivo](enumerate-devices.md) | Aprenda a usar técnicas de enumeração avançadas para encontrar seus periféricos.|
| [Aplicativos adaptáveis de versão](../debug-test-perf/version-adaptive-apps.md) | Saiba como projetar seu aplicativo para que ele seja executado em várias versões do Windows 10.|
|


## <a name="sample-code"></a>Código de exemplo
+ [Exemplo de scanner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemplo de caixa registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemplo de display de balcão](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemplo de leitor de tarjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemplo de impressora de Ponto de Serviço](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
