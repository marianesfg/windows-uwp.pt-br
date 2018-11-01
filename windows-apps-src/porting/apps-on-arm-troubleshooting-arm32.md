---
title: Solução de problemas de aplicativos UWP ARM32
author: msatranjr
description: Problemas comuns com aplicativos ARM32 quando executados em ARM e como corrigi-los.
ms.author: misatran
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, sempre conectado, aplicativos ARM32 no ARM, windows 10 no ARM, solução de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 4eaeccb8b4003bb835ee4700d1df57cd8cefcda0
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5947767"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>Solução de problemas de aplicativos UWP ARM32
>[!IMPORTANT]
> O SDK ARM64 agora está disponível como parte do Visual Studio 15.8 Preview 1. Recomendamos que você recompile seu aplicativo para ARM64 de forma que o aplicativo seja executado na velocidade nativa total. Para obter mais informações, veja a postagem do blog [Visualização antecipada de suporte do Visual Studio para Windows 10 no desenvolvimento do ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/).

Se seu aplicativo UWP ARM32 não estiver funcionando corretamente no ARM, veja algumas diretrizes que podem ajudar. 

## <a name="common-issues"></a>Problemas comuns
Veja alguns problemas comuns a ter em mente ao solucionar problemas de aplicativos ARM32.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Usando as APIs somente de Windows 10 Mobile em processadores baseados em ARM 
Aplicativos ARM32 poderão ter problemas ao usar APIs somente para dispositivo móvel (por exemplo, **HardwareButtons**). Para atenuar isso, você pode detectar dinamicamente se seu aplicativo está em execução no Windows 10 Mobile antes de chamar essas APIs. Siga as orientações na postagem do blog, [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluindo dependências não compatíveis com aplicativos UWP
Aplicativos da Plataforma Universal do Windows (UWP) que não são criados corretamente com o Visual Studio e o SDK da UWP podem ter dependências em componentes do SO que não estão disponíveis para aplicativos ARM32 em execução em um sistema ARM64. Entre os exemplos dessas dependências estão:

- Esperando partes do .NET Framework ficarem disponíveis.
- Referenciando componentes .NET de terceiros que não são compatíveis com UWP.

Esses problemas podem ser resolvidos: removendo as dependências indisponíveis e recriando o aplicativo usando as versões mais recentes do Microsoft Visual Studio e SDK da UWP; ou, como último recurso, removendo o aplicativo ARM32 da Microsoft Store, para que a versão x86 do aplicativo (se disponível) seja baixada para computadores dos usuários. 

Para saber mais sobre as APIs .NET disponíveis para aplicativos UWP, consulte [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilando um aplicativo com uma versão anterior do Visual Studio e do SDK
Se você tiver problemas, use as versões mais recentes do Microsoft Visual Studio e do SDK do Windows para compilar seu aplicativo. Os aplicativos compilados com uma versão anterior do Visual Studio e do SDK podem ter problemas que foram corrigidos em versões posteriores.

## <a name="debugging"></a>Depuração
Você pode usar ferramentas existentes para desenvolver aplicativos ARM32 para a plataforma ARM. Veja alguns recursos úteis.

- O Visual Studio 15.5 Preview 1 e posterior oferece suporte à execução de aplicativos ARM32 usando o modo de Autenticação Universal. Isso inicializa automaticamente as ferramentas de depuração remotas necessárias.
- Consulte [Depuração em ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para saber mais sobre ferramentas e estratégias para a depuração em ARM.