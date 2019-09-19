---
title: Recursos do sistema para jogos e aplicativos UWP no Xbox One
description: UWP em recursos do sistema do Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: a78d669902876cdb05a24cee421b0f95a71de31a
ms.sourcegitcommit: bac5574a1f47a5b38c984a5482272c9e49a9c91e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71100839"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos do sistema para jogos e aplicativos UWP no Xbox One

Aplicativos UWP em execução no Xbox One compartilham recursos com o sistema e outros aplicativos. Os recursos disponíveis para um aplicativo UWP no Xbox One dependem se você envia como um aplicativo ou como um jogo do Programa de Criadores do Xbox Live.

* Memória máxima disponível durante a execução em primeiro plano:
    * Os 1 GB
    * Jogos 5 GB

A memória máxima disponível para um aplicativo em execução em segundo plano é 128 MB. O modo de tela de fundo só se aplica a aplicativos simultâneos, como players de música na tela de fundo.  Jogos serão suspenso e encerrados na tela de fundo.


Exceder essas limitações causará falhas de alocação de memória. Para obter mais informações sobre monitoramento de uso de memória, consulte a referência [classe MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Ao executar seu aplicativo ou jogo do depurador do Visual Studio, essas restrições de memória não se aplicam. Esse limite é aplicável apenas quando não está em execução no modo de depuração.

* CPU
    * Aplicativos: compartilhamento de 2 a 4 núcleos de CPU dependendo do número de aplicativos e jogos executados no sistema.
    * Jogos 4 núcleos de CPU exclusivos e 2 compartilhados.

* GPU
    * Aplicativos: compartilhamento de 45% de GPU dependendo do número de aplicativos e jogos executados no sistema.
    * Jogos: acesso completo à ciclos de GPU disponíveis.

* Suporte a DirectX
    * Os Nível de recurso 10 do DirectX 11.
    * Jogos O DirectX 12 e o DirectX 11 de nível de recurso 10.

* Todos os aplicativos e jogos devem ter como destino a arquitetura x64 para serem desenvolvidos ou enviados para a loja da Xbox.  

Para **desenvolvimento de aplicativos**, os recursos disponíveis podem ser limitados, em comparação com um computador padrão e podem variar com base no número de aplicativos e jogos executados no sistema.

Para **desenvolvimento de jogos**, o Xbox One, como outros consoles de jogos, é um componente especializado de hardware que exige um kit de desenvolvimento baseado em hardware específico para acessar todo o seu potencial. Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, considere registrar com o programa [ID@Xbox](https://www.xbox.com/Developers/id) para obter acesso a um kit de desenvolvimento do Xbox One.

## <a name="see-also"></a>Consulte também
- [UWP no Xbox One](index.md)
- [Introdução ao programa de criadores do Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX e UWP no Xbox One](https://walbourn.github.io/)