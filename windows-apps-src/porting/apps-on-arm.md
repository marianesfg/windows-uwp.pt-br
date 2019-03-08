---
title: Windows 10 em execução no ARM
description: Este artigo fornece uma visão geral de como os aplicativos e experiências serão executado no ARM, quais são as limitações e onde você pode ir para saber mais.
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, sempre conectado, ARM, ARM64, emulação x86
ms.localizationpriority: medium
ms.openlocfilehash: 47677cb2a9e8d62c76f10f932b142c4dba9752c6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640991"
---
# <a name="windows-10-on-arm"></a>Windows 10 em execução no ARM
Originalmente, o Windows 10 (diferente do Windows 10 Mobile) pode ser executado somente em computadores que foram equipados com processadores x86 e x64. Agora, a área de trabalho do Windows 10 (edições Pro e S) pode ser executada em computadores com processadores ARM64 e a Fall Creators Update. A natureza de economia de energia da arquitetura de CPU do ARM permite que esses computadores tenham bateria o dia todo e ofereçam suporte para redes de dados móveis. Esses computadores fornecerão ótima compatibilidade de aplicativos e permitirão que você execute aplicativos win32 x86 existentes sem modificação. Ex. Adobe Reader. Para obter mais informações ou demonstração, veja o [vídeo do Channel 9 para o computador sempre conectado](https://channel9.msdn.com/Events/Build/2017/P4171).

Usamos o termo *ARM* aqui como um atalho para computadores que executam a versão da área de trabalho do Windows 10 em processadores ARM64 (também conhecida como *AArch64*).  Usamos o termo *ARM32* aqui como um atalho para a arquitetura de 32 bits do ARM (mais conhecida como *ARM* em outras documentações).

## <a name="apps-and-experiences-on-arm"></a>Aplicativos e experiências no ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiências integradas do Windows 10, aplicativos e drivers
As experiências integradas do Windows 10 como Edge, Cortana, menu Iniciar e Explorador são nativas e executadas como ARM64 (ou ARM32). Isso também inclui todos os drivers de dispositivo, como elementos gráficos, rede ou disco rígido. Isso garante que você obtenha a melhor experiência do usuário e duração da bateria do seu dispositivo em execução na velocidade nativa total do processador Qualcomm Snapdragon

### <a name="universal-windows-platform-uwp-apps"></a>Aplicativos da Plataforma Universal do Windows (UWP)
Windows 10 no ARM é executado em todos os x86, ARM64 e ARM32 [aplicativos da UWP](../get-started/universal-application-platform-guide.md) da Microsoft Store. ARM32 e ARM64 aplicativos executados nativamente sem qualquer emulação, enquanto os aplicativos executados em emulação do x86. Se você for um desenvolvedor de UWP, certifique-se de enviar um pacote ARM para seu aplicativo, pois isso fornecerá a melhor experiência de usuário para o dispositivo. Para obter mais informações, consulte [Arquiteturas de pacote do aplicativo](../packaging/device-architecture.md).

>[!NOTE]
> Para compilar seu aplicativo UWP nativamente a ARM64 para plataforma como destino, você deve ter o Visual Studio 2017 versão 15.9 ou posterior. Para obter mais informações, consulte [esta postagem de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

>[!IMPORTANT]
> Quando um usuário baixar um aplicativo UWP na Microsoft Store, a versão ARM32 será instalada em um dispositivo ARM64, a menos que somente a versão x86 esteja disponível. Para obter mais informações sobre arquiteturas, consulte [Arquiteturas de pacote do aplicativo](../packaging/device-architecture.md).

### <a name="win32-apps"></a>Aplicativos Win32
Além de aplicativos UWP, Windows 10 no ARM também pode executar seu x86 Win32 aplicativos (como o Adobe Reader) sem modificações, com bom desempenho e experiência perfeita ao usuário, assim como qualquer PC. Esses x86 aplicativos Win32 não tem recompilado para ARM e até mesmo não percebem que eles estão em execução no processador ARM. Observe que aplicativos Win32 x64 de 64 bits não têm suporte, mas a maioria dos aplicativos têm versões x86 dos aplicativos. Portanto, da perspectiva do usuário, basta escolher o instalador x86 de 32 bits para executar no computador Windows no ARM.

## <a name="in-this-section"></a>Nesta seção
|Tópico | Descrição |
|-----|-----|
|[Como x86 emulação funciona no ARM](apps-on-arm-x86-emulation.md)|Uma visão geral detalhando como aplicativos x86 são emulados em ARM.|
|[Solução de problemas de x86 aplicativos no ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comuns com aplicativos x86 quando executados em ARM e como corrigi-los. |
|[Solucionar problemas de aplicativos do ARM no ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comuns com aplicativos ARM32 e ARM64 quando em execução no ARM e como corrigi-los. |
|[Solução de problemas de compatibilidade de programa no ARM](apps-on-arm-program-compat-troubleshooter.md)|Diretrizes para ajustar as configurações de compatibilidade se seu aplicativo não estiver funcionando corretamente no ARM. |

## <a name="related-topics"></a>Tópicos relacionados
|Tópico | Descrição |
|-----|-----|
|[Criação de Drivers ARM64 com o WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Instruções para a criação de um driver ARM64. |
| [Depuração x86 aplicativos no ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Diretrizes para depuração de aplicativos x86 em ARM. |
