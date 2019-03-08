---
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: Este artigo mostra como reduzir memória quando o aplicativo é movido para segundo plano.
title: Reduzir o uso da memória quando o aplicativo é movido para o estado em segundo plano
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 28c21b3d3b3e53def2181e96a58b53998ee0f04a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660721"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>Liberar memória quando seu app é movido para o segundo plano

Este artigo mostra como reduzir a quantidade de memória que o aplicativo usa quando é movido para o estado em segundo plano, de maneira que não seja suspenso e possivelmente encerrado.

## <a name="new-background-events"></a>Novos eventos em segundo plano

O Windows 10, versão 1607, apresenta dois novos eventos de ciclo de vida do aplicativo, [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) e [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Esses eventos informam o aplicativo quando ele está entrando no e saindo do segundo plano.

Quando o aplicativo é movido para segundo plano, as restrições de memória impostas pelo sistema podem mudar. Use esses eventos para verificar o consumo de memória atual e liberar recursos para ficar abaixo do limite, de maneira que o aplicativo não seja suspenso e possivelmente encerrado enquanto está no segundo plano.

### <a name="events-for-controlling-your-apps-memory-usage"></a>Eventos para controlar o uso da memória do aplicativo

[MemoryManager.AppMemoryUsageLimitChanging](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelimitchanging.aspx) é acionado pouco antes do limite de memória total que o aplicativo pode usar ser alterado. Por exemplo, quando o aplicativo é movido para o segundo plano e no Xbox o limite de memória muda de 1.024 MB para 128 MB.  
Este é o evento mais importante a ser manipulado para evitar que a plataforma suspenda ou encerre o aplicativo.

[MemoryManager.AppMemoryUsageIncreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusageincreased.aspx) é acionado quando o consumo de memória do aplicativo é aumentado para um valor superior na enumeração [AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.appmemoryusagelevel.aspx). Por exemplo, de **Baixo** para **Médio**. A manipulação desse evento é opcional, mas recomendada porque o aplicativo continua sendo responsável por se manter abaixo do limite.

[MemoryManager.AppMemoryUsageDecreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagedecreased.aspx) é acionado quando o consumo de memória do aplicativo é diminuído para um valor inferior na enumeração **AppMemoryUsageLevel**. Por exemplo, de **Alto** para **Baixo**. A manipulação desse evento é opcional, mas indica que o aplicativo poderá ser capaz de alocar memória adicional, se necessário.

## <a name="handle-the-transition-between-foreground-and-background"></a>Manipular a transição entre primeiro e segundo plano

Quando o aplicativo é movido do primeiro para o segundo plano, o evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) é acionado. Quando o aplicativo retorna ao primeiro plano, o evento [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) é acionado. Você pode registrar manipuladores para esses eventos quando o aplicativo é criado. No modelo de projeto padrão, isso é feito no construtor de classe **App** em App.xaml.cs.

Como a execução em segundo plano reduz os recursos de memória que o aplicativo tem permissão para reter, também é necessário realizar o registro dos eventos [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) e [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging), que podem ser usados para verificar o uso da memória do aplicativo e o limite atuais. Os manipuladores para esses eventos são mostrados nos exemplos a seguir. Para saber mais sobre o ciclo de vida de aplicativos para aplicativos UWP, consulte [Ciclo de vida do aplicativo](..//launch-resume/app-lifecycle.md).

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

Quando o evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) for acionado, defina a variável de rastreamento para indicar que você está em execução em segundo plano. Isso será útil quando você escreverá o código para reduzir o uso da memória.

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

Quando o aplicativo faz a transição para o segundo plano, o sistema reduz o limite de memória para o aplicativo para garantir que o aplicativo em primeiro plano no momento tenha recursos suficientes para proporcionar uma experiência de usuário responsiva.

O manipulador de eventos [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) permite que o aplicativo saiba que sua memória alocada foi reduzida e fornece o novo limite nos argumentos de eventos passados ao manipulador. Compare a propriedade [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage), que fornece o uso atual do aplicativo, com a propriedade [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) dos argumentos de eventos, que especifica o novo limite. Se o uso da memória exceder o limite, você precisará reduzi-lo.

Neste exemplo, isso é feito no método auxiliar **ReduceMemoryUsage**, que é definido posteriormente neste artigo.

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> Algumas configurações de dispositivo permitirão que um aplicativo continue em execução com o novo limite de memória até que o sistema sofra pressão por recursos, mas algumas delas não permitirão. No Xbox especificamente, os aplicativos serão suspensos ou encerrados caso não reduzam o uso de memória para os novos limites dentro de 2 segundos. Isso significa que você pode oferecer a melhor experiência na mais ampla variedade de dispositivos usando esse evento para reduzir o uso de recursos para abaixo do limite, dentro dos dois segundos após o acionamento do evento.

É possível que, embora o uso da memória do aplicativo esteja abaixo do limite de memória para aplicativos em segundo plano quando ocorre a primeira transição para o segundo plano, isso pode aumentar o consumo de memória ao longo do tempo e começar a se aproximar do limite. O manipulador [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) oferece uma oportunidade para verificar o uso quando ele aumenta e, se necessário, liberar espaço na memória.

Verifique se o [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) está **High** ou **OverLimit**e, em caso afirmativo, reduza o uso de memória. Neste exemplo, isso é tratado pelo método auxiliar **ReduceMemoryUsage**. Você também pode se inscrever no evento [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased). Verifique se o aplicativo está abaixo do limite e, em caso afirmativo, você sabe que pode alocar recursos adicionais.

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** é um método auxiliar que pode ser implementado para liberar espaço na memória quando o aplicativo estiver além do limite de uso em execução em segundo plano. A maneira de liberar espaço na memória depende das especificações do seu aplicativo, mas uma forma recomendada para liberar memória é descartar a interface do usuário e outros recursos associados ao modo de exibição do aplicativo. Para isso, certifique-se de que esteja executando no estado em segundo plano e defina a propriedade [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) da janela do aplicativo como `null` e cancele o registro dos manipuladores de eventos da interface do usuário, além de remover todas as outras referências que você possa ter à página. Deixar de cancelar o registro dos manipuladores de eventos da interface do usuário e limpar todas as outras referências que você possa ter à página evitará que os recursos sejam liberados. Em seguida, chame **GC.Collect** para recuperar a memória liberada imediatamente. Normalmente, não imponha coleta de lixo porque o sistema cuidará disso para você. Neste caso específico, estamos reduzindo a quantidade de memória, carregada para este aplicativo conforme ele entra em segundo plano para reduzir a probabilidade de que o sistema determinará que ele deve encerrar o aplicativo para recuperar memória.

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

Quando o conteúdo da janela for coletado, cada quadro começará seu processo de desconexão. Se houver páginas na árvore de objetos visual no conteúdo da janela, elas começarão a acionar o eventos Unloaded. As páginas não podem ser completamente limpas da memória, a menos que todas as referências a elas sejam removidas. No retorno de chamada Unloaded, faça o seguinte para garantir que a memória seja liberada rapidamente:
* Limpe e defina qualquer estrutura de dados grande em sua página como `null`.
* Cancele o registro de todos os manipuladores de eventos que tenham métodos de retorno de chamada dentro da página. Assegure-se de registrar esses retornos de chamada durante a utilização do manipulador de eventos Loaded da página. O evento Loaded é acionado quando a interface do usuário for reconstituída e a página for adicionada à árvore visual do objeto.
* Chame `GC.Collect` no final do retorno da chamada Unloaded para coletar rapidamente o lixo de qualquer uma das estruturas de dados grandes que você definiu como `null`. Mais uma vez, normalmente você não impõe coleta de lixo porque o sistema cuidará disso para você. Neste caso específico, estamos reduzindo a quantidade de memória, carregada para este aplicativo conforme ele entra em segundo plano para reduzir a probabilidade de que o sistema determinará que ele deve encerrar o aplicativo para recuperar memória.

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

No manipulador de eventos [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), defina a variável de rastreamento (`isInBackgroundMode`) para indicar que o aplicativo não está mais em execução em segundo plano. Em seguida, verifique se [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) da janela atual é `null`, o que acontecerá se você tiver descartado os modos de exibição do aplicativo para liberar memória enquanto estava em execução em segundo plano. Se o conteúdo da janela for `null`, recompile o modo de exibição do aplicativo. Neste exemplo, o conteúdo da janela é criado no método auxiliar **CreateRootFrame**.

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

O método auxiliar **CreateRootFrame** recria o conteúdo do modo de exibição para o aplicativo. O código desse método é quase idêntico ao do código do manipulador [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) fornecido no modelo de projeto padrão. A única diferença é que o manipulador **Launching** determina o estado de execução anterior a partir da propriedade [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) do [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) e o método **CreateRootFrame** simplesmente obtém o estado de execução anterior passado como um argumento. Para minimizar o código duplicado, você pode refatorar o código padrão do manipulador de eventos **Launching** para chamar **CreateRootFrame**.

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>Diretrizes

### <a name="moving-from-the-foreground-to-the-background"></a>Mudança do primeiro para o segundo plano

Quando um aplicativo é movido do primeiro para o segundo plano, o sistema funciona em nome do aplicativo para liberar recursos que não sejam necessários no segundo plano. Por exemplo, as estruturas da interface do usuário liberam texturas em cache, e o subsistema de vídeo libera memória alocada em nome do aplicativo. No entanto, um aplicativo ainda precisará monitorar cuidadosamente o uso de memória para evitar ser suspenso ou encerrado pelo sistema.

Quando for movido do primeiro para o segundo plano, um aplicativo receberá primeiramente um evento **EnteredBackground** e, em seguida, um evento **AppMemoryUsageLimitChanging**.

- **Use** o evento **EnteredBackground** para liberar recursos de interface do usuário de que o aplicativo reconhecidamente não precisa durante a execução no segundo plano. Por exemplo, você pode liberar a imagem de arte da capa de uma música.
- **Use** o evento **AppMemoryUsageLimitChanging** para garantir que o aplicativo use menos memória do que o novo limite em segundo plano. Certifique-se de liberar recursos, caso não tenha feito isso. Se você não fizer isso, o aplicativo poderá ser suspenso ou encerrado de acordo com a política de dispositivo específica.
- **Invoque** manualmente o coletor de lixo se o aplicativo estiver acima do novo limite da memória quando o evento **AppMemoryUsageLimitChanging** for acionado.
- **Use** o evento **AppMemoryUsageIncreased** para continuar monitorando o uso de memória do aplicativo em execução no segundo plano se achar que isso deve mudar. Se o **AppMemoryUsageLevel** for **High** ou **OverLimit**, não se esqueça de liberar recursos.
- **Considere** liberar recursos da interface do usuário no manipulador de eventos **AppMemoryUsageLimitChanging** em vez do manipulador **EnteredBackground** como uma otimização de desempenho. Use um valor booliano definido nos manipuladores de eventos **EnteredBackground/LeavingBackground** para acompanhar se o aplicativo está no segundo ou no primeiro plano. Em seguida, no manipulador de eventos **AppMemoryUsageLimitChanging**, se **AppMemoryUsage** estiver acima do limite e o aplicativo estiver no segundo plano (com base no valor booliano), você poderá liberar recursos de interface do usuário.
- **Não** execute operações de longa duração no evento **EnteredBackground** porque você pode fazer com que a transição entre os aplicativos pareça lenta para o usuário.

### <a name="moving-from-the-background-to-the-foreground"></a>Mudança do segundo para o primeiro plano

Quando for movido do segundo para o primeiro plano, o aplicativo receberá primeiramente um evento **AppMemoryUsageLimitChanging** e, em seguida, um evento **LeavingBackground**.

- **Use** o evento **LeavingBackground** para recriar recursos de interface do usuário que o aplicativo descartou ao mover para o segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

* [Amostra de reprodução de mídia em segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=800141) – mostra como liberar memória quando o aplicativo é movido para o estado em segundo plano.
* [Ferramentas de Diagnóstico](https://blogs.msdn.microsoft.com/visualstudioalm/2015/01/16/diagnostic-tools-debugger-window-in-visual-studio-2015/) – use as ferramentas de diagnóstico para observar eventos de coleta de lixo e validar se o aplicativo está liberando memória da maneira esperada.
