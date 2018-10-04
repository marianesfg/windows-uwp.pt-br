---
author: Mtoepke
title: Recursos do sistema para aplicativos UWP e jogos no Xbox One
description: UWP em recursos do sistema do Xbox
ms.author: mstahl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8d6ebe8e3344f5276939471d7ac569ae83d48311
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4359803"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos do sistema para jogos e aplicativos UWP no Xbox One

Aplicativos UWP em execução no Xbox One compartilham recursos com o sistema e outros aplicativos. Os recursos disponíveis para um aplicativo UWP no Xbox One dependem se você envia como um aplicativo ou como um jogo do Programa de Criadores do Xbox Live.

* Memória máxima disponível durante a execução em primeiro plano:
    * Aplicativos: 1 GB
    * Jogos: 5 GB

A memória máxima disponível para um aplicativo em execução na tela de fundo é 128 MB. O modo de tela de fundo só se aplica a aplicativos simultâneos, como players de música na tela de fundo.  Jogos serão suspenso e encerrados na tela de fundo.

Exceder essas limitações causará falhas de alocação de memória. Para obter mais informações sobre monitoramento de uso de memória, consulte a referência [Classe MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx).
    
    > [!NOTE]
    > When running your app or game from the Visual Studio debugger, these memory constraints do not apply. This limit is only applicable when not running in debugging mode.

* CPU
    * Aplicativos: compartilhamento de 2 a 4 núcleos de CPU dependendo do número de aplicativos e jogos executados no sistema.
    * Jogos: 4 núcleos exclusivos e 2 compartilhados de CPU.

* GPU
    * Aplicativos: compartilhamento de 45% de GPU dependendo do número de aplicativos e jogos executados no sistema.
    * Jogos: acesso completo à ciclos de GPU disponíveis.

* Suporte a DirectX
    * Aplicativos: Nível 10 de recursos do DirectX 11.
    * Jogos: DirectX 12 e Nível 10 de recurso do DirectX 11.

* Todos os aplicativos e jogos devem ter como destino a arquitetura x64 para serem desenvolvidos ou enviados para a loja da Xbox.  

Para **desenvolvimento de aplicativos**, os recursos disponíveis podem ser limitados, em comparação com um computador padrão e podem variar com base no número de aplicativos e jogos executados no sistema.

Para **desenvolvimento de jogos**, o Xbox One, como outros consoles de jogos, é um componente especializado de hardware que exige um kit de desenvolvimento baseado em hardware específico para acessar todo o seu potencial. Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, considere registrar com o programa [ID@Xbox](http://www.xbox.com/Developers/id) para obter acesso a um kit de desenvolvimento do Xbox One.


Para obter mais informações sobre recursos do sistema para aplicativos UWP no Xbox One, veja a primeira parte deste vídeo.
</br>
</br>
<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developing-xbox-one-applications-16860/Video-What-s-Unique--vk0fOPf9C_2006218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="see-also"></a>Consulte também
- [UWP no Xbox One](index.md)
- [Introdução ao Programa de Criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)
- [DirectX e UWP no Xbox One](https://blogs.msdn.microsoft.com/chuckw/2017/12/15/directx-and-uwp-on-xbox-one/)

