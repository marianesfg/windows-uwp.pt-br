---
title: Recursos do sistema para jogos e aplicativos UWP no Xbox One
description: UWP em recursos do sistema do Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 40cf41df4ccf15206e5855f989bc46c599baa473
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372074"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos do sistema para jogos e aplicativos UWP no Xbox One

Aplicativos UWP em execução no Xbox One compartilham recursos com o sistema e outros aplicativos. Os recursos disponíveis para um aplicativo UWP no Xbox One dependem se você envia como um aplicativo ou como um jogo do Programa de Criadores do Xbox Live.

* Memória máxima disponível durante a execução em primeiro plano:
    * Aplicativos: 1 GB
    * Jogos: 5 GB

A memória máxima disponível para um aplicativo em execução em segundo plano é 128 MB. O modo de tela de fundo só se aplica a aplicativos simultâneos, como players de música na tela de fundo.  Jogos serão suspenso e encerrados na tela de fundo.

Exceder essas limitações causará falhas de alocação de memória. Para obter mais informações sobre monitoramento de uso de memória, consulte a referência [classe MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager).
    
    > [!NOTE]
    > When running your app or game from the Visual Studio debugger, these memory constraints do not apply. This limit is only applicable when not running in debugging mode.

* CPU
    * Aplicativos: compartilhamento de 2 a 4 núcleos de CPU dependendo do número de aplicativos e jogos executados no sistema.
    * Jogos: 4 exclusivo e 2 compartilhado núcleos de CPU.

* GPU
    * Aplicativos: compartilhamento de 45% de GPU dependendo do número de aplicativos e jogos executados no sistema.
    * Jogos: acesso completo à ciclos de GPU disponíveis.

* Suporte a DirectX
    * Aplicativos: Nível de recurso do DirectX 11 10.
    * Jogos: O DirectX 12 e nível de recurso do DirectX 11 10.

* Todos os aplicativos e jogos devem ter como destino a arquitetura x64 para serem desenvolvidos ou enviados para a loja da Xbox.  

Para **desenvolvimento de aplicativos**, os recursos disponíveis podem ser limitados, em comparação com um computador padrão e podem variar com base no número de aplicativos e jogos executados no sistema.

Para **desenvolvimento de jogos**, o Xbox One, como outros consoles de jogos, é um componente especializado de hardware que exige um kit de desenvolvimento baseado em hardware específico para acessar todo o seu potencial. Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, considere registrar com o programa [ID@Xbox](https://www.xbox.com/Developers/id) para obter acesso a um kit de desenvolvimento do Xbox One.


Para obter mais informações sobre recursos do sistema para aplicativos UWP no Xbox One, veja a primeira parte deste vídeo.
</br>
</br>
<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developing-xbox-one-applications-16860/Video-What-s-Unique--vk0fOPf9C_2006218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="see-also"></a>Consulte também
- [UWP no Xbox One](index.md)
- [Comece com o programa de criadores do Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [O DirectX e UWP no Xbox One](https://walbourn.github.io/)

