---
title: Iniciar um aplicativo em um dispositivo remoto
description: Saiba como iniciar um aplicativo em um dispositivo remoto usando o projeto Roma.
ms.date: 02/12/2018
ms.topic: article
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, projeto Roma
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 26a67816195105572d9f690599b9a880ece90c98
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748291"
---
# <a name="launch-an-app-on-a-remote-device"></a>Iniciar um app em um dispositivo remoto

Este artigo explica como iniciar um aplicativo do Windows em um dispositivo remoto.

A partir do Windows 10, versão 1607, um aplicativo UWP pode iniciar um aplicativo UWP ou um aplicativo da área de trabalho do Windows remotamente em outro dispositivo que também executa o Windows 10, versão 1607 ou posterior, desde que ambos os dispositivos sejam conectados com a mesma conta da Microsoft (MSA). Esse é o caso de uso mais simples do Project Rome.

O recurso de inicialização remota proporciona experiências de usuário orientadas à tarefa, em que um usuário pode iniciar uma tarefa em um dispositivo e concluí-la em outro. Por exemplo, se o usuário estiver ouvindo música pelo telefone no seu carro, ele pode transferir a funcionalidade de reprodução para o Xbox One quando chegar em casa. A inicialização remota permite que os aplicativos passem dados contextuais para o app remoto que será iniciado para retomar a tarefa de onde ela foi interrompida.

## <a name="preliminary-setup"></a>Configuração preliminar

### <a name="add-the-remotesystem-capability"></a>Adicionar funcionalidade remoteSystem

Para seu app iniciar um app em um dispositivo remoto, você deve adicionar a funcionalidade `remoteSystem` ao manifesto do conjunto de aplicativo. Você pode usar o designer de manifesto de pacote para adicioná-lo selecionando Sistema Remoto na guia **Remote System** em **Funcionalidades**, ou adicionar manualmente a linha a seguir ao arquivo _Package.appxmanifest_ do projeto.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Habilite compartilhamento entre dispositivos

Além disso, o dispositivo cliente deve ser definido para permitir o compartilhamento entre dispositivos. Essa configuração, que é acessada em **Configurações**: **Sistema** > **Experiências compartilhadas** > **Compartilhar em todos os dispositivos**, é habilitada por padrão. 

![página de configuração de experiências compartilhadas](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Localizar um dispositivo remoto

Primeiro você deve localizar o dispositivo ao qual deseja se conectar. [Descobrir dispositivos remotos](discover-remote-devices.md) descreve em detalhes como fazer isso. Vamos usar uma abordagem simples aqui que omite a filtragem por tipo de dispositivo ou conectividade. Criaremos um inspetor de sistema remoto que procura por dispositivos remotos e grava manipuladores dos eventos que são acionados quando dispositivos são descobertos ou removidos. Isso nos fornecerá uma coleção de dispositivos remotos.

O código nestes exemplos exige que você tenha uma instrução `using Windows.System.RemoteSystems` no(s) seu(s) arquivo(s) de classe.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

A primeira coisa que você deve fazer antes de fazer uma inicialização remota é chamar `RemoteSystem.RequestAccessAsync()`. Verifique o valor de retorno para confirmar se seu aplicativo tem permissão para acessar dispositivos remotos. Uma razão para essa verificação falhar seria você não ter adicionado a funcionalidade `remoteSystem` ao seu aplicativo.

Os manipuladores de eventos do inspetor de sistema são chamados quando um dispositivo ao qual nos conectamos é descoberto ou não está mais disponível. Usaremos esses manipuladores de eventos para manter uma lista atualizada de dispositivos ao quais podemos nos conectar.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


Rastrearemos os dispositivos por ID de sistema remoto usando um **Dicionário**. Uma **ObservableCollection** é usada para manter a lista de dispositivos que podemos enumerar. Uma **ObservableCollection** também torna fácil associar a lista de dispositivos à interface do usuário, mas não faremos isso neste exemplo.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Adicione uma chamada para `BuildDeviceList()` no código de inicialização do aplicativo antes de tentar iniciar um aplicativo remoto.

## <a name="launch-an-app-on-a-remote-device"></a>Iniciar um app em um dispositivo remoto

Inicie um app remotamente passando o dispositivo ao qual você deseja se conectar à API [**RemoteLauncher.LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx). Há três sobrecargas para esse método. A mais simples, que este exemplo demonstra, especifica o URI que ativará o app no dispositivo remoto. Neste exemplo o URI abre o aplicativo Mapas no computador remoto com uma exibição 3D do Space Needle.

Outras sobrecargas **RemoteLauncher.LaunchUriAsync** permitem especificar opções como o URI do site para visualizar se nenhum app adequado pode ser iniciado no dispositivo remoto e uma lista opcional de nomes da família de pacotes que pode ser usada para iniciar o URI no dispositivo remoto. Você também pode fornecer dados na forma de pares chave/valor. Você pode passar dados ao app que está ativando para fornecer contexto para o app remoto, como o nome da música a ser reproduzida e o local de reprodução atual ao transferir a reprodução de um dispositivo para outro.

Em cenários práticos, você pode fornecer a interface do usuário para selecionar o dispositivo de destino. Mas, para simplificar este exemplo, usaremos apenas o primeiro dispositivo remoto da lista.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

O objeto [**RemoteLaunchUriStatus**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelaunchuristatus.aspx) retornado de **RemoteLauncher.LaunchUriAsync()** fornece informações sobre o êxito da inicialização remota e, em caso negativo, o motivo.

## <a name="related-topics"></a>Tópicos relacionados

[Referência de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Visão geral de aplicativos e dispositivos conectados (projeto Roma)](connected-apps-and-devices.md)  
[Descobrir dispositivos remotos](discover-remote-devices.md)  
[Exemplo de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) demonstra como descobrir um sistema remoto, iniciar um apps em um sistema remoto e usar os serviços de apps para enviar mensagens entre apps em execução em dois sistemas.
