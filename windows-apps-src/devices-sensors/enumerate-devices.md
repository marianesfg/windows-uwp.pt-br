---
author: DBirtolo
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: Enumerar dispositivos
description: O namespace de enumeração permite localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio.
---
# Enumerar dispositivos

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** APIs Importantes **

-   [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)

O namespace de enumeração permite localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio. As APIs que você usa para enumerar os dispositivos possíveis são o namespace [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459). Alguns motivos para usar essas APIs são enumerados a seguir.

-   Localizar um dispositivo para conectar ao seu aplicativo.
-   Obter informações sobre dispositivos conectados ao ou detectáveis pelo sistema.
-   Fazer com que um aplicativo receba notificações quando dispositivos são adicionados, conectados, desconectados, mudam o status online ou outras propriedades.
-   Fazer com que um aplicativo receba gatilhos em segundo plano quando dispositivos se conectam, desconectam, mudam o status online ou outras propriedades.

Essas APIs podem enumerar dispositivos sobre qualquer um dos seguintes protocolos e barramentos, contanto que o dispositivo individual e o sistema que executa o aplicativo aceitem essa tecnologia. Essa não é uma lista completa, e outros protocolos podem ser aceitos por um dispositivo específico.

-   Barramentos fisicamente conectados. Isso inclui PCI e USB. Por exemplo, tudo que você pode ver no **Gerenciador de Dispositivos**
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   Digital Living Network Alliance (DLNA)
-   [**Descoberta e Inicialização (DIAL)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**Descoberta de Serviços DNS (DNS-SD)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [Serviços Web em Dispositivos (WSD)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [Bluetooth](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**Wi-Fi Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**Ponto de Serviço**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Em muitos casos, você não precisa se preocupar em usar as APIs de enumeração. Isso ocorre porque muitas APIs que usam dispositivos selecionarão automaticamente o dispositivo padrão apropriado ou fornecerão uma API de enumeração mais simplificada. Por exemplo, [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) usará automaticamente o dispositivo renderizador de áudio padrão. Contanto que seu aplicativo possa usar o dispositivo padrão, não é necessário usar as APIs de enumeração em seu aplicativo. As APIs de enumeração fornecem uma maneira geral e flexível para você descobrir e se conectar aos dispositivos disponíveis. Este tópico fornece informações sobre como enumerar dispositivos e descreve as quatro maneiras comuns de enumerar dispositivos.

-   Usar a interface do usuário do [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)
-   Enumerar um instantâneo dos dispositivos atualmente detectáveis pelo sistema
-   Enumerar dispositivos atualmente detectáveis e inspecionar alterações
-   Enumerar dispositivos atualmente detectáveis e observar as alterações em uma tarefa em segundo plano

## Objetos DeviceInformation


Trabalhar com as APIs de enumeração, você frequentemente precisa usar objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Esses objetos contêm a maior parte das informações disponíveis sobre o dispositivo. A tabela a seguir explica algumas das propriedades **DeviceInformation** que serão interessantes para você. Para obter uma lista completa, consulte a página de referência de **DeviceInformation**

| Propriedade                         | Comentários                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | Esse é o identificador exclusivo do dispositivo e é fornecido como uma variável de cadeia de caracteres. Na maioria dos casos, esse é um valor opaco que você apenas passará de um método para outro para indicar o dispositivo específico que lhe interessa. Você também pode usar essa propriedade e a propriedade **DeviceInformation.Kind** depois de fechar seu aplicativo e reabri-lo. Isso garante que você possa recuperar e reutilizar o mesmo objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). |
| **DeviceInformation.Kind**       | Isso indica o tipo de objeto de dispositivo representado pelo objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Essa não é a categoria de dispositivo ou o tipo de dispositivo. Um único dispositivo pode ser representado por vários objetos **DeviceInformation** diferentes de tipos diferentes. Os valores possíveis para esta propriedade são listados em [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/BR225393kind), bem como a relação entre eles.                           |
| **DeviceInformation.Properties** | Esse recipiente de propriedades contém informações solicitadas para o objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). As propriedades mais comuns são referenciadas facilmente como propriedades do objeto **DeviceInformation**, como com [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name). Para saber mais, consulte [Propriedades de informações do dispositivo](device-information-properties.md)                                                                |

 

## Interface do usuário do DevicePicker


O [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) é um controle fornecido pelo Windows que cria uma pequena interface do usuário que permite que o usuário selecione um dispositivo de uma lista. Você pode personalizar a janela **DevicePicker** de algumas maneiras.

-   Você pode controlar os dispositivos que são exibidos na interface do usuário adicionando um [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/Dn930841filter_supporteddeviceselectors), um [**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/Dn930841filter_supporteddeviceclasses) ou ambos ao [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter). Na maioria dos casos, você só precisa adicionar um seletor ou uma classe, mas se precisar de mais de um, poderá adicionar vários. Se você adicionar vários seletores ou classes, eles serão combinados usando uma função de lógica OR.
-   Você pode especificar as propriedades que deseja recuperar para os dispositivos. Você pode fazer isso adicionando propriedades a [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties)
-   Você pode alterar a aparência do [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) usando [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance)
-   Você pode especificar o tamanho e o local do [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) quando ele é exibido.

Enquanto o [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) estiver sendo exibido, o conteúdo da interface do usuário será atualizado automaticamente se dispositivos forem adicionados, removidos ou atualizados.

**Observação**  Você não pode especificar o [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/BR225393kind) usando o [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841). Se quiser ter dispositivos de determinado **DeviceInformationKind**, você precisará criar um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) e fornecer sua própria interface do usuário.

 

O conteúdo de mídia de transmissão e DIAL também cada fornecem seus próprios seletores se você quiser usá-los. Eles são [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) e [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783), respectivamente.

## Enumerar um instantâneo de dispositivos


Em alguns cenários, o [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) não será adequado às suas necessidades e você precisará de algo mais flexível. Talvez seja preciso criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário. Nessas situações, você poderia enumerar um instantâneo de dispositivos. Isso envolve procurar os dispositivos que estão conectados ou emparelhados ao sistema. No entanto, você precisa estar ciente de que esse método procura apenas um instantâneo dos dispositivos que estão disponíveis, portanto, não você não poderá localizar os dispositivos que se conectarem após a enumeração na lista. Você também não será notificado se um dispositivo for atualizado ou removido. Outra desvantagem potencial a ter em mente é que esse método manterá os resultados até que toda a enumeração seja concluída. Por esse motivo, você não deve usar esse método quando estiver interessado em objetos **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, pois eles são encontrados via protocolo de rede ou sem fio. Isso pode levar até 30 segundos para terminar. Nesse cenário, você deve usar um objeto [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) para enumerar os possíveis dispositivos.

Para enumerar por meio de um instantâneo de dispositivos, use o método [**FindAllAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx). Esse método aguarda o término do processo de enumeração inteiro e retorna todos os resultados como um objeto [**DeviceInformationCollection**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx). Esse método também é sobrecarregado para fornecer várias opções para filtrar os resultados e limitá-los aos dispositivos que lhe interessam. Você pode fazer isso fornecendo um [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) ou passando um seletor de dispositivo. O seletor de dispositivo é uma cadeia de caracteres AQS que especifica os dispositivos que você deseja enumerar. Para obter mais informações, consulte [Criar um seletor de dispositivo](build-a-device-selector.md)

Além de limitar os resultados, você também pode especificar as propriedades que deseja recuperar para os dispositivos. Se você fizer isso, as propriedades especificadas estarão disponíveis no recipiente de propriedades para cada um dos objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) retornados na coleção. É importante notar que nem todas as propriedades estão disponíveis para todos os tipos de dispositivo. Para ver quais propriedades estão disponíveis para quais tipos de dispositivo, consulte [Propriedades de informações do dispositivo](device-information-properties.md).

## Enumerar e inspecionar dispositivos


Um método mais avançado e flexível de enumeração de dispositivos é criar um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446). Essa opção fornece mais flexibilidade para enumerar dispositivos. Isso permite enumerar dispositivos que estão presentes no momento e também recebem notificações quando dispositivos que correspondem ao seletor de dispositivo são adicionados ou removidos, ou quando as propriedades são alteradas. Ao criar um **DeviceWatcher**, você fornece um seletor de dispositivo. Para saber mais sobre seletores de dispositivo, consulte [Criar um seletor de dispositivo](build-a-device-selector.md). Depois de criar o inspetor, você receberá as notificações a seguir para qualquer dispositivo que corresponda aos critérios fornecidos.

-   Notificação de adição quando um novo dispositivo é adicionado.
-   Notificação de atualização quando uma propriedade de seu interesse é alterada.
-   Notificação de remoção quando um dispositivo não está mais disponível ou não corresponde mais ao seu filtro.

Na maioria dos casos em que estiver usando um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), você mantém uma lista de dispositivos, adiciona e remove itens da lista ou atualiza os itens conforme o inspetor recebe atualizações dos dispositivos inspecionados. Quando você receber uma notificação de atualização, as informações atualizadas estarão disponíveis como um objeto [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/BR225393update). Para atualizar sua lista de dispositivos, primeiro localize a [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) apropriada que foi alterada. Em seguida, chame o método [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update) para esse objeto, fornecendo o objeto **DeviceInformationUpdate**. Essa é uma função conveniente que atualizará automaticamente seu objeto **DeviceInformation**.

Como um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) envia notificações à medida que os dispositivos chegam e quando eles são alterados, você deve usar esse método de enumeração de dispositivos quando está interessado em objetos **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, pois eles são enumerados em protocolos de rede ou sem fio.

Para criar um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), use um dos métodos [**CreateWatcher**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx). Esses métodos são sobrecarregados para permitir que você especifique os dispositivos de seu interesse. Você pode fazer isso fornecendo um [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) ou passando um seletor de dispositivo. O seletor de dispositivo é uma cadeia de caracteres AQS que especifica os dispositivos que você deseja enumerar. Para obter mais informações, consulte [Criar um seletor de dispositivo](build-a-device-selector.md). Você também pode especificar as propriedades que deseja recuperar para os dispositivos de seu interesse. Se você fizer isso, as propriedades especificadas estarão disponíveis no recipiente de propriedades para cada um dos objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) retornados na coleção. É importante notar que nem todas as propriedades estão disponíveis para todos os tipos de dispositivo. Para ver quais propriedades estão disponíveis para quais tipos de dispositivo, consulte [Propriedades de informações do dispositivo](device-information-properties.md).

## Inspecionar dispositivos como uma tarefa em segundo plano


Inspecionar dispositivos como uma tarefa em segundo plano é muito semelhante à criação de um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) conforme descrito anteriormente. Na verdade, você ainda precisa criar um objeto **DeviceWatcher** normal primeiro conforme descrito na seção anterior. Depois de criá-lo, você chama [**GetBackgroundTrigger**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx), em vez de [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start). Ao chamar **GetBackgroundTrigger**, você deve especificar quais notificações são de seu interesse: adição, remoção ou atualização. Você não pode solicitar atualização ou remoção sem solicitar adição também. Depois de registrar o gatilho, a execução do **DeviceWatcher** será iniciada imediatamente em segundo plano. Desse ponto em diante, sempre que ele receber uma nova notificação para seu aplicativo que corresponder aos critérios, a tarefa em segundo plano será disparada e fornecerá as últimas alterações desde o último disparo de seu aplicativo.

**Importante**  Um [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) dispara seu aplicativo pela primeira vez quando o inspetor atinge o estado **EnumerationCompleted**. Isso significa que ele conterá todos os resultados iniciais. Nas próximas vezes que seu aplicativo for disparado, ele conterá apenas notificações de adição, atualização e remoção que ocorreram desde o último disparo. Isso é um pouco diferente de um objeto [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) de primeiro plano porque os resultados iniciais não vêm um de cada vez e só são fornecidos em um pacote depois que o **EnumerationCompleted** é atingido.

 

Alguns protocolos sem fio se comportam de maneira diferente quando fazem a verificação em segundo plano em vez de primeiro plano, ou podem não oferecer suporte para a verificação em segundo plano de maneira alguma. Há três possibilidades com relação à verificação em segundo plano. A tabela a seguir lista as possibilidades e os efeitos que isso pode ter em seu aplicativo. Por exemplo, as tecnologias Bluetooth e Wi-Fi Direct não dão suporte a verificações em segundo plano. Portanto, consequentemente, não dão suporte a um [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)

| Comportamento                                  | Impacto                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Mesmo comportamento em seguindo plano               | Nenhuma                                                                                                                                    |
| Somente verificações passivas são possíveis em segundo plano | O dispositivo pode demorar mais tempo para ser descoberto enquanto aguarda uma verificação passiva.                                                           |
| Não há suporte para verificações em segundo plano            | Nenhum dispositivos poderá ser detectado pelo [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838), e nenhuma atualização será relatada. |

 

Se seu [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) incluir um protocolo que não aceita a verificação como uma tarefa em segundo plano, seu gatilho ainda funcionará. No entanto, você não poderá obter atualizações ou resultados por esse protocolo. As atualizações de outros protocolos ou dispositivos ainda serão detectadas normalmente.

## Usando DeviceInformationKind


Na maioria dos casos, você não precisa se preocupar sobre o [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/BR225393kind) de um objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Isso porque o seletor de dispositivo retornado pela API de dispositivo que você está usando geralmente garantirá que você obtenha os tipos corretos de objetos de dispositivo para usar com sua API. No entanto, em algumas situações, convém obter a **DeviceInformation** dos dispositivos, mas não há uma API de dispositivo correspondente para fornecer um seletor de dispositivo. Nesses casos, você precisará criar seu próprio seletor. Por exemplo, a funcionalidade Serviços Web em Dispositivos não tem uma API dedicada, mas você pode descobrir esses dispositivos e obter informações sobre eles usando as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) e usá-las com as APIs de soquete.

Se você estiver criando seu próprio seletor de dispositivo para enumerar por meio de objetos de dispositivo, será importante você entender o [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/BR225393kind). Todos os tipos possíveis, bem como a relação entre eles, são descritos na página de referência de **DeviceInformationKind**. Um dos usos mais comuns do **DeviceInformationKind** é especificar o tipo de dispositivo que você está procurando ao enviar uma consulta em conjunto com um seletor de dispositivo. Fazer isso garante que você enumere somente os dispositivos que correspondam ao **DeviceInformationKind** fornecido. Por exemplo, você poderia encontrar um objeto **DeviceInterface** e, em seguida, executar uma consulta para obter as informações sobre o objeto **Device** pai. Esse objeto pai pode conter informações adicionais.

É importante observar que as propriedades disponíveis no recipiente de propriedades para um objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) irá variar dependendo do [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/BR225393kind) do dispositivo. Determinadas propriedades só estão disponíveis com determinados tipos. Para obter mais informações sobre quais propriedades estão disponíveis para quais tipos, consulte [Propriedades de informações do dispositivo](device-information-properties.md). Portanto, no exemplo acima, procurar o **Device** pai lhe dará acesso a mais informações que não estavam disponíveis no objeto de dispositivo **DeviceInterface**. Por isso, quando você cria suas cadeias de caracteres de filtro AQS, é importante garantir que as propriedades solicitadas estejam disponíveis para os objetos **DeviceInformationKind** que você estiver enumerando. Para saber mais sobre como criar um filtro, consulte [Criar um seletor de dispositivo](build-a-device-selector.md)

Ao enumerar objetos **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, você está enumerando por meio de um protocolo sem fio ou de rede. Nessas situações, recomendamos que você não use [**FindAllAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx). Em vez disso, use [**CreateWatcher**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx). Isso ocorre porque a pesquisa por uma rede geralmente resulta em operações de pesquisa que não limitará o tempo por 10 ou mais segundos antes de gerar [**EnumerationCompleted**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx). **FindAllAsync** não concluir sua operação até **EnumerationCompleted** ser disparada. Se você estiver usando um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), obterá resultados quase em tempo real independentemente de quando **EnumerationCompleted** é chamado. Salvar um dispositivo para uso posterior

## Qualquer objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) exclusivamente identificado por uma combinação de duas informações: [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) e [**DeviceInformation.Kind**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx).


Se você mantiver essas duas informações, poderá recriar o objeto **DeviceInformation** depois de perdê-lo fornecendo essas informações para [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync_907774063). Se fizer isso, você poderá salvar as preferências do usuário para um dispositivo que se integra ao seu aplicativo. Amostra

## Para baixar uma amostra de como usar as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), clique [aqui](http://go.microsoft.com/fwlink/?LinkID=620536)


To download a sample showing how to use the <bpt id="p1">[</bpt><bpt id="p2">**</bpt>Windows.Devices.Enumeration<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/apps/BR225459)</ept> APIs, click <bpt id="p3">[</bpt>here<ept id="p3">](http://go.microsoft.com/fwlink/?LinkID=620536)</ept>.

 

 






<!--HONumber=May16_HO2-->


