---
author: mukin
title: Registradora
description: "Este artigo contém informações sobre o ponto de família de serviços dos dispositivos da Caixa Registradora"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 
ms.openlocfilehash: 376272356cf720ddd9519f0077e771a1016abb1e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="cash-drawer"></a>Registradora

Permite aos desenvolvedores de aplicativos interagir com a [registradora](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.cashdrawer).

## <a name="requirements"></a>Requisitos
Os aplicativos que exigem esse namespace exigem a adição de "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) ao manifesto do pacote do aplicativo.

## <a name="device-support"></a>Suporte a dispositivos
A conexão diretamente para a registradora pode ser feita pela rede ou por meio de Bluetooth, dependendo dos recursos da unidade registradora. Além disso, registradoras que não têm recursos de Bluetooth ou rede podem ser conectadas por meio da porta DK em uma impressora POS compatível ou do acessório Star Micronics DK-AirCash. Neste momento, não há nenhum suporte para registradoras conectadas via USB ou porta serial.

**Observação:**  Entre em contato com a Star Micronics para obter mais informações sobre o DK-AirCash.

### <a name="networkbluetooth"></a>Rede/Bluetooth
| Fabricante |    Model(os) |
|--------------|-----------|
| Registradora APG |    NetPRO, BluePRO |

## <a name="examples"></a>Exemplos
Veja a amostra da registradora para obter um exemplo de implementação.
+    [Amostra de Registradora](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
