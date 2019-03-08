---
title: Usando APIs do Xbox Live internos para o XDK
description: Saiba como usar as APIs do internos Xbox Live em seu projeto de Kit de desenvolvedor do Xbox (XDK).
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598401"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>Usando APIs do Xbox Live internos para o XDK

1. Clique com botão direito no projeto no Visual Studio e escolha "Referências".
1. Escolha "Adicionar nova referência"
1. Clique em "Durango. <build number>" e "Extensões" no painel à esquerda
1. No meio, escolha:
- Se você quiser usar a API do WinRT XSAPI, escolha "API de serviços do Xbox"
- Se você quiser usar a API de XSAPI C++, escolha "Cpp de API de serviços do Xbox"
1. Clique em OK

Observação: Se seu sistema de compilação não dá suporte a arquivos de objetos, você deve adicionar manualmente as definições de pré-processador e bibliotecas, como visto no `%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`
