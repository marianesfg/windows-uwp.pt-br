---
title: Comunicar-se com um serviço de app remoto
description: Troque mensagens com um serviço de app em execução em um dispositivo remoto usando o projeto Roma.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, projeto Roma, tarefa em segundo plano, o serviço de aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 067b465feccda424dd6a8e3f44e784166afe6d48
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366427"
---
# <a name="communicate-with-a-remote-app-service"></a>Comunicar-se com um serviço de app remoto

Além de iniciar um app em um dispositivo remoto usando um URI, você também pode executar e se comunicar com *serviços de app* em dispositivos remotos. Qualquer dispositivo baseado no Windows pode ser usado como o dispositivo cliente ou host. Isso proporciona um número praticamente ilimitado de maneiras de interagir com dispositivos conectados sem a necessidade de trazer um app para o primeiro plano.

## <a name="set-up-the-app-service-on-the-host-device"></a>Configurar o serviço de app no dispositivo host
Para executar um serviço de app em um dispositivo remoto, você já deve ter um provedor desse serviço de app instalado no dispositivo. Este guia usará a versão CSharp do [exemplo do serviço de aplicativo Gerador de Número Aleatório](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices), disponível no [repositório de exemplos universais do Windows](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Para obter instruções sobre como criar seu próprio serviço de app, consulte [Criar e consumir um serviço de app](how-to-create-and-consume-an-app-service.md).

Se você estiver usando um serviço de aplicativo já criado ou estiver criando seu próprio serviço, precisará fazer algumas edições para tornar o serviço compatível com sistemas remotos. No Visual Studio, vá para o projeto do provedor de serviço (chamado de "AppServicesProvider" no exemplo) e selecione seu arquivo _Package.appxmanifest_. Clique com botão direito e selecione **Exibir Código** para exibir todo o conteúdo do arquivo. Criar uma **extensões** elemento dentro do principal **aplicativo** elemento (ou encontrá-lo caso ele já exista). Em seguida, crie uma **extensão** para definir o projeto como um serviço de aplicativo e fazer referência a seu projeto pai.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Ao lado de **AppService** elemento, adicionar o **SupportsRemoteSystems** atributo:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Para usar elementos desta **uap3** namespace, você deve adicionar a definição de namespace na parte superior do arquivo de manifesto se ainda não estiver lá.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Em seguida, compilar seu projeto de provedor de serviço de aplicativo e implantá-lo para o dispositivo de host (s).

## <a name="target-the-app-service-from-the-client-device"></a>Direcionar o serviço de app do dispositivo cliente
O dispositivo do qual o serviço de app remoto deve ser chamado precisa de um app com a funcionalidade Sistemas Remotos. Isso pode ser adicionado ao mesmo app que fornece o serviço de app no dispositivo host (nesse caso, você deve instalar o mesmo app nos dois dispositivos) ou implementado em um app completamente diferente.

As seguintes instruções **using** são necessárias para o código nesta seção ser executado no estado em que se encontra:

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Primeiro você deve instanciar um objeto [**AppServiceConnection**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), como se estivesse chamando um serviço de aplicativo localmente. Esse processo é explicado com mais detalhes em [Criar e consumir um serviço de app](how-to-create-and-consume-an-app-service.md). Neste exemplo, o serviço de app de destino é o serviço de gerador de número aleatório.

> [!NOTE]
> Presume-se que um objeto [RemoteSystem](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem) já tenha sido adquirido por outros meios no código que chama o método a seguir. Consulte [Iniciar um aplicativo remoto](launch-a-remote-app.md) para obter instruções sobre como configurá-lo.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Em seguida, um objeto [**RemoteSystemConnectionRequest**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) é criado para o dispositivo remoto pretendido. Ele é usado para abrir o **AppServiceConnection** para esse dispositivo. Observe que, no exemplo a seguir, o tratamento de erros e os relatórios estão bastante simplificados por questões de brevidade.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

Neste ponto, você deve ter uma conexão aberta com um serviço de app em uma máquina remota.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Trocar mensagens específicas de serviços pela conexão remota

A partir daqui, você pode enviar e receber mensagens de e para o serviço na forma de objetos [**ValueSet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) (para obter mais informações, consulte [Criar e consumir um serviço de app](how-to-create-and-consume-an-app-service.md)). O serviço de gerador de número aleatório considera dois inteiros com as chaves `"minvalue"` e `"maxvalue"` como entradas, seleciona aleatoriamente um inteiro em seu intervalo e o devolve para o processo de chamada com a chave `"Result"`.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Agora que você já está conectado a um serviço de app em um dispositivo host de destino, execute uma operação nesse dispositivo e receba os dados em seu dispositivo cliente em resposta.

## <a name="related-topics"></a>Tópicos relacionados

[Visão de geral (projeto Roma) aplicativos e dispositivos conectado](connected-apps-and-devices.md)  
[Inicie um aplicativo remoto](launch-a-remote-app.md)  
[Criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md)  
[Referência da API de sistemas remota](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)  
[Exemplo de Sistemas Remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
