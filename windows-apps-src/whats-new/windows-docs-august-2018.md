---
author: QuinnRadich
title: What's New in Windows Docs em agosto de 2018 - como desenvolver aplicativos UWP
description: Foram adicionados novos recursos, vídeos, exemplos e orientação do desenvolvedor para a documentação do desenvolvedor do Windows 10 para agosto de 2018.
keywords: What's new, atualização, recursos, orientação do desenvolvedor, Windows 10, agosto
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2888105"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>What's New in os documentos do desenvolvedor de Windows no de 2018 de agosto

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As seguintes visões gerais do recurso, orientação do desenvolvedor e vídeos tenham sido disponibilizados no mês de agosto.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="design"></a>Criar

Os seguintes recursos foram adicionados Windows compilações Insider Preview, disponíveis por meio do programa [Insider Windows](https://insider.windows.com/) .

* A [Biblioteca de interface do usuário do Windows](https://aka.ms/winui-docs) é um conjunto de pacotes NuGet que fornecem controles e outros elementos de fichas de usuário para aplicativos UWP. Esses pacotes também são compatíveis com versões anteriores do Windows 10, seu aplicativo funciona mesmo quando os usuários não têm a versão mais recente do sistema operacional.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [divisão](../design/controls-and-patterns/buttons.md#create-a-split-button)e [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fornecem controles de botão com recursos especializados para aprimorar a interface do usuário do seu aplicativo.

![Um botão de divisão para selecionar a cor de primeiro plano](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView agora oferece suporte a [navegação superior](../design/controls-and-patterns/navigationview.md), para casos em que o seu aplicativo tem um número menor de opções de navegação e exige mais espaço para o conteúdo do seu aplicativo.

* TreeView foi aprimorada para oferecer suporte a [associação de dados, modelos, item e arrastar e soltar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de código-fonte aberto ajudá-lo a aplicar correções ao seu aplicativo win32 quando você não tem acesso ao código-fonte, para que ele possa ser executado em um recipiente MSIX.

Para saber mais, consulte [Aplicar runtime corrige a um pacote de MSIX usando a estrutura de suporte do pacote](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="web-api-extensions"></a>Extensões de API da Web

Uma lista de [extensões de API do Microsoft herdadas](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) foi adicionada à documentação para o desenvolvimento de vários navegadores web Mozilla Developer Network. Essas extensões de API são exclusivos para o Internet Explorer ou Microsoft Edge e complementam existentes informações sobre o suporte de compatibilidade e navegador nos documentos MDN web. Herdado Microsoft [extensões CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) e [Extensões JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) também estão disponíveis e você pode encontrar rich web informações de API do MDN surgem diretamente no [código do Visual Studio.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + / exemplos de código do WinRT

Adicionamos 250 [C + + / WinRT](../cpp-and-winrt-apis/index.md) código listagens para tópicos em nossos docs, que acompanha existente C + + / exemplos de código CX.

### <a name="project-rome"></a>Project Rome

O site de [projeto Roma docs](https://docs.microsoft.com/windows/project-rome/) tiver sido reorganizado a uma abordagem de recurso primeiro. Isso deve facilitar para os desenvolvedores para encontrar o que estão procurando e implementar os recursos de sua preferência em diversas plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Plug-in do Xbox Live unidade

O plug-in Xbox Live para unidade contém suporte para adicionar a assinatura Xbox Live, stats, listas de amigos, armazenamento na nuvem e tabelas para seu título. [Assista ao vídeo](https://youtu.be/fVQZ-YgwNpY) para saber mais e [baixar o pacote GitHub](https://aka.ms/UnityPlugin) para começar.

### <a name="one-dev-question"></a>Uma pergunta de desenv.

A série de vídeos de uma pergunta Dev, há muito tempo desenvolvedores da Microsoft cobrem uma série de perguntas sobre o desenvolvimento, cultura de equipe e histórico do Windows. Eis as perguntas mais recentes que podemos atender!

Raymond Chen:

* [Como o kernel sabe quando reiniciar um driver de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Qual é a história atrás do objeto Burgermaster no Windows?](https://youtu.be/0TDSbyAIvX0)