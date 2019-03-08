---
title: DeviceEndpoint (JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
description: " DeviceEndpoint (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eaa21072ebf14b6f6d959ff40af34724a45522f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618871"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint (JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
O objeto DeviceEndpoint tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| deviceName| cadeia de caracteres| Opcional. Um nome amigável para o dispositivo, se aplicável. Atualmente, esse valor não é usado.| 
| endpointUri| cadeia de caracteres| Obrigatório. A URL que a plataforma de cliente (Windows ou Windows Phone) tenha obtido de seu serviço de notificação por push (WNS ou MPNS).| 
| locale| cadeia de caracteres| Obrigatório. O idioma desejado de notificações enviadas para esse ponto de extremidade. Pode ser uma lista de valores separados por vírgula na ordem de preferência. Exemplo: "de-DE, en-US, en".| 
| Plataforma| cadeia de caracteres| Opcional. Valores com suporte no momento são "WindowsPhone" e "Windows". Se não for especificado, ele é derivado do token do dispositivo.| 
| platformVersion| cadeia de caracteres| Opcional. O formato dessa cadeia de caracteres é específico para cada plataforma. Atualmente, esse valor não é usado.| 
| systemId| GUID| Obrigatório. Identificador exclusivo para a "instância de aplicativo" (combinação de usuário do dispositivo). Implementação de práticas recomendadas é para um aplicativo gerar um GUID aleatório após a instalação/primeira execução e continuar a usar esse valor em execuções subsequentes do aplicativo.| 
| titleId| inteiro sem sinal de 32 bits| Obrigatório. A ID do título do jogo emitir a chamada para o serviço.| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "https://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Referência   