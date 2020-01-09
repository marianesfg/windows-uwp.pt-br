---
title: Limitações de aplicativos e experiências em ARM
description: Etapas de solução de problemas para aplicativos que não estão funcionando corretamente no ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, sempre conectado, limitações, windows 10 no ARM
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: e9bbef6f9b714b99148cf4ac082f98b4422f23b2
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683959"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>Limitações de aplicativos e experiências em ARM
O Windows 10 no ARM tem as seguintes limitações necessárias:

- **Somente os drivers ARM64 têm suporte**. Assim como acontece com todas as arquiteturas, drivers de modo kernel, drivers [Estrutura de Driver em Modo de Usuário (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) e drivers de impressão devem ser compilados para corresponder à arquitetura do SO. Enquanto o SO ARM tem os recursos para emular aplicativos x86 em modo de usuário, drivers implementados para outras arquiteturas (por exemplo, x64 ou x86) atualmente não são emulados e, portanto, não têm suporte nesta plataforma. Qualquer aplicativo que funcione com seu próprio driver personalizado precisa ser portado para ARM64. Em cenários limitados, o aplicativo pode ser executado como x86 em emulação, mas a parte de driver do aplicativo deve ser portada para ARM64. Para obter mais informações sobre a compilação do seu driver para ARM64, consulte [Criando drivers ARM64 com o WDK](/windows-hardware/drivers/develop/building-arm64-drivers).

- **Não há suporte para aplicativos x64**. O Windows 10 no ARM não oferece suporte a emulação de aplicativos x64.

- **Alguns jogos não funcionam**. Jogos e aplicativos que usam uma versão do OpenGL posterior à 1.1 ou que exigem OpenGL acelerado por hardware não funcionam. Além disso, os jogos que dependem de drivers "antitrapaça" não são permitidos nesta plataforma.

- **Aplicativos que personalizam a experiência do Windows podem não funcionar corretamente**. Componentes do SO nativos não podem carregar componentes não nativos. Exemplos de aplicativos que costumam fazer isso incluem alguns editores de método de entrada (IMEs), tecnologias assistenciais e aplicativos de armazenamento em nuvem. Os IMEs e as tecnologias assistenciais geralmente se vinculam à pilha de entrada para a maior parte da funcionalidade do aplicativo. Aplicativos de armazenamento em nuvem normalmente usam extensões de shell (por exemplo, ícones no Explorador e adições de menus de atalho); as extensões do shell poderão falhar e, se a falha não for manipulada normalmente, o próprio aplicativo poderá não funcionar.

- **Aplicativos que supõem que todos os dispositivos baseados em ARM estão executando uma versão móvel do Windows podem não funcionar corretamente**. Aplicativos que fazem essa suposição podem aparecer na orientação incorreta, apresentam layout de interface do usuário ou renderização inesperada ou não iniciam completamente quando tentam invocar APIs somente para dispositivo móvel sem primeiro testar a disponibilidade de contrato.

- **Não há suporte para a plataforma de hipervisor do Windows em ARM**. Executar máquinas virtuais usando Hyper-V em um dispositivo ARM não funcionará.

A tabela a seguir lista alguns problemas comuns e oferece sugestões sobre como resolvê-los.

|Problema|Solução|
|-----|--------|
| Seu aplicativo depende de um driver que não foi projetado para ARM. | Recompile seu driver x86 para ARM64. Consulte [Criando drivers ARM64 com o WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers). |
| Seu aplicativo está disponível somente para x64. | Se você desenvolver para a Microsoft Store, envie uma versão para ARM do seu aplicativo. Para obter mais informações, consulte [Arquiteturas do pacote do aplicativo](/windows/msix/package/device-architecture). Se você for desenvolvedor de Win32, distribua uma versão x86 do seu aplicativo. |
| Seu aplicativo usa uma versão de OpenGL posterior à 1.1 ou requer OpenGL acelerado por hardware. | Aplicativos x86 que usam DirectX 9, DirectX 10, DirectX 11 e DirectX 12 funcionarão em ARM. Para obter mais informações, consulte [Elementos gráficos e jogos do DirectX](https://docs.microsoft.com/windows/desktop/directx). |
| Seu aplicativo x86 não funciona como esperado. | Tente usar a solução de problemas de compatibilidade seguindo a orientação em [Solução de problemas de compatibilidade de programas no ARM](apps-on-arm-program-compat-troubleshooter.md). Para algumas outras etapas de solução de problemas, consulte o artigo [Solução de problemas de aplicativos x86 no ARM](apps-on-arm-troubleshooting-x86.md). |
| Seu aplicativo x86 não detecta se está em execução em ARM. | Use [IsWow64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) para determinar se seu aplicativo está em execução em ARM. |
| Seu aplicativo UWP ARM32 não funciona como esperado. | Consulte [Solução de problemas de aplicativos ARM32 em ARM](apps-on-arm-troubleshooting-arm32.md) para saber como colocar seu aplicativo para funcionar corretamente em ARM. |
