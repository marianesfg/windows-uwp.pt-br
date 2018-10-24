---
author: TylerMSFT
title: Executar em segundo plano indefinidamente
description: Use a funcionalidade extendedExecutionUnconstrained para executar uma tarefa em segundo plano ou a sessão de execução estendida em segundo plano indefinidamente.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: tarefa em segundo plano, execução estendida, recursos, limites, tarefa em segundo plano
ms.author: twhitney
ms.date: 10/3/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: af0f7670f2b131671ce82708d2b0a826db0fcfb1
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475827"
---
# <a name="run-in-the-background-indefinitely"></a>Executar em segundo plano indefinidamente

Para fornecer a melhor experiência para os usuários, o Windows impõe limites de recursos em aplicativos da Plataforma Universal do Windows (UWP). Aplicativos em primeiro plano recebem mais memória e tempo de execução; aplicativos em segundo plano recebem menos. Os usuários, portanto, são protegidos de desempenho ruim de aplicativo em primeiro plano e muito uso bateria.

No entanto, os desenvolvedores que criam aplicativos UWP para uso pessoal (ou seja, aplicativos de sideload que não serão publicados na Microsoft Store), ou os desenvolvedores que criam aplicativos UWP empresariais, talvez queiram usar todos os recursos disponíveis no dispositivo sem qualquer tela de fundo ou limitação de execução estendida. Linha de negócios e aplicativos UWP pessoais podem usar APIs no Windows Creators Update (versão 1703) para desativar a otimização. Lembre-se de que você não pode colocar um aplicativo na Microsoft Store se ele usar essas APIs.

## <a name="run-while-minimized"></a>Executar enquanto minimizado

Aplicativos UWP movem-se para um estado suspenso quando eles não estão em execução em primeiro plano. Na área de trabalho, isso ocorre quando um usuário minimiza o aplicativo. Aplicativos usam uma sessão de execução estendida para continuar em execução enquanto minimizados. APIs de execução estendida que são aceitos pela Microsoft Store estão detalhados em [Adiar a suspensão do aplicativo com execução estendida](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution).

Se você estiver desenvolvendo um aplicativo que não se destina a ser enviado para a Microsoft Store, você pode usar a funcionalidade restrita [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) com o `extendedExecutionUnconstrained` para que seu aplicativo possa continuar a ser executado enquanto minimizado, independentemente do estado de energia do dispositivo.  

A funcionalidade `extendedExecutionUnconstrained` é adicionada como um recurso restrito no manifesto do aplicativo. Consulte [Declarações de funcionalidades do aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para obter mais informações sobre recursos restritos.

_Package.appxmanifest_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

Quando você usar as funcionalidades `extendedExecutionUnconstrained`, [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) e [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) são usadas em vez de [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) e [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). O mesmo padrão para criar a sessão, definir membros e solicitar a extensão de forma assíncrona ainda se aplica: 

```cs
var newSession = new ExtendedExecutionForegroundSession();  
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;  
newSession.Description = "Long Running Processing";  
newSession.Revoked += SessionRevoked;  
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();  
switch (result)  
{  
    case ExtendedExecutionResult.Allowed:  
        DoLongRunningWork();  
        break;  

    default:  
    case ExtendedExecutionResult.Denied:  
        DoShortRunningWork();  
        break;  
}
```

Você pode solicitar essa sessão de execução estendida assim que o aplicativo for fornecido para o primeiro plano. As sessões de execução estendida irrestritas não estão limitadas por cotas de energia ou pela economia de bateria do sistema operacional. Existe uma referência ao objeto de sessão, desde que o aplicativo fique no estado de execução e não entre no estado suspenso. Se o aplicativo for fechado pelo usuário, a sessão será revogada.

Registrar-se para o evento **Revoked** permitirá que seu aplicativo realize qualquer trabalho de limpeza necessário. No estado de suspensão, você pode criar uma sessão de execução estendida com [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) para salvar os dados do usuário antes do aplicativo ser encerrado e removido da memória.

## <a name="run-background-tasks-indefinitely"></a>Executar tarefas em segundo plano de forma indefinida

Na Plataforma Universal do Windows, tarefas em segundo plano são processos executados em segundo plano sem qualquer forma de interface do usuário. Tarefas em segundo plano geralmente podem ser executadas por um máximo de 25 segundos antes de serem canceladas. Algumas das tarefas de execução mais longas também têm uma verificação para garantir que a tarefa em segundo plano não está inutilizada ou usando memória. Na Atualização do Windows para Criadores (versão 1703), a funcionalidade restrita [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) foi introduzida para remover esses limites. A funcionalidade **extendedBackgroundTaskTime** é adicionada como um recurso restrito no arquivo do manifesto do aplicativo:

_Package.appxmanifest_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

Essa funcionalidade remove as limitações de tempo de execução e o watchdog de tarefa ociosa. Após o início de uma tarefa em segundo plano por um gatilho ou uma chamada de serviço de aplicativo, depois de receber um adiamento [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) fornecido pelo método **Executar**, ele pode ser executado indefinidamente. Se o aplicativo for definido como **Gerenciado pelo Windows**, em seguida, ele ainda pode ter uma cota de energia aplicada a si e a suas tarefas em segundo plano não serão ativadas quando a economia de bateria estiver ativa.Isso pode ser alterado com configurações do sistema operacional. Mais informações estão disponíveis em [Otimizando atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

A Plataforma Universal do Windows monitora a execução de tarefas em segundo plano para garantir uma experiência de aplicativo em primeiro plano suave e com boa duração da bateria. No entanto, aplicativos pessoais e aplicativos de linha de negócios corporativos podem usar execução estendida e a funcionalidade **extendedBackgroundTaskTime** de criar aplicativos que serão executados, conforme necessário, independentemente da disponibilidade de recursos do dispositivo.

Lembre-se de que as funcionalidades **extendedExecutionUnconstrained** e **extendedBackgroundTaskTime** podem substituir a política padrão para aplicativos UWP e causar uso significativo da bateria. Antes de usar esses recursos, primeiro confirme que o padrão de execução estendido e políticas de tempo de tarefa em segundo plano não atendem às suas necessidades e realize testes em condições de restrição de bateria para entender o impacto que seu aplicativo terá em um dispositivo.

## <a name="see-also"></a>Veja também

[Remover as restrições de recurso de tarefa em segundo plano](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)
