---
title: Novidades nos documentos do Windows de agosto de 2017 - Desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 de agosto de 2017
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 824cdb74a809ae3f9ae2ed202a9889535021f711
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684721"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Novidades nos documentos de desenvolvedor do Windows de agosto de 2017

A Documentação do Desenvolvedor do Windows recebe atualizações constantes com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As visões gerais de recurso, diretrizes para desenvolvedores e vídeos a seguir foram disponibilizados e contêm informações novas e atualizadas para desenvolvedores do Windows.

[Instale as ferramentas e o SDK](https://developer.microsoft.com/windows/downloads#_blank) no Windows 10 e você estará pronto para [criar um aplicativo universal do Windows](../get-started/your-first-app.md) ou explorar como você pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="windows-template-studio"></a>Windows Template Studio

Use a nova extensão do [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio) para Visual Studio 2019 para criar rapidamente um aplicativo UWP com as páginas, a estrutura e os recursos desejados. Essa experiência baseada em assistente implementa padrões comprovados e práticas recomendadas para economizar seu tempo e evitar problemas ao adicionar recursos ao seu aplicativo.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>XAML condicional

Agora você pode visualizar [XAML condicional](../debug-test-perf/conditional-xaml.md) para criar [aplicativos adaptáveis à versão](../debug-test-perf/version-adaptive-apps.md). A XAML condicional permite usar o método ApiInformation.IsApiContractPresent na marcação XAML. Assim, é possível definir propriedades e instanciar objetos na marcação com base na presença de uma API, sem a necessidade de usar code-behind.

### <a name="game-mode"></a>Modo de Jogo

Com as APIs de [Modo de Jogo](https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal) para a UWP (Plataforma Universal do Windows), você pode produzir a melhor experiência de jogo, aproveitando o Modo de Jogo no Windows 10. Essas APIs estão no cabeçalho **&lt;expandedresources.h&gt;** .

![Modo de Jogo](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>A API de envio dá suporte a trailers de vídeo e opções de jogos

Agora, a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) permite que você inclua [trailers de vídeo](../monetize/manage-app-submissions.md#trailer-object) e [opções de jogos](../monetize/manage-app-submissions.md#gaming-options-object) com seus envios de aplicativo.


## <a name="developer-guidance"></a>Diretrizes do desenvolvedor

### <a name="data-schemas-for-store-products"></a>Esquemas de dados de produtos da Store

Adicionamos o artigo [Esquemas de dados para produtos da Store](../monetize/data-schemas-for-store-products.md). Este artigo fornece esquemas para os dados relacionados à Store disponíveis para vários objetos no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store), incluindo [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) e [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Ponte de Desktop

Adicionamos dois guias que ajudam você a adicionar experiências modernas que facilitam a vida dos usuários do Windows 10.

Confira [Aprimorar seu aplicativo da área de trabalho para Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) para encontrar e consultar os arquivos corretos e, em seguida, escrever códigos que destacam experiências UWP para usuários do Windows 10.  

Confira [Estender seu aplicativo da área de trabalho com componentes UWP modernos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) para incorporar interfaces do usuário XAML modernas e outras experiências UWP que devem ser executadas em um contêiner de aplicativo UWP.

### <a name="getting-started-with-point-of-service"></a>Introdução ao ponto de serviço

Adicionamos um novo guia para ajudar você [a começar a usar os dispositivos de ponto de serviço](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-get-started). Ele cobre tópicos como enumeração do dispositivo, verificação de recursos do dispositivo, declaração e compartilhamento de dispositivos. 

### <a name="xbox-live"></a>Xbox Live

Adicionamos documentos para desenvolvedores do Xbox Live, tanto para jogos da UWP quanto do XDK (Kit do Desenvolvedor Xbox).

Confira o [Guia do desenvolvedor do Xbox Live](https://docs.microsoft.com//gaming/xbox-live/index) e saiba como usar as APIs do Xbox Live para conectar seu jogo à rede social de jogos do Xbox Live.

Com o [Programa de Criadores do Xbox Live](https://docs.microsoft.com//gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators), qualquer desenvolvedor de jogos da UWP pode desenvolver e publicar um jogo habilitado para Xbox Live no computador e no Xbox One.

Confira a [Visão geral do programa para desenvolvedores do Xbox Live](https://docs.microsoft.com//gaming/xbox-live/developer-program-overview) para saber mais sobre os programas e recursos disponíveis para os desenvolvedores do Xbox Live.

## <a name="videos"></a>Vídeos

### <a name="mixed-reality"></a>Realidade Misturada

Uma série com novos tutoriais em vídeo foi lançada para o [Curso Microsoft HoloLens 250](https://developer.microsoft.com/windows/mixed-reality/mixed_reality_250). Se você já instalou as ferramentas e está familiarizado com as noções básicas de desenvolvimento de realidade misturada, confira esses cursos em vídeo para obter informações sobre como criar experiências compartilhadas em dispositivos de realidade misturada.

### <a name="narrator-and-dev-mode"></a>Narrador e modo de desenvolvedor

Talvez você já saiba que é possível usar o [Narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) para testar a experiência de leitura da tela do seu aplicativo. No entanto, o Narrador também apresenta um modo de desenvolvedor que fornece uma boa representação visual das informações expostas para ele. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode) para saber mais sobre o [Modo de desenvolvedor do Narrador](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

Veja [neste vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio) uma visão geral mais detalhada do Windows Template Studio. Quando estiver pronto, [instale a extensão](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio) ou [confira a documentação e o código-fonte](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio).