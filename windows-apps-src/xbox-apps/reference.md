---
title: API REST do Portal de Dispositivos do Xbox
description: Referência de API para UWP no Xbox One.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: d8fdcf01d7d1f72624d73acf2d10ce28dfb75e04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656991"
---
# <a name="xbox-device-portal-rest-api"></a>API REST do Portal de Dispositivos do Xbox

Esta seção contém tópicos de referência para a API REST do Portal de Dispositivos do Xbox, usado para configurar e gerenciar seu console remotamente.

| URI        | Descrição |
|------------|-------------|
|[/API/App/packagemanager/Register](wdp-loose-folder-register-api.md)| Registra um aplicativo que está contido em uma pasta livre. |
|[/API/App/packagemanager/upload](wdp-folder-upload.md)| Carrega uma pasta inteira no console. |
|[/ext/App/sshpins](uwp-sshpins-api.md)| Desmarque todas as marcações SSH confiáveis remotamente. Isso exige o emparelhamento de marcador novamente para o desenvolvimento UWP do Visual Studio. |
|[/ext/App/deployinfo](uwp-deployinfo-api.md)| Solicita informações de implantação para um ou mais pacotes instalados. |
|[/ ext/fiddler](wdp-fiddler-api.md)| Habilite e desabilite o rastreamento de rede Fiddler. |
|[/ext/httpmonitor/Sessions](wdp-httpMonitor-api.md)| Obtenha o tráfego HTTP do aplicativo focado no Xbox. |
|[/ ext/networkcredential](uwp-networkcredentials-api.md)| Adicione, remova ou atualize as credenciais de rede. |
|[/ ext/remoteinput](uwp-remoteinput-api.md)| Envie entrada de teclado, mouse ou controle remotamente para um Xbox. |
|[/ext/remoteinput/Controllers](uwp-remoteinput-controllers-api.md)| Obtenha o número de controladores físicos conectados ou desative todos os controles físicos. |
|[captura de tela/ext /](wdp-media-capture-api.md)| Captura uma representação PNG da tela exibida atualmente no console. |
|[/ ext/configurações](wdp-xboxsettings-api.md)| Acessa as configurações de desenvolvedor do Xbox One. |
|[/ext/SMB/developerfolder](wdp-smb-api.md)| Acessa a pasta de desenvolvedor no seu console através de Explorador de Arquivos em seu PC de desenvolvimento. |
|[/ ext/usuário](wdp-user-management.md)| Gerencia usuários no console do Xbox One. |
|[/ext/Xbox/Info](wdp-xboxinfo-api.md)| Fornece informações sobre o uso o dispositivo Xbox One. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Gerencia sua área restrita do Xbox Live. |

## <a name="see-also"></a>Consulte também

- [UWP no Xbox One](index.md)
- [Portal de Dispositivos do Windows](../debug-test-perf/device-portal.md)