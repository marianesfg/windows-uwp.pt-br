---
author: TylerMSFT
title: "Início, retomada e tarefas em segundo plano"
description: "Esta seção descreve o que acontece quando um aplicativo UWP (Plataforma Universal do Windows) é iniciado, suspenso, retomado e encerrado."
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.sourcegitcommit: a8e6145f7a5c75d3b37277b80b07b0b3ad739d5c
ms.openlocfilehash: ab20c4af5b9a87dc73775d304c314c9861d989d4

---

# Início, retomada e tarefas em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Esta seção descreve o que acontece quando um aplicativo UWP (Plataforma Universal do Windows) é iniciado, suspenso, retomado e encerrado. Ela detalha como ativar aplicativos usando um contrato ou uma extensão e como usar tarefas em segundo plano que permitem que um aplicativo UWP faça o trabalho mesmo quando o aplicativo não está em primeiro plano. Por fim, ela detalha como adicionar uma tela inicial ao seu aplicativo.

## O ciclo de vida do aplicativo

| Tópico                                            | Descrição                                                                                                     |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Ciclo de vida do aplicativo](app-lifecycle.md)               | Saiba mais sobre o ciclo de vida de um aplicativo UWP e o que acontece quando o Windows inicia, suspende e retoma seu aplicativo. |
| [Manipular a pré-inicialização de aplicativos](handle-app-prelaunch.md) | Saiba como manipular o pré-lançamento do aplicativo.                                                                              |
| [Manipular a ativação do aplicativo](activate-an-app.md)     | Saiba como manipular a ativação do aplicativo.                                                                             |
| [Manipular a suspensão do aplicativo](suspend-an-app.md)         | Saiba como salvar dados de aplicativo importantes quando o sistema suspende o seu aplicativo.                                 |
| [Manipular a retomada do aplicativo](resume-an-app.md)           | Saiba como atualizar o conteúdo exibido quando o sistema retomar o aplicativo.                                        |

 

## Iniciar aplicativos


| URI e ativação de arquivos                                                                         | Descrição                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Iniciar um aplicativo para obter resultados](how-to-launch-an-app-for-results.md)                               | Saiba como iniciar um aplicativo a partir de outro aplicativo e trocar dados entre os dois.                                                                                             |
| [Iniciar o aplicativo padrão para um URI](launch-default-app.md)                                      | Saiba como iniciar o aplicativo padrão para um URI (Uniform Resource Identifier).                                                                                               |
| [Manipular a ativação do URI](handle-uri-activation.md)                                              | Saiba como registrar um aplicativo para ser o manipulador padrão de um nome de esquema de URI.                                                                                          |
| [Iniciar o aplicativo padrão para um arquivo](launch-the-default-app-for-a-file.md)                      | Saiba como iniciar o aplicativo padrão para um tipo de arquivo.                                                                                                                       |
| [Manipular a ativação de arquivos](handle-file-activation.md)                                            | Saiba como registrar seu aplicativo para ser o manipulador padrão de um tipo de arquivo.                                                                                                  |
| [Diretrizes de tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321) | Entendendo a relação entre os aplicativos UWP e os tipos de arquivo e protocolos compatíveis, você pode oferecer uma experiência mais consistente e refinada a seus usuários. |
| [Arquivo reservado e nomes de esquemas de URI](reserved-uri-scheme-names.md)                             | Este tópico lista os arquivos reservados e os nomes de esquemas de URI que não estão disponíveis para seu aplicativo.                                                                                |
| [Iniciar o aplicativo Configurações do Windows](launch-settings-app.md)                                      | Saiba como iniciar o aplicativo Configurações do Windows.                                                                                                                              |
| [Iniciar o aplicativo da Windows Store](launch-store-app.md)                                            | Saiba como iniciar o aplicativo da Windows Store.                                                                                                                                 |
| [Iniciar o aplicativo Mapas do Windows](launch-maps-app.md)                                              | Saiba como iniciar o aplicativo Mapas do Windows.                                                                                                                                  |

 

## Serviços e tarefas em segundo plano



| Tópico                                                                                                            | Descrição                                                                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md)                             | Os tópicos nesta seção mostram como executar seu próprio código leve em segundo plano ao responder a gatilhos com tarefas em segundo plano.                                                       |
| [Acessar sensores e dispositivos a partir de uma tarefa em segundo plano](access-sensors-and-devices-from-a-background-task.md)       | [
              **DeviceUseTrigger**
            ](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que seu aplicativo Universal do Windows acesse sensores e dispositivos periféricos em segundo plano, mesmo quando seu aplicativo em primeiro plano estiver suspenso. |
| [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)                                           | Verifique se seu aplicativo atende aos requisitos para executar tarefas em segundo plano.                                                                                                                          |
| [Criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md)                                | Saiba como escrever um aplicativo UWP que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços.                                                                                  |
| [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md)                               | Crie uma classe de tarefa em segundo plano e a registre para ser executada quando seu aplicativo não estiver em primeiro plano.                                                                                                 |
| [Depurar uma tarefa em segundo plano](debug-a-background-task.md)                                                           | Aprenda a depurar uma tarefa em segundo plano, incluindo ativação e rastreamento de depuração de tarefas em seguindo plano no log de eventos do Windows.                                                                        |
| [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md) | Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo.                                                                                                       |
| [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)                                     | Saiba como criar uma tarefa em segundo plano que reconhece solicitações de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente.                                     |
| [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)           | Saiba como o aplicativo pode reconhecer o progresso e a conclusão de uma tarefa em segundo plano.                                                                                                                     |
| [Registrar uma tarefa em segundo plano](register-a-background-task.md)                                                     | Aprenda a criar uma função que pode ser reutilizada para registrar com segurança a maioria das tarefas em segundo plano.                                                                                                  |
| [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)             | Saiba como criar uma tarefa em segundo plano que responda a eventos do [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).                                                                         |
| [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)                                        | Aprenda a agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano.                                                                                                          |
| [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)                 | Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.                                                                                                                  |
| [Transferir dados em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | Use a API de transferência em segundo plano para copiar arquivos em segundo plano.                                                                                                                              |
| [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)                       | Use uma tarefa em segundo plano para atualizar o bloco dinâmico de seu aplicativo com novo conteúdo.                                                                                                                      |
| [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)                                                       | Saiba como usar a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para executar um código leve em segundo plano enquanto o dispositivo estiver conectado.                             |

 

## Adicionar uma tela inicial


Todos os aplicativos UWP devem ter uma tela inicial composta de uma imagem de tela inicial e uma cor da tela de fundo, e ambas podem ser personalizadas.

Sua tela inicial é exibida imediatamente quando o usuário inicia seu aplicativo. Isso proporciona uma resposta imediata aos usuários enquanto os recursos do aplicativo são inicializados. Assim que o aplicativo estiver pronto para interação, a tela inicial será ignorada.

Uma tela inicial bem projetada pode tornar seu aplicativo mais atraente. Aqui está uma tela inicial simples e sutil:

![uma captura de tela em escala de 75% da tela inicial da amostra de tela inicial.](images/regularsplashscreen.png)

Essa tela inicial foi criada combinando uma cor da tela de fundo verde com um PNG transparente.

A junção de imagem e cor de tela de fundo para formar a tela inicial contribui para uma boa aparência da tela inicial, independentemente do dispositivo em que o aplicativo foi instalado. Quando a tela inicial é exibida, apenas o tamanho da tela de fundo muda para compensar os vários tamanhos de tela. A imagem permanece sempre intacta.

Além disso, você pode usar a classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) para personalizar a experiência de inicialização do seu aplicativo. Você pode posicionar uma tela inicial estendida, criada por você, para dar ao aplicativo mais tempo para concluir tarefas adicionais, como a preparação da interface do usuário do aplicativo ou a conclusão de operações de rede. Você também pode usar a classe **SplashScreen** como notificação para quando a tela inicial for ignorada, para que você possa dar início às animações de entrada.

| Tópico                                                                          | Descrição                                                                                                                                                                                       |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Adicionar uma tela inicial](add-a-splash-screen.md)                                 | Defina a imagem e a cor de fundo da tela inicial do seu aplicativo.                                                                                                                                          |
| [Exibir uma tela inicial por mais tempo](create-a-customized-splash-screen.md) | Exiba uma tela inicial por mais tempo criando uma tela inicial estendida para o seu aplicativo. Essa tela estendida imita a tela inicial exibida quando o aplicativo é iniciado e pode ser personalizada. |

 

 

 



<!--HONumber=Jun16_HO5-->


