---
title: Windows 10 em execução no ARM
author: msatranjr
description: Este artigo fornece uma visão geral de como os aplicativos e experiências serão executado no ARM, quais são as limitações e onde você pode ir para saber mais.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, sempre conectado, ARM, ARM64, emulação x86
ms.localizationpriority: medium
ms.openlocfilehash: 8f62a873e84f200a019bde23038ae10b21150072
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843883"
---
# <a name="windows-10-on-arm"></a>Windows 10 em execução no ARM
Originalmente, o Windows 10 (diferente do Windows 10 Mobile) pode ser executado somente em computadores que foram equipados com processadores x86 e x64. Agora, a área de trabalho do Windows 10 (edições Pro e S) pode ser executada em computadores com processadores ARM64 e a Fall Creators Update. A natureza de economia de energia da arquitetura de CPU do ARM permite que esses computadores tenham bateria o dia todo e ofereçam suporte para redes de dados móveis. Esses computadores fornecerão ótima compatibilidade de aplicativos e permitirão que você execute aplicativos win32 x86 existentes sem modificação. Ex. Adobe Reader. Para obter mais informações ou demonstração, veja o [vídeo do Channel 9 para o computador sempre conectado](https://channel9.msdn.com/Events/Build/2017/P4171). 

Usamos o termo *ARM* aqui como um atalho para computadores que executam a versão da área de trabalho do Windows 10 em processadores ARM64 (também conhecida como *AArch64*).  Usamos o termo *ARM32* aqui como um atalho para a arquitetura de 32 bits do ARM (mais conhecida como *ARM* em outras documentações).

## <a name="apps-and-experiences-on-arm"></a>Aplicativos e experiências no ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiências integradas do Windows 10, aplicativos e drivers
As experiências integradas do Windows 10 como Edge, Cortana, menu Iniciar e Explorador são nativas e executadas como ARM64 (ou ARM32). Isso também inclui todos os drivers de dispositivo, como elementos gráficos, rede ou disco rígido. Isso garante que você obtenha a melhor experiência do usuário e duração da bateria do seu dispositivo em execução na velocidade nativa total do processador Qualcomm Snapdragon

### <a name="universal-windows-platform-uwp-apps"></a>Aplicativos da Plataforma Universal do Windows (UWP)
O Windows 10 no ARM executa todos os [aplicativos UWP](../get-started/universal-application-platform-guide.md) x86 e ARM32 da Microsoft Store. Os aplicativos ARM32 são executados nativamente sem emulação, enquanto os aplicativos x86 são executados com emulação. Se você for um desenvolvedor de UWP, certifique-se de enviar um pacote ARM para seu aplicativo, pois isso fornecerá a melhor experiência de usuário para o dispositivo. Para obter mais informações, consulte [Arquiteturas de pacote do aplicativo](../packaging/device-architecture.md).

>[!IMPORTANT] 
> Quando um usuário baixar um aplicativo UWP na Microsoft Store, a versão ARM32 será instalada em um dispositivo ARM64, a menos que somente a versão x86 esteja disponível. Para obter mais informações sobre arquiteturas, consulte [Arquiteturas de pacote do aplicativo](../packaging/device-architecture.md).

### <a name="win32-apps"></a>Aplicativos Win32
Além de aplicativos UWP, o Windows 10 no ARM também pode executar seus aplicativos Win32 x86 (por exemplo, Adobe Reader) sem modificação, com bom desempenho e experiência do usuário perfeita, assim como qualquer computador. Esses aplicativos win32 x86 não precisam ser recompilados para ARM e nem percebem que estão sendo executados em processadores ARM. Observe que aplicativos Win32 x64 de 64 bits não têm suporte, mas a maioria dos aplicativos têm versões x86 dos aplicativos. Portanto, da perspectiva do usuário, basta escolher o instalador x86 de 32 bits para executar no computador Windows no ARM.

## <a name="in-this-section"></a>Nesta seção
|Tópico | Descrição |
|-----|-----|
|[Como a emulação x86 funciona no ARM](apps-on-arm-x86-emulation.md)|Uma visão geral detalhando como aplicativos x86 são emulados em ARM.|
|[Solução de problemas de aplicativos x86 em ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comuns com aplicativos x86 quando executados em ARM e como corrigi-los. |
|[Solução de problemas de aplicativos ARM32 em ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comuns com aplicativos ARM32 quando executados em ARM e como corrigi-los. |
|[Solução de problemas de compatibilidade de programas no ARM](apps-on-arm-program-compat-troubleshooter.md)|Diretrizes para ajustar as configurações de compatibilidade se seu aplicativo não estiver funcionando corretamente no ARM. |

## <a name="related-topics"></a>Tópicos relacionados
|Tópico | Descrição |
|-----|-----|
|[Criando drivers ARM64 com o WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Instruções para a criação de um driver ARM64. |
| [Depurando aplicativos x86 no ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Diretrizes para depuração de aplicativos x86 em ARM. |
