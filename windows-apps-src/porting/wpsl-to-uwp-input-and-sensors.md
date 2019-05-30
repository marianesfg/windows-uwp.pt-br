---
description: O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele.
title: Portabilidade de Windows Phone Silverlight para UWP para o modelo de e/s, o dispositivo e o aplicativo '
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a62fcb4a208a52fd77be2a9913e265b12bf31f43
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372182"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Portabilidade de e/s, o dispositivo e o modelo de aplicativo Windows Phone Silverlight para UWP


O tópico anterior era [Portando XAML e a interface do usuário](wpsl-to-uwp-porting-xaml-and-ui.md).

O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário ou a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta.

## <a name="application-lifecycle-process-lifetime-management"></a>Ciclo de vida do aplicativo (gerenciamento do tempo de vida do processo)

Seu aplicativo do Windows Phone Silverlight contém código para salvar e restaurar o estado do seu aplicativo e seu estado de exibição para dar suporte a que está sendo marcado para exclusão e subsequentemente reativado. Ciclo de vida de aplicativos da plataforma Universal do Windows (UWP) tem fortes paralelos com de aplicativos do Windows Phone Silverlight, uma vez que eles são projetados com o mesmo objetivo de maximizar os recursos disponíveis para qualquer aplicativo que o usuário tiver optado por ter em primeiro plano a qualquer momento. Você descobrirá que seu código se adaptará facilmente ao novo sistema.

**Observação**    pressionando o hardware **volta** botão encerra automaticamente um aplicativo do Windows Phone Silverlight. Pressionar o botão **Voltar** em um dispositivo móvel *não* encerra automaticamente um aplicativo UWP. Em vez disso, ele fica suspenso e, em seguida, pode ser encerrado. Porém, esses detalhes são transparentes para um aplicativo que responde corretamente a eventos do ciclo de vida do aplicativo.

Uma "janela de enumeração" é o período entre a entrada do aplicativo em inatividade e o acionamento do evento de suspensão pelo sistema. Para um aplicativo UWP, não existe uma janela de enumeração; o evento de suspensão é acionado assim que o aplicativo se torna inativo.

Para obter mais informações, consulte [Ciclo de vida do aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle).

## <a name="camera"></a>Câmera

Código de captura de câmera do Windows Phone Silverlight usa o **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera**, ou **Microsoft.Phone.Tasks.CameraCaptureTask**classes. Para portar esse código para a UWP (Plataforma Universal do Windows), você pode usar a classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture). Há um exemplo de código no tópico [**CapturePhotoToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync). Esse método permite que você capture uma foto para um arquivo de armazenamento, e requer **microfone** e **webcam** como  [**funcionalidades de dispositivo**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) sejam definidas no manifesto do pacote do aplicativo.

Outra opção é a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI), que também requer **microfone** e **webcam** como  [**funcionalidades do dispositivo**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability).

Não há suporte a aplicativos de lente para aplicativos UWP.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Detectando a plataforma em que seu aplicativo é executado.

A maneira de pensar sobre alterações de direcionamento de aplicativo com o Windows 10. O novo modelo conceitual é um aplicativo direcionado à UWP (Plataforma Universal do Windows) e executado em todos os dispositivos Windows. Ele também pode destacar recursos que sejam exclusivos de famílias de dispositivos específicas. Se necessário, o aplicativo também tem a opção de se limitar a uma ou mais famílias de dispositivos especificamente. Para saber mais sobre o que são famílias de dispositivos (e como decidir qual família de dispositivos priorizar), veja [Guia para aplicativos UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

**Observação**    é recomendável que você não usa sistema operacional ou da família do dispositivo para detectar a presença de recursos. Identificar o sistema operacional ou a família de dispositivos atual normalmente não é a melhor maneira de determinar se um recurso de sistema operacional ou de família de dispositivos está presente. Em vez de detectar o sistema operacional ou a família de dispositivos (e o número de versão), teste a presença do próprio recurso (consulte [Compilação condicional e código adaptável](wpsl-to-uwp-porting-to-a-uwp-project.md)). Se você precisar de um determinado sistema operacional ou uma família de dispositivos, certifique-se de usá-la como uma versão mínima compatível, em vez de criar o teste para essa versão.

Recomendamos várias técnicas para personalizar a interface do usuário do seu aplicativo em diferentes dispositivos. Continue a usar elementos de dimensionamento automático e painéis de layout dinâmico como você sempre fez. Em sua marcação XAML, continue usando tamanhos em pixels efetivos (anteriormente conhecidos como pixels de exibição) para que sua interface do usuário se adapte a diferentes resoluções e fatores de escala (consulte [Pixels de exibição/efetivos, distância de exibição e fatores de escala](wpsl-to-uwp-porting-xaml-and-ui.md).). E use os acionadores e setters adaptáveis do Gerenciador de Estado Visual para adaptar sua interface do usuário ao tamanho de janela (consulte [Guia para aplicativos UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).).

No entanto, se você tiver um cenário em que seja inevitável detectar a família de dispositivos, faça isso. Neste exemplo, usamos a classe [**AnalyticsVersionInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) para navegar até uma página adaptada à família de dispositivos móveis em que seja apropriado, e nos certificamos de retornar a uma página.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Seu aplicativo também pode determinar a família de dispositivos que está sendo executada nos fatores de seleção de recursos que estão em vigor. O exemplo abaixo mostra como fazer isso de maneira imperativa, e o tópico [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) descreve o caso de uso mais comum da classe no carregamento de recursos específicos à família de dispositivos com base no fator de família de dispositivos.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Além disso, consulte também [Compilação condicional e código adaptável](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>Status do dispositivo

Um aplicativo do Windows Phone Silverlight pode usar o **Microsoft.Phone.Info.DeviceStatus** classe para obter informações sobre o dispositivo no qual o aplicativo está em execução. Embora não haja equivalente direto da UWP para o namespace **Microsoft.Phone.Info**, aqui estão algumas propriedades e eventos que você pode usar em um aplicativo UWP no lugar das chamadas aos membros da classe **DeviceStatus**.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Propriedades **ApplicationCurrentMemoryUsage** e **ApplicationCurrentMemoryUsageLimit** | [**MemoryManager.AppMemoryUsage** ](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusage) e [ **AppMemoryUsageLimit** ](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimit) propriedades                                                                                                                                    |
| Propriedade **ApplicationPeakMemoryUsage**                                                 | Use as ferramentas de criação de perfil de memória no Visual Studio. Para saber mais, veja [Analisar o uso da memória](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).                                                                                                                                                                          |
| Propriedade **DeviceFirmwareVersion**                                                      | [**EasClientDeviceInformation.SystemFirmwareVersion** ](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion) propriedade (somente a família de dispositivos da área de trabalho)                                                                                                                                                                             |
| Propriedade **DeviceHardwareVersion**                                                      | [**EasClientDeviceInformation.SystemHardwareVersion** ](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion) propriedade (somente a família de dispositivos da área de trabalho)                                                                                                                                                                             |
| Propriedade **DeviceManufacturer**                                                         | [**EasClientDeviceInformation.SystemManufacturer** ](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer) propriedade (somente a família de dispositivos da área de trabalho)                                                                                                                                                                                |
| Propriedade **DeviceName**                                                                 | [**EasClientDeviceInformation.SystemProductName** ](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname) propriedade (somente a família de dispositivos da área de trabalho)                                                                                                                                                                                 |
| Propriedade **DeviceTotalMemory**                                                          | Sem equivalente                                                                                                                                                                                                                                                                                                                      |
| Propriedade **IsKeyboardDeployed**                                                         | Sem equivalente. Essa propriedade fornece informações sobre teclados de hardware para dispositivos móveis, que não são comumente usados.                                                                                                                                                                                                        |
| Propriedade **IsKeyboardPresent**                                                          | Sem equivalente. Essa propriedade fornece informações sobre teclados de hardware para dispositivos móveis, que não são comumente usados.                                                                                                                                                                                                        |
| Evento **KeyboardDeployedChanged**                                                       | Sem equivalente. Essa propriedade fornece informações sobre teclados de hardware para dispositivos móveis, que não são comumente usados.                                                                                                                                                                                                        |
| Propriedade **PowerSource**                                                                | Sem equivalente                                                                                                                                                                                                                                                                                                                      |
| Evento **PowerSourceChanged**                                                            | Manipule o evento [**RemainingChargePercentChanged**](https://docs.microsoft.com/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged) (somente família de dispositivos móveis). O evento é acionado quando o valor da propriedade [**RemainingChargePercent**](https://docs.microsoft.com/uwp/api/windows.phone.devices.power.battery.remainingchargepercent) (somente família de dispositivos móveis) diminui em 1%. |

## <a name="location"></a>Location

Quando um aplicativo que declara a funcionalidade de localização em suas execuções de manifesto de pacote de aplicativo no Windows 10, o sistema solicitará que o usuário final para consentimento. Portanto, se o aplicativo exibir a própria solicitação de consentimento personalizado, ou se ele fornecer um botão ativar/desativar, remova-a de maneira que o usuário final receba apenas uma solicitação.

## <a name="orientation"></a>Orientação

O equivalente ao aplicativo UWP das propriedades **PhoneApplicationPage.SupportedOrientations** e **Orientation** é o elemento [**uap:InitialRotationPreference**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen) no manifesto do pacote do aplicativo. Selecione a guia **Aplicativo** caso ela ainda não esteja selecionada e marque uma ou mais caixas de seleção em **Rotações suportadas** para registrar as preferências.

Entretanto, você é incentivado a projetar a interface do usuário do aplicativo UWP de maneira que ela tenha boa aparência, independentemente do tamanho da tela e da orientação do dispositivo. Há mais informações sobre isso em [Portabilidade para fatores forma e experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md), que é o tópico após o próximo.

O próximo tópico é [Portando negócios e as camadas de dados](wpsl-to-uwp-business-and-data.md).

