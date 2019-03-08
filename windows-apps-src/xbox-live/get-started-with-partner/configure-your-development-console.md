---
title: Configurar o console de desenvolvimento do Xbox
description: Saiba como configurar o console de desenvolvimento do Xbox para dar suporte ao desenvolvimento do Xbox Live.
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 479be2401e0c54801645ad1c0d91b11b7ffb6869
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649011"
---
# <a name="configure-your-xbox-development-console"></a>Configurar o console de desenvolvimento do Xbox

Para configurar seu console de desenvolvimento:
- Obter suas IDs
- Definir sua área restrita em seus kits de desenvolvimento
- Entrar com uma conta de desenvolvimento

## <a name="get-your-ids"></a>Obter suas IDs
Para habilitar as áreas restritas e serviços do Xbox Live, você precisará obter várias IDs para configurar o seu kit de desenvolvimento e seu título. Eles podem ser feitos com o mesmo processo.

Siga [configuração do serviço Xbox Live](../xbox-live-service-configuration.md) para obter suas IDs

## <a name="set-your-sandbox-on-your-development-kits"></a>Definir sua área restrita em seus kits de desenvolvimento
Você não poderá inicializar seu kit de desenvolvimento sem definir sua ID de área restrita. Para fazer isso, você pode usar o "Xbox um Gerenciador de" que é instalado em seu PC pelo XDK, ou você pode abrir uma janela de comando do XDK e use o comando de configuração (xbconfig.exe) da seguinte maneira:

Verifique sua área de segurança atual. Digite xbconfig sandboxid no prompt de comando.

Se for não o que você espera, altere sua id de área restrita. Digite xbconfig Message={0}][sandboxid =<your sandbox id> no prompt de comando.

Reinicialize o seu console usando a reinicialização (xbreboot.exe) no prompt de comando.

Verifique se a que sua área restrita foi redefinida corretamente. Digite xbconfig sandboxid no prompt de comando.

## <a name="sign-in-with-a-development-account"></a>Entrar com uma conta de desenvolvimento

Você pode criar contas de desenvolvimento usadas para entrar em [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) ou [Partner Center](https://partner.microsoft.com/dashboard)
