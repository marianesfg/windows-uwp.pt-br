---
author: PatrickFarley
title: Apps e dispositivos conectados (projeto Roma)
description: Esta seção descreve como usar a plataforma de Sistemas Remotos para descobrir dispositivos remotos, iniciar um app em um dispositivo remoto e comunicar-se com um serviço de app em um dispositivo remoto.
ms.author: pafarley
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: ff5871c444166770f2512e4e00b638a8fc57a9dd
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018480"
---
# <a name="connected-apps-and-devices-project-rome"></a>Apps e dispositivos conectados (projeto Roma)

Esta seção explica como conectar apps entre dispositivos e plataformas usando o Project Rome. Saiba como descobrir dispositivos remotos, iniciar um app em um dispositivo remoto e comunicar-se com um serviço de app em um dispositivo remoto.

A maioria dos usuários tem vários dispositivos e muitas vezes começam uma atividade em um dispositivo e a concluem em outro. Para acomodar isso, os aplicativos precisam estendem a dispositivos e plataformas.

As [APIs de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) introduzidas no Windows 10, versão 1607, permitem que você crie aplicativos que permitem aos usuários iniciar uma tarefa em um dispositivo e concluí-la em outro. A tarefa permanece o foco central, e os usuários podem fazer seu trabalho no dispositivo que for mais conveniente. Por exemplo, um usuário pode ouvir rádio no seu telefone enquanto estiver no carro, mas quando chegar em casa pode transferir a reprodução para o Xbox One que está conectado ao seu sistema estéreo doméstico.

Você também pode usar o Project Rome para dispositivos complementares ou cenários de controle remoto. Use as APIs de mensagens de serviço de aplicativo para criar um canal de aplicativo entre dois dispositivos e enviar e receber mensagens personalizadas. Por exemplo, você pode criar um aplicativo para o seu telefone que controle a reprodução em sua TV ou um aplicativo complementar que forneça informações sobre os caracteres em um programa de TV que você esteja assistindo por outro aplicativo.  

Os dispositivos podem ser conectados de modo proximal por meio de Bluetooth e conexão sem fio ou remotamente por meio da nuvem; eles são conectados pela conta da Microsoft (MSA) da pessoa que os estiver usando.

Consulte o [Exemplo de Sistemas Remotos UWP](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para obter exemplos de como descobrir o sistema remoto, iniciar um aplicativo em um sistema remoto e usar os serviços de aplicativo para enviar mensagens entre aplicativos em execução em dois sistemas.

Para obter mais informações gerais sobre o Project Rome, incluindo recursos de integração de plataformas, acesse [aka.ms/project-rome](https://aka.ms/project-rome).

| Tópico | Descrição |
|-------|-------------|
| [Iniciar um app em um dispositivo remoto](launch-a-remote-app.md) | Saiba como iniciar um aplicativo em um dispositivo remoto. Este tópico aborda os casos de uso mais simples e a configuração preliminar.  |
| [Descobrir dispositivos remotos](discover-remote-devices.md)  | Saiba como descobrir dispositivos aos quais você pode se conectar. |
| [Comunicar-se com um serviço de app remoto](communicate-with-a-remote-app-service.md) | Saiba como interagir com um app em um dispositivo remoto. |
| [Conectar dispositivos por meio de sessões remotas](remote-sessions.md) | Crie experiências compartilhadas em vários dispositivos ingressando-os em uma sessão remota. |