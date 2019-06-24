---
title: Como a emulação x86 e ARM32 funciona no ARM
description: Uma visão geral de emulação de aplicativos x86 no ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, sempre conectado, emulação x86 no ARM
ms.localizationpriority: medium
ms.openlocfilehash: 8af6ba39468dd16a043040b797a03c862d6ce7db
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319654"
---
# <a name="how-x86-emulation-works-on-arm"></a>Como a emulação x86 funciona no ARM
A emulação para aplicativos x86 disponibiliza o ecossistema avançado de aplicativos Win32 no ARM. Isso fornece ao usuário a experiência mágica da execução de um aplicativo win32 x86 existente sem nenhuma modificação no aplicativo. O aplicativo não sabe nem mesmo se está em execução em um computador Windows no ARM, a menos que chame APIs específicas ([IsWoW64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)).

A camada [WOW64](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications) do Windows 10 permite que o código x86 seja executado na versão ARM64 do Windows 10. A emulação x86 funciona compilando blocos de instruções x86 em instruções de ARM64 com otimizações para melhorar o desempenho. Um serviço armazena em cache esses blocos de código convertidos para reduzir a sobrecarga de conversão de instrução e permitir a otimização quando o código for executado novamente. Os caches são gerados para cada módulo para que outros aplicativos possam utilizá-los na primeira inicialização. 

Para obter mais detalhes sobre essas tecnologias, consulte o vídeo do Channel9 [Windows 10 no ARM](https://channel9.msdn.com/Events/Build/2017/P4171). 