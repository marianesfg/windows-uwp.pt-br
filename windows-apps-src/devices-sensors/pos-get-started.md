---
author: TerryWarwick
title: Introdução ao Ponto de Serviço
description: Este artigo contém informações sobre a introdução às APIs de UWP de ponto de serviço.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: f3959254787ce22bea27495520805485e0ea179b
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6842812"
---
# <a name="getting-started-with-point-of-service"></a>Introdução ao Ponto de Serviço

Ponto de serviço, ponto de venda ou dispositivos de Ponto de Serviço são periféricos de computador usados para facilitar as transações de varejo. Exemplos de dispositivos de Ponto de Serviço incluem caixas registradoras eletrônicos, scanners de código, leitores de tarjas magnéticas e impressoras de recibo.

Aqui você conhecerá as noções básicas de estabelecer interface com dispositivos de Ponto de Serviço usando as APIs de Ponto de Serviço da Plataforma Universal do Windows (UWP). Abordaremos enumeração do dispositivo, verificação de recursos do dispositivo, declaração de dispositivos e compartilhamento de dispositivos. Usamos um dispositivo de scanner de código de barras como exemplo, mas quase todas as diretrizes aqui se aplicam a qualquer dispositivo de Ponto de Serviço compatível com a UWP. (Para obter uma lista de dispositivos compatíveis, consulte [Suporte a dispositivo de Ponto de Serviço](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Localização e conexão com periféricos de Ponto de Serviço

Antes que um dispositivo de Ponto de Serviço possa ser usado por um aplicativo, ele deve ser emparelhado com o computador no qual o aplicativo está sendo executado. Há várias formas de se conectar com os dispositivos de Ponto de Serviço, de modo programático ou por meio do aplicativo Configurações.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Conexão com dispositivos usando o aplicativo Configurações
Ao conectar um dispositivo de Ponto de Serviço como um scanner de código de barras a um computador, ele aparecerá como qualquer outro dispositivo. Você pode encontrá-lo na seção **Dispositivos > Bluetooth e outros dispositivos** do aplicativo Configurações. Nessa seção, é possível fazer o emparelhamento com um dispositivo de Ponto de Serviço selecionando **Adicionar Bluetooth ou outro dispositivo**.

Alguns dispositivos de Ponto de Serviço talvez não apareçam no aplicativo Configurações até que sejam enumerados programaticamente usando as APIs de Ponto de Serviço.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Obtenção de um único dispositivo de Ponto de Serviço com GetDefaultAsync
Em um caso de uso simples, só é possível ter um periférico de Ponto de Serviço conectado ao computador no qual o aplicativo está em execução e no qual se deseja configurá-lo o mais rápido possível. Para fazê-lo, recupere o dispositivo “padrão” com o método **GetDefaultAsync** conforme mostrado aqui.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Se o dispositivo padrão for encontrado, o objeto de dispositivo recuperado está pronto para ser declarado. “Declarar” um dispositivo fornece a ele o acesso exclusivo ao aplicativo, evitando comandos conflitantes decorrentes de vários processos.

> [!NOTE] 
> Se mais de um dispositivo de Ponto de Serviço for conectado ao computador, o **GetDefaultAsync** retorna o primeiro dispositivo encontrado. Por esse motivo, use **FindAllAsync**, a menos que você tenha certeza de que apenas um dispositivo de Ponto de Serviço está visível para o aplicativo.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Enumeração de uma coleção de dispositivos com FindAllAsync

Quando conectado a mais de um dispositivo, você deve enumerar a coleção de objetos de dispositivo de **PointOfService** para encontrar aquele que deseja declarar. Por exemplo, o código a seguir cria uma coleção de todos os scanners de código de barras atualmente conectados e, em seguida, procura a coleção de um scanner com um nome específico.

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>Escopo da seleção do dispositivo
Ao se conectar a um dispositivo, você talvez queira limitar sua pesquisa a um subconjunto de periféricos de Ponto de Serviço a que seu aplicativo tem acesso. Usando o método **GetDeviceSelector**, você pode definir o escopo da seleção para recuperar dispositivos conectados apenas por um determinado método (Bluetooth, USB etc.). Você pode criar um seletor que procura dispositivos em **Bluetooth**, **IP**, **Local** ou **Todos os tipos de conexão**. Isso pode ser útil, já que a descoberta de dispositivo sem fio leva muito tempo em comparação à descoberta local (com fio). Você pode garantir um tempo de espera determinista para conexão de dispositivo local limitando **FindAllAsync** a tipos de conexão **Local**. Por exemplo, este código recupera todos os scanners de código de barras acessíveis por meio de uma conexão local. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Reação a mudanças na conexão do dispositivo com DeviceWatcher

À medida que seu aplicativo é executado, os dispositivos serão ocasionalmente desconectados ou atualizados ou novos dispositivos precisarão ser adicionados. Você pode usar a classe **DeviceWatcher** para acessar eventos relacionados ao dispositivo. Com isso, seu aplicativo poderá responder adequadamente. Eis um exemplo de como usar **DeviceWatcher**, com stubs de métodos a serem chamados se um dispositivo for adicionado, removido ou atualizado.

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Verificando os recursos de um dispositivo de Ponto de Serviço
Até mesmo dentro de uma classe de dispositivo, como scanners de código de barras, os atributos de cada dispositivo podem variar consideravelmente entre modelos. Se seu aplicativo requer um atributo de dispositivo específico, talvez seja necessário inspecionar cada objeto de dispositivo conectado para determinar se o atributo é compatível. Por exemplo, talvez seu negócio requeira que os rótulos de serem criados usando um padrão de impressão de código de barras específico. Veja como você pode verificar se um scanner de código de barras conectado é compatível com uma simbologia. 

> [!NOTE]
> Uma simbologia é o mapeamento de idioma que um código de barras usa para codificar as mensagens.

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>Uso da classe Device.Capabilities
A classe **Device.Capabilities** é um atributo de todas as classes de dispositivo de Ponto de Serviço e pode ser usada para obter informações gerais sobre cada dispositivo. Por exemplo, este exemplo determina se um dispositivo é compatível com o relatório de estatísticas e, em caso afirmativo, recupera as estatísticas de quaisquer tipos compatíveis.

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>Declaração de um dispositivo de Ponto de Serviço
Antes de usar um dispositivo de Ponto de Serviço para entrada ou saída ativos, você deve declará-lo, concedendo ao aplicativo acesso exclusivo a muitas das suas funções. Este código mostra como declarar um dispositivo de scanner de código de barras, depois que você encontrou o dispositivo usando um dos métodos descritos anteriormente.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>Retenção do dispositivo
Ao usar um dispositivo de Ponto de Serviço em uma rede ou conexão Bluetooth, talvez você queira compartilhar o dispositivo com outros aplicativos na rede. (Para obter mais informações sobre isso, consulte [Sharing Devices](#sharing-a-device-between-apps).) Em outros casos, pode ser que você queira manter o dispositivo para uso prolongado. Este exemplo mostra como manter um scanner de código de barras declarado depois que outro aplicativo solicitou que o dispositivo seja lançado.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Entrada e saída

Depois que você declarou um dispositivo, você está quase pronto para usá-lo. Para receber a entrada do dispositivo, você deve configurar e habilitar um representante para receber dados. No exemplo abaixo, podemos declarar um dispositivo de scanner de código de barras, definir sua propriedade de decodificação e, por fim, chamar **EnableAsync** para habilitar a entrada decodificada do dispositivo. Como esse processo varia entre classes de dispositivo, para obter orientações sobre como configurar um representante para dispositivos que não são de código de barras, consulte a [amostra de aplicativo UWP](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors) relevante.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>Compartilhamento de um dispositivo entre aplicativos

Dispositivos de Ponto de Serviço geralmente são usados em casos onde mais de um aplicativo precisará acessá-los em um breve período.  Um dispositivo pode ser compartilhado quando conectado a vários aplicativos localmente (USB ou outra conexão com fio) ou por meio de uma rede Bluetooth ou IP. Dependendo das necessidades de cada aplicativo, pode ser que um processo precise descartar sua declaração no dispositivo. Esse código descarta nosso dispositivo de scanner de código de barras declarado, permitindo que outros aplicativos o declarem e o usem.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> As classes de dispositivos de Ponto de Serviço declarados e não declarados implementam [Interface IClosable](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Se um dispositivo for conectado a um aplicativo por meio de rede ou bluetooth, os objetos declarados e não declarados devem ser descartados antes que outro aplicativo possa se conectar.

## <a name="see-also"></a>Veja também
+ [Exemplo de scanner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemplo de caixa registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemplo de display de balcão](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemplo de leitor de tarjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemplo de impressora de Ponto de Serviço](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

