---
title: Solução de problemas de aplicativos UWP ARM32
description: Problemas comuns com aplicativos ARM32 quando executados em ARM e como corrigi-los.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, sempre conectado, aplicativos ARM32 no ARM, windows 10 no ARM, solução de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 6213c8c69695d160d4e6fa362aa7aa322a0326fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683939"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solução de problemas de aplicativos UWP do ARM

Se seu aplicativo ARM32 ou ARM64 UWP não estiver funcionando corretamente no ARM, aqui estão algumas diretrizes que podem ajudar.

>[!NOTE]
> Para criar seu aplicativo UWP para direcionar nativamente a plataforma ARM64, você deve ter o Visual Studio 2017 versão 15,9 ou posterior ou o Visual Studio 2019. Para obter mais informações, consulte [esta postagem do blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


## <a name="common-issues"></a>Problemas comuns
Aqui estão alguns problemas comuns a serem considerados ao solucionar problemas de aplicativos ARM32 e ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Usando as APIs somente de Windows 10 Mobile em processadores baseados em ARM
Os aplicativos ARM podem ter problemas ao usar APIs somente móveis (por exemplo, **HardwareButtons**). Para atenuar isso, você pode detectar dinamicamente se seu aplicativo está em execução no Windows 10 Mobile antes de chamar essas APIs. Siga as orientações na postagem do blog, [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluindo dependências não compatíveis com aplicativos UWP
Os aplicativos Plataforma Universal do Windows (UWP) que não são compilados corretamente com o Visual Studio e o SDK do UWP podem ter dependências em componentes do sistema operacional que não estão disponíveis para aplicativos ARM em execução em um sistema ARM64. Entre os exemplos dessas dependências estão:

- Esperando partes do .NET Framework ficarem disponíveis.
- Referenciando componentes .NET de terceiros que não são compatíveis com UWP.

Esses problemas podem ser resolvidos: removendo as dependências indisponíveis e recompilando o aplicativo usando as versões mais recentes do SDK Microsoft Visual Studio e UWP; ou como último recurso, remover o aplicativo ARM da Microsoft Store, para que a versão x86 do aplicativo (se disponível) seja baixada nos computadores dos usuários.

Para saber mais sobre as APIs .NET disponíveis para aplicativos UWP, consulte [.NET para aplicativos UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilando um aplicativo com uma versão anterior do Visual Studio e do SDK
Se você tiver problemas, use as versões mais recentes do Microsoft Visual Studio e do SDK do Windows para compilar seu aplicativo. Os aplicativos compilados com uma versão anterior do Visual Studio e do SDK podem ter problemas que foram corrigidos em versões posteriores.

## <a name="debugging"></a>Depuração
Você pode usar as ferramentas existentes para desenvolver aplicativos para a plataforma ARM. Veja alguns recursos úteis.

- O Visual Studio 15.5 Preview 1 e posterior oferece suporte à execução de aplicativos ARM32 usando o modo de Autenticação Universal. Isso inicializa automaticamente as ferramentas de depuração remotas necessárias.
- Consulte [Depuração em ARM64](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) para saber mais sobre ferramentas e estratégias para a depuração em ARM.
