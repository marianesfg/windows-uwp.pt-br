---
author: stevewhims
description: O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele.
title: Portabilidade do Windows Runtime 8.x para UWP para E/S, dispositivo e modelo de aplicativo
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e15014e39ed6d980cbe80daa0a129ff83a021b9
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282232"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>Portabilidade do Windows Runtime 8.x para UWP para E/S, dispositivo e modelo de aplicativo




O tópico anterior era [Portando XAML e a interface do usuário](w8x-to-uwp-porting-xaml-and-ui.md).

O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário *ou* a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta.

## <a name="application-lifecycle-process-lifetime-management"></a>Ciclo de vida do aplicativo (gerenciamento do tempo de vida do processo)


Em um aplicativo Universal 8.1, há uma "janela de contagem" de dois segundos entre a entrada do aplicativo em inatividade e o acionamento do evento de suspensão pelo sistema. Usar essa janela de enumeração como tempo extra para o estado de suspensão não é seguro, e para um aplicativo da Plataforma Universal do Windows (UWP), sequer há janela de enumeração; o evento de suspensão é acionado assim que o aplicativo se torna inativo.

Para obter mais informações, consulte [Ciclo de vida do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt243287).

## <a name="background-audio"></a>Áudio em segundo plano


Para a propriedade [**Audiocategory**](https://msdn.microsoft.com/library/windows/apps/br227352) , **ForegroundOnlyMedia** e **BackgroundCapableMedia** foram preteridos para aplicativos do Windows 10. Use o modelo de aplicativo da Loja do Windows Phone. Para obter mais informações, consulte [Áudio em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt282140).

## <a name="detecting-the-platform-your-app-is-running-on"></a>Detectando a plataforma em que seu aplicativo está sendo executado


A maneira de pensar sobre alterações direcionadas de aplicativo ao Windows 10. O novo modelo conceitual é um aplicativo direcionado à UWP (Plataforma Universal do Windows) e executado em todos os dispositivos Windows. Ele também pode destacar recursos que sejam exclusivos de famílias de dispositivos específicas. Se necessário, o aplicativo também tem a opção de se limitar a uma ou mais famílias de dispositivos especificamente. Para saber mais sobre o que são famílias de dispositivos (e como decidir qual família de dispositivos priorizar), veja [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).

Se você tiver em seu aplicativo Universal 8.1 um código que detecte qual sistema operacional está em execução, então talvez seja necessário alterar isso, dependendo do motivo dessa lógica. Se o aplicativo estiver passando o valor e não agindo em relação a ele, então talvez você queira continuar a coletar informações do sistema operacional.

**Observação**  , recomendamos que você não use sistema operacional ou a família de dispositivos para detectar a presença de recursos. Identificar o sistema operacional ou a família de dispositivos atual normalmente não é a melhor maneira de determinar se um recurso de sistema operacional ou de família de dispositivos está presente. Em vez de detectar o sistema operacional ou a família de dispositivos (e o número de versão), teste a presença do próprio recurso (consulte [Compilação condicional e código adaptável](w8x-to-uwp-porting-to-a-uwp-project.md)). Se você precisar de um determinado sistema operacional ou uma família de dispositivos, certifique-se de usá-la como uma versão mínima compatível, em vez de criar o teste para essa versão.

 

Recomendamos várias técnicas para personalizar a interface do usuário do seu aplicativo em diferentes dispositivos. Continue a usar elementos de dimensionamento automático e painéis de layout dinâmico como você sempre fez. Em sua marcação XAML, continue a usar tamanhos em pixels efetivos (anteriormente conhecidos como pixels de exibição) para que sua interface do usuário se adapte a diferentes resoluções e fatores de escala (consulte [pixels efetivos, distância de exibição e fatores de escala](w8x-to-uwp-porting-xaml-and-ui.md).). E use os acionadores e setters adaptáveis do Gerenciador de Estado Visual para adaptar sua interface do usuário ao tamanho de janela (consulte [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).).

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

Além disso, consulte também [Compilação condicional e código adaptável](w8x-to-uwp-porting-to-a-uwp-project.md).

## <a name="location"></a>Localização


Quando um aplicativo que declara a funcionalidade de localização em seu manifesto de pacote de aplicativo é executado no Windows 10, o sistema solicita o consentimento do usuário final. Isso é verdadeiro se o aplicativo é um aplicativo da loja do Windows Phone ou um aplicativo do Windows 10. Portanto, se o aplicativo exibir a própria solicitação de consentimento personalizado, ou se ele fornecer um botão ativar/desativar, remova-a de maneira que o usuário final receba apenas uma solicitação.

 

 




