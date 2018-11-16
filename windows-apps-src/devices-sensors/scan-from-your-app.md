---
author: PatrickFarley
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: Digitalize a partir de seu aplicativo
description: Confira aqui como digitalizar conteúdos do seu aplicativo usando uma fonte de scanner de mesa, alimentador ou autoconfigurado.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f9128056cbb3b9218d164b243948d9dd16af0786
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6989181"
---
# <a name="scan-from-your-app"></a>Digitalizar de seu aplicativo


**APIs importantes**

-   [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250)
-   [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)
-   [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)

Aprenda aqui a digitalizar conteúdos do seu aplicativo usando uma fonte de scanner de mesa, alimentador ou autoconfigurado.

**Importante**as APIs de [**scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) fazem parte da [família de dispositivos](https://msdn.microsoft.com/library/windows/apps/Dn894631)da área de trabalho. Aplicativos podem usar essas APIs somente na versão da área de trabalho do Windows 10.

Para digitalizar de seu aplicativo, você deve primeiro listar os scanners disponíveis declarando um novo objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) e obtendo o tipo [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381). Somente scanners instalados localmente com drivers WIA são listados e disponibilizados a seu aplicativo.

Depois que seu aplicativo relacionou os scanners disponíveis, ele pode usar as configurações de digitalização configuradas automaticamente, com base no tipo de scanner, ou somente digitalizar usando a fonte de digitalização do scanner de mesa ou do alimentador disponível. Para usar as configurações automáticas, o scanner deve estar ativado para configuração automática e não pode estar equipado com scanner de mesa ou alimentador. Para obter mais informações, consulte [Digitalização Configurada Automaticamente](https://msdn.microsoft.com/library/windows/hardware/Ff539393).

## <a name="enumerate-available-scanners"></a>Enumerar os scanners disponíveis

O Windows não detecta scanners automaticamente. Você deve executar essa etapa para que seu aplicativo se comunique com o scanner. Nesse exemplo, a enumeração do dispositivo de scanner é feita usando o namespace [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459).

1.  Primeiro, adicione-os utilizando instruções de seu arquivo de definição de classe.

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  Em seguida, implemente um observador de dispositivo para iniciar a enumeração de scanners. Para obter mais informações, consulte [Enumerar dispositivos](enumerate-devices.md).

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  Crie um manipulador de eventos para quando um scanner for adicionado.

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =>
             {
                MainPage.Current.NotifyUser(String.Format("Scanner with device id {0} has been added", deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## <a name="scan"></a>Digitalizar

1.  **Obter um objeto ImageScanner**

Para cada tipo de enumeração [**ImageScannerScanSource**](https://msdn.microsoft.com/library/windows/apps/Dn264238), independentemente de ser **Default**, **AutoConfigured**, **Flatbed** ou **Feeder**, primeiro você deve criar um objeto [**ImageScanner**](https://msdn.microsoft.com/library/windows/apps/Dn263806) chamando o método [**ImageScanner.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.scanners.imagescanner.fromidasync), da seguinte maneira.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **Apenas digitalizar**

Para digitalizar com as configurações padrão, seu aplicativo conta com o namespace [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) para selecionar um scanner e digitalizar dessa origem. Nenhuma configuração de digitalização é alterada. Os scanners possíveis são configurado automaticamente, plano ou alimentador. Esse tipo de digitalização é o mais provável que produza uma operação de digitalização bem-sucedida, mesmo se digitalizar da fonte errada, como de scanner plano em vez de alimentador.

**Observação**se o usuário coloca o documento para digitalizar no alimentador, o scanner verificará da superfície em vez disso. Se o usuário tentar digitalizar de um alimentador vazio, a tarefa de digitalização não produzirá qualquer arquivo digitalizado.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **Digitalizar de origem Configurada automaticamente, Plana ou de Alimentador**

Seu aplicativo pode usar [Digitalização configurada automaticamente](https://msdn.microsoft.com/library/windows/hardware/Ff539393) do dispositivo, para digitalizar com as melhores configurações de digitalização. Com essa opção, o próprio dispositivo pode determinar as melhores configurações de digitalização, como modo de cor e resolução da digitalização, de acordo com o conteúdo que está sendo digitalizado. O dispositivo seleciona as configurações de digitalização no tempo de execução para cada nova tarefa de digitalização.

**Observação**nem todos os scanners dão suporte a esse recurso, portanto, o aplicativo deve verificar se o scanner é compatível com esse recurso antes de usar essa configuração.

Nesse exemplo, o aplicativo primeiro verifica se o scanner é capaz de usar configuração automática e então digitaliza. Para especificar se em plano ou alimentador, simplesmente substitua **AutoConfigured** por **Flatbed** ou **Feeder**.

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings.
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## <a name="preview-the-scan"></a>Visualizar a digitalização

Você pode adicionar código para visualizar a digitalização antes de digitalizar para uma pasta, como essa. No exemplo abaixo, o aplicativo verifica se o scanner **Flatbed** oferece suporte para visualização, então visualiza a digitalização.

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>Cancelar a digitalização

Você pode permitir que os usuários cancelem a digitalização no meio da tarefa por meio da opção digitalizar, como essa.

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## <a name="scan-with-progress"></a>Digitalização com progresso

1.  Crie um objeto **System.Threading.CancellationTokenSource**.

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  Configure o manipulador de eventos de progresso e obtenha o progresso da digitalização.

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>Digitalizando para a biblioteca de imagens

Os usuários podem digitalizar para qualquer pasta dinamicamente, usando a classe [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/BR207881), mas você deve declarar o recurso *Biblioteca de Imagens* no manifesto para permitir que os usuários digitalizem para essa pasta. Para obter mais informações sobre as funcionalidades do aplicativo, consulte [Declarações de funcionalidades do aplicativo](https://msdn.microsoft.com/library/windows/apps/Mt270968).
