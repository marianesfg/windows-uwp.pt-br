---
ms.assetid: 28B30708-FE08-4BE9-AE11-5429F963C330
title: GATT de Bluetooth
description: Este artigo fornece uma visão geral do GATT (Perfil de Atributo Genérico) de Bluetooth para aplicativos UWP (Plataforma Universal do Windows), juntamente com o código de exemplo para três cenários comuns de GATT.
---
# GATT de Bluetooth

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** APIs importantes

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Este artigo fornece uma visão geral do GATT (Perfil de Atributo Genérico) de Bluetooth para aplicativos UWP (Plataforma Universal do Windows), juntamente com o código de exemplo para três cenários comuns de GATT: recuperação de dados de Bluetooth, controle de um dispositivo de termômetro Bluetooth LE e controle da apresentação de dados de dispositivo Bluetooth LE.

## Visão geral

Os desenvolvedores podem usar as APIs no namespace [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) para acessar serviços, descritores e características do Bluetooth LE. Os dispositivos Bluetooth LE expõem sua funcionalidade por meio de coleção de:

-   Serviços primários
-   Serviços incluídos
-   Características
-   Descritores

Serviços primários definem o contrato funcional do dispositivo LE e contêm uma coleção de características que definem o serviço. Essas características, por sua vez, contêm descritores que descrevem as características.

As APIs de GATT de Bluetooth expõem objetos e funções, em vez de acessarem o transporte bruto. No nível do driver, os serviços primários são enumerados como Nós de Dispositivo Filho do dispositivo Bluetooth LE usando as APIs do [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459).

As APIs de GATT de Bluetooth também permitem que os desenvolvedores usem dispositivos Bluetooth LE com a capacidade de executar as seguintes tarefas:

-   Executar descoberta de Serviço / Característica / Descritor
-   Ler e gravar valores de Características / Descritor
-   Registrar o seu retorno de chamada para o evento ValueChanged de Característica

As APIs de GATT de Bluetooth simplificam o desenvolvimento lidando com propriedades comuns e fornecendo padrões razoáveis para adicionar no gerenciamento de dispositivo e configuração. Elas fornecem meios para os desenvolvedores acessarem a funcionalidade de um dispositivo Bluetooth LE de um aplicativo.

Para criar uma implementação útil, um desenvolvedor precisa antes conhecer os serviços GATT e características que o aplicativo pretende consumir, e para processar os valores característicos específicos, como aqueles dados binários fornecidos pela API, são transformados em dados úteis antes de serem apresentados ao usuário. As APIs de GATT de Bluetooth expõem somente primitivas básicas necessárias para comunicação com um dispositivo Bluetooth LE. Para interpretar dados, um perfil de aplicativo deve ser definido, tanto pelo perfil padrão de um Bluetooth SIG, como por um perfil personalizado implementado por um fornecedor de dispositivo. Um perfil cria um contrato de ligação entre o aplicativo e o dispositivo, como o que os dados de intercâmbio representam e como ele os interpreta.

Por conveniência, o Bluetooth SIG mantém uma [lista de perfis públicos](http://go.microsoft.com/fwlink/p/?LinkID=317977) disponíveis.

## Recuperar dados de Bluetooth

Nesse exemplo, o aplicativo usa medições de temperatura de um dispositivo Bluetooth que implementa o Serviço de Termômetro de Integridade do Bluetooth LE. O aplicativo especifica que quer ser notificado quando uma nova medição de temperatura estiver disponível. Ao registrar um manipulador de eventos para o evento "Valor característico do termômetro alterado", o aplicativo receberá notificações de evento de valor característica alterado enquanto estiver sendo executado em primeiro plano.

Observe que, quando o aplicativo está suspenso, ele deve liberar todos os recursos do dispositivo e, quando retomar, deve executar enumeração do dispositivo e inicialização novamente.

```csharp
double convertTemperatureData(byte[] temperatureData)
{
    // Read temperature data in IEEE 11703 floating point format
    // temperatureData[0] contains flags about optional data - not used
    UInt32 mantissa = ((UInt32)temperatureData[3] << 16) |
        ((UInt32)temperatureData[2] << 8) |
        ((UInt32)temperatureData[1]);

    Int32 exponent = (Int32)temperatureData[4];

    return mantissa * Math.Pow(10.0, exponent);
}

async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService firstThermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic thermometerCharacteristic =
        firstThermometerService.GetCharacteristics(
            GattCharacteristicUuids.TemperatureMeasurement)[0];

    thermometerCharacteristic.ValueChanged += temperatureMeasurementChanged;

    await thermometerCharacteristic
        .WriteClientCharacteristicConfigurationDescriptorAsync(
            GattClientCharacteristicConfigurationDescriptorValue.Notify);
}

void temperatureMeasurementChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte[] temperatureData = new byte[eventArgs.CharacteristicValue.Length];
    Windows.Storage.Streams.DataReader.FromBuffer(
        eventArgs.CharacteristicValue).ReadBytes(temperatureData);

    var temperatureValue = convertTemperatureData(temperatureData);

    temperatureTextBlock.Text = temperatureValue.ToString();
}
```

```cpp
double MainPage::ConvertTemperatureData(
    const Array<unsigned char>^ temperatureData)
{
    unsigned mantissa = ((unsigned)temperatureData[3] << 16) |
        ((unsigned)temperatureData[2] << 8) |
        ((unsigned)temperatureData[1]);

    int exponent = (int)temperatureData[4];

    return mantissa * pow(10.0, (double)exponent);
}

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id))
            .then([this] (GattDeviceService^ firstThermometerService) 
        {
            GattCharacteristic^ thermometerCharacteristic = 
                firstThermometerService->GetCharacteristics(
                    GattCharacteristicUuids::TemperatureMeasurement)
                        ->GetAt(0);

            thermometerCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>(
                        this, &amp;MainPage::TemperatureMeasurementChanged);

            create_task(thermometerCharacteristic->
                WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}


void MainPage::TemperatureMeasurementChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    auto temperatureData =  ref new Array<unsigned char>(
        eventArgs->CharacteristicValue->Length);
    DataReader::FromBuffer(eventArgs->CharacteristicValue)
        ->ReadBytes(temperatureData);

    double temperatureValue = ConvertTemperatureData(temperatureData);
    std::wstringstream str;
    str << temperatureValue;

    temperatureTextBlock->Text = ref new String(str.str().c_str());
}
```

## Controlar um termômetro Bluetooth LE

Neste exemplo, um aplicativo UWP age como um controlador para um dispositivo de Termômetro Bluetooth LE fictício. O dispositivo também declara uma característica de formato que permite que os usuários recuperem a leitura do valor em graus Celsius ou Fahrenheit, além das características padrão do perfil de [**HealthThermometer**](https://msdn.microsoft.com/library/windows/apps/Dn297603). O aplicativo utiliza transações de gravação confiáveis para garantir que o formato e o intervalo da medição sejam definidos como um único valor.

```csharp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid = 
    new Guid("{00000000-0000-0000-0000-000000000010}");

// Constant representing a Fahrenheit scale temperature measurement
const byte FahrenheitReading = 1;
async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService thermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic intervalCharacteristic = thermometerService
        .GetCharacteristics(GattCharacteristicUuids.MeasurementInterval)[0];

    GattCharacteristic formatCharacteristic = thermometerService
        .GetCharacteristics(formatCharacteristicUuid)[0];

    GattReliableWriteTransaction gattTransaction = 
        new GattReliableWriteTransaction();

    var writer = new Windows.Storage.Streams.DataWriter();

    // Get a temperature reading every 60 seconds
    writer.WriteUInt16(60);

    gattTransaction.WriteValue(
        intervalCharacteristic, 
        writer.DetachBuffer());

    // Get the reading on the Fahrenheit scale
    writer.WriteByte(FahrenheitReading);

    gattTransaction.WriteValue(
        formatCharacteristic, 
        writer.DetachBuffer());

    GattCommunicationStatus status = await gattTransaction.CommitAsync();

    if (GattCommunicationStatus.Unreachable == status)
    {
        statusTextBlock.Text = "Writing to your device failed !";
    }
}
```

```cpp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid(0x00000000, 0x0000, 0x0000, 0x00, 0x00, 
                                  0x00, 0x00, 0x00, 0x00, 0x00, 0x10);

// Constant representing a Fahrenheit scale temperature measurement
const unsigned char FAHRENHEIT_READING = 1;

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ thermometerService) 
        {
            GattCharacteristic^ intervalCharacteristic = 
                thermometerService->GetCharacteristics(
                    GattCharacteristicUuids::MeasurementInterval)
                        ->GetAt(0);

            GattCharacteristic^ formatCharacteristic = 
                thermometerService->GetCharacteristics(
                    formatCharacteristicUuid)->GetAt(0);

            GattReliableWriteTransaction^ gattTransaction = 
                ref new GattReliableWriteTransaction();

            DataWriter^ writer = ref new DataWriter();

            // Get a temperature reading every 60 seconds
            writer->WriteUInt16(60);

            gattTransaction->WriteValue(
                intervalCharacteristic, 
                writer->DetachBuffer());

            writer->WriteByte(FAHRENHEIT_READING);

            gattTransaction->WriteValue(
                formatCharacteristic, 
                writer->DetachBuffer());

            create_task(gattTransaction->CommitAsync())
                .then([this] (GattCommunicationStatus status) 
            {
                if (GattCommunicationStatus::Unreachable == status) 
                { 
                    statusTextBlock->Text = 
                        ref new String(L"Writing to your device failed !");
                }
            });
        });
    });

```

## Controlar a apresentação de dados do dispositivo Bluetooth LE

Os dispositivos Bluetooth LE podem exibir um serviço de bateria que fornece o nível de bateria atual ao usuário. O serviço de bateria inclui um descritor [**PresentationFormats**](https://msdn.microsoft.com/library/windows/apps/Dn263742) opcional que permite alguma flexibilidade na interpretação dos dados do nível da bateria. Esse cenário fornece um exemplo de um aplicativo que funciona com esse dispositivo e utiliza a propriedade **PresentationFormats** para formatar um valor de característica antes de apresentá-lo ao usuário.

```csharp
async void Initialize()
{
    var batteryServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(GattServiceUuids.Battery),
        null);

    if (batteryServices.Count > 0)
    {
        // Use the first Battery service on the system
        GattDeviceService batteryService = await GattDeviceService
            .FromIdAsync(batteryServices[0].Id);

        // Use the first Characteristic of that Service
        GattCharacteristic batteryLevelCharacteristic =
            batteryService.GetCharacteristics(
                GattCharacteristicUuids.BatteryLevel)[0];

        batteryLevelCharacteristic.ValueChanged += batteryLevelChanged;
    }
    else
    {
        statusTextBlock.Text = "No Battery services found !";
    }
}

void batteryLevelChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte levelData = Windows.Storage.Streams.DataReader
        .FromBuffer(eventArgs.CharacteristicValue).ReadByte();

    double levelValue;

    if (sender.PresentationFormats.Count > 0)
    {
        levelValue = levelData * 
            Math.Pow(10.0, sender.PresentationFormats[0].Exponent);
    }
    else
    {
        levelValue = (double)levelData;
    }

    batteryLevelTextBlock.Text = levelValue.ToString();
}
```

```cpp
void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::Battery), 
        nullptr)).then([this] (DeviceInformationCollection^ batteryServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            batteryServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ batteryService) 
        {
            GattCharacteristic^ batteryLevelCharacteristic = 
                batteryService->GetCharacteristics(
                    GattCharacteristicUuids::BatteryLevel)->GetAt(0);

            batteryLevelCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>
                    (this, &amp;MainPage::BatteryLevelChanged);

            create_task(batteryLevelCharacteristic
                ->WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}

void MainPage::BatteryLevelChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    unsigned char batteryLevelData = DataReader::FromBuffer(
        eventArgs->CharacteristicValue)->ReadByte();

    // if this characteristic has a presentation format
    // use that information to format the value
    double batteryLevelValue;
    if (sender->PresentationFormats->Size > 0)
    {
        batteryLevelValue = batteryLevelData * 
            pow(10.0, sender->PresentationFormats->GetAt(0)->Exponent);
    }
    else
    {
        batteryLevelValue = batteryLevelData;
    }

    std::wstringstream str;
    str << batteryLevelValue;
    batteryLevelTextBlock->Text = 
        ref new String(str.str().c_str());
}
```





<!--HONumber=Mar16_HO1-->


