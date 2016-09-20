---
author: drewbatgit
ms.assetid: 
description: "Este artigo mostra como reproduzir mídia enquanto seu aplicativo é executado em segundo plano."
title: "Reproduzir mídia em segundo plano"
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# Reproduzir mídia em segundo plano
Este artigo mostra como configurar seu aplicativo para que a mídia continue a ser reproduzida quando o aplicativo for movido do primeiro para o segundo plano. Isso significa que, mesmo depois que o usuário minimizar o aplicativo, retornar à tela inicial ou sair do aplicativo de alguma outra forma, o aplicativo poderá continuar a reproduzir o áudio. 

Os cenários de reprodução de áudio em segundo plano incluem:

-   **Playlists de longa duração:** o usuário ativa brevemente o aplicativo em primeiro plano para selecionar e iniciar uma playlist, esperando que depois disso a playlist continue sendo reproduzida em segundo plano.

-   **Uso do alternador de tarefas:** o usuário ativa brevemente um aplicativo em primeiro plano para iniciar a reprodução de áudio e, em seguida, alterna para outro aplicativo aberto usando o alternador de tarefas. O usuário espera que o áudio continue sendo reproduzido em segundo plano.

A implementação de áudio em segundo plano descrita neste artigo permitirá que seu aplicativo seja executado universalmente em todos os dispositivos Windows, incluindo dispositivos móveis, desktop e Xbox.

> [!NOTE]
> O código neste artigo foi adaptado da [Amostra de áudio em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=800141) da UWP.

## Explicação do modelo de um processo
Com o Windows 10, versão 1607, foi introduzido um novo modelo de processo único que simplifica significativamente o processo para habilitar áudio em segundo plano. Anteriormente, era necessário que o aplicativo gerenciasse um processo em segundo plano além do aplicativo em primeiro plano e comunicasse manualmente as mudanças de estado entre os dois processos. Com o novo modelo, você simplesmente adiciona a funcionalidade de áudio em segundo plano ao manifesto do aplicativo e ele continuará reproduzindo áudio automaticamente quando for movido para o segundo plano. Dois novos eventos de ciclo de vida do aplicativo, [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) e [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) permitem que o aplicativo saiba quando ele entra em segundo plano e sai dele. Quando o aplicativo move para as transições de ou para o segundo plano, as restrições de memória impostas pelo sistema podem mudar, portanto, você pode usar esses eventos para verificar o consumo de memória atual e liberar recursos para ficar abaixo do limite.

Eliminando a complexa comunicação entre processos e o gerenciamento de estados, o novo modelo permite que você implemente áudio em segundo plano de forma muito mais rápida, com uma redução significativa no código. No entanto, o modelo de dois processos ainda tem suporte na versão atual para a compatibilidade com versões anteriores. Para saber mais, consulte [Modelo de áudio em segundo plano herdado](background-audio.md).

## Requisitos para áudio em segundo plano
O aplicativo deve atender aos seguintes requisitos para reprodução de áudio enquanto ele estiver em segundo plano.

* Adicione a funcionalidade **Reprodução de Mídia em Segundo Plano** ao manifesto do aplicativo, conforme descrito mais adiante neste artigo.
* Se o aplicativo desabilita a integração automática do **MediaPlayer** com o Controle de transporte de mídia do sistema (SMTC), como ao definir a propriedade [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como falsa, então é necessário implementar a integração manual com o SMTC para habilitar a reprodução de mídia em segundo plano. Você deve também integrar manualmente o SMTC se estiver usando uma API que não seja **MediaPlayer**, por exemplo [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), para reproduzir áudio se quiser que o áudio continue sendo reproduzido quando o aplicativo for movido para o segundo plano. Os requisitos mínimos de integração do SMTC são descritos na seção "Usar os controles de transporte de mídia do sistema para áudio em segundo plano" do [Controle manual dos controles de transporte de mídia do sistema](system-media-transport-controls.md).
* Enquanto o aplicativo estiver em segundo plano, você deve permanecer dentro dos limites de uso de memória definidos pelo sistema para aplicativos em segundo plano. Neste artigo, são fornecidas orientações para o gerenciamento de memória enquanto o aplicativo estiver em segundo plano.

## Recurso do manifesto de reprodução de mídia em segundo plano
Para habilitar o áudio em segundo plano, você deve adicionar o recurso de reprodução de mídia em segundo plano ao arquivo de manifesto do aplicativo, Package.appxmanifest. 

**Para adicionar funcionalidades ao manifesto do aplicativo usando o designer de manifesto**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa de seleção **Reprodução de Mídia em Segundo Plano**.

Para definir o recurso editando manualmente o xml de manifesto do aplicativo, primeiro verifique se o prefixo do namespace *uap3* está definido no elemento **Package**. Se não estiver, adicione-o conforme mostrado abaixo.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

Em seguida, adicione o recurso *backgroundMediaPlayback* ao elemento **Capabilities**:
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

##Tratar a transição entre primeiro e segundo plano
Quando o aplicativo é movido do primeiro para o segundo plano, o evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) é acionado. E quando o aplicativo retorna ao primeiro plano, o evento [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) é acionado. Como eles são eventos de ciclo de vida do aplicativo, é necessário registrar manipuladores para esses eventos quando o aplicativo for criado. No modelo de projeto padrão, isso significa adicioná-los ao construtor de classe **App** em App.xaml.cs. Como a execução em segundo plano reduz os recursos de memória que o aplicativo tem permissão para reter pelo sistema, também é necessário realizar o registro dos eventos [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) e [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging), que serão usados para verificar o uso da memória do aplicativo e o limite atuais. Os manipuladores para esses eventos são mostrados nos exemplos a seguir. Para saber mais sobre o ciclo de vida de aplicativos para aplicativos UWP, consulte [Ciclo de vida do aplicativo](../\launch-resume\app-lifecycle.md).

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Crie uma variável para controlar se você está em execução em segundo plano.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Quando o evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) for acionado, defina a variável de rastreamento para indicar que você está em execução em segundo plano. Você não deve executar tarefas de longa duração no evento **EnteredBackground** porque isso pode fazer com que a transição para o segundo plano pareça lenta para o usuário.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Quando o aplicativo faz a transição para o segundo plano, o limite de memória para o aplicativo é reduzido pelo sistema para garantir que o aplicativo em primeiro plano no momento tenha recursos suficientes para proporcionar uma experiência de usuário responsiva. O manipulador de eventos [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) permite que o aplicativo saiba que sua memória alocada foi reduzida e fornece o novo limite nos argumentos de eventos passados ao manipulador. Compare a propriedade [**Appmemoryusage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage), que fornece o uso atual do aplicativo, com a propriedade [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) dos argumentos de eventos, que especifica o novo limite. Se o uso da memória exceder o limite, você precisará reduzi-lo. Neste exemplo, isso é feito no método auxiliar **ReduceMemoryUsage**, que é definido posteriormente neste artigo.

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> Algumas configurações de dispositivo permitirão que um aplicativo continue em execução com o novo limite de memória até que o sistema sofra pressão por recursos, mas algumas delas não permitirão. No Xbox especificamente, os aplicativos serão suspensos ou encerrados caso não reduzam o uso de memória para os novos limites dentro de 2 >>> segundos. Isso significa que você pode oferecer a melhor experiência na mais ampla variedade de dispositivos usando esse evento para reduzir o uso de recursos para abaixo do limite, dentro dos dois segundos após o acionamento do evento.


É possível que quando o aplicativo faça a transição para o segundo plano pela primeira vez, seu uso de memória esteja abaixo do limite de memória para aplicativos em segundo plano, mas, em algum momento posterior, ele aumente sua utilização e comece a se aproximar do limite. O manipulador [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) oferece uma oportunidade para verificar o uso quando ele aumenta e, se necessário, liberar espaço na memória. Verifique se o [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) está **High** ou **OverLimit**e, em caso afirmativo, reduza o uso de memória. Mais uma vez, neste exemplo, esse processo é tratado pelo método auxiliar **ReduceMemoryUsage**. Você também pode se inscrever no evento [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased). Verifique se o aplicativo está abaixo do limite e, em caso afirmativo, você sabe que pode alocar recursos adicionais.

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** é um método auxiliar que pode ser implementado para liberar espaço na memória quando o aplicativo estiver além do limite de uso para aplicativos em execução em segundo plano. A maneira de liberar espaço na memória depende das especificações do seu aplicativo, mas uma forma recomendada para liberar memória é descartar a interface do usuário e outros recursos associados ao modo de exibição do aplicativo. Primeiro, assegure-se de que você está no modo de execução em segundo plano e, em seguida, defina a propriedade [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) da janela do aplicativo como nula. Chame **GC.Collect** para instruir o sistema para recuperar a memória liberada imediatamente.

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

Quando o conteúdo da janela for coletado, cada quadro começará seu processo de desconexão. Se houver páginas na árvore visual de objetos sob o conteúdo da janela, elas começarão a acionar seus eventos [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded). As páginas não podem ser completamente limpas da memória, a menos que todas as referências a elas sejam removidas. No retorno de chamada **Unloaded**, faça o seguinte para garantir que a memória seja liberada rapidamente:
* Limpe e defina qualquer estrutura de dados grande em sua página como nula.
* Cancele o registro de todos os manipuladores de eventos que tenham métodos de retorno de chamada dentro da página. Assegure-se de registrar esses retornos de chamada durante a utilização do manipulador de eventos Loaded da página. O evento Loaded é acionado quando a interface do usuário for reconstituída e a página for adicionada à árvore visual do objeto.
* Chame **GC.Collect** no final do retorno da chamada Unloaded para coletar rapidamente o lixo de qualquer uma das estruturas de dados grandes que você definiu como nulas.

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

No manipulador de eventos [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), você deve definir a variável de rastreamento para indicar que o aplicativo não está mais em execução em segundo plano. Em seguida, verifique se [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) da janela atual é nulo, o que acontecerá se você tiver descartado os modos de exibição do aplicativo para liberar memória enquanto estava em execução em segundo plano. Se o conteúdo da janela for nulo, recompile o modo de exibição do aplicativo. Neste exemplo, o conteúdo da janela é criado no método auxiliar **CreateRootFrame**.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

O método auxiliar **CreateRootFrame** recria o conteúdo do modo de exibição para o aplicativo. O código desse método é idêntico ao do código do manipulador [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) fornecido no modelo de projeto padrão. A única diferença é que o manipulador **Launching** determina o estado de execução anterior a partir da propriedade [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) do [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) e o método **CreateRootFrame** simplesmente obtém o estado de execução anterior passado como um argumento. Para minimizar o código duplicado, você pode refatorar o código padrão do manipulador de eventos **Launching** para chamar **CreateRootFrame**, se quiser.

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## Disponibilidade da rede para aplicativos de mídia em segundo plano
Todas as fontes de mídia com reconhecimento de rede, aquelas que não são criadas de um fluxo ou de um arquivo, manterão a conexão de rede ativa enquanto recuperam conteúdo remoto, e a liberarão quando não estiverem recuperando conteúdo remoto. [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource), especificamente, depende do aplicativo reportar corretamente o intervalo de buffer correto à plataforma, usando [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762). Depois que todo o conteúdo for armazenado totalmente em buffer, a rede não será mais reservada em nome do aplicativo.

Se você precisar fazer chamadas de rede que ocorrem em segundo plano quando a mídia não estiver sendo baixada, elas deverão ser encapsuladas em uma tarefa apropriada como [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger), [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) ou [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger). Para saber mais, consulte [Dar suporte a seu aplicativo com tarefas em segundo plano](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar ao Controle de Transporte de Mídia do Sistema](integrate-with-systemmediatransportcontrols.md)
* [Amostra de áudio em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


