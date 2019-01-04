---
title: Solução de problemas de aplicativos UWP ARM32
description: Problemas comuns com aplicativos ARM32 quando executados em ARM e como corrigi-los.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, sempre conectado, aplicativos ARM32 no ARM, windows 10 no ARM, solução de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 75019df4b7d70dad20aea1a256abac05c93a481d
ms.sourcegitcommit: 62bc4936ca8ddf1fea03d43a4ede5d14a5755165
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991632"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solução de problemas do ARM aplicativos UWP

Se seu aplicativo UWP de ARM64 ou ARM32 não estiver funcionando corretamente no ARM, veja algumas diretrizes que podem ajudar.

>[!NOTE]
> Para criar seu aplicativo UWP de destino nativamente a plataforma ARM64, você deve ter o Visual Studio 2017 versão 15.9 ou posterior. Para obter mais informações, consulte [esta postagem no blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

## <a name="common-issues"></a>Problemas comuns
Aqui estão alguns problemas comuns a ter em mente ao solucionar problemas de aplicativos ARM32 e ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Usando as APIs somente de Windows 10 Mobile em processadores baseados em ARM
Aplicativos ARM poderão ter problemas ao usar APIs somente mobile (por exemplo, **HardwareButtons**). Para atenuar isso, você pode detectar dinamicamente se seu aplicativo está em execução no Windows 10 Mobile antes de chamar essas APIs. Siga as orientações na postagem do blog, [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluindo dependências não compatíveis com aplicativos UWP
Aplicativos universais do Windows Platform (UWP) que não são criados corretamente com o Visual Studio e SDK da UWP podem ter dependências em componentes do sistema operacional que não estão disponíveis para aplicativos ARM em execução em um sistema ARM64. Entre os exemplos dessas dependências estão:

- Esperando partes do .NET Framework ficarem disponíveis.
- Referenciando componentes .NET de terceiros que não são compatíveis com UWP.

Esses problemas podem ser resolvidos: Removendo as dependências indisponíveis e recriando o aplicativo usando as versões mais recentes do Microsoft Visual Studio e SDK da UWP; ou, como último recurso, removendo o aplicativo ARM da Microsoft Store, para que a versão x86 do aplicativo (se disponível) seja baixada para computadores dos usuários.

Para saber mais sobre as APIs .NET disponíveis para aplicativos UWP, consulte [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilando um aplicativo com uma versão anterior do Visual Studio e do SDK
Se você tiver problemas, use as versões mais recentes do Microsoft Visual Studio e do SDK do Windows para compilar seu aplicativo. Os aplicativos compilados com uma versão anterior do Visual Studio e do SDK podem ter problemas que foram corrigidos em versões posteriores.

## <a name="debugging"></a>Depuração
Você pode usar as ferramentas existentes para desenvolver aplicativos para a plataforma ARM. Veja alguns recursos úteis.

- O Visual Studio 15.5 Preview 1 e posterior oferece suporte à execução de aplicativos ARM32 usando o modo de Autenticação Universal. Isso inicializa automaticamente as ferramentas de depuração remotas necessárias.
- Consulte [Depuração em ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para saber mais sobre ferramentas e estratégias para a depuração em ARM.
