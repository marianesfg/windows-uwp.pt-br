---
title: Teste em um único Console do Xbox
description: Saiba como testar os serviços do Xbox Live no Console do Xbox Live
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, jogos, xbox, xbox live, xbox um
ms.localizationpriority: low
ms.openlocfilehash: aa14071f5ddc9af778fc59cc20891bf83310088b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628011"
---
# <a name="testing-on-the-xbox-one-console"></a>Um teste no console do Xbox One

Ao desenvolver seu título para a família Xbox One dos consoles é natural que seria você deseja ser capaz de testar seu título e os recursos do Xbox Live em um console real. Há algumas opções para testar seu título no hardware. Você pode usar qualquer varejo Xbox um único Console para testar um aplicativo ou o título da plataforma Universal do Windows (UWP), ativando o modo de desenvolvedor do console. Essa opção é acessível a todos os desenvolvedores e é a única opção para os desenvolvedores de programa de criadores do Xbox Live. ID@Xbox e parceiros gerenciados tem a opção de ordenação e usando um Kit de desenvolvimento do Xbox.

## <a name="retail-console-testing-xbox-live-creators"></a>Teste de console de varejo: Criadores do Xbox Live

Ativando o modo de desenvolvedor em um console Xbox One do varejo permitirá que você implante títulos UWP e aplicativos em um Console Xbox através do emparelhamento dele com um build do Visual Studio. Isso é o console de teste de opção para os desenvolvedores de programa de criadores do Xbox Live. Você não poderá testar jogos XDK em um console do Xbox One de varejo.

* Siga as [instruções de ativação do modo de desenvolvedor](../xbox-apps/devkit-activation.md) para permitir o desenvolvimento de testes no seu console de varejo.  
* Carregar seu título no Xbox One, seguindo a [Configurando as instruções do Xbox One](../xbox-apps/development-environment-setup.md#setting-up-your-xbox-one).  
* Siga as [instruções de desativação do modo de desenvolvedor](../xbox-apps/devkit-deactivation.md) para colocar seu console de volta no modo de varejo ou desinstalar o ambiente de desenvolvimento no seu console de varejo.  
* Embora o console está no modo de desenvolvedor você pode acessá-lo remotamente por meio de seu PC usando o [portal de dispositivos do Windows para o Xbox](../debug-test-perf/device-portal-xbox.md).  

## <a name="xbox-development-kit-testing-idxbox-and-managed-partners"></a>Testes de kit de desenvolvimento de Xbox: ID@Xbox e parceiros gerenciados

Gerenciado parceiros e ID@Xbox os desenvolvedores têm a opção de adquirir um kit de desenvolvedor do Xbox do [Store de desenvolvimento do Xbox](https://gamedevstore.partners.extranet.microsoft.com/), que é acessível apenas para aqueles com contas de desenvolvedor gerenciado. Kits de desenvolvedor do Xbox permitirá que você carregue XDK jogos para o console para testar, jogos UWP também podem ser testados pelo kit de desenvolvimento. Kits de desenvolvedor vêm com as opções de hardware e os recursos de teste que permitem que o gerenciamento de teste e o console de desempenho mais detalhado.

Comece sua jornada com o kit de desenvolvedor do Xbox ler o [Introdução ao artigo de desenvolvimento de um Xbox](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-getting-started) no site de documentação do parceiro gerenciado. Esta documentação é acessível somente aos desenvolvedores autorizados no ID@Xbox programa e parceiros gerenciados.