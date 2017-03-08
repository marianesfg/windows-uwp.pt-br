---
author: TylerMSFT
title: Aplicativos e dispositivos conectados (Project &quot;Rome&quot;)
description: "Esta seção descreve como usar a plataforma de Sistemas Remotos para descobrir dispositivos remotos, iniciar um app em um dispositivo remoto e comunicar-se com um serviço de app em um dispositivo remoto."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 357cf459fffc46e5d77f316af881e1a6e8bd8867
ms.lasthandoff: 02/08/2017

---

# <a name="connected-apps-and-devices-project-rome"></a>Aplicativos e dispositivos conectados (Project "Rome")

Esta seção explica como conectar apps entre dispositivos e plataformas usando o Project "Rome". Saiba como descobrir dispositivos remotos, iniciar um app em um dispositivo remoto e comunicar-se com um serviço de app em um dispositivo remoto.

A maioria das pessoas tem vários dispositivos e muitas vezes começam uma atividade em um dispositivo e a concluem em outro. Para acomodar isso, os apps precisam estendem a dispositivos e plataformas.

As [APIs de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) introduzidas no Windows 10, versão 1607, permitem que você crie apps que permitem aos usuários iniciar uma tarefa em um dispositivo e concluí-la em outro. A tarefa permanece o foco central, e os usuários podem fazer seu trabalho no dispositivo que for mais conveniente. Por exemplo, você pode ouvir rádio no seu telefone no carro, mas quando chegar em casa pode transferir a reprodução para o Xbox One que está associado ao seu sistema estéreo doméstico.

Você também pode usar o projeto "Roma" para dispositivos complementares ou cenários de controle remoto. Use as APIs de mensagens de app para criar um canal de app entre dois dispositivos e enviar e receber mensagens personalizadas. Por exemplo, você pode criar um app para o seu telefone que controle a reprodução em sua TV ou um app complementar que forneça informações sobre os caracteres em um programa de TV que você esteja assistindo em outro app.  

Os dispositivos podem ser conectados de modo proximal por meio de Bluetooth e conexão sem fio ou remotamente por meio da nuvem, e eles são conectados pela conta da Microsoft da pessoa que os estiver usando.

Consulte o [Exemplo de Sistemas Remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para obter exemplos de como descobrir o sistema remoto, iniciar um app em um sistema remoto e usar os serviços de app para enviar mensagens entre apps em execução em dois sistemas.

| Tópico | Descrição |
|-------|-------------|
| [Descobrir dispositivos remotos](discover-remote-devices.md)  | Saiba como descobrir dispositivos aos quais você pode se conectar. |
| [Iniciar um app em um dispositivo remoto](launch-a-remote-app.md) | Saiba como iniciar um app em um dispositivo remoto.  |
| [Comunicar-se com um serviço de app remoto](communicate-with-a-remote-app-service.md) | Saiba como interagir com um app em um dispositivo remoto. |

