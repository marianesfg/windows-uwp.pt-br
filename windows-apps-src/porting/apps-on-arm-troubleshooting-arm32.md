---
title: Solução de problemas de aplicativos UWP ARM32
description: Problemas comuns com aplicativos ARM32 quando executados em ARM e como corrigi-los.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, sempre conectado, aplicativos ARM32 no ARM, windows 10 no ARM, solução de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 3431b12fc6f6b6ba2d870400ec4f6684f8290a61
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63815283"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solução de problemas de ARM aplicativos UWP

Se seu aplicativo ARM32 ou ARM64 UWP não está funcionando corretamente no ARM, aqui estão algumas diretrizes que podem ajudar.

>[!NOTE]
> Para compilar seu aplicativo UWP nativamente a ARM64 para plataforma como destino, você deve ter o Visual Studio 2017 versão 15.9 ou posterior. Para obter mais informações, consulte [esta postagem de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

## <a name="common-issues"></a>Problemas comuns
Aqui estão alguns problemas comuns para ter em mente ao solucionar problemas de aplicativos ARM32 e ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Usando as APIs somente de Windows 10 Mobile em processadores baseados em ARM
ARM aplicativos poderão ter problemas ao usar APIs somente móvel (por exemplo, **HardwareButtons**). Para atenuar isso, você pode detectar dinamicamente se seu aplicativo está em execução no Windows 10 Mobile antes de chamar essas APIs. Siga as orientações na postagem do blog, [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluindo dependências não compatíveis com aplicativos UWP
Aplicativos universais do Windows Platform (UWP) que não são compilados corretamente com o Visual Studio e o SDK da UWP podem ter dependências em componentes do sistema operacional que não estão disponíveis para aplicativos do ARM executando em um sistema ARM64. Entre os exemplos dessas dependências estão:

- Esperando partes do .NET Framework ficarem disponíveis.
- Referenciando componentes .NET de terceiros que não são compatíveis com UWP.

Esses problemas podem ser resolvidos por: Removendo as dependências não está disponíveis e recompilar o aplicativo usando as versões mais recentes do Microsoft Visual Studio e SDK de UWP; ou como um último recurso, remover o aplicativo do ARM da Microsoft Store, para que a versão do aplicativo (se disponível) é baixado para os computadores dos usuários de x86.

Para saber mais sobre as APIs .NET disponíveis para aplicativos UWP, consulte [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilando um aplicativo com uma versão anterior do Visual Studio e do SDK
Se você tiver problemas, use as versões mais recentes do Microsoft Visual Studio e do SDK do Windows para compilar seu aplicativo. Os aplicativos compilados com uma versão anterior do Visual Studio e do SDK podem ter problemas que foram corrigidos em versões posteriores.

## <a name="debugging"></a>Depuração
Você pode usar as ferramentas existentes para o desenvolvimento de aplicativos para a plataforma ARM. Veja alguns recursos úteis.

- O Visual Studio 15.5 Preview 1 e posterior oferece suporte à execução de aplicativos ARM32 usando o modo de Autenticação Universal. Isso inicializa automaticamente as ferramentas de depuração remotas necessárias.
- Consulte [Depuração em ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para saber mais sobre ferramentas e estratégias para a depuração em ARM.
