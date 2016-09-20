---
author: TylerMSFT
title: Iniciar um aplicativo em um dispositivo remoto
description: Saiba como iniciar um aplicativo em um dispositivo remoto usando o projeto &quot;Roma&quot;.
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: d8c3783d68a1b3b216058790d84255a7fb4b612c

---

# Iniciar um aplicativo em um dispositivo remoto

Este artigo explica como iniciar um aplicativo da Plataforma Universal do Windows (UWP) ou um aplicativo da área de trabalho do Windows em um dispositivo remoto.

A partir do Windows 10, versão 1607, um aplicativo UWP pode iniciar um aplicativo UWP ou um aplicativo da área de trabalho do Windows remotamente em outro dispositivo que também tem o Windows 10, versão 1607 ou posterior.

Um cenário para iniciar um aplicativo em um dispositivo remoto é permitir que um usuário inicie uma tarefa em um dispositivo e a conclua em outro. Por exemplo, se você estiver ouvindo música em seu telefone no caminho para casa, poderia usar a inicialização remota para transferir a reprodução para seu Xbox quando chegar em casa. Você pode transmitir dados para o aplicativo remoto para fornecer contexto para o aplicativo remoto continuar a tarefa.

## Adicionar a funcionalidade RemoteSystem

Para seu aplicativo iniciar um aplicativo em um dispositivo remoto, você deve adicionar a funcionalidade <uap3:Capability Name="remoteSystem"/> ao manifesto do pacote do aplicativo. Você pode usar o designer de manifesto de pacote para adicioná-lo selecionando **Sistema Remoto** na guia **Recursos**, ou pode fazer manualmente o que o designer de manifesto do pacote faria e adicionar o seguinte ao arquivo Package.appxmanifest.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## Localizar um dispositivo remoto

Primeiro você deve localizar o dispositivo ao qual deseja se conectar. [Descobrir dispositivos remotos](discover-remote-devices.md) descreve em detalhes como fazer isso. Vamos usar uma abordagem simples aqui que omite a filtragem por tipo de dispositivo ou conectividade. Criaremos um inspetor de sistema remoto que procura por dispositivos remotos e grava manipuladores de eventos que são notificados quando dispositivos que usam a mesma conta da Microsoft são descobertos ou removidos. Isso nos fornecerá uma coleção de dispositivos remotos.

O código nestes exemplos pressupõe que você tenha uma instrução `using Windows.System.RemoteSystems` no seu arquivo.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

A primeira coisa que você deve fazer antes de fazer uma inicialização remota é chamar `RemoteSystem.RequestAccessAsync()`. Verifique o valor de retorno para confirmar se seu aplicativo tem permissão para acessar dispositivos remotos. Uma razão para essa verificação falhar seria você não ter adicionado a funcionalidade `remoteSystem` ao seu aplicativo.

Os manipuladores de eventos do inspetor de sistema são chamados quando um dispositivo ao qual nos conectamos é descoberto ou não está mais disponível. Usaremos esses manipuladores de eventos para manter uma lista atualizada de dispositivos ao quais podemos nos conectar.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

Rastreamos os dispositivos por ID de sistema remoto usando um dicionário. Uma ObservableCollection é usada para manter a lista de dispositivos que podemos enumerar. Uma ObservableCollection também torna fácil associar a lista de dispositivos à interface do usuário, mas não faremos isso neste exemplo.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Adicione uma chamada para `BuildDeviceList()` no código de inicialização do aplicativo antes de tentar iniciar um aplicativo remoto.

## Iniciar um aplicativo em um dispositivo remoto

Inicie um aplicativo remotamente passando o dispositivo ao qual você deseja se conectar à API [RemoteLauncher.LaunchUri](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx). Há três sobrecargas para essa função. A mais simples, que este exemplo demonstra, especifica o URI que ativará o aplicativo no dispositivo remoto. Neste exemplo o URI abre o aplicativo Mapas no computador remoto com uma exibição 3D do Space Needle.

Outras sobrecargas **RemoteLauncher.LaunchUriAsync** permitem que você especifique opções, como o URI do site para ver se um aplicativo que pode manipular o URI não pode ser iniciado no dispositivo remoto e uma lista de nomes de famílias de pacotes que pode ser usada para iniciar o URI do dispositivo remoto. Você também pode fornecer dados na forma de pares de chave/valor. Você pode transmitir dados para o aplicativo que está ativando no dispositivo remoto para fornecer contexto para o aplicativo remoto, como o nome da música a ser reproduzida e o local de reprodução atual ao transferir a reprodução de um dispositivo para outro.

No uso real, você pode fornecer a interface do usuário para selecionar o dispositivo que deseja usar. Mas, para simplificar neste exemplo, usaremos apenas o primeiro da lista para fazer a chamada remota.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

O [RemoteLaunchUriStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelaunchuristatus.aspx) que é retornado de **RemoteLauncher.LaunchUriAsync()** fornece informações sobre se a inicialização remota foi bem-sucedida e, se não foi, o motivo.

## Tópicos relacionados

[Referência de API de sistemas remotos](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[Visão geral de aplicativos e dispositivos conectados (projeto "Roma")](connected-apps-and-devices.md)  
[Exemplo de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) demonstra como descobrir um sistema remoto, iniciar um aplicativo em um sistema remoto e usar os serviços de aplicativo para enviar mensagens entre aplicativos em execução em dois sistemas.



<!--HONumber=Aug16_HO5-->


