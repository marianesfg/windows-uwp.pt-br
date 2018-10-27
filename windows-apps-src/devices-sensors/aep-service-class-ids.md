---
author: muhsinking
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: IDs de classe de serviço AEP
description: Os serviços do Ponto de Extremidade de Associação (AEP) oferecem um contrato de programação para serviços compatível com um dispositivo via um determinado protocolo. Vários desses serviços estabeleceram identificadores que devem ser usados ao fazer referência a eles.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5f103ee3c281ca95abcaee76cdc6f88b74a49eb1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5694890"
---
# <a name="aep-service-class-ids"></a>IDs de classe de serviço AEP



**APIs importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Os serviços de pontos de extremidade de associação (AEP) fornecem um contrato de programação para serviços aos quais um dispositivo oferece suporte em determinado protocolo. Vários desses serviços estabeleceram identificadores que devem ser usados ao fazer referência a eles. Esses contratos são identificados com a propriedade **System.Devices.AepService.ServiceClassId**. Este tópico lista várias IDs de classe de serviço AEP conhecidas. A ID de classe de serviço AEP também é aplicável a protocolos com IDs de classe personalizada.

Um desenvolvedor de aplicativos deve usar filtros de sintaxe de consulta avançada (AQS) com base nas IDs de classe para limitar suas consultas aos serviços AEP que pretendem usar. Isso limitará os resultados da consulta aos serviços relevantes e aumentará consideravelmente o desempenho, a duração da bateria e a qualidade de serviço do dispositivo. Por exemplo, um aplicativo pode usar essas IDs de classe de serviço para usar um dispositivo como uma sincronização de Miracast ou um renderizador de mídia digital DLNA (DMR). Para obter mais informações sobre como dispositivos e serviços interagem uns com os outros, consulte [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991).

## <a name="bluetooth-and-bluetooth-le-services"></a>Serviços Bluetooth e Bluetooth LE

Os serviços Bluetooth se enquadram em um destes dois protocolos: o protocolo de Bluetooth ou o protocolo de Bluetooth LE. Os identificadores desses protocolos são:

-   ID do protocolo Bluetooth: {e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
-   ID do protocolo Bluetooth LE: {bb7bb05e-5972-42b5-94fc-76eaa7084d49}

O protocolo Bluetooth oferece suporte a vários serviços, todos seguindo o mesmo formato básico. Os quatro primeiros dígitos do GUID variam de acordo com o serviço, mas todos os GUIDs de Bluetooth terminam com **0000-0000-1000-8000-00805F9B34FB**. Por exemplo, o serviço RFCOMM tem o precursor 0x0003, então a ID completa seria **00030000-0000-1000-8000-00805F9B34FB**. A tabela a seguir lista alguns serviços Bluetooth comuns.

| Nome do serviço                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT - Serviço de notificação de alerta    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT - E/S de automação                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT - Serviço de bateria               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT - Pressão sanguínea                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT - Composição corporal              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT - Gerenciamento de vínculos               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT - Monitoramento contínuo de glicose | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT - Serviço de hora atual          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT - Potência de pedalada                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT - Velocidade e cadência de pedalada     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT - Informações do dispositivo            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT - Detecção ambiental         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT - Acesso genérico                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT - Atributo genérico             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT - Glicose                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT - Termômetro de integridade            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT - Frequência cardíaca                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT - Dispositivo de interface humana        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT - Alerta imediato               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT - Posicionamento interno            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT - Suporte ao protocolo de Internet     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT - Perda de vínculo                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT - Localização e navegação       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT - Próximo serviço de alteração de horário de verão       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT - Serviço de status de alerta telefônico    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT - Oxímetro de pulso                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT - Serviço de atualização de tempo de referência | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT - Velocidade e cadência de corrida     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT - Parâmetros de digitalização               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT - Potência de transmissão                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT - Dados de usuário                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT - Balança                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

Para uma lista mais completa de serviços Bluetooth disponíveis, consulte as páginas de protocolo e serviço de Bluetooth [aqui](http://go.microsoft.com/fwlink/p/?LinkID=619586) e [aqui](http://go.microsoft.com/fwlink/p/?LinkID=619587). Você também pode usar a API [**GattServiceUuids**](https://msdn.microsoft.com/library/windows/apps/Dn297571) para obter alguns serviços GATT comuns.

## <a name="custom-bluetooth-le-services"></a>Serviços Bluetooth LE personalizados

Os serviços Bluetooth LE personalizados usam o seguinte identificador de protocolo: {bb7bb05e-5972-42b5-94fc76eaa7084d49}

Perfis personalizados são definidos com seus próprios GUIDs definidos. O GUID personalizado deve ser usado para **System.Devices.AepService.ServiceClassId**.

## <a name="upnp-services"></a>Serviços UPnP

Os serviços UPnP usam o seguinte identificador de protocolo: {0e261de4-12f0-46e6-91ba-428607ccef64}

Em geral, todos os serviços UPnP têm seus nomes com hash em um GUID usando o algoritmo definido em RFC 4122. A tabela a seguir lista alguns serviços UPnP comuns definidos no Windows.

| Nome do serviço                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| Gerenciador de conexões                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| Transporte AV                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| Controle de renderização                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| Encaminhamento de camada 3                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| Configuração da interface WAN comum | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| Conexão IP WAP                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| Configuração de WLAN WFA             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| Impressora avançada                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| Impressora básica                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| Registrador de receptor de mídia           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| Diretório de conteúdo                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| DIAL                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

 

## <a name="wsd-services"></a>Serviços WSD

Os serviços WSD usam o seguinte identificador de protocolo: {782232aa-a2f9-4993-971b-aedc551346b0}

Em geral, todos os serviços WSD têm seus nomes com hash em um GUID usando o algoritmo definido em RFC 4122. A tabela a seguir lista alguns serviços WSD comuns definidos no Windows.

| Nome do serviço | GUID                                     |
|--------------|------------------------------------------|
| Impressora      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| Scanner      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

 

## <a name="aqs-sample"></a>Amostra de AQS

Esse AQS filtrará para todos os objetos **AssociationEndpointService** UPnP que ofereçam suporte para DIAL. Neste caso, [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) é definido como **AsssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```

 

 
