---
title: Windows 10 no ARM
description: Este artigo fornece uma visão geral de como os aplicativos e experiências serão executado no ARM, quais são as limitações e onde você pode ir para saber mais.
ms.date: 05/22/2020
ms.topic: article
keywords: Windows 10 s, sempre conectado, ARM, ARM64, emulação x86
ms.localizationpriority: medium
ms.openlocfilehash: 006de4f1f04ffb98d46b6ceb981d3d0fba311026
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854742"
---
# <a name="windows-10-on-arm"></a>Windows 10 no ARM
Originalmente, o Windows 10 (diferente do Windows 10 Mobile) pode ser executado somente em computadores que foram equipados com processadores x86 e x64. Agora, o Windows 10 desktop pode ser executado em computadores que são alimentados por processadores ARM64 com a atualização de criadores de outono ou mais recente. A natureza de economia de energia da arquitetura de CPU do ARM permite que esses computadores tenham bateria o dia todo e ofereçam suporte para redes de dados móveis. Esses computadores fornecerão ótima compatibilidade de aplicativos e permitirão que você execute aplicativos win32 x86 existentes sem modificação. Para obter mais informações ou uma demonstração, consulte o [vídeo do canal 9 para o PC sempre conectado](https://channel9.msdn.com/Events/Build/2017/P4171).

Usamos o termo *ARM* aqui como um atalho para computadores que executam a versão da área de trabalho do Windows 10 em processadores ARM64 (também conhecida como *AArch64*).  Usamos o termo *ARM32* aqui como um atalho para a arquitetura de 32 bits do ARM (mais conhecida como *ARM* em outras documentações).

## <a name="apps-and-experiences-on-arm"></a>Aplicativos e experiências no ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiências integradas do Windows 10, aplicativos e drivers
As experiências internas do Windows 10, como Edge, Cortana, start menu e Explorer, são todas nativas e executadas como ARM64. Isso também inclui todos os drivers de dispositivo, como gráficos, rede ou disco rígido. Isso garante que você obtenha a melhor experiência do usuário e a vida útil da bateria do dispositivo em execução na velocidade nativa completa do processador Qualcomm Snapdragon.

### <a name="universal-windows-platform-uwp-apps"></a>Aplicativos da Plataforma Universal do Windows (UWP)
O Windows 10 no ARM executa todos os [aplicativos UWP](../get-started/universal-application-platform-guide.md) x86, ARM32 e ARM64 da Microsoft Store. Os aplicativos ARM32 e ARM64 são executados nativamente sem qualquer emulação, enquanto aplicativos x86 são executados sob emulação. Se você for um desenvolvedor de UWP, certifique-se de enviar um pacote ARM para seu aplicativo, pois isso fornecerá a melhor experiência de usuário para o dispositivo. Para obter mais informações, consulte [Arquiteturas de pacote do aplicativo](/windows/msix/package/device-architecture).

>[!NOTE]
> Para criar seu aplicativo UWP para direcionar nativamente a plataforma ARM64, você deve ter o Visual Studio 2017 versão 15,9 ou posterior ou o Visual Studio 2019. Para obter mais informações, consulte [esta postagem do blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> O Windows 10 no ARM dá suporte a aplicativos UWP x86, ARM32 e ARM64 da loja em dispositivos ARM64. Quando um usuário baixa seu aplicativo UWP em um dispositivo ARM64, o sistema operacional instalará automaticamente a versão ideal do seu aplicativo que está disponível. Se você enviar versões x86, ARM32 e ARM64 do seu aplicativo para a loja, o sistema operacional instalará automaticamente a versão ARM64 do seu aplicativo. Se você enviar apenas as versões x86 e ARM32 do seu aplicativo, o sistema operacional instalará a versão ARM32. Se você enviar apenas a versão x86 do seu aplicativo, o sistema operacional instalará essa versão e a executará em emulação. Para obter mais informações sobre arquiteturas, consulte [Arquiteturas de pacote do aplicativo](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Aplicativos Win32
Além dos aplicativos UWP, o Windows 10 no ARM também pode executar seus aplicativos Win32 x86 sem modificações, com bom desempenho e uma experiência de usuário tranqüila, assim como qualquer PC. Esses aplicativos Win32 x86 não precisam ser recompilados para o ARM e nem mesmo percebem que estão em execução em um processador ARM. Observe que os aplicativos do Win32 de 64 bits x64 não têm suporte, mas a grande maioria dos aplicativos tem versões x86 disponíveis.  Quando for dada a opção de arquitetura do aplicativo, basta escolher a versão x86 de 32 bits para executar o aplicativo em um PC com Windows 10 no ARM.

## <a name="downloads"></a>Downloads

O Visual Studio 2019 fornece vários downloads de ferramentas para Windows 10 no ARM. Os usuários Stil usando o Visual Studio 2017 podem usar o instalador para localizar e instalar ferramentas e pacotes comparáveis. Observe que, para seguir essas etapas, você deve estar usando o Visual Studio 2019.

### <a name="visual-c-redistributable"></a>Pacotes Redistribuíveis do Visual C++

O pacote Redist Visual C++ está disponível para aplicativos ARM. Visite a [página do Visual Studio Downlaods](https://visualstudio.microsoft.com/downloads/) role para baixo até **todos os downloads**, abra **outras ferramentas e estruturas**e, em seguida, navegue até a entrada **Microsoft Visual C++ redistribuível para Visual Studio 2019** . Selecione o botão de opção **ARM64** e **Baixe**.

### <a name="remote-tools"></a>Ferramentas remotas

Ferramentas Remotas para Visual Studio estão disponíveis para aplicativos ARM. Visite a [página do Visual Studio Downlaods](https://visualstudio.microsoft.com/downloads/) role para baixo até **todos os downloads**, abra **Tools for Visual Studio 2019**e, em seguida, navegue até a entrada **ferramentas remotas para Visual Studio 2019** . Selecione o botão de opção **ARM64* e, em seguida, **Baixe**.


## <a name="in-this-section"></a>Nesta seção
|Tópico | Descrição |
|-----|-----|
|[Como a emulação x86 funciona no ARM](apps-on-arm-x86-emulation.md)|Uma visão geral detalhando como aplicativos x86 são emulados em ARM.|
|[Solução de problemas de aplicativos x86 no ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comuns com aplicativos x86 quando executados em ARM e como corrigi-los. |
|[Solução de problemas de aplicativos ARM no ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comuns com aplicativos ARM32 e ARM64 ao serem executados no ARM e como corrigi-los. |
|[Solução de problemas de compatibilidade de programas no ARM](apps-on-arm-program-compat-troubleshooter.md)|Diretrizes para ajustar as configurações de compatibilidade se seu aplicativo não estiver funcionando corretamente no ARM. |

## <a name="related-topics"></a>Tópicos relacionados
|Tópico | Descrição |
|-----|-----|
|[Criando drivers ARM64 com o WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)|Instruções para a criação de um driver ARM64. |
| [Depurando aplicativos x86 no ARM](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) | Diretrizes para depuração de aplicativos x86 em ARM. |
