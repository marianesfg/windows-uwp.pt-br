---
title: Solução de problemas de aplicativos de área de trabalho x86
description: Problemas comuns com aplicativos x86 quando executados em ARM e como corrigi-los.
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, sempre conectado, emulação x86 no ARM, solução de problemas
ms.localizationpriority: medium
ms.openlocfilehash: a71f33438a336aba67afbb30b19987b0e0aef83b
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683929"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Solução de problemas de aplicativos de área de trabalho x86
>[!IMPORTANT]
> O SDK ARM64 agora está disponível como parte do Visual Studio 15.8 Preview 1. Recomendamos que você recompile seu aplicativo para ARM64 de forma que o aplicativo seja executado na velocidade nativa total. Para obter mais informações, veja a postagem do blog [Visualização antecipada de suporte do Visual Studio para Windows 10 no desenvolvimento do ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/).

Se um aplicativo de área de trabalho x86 não funcionar como faz em um computador x86, veja algumas diretrizes para ajudá-lo a solucionar problemas.

|Problema|Solução|
|-----|--------|
| Seu aplicativo depende de um driver que não foi projetado para ARM. | Recompile seu driver x86 para ARM64. Consulte [Criando drivers ARM64 com o WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers). |
| Seu aplicativo está disponível somente para x64. | Se você desenvolver para a Microsoft Store, envie uma versão para ARM do seu aplicativo. Para obter mais informações, consulte [Arquiteturas do pacote do aplicativo](/windows/msix/package/device-architecture). Se você for um desenvolvedor do Win32, recomendamos que você recompile seu aplicativo para ARM64. Para obter mais informações, veja [Visualização antecipada de suporte do Visual Studio para Windows 10 no desenvolvimento do ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). |
| Seu aplicativo usa uma versão de OpenGL posterior à 1.1 ou requer OpenGL acelerado por hardware. | Use o modo DirectX do aplicativo, se estiver disponível. Aplicativos x86 que usam DirectX 9, DirectX 10, DirectX 11 e DirectX 12 funcionarão em ARM. Para obter mais informações, consulte [Elementos gráficos e jogos do DirectX](https://docs.microsoft.com/windows/desktop/directx). |
| Seu aplicativo x86 não funciona como esperado. | Tente usar a solução de problemas de compatibilidade seguindo a orientação em [Solução de problemas de compatibilidade de programas no ARM](apps-on-arm-program-compat-troubleshooter.md). Para algumas outras etapas de solução de problemas, consulte o artigo [Solução de problemas de aplicativos x86 no ARM](apps-on-arm-troubleshooting-x86.md). |

## <a name="best-practices-for-wow"></a>Práticas recomendadas para WOW
Um problema comum ocorre quando um aplicativo descobre que está em execução em WOW e presume que está em um sistema x64. Tendo feito essa suposição, o aplicativo pode fazer o seguinte:

- Tentar instalar sua versão x64, que não é compatível com ARM.
- Procurar outro software na exibição do registro nativo.
- Supor que um .NET Framework de 64 bits está disponível.

Em geral, um aplicativo não deve fazer suposições sobre o sistema host quando é constatado que está sendo executado em WOW. Evite interagir com componentes nativos do SO o máximo possível.

Um aplicativo pode colocar chaves de registro na exibição do registro nativo ou executar as funções com base na presença de WOW. O **IsWow64Process** original indica apenas se o aplicativo está em execução em um computador x64. Agora os aplicativos devem usar [IsWow64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) para determinar se estão em execução em um sistema com suporte para WOW. 

## <a name="drivers"></a>Drivers 
Todos os drivers de modo kernel, drivers [Estrutura de Driver em Modo de Usuário (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) e drivers de impressão devem ser compilados para corresponder à arquitetura do SO. Se um aplicativo x86 tiver um driver, esse driver deverá ser recompilado para ARM64. O aplicativo x86 pode ser executado sem problemas em emulação. No entanto, seu driver precisa ser recompilado para ARM64 e nenhuma experiência de aplicativo que dependa do driver estará disponível. Para obter mais informações sobre a compilação do seu driver para ARM64, consulte [Criando drivers ARM64 com o WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>Extensões de shell 
Aplicativos que tentam interceptar componentes do Windows ou carregar suas DLLs em processos do Windows precisarão recompilar essas DLLs para corresponder à arquitetura do sistema. ou seja, ARM64. Normalmente, eles são usados por IMEs (Input Method Editors), tecnologias assistenciais e aplicativos de extensão de Shell (por exemplo, para mostrar ícones de armazenamento em nuvem no Explorer ou um menu de contexto de clique com o botão direito do mouse). Para saber como recompilar seus aplicativos ou DLLs para ARM64, veja a postagem de blog [Visualização antecipada de suporte do Visual Studio para Windows 10 no desenvolvimento do ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). 

## <a name="debugging"></a>Depuração
Para investigar o comportamento do aplicativo em mais detalhes, consulte [Depuração no ARM](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) para saber mais sobre ferramentas e estratégias para a depuração em ARM.

## <a name="virtual-machines"></a>Máquinas virtuais
A plataforma de hipervisor do Windows não é compatível com a plataforma de PC Qualcomm Snapdragon 835 Mobile. Assim, executar máquinas virtuais usando Hyper-V não funcionará. Continuamos a fazer investimentos nessas tecnologias em futuros chipsets Qualcomm. 

## <a name="dynamic-code-generation"></a>Geração de código dinâmico
Os aplicativos de área de trabalho x86 são emulados em ARM64 pelo sistema que gera instruções ARM64 em tempo de execução. Isso significa que se um aplicativo de desktop x86 impedir a geração ou modificação de código dinâmico em seu processo, esse aplicativo não terá suporte para ser executado como x86 no ARM64. 

Essa é uma mitigação de segurança que alguns aplicativos habilitam em seu processo usando a API [SetProcessMitigationPolicy](https://docs.microsoft.com/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) com o sinalizador `ProcessDynamicCodePolicy`. Para ser executado com êxito no ARM64 como um processo x86, essa política de mitigação terá que ser desabilitada. 
