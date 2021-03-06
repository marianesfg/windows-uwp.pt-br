---
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: Enumerar dispositivos
description: O namespace de enumeração permite localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b562dd139705e983bc8a8ad10962d923cff55559
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259674"
---
# <a name="enumerate-devices"></a>Enumerar dispositivos


## <a name="samples"></a>Exemplos

A maneira mais simples de enumerar todos os dispositivos disponíveis é fazer uma captura de tela com o comando [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) (explicado em mais detalhes em uma seção abaixo).

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

Para baixar uma amostra dos usos mais avançados das APIs [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), clique [aqui](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

## <a name="enumeration-apis"></a>APIs de enumeração

O namespace de enumeração permite localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio. As APIs que você usa para enumerar os dispositivos possíveis são o namespace [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration). Alguns motivos para usar essas APIs são enumerados a seguir.

-   Localizar um dispositivo para conectar ao seu aplicativo.
-   Obter informações sobre dispositivos conectados ao ou detectáveis pelo sistema.
-   Fazer com que um aplicativo receba notificações quando dispositivos são adicionados, conectados, desconectados, mudam o status online ou outras propriedades.
-   Fazer com que um aplicativo receba gatilhos em segundo plano quando dispositivos se conectam, desconectam, mudam o status online ou outras propriedades.

Essas APIs podem enumerar dispositivos sobre qualquer um dos seguintes protocolos e barramentos, contanto que o dispositivo individual e o sistema que executa o aplicativo aceitem essa tecnologia. Essa não é uma lista completa, e outros protocolos podem ser aceitos por um dispositivo específico.

-   Barramentos fisicamente conectados. Isso inclui PCI e USB. Por exemplo, tudo que você pode ver no **Gerenciador de Dispositivos**.
-   [UPnP](https://docs.microsoft.com/windows/desktop/UPnP/universal-plug-and-play-start-page)
-   Digital Living Network Alliance (DLNA)
-   [**Descoberta e inicialização (DIAL)** ](https://docs.microsoft.com/uwp/api/Windows.Media.DialProtocol)
-   [**Descoberta de serviço DNS (DNS-SD)** ](https://docs.microsoft.com/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd)
-   [Serviços Web em dispositivos (WSD)](https://docs.microsoft.com/windows/desktop/WsdApi/wsd-portal)
-   [Bluetooth](https://docs.microsoft.com/windows/desktop/Bluetooth/bluetooth-start-page)
-   [**Wi-Fi Direct**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFiDirect)
-   WiGig
-   [**Ponto de serviço**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)

Em muitos casos, você não precisa se preocupar em usar as APIs de enumeração. Isso ocorre porque muitas APIs que usam dispositivos selecionarão automaticamente o dispositivo padrão apropriado ou fornecerão uma API de enumeração mais simplificada. Por exemplo, [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) usará automaticamente o dispositivo renderizador de áudio padrão. Contanto que seu aplicativo possa usar o dispositivo padrão, não é necessário usar as APIs de enumeração em seu aplicativo. As APIs de enumeração fornecem uma maneira geral e flexível para você descobrir e se conectar aos dispositivos disponíveis. Este tópico fornece informações sobre como enumerar dispositivos e descreve as quatro maneiras comuns de enumerar dispositivos.

-   Usar a interface do usuário do [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker)
-   Enumerar um instantâneo dos dispositivos atualmente detectáveis pelo sistema
-   Enumerar dispositivos atualmente detectáveis e inspecionar alterações
-   Enumerar dispositivos atualmente detectáveis e observar as alterações em uma tarefa em segundo plano

## <a name="deviceinformation-objects"></a>Objetos DeviceInformation


Trabalhar com as APIs de enumeração, você frequentemente precisa usar objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation). Esses objetos contêm a maior parte das informações disponíveis sobre o dispositivo. A tabela a seguir explica algumas das propriedades **DeviceInformation** que serão interessantes para você. Para obter uma lista completa, consulte a página de referência de **DeviceInformation**.

| Propriedade                         | Comentários                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | Esse é o identificador exclusivo do dispositivo e é fornecido como uma variável de cadeia de caracteres. Na maioria dos casos, esse é um valor opaco que você apenas passará de um método para outro para indicar o dispositivo específico que lhe interessa. Você também pode usar essa propriedade e a propriedade **DeviceInformation.Kind** depois de fechar seu aplicativo e reabri-lo. Isso garante que você possa recuperar e reutilizar o mesmo objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation). |
| **DeviceInformation. Kind**       | Isso indica o tipo de objeto de dispositivo representado pelo objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation). Essa não é a categoria de dispositivo ou o tipo de dispositivo. Um único dispositivo pode ser representado por vários objetos **DeviceInformation** diferentes de tipos diferentes. Os valores possíveis para esta propriedade são listados em [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind), bem como a relação entre eles.                           |
| **DeviceInformation. Properties** | Esse recipiente de propriedades contém informações solicitadas para o objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation). As propriedades mais comuns são referenciadas facilmente como propriedades do objeto **DeviceInformation**, como com [**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name). Para saber mais, consulte [Propriedades de informações do dispositivo](device-information-properties.md).                                                                |

 

## <a name="devicepicker-ui"></a>Interface do usuário do DevicePicker


O [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker) é um controle fornecido pelo Windows que cria uma pequena interface do usuário que permite que o usuário selecione um dispositivo de uma lista. Você pode personalizar a janela **DevicePicker** de algumas maneiras.

-   Você pode controlar os dispositivos que são exibidos na interface do usuário adicionando um [**SupportedDeviceSelectors**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors), um [**SupportedDeviceClasses**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) ou ambos ao [**DevicePicker.Filter**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter). Na maioria dos casos, você só precisa adicionar um seletor ou uma classe, mas se precisar de mais de um, poderá adicionar vários. Se você adicionar vários seletores ou classes, eles serão combinados usando uma função de lógica OR.
-   Você pode especificar as propriedades que deseja recuperar para os dispositivos. Você pode fazer isso adicionando propriedades a [**DevicePicker.RequestedProperties**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.requestedproperties).
-   Você pode alterar a aparência do [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker) usando [**Appearance**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.appearance).
-   Você pode especificar o tamanho e o local do [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker) quando ele é exibido.

Enquanto o [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker) estiver sendo exibido, o conteúdo da interface do usuário será atualizado automaticamente se dispositivos forem adicionados, removidos ou atualizados.

**Observação**  você não pode especificar o [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind) usando o [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker). Se quiser ter dispositivos de determinado **DeviceInformationKind**, você precisará criar um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) e fornecer sua própria interface do usuário.

 

O conteúdo de mídia de transmissão e DIAL também cada fornecem seus próprios seletores se você quiser usá-los. Eles são [**CastingDevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePicker) e [**DialDevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Media.DialProtocol.DialDevicePicker), respectivamente.

## <a name="enumerate-a-snapshot-of-devices"></a>Enumerar um instantâneo de dispositivos


Em alguns cenários, o [**DevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePicker) não será adequado às suas necessidades e você precisará de algo mais flexível. Talvez seja preciso criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário. Nessas situações, você poderia enumerar um instantâneo de dispositivos. Isso envolve procurar os dispositivos que estão conectados ou emparelhados ao sistema. No entanto, você precisa estar ciente de que esse método procura apenas um instantâneo dos dispositivos que estão disponíveis, portanto, não você não poderá localizar os dispositivos que se conectarem após a enumeração na lista. Você também não será notificado se um dispositivo for atualizado ou removido. Outra desvantagem potencial a ter em mente é que esse método manterá os resultados até que toda a enumeração seja concluída. Por esse motivo, você não deve usar esse método quando estiver interessado em objetos **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, pois eles são encontrados via protocolo de rede ou sem fio. Isso pode levar até 30 segundos para terminar. Nesse cenário, você deve usar um objeto [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) para enumerar os possíveis dispositivos.

Para enumerar por meio de um instantâneo de dispositivos, use o método [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync). Esse método aguarda o término do processo de enumeração inteiro e retorna todos os resultados como um objeto [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcollection). Esse método também é sobrecarregado para fornecer várias opções para filtrar os resultados e limitá-los aos dispositivos que lhe interessam. Você pode fazer isso fornecendo um [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass) ou passando um seletor de dispositivo. O seletor de dispositivo é uma cadeia de caracteres AQS que especifica os dispositivos que você deseja enumerar. Para obter mais informações, consulte [Criar um seletor de dispositivo](build-a-device-selector.md).

Um exemplo de um instantâneo de enumeração do dispositivo é fornecido abaixo:



Além de limitar os resultados, você também pode especificar as propriedades que deseja recuperar para os dispositivos. Se você fizer isso, as propriedades especificadas estarão disponíveis no recipiente de propriedades para cada um dos objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) retornados na coleção. É importante notar que nem todas as propriedades estão disponíveis para todos os tipos de dispositivo. Para ver quais propriedades estão disponíveis para quais tipos de dispositivo, consulte [Propriedades de informações do dispositivo](device-information-properties.md).



## <a name="enumerate-and-watch-devices"></a>Enumerar e inspecionar dispositivos


Um método mais avançado e flexível de enumeração de dispositivos é criar um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher). Essa opção fornece mais flexibilidade para enumerar dispositivos. Isso permite enumerar dispositivos que estão presentes no momento e também recebem notificações quando dispositivos que correspondem ao seletor de dispositivo são adicionados ou removidos, ou quando as propriedades são alteradas. Ao criar um **DeviceWatcher**, você fornece um seletor de dispositivo. Para saber mais sobre seletores de dispositivo, consulte [Criar um seletor de dispositivo](build-a-device-selector.md). Depois de criar o inspetor, você receberá as notificações a seguir para qualquer dispositivo que corresponda aos critérios fornecidos.

-   Notificação de adição quando um novo dispositivo é adicionado.
-   Notificação de atualização quando uma propriedade de seu interesse é alterada.
-   Notificação de remoção quando um dispositivo não está mais disponível ou não corresponde mais ao seu filtro.

Na maioria dos casos em que estiver usando um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher), você mantém uma lista de dispositivos, adiciona e remove itens da lista ou atualiza os itens conforme o inspetor recebe atualizações dos dispositivos inspecionados. Quando você receber uma notificação de atualização, as informações atualizadas estarão disponíveis como um objeto [**DeviceInformationUpdate**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationupdate). Para atualizar sua lista de dispositivos, primeiro localize a [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) apropriada que foi alterada. Em seguida, chame o método [**Update**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.update) para esse objeto, fornecendo o objeto **DeviceInformationUpdate**. Essa é uma função conveniente que atualizará automaticamente seu objeto **DeviceInformation**.

Como um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) envia notificações à medida que os dispositivos chegam e quando eles são alterados, você deve usar esse método de enumeração de dispositivos quando está interessado em objetos **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, pois eles são enumerados em protocolos de rede ou sem fio.

Para criar um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher), use um dos métodos [**CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). Esses métodos são sobrecarregados para permitir que você especifique os dispositivos de seu interesse. Você pode fazer isso fornecendo um [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass) ou passando um seletor de dispositivo. O seletor de dispositivo é uma cadeia de caracteres AQS que especifica os dispositivos que você deseja enumerar. Para obter mais informações, consulte [Criar um seletor de dispositivo](build-a-device-selector.md). Você também pode especificar as propriedades que deseja recuperar para os dispositivos de seu interesse. Se você fizer isso, as propriedades especificadas estarão disponíveis no recipiente de propriedades para cada um dos objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) retornados na coleção. É importante notar que nem todas as propriedades estão disponíveis para todos os tipos de dispositivo. Para ver quais propriedades estão disponíveis para quais tipos de dispositivo, consulte [Propriedades de informações do dispositivo](device-information-properties.md).

## <a name="watch-devices-as-a-background-task"></a>Inspecionar dispositivos como uma tarefa em segundo plano


Inspecionar dispositivos como uma tarefa em segundo plano é muito semelhante à criação de um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) conforme descrito anteriormente. Na verdade, você ainda precisa criar um objeto **DeviceWatcher** normal primeiro conforme descrito na seção anterior. Depois de criá-lo, você chama [**GetBackgroundTrigger**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted), em vez de [**DeviceWatcher.Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start). Ao chamar **GetBackgroundTrigger**, você deve especificar quais notificações são de seu interesse: adição, remoção ou atualização. Você não pode solicitar atualização ou remoção sem solicitar adição também. Depois de registrar o gatilho, a execução do **DeviceWatcher** será iniciada imediatamente em segundo plano. Desse ponto em diante, sempre que ele receber uma nova notificação para seu aplicativo que corresponder aos critérios, a tarefa em segundo plano será disparada e fornecerá as últimas alterações desde o último disparo de seu aplicativo.

**Importante**  a primeira vez que um [**DeviceWatcherTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) dispara seu aplicativo será quando o observador atingir o estado **EnumerationCompleted** . Isso significa que ele conterá todos os resultados iniciais. Nas próximas vezes que seu aplicativo for disparado, ele conterá apenas notificações de adição, atualização e remoção que ocorreram desde o último disparo. Isso é um pouco diferente de um objeto [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) de primeiro plano porque os resultados iniciais não vêm um de cada vez e só são fornecidos em um pacote depois que o **EnumerationCompleted** é atingido.

 

Alguns protocolos sem fio se comportam de maneira diferente quando fazem a verificação em segundo plano em vez de primeiro plano, ou podem não oferecer suporte para a verificação em segundo plano de maneira alguma. Há três possibilidades com relação à verificação em segundo plano. A tabela a seguir lista as possibilidades e os efeitos que isso pode ter em seu aplicativo. Por exemplo, as tecnologias Bluetooth e Wi-Fi Direct não dão suporte a verificações em segundo plano. Portanto, consequentemente, não dão suporte a um [**DeviceWatcherTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger).

| Comportamento                                  | Impacto                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Mesmo comportamento em seguindo plano               | Nenhuma                                                                                                                                    |
| Somente verificações passivas são possíveis em segundo plano | O dispositivo pode demorar mais tempo para ser descoberto enquanto aguarda uma verificação passiva.                                                           |
| Não há suporte para verificações em segundo plano            | Nenhum dispositivos poderá ser detectado pelo [**DeviceWatcherTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger), e nenhuma atualização será relatada. |

 

Se seu [**DeviceWatcherTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) incluir um protocolo que não aceita a verificação como uma tarefa em segundo plano, seu gatilho ainda funcionará. No entanto, você não poderá obter atualizações ou resultados por esse protocolo. As atualizações de outros protocolos ou dispositivos ainda serão detectadas normalmente.

## <a name="using-deviceinformationkind"></a>Usando DeviceInformationKind


Na maioria dos casos, você não precisa se preocupar sobre o [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind) de um objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation). Isso porque o seletor de dispositivo retornado pela API de dispositivo que você está usando geralmente garantirá que você obtenha os tipos corretos de objetos de dispositivo para usar com sua API. No entanto, em algumas situações, convém obter a **DeviceInformation** dos dispositivos, mas não há uma API de dispositivo correspondente para fornecer um seletor de dispositivo. Nesses casos, você precisará criar seu próprio seletor. Por exemplo, a funcionalidade Serviços Web em Dispositivos não tem uma API dedicada, mas você pode descobrir esses dispositivos e obter informações sobre eles usando as APIs [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) e usá-las com as APIs de soquete.

Se você estiver criando seu próprio seletor de dispositivo para enumerar por meio de objetos de dispositivo, será importante você entender o [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind). Todos os tipos possíveis, bem como a relação entre eles, são descritos na página de referência de **DeviceInformationKind**. Um dos usos mais comuns do **DeviceInformationKind** é especificar o tipo de dispositivo que você está procurando ao enviar uma consulta em conjunto com um seletor de dispositivo. Fazer isso garante que você enumere somente os dispositivos que correspondam ao **DeviceInformationKind** fornecido. Por exemplo, você poderia encontrar um objeto **DeviceInterface** e, em seguida, executar uma consulta para obter as informações sobre o objeto **Device** pai. Esse objeto pai pode conter informações adicionais.

É importante observar que as propriedades disponíveis no recipiente de propriedades para um objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) irá variar dependendo do [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind) do dispositivo. Determinadas propriedades só estão disponíveis com determinados tipos. Para obter mais informações sobre quais propriedades estão disponíveis para quais tipos, consulte [Propriedades de informações do dispositivo](device-information-properties.md). Portanto, no exemplo acima, procurar o **Device** pai lhe dará acesso a mais informações que não estavam disponíveis no objeto de dispositivo **DeviceInterface**. Por isso, quando você cria suas cadeias de caracteres de filtro AQS, é importante garantir que as propriedades solicitadas estejam disponíveis para os objetos **DeviceInformationKind** que você estiver enumerando. Para saber mais sobre como criar um filtro, consulte [Criar um seletor de dispositivo](build-a-device-selector.md).

Ao enumerar objetos **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, você está enumerando por meio de um protocolo sem fio ou de rede. Nessas situações, recomendamos que você não use [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync). Em vez disso, use [**CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). Isso porque a pesquisa por uma rede geralmente resulta em operações de pesquisa que não se encerram por 10 ou mais segundos antes de gerar [**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted). **FindAllAsync** não conclui a operação até **EnumerationCompleted** ser disparado. Se você estiver usando um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher), obterá resultados quase em tempo real independentemente de quando **EnumerationCompleted** é chamado.

## <a name="save-a-device-for-later-use"></a>Salvar um dispositivo para uso posterior


Qualquer objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) exclusivamente identificado por uma combinação de duas informações: [**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) e [**DeviceInformation.Kind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.kind). Se você mantiver essas duas informações, poderá recriar um objeto **DeviceInformation** depois de perdê-lo fornecendo essas informações para [**CreateFromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createfromidasync). Se fizer isso, você poderá salvar as preferências do usuário para um dispositivo que se integra ao seu aplicativo.


 

 




