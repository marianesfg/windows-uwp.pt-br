---
title: Como a emulação x86 e ARM32 funciona no ARM
author: msatranjr
description: Uma visão geral de emulação de aplicativos x86 no ARM.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, sempre conectado, emulação x86 no ARM
ms.localizationpriority: medium
ms.openlocfilehash: 6b596ab9abd31fa10d0ca07dec973082b495262e
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6043339"
---
# <a name="how-x86-emulation-works-on-arm"></a>Como a emulação x86 funciona no ARM
A emulação para aplicativos x86 disponibiliza o ecossistema avançado de aplicativos Win32 no ARM. Isso fornece ao usuário a experiência mágica da execução de um aplicativo win32 x86 existente sem nenhuma modificação no aplicativo. O aplicativo não sabe nem mesmo se está em execução em um computador Windows no ARM, a menos que chame APIs específicas ([IsWoW64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318.aspx)).

A camada [WOW64](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384249(v=vs.85).aspx) do Windows 10 permite que o código x86 seja executado na versão ARM64 do Windows 10. A emulação x86 funciona compilando blocos de instruções x86 em instruções de ARM64 com otimizações para melhorar o desempenho. Um serviço armazena em cache esses blocos de código convertidos para reduzir a sobrecarga de conversão de instrução e permitir a otimização quando o código for executado novamente. Os caches são gerados para cada módulo para que outros aplicativos possam utilizá-los na primeira inicialização. 

Para obter mais detalhes sobre essas tecnologias, consulte o vídeo do Channel9 [Windows 10 no ARM](https://channel9.msdn.com/Events/Build/2017/P4171). 