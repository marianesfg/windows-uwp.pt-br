---
author: PatrickFarley
title: Descobrir dispositivos remotos
description: Saiba como descobrir dispositivos remotos de seu aplicativo usando o projeto Roma.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, project rome
ms.localizationpriority: medium
ms.openlocfilehash: 02d04074ece0033da8c3454a95bc35af201903f3
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2916935"
---
# <a name="discover-remote-devices"></a>Descobrir dispositivos remotos
O aplicativo pode usar a rede sem fio, Bluetooth e uma conexão na nuvem para descobrir dispositivos Windows conectados com a mesma conta da Microsoft que o dispositivo de descoberta. Os dispositivos remotos não precisam ter nenhum software especial instalado para ser descoberto.

> [!NOTE]
> Este guia considera que você já tenha obtido acesso ao recurso Sistemas Remotos seguindo as etapas em [Iniciar um aplicativo remoto](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Filtrar o conjunto de dispositivos detectáveis
Você pode reduzir o conjunto de dispositivos detectáveis usando um [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) com filtros. Os filtros podem detectar o tipo de detecção (proximal, rede local e conexão de nuvem), o tipo de dispositivo (desktop, dispositivo móvel, Xbox, Hub e holográfico) e o status de disponibilidade (o status de disponibilidade do dispositivo para usar recursos de Sistema remoto).

Os objetos de filtro devem ser construídos antes ou enquanto o objeto **RemoteSystemWatcher** é inicializado, pois são transmitidos como um parâmetro no construtor. O código a seguir cria um filtro de cada tipo disponível e, em seguida, os adiciona a uma lista.

> [!NOTE]
> O código nestes exemplos exige que você tenha uma instrução `using Windows.System.RemoteSystems` no seu arquivo.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> O valor de filtro "proximal" não garante o grau de proximidade física. Para cenários que exigem uma proximidade física confiável, use o valor [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) no filtro. No momento, esse filtro permite somente dispositivos descobertos por Bluetooth. Como novos mecanismos de descoberta e protocolos que garantem a proximidade física são compatíveis, eles serão incluídos aqui também.  
Há também uma propriedade na classe [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem)que indica se um dispositivo descoberto está na verdade na proximidade física: [**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Se você pretende descobrir dispositivos em uma rede local (determinada pela seleção do filtro de tipo de descoberta), a rede precisa usar um perfil "privado" ou de "domínio". O dispositivo não vai descobrir outros dispositivos em uma rede "pública".

Depois que uma lista de objetos [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) é criada, ela pode ser transmitida para o construtor de um **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Quando o método [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) desse inspetor for chamado, ele acionará o evento [**RemoteSystemAdded** ](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) somente se for detectado um dispositivo que atenda a todos os seguintes critérios:
* Ele é detectável por conexão proximal
* É um desktop ou um telefone
* Ele é classificado como disponível

A partir daí, o procedimento para tratar eventos, recuperar objetos [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) e se conectar a dispositivos remotos é exatamente o mesmo descrito em [Iniciar um aplicativo remoto](launch-a-remote-app.md). Ou seja, os objetos **RemoteSystem** são armazenados como propriedades dos objetos [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), que são passados em cada evento **RemoteSystemAdded**.

## <a name="discover-devices-by-address-input"></a>Descobrir dispositivos por entrada de endereço
Alguns dispositivos podem não estar associados um usuário nem ser detectáveis com uma verificação, mas eles ainda poderão ser acessados se o aplicativo descoberto usar um endereço direto. A classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) é usada para representar o endereço de um dispositivo remoto. Isso geralmente é armazenado na forma de um endereço IP, mas vários outros formatos são permitidos (consulte o [**Construtor HostName**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx) para obter detalhes).

Um objeto **RemoteSystem** será recuperado se o objeto **HostName** válido for fornecido. Se os dados de endereço forem inválidos, uma referência ao objeto `null` será retornada.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Consultando uma funcionalidade em um sistema remoto

Embora separada da filtragem de descoberta, consultar os recursos do dispositivo pode ser uma parte importante do processo de detecção. Com o método [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync), você pode consultar sistemas remotos descobertos para oferecer suporte a determinados recursos, como conectividade de sessão remota ou compartilhamento de entidade espacial (holográfica). Consulte a classe [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) para obter a lista dos recursos que podem ser consultados.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Descoberta entre usuários

Os desenvolvedores podem especificar a detecção de _todos_ os dispositivos próximos ao dispositivo cliente, não apenas dispositivos registrados para o mesmo usuário. Isso é implementado por meio um **IRemoteSystemFilter**, [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter) especial. Ele é implementado como os outros tipos de filtro:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* A valor [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) de **Anônimo** permite a detecção de todos os dispositivos proximais, mesmo aqueles de usuários não confiáveis.
* Um valor **SameUser** filtra a descoberta somente para dispositivos registrados do mesmo usuário como o dispositivo cliente. Esse é o comportamento padrão.

### <a name="checking-the-cross-user-sharing-settings"></a>Verificar as configurações de Compartilhamento entre usuários

Além do filtro acima especificado no seu aplicativo de descoberta, o próprio dispositivo cliente também deve ser configurado para permitir experiências compartilhadas de dispositivos registrados com outros usuários. Essa é uma configuração de sistema que pode ser consultada com um método estático na classe **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Para alterar essa configuração, o usuário deve abrir o aplicativo de **Configurações**. No menu **Sistema** > **Experiências compartilhadas** > **Compartilhar entre dispositivos**, há uma caixa de lista suspensa na qual o usuário pode especificar com quais dispositivos o sistema pode ser compartilhado.

![página de configurações de experiências compartilhadas](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Tópicos relacionados
* [Apps e dispositivos conectados (projeto Roma)](connected-apps-and-devices.md)
* [Iniciar um aplicativo remoto](launch-a-remote-app.md)
* [Referência de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [Exemplo de Sistemas Remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
