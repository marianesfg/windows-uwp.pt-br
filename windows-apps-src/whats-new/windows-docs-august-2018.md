---
author: QuinnRadich
title: Novidades no Windows Docs em agosto de 2018 - desenvolver aplicativos UWP
description: Foram adicionados novos recursos, vídeos, exemplos e diretrizes para o desenvolvedor a documentação do desenvolvedor do Windows 10 para de 2018 de agosto.
keywords: o que há de novo, atualização, recursos, diretrizes para desenvolvedores, o Windows 10, agosto
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815965"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novidades do Windows Developer documentos em agosto de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As seguintes visões gerais dos recursos, diretrizes para o desenvolvedor e vídeos disponibilizados no mês de agosto.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="design"></a>Criar

Os seguintes recursos foram adicionados ao Windows compilações de visualização de Insider, disponíveis por meio do programa [Windows Insider](https://insider.windows.com/) .

* A [Biblioteca de interface do usuário do Windows](https://aka.ms/winui-docs) é um conjunto de pacotes do NuGet que fornecem controles e outros elementos de fichas do usuário para aplicativos UWP. Esses pacotes também são compatíveis com versões anteriores do Windows 10, seu aplicativo funciona mesmo que os usuários não tenham a versão mais recente do sistema operacional.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [divisão](../design/controls-and-patterns/buttons.md#create-a-split-button)e [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fornecem controles de botão com recursos especializados para aprimorar a interface do usuário do seu aplicativo.

![Um botão de divisão para a seleção de cor do primeiro plano](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView agora oferece suporte a [navegação superior](../design/controls-and-patterns/navigationview.md), para casos em que seu aplicativo tem um número menor de opções de navegação e exigem mais espaço para conteúdo do aplicativo.

* TreeView foi aprimorado para oferecer suporte a [associação de dados, modelos, item e arraste e solte.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de código-fonte aberto que o ajudará a aplicar correções para o seu aplicativo win32 quando você não tem acesso ao código-fonte, para que ele possa ser executado em um recipiente MSIX.

Para obter mais informações, consulte [Aplicar runtime corrige um pacote MSIX usando a estrutura de suporte do pacote](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="web-api-extensions"></a>Extensões da API da Web

Foi adicionada uma lista de [extensões de API Microsoft herdadas](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a documentação do Mozilla Developer Network para desenvolvimento web de vários navegadores. Essas extensões API são exclusivos para o Internet Explorer ou Microsoft Edge e complementam as informações existentes sobre o suporte ao navegador e compatibilidade nos documentos da web MDN. Herdado do Microsoft [extensões CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) e [JavaScript extensões](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) também estão disponíveis e você encontrará rich web informações de API de MDN reproduzidos diretamente em [código do Visual Studio.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + / exemplos de código do WinRT

Adicionamos 250 [C + + / WinRT](../cpp-and-winrt-apis/index.md) código de programação para tópicos em nossos documentos que acompanha existente C + + / exemplos de código do CX.

### <a name="project-rome"></a>Project Rome

O site do [projeto Roma docs](https://docs.microsoft.com/windows/project-rome/) tiver sido reorganizado em uma abordagem de recurso primeiro. Isso deve facilitar para os desenvolvedores a localizar o que estão procurando e implementar recursos de sua escolha em várias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Plug-in do Xbox Live Unity

O plug-in Xbox Live para Unity contém suporte para adicionar a assinatura do Xbox Live, estatísticas, listas de amigos, armazenamento em nuvem e tabelas em seu título. [Assista ao vídeo](https://youtu.be/fVQZ-YgwNpY) para saber mais e, em seguida, [Baixe o pacote do GitHub](https://aka.ms/UnityPlugin) para começar.

### <a name="one-dev-question"></a>Uma pergunta de desenv.

A série de vídeos de uma pergunta de desenvolvimento, os desenvolvedores da Microsoft há muito tempo abrangem uma série de perguntas sobre o desenvolvimento do Windows, cultura da equipe e histórico. Eis aqui as perguntas mais recentes que podemos ter respondido!

Raymond Chen:

* [Como o kernel sabe quando reiniciar um driver de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [O que é a história por trás do objeto Burgermaster no Windows?](https://youtu.be/0TDSbyAIvX0)