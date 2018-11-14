---
author: laurenhughes
title: Arquiteturas de pacotes de apps
description: Saiba mais sobre quais arquiteturas de processador você deve usar ao criar seu pacote de aplicativos UWP.
ms.author: lahugh
ms.date: 7/13/2017
ms.topic: article
keywords: windows 10, uwp, empacotamento, arquitetura, configuração do pacote
ms.localizationpriority: medium
ms.openlocfilehash: 3e265df32a8c4168cddced905e7b0712e4601264
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6192087"
---
# <a name="app-package-architectures"></a>Arquiteturas de pacotes de apps

Os pacotes de apps estão configurados para serem executados em uma arquitetura de processador específica. Ao selecionar uma arquitetura, você está especificando em quais dispositivos quer que seu app seja executado. Os aplicativos UWP (Plataforma Universal do Windows) podem ser configurados para serem executados nas seguintes arquiteturas:
- x86
- x64
- ARM

É **altamente** recomendado que você compile o pacote do aplicativo para todas as arquiteturas. Ao desmarcar uma arquitetura de dispositivo, você está limitando o número de dispositivos em que seu app pode ser executado e que, por sua vez, limitará a quantidade de pessoas que podem usar seu aplicativo!

## <a name="windows-10-devices-and-architectures"></a>Dispositivos e arquiteturas do Windows 10

> [!div class="mx-tableFixed"]
| Arquitetura UWP | Desktop (x86)      | Desktop (x86)      | Desktop (ARM)      | Celular             | HoloLens           | Xbox               | IoT Core (depende do dispositivo) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |
 

Vamos falar sobre essas arquiteturas mais detalhadamente. 

### <a name="x86"></a>x86
Escolher x86 é geralmente a configuração mais segura para um pacote de aplicativo, já que será executada em quase todos os dispositivos. Em alguns dispositivos, um pacote de aplicativo com a configuração x86 não será executado, como no Xbox ou em alguns dispositivos IoT Core. No entanto, para um PC, o pacote x86 é a opção mais segura e tem o maior alcance para implantação de dispositivo. Uma parte considerável de dispositivos Windows 10 continua a executar a versão x86 do Windows. 

### <a name="x64"></a>x64
Essa configuração é usada com menos frequência do que a configuração x86. Observe que essa configuração é reservada para desktops que usam versões de 64 bits do Windows 10, [aplicativos UWP no Xbox](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation) e o Windows 10 IoT Core no Intel Joule.

### <a name="arm"></a>ARM
O Windows 10 na configuração do ARM inclui computadores desktop, dispositivos móveis e alguns dispositivos IoT Core (Rasperry Pi 2, Raspberry Pi 3 e DragonBoard). O Windows 10 em computadores desktop ARM são uma novidade para a família do Windows e, portanto, se você for um desenvolvedor de aplicativos UWP, deverá enviar pacotes ARM para a Loja para ter a melhor experiência nesses computadores. 

Confira esta //Build talk para ver uma demonstração do [Windows 10 em ARM](https://channel9.msdn.com/Events/Build/2017/P4171) e saiba mais sobre como ele funciona. 

Para obter mais informações sobre tópicos específicos da IoT, consulte [Implantação de um app com o Visual Studio](https://developer.microsoft.com/windows/iot/Docs/AppDeployment).
