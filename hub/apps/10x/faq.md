---
Description: Conheça as respostas a algumas perguntas básicas sobre desenvolvedores sobre o Windows 10X.
title: FAQ do desenvolvedor do Windows 10X
ms.topic: article
ms.date: 04/1/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: d168faf16bdd8a88e0cd4c52c680781061bfcf0d
ms.sourcegitcommit: dadbc20564e4b0c0ecbab5fd5649dbf8ab08095a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80581950"
---
# <a name="windows-10x-developer-faq"></a>FAQ do desenvolvedor do Windows 10X

O Windows 10X é uma linha de produtos na família Windows otimizada para uso em dispositivos de tela dupla. Como desenvolvedor, você pode alcançar um público mais amplo otimizando seu aplicativo para Windows 10X, aproveitando os novos recursos específicos para um público móvel e de tela dupla e, ao mesmo tempo, aproveitando a mesma amplitude da funcionalidade do Windows 10 e do suporte avançado a desktops. [Anunciamos o Windows 10x no final de 2019](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97), e estamos ansiosos para liberá-lo no final da 2020.

![Dispositivos que executam o Windows 10x](images/windows-10x-devices.png)
 
*[Produto de pré-lançamento mostrado, telas simuladas e sujeitas a alterações]*

Para obter mais informações sobre a criação de experiências de tela dupla e do Windows 10X, Confira as sessões virtuais do [dia do Microsoft 365 dev](https://developer.microsoft.com/microsoft-365/virtual-events)ou os [documentos de desenvolvedor de tela dupla](https://docs.microsoft.com/dual-screen/). Para obter informações rápidas, aqui estão as respostas para algumas perguntas que você pode ter.

### <a name="how-is-this-different-from-developing-for-windows-10"></a>Como isso é diferente do desenvolvimento para o Windows 10?

Para a maioria dos aplicativos, isso não é diferente. A gravação de aplicativos do Windows 10X é suportada por meio do SDK do Windows 10. Como uma expressão do Windows 10, o Windows 10X dá suporte a APIs do Windows Runtime (WinRT) e executa aplicativos Win32 por meio de um contêiner nativo. Em seguida, você pode chamar novas APIs projetadas especificamente para dispositivos de tela dupla de seus aplicativos UWP ou Win32 novos ou existentes, permitindo que eles acessem os recursos e os benefícios dessa nova plataforma.

### <a name="does-this-replace-desktop-windows-10"></a>Isso substitui o Windows 10 na área de trabalho?

Não. O Windows 10X será lançado em paralelo às versões de desktop do Windows 10. As versões de desktop do Windows 10 continuarão a fornecer melhorias e melhorias à história moderna do aplicativo de desktop. O Windows 10X é outra plataforma otimizada para dar suporte a plataformas de tela dupla.

### <a name="when-will-windows-10x-be-released"></a>Quando o Windows 10X será lançado?

O Windows 10X será lançado para acompanhar a superfície neo e outros dispositivos de tela dupla de terceiros no final de 2020.

### <a name="when-can-i-start-development-for-windows-10x"></a>Quando posso iniciar o desenvolvimento para o Windows 10X?

Você pode baixar o [Microsoft Emulator e a imagem do emulador do Windows 10x](https://docs.microsoft.com/dual-screen/windows/get-dev-tools) hoje mesmo. Continuaremos a melhorar esse emulador e complementá-lo com suporte para outros dispositivos habilitados para Windows 10 vezes. Esses emuladores, combinados com versões de pré-lançamento do SDK do Windows, permitirão que você desenvolva para o Windows 10X antes que o primeiro dispositivo de tela dupla seja lançado publicamente.

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>Meus aplicativos Plataforma Universal do Windows (UWP) serão executados no Windows 10X?

A maioria dos aplicativos UWP tem suporte total no Windows 10X e funciona em dispositivos que executam o Windows 10X sem nenhuma alteração. Todas as APIs do WinRT têm suporte, assim como a maioria dos outros recursos que os aplicativos UWP têm acesso. À medida que o desenvolvimento de pré-lançamento continuar, lançaremos a documentação detalhando outros recursos sem suporte.

### <a name="will-my-win32-apps-run-on-windows-10x"></a>Meus aplicativos Win32 serão executados no Windows 10X?

O Windows 10X fornece suporte nativo para executar aplicativos Win32 em um ambiente independente. A maioria dos aplicativos Win32 pode ser executada e depurada em um dispositivo Windows 10 sem incidentes, e você também pode usar novas APIs do Win32 para adicionar suporte de tela dupla ao seu aplicativo.

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>Há algum recurso do meu aplicativo que não funcionará no Windows 10X?

À medida que o Windows 10 vezes continuar seu desenvolvimento de pré-lançamento, lançaremos a documentação destacando suas limitações específicas. No entanto, o ambiente independente usado para executar aplicativos Win32 não inclui o Shell do Windows e, portanto, as extensões de shell e recursos semelhantes não terão suporte. Da mesma forma, os dispositivos Windows 10X não dão suporte a APIs relacionadas a determinadas configurações do sistema, como opções de uso de energia.

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>Se eu aprimorar meu aplicativo com recursos do Windows 10X, ele ainda será executado em dispositivos que executam o Windows 10 na área de trabalho?

Os aplicativos projetados para o Windows 10X ainda funcionam em dispositivos que executam a versão da área de trabalho do Windows 10, embora essas novas APIs do Windows não sejam adicionadas às versões da área de trabalho do Windows 10 até a próxima atualização da versão principal. Da mesma forma que se você estivesse desenvolvendo um aplicativo com suporte em várias versões do Windows 10, siga [as práticas recomendadas de codificação adaptável](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code) para garantir que seu aplicativo funcione normalmente em 10 vezes e em desktop Windows 10. 
