---
title: Configurar seu AppXManifest para vários jogadores
description: Saiba como configurar seu AppXManifest UWP para habilitar os convites para múltiplos jogadores do Xbox Live.
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, ativação de protocolo, com vários participantes
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646021"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>Configurar seu AppXManifest para vários jogadores

Você precisa fazer algumas atualizações para o arquivo. appxmanifest no projeto do Visual Studio, se as seguintes condições forem verdadeiras:
- Você está desenvolvendo um UWP
- Você deseja implementar a capacidade de jogadores e convidar outros usuários ao seu título

Se você não fizer essa etapa, o título não obterá protocolo ativado quando um player de destinatário aceita um convite para reproduzir.

## <a name="open-your-packageappxmanifest"></a>Abra seu Package. appxmanifest

O arquivo Package. appxmanifest normalmente está localizado no mesmo diretório que o arquivo de solução do projeto do Visual Studio.  Ou você pode encontrá-lo no Gerenciador de soluções.

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>Adicionar nova entrada

Você precisará adicionar o seguinte para o ```<Extensions>``` sob o elemento ```<Applications>``` no arquivo Package. appxmanifest

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Eg:

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

Salve e recompile seu título.  Para saber como usar o Gerenciador de com vários participantes para implementar a capacidade para convidar participantes para seu título, consulte [reproduzir com vários participantes com amigos](../multiplayer-manager/play-multiplayer-with-friends.md)
