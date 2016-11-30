---
author: PatrickFarley
title: Descobrir dispositivos remotos
description: Learn how to discover remote devices from your app using Project "Rome".
translationtype: Human Translation
ms.sourcegitcommit: dc5030fea8e149b3926b926b09bfec3eeb5092ee
ms.openlocfilehash: b4b4eb28ea56ab3d84e2fe0bc0c281cb53149d5d

---

# Descobrir dispositivos remotos
Seu aplicativo pode usar a rede sem fio, Bluetooth e uma conexão na nuvem para descobrir dispositivos Windows que são assinados com a mesma conta da Microsoft que o dispositivo de descoberta. Os dispositivos públicos que podem aceitar conexões anônimas, como o Surface Hub e o Xbox One, também são detectáveis. Os dispositivos remotos não precisam ter nenhum software especial instalado para ser descoberto.

> [!NOTE]
> Este guia considera que você já tenha obtido acesso ao recurso Sistemas Remotos seguindo as etapas em [Iniciar um aplicativo remoto](launch-a-remote-app.md).

## Filtrar o conjunto de dispositivos detectáveis
Você pode reduzir o conjunto de dispositivos detectáveis usando um [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) com filtros. Os filtros podem detectar o tipo de detecção (rede local vs. conexão de nuvem), o tipo de dispositivo (desktop, dispositivo móvel, Xbox, Hub e Holographic) e o status de disponibilidade (o status de disponibilidade do dispositivo para usar recursos de Sistema Remoto).

Os objetos de filtro devem ser construídos antes de o objeto **RemoteSystemWatcher** ser inicializado porque eles são transmitidos como um parâmetro em seu construtor. O código a seguir cria um filtro de cada tipo disponível e, em seguida, os adiciona a uma lista.

> [!NOTE]
> O código nestes exemplos pressupõe que você tenha uma instrução `using Windows.System.RemoteSystems` no seu arquivo.

[!code-cs[Principal](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

Depois de criar uma lista de objetos [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter), ela pode ser transmitida para o construtor de um **RemoteSystemWatcher**.

[!code-cs[Principal](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Quando o método [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) desse inspetor for chamado, ele acionará o evento [**RemoteSystemAdded** ](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) somente se for detectado um dispositivo que atenda a todos os seguintes critérios:
* Ele é detectável por conexão proximal
* É um desktop ou um telefone
* Ele é classificado como disponível

A partir daí, o procedimento para tratar eventos, recuperar objetos [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) e se conectar a dispositivos remotos é exatamente o mesmo descrito em [Iniciar um aplicativo remoto](launch-a-remote-app.md). Em poucas palavras, os objetos **RemoteSystem** são armazenados como propriedades de [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), que são parâmetros de cada evento **RemoteSystemAdded**.

## Descobrir dispositivos por entrada de endereço
Alguns dispositivos podem não estar associados um usuário nem ser detectáveis com uma verificação, mas eles ainda poderão ser acessados se o aplicativo descoberto usar um endereço direto. A classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) é usada para representar o endereço de um dispositivo remoto. Isso geralmente é armazenado na forma de um endereço IP, mas vários outros formatos são permitidos (consulte o [**Construtor HostName**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx) para obter detalhes).

Um objeto **RemoteSystem** será recuperado se o objeto **HostName** válido for fornecido. Se os dados de endereço forem inválidos, uma referência ao objeto `null` será retornada.

[!code-cs[Principal](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## Tópicos relacionados
[Aplicativos e dispositivos conectados (projeto "Roma")](connected-apps-and-devices.md)  
[Iniciar um aplicativo remoto](launch-a-remote-app.md)  
[Referência de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Exemplo de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) demonstra como descobrir um sistema remoto, iniciar um aplicativo em um sistema remoto e usar os serviços de aplicativo para enviar mensagens entre aplicativos em execução em dois sistemas.



<!--HONumber=Nov16_HO1-->


