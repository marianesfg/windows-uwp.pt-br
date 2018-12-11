---
description: O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele.
title: Portabilidade do WindowsPhone Silverlight para UWP para e/s, dispositivo e modelo de aplicativo '
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ef1814443b3831e514eafb3f5a0c58b7703126b
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8893297"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Portabilidade do WindowsPhone Silverlight para UWP para e/s, dispositivo e modelo de aplicativo


O tópico anterior era [Portando XAML e a interface do usuário](wpsl-to-uwp-porting-xaml-and-ui.md).

O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário ou a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta.

## <a name="application-lifecycle-process-lifetime-management"></a>Ciclo de vida do aplicativo (gerenciamento do tempo de vida do processo)

Seu aplicativo WindowsPhone Silverlight contém o código para salvar e restaurar o estado do aplicativo e o estado da exibição para que possa ser e reativado posteriormente. O ciclo de vida de aplicativos da plataforma Universal do Windows (UWP) tem forte paralelo com WindowsPhone dos aplicativos do Silverlight, já que eles foram criados com o mesmo objetivo de maximizar os recursos disponíveis para qualquer aplicativo que o usuário escolheu ter o em primeiro plano a qualquer momento. Você descobrirá que seu código se adaptará facilmente ao novo sistema.

**Observação**  pressionar o botão **Voltar** do hardware automaticamente encerra um aplicativo do WindowsPhone Silverlight. Pressionar o botão **Voltar** em um dispositivo móvel *não* encerra automaticamente um aplicativo UWP. Em vez disso, ele fica suspenso e, em seguida, pode ser encerrado. Porém, esses detalhes são transparentes para um aplicativo que responde corretamente a eventos do ciclo de vida do aplicativo.

Uma "janela de enumeração" é o período entre a entrada do aplicativo em inatividade e o acionamento do evento de suspensão pelo sistema. Para um aplicativo UWP, não existe uma janela de enumeração; o evento de suspensão é acionado assim que o aplicativo se torna inativo.

Para obter mais informações, consulte [Ciclo de vida do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt243287).

## <a name="camera"></a>Câmera

Código de captura de câmera WindowsPhone Silverlight usa as classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera**ou **Microsoft.Phone.Tasks.CameraCaptureTask** . Para portar esse código para a UWP (Plataforma Universal do Windows), você pode usar a classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). Há um exemplo de código no tópico [**CapturePhotoToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700836). Esse método permite que você capture uma foto para um arquivo de armazenamento, e requer o **microfone** e **webcam**[**funcionalidades do dispositivo**](https://msdn.microsoft.com/library/windows/apps/dn934747) para ser definido no manifesto do pacote do aplicativo.

Outra opção é a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) , que também requer as[**funcionalidades do dispositivo**](https://msdn.microsoft.com/library/windows/apps/dn934747)do **microfone** e **webcam**.

Não há suporte a aplicativos de lente para aplicativos UWP.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Detectando a plataforma em que seu aplicativo é executado.

A maneira de pensar sobre alterações direcionadas aplicativo com Windows 10. O novo modelo conceitual é um aplicativo direcionado à UWP (Plataforma Universal do Windows) e executado em todos os dispositivos Windows. Ele também pode destacar recursos que sejam exclusivos de famílias de dispositivos específicas. Se necessário, o aplicativo também tem a opção de se limitar a uma ou mais famílias de dispositivos especificamente. Para saber mais sobre o que são famílias de dispositivos (e como decidir qual família de dispositivos priorizar), veja [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).

**Observação**  , recomendamos que você não use família ou sistema operacional para detectar a presença de recursos. Identificar o sistema operacional ou a família de dispositivos atual normalmente não é a melhor maneira de determinar se um recurso de sistema operacional ou de família de dispositivos está presente. Em vez de detectar o sistema operacional ou a família de dispositivos (e o número de versão), teste a presença do próprio recurso (consulte [Compilação condicional e código adaptável](wpsl-to-uwp-porting-to-a-uwp-project.md)). Se você precisar de um determinado sistema operacional ou uma família de dispositivos, certifique-se de usá-la como uma versão mínima compatível, em vez de criar o teste para essa versão.

Recomendamos várias técnicas para personalizar a interface do usuário do seu aplicativo em diferentes dispositivos. Continue a usar elementos de dimensionamento automático e painéis de layout dinâmico como você sempre fez. Em sua marcação XAML, continue usando tamanhos em pixels efetivos (anteriormente conhecidos como pixels de exibição) para que sua interface do usuário se adapte a diferentes resoluções e fatores de escala (consulte [Pixels de exibição/efetivos, distância de exibição e fatores de escala](wpsl-to-uwp-porting-xaml-and-ui.md).). E use os acionadores e setters adaptáveis do Gerenciador de Estado Visual para adaptar sua interface do usuário ao tamanho de janela (consulte [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).).

No entanto, se você tiver um cenário em que seja inevitável detectar a família de dispositivos, faça isso. Neste exemplo, usamos a classe [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) para navegar até uma página adaptada à família de dispositivos móveis em que seja apropriado, e nos certificamos de retornar a uma página.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Seu aplicativo também pode determinar a família de dispositivos que está sendo executada nos fatores de seleção de recursos que estão em vigor. O exemplo abaixo mostra como fazer isso de maneira imperativa, e o tópico [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) descreve o caso de uso mais comum da classe no carregamento de recursos específicos à família de dispositivos com base no fator de família de dispositivos.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Além disso, consulte também [Compilação condicional e código adaptável](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>Status do dispositivo

Um aplicativo do WindowsPhone Silverlight pode usar a classe **devicestatus** para obter informações sobre o dispositivo no qual o aplicativo é executado. Embora não haja equivalente direto da UWP para o namespace **Microsoft.Phone.Info**, aqui estão algumas propriedades e eventos que você pode usar em um aplicativo UWP no lugar das chamadas aos membros da classe **DeviceStatus**.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Propriedades **ApplicationCurrentMemoryUsage** e **ApplicationCurrentMemoryUsageLimit** | Propriedades [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/dn633832) e [**AppMemoryUsageLimit**](https://msdn.microsoft.com/library/windows/apps/dn633836)                                                                                                                                    |
| Propriedade **ApplicationPeakMemoryUsage**                                                 | Use as ferramentas de criação de perfil de memória no Visual Studio. Para saber mais, veja [Analisar o uso da memória](http://msdn.microsoft.com/library/windows/apps/dn645469.aspx).                                                                                                                                                                          |
| Propriedade **DeviceFirmwareVersion**                                                      | Propriedade [**EasClientDeviceInformation.SystemFirmwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608144) (somente para famílias de dispositivos de desktop)                                                                                                                                                                             |
| Propriedade **DeviceHardwareVersion**                                                      | Propriedade [**EasClientDeviceInformation.SystemHardwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608145) (somente família de dispositivos de desktop)                                                                                                                                                                             |
| Propriedade **DeviceManufacturer**                                                         | Propriedade [**EasClientDeviceInformation.SystemManufacturer**](https://msdn.microsoft.com/library/windows/apps/hh701398) (somente família de dispositivos de desktop)                                                                                                                                                                                |
| Propriedade **DeviceName**                                                                 | Propriedade [**EasClientDeviceInformation.SystemProductName**](https://msdn.microsoft.com/library/windows/apps/hh701401) (somente família de dispositivos de desktop)                                                                                                                                                                                 |
| Propriedade **DeviceTotalMemory**                                                          | Sem equivalente                                                                                                                                                                                                                                                                                                                      |
| Propriedade **IsKeyboardDeployed**                                                         | Sem equivalente. Essa propriedade fornece informações sobre teclados de hardware para dispositivos móveis, que não são comumente usados.                                                                                                                                                                                                        |
| Propriedade **IsKeyboardPresent**                                                          | Sem equivalente. Essa propriedade fornece informações sobre teclados de hardware para dispositivos móveis, que não são comumente usados.                                                                                                                                                                                                        |
| Evento **KeyboardDeployedChanged**                                                       | Sem equivalente. Essa propriedade fornece informações sobre teclados de hardware para dispositivos móveis, que não são comumente usados.                                                                                                                                                                                                        |
| Propriedade **PowerSource**                                                                | Sem equivalente                                                                                                                                                                                                                                                                                                                      |
| Evento **PowerSourceChanged**                                                            | Manipule o evento [**RemainingChargePercentChanged**](https://msdn.microsoft.com/library/windows/apps/jj207240) (somente família de dispositivos móveis). O evento é acionado quando o valor da propriedade [**RemainingChargePercent**](https://msdn.microsoft.com/library/windows/apps/jj207239) (somente família de dispositivos móveis) diminui em 1%. |

## <a name="location"></a>Localização

Quando um aplicativo que declara a funcionalidade de localização em seu manifesto de pacote de aplicativo é executado no Windows 10, o sistema solicita o consentimento do usuário final. Portanto, se o aplicativo exibir a própria solicitação de consentimento personalizado, ou se ele fornecer um botão ativar/desativar, remova-a de maneira que o usuário final receba apenas uma solicitação.

## <a name="orientation"></a>Orientação

O equivalente ao aplicativo UWP das propriedades **PhoneApplicationPage.SupportedOrientations** e **Orientation** é o elemento [**uap:InitialRotationPreference**](https://msdn.microsoft.com/library/windows/apps/dn934798) no manifesto do pacote do aplicativo. Selecione a guia **Aplicativo** caso ela ainda não esteja selecionada e marque uma ou mais caixas de seleção em **Rotações suportadas** para registrar as preferências.

Entretanto, você é incentivado a projetar a interface do usuário do aplicativo UWP de maneira que ela tenha boa aparência, independentemente do tamanho da tela e da orientação do dispositivo. Há mais informações sobre isso em [Portabilidade para fatores forma e experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md), que é o tópico após o próximo.

O próximo tópico é [Portando negócios e as camadas de dados](wpsl-to-uwp-business-and-data.md).

