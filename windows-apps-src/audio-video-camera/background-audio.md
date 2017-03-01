---
author: drewbatgit
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: "Este artigo mostra como reproduzir mídia enquanto seu app é executado em segundo plano."
title: "Reproduzir mídia em segundo plano"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 6251ff13e6cc751ad370a43950cfdbb9dca0ecc8
ms.lasthandoff: 02/08/2017

---

# <a name="play-media-in-the-background"></a>Reproduzir mídia em segundo plano
Este artigo mostra como configurar seu app para que a mídia continue a ser reproduzida quando o app for movido do primeiro para o segundo plano. Isso significa que, mesmo depois que o usuário minimizar o app, retornar à tela inicial ou sair do app de alguma outra forma, o app poderá continuar a reproduzir o áudio. 

Os cenários de reprodução de áudio em segundo plano incluem:

-   **Playlists de longa duração:** o usuário ativa brevemente o app em primeiro plano para selecionar e iniciar uma playlist, esperando que depois disso a playlist continue sendo reproduzida em segundo plano.

-   **Uso do alternador de tarefas:** o usuário ativa brevemente um app em primeiro plano para iniciar a reprodução de áudio e, em seguida, alterna para outro app aberto usando o alternador de tarefas. O usuário espera que o áudio continue sendo reproduzido em segundo plano.

A implementação de áudio em segundo plano descrita neste artigo permitirá que seu app seja executado universalmente em todos os dispositivos Windows, incluindo dispositivos móveis, desktop e Xbox.

> [!NOTE]
> O código neste artigo foi adaptado da [Amostra de áudio em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=800141) da UWP.

## <a name="explanation-of-one-process-model"></a>Explicação do modelo de um processo
Com o Windows 10, versão 1607, foi introduzido um novo modelo de processo único que simplifica significativamente o processo para habilitar áudio em segundo plano. Anteriormente, era obrigatório que o app gerenciasse um processo em segundo plano além do app em primeiro plano e comunicasse manualmente as mudanças de estado entre os dois processos. Com o novo modelo, você simplesmente adiciona a funcionalidade de áudio em segundo plano ao manifesto do app e ele continuará reproduzindo áudio automaticamente quando for movido para o segundo plano. Dois novos eventos de ciclo de vida do app, [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) e [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) permitem que o app saiba quando ele entra em segundo plano e sai dele. Quando o app move para as transições de ou para o segundo plano, as restrições de memória impostas pelo sistema podem mudar, portanto, você pode usar esses eventos para verificar o consumo de memória atual e liberar recursos para ficar abaixo do limite.

Eliminando a complexa comunicação entre processos e o gerenciamento de estados, o novo modelo permite que você implemente áudio em segundo plano de forma muito mais rápida, com uma redução significativa no código. No entanto, o modelo de dois processos ainda tem suporte na versão atual para a compatibilidade com versões anteriores. Para saber mais, consulte [Modelo de áudio em segundo plano herdado](legacy-background-media-playback.md).

## <a name="requirements-for-background-audio"></a>Requisitos para áudio em segundo plano
O app deve atender aos seguintes requisitos para reprodução de áudio enquanto ele estiver em segundo plano.

* Adicione a funcionalidade **Reprodução de Mídia em Segundo Plano** ao manifesto do app, conforme descrito mais adiante neste artigo.
* Se o app desabilita a integração automática do **MediaPlayer** com o Controle de transporte de mídia do sistema (SMTC), como ao definir a propriedade [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como falsa, então é obrigatório implementar a integração manual com o SMTC para habilitar a reprodução de mídia em segundo plano. Você deve também integrar manualmente o SMTC se estiver usando uma API que não seja **MediaPlayer**, por exemplo [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), para reproduzir áudio se quiser que o áudio continue sendo reproduzido quando o app for movido para o segundo plano. Os requisitos mínimos de integração do SMTC são descritos na seção "Usar os controles de transporte de mídia do sistema para áudio em segundo plano" do [Controle manual dos controles de transporte de mídia do sistema](system-media-transport-controls.md).
* Enquanto o app estiver em segundo plano, você deve permanecer dentro dos limites de uso de memória definidos pelo sistema para apps em segundo plano. Neste artigo, são fornecidas orientações para o gerenciamento de memória enquanto o app estiver em segundo plano.

## <a name="background-media-playback-manifest-capability"></a>Recurso do manifesto de reprodução de mídia em segundo plano
Para habilitar o áudio em segundo plano, você deve adicionar o recurso de reprodução de mídia em segundo plano ao arquivo de manifesto do app, Package.appxmanifest. 

**Para adicionar funcionalidades ao manifesto do app usando o designer de manifesto**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do app clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa de seleção **Reprodução de Mídia em Segundo Plano**.

Para definir o recurso editando manualmente o xml de manifesto do app, primeiro verifique se o prefixo do namespace *uap3* está definido no elemento **Package**. Se não estiver, adicione-o conforme mostrado abaixo.
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

##<a name="handle-transitioning-between-foreground-and-background"></a>Tratar a transição entre primeiro e segundo plano
Quando o app é movido do primeiro para o segundo plano, o evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) é acionado. E quando o app retorna ao primeiro plano, o evento [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) é acionado. Como eles são eventos de ciclo de vida do app, é obrigatório registrar manipuladores para esses eventos quando o app for criado. No modelo de projeto padrão, isso significa adicioná-los ao construtor de classe **App** em App.xaml.cs. 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Crie uma variável para controlar se você está em execução em segundo plano.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Quando o evento [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) for acionado, defina a variável de rastreamento para indicar que você está em execução em segundo plano. Você não deve executar tarefas de longa duração no evento **EnteredBackground** porque isso pode fazer com que a transição para o segundo plano pareça lenta para o usuário.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

No manipulador de eventos [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), você deve definir a variável de rastreamento para indicar que o app não está mais em execução em segundo plano.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>Requisitos de gerenciamento de memória
A parte mais importante de manipulação da transição entre primeiro e segundo plano é o gerenciamento da memória que seu app usa. Como a execução em segundo plano reduz os recursos de memória que seu app tem permissão para reter pelo sistema, você também deve ser registrado para os eventos [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) e [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging). Quando esses eventos são gerados, você deve verificar o uso atual da memória e o limite atual do seu app e, em seguida, reduzir o uso de memória, se necessário. Para obter informações sobre como reduzir o uso da memória durante a execução em segundo plano, consulte [Liberar memória quando seu app é movido para o segundo plano](../launch-resume/reduce-memory-usage.md).

## <a name="network-availability-for-background-media-apps"></a>Disponibilidade da rede para apps de mídia em segundo plano
Todas as fontes de mídia com reconhecimento de rede, aquelas que não são criadas de um fluxo ou de um arquivo, manterão a conexão de rede ativa enquanto recuperam conteúdo remoto, e a liberarão quando não estiverem recuperando conteúdo remoto. [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource), especificamente, depende do app reportar corretamente o intervalo de buffer correto à plataforma, usando [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762). Depois que todo o conteúdo for armazenado totalmente em buffer, a rede não será mais reservada em nome do app.

Se você precisar fazer chamadas de rede que ocorrem em segundo plano quando a mídia não estiver sendo baixada, elas deverão ser encapsuladas em uma tarefa apropriada como [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger), [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) ou [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger). Para saber mais, consulte [Dar suporte a seu app com tarefas em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar ao Controle de Transporte de Mídia do Sistema](integrate-with-systemmediatransportcontrols.md)
* [Amostra de áudio em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 





