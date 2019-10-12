---
title: Prioridades de notificação do WNS
description: Descrição das várias prioridades que você pode definir em uma notificação
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, API do WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 3310b34b2748bd684e46e04775c973680f8e03a9
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282246"
---
# <a name="wns-notification-priorities"></a>Prioridades de notificação do WNS
Ao definir a prioridade de uma notificação com um cabeçalho simples para mensagens de POSTAgem do WNS, você pode controlar como as notificações são entregues em situações sensíveis à bateria.

## <a name="power-on-windows"></a>Ligar o Windows
À medida que mais usuários estiverem trabalhando somente em dispositivos com bateria, minimizar o consumo de energia se tornará um requisito padrão para todos os aplicativos. Se os aplicativos consumirem mais energia do que o valor que eles fornecem, os usuários poderão desinstalar os aplicativos. Embora o sistema operacional Windows reduza o consumo de energia na bateria sempre que possível, é responsabilidade do aplicativo trabalhar com eficiência. 

As prioridades do WNS são uma maneira de mover o trabalho não crítico para fora da bateria. As prioridades do WNS dizem ao sistema que as notificações devem ser entregues instantaneamente e que podem esperar até que o dispositivo seja conectado a uma fonte de energia. Com essas dicas, o sistema pode entregar as notificações no momento exato em que elas são as mais valiosas para o usuário e o aplicativo. 

## <a name="power-modes-on-the-device"></a>Modos de energia no dispositivo
Cada dispositivo do Windows opera por meio de uma variedade de modos de energia (bateria, economia de bateria e cobrança), e os usuários esperam comportamentos diferentes de aplicativos em modos de energia diferentes. Quando o dispositivo estiver ativado, todas as notificações deverão ser entregues. No modo de economia de bateria, somente as notificações mais importantes devem ser entregues. Enquanto o dispositivo está conectado, as operações de sincronização ou não críticas podem ser concluídas.

O Windows não sabe quais notificações são importantes para qualquer usuário ou aplicativo, portanto, o sistema depende totalmente dos aplicativos para definir a prioridade certa para suas notificações. 

## <a name="priorities"></a>Suas
Há quatro prioridades disponíveis para um aplicativo usar ao enviar notificações por push. A prioridade é definida em notificações individuais, permitindo que você escolha quais notificações precisam ser entregues instantaneamente (por exemplo, uma mensagem de mensagens instantâneas) e quais podem esperar (por exemplo, contatar atualizações de fotos).

As prioridades são: 

|    Priority    |    Substituição do usuário    |    Descrição    |    Exemplo    |
|----------------|---------------------|-------------------|---------------|
|    Alto    |    Sim – o usuário pode bloquear todas as notificações de um aplicativo ou pode impedir que um aplicativo seja limitado no modo de economia de bateria.    |    As notificações mais importantes que devem ser entregues imediatamente em qualquer circunstância quando o dispositivo puder receber notificações. Coisas como chamadas de VoIP ou alertas críticos que devem ativar o dispositivo se enquadram nessa categoria.    |    Chamadas VoIP, alertas críticos para o tempo    |
|    Média    |    Sim – o usuário pode bloquear todas as notificações de um aplicativo ou pode impedir que um aplicativo seja limitado no modo de economia de bateria.    |    Essas são as coisas que não são tão importantes, coisas que não precisam acontecer imediatamente, mas os usuários seriam incomodar se não estiverem em execução em segundo plano.    |    Sincronização de conta de email secundária, atualizações de bloco dinâmico.    |
|    Baixa    |    Sim – o usuário pode bloquear todas as notificações de um aplicativo ou pode impedir que um aplicativo seja limitado no modo de economia de bateria.    |    Notificações que fazem sentido apenas quando o usuário está usando o dispositivo ou quando a atividade em segundo plano faz sentido. Eles são armazenados em cache e não processados até que o usuário entre ou conecte seu dispositivo.    |    Status do contato (online/offline)    |
|    Muito baixo     |    Não, não é possível impedir que notificações de prioridade muito baixa sejam limitadas no modo de economia de bateria.    |    Isso é quase o mesmo que a baixa prioridade, exceto que os usuários não podem substituir a política de economia de bateria. Essas notificações nunca serão entregues na economia de bateria.    |    Sincronizando arquivos para um serviço de sincronização.    |

Observe que muitos aplicativos terão notificações de prioridade diferente em todo o ciclo de vida. Como a prioridade é definida em uma base por notificação, isso não é um problema. Um aplicativo VoIP pode enviar uma notificação de alta prioridade para uma chamada de entrada e, em seguida, acompanhá-la com uma prioridade baixa quando um contato ficar online. 

## <a name="setting-the-priority"></a>Definindo a prioridade

A definição da prioridade na solicitação de notificação é feita por meio de um cabeçalho adicional na solicitação POST, `X-WNS-PRIORITY`. Este é um valor inteiro entre 1 e 4 que é mapeado para uma prioridade: 

| Nome da prioridade | X-WNS-valor de prioridade | Padrão para: |
|---------------|----------------------|------------------|
| Alto | 1 | Notificações do sistema |
| Severidade | 2 | Blocos e notificações |
| Baixa | 3 | Bruta |
| Muito baixo | 4 |  |

Para ser compatível com versões anteriores, não é necessário definir uma prioridade. Caso um aplicativo não defina a prioridade de suas notificações, o sistema fornecerá uma prioridade padrão. Os padrões são mostrados no gráfico acima e correspondem ao comportamento das versões existentes do Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Listagem detalhada do comportamento da área de trabalho 

Se você estiver enviando seu aplicativo por vários SKUs diferentes do Windows, normalmente será melhor seguir o gráfico na seção acima. 

Os comportamentos recomendados mais específicos para cada prioridade são listados abaixo. Isso não é uma garantia de que cada dispositivo funcionará exatamente de acordo com o gráfico. Os OEMs são livres para configurar o comportamento de forma diferente, mas a maioria está perto deste gráfico. 

| Status do dispositivo    | PRIORIDADE Alto    |    PRIORIDADE Média        | PRIORIDADE Baixa    |    PRIORIDADE Muito baixo    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Tela ligada ou conectada    |    Enviá    |    Enviá    |    Enviá    |    Enviá    |
|    Tela desligada e bateria    |    Enviá    |    Se o usuário tiver isentado: enviar mais: lote     |    Se o usuário tiver isentado: entregar else: cache *    |    Cache    |
|    Economia de bateria habilitada    |    Se o usuário tiver isentado: entregar else: cache    |    Se o usuário tiver isentado: entregar else: cache    |    Se o usuário tiver isentado: entregar else: cache    |    Cache     |
|    Na bateria + economia de bateria habilitada + tela desativada    |    Se o usuário tiver isentado: entregar else: cache    |    Se o usuário tiver isentado: entregar else: cache    |    Se o usuário tiver isentado: entregar else: cache    |    Cache    |

Observe que as notificações de baixa prioridade serão entregues por padrão para desativar a tela e a bateria somente para dispositivos com base Windows Phone. Isso é para a compatibilidade maintian com a política MPNS pré-existente. Observe também que as quarta e quinta linhas são as mesmas, apenas chamando cenários diferentes.

Para isentar um aplicativo na economia de bateria, os usuários devem acessar o "uso da bateria por aplicativo" em configurações e selecionar "permitir que o aplicativo Execute tarefas em segundo plano". Essa seleção de usuário isenta o aplicativo da economia de bateria para notificações de prioridade alta, média e baixa. Você também pode chamar a [API BackgroundExecutionManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) para solicitar programaticamente a permissão do usuário.  

## <a name="related-topics"></a>Tópicos relacionados
- [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Solicitando permissão para execução em segundo plano](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
