---
title: Limitações de aplicativos e experiências em ARM
author: msatranjr
description: Etapas de solução de problemas para aplicativos que não estão funcionando corretamente no ARM.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, sempre conectado, limitações, windows 10 no ARM
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/en-us/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: 24afc8a876b976f21d0f4ebd5892ceef7c403018
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691781"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>Limitações de aplicativos e experiências em ARM
O Windows 10 no ARM tem as seguintes limitações necessárias:

- **Somente os drivers ARM64 têm suporte**. Assim como acontece com todas as arquiteturas, drivers de modo kernel, drivers [Estrutura de Driver em Modo de Usuário (UMDF)](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/overview-of-the-umdf) e drivers de impressão devem ser compilados para corresponder à arquitetura do SO. Enquanto o SO ARM tem os recursos para emular aplicativos x86 em modo de usuário, drivers implementados para outras arquiteturas (por exemplo, x64 ou x86) atualmente não são emulados e, portanto, não têm suporte nesta plataforma. Qualquer aplicativo que funcione com seu próprio driver personalizado precisa ser portado para ARM64. Em cenários limitados, o aplicativo pode ser executado como x86 em emulação, mas a parte de driver do aplicativo deve ser portada para ARM64. Para obter mais informações sobre a compilação do seu driver para ARM64, consulte [Criando drivers ARM64 com o WDK](https://review.docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers?branch=rs4-arm64).

- **Não há suporte para aplicativos x64**. O Windows 10 no ARM não oferece suporte a emulação de aplicativos x64.

- **Alguns jogos não funcionam**. Jogos e aplicativos que usam uma versão do OpenGL posterior à 1.1 ou que exigem OpenGL acelerado por hardware não funcionam. Além disso, os jogos que dependem de drivers "antitrapaça" não são permitidos nesta plataforma.

- **Aplicativos que personalizam a experiência do Windows podem não funcionar corretamente**. Componentes do SO nativos não podem carregar componentes não nativos. Exemplos de aplicativos que costumam fazer isso incluem alguns editores de método de entrada (IMEs), tecnologias assistenciais e aplicativos de armazenamento em nuvem. Os IMEs e as tecnologias assistenciais geralmente se vinculam à pilha de entrada para a maior parte da funcionalidade do aplicativo. Aplicativos de armazenamento em nuvem normalmente usam extensões de shell (por exemplo, ícones no Explorador e adições de menus de atalho); as extensões do shell poderão falhar e, se a falha não for manipulada normalmente, o próprio aplicativo poderá não funcionar.

- **Aplicativos que supõem que todos os dispositivos baseados em ARM estão executando uma versão móvel do Windows podem não funcionar corretamente**. Aplicativos que fazem essa suposição podem aparecer na orientação incorreta, apresentam layout de interface do usuário ou renderização inesperada ou não iniciam completamente quando tentam invocar APIs somente para dispositivo móvel sem primeiro testar a disponibilidade de contrato.

- **Não há suporte para a plataforma de hipervisor do Windows em ARM**. Executar máquinas virtuais usando Hyper-V em um dispositivo ARM não funcionará.

A tabela a seguir lista alguns problemas comuns e oferece sugestões sobre como resolvê-los.

|Problema|Solução|
|-----|--------|
| Seu aplicativo depende de um driver que não foi projetado para ARM. | Recompile seu driver x86 para ARM64. Consulte [Criando drivers ARM64 com o WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| Seu aplicativo está disponível somente para x64. | Se você desenvolver para a Microsoft Store, envie uma versão para ARM do seu aplicativo. Para obter mais informações, consulte [Arquiteturas do pacote do aplicativo](../packaging/device-architecture.md). Se você for desenvolvedor de Win32, distribua uma versão x86 do seu aplicativo. |
| Seu aplicativo usa uma versão de OpenGL posterior à 1.1 ou requer OpenGL acelerado por hardware. | Aplicativos x86 que usam DirectX 9, DirectX 10, DirectX 11 e DirectX 12 funcionarão em ARM. Para obter mais informações, consulte [Elementos gráficos e jogos do DirectX](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx). |
| Seu aplicativo x86 não funciona como esperado. | Tente usar a solução de problemas de compatibilidade seguindo a orientação em [Solução de problemas de compatibilidade de programas no ARM](apps-on-arm-program-compat-troubleshooter.md). Para algumas outras etapas de solução de problemas, consulte o artigo [Solução de problemas de aplicativos x86 no ARM](apps-on-arm-troubleshooting-x86.md). |
| Seu aplicativo x86 não detecta se está em execução em ARM. | Use [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) para determinar se seu aplicativo está em execução em ARM. |
| Seu aplicativo UWP ARM32 não funciona como esperado. | Consulte [Solução de problemas de aplicativos ARM32 em ARM](apps-on-arm-troubleshooting-arm32.md) para saber como colocar seu aplicativo para funcionar corretamente em ARM. |
