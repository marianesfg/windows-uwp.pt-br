---
author: Mtoepke
title: Recursos do sistema para jogos e aplicativos UWP no Xbox One
description: UWP em recursos do sistema do Xbox
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8d6876ee6235546e74341609a55db995a77323d6
ms.lasthandoff: 02/08/2017

---

# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos do sistema para jogos e aplicativos UWP no Xbox One

Aplicativos UWP e jogos em execução no Xbox One compartilham recursos com o sistema e outros aplicativos. Portanto, jogos e aplicativos UWP terão acesso os seguintes recursos:

* A memória máxima disponível para um aplicativo em execução em primeiro plano é 1 GB.
    * A memória máxima disponível para um aplicativo em execução em segundo plano é 128 MB.
    * Aplicativos que excedem esses requisitos de memória terão falhas de alocação de memória. Para obter mais informações sobre monitoramento de uso de memória, consulte a referência [classe MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx).
    
    > [!NOTE]
    > Ao executar seu aplicativo ou jogo do depurador do Visual Studio, não se aplicam essas restrições de memória. Esse limite é aplicável apenas quando não está em execução no modo de depuração.

* Compartilhamento de 2 a 4 núcleos de CPU dependendo do número de aplicativos e jogos executados no sistema.

* Compartilhamento de 45% de GPU dependendo do número de aplicativos e jogos executados no sistema.

* A UWP no Xbox One dá suporte ao DirectX 11 Feature Level 10. Não há suporte para o DirectX 12 no momento.

* Todos os aplicativos devem ter como destino a arquitetura x64 para serem desenvolvidos ou enviados para a loja da Xbox.  

Para o **desenvolvimento de aplicativos**, é importante ter em mente que os recursos disponíveis podem ser limitados em comparação a um PC padrão.

Para **desenvolvimento de jogos**, é importante ter em mente que o Xbox One, como outros consoles de jogos, é um componente especializado de hardware que exige um kit de desenvolvimento baseado em hardware específico para acessar todo o seu potencial. Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, você pode se registrar no programa [ID@Xbox](http://www.xbox.com/Developers/id) para obter acesso aos kits de desenvolvimento do Xbox One, que incluem suporte ao DirectX 12.

## <a name="see-also"></a>Consulte também
- [UWP no Xbox One](index.md)

