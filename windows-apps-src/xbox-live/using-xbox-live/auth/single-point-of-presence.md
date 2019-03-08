---
title: Único ponto de presença (SPOP)
description: Saiba como usar o Xbox Live único ponto de presença (SPOP) para garantir que um título é reproduzido em apenas um único dispositivo por vez.
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, spop, ponto único de presença
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595001"
---
# <a name="single-point-of-presence-spop"></a>Único ponto de presença (SPOP)

## <a name="overview"></a>Visão geral
Único ponto de presença (SPOP), é uma condição de Xbox Live imposta em que um título só poderão ser executado em um único dispositivo por vez. Isso é imposto para Xbox um XDK e títulos UWP em qualquer dispositivo.
Um título do Xbox Live, independentemente do dispositivo é ativado, poderá iniciar um usuário que está conectado a um título em outro dispositivo Xbox One ou Windows 10.

## <a name="how-to-handle-spop"></a>Como manipular SPOP
SPOP deve ser manipulada pelo título da mesma maneira que qualquer outro tipo de evento de saída. O usuário sempre será notificado por meio da interface do usuário quando eles fazem uma ação que iniciaria uma SPOP para verificar que gostaria de fazer com que o título a ser desconectado no outro dispositivo.

* Para títulos XDK o `User::SignOutCompleted` evento será disparado quando isso ocorre.
* Para títulos UWP, eles serão notificados sobre a saída por meio de `sign_out_complete` manipulador o `xbox_live_user` classe. Ver [autenticação para projetos UWP](authentication-for-UWP-projects.md) para obter mais detalhes
