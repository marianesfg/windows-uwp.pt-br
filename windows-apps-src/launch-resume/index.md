---
author: TylerMSFT
title: "Início, retomada e tarefas em segundo plano"
description: "Esta seção descreve o que acontece quando um app UWP (Plataforma Universal do Windows) é iniciado, suspenso, retomado e encerrado."
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 40400b8b17da9b010c0f03e9976201a1def025f8
ms.lasthandoff: 02/07/2017

---

# <a name="launching-resuming-and-background-tasks"></a>Início, retomada e tarefas em segundo plano

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção inclui informações sobre os itens a seguir:

- O que acontece quando um app UWP (Plataforma Universal do Windows) é iniciado, suspenso, retomado e encerrado.
- Como iniciar apps por URI ou ativação de arquivo.
- Como usar serviços de app, que permitem que seu app UWP (Plataforma Universal do Windows) compartilhe dados e funcionalidades com outros apps.
- Como usar tarefas em segundo plano, que permitem que um app UWP funcione mesmo quando o próprio app não estiver em primeiro plano.
- Como descobrir dispositivos conectados, iniciar um app em outro dispositivo e comunicar-se com um serviço de app em um dispositivo remoto para que você possa criar experiências de usuário que fluam de um dispositivo para outro.
- Como adicionar e configurar uma tela inicial para seu app.

## <a name="the-app-lifecycle"></a>O ciclo de vida do app

Esta seção detalha o ciclo de vida de um app UWP (Plataforma Universal do Windows) do Windows 10, do momento em que ele é ativado até quando é fechado.

| Tópico | Descrição |
|-------|-------------|
| [Ciclo de vida do app](app-lifecycle.md)               | Saiba mais sobre o ciclo de vida de um app UWP e o que acontece quando o Windows inicia, suspende e retoma seu app. |
| [Manipular a pré-inicialização de apps](handle-app-prelaunch.md) | Saiba como manipular o pré-lançamento do app.                                                                              |
| [Manipular a ativação do app](activate-an-app.md)     | Saiba como manipular a ativação do app.                                                                             |
| [Manipular a suspensão do app](suspend-an-app.md)         | Saiba como salvar dados de app importantes quando o sistema suspende o seu app.                                 |
| [Tratar a retomada do app](resume-an-app.md)           | Saiba como atualizar o conteúdo exibido quando o sistema retomar o app.                                        |
| [Liberar memória quando seu app é movido para o segundo plano](reduce-memory-usage.md) | Saiba como reduzir a quantidade de memória usada pelo app quando ele está em segundo plano, de maneira que ele não seja encerrado.|
| [Executar enquanto minimizado com execução estendida](run-minimized-with-extended-execution.md) | Saiba como usar a execução estendida para manter o app em execução quando minimizado |

## <a name="launch-apps"></a>Iniciar apps

A seção [Iniciar um app com um URI](launch-app-with-uri.md) detalha como usar um URI (Uniform Resource Identifier) para iniciar um app com base em outro app.

| Tópico | Descrição |
|-------|-------------|
| [Iniciar o app padrão de um URI](launch-default-app.md) | Saiba como iniciar o app padrão para um URI (Uniform Resource Identifier). Os URIs permitem iniciar outro app para realizar uma tarefa específica. Este tópico também apresenta uma visão geral dos muitos esquemas de URI compilados no Windows. |
| [Tratar a ativação do URI](handle-uri-activation.md) | Saiba como registrar um app para ser o manipulador padrão de um nome de esquema de URI (Uniform Resource Identifier). |
| [Iniciar um app para obter resultados](how-to-launch-an-app-for-results.md) | Saiba como iniciar um app a partir de outro app e trocar dados entre os dois. Isso é chamado de "iniciar" um app para obter resultados. |
| [Escolher e salvar tons usando o esquema de URI ms-tonepicker](launch-ringtone-picker.md) | Este tópico descreve o esquema de URI ms-tonepicker e como usá-lo para exibir um seletor de tom para selecionar e salvar um tom e obter o nome amigável para ele. |
| [Iniciar o app Configurações do Windows](launch-settings-app.md) | Saiba como iniciar o app Configurações do Windows a partir de seu app. Este tópico descreve o esquema de URI ms-settings. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas. |
| [Iniciar o aplicativo da Windows Store](launch-store-app.md) | Este tópico descreve o esquema de URI ms-windows-store. Seu app pode usar esse esquema de URI para iniciar o aplicativo da Windows Store para páginas específicas na Loja. |
| [Iniciar o app Mapas do Windows](launch-maps-app.md) | Saiba como iniciar o app Mapas do Windows a partir de seu app. |
| [Iniciar o app Pessoas](launch-people-apps.md) | Este tópico descreve o esquema de URI ms-people. Seu app pode usar esse esquema de URI para iniciar o app Pessoas para ações específicas. |
| [Dar suporte à vinculação de apps à Web com manipuladores de URI de apps](web-to-app-linking.md) | Promover o envolvimento do usuário com seu app usando os manipuladores de URI de apps. |

A seção [Iniciar um app por meio de ativação de arquivo](launch-app-from-file.md) detalha como configurar seu app para ser inicializado quando um arquivo de determinado tipo for aberto.

| Tópico | Descrição |
|-------|-------------|
| [Iniciar o app padrão para um arquivo](launch-the-default-app-for-a-file.md) | Aprenda como iniciar o app padrão para um arquivo. |
| [Tratar a ativação do arquivo](handle-file-activation.md) | Saiba como registrar seu app para ser o manipulador padrão de um determinado tipo de arquivo. |

Veja outros tópicos relacionados à inicialização de um app abaixo.

| Tópico | Descrição |
|-------|-------------|
| [Arquivo reservado e nomes de esquemas de URI](reserved-uri-scheme-names.md) | Este tópico lista os arquivos reservados e os nomes de esquemas de URI que não estão disponíveis para seu app. |
| [Iniciando automaticamente com a Reprodução Automática](auto-launching-with-autoplay.md) | Você pode usar a Reprodução Automática para fornecer seu app como uma opção quando um usuário conecta um dispositivo ao computador. Isso inclui dispositivos sem volume, como câmeras ou players de mídia, ou dispositivos com volume, como pen drives, cartões SD ou DVDs. |

## <a name="app-services"></a>Serviços de app

A seção [Serviços de app](app-services.md) descreve como integrar serviços de app ao seu app UWP para permitir o compartilhamento de dados e funcionalidades entre apps.

| Tópico | Descrição |
|-------|-------------|
| [Criar e consumir um serviço de app](how-to-create-and-consume-an-app-service.md) | Saiba como escrever um app UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros apps UWP e também como consumir esses serviços. |
| [Converter um serviço de app para ser executado no mesmo processo de seu app host](convert-app-service-in-process.md) | Converta o código de serviço de app executado em um processo separado em segundo plano em código que é executado no mesmo processo de seu provedor de serviços de app. |

## <a name="background-tasks"></a>Tarefas em segundo plano

A seção [Tarefas em segundo plano](support-your-app-with-background-tasks.md) mostra como fazer o código leve ser executado em segundo plano em resposta a gatilhos.

| Tópico | Descrição |
|-------|-------------|
| [Acessar sensores e dispositivos a partir de uma tarefa em segundo plano](access-sensors-and-devices-from-a-background-task.md)       | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que seu aplicativo universal do Windows acesse sensores e dispositivos periféricos em segundo plano, mesmo quando seu app em primeiro plano estiver suspenso. |
| [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)                                           | Verifique se seu app atende aos requisitos para executar tarefas em segundo plano.                                                                                                                          |
| [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)                               | Crie e registre uma tarefa em segundo plano que é executada em um processo separado do seu app e registre-a para ser executada quando o app não estiver em primeiro plano.                                                                                                 |
| [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)                               | Crie e registre uma tarefa em segundo plano que é executada no mesmo processo de seu app em primeiro plano.                                                                                                 |
| [Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo](convert-out-of-process-background-task.md)                               | Saiba como converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo que é executada no mesmo processo do seu app em primeiro plano.
| [Depurar uma tarefa em segundo plano](debug-a-background-task.md)                                                           | Aprenda a depurar uma tarefa em segundo plano, incluindo ativação e rastreamento de depuração de tarefas em seguindo plano no log de eventos do Windows.                                                                        |
| [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md) | Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do app.                                                                                                       |
| [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)                                     | Saiba como criar uma tarefa em segundo plano que reconhece solicitações de cancelamento e interrompe o trabalho, relatando o cancelamento ao app usando armazenamento persistente.                                     |
| [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)           | Saiba como o app pode reconhecer o progresso e a conclusão de uma tarefa em segundo plano.                                                                                                                     |
| [Registrar uma tarefa em segundo plano](register-a-background-task.md)                                                     | Aprenda a criar uma função que pode ser reutilizada para registrar com segurança a maioria das tarefas em segundo plano.                                                                                                  |
| [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)             | Saiba como criar uma tarefa em segundo plano que responda a eventos do [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).                                                                         |
| [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)                                        | Aprenda a agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano.                                                                                                          |
| [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)                 | Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.                                                                                                                  |
| [Transferir dados em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | Use a API de transferência em segundo plano para copiar arquivos em segundo plano.                                                                                                                              |
| [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)                       | Use uma tarefa em segundo plano para atualizar o bloco dinâmico de seu app com novo conteúdo.                                                                                                                      |
| [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)                                                       | Saiba como usar a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para executar um código leve em segundo plano enquanto o dispositivo estiver conectado.                             |

## <a name="remote-systems"></a>Sistemas remotos

A seção [Apps e dispositivos conectados (Project "Rome")](connected-apps-and-devices.md) descreve como usar a plataforma de Sistemas Remotos para descobrir dispositivos remotos, iniciar um app em um dispositivo remoto e comunicar-se com um serviço de app em um dispositivo remoto.

| Tópico | Descrição |
|-------|-------------|
| [Descobrir dispositivos remotos](discover-remote-devices.md)  | Saiba como descobrir dispositivos aos quais você pode se conectar. |
| [Iniciar um app em um dispositivo remoto](launch-a-remote-app.md) | Saiba como iniciar um app em um dispositivo remoto.  |
| [Comunicar-se com um serviço de app remoto](communicate-with-a-remote-app-service.md) | Saiba como interagir com um app em um dispositivo remoto. |

## <a name="splash-screens"></a>Telas iniciais

A seção [Telas iniciais](splash-screens.md) descreve como definir e configurar a tela inicial de seu app.

| Tópico | Descrição |
|-------|-------------|
| [Adicionar uma tela inicial](add-a-splash-screen.md) | Defina a imagem e a cor de fundo da tela inicial do seu app. |
| [Exibir uma tela inicial por mais tempo](create-a-customized-splash-screen.md) | Exiba uma tela inicial por mais tempo criando uma tela inicial estendida para o seu app. Essa tela estendida imita a tela inicial exibida quando o app é iniciado e pode ser personalizada. |

