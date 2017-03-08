---
author: msatranjr
title: "Anúncios de Bluetooth"
description: "Esta seção contém artigos sobre como integrar anúncios de Bluetooth de baixa energia (LE) a apps UWP (Plataforma Universal do Windows) por meio do usuário de APIs AdvertisementWatcher e AdvertisementPublisher."
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: bfdb1b218676503699674c97fc962ad8161769dd
ms.lasthandoff: 02/08/2017

---

# <a name="bluetooth-le-advertisements"></a>Anúncios de Bluetooth LE

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

Este artigo fornece uma visão geral dos beacons de anúncios de Bluetooth de baixa energia (LE) para apps UWP (Plataforma Universal do Windows).  

## <a name="overview"></a>Visão geral

Há duas funções principais que um desenvolvedor pode executar usando as APIs de anúncio LE:

-   [Inspetor de anúncio](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx): escuta beacons nas proximidades e filtra com base na carga ou proximidade.  
-   [Fornecedor de anúncio](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): define uma carga para o Windows anunciar em nome de um desenvolvedor.  

O código de exemplo completo está disponível no [Exemplo de anúncio de Bluetooth](http://go.microsoft.com/fwlink/p/?LinkId=619990) no Github

## <a name="basic-setup"></a>Configuração básica

Para usar a funcionalidade Bluetooth LE básica em um app da Plataforma Universal do Windows, você deve selecionar a funcionalidade Bluetooth no Package.appxmanifest.

1. Abra Package.appxmanifest
2. Vá para a guia Capabilities
3. Encontre Bluetooth na lista à esquerda e marque a caixa ao lado.

## <a name="publishing-advertisements"></a>Anúncios de publicação

Anúncios de Bluetooth LE permitem que seu dispositivo envie constantemente beacons de uma carga específica, chamada de anúncio. Esse anúncio pode ser visto por qualquer dispositivo compatível com Bluetooth LE nas proximidades, se estiver configurado para escutar esse anúncio específico.

**Observação** Para a privacidade do usuário, o tempo de vida do anúncio está vinculado ao do app. Você pode criar BluetoothLEAdvertisementPublisher e chamar Start em uma tarefa em segundo plano para o anúncio em segundo plano. Para obter mais informações sobre tarefas em segundo plano, consulte [Início, retomada e tarefas em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/index).

### <a name="basic-publishing"></a>Publicação básica

Há muitas maneiras diferentes de adicionar dados a um anúncio. Este exemplo mostra uma forma comum de criar um anúncio específico da empresa. 

Primeiro, crie o fornecedor de anúncio que controla se o dispositivo sinalizará ou não um anúncio específico.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

Depois, crie uma seção de dados personalizados. Este exemplo usa um valor **CompanyId** não atribuído *0xFFFE* e adiciona o texto *Hello World* ao anúncio. 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

Agora que o fornecedor foi criado e configurado, você pode chamar **Start** para começar o anúncio.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>Monitorando anúncios

### <a name="basic-watching"></a>Monitoramento básico

O código a seguir demonstra como criar um inspetor de anúncio de Bluetooth LE, definir um retorno de chamada e começar a monitorar todos os anúncios LE.

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
```    

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>Verificação ativa
Para também receber anúncios de resposta de verificação, defina os itens a seguir depois de criar o inspetor. Observe que isso causa maior consumo de energia e não está disponível nos modos em segundo plano.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>Monitorando um padrão específico de anúncio

Às vezes, você deseja escutar um anúncio específico. Nesse caso, escute um anúncio que contenha uma carga com uma empresa fictícia (identificada como 0xFFFE) e a cadeia de caracteres *Hello World* no anúncio. Isso pode ser combinado com o exemplo Publicação Básica para ter uma máquina Windows fazendo o anúncio e outra máquina fazendo o monitoramento. Certifique-se de definir esse filtro de anúncio antes de iniciar o inspetor!

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>Monitorando um anúncio nas proximidades

Às vezes, você quer disparar o inspetor apenas quando o anúncio do dispositivo estiver dentro do alcance. Você pode definir seu próprio alcance, mas observe que os valores serão recortados para estar entre 0 e -128. 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>Medindo a distância

Quando o retorno de chamada do inspetor de Bluetooth LE é disparado, os eventArgs incluem um valor RSSI que informa a intensidade do sinal recebido (a intensidade do sinal Bluetooth).

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

Isso pode ser convertido aproximadamente em distância, mas não deve ser usado para medir distâncias reais, pois cada rádio é diferente. Diferentes fatores ambientais podem dificultar a medição da distância (como paredes, capas ao redor do rádio ou até mesmo a umidade do ar).

Uma alternativa para julgar a distância pura é definir "buckets". Os rádios tendem a informar 0 para -50 DBm quando estão muito próximos, -50 a -90 quando estão a uma distância média e abaixo de -90 quando estão distantes. Tentativa e erro é a melhor forma de determinar quais devem ser esses buckets para seu app.
