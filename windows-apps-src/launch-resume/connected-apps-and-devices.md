---
author: TylerMSFT
title: Aplicativos e dispositivos conectados (projeto &quot;Roma&quot;)
description: "Esta seção descreve como usar o projeto &quot;Roma&quot; para descobrir dispositivos conectados, iniciar um aplicativo em outro dispositivo e se comunicar com um aplicativo em um dispositivo remoto."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: 4f49acfd7efcb10d99f9d23884d20c0fc51e5a4a

---

# Aplicativos e dispositivos conectados (projeto "Roma")

Esta seção explica como conectar aplicativos entre dispositivos e plataformas usando o projeto "Roma". Saiba como descobrir dispositivos conectados, iniciar um aplicativo em outro dispositivo e se comunicar com um aplicativo em um dispositivo remoto.

A maioria das pessoas tem vários dispositivos e muitas vezes começam uma atividade em um dispositivo e a concluem em outro. Para acomodar isso, os aplicativos precisam estendem a dispositivos e plataformas.

As [APIs de sistemas remotos](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems) introduzidas no Windows 10, versão 1607, permitem que você crie aplicativos que permitem aos usuários iniciar uma tarefa em um dispositivo e concluí-la em outro. A tarefa permanece o foco central, e os usuários podem fazer seu trabalho no dispositivo que for mais conveniente. Por exemplo, você pode ouvir rádio no seu telefone no carro, mas quando chegar em casa pode transferir a reprodução para o Xbox One que está associado ao seu sistema estéreo doméstico.

Você também pode usar o projeto "Roma" para dispositivos complementares ou cenários de controle remoto. Use as APIs de mensagens de aplicativo para criar um canal de aplicativo entre dois dispositivos e enviar e receber mensagens personalizadas. Por exemplo, você pode criar um aplicativo para o seu telefone que controle a reprodução em sua TV ou um aplicativo complementar que forneça informações sobre os caracteres em um programa de TV que você esteja assistindo em outro aplicativo.  

Os dispositivos podem ser conectados de modo proximal por meio de Bluetooth e conexão sem fio ou remotamente por meio da nuvem, e eles são conectados pela conta da Microsoft da pessoa que os estiver usando.

Consulte o [Exemplo de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para obter exemplos de como descobrir o sistema remoto, iniciar um aplicativo em um sistema remoto e usar os serviços de aplicativo para enviar mensagens entre aplicativos em execução em dois sistemas.

| Atividade remota | Descrição                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Descobrir dispositivos remotos](discover-remote-devices.md)  | Saiba como descobrir dispositivos aos quais você pode se conectar. |
| [Iniciar um aplicativo em um dispositivo remoto](launch-a-remote-app.md) | Saiba como iniciar um aplicativo em um dispositivo remoto.  |
| [Comunicar-se com um serviço de aplicativo remoto](communicate-with-a-remote-app-service.md) | Saiba como interagir com um aplicativo em um dispositivo remoto. |



<!--HONumber=Aug16_HO5-->


