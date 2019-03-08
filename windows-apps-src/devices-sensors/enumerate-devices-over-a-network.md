---
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: Enumerar dispositivos em uma rede
description: Além da descoberta de dispositivos conectados localmente, você pode usar as APIs Windows.Devices.Enumeration para enumerar dispositivos por meio de protocolos de rede e sem fio.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e80d16b3338291c756b543018812e9db1370a4ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630411"
---
# <a name="enumerate-devices-over-a-network"></a>Enumerar dispositivos em uma rede



**APIs importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Além da descoberta de dispositivos conectados localmente, você pode usar as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) para enumerar dispositivos por meio de protocolos de rede e sem fio.

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>Enumerando dispositivos por meio de protocolos de rede ou sem fio

Às vezes, você precisa enumerar dispositivos que não estão conectados localmente e só podem ser descobertos por um protocolo sem fio ou de rede. Para fazer isso, as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) têm três tipos diferentes de objetos de dispositivo: o **AssociationEndpoint** (AEP), o **AssociationEndpointContainer** (contêiner AEP) e o **AssociationEndpointService** (serviço AEP). Como grupo, eles são conhecidos como AEPs ou objetos AEP.

Algumas APIs de dispositivo fornecem uma cadeia de caracteres de seletor que você pode usar para enumerar os objetos AEP disponíveis. Isso pode incluir os dois dispositivos que estão e não estão emparelhados com o sistema. Alguns dos dispositivos podem não exigir emparelhamento. Essas APIs de dispositivo podem tentar emparelhar o dispositivo se for necessário antes de interagir com ele. Wi-Fi Direct é um exemplo de APIs que seguem esse padrão. Se essas APIs de dispositivo não emparelharem automaticamente o dispositivo, você pode emparelhá-las usando o objeto [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396) disponível em [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960).

No entanto, pode haver casos em que você queira descobrir manualmente dispositivos por conta própria, sem usar uma cadeia de caracteres predefinida do seletor. Por exemplo, talvez você só precise coletar informações sobre dispositivos AEP sem interagir com eles ou pode querer localizar mais objetos AEP do que os que serão descobertos com a cadeia de caracteres predefinida do seletor. Nesse caso, você irá criar sua própria cadeia de caracteres do seletor e usá-la seguindo as instruções em [Criar um seletor de dispositivo](build-a-device-selector.md).

Quando você cria seu próprio seletor, é altamente recomendável limitar o escopo de enumeração aos protocolos de seu interesse. Por exemplo, você não quer que o rádio Wi-Fi procure dispositivos Wi-Fi Direct se estiver interessado particularmente em dispositivos UPnP. O Windows definiu uma identidade para cada protocolo que você pode usar para definir o escopo da enumeração. A tabela a seguir lista os tipos e identificadores de protocolo.

| Tipo de dispositivo de rede ou protocolo              | Id                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP (incluindo DIAL e DLNA)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| Serviços Web em dispositivos (WSD)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-Fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| Descoberta de serviços DNS (DNS-SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| Ponto de serviço                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| Impressoras de rede (impressoras do active directory) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Conexão Fácil do Windows (WNC)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| WiGig docks                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| Provisionamento de Wi-Fi para impressoras HP           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| Bluetooth                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| Bluetooth LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## <a name="aqs-examples"></a>Exemplos de AQS

Cada tipo AEP tem uma propriedade que você pode usar para restringir a enumeração a um protocolo específico. Lembre-se que você pode usar o operador OR em um filtro AQS para combinar vários protocolos. Aqui estão alguns exemplos de cadeias de caracteres de filtro AQS que mostram como consultar dispositivos AEP.

Esse AQS consulta todos os objetos UPnP **AssociationEndpoint** quando o [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está definido como **AsssociationEndpoint**.

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Esse AQS consulta todos os objetos UPnP e WSD **AssociationEndpoint** quando o [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está definido como **AsssociationEndpoint**.

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Esse AQS consulta todos os objetos UPnP **AssociationEndpointService** se o [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está definido como **AsssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Esse AQS consulta objetos **AssociationEndpointContainer** quando [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está definido como **AssociationEndpointContainer**, mas apenas os encontra enumerando o protocolo UPnP. Normalmente, não é útil enumerar contêineres somente provenientes de um protocolo. No entanto, isso pode ser útil ao limitar seu filtro a protocolos onde você sabe que seu dispositivo pode ser descoberto.

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 
