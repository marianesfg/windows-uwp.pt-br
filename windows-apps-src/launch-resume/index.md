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
ms.openlocfilehash: 6eed2ecb5766edea8678ea2af10c02332d5a5928
ms.sourcegitcommit: a61e9fc06f74dc54c36abf7acb85eeb606e475b8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2017
---
# <a name="launching-resuming-and-background-tasks"></a>Início, retomada e tarefas em segundo plano

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção inclui informações sobre os itens a seguir:

- O que acontece quando um aplicativo UWP (Plataforma Universal do Windows) é iniciado, suspenso, retomado e encerrado.
- Como iniciar apps por URI ou ativação de arquivo.
- Como usar serviços de app, que permitem que seu aplicativo UWP (Plataforma Universal do Windows) compartilhe dados e funcionalidades com outros apps.
- Como usar tarefas em segundo plano, que permitem que um aplicativo UWP funcione mesmo quando o próprio app não estiver em primeiro plano.
- Como descobrir dispositivos conectados, iniciar um app em outro dispositivo e comunicar-se com um serviço de app em um dispositivo remoto para que você possa criar experiências de usuário que fluam de um dispositivo para outro.
- Como escolher a tecnologia adequada para estender e dividir o aplicativo.
- Como adicionar e configurar uma tela inicial para seu app.

## <a name="the-app-lifecycle"></a>O ciclo de vida do app

Esta seção detalha o ciclo de vida de um aplicativo UWP (Plataforma Universal do Windows) do Windows 10, do momento em que ele é ativado até quando é fechado.

| Tópico | Descrição |
|-------|-------------|
| [Ciclo de vida do app](app-lifecycle.md)               | Saiba mais sobre o ciclo de vida de um aplicativo UWP e o que acontece quando o Windows inicia, suspende e retoma seu app. |
| [Manipular a pré-inicialização de aplicativos](handle-app-prelaunch.md) | Saiba como manipular o pré-lançamento do aplicativo.                                                                              |
| [Manipular a ativação do aplicativo](activate-an-app.md)     | Saiba como manipular a ativação do aplicativo.                                                                             |
| [Manipular a suspensão do aplicativo](suspend-an-app.md)         | Saiba como salvar dados de aplicativo importantes quando o sistema suspende o seu aplicativo.                                 |
| [Tratar a retomada do app](resume-an-app.md)           | Saiba como atualizar o conteúdo exibido quando o sistema retomar o aplicativo.                                        |
| [Liberar memória quando seu app é movido para o segundo plano](reduce-memory-usage.md) | Saiba como reduzir a quantidade de memória usada pelo aplicativo quando ele está em segundo plano, de maneira que ele não seja encerrado.|
| [Executar enquanto minimizado com execução estendida](run-minimized-with-extended-execution.md) | Saiba como usar a execução estendida para manter o aplicativo em execução quando minimizado |

## <a name="launch-apps"></a>Iniciar aplicativos

A seção [Iniciar um aplicativo com um URI](launch-app-with-uri.md) detalha como usar um URI (Uniform Resource Identifier) para iniciar um aplicativo com base em outro aplicativo.

| Tópico | Descrição |
|-------|-------------|
| [Iniciar o app padrão para um URI](launch-default-app.md) | Saiba como iniciar o app padrão para um URI (Uniform Resource Identifier). Os URIs permitem iniciar outro aplicativo para realizar uma tarefa específica. Este tópico também apresenta uma visão geral dos muitos esquemas de URI compilados no Windows. |
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
| [Iniciando automaticamente com a Reprodução Automática](auto-launching-with-autoplay.md) | Você pode usar a Reprodução Automática para fornecer seu aplicativo como uma opção quando um usuário conecta um dispositivo ao computador. Isso inclui dispositivos sem volume, como uma câmera ou um player de mídia, ou dispositivos com volume, como pen drives, cartões de memória ou DVDs. |

## <a name="app-services"></a>Serviços de app

A seção [Serviços de app](app-services.md) descreve como integrar serviços de app ao seu aplicativo UWP para permitir o compartilhamento de dados e funcionalidades entre apps.

| Tópico | Descrição |
|-------|-------------|
| [Criar e consumir um serviço de app](how-to-create-and-consume-an-app-service.md) | Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços. |
| [Converter um serviço de app para ser executado no mesmo processo de seu app host](convert-app-service-in-process.md) | Converta o código de serviço de app executado em um processo separado em segundo plano em código que é executado no mesmo processo de seu provedor de serviços de app. |
| [Estender seu aplicativo com serviços de aplicativo, extensões e pacotes](extend-your-app-with-services-extensions-packages.md) | Há diferentes tecnologias no Windows 10 que ajudam você a estender e separar os componentes do aplicativo. Este tópico ajudará você a determinar qual tecnologia é adequada para usar e fornecer uma breve visão geral de cada um. |

## <a name="background-tasks"></a>Tarefas em segundo plano

A seção [Tarefas em segundo plano](support-your-app-with-background-tasks.md) mostra como fazer o código leve ser executado em segundo plano em resposta a gatilhos.

| Tópico | Descrição |
|-------|-------------|
| [Diretrizes de tarefas em segundo plano](guidelines-for-background-tasks.md)                                       | Verifique se seu aplicativo atende aos requisitos para executar tarefas em segundo plano. |
| [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md)       | Crie e registre uma tarefa em segundo plano que é executada no mesmo processo de seu aplicativo em primeiro plano. |
| [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)           | Crie e registre uma tarefa em segundo plano que é executada em um processo separado do seu aplicativo e registre-a para ser executada quando o aplicativo não estiver em primeiro plano. |
| [Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo](convert-out-of-process-background-task.md) | Saiba como converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo que é executada no mesmo processo do seu aplicativo em primeiro plano.|
| [Depurar uma tarefa em segundo plano](debug-a-background-task.md)                                                       | Aprenda a depurar uma tarefa em segundo plano, incluindo ativação e rastreamento de depuração de tarefas em seguindo plano no log de eventos do Windows. |
| [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md) | Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo. |
| [Registro de tarefa de grupo em segundo plano](group-background-tasks.md)                                             | Isole o registro de tarefa em segundo plano com grupos. |
| [Processar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)                                 | Saiba como criar uma tarefa em segundo plano que reconhece solicitações de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente. |
| [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)       | Saiba como o aplicativo pode reconhecer o progresso e a conclusão de uma tarefa em segundo plano. |
| [Registrar uma tarefa em segundo plano](register-a-background-task.md)                                                 | Aprenda a criar uma função que pode ser reutilizada para registrar com segurança a maioria das tarefas em segundo plano. |
| [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)         | Saiba como criar uma tarefa em segundo plano que responda a eventos do [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). |
| [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)                                    | Saiba como agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano. |
| [Disparar uma tarefa em segundo plano no aplicativo](trigger-background-task-from-app.md) | Saiba como usar o [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para ativar uma tarefa em segundo plano no aplicativo.|
| [Acessar sensores e dispositivos a partir de uma tarefa em segundo plano](access-sensors-and-devices-from-a-background-task.md)   | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que o aplicativo universal do Windows acesse sensores e dispositivos periféricos em segundo plano, mesmo quando o aplicativo em primeiro plano estiver suspenso. |
| [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)             | Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada. |
| [Transferir dados em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt280377)                 | Use a API de transferência em segundo plano para copiar arquivos em segundo plano. |
| [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)                   | Use uma tarefa em segundo plano para atualizar o bloco dinâmico de seu aplicativo com novo conteúdo. |
| [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)                                                   | Saiba como usar a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para executar um código leve em segundo plano enquanto o dispositivo estiver conectado. |
### <a name="see-also"></a>Veja também
* [Otimizar a atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) – saiba como reduzir a energia usada em segundo plano e interagir com as configurações do usuário para atividade em segundo plano.

## <a name="remote-systems"></a>Sistemas Remotos

A seção [Aplicativos e dispositivos conectados (Project Rome)](connected-apps-and-devices.md) descreve como usar a plataforma de Sistemas Remotos para descobrir dispositivos remotos, iniciar um aplicativo e comunicar-se com um serviço de aplicativo em um dispositivo remoto.

| Tópico | Descrição |
|-------|-------------|
| [Descobrir dispositivos remotos](discover-remote-devices.md)  | Saiba como descobrir dispositivos aos quais você pode se conectar. |
| [Iniciar um app em um dispositivo remoto](launch-a-remote-app.md) | Saiba como iniciar um aplicativo em um dispositivo remoto.  |
| [Comunicar-se com um serviço de app remoto](communicate-with-a-remote-app-service.md) | Saiba como interagir com um app em um dispositivo remoto. |

## <a name="splash-screens"></a>Telas iniciais

A seção [Telas iniciais](splash-screens.md) descreve como definir e configurar a tela inicial de seu app.

| Tópico | Descrição |
|-------|-------------|
| [Adicionar uma tela inicial](add-a-splash-screen.md) | Defina a imagem e a cor de fundo da tela inicial do seu app. |
| [Exibir uma tela inicial por mais tempo](create-a-customized-splash-screen.md) | Exiba uma tela inicial por mais tempo criando uma tela inicial estendida para o seu aplicativo. Essa tela estendida imita a tela inicial exibida quando o aplicativo é iniciado e pode ser personalizada. |
