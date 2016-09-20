---
author: PatrickFarley
title: "Comunicar-se com um serviço de aplicativo remoto"
description: "Troque mensagens com um serviço de aplicativo em execução em um dispositivo remoto usando o projeto &quot;Roma&quot;."
translationtype: Human Translation
ms.sourcegitcommit: c90304b7ca3f7185fca9146aa2303b09cba5ab9a
ms.openlocfilehash: bff77a63d0f88907410c74d4dce19fb422c1bd3f

---

# Comunicar-se com um serviço de aplicativo remoto

Além de iniciar um aplicativo em um dispositivo remoto usando um URI, você também pode executar e se comunicar com *serviços de aplicativo* em dispositivos remotos. Qualquer dispositivo baseado no Windows pode ser usado como o dispositivo de início ou de destino ou ambos. Isso proporciona um número praticamente ilimitado de maneiras de interagir com dispositivos conectados sem a necessidade de trazer um aplicativo para o primeiro plano.

## Configurar o serviço de aplicativo no dispositivo de destino
Para executar um serviço de aplicativo em um dispositivo remoto, você já deve ter um provedor desse serviço de aplicativo instalado no dispositivo de destino. Este guia usará o serviço de aplicativo de gerador de número aleatório, que está disponível no [Repositório de exemplos universais do Windows](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Para obter instruções sobre como criar seu próprio serviço de aplicativo, consulte [Criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md).

Se você estiver usando um serviço de aplicativo já criado ou estiver criando seu próprio serviço, precisará fazer algumas edições para tornar o serviço compatível com sistemas remotos. No Visual Studio, vá para o projeto do provedor de serviço de aplicativo e selecione o arquivo Package.appxmanifest. Clique com botão direito e selecione **Exibir Código** para exibir todo o conteúdo do arquivo. Encontre o elemento **Extension** que define o projeto como um serviço de aplicativo e nomeia seu projeto pai.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Altere o namespace do elemento **AppService** para **uap3** e adicione o atributo **SupportsRemoteSystems**:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Para usar elementos nesse novo namespace, você deve adicionar a definição de namespace à parte superior do arquivo de manifesto.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Compile seu projeto de provedor de serviço de aplicativo e implante-o no(s) dispositivo(s) de destino.

## Direcionar o serviço de aplicativo do dispositivo de início
O dispositivo *do qual* o serviço de aplicativo remoto deve ser chamado precisa ser um aplicativo com a funcionalidade Sistemas Remotos. Isso pode ser adicionado ao mesmo aplicativo que fornece o serviço de aplicativo no dispositivo de destino (nesse caso, você deve instalar o mesmo aplicativo nos dois dispositivos) ou colocá-lo em um aplicativo completamente diferente.

As seguintes instruções **using** são necessárias para o código nesta seção ser executado no estado em que se encontra:

[!code-cs[Principal](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Primeiro você deve instanciar um objeto [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection), como se estivesse chamando um serviço de aplicativo localmente. Esse processo é explicado com mais detalhes em [Criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md). Neste exemplo, o serviço de aplicativo de destino é o serviço de gerador de número aleatório.

> [!NOTE]
> Presume-se que um objeto [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) já tenha sido adquirido por outros meios no código que chama o método a seguir. Consulte [Iniciar um aplicativo remoto](launch-a-remote-app.md) para obter instruções sobre como configurá-lo.

[!code-cs[Principal](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Em seguida, um objeto [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) é criado para o dispositivo remoto pretendido. Ele é usado para abrir o **AppServiceConnection** para esse dispositivo. Observe que no exemplo a seguir, o tratamento de erros e os relatórios estão bastante simplificados por questões de brevidade.

[!code-cs[Principal](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

Neste ponto, você deve ter uma conexão aberta com um serviço de aplicativo em uma máquina remota.

## Trocar mensagens específicas de serviços pela conexão remota

A partir daqui, você pode enviar e receber mensagens de e para o serviço na forma de objetos [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset) (para obter mais informações, consulte [Criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md)). O serviço de gerador de número aleatório considera dois inteiros com as teclas `"minvalue"` e `"maxvalue"` como entradas, seleciona aleatoriamente um inteiro em seu intervalo e o devolve para o processo de chamada com a chave `"Result"`.

[!code-cs[Principal](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Agora que você já está conectado a um serviço de aplicativo em um dispositivo remoto de destino, execute uma operação nesse dispositivo e receba os dados em seu dispositivo de início em resposta.

## Tópicos relacionados

[Visão geral de aplicativos e dispositivos conectados (projeto "Roma")](connected-apps-and-devices.md)  
[Iniciar um aplicativo remoto](launch-a-remote-app.md)  
[Criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md)  
[Referência de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Exemplo de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) demonstra como descobrir um sistema remoto, iniciar um aplicativo em um sistema remoto e usar os serviços de aplicativo para enviar mensagens entre aplicativos em execução em dois sistemas.



<!--HONumber=Aug16_HO3-->


