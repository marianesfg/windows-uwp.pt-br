---
title: Funcionalidade de dispositivo PointOfService
description: A funcionalidade de PointOfService é obrigatória para usar o namespace Windows.Devices.PointOfService
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 832881919a3ed1f44c0912703ede5948c5b10d11
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872816"
---
# <a name="pointofservice-device-capability"></a>Funcionalidade de dispositivo PointOfService
Solicite acesso às APIs do PointOfService declarando a funcionalidade no manifesto do pacote do aplicativo]. Você pode declarar a maioria das funcionalidades usando o Designer de manifesto no Microsoft Visual Studio ou você pode adicioná-los manualmente.  

> [!Important]
> Você receberá o erro **System.UnauthorizedAccessException** ao tentar usar uma API no namespace Winodws.Devices.PointOfService se você não declarar a funcionalidade **pointOfService** no manifesto do aplicativo. 

## <a name="declare-capability-using-manifest-designer"></a>Declare a funcionalidade usando o Designer de manifesto

1. No **Gerenciador de soluções**, expanda o nó do projeto do seu aplicativo UWP.
2. Clique duas vezes no arquivo **Package.appxmanifest**.  
*Se o arquivo de manifesto já estiver aberto no modo de exibição de código XML, o Visual Studio solicitará que você feche o arquivo.*
3. Clique na guia **Funcionalidades**
4. Clique na caixa de seleção ao lado de **Ponto do serviço** na lista de funcionalidades para permitir a funcionalidade do dispositivo do Ponto do serviço


## <a name="declare-capability-manually"></a>Declarar a funcionalidade manualmente

1. No **Gerenciador de soluções**, expanda o nó do projeto do seu aplicativo UWP.
2. Clique com o botão direito do mouse no arquivo **Package.appxmanifest** e escolha **Exibir código**
3. Adicione o elemento PointOfService DeviceCapability à seção de Funcionalidades do manifesto do aplicativo.  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
