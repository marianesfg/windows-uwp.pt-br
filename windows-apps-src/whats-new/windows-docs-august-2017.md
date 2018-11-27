---
title: Novidades do Windows Docs em agosto de 2017 - Desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 referente a agosto de 2017.
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aeb6f60396b270a78df5203106635436fe2dabe5
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7834278"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Novidades dos documentos de desenvolvedor do Windows em agosto de 2017

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Disponibilizaram-se as visões gerais de recurso, as diretrizes para desenvolvedores e os vídeos a seguir, contendo informações novas e atualizadas para desenvolvedores do Windows.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/your-first-app.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="windows-template-studio"></a>Windows Template Studio

Use a nova extensão do [Windows Template Studio](https://aka.ms/wtsinstall) para o Visual Studio 2017 para criar rapidamente um aplicativo UWP com as páginas, a estrutura e os recursos desejados. Essa experiência baseada em assistente implementa padrões comprovados e práticas recomendadas para economizar tempo e resolver problemas em adicionar recursos ao seu aplicativo.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>XAML condicional

Agora você pode visualizar [XAML condicional](../debug-test-perf/conditional-xaml.md) para criar [aplicativos adaptáveis de versão](../debug-test-perf/version-adaptive-apps.md). A XAML condicional permite usar o método ApiInformation.IsApiContractPresent na marcação XAML. Por isso, é possível definir propriedades e instanciar objetos na marcação com base na presença de uma API, sem a necessidade de usar code-behind.

### <a name="game-mode"></a>Modo de Jogo

As APIs de [Modo de Jogo](https://msdn.microsoft.com/library/windows/desktop/mt808808) para a Plataforma Universal do Windows (UWP) permitem produzir a experiência de jogos mais otimizada, aproveitando o modo de jogo no Windows 10. Essas APIs são localizadas no cabeçalho **&lt;expandedresources.h&gt;**.

![Modo de Jogo](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>A API de envio é compatível com trailers de vídeo e opções de jogos

A [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) agora permite incluir [trailers de vídeo](../monetize/manage-app-submissions.md#trailer-object) e [opções de jogos](../monetize/manage-app-submissions.md#gaming-options-object) com envios de seus aplicativos.


## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="data-schemas-for-store-products"></a>Esquemas de dados de produtos da Loja

Adicionamos o artigo [Esquemas de dados de produtos da Loja](../monetize/data-schemas-for-store-products.md). Este artigo fornece esquemas para os dados relacionados à Loja disponíveis para vários objetos no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx), incluindo [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) e [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Ponte de Desktop

Adicionamos dois guias que ajudam você a adicionar experiências modernas que destacam usuários do Windows 10.

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) para encontrar e fazer referência aos arquivos corretos e, em seguida, escrever códigos que destacam experiências UWP para usuários do Windows 10.  

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) para incorporar interfaces de usuário de XAML modernas e outras experiências UWP que devem ser executadas em um contêiner de aplicativo UWP.

### <a name="getting-started-with-point-of-service"></a>Introdução ao ponto de serviço

Adicionamos um novo guia para ajudá-lo [a começar a usar os dispositivos de ponto de serviço](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started). Ele aborda tópicos como enumeração do dispositivo, verificação de recursos do dispositivo, declaração de dispositivos e compartilhamento de dispositivos. 

### <a name="xbox-live"></a>Xbox Live

Adicionamos documentos para desenvolvedores do Xbox Live, em jogos de UWP e do kit do desenvolvedor Xbox (XDK).

Consulte o [Guia do desenvolvedor do Xbox Live](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/) para saber como usar as APIs do Xbox Live para conectar seu jogo à rede social de jogos do Xbox Live.

Com o [Programa de Criadores do Xbox Live](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators), qualquer desenvolvedor de jogos UWP pode desenvolver e publicar um jogo habilitado para Xbox Live no computador e no Xbox One.

Consulte a [Visão geral do programa para desenvolvedores do Xbox Live](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview) para obter informações sobre os programas e recursos disponíveis para os desenvolvedores do Xbox Live.

## <a name="videos"></a>Vídeos

### <a name="mixed-reality"></a>Realidade Misturada

Uma série de novos vídeos de tutorial foi lançada para o [Microsoft HoloLens Course 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250). Se você já instalou as ferramentas e está familiarizado com as noções básicas de desenvolvimento de realidade misturada, verifique esses cursos em vídeo para obter informações sobre como criar experiências compartilhadas em dispositivos de realidade misturada.

### <a name="narrator-and-dev-mode"></a>Narrador e Modo de Desenvolvedor

Talvez você já saiba que é possível usar o [Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started) para testar a experiência de leitura da tela do seu aplicativo. No entanto, o Narrador também apresenta um modo de desenvolvedor, que fornece uma boa representação visual das informações que são expostas para ele. [Assista ao vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode) para saber mais sobre o [Modo de desenvolvedor do Narrador](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

Uma visão geral mais detalhada do Windows Template Studio é fornecida [neste vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio). Quando estiver pronto, [instale a extensão](https://aka.ms/wtsinstall) ou [verifique a documentação e o código-fonte](https://aka.ms/wtsinstall).