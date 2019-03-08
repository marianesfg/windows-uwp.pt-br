---
title: Adicionar o Xbox Live para um projeto do XDK
description: Saiba como adicionar o Xbox Live a um projeto novo ou existente do Kit de desenvolvedor do Xbox (XDK).
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, xdk
ms.localizationpriority: medium
ms.openlocfilehash: f17765b09dcb0b6f5c89d168f2d3d9611a60ffa6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649041"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>Adicionar o Xbox Live a um projeto novo ou existente do XDK

Este tópico descreve como adicionar o Xbox Live a um projeto novo ou existente do XDK.

O processo é:

- Configurar seu ambiente de desenvolvimento do Xbox One
- Obter suas IDs
- Configurar seu console de desenvolvimento
- Adicione o TitleID e SCID seu binário


## <a name="setup-up-your-xbox-one-development-environment"></a>Configurar seu ambiente de desenvolvimento do Xbox One
Primeiro, o console de configuração seguindo a seção "Configuração inscrever seu Xbox um ambiente de desenvolvimento" na documentação do XDK

## <a name="get-your-ids"></a>Obter suas IDs

Para habilitar serviços do Xbox Live, você precisará obter várias IDs para configurar o seu kit de desenvolvimento e seu título. Eles podem ser feitos com o mesmo processo.

Você irá obter suas IDs de seguindo o processo de [configuração do serviço Xbox Live](../xbox-live-service-configuration.md)

## <a name="configure-your-development-console"></a>Configurar seu console de desenvolvimento

Depois de ter suas IDs, execute as [configurar seu console de desenvolvimento](configure-your-development-console.md) guia para configurar seu console de desenvolvimento.

## <a name="add-the-titleid-and-scid-to-your-binary"></a>Adicione o TitleID e SCID seu binário
Enquanto a área restrita é configurada em um nível de plataforma para cada Kit de desenvolvimento, o TitleID e SCID são associados a um binário específico. Para adicionar um TitleID e SCID para seu binário, modifique o Package. appxmanifest para binário, adicionando um novo nó no <Extensions> nó da seguinte maneira:

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

Para obter mais informações sobre o arquivo Appxmanifest XML, consulte modelos de projeto no Visual Studio para desenvolvimento de um Xbox.

Consulte o esquema do manifesto do aplicativo para obter uma descrição do aplicativo de esquema de manifesto.

**O sinalizador RequireXboxLive** se o sinalizador é definido como true, o título de RequireXboxLive não serão iniciados a menos que o nível de Conexão Windows.Networking.Connectivity retorna como XboxLiveAccess e o título limpa a autenticação com o Xbox Live. Isso assegura que o título obteve as últimas atualizações de conteúdo. Se a conectividade for perdida enquanto o título está em execução, o título está suspenso.

Somente títulos "Internet necessária" devem marcar RequireXboxLive como verdadeiro e observe que marcar seu título dessa maneira não garante que os serviços necessários para o título estão funcionando e em execução.
