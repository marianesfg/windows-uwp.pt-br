---
author: QuinnRadich
title: Novidades do Windows Docs em agosto de 2018 - desenvolver aplicativos UWP
description: Novos recursos, vídeos, amostras e diretrizes para desenvolvedores têm foram adicionados à documentação do desenvolvedor do Windows 10 de agosto de 2018.
keywords: Novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, agosto
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4573541"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novidades dos documentos de desenvolvedor do Windows em agosto de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. O seguinte visões gerais de recursos, diretrizes para desenvolvedores e vídeos foram disponibilizados no mês de agosto.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="design"></a>Criar

Os recursos a seguir foram adicionados para o Windows compilações do Insider Preview, disponíveis por meio do programa [Windows Insider](https://insider.windows.com/) .

* A [Biblioteca de interface do usuário do Windows](https://aka.ms/winui-docs) é um conjunto de pacotes NuGet que fornecem controles e outros elementos de fichas do usuário para aplicativos UWP. Esses pacotes também são compatíveis com versões anteriores do Windows 10, para que seu aplicativo funcione mesmo se os usuários não tiverem a versão mais recente do sistema operacional.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [divisão](../design/controls-and-patterns/buttons.md#create-a-split-button)e [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fornecem controles de botão com recursos especializados para aprimorar a interface do usuário do seu aplicativo.

![Um botão de divisão para a seleção de cor de primeiro plano](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView agora oferece suporte a [navegação superior](../design/controls-and-patterns/navigationview.md), para casos em que seu aplicativo tem um número menor de opções de navegação e exigem mais espaço para conteúdo do seu aplicativo.

* TreeView foi aprimorado para dar suporte ao [modelos, item de vinculação de dados e arrastar e soltar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de código-fonte aberto que ajuda você a aplicar correções em seu aplicativo win32 quando você não tiver acesso ao código-fonte, para que ele possa executar em um contêiner de MSIX.

Para saber mais, consulte [correções de tempo de execução de aplicar um pacote MSIX usando a estrutura de suporte do pacote](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="web-api-extensions"></a>Extensões de API da Web

Uma lista de [extensões de API do Microsoft herdadas](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) foi adicionada à documentação do Mozilla Developer Network para desenvolvimento da web entre navegadores. Essas extensões de API são exclusivos do Internet Explorer ou o Microsoft Edge e complementam as informações existentes sobre o suporte de compatibilidade e navegador nos documentos de web MDN. Microsoft herdados [extensões CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) e [JavaScript extensões](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) também estão disponíveis, e você pode encontrar rich web informações sobre a API de MDN mostrada diretamente na [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C++ c++ exemplos de código do WinRT

Adicionamos 250 [C++ c++ WinRT](../cpp-and-winrt-apis/index.md) listagens para todos os tópicos documentos que acompanha C existentes + de código c++ exemplos de código CX.

### <a name="project-rome"></a>Project Rome

O site de [documentos do projeto Roma](https://docs.microsoft.com/windows/project-rome/) tiver sido reorganizado em uma abordagem de recurso primeiro. Isso deve ser mais fácil para desenvolvedores para encontrar o que estão procurando e a implementação dos recursos de sua escolha em várias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Plug-in do Xbox Live Unity

O plug-in do Xbox Live para o Unity contém suporte para adicionar a assinatura do Xbox Live, estatísticas, listas de amigos, armazenamento em nuvem e placares de líderes ao seu título. [Assista ao vídeo](https://youtu.be/fVQZ-YgwNpY) para saber mais e [baixar o pacote do GitHub](https://aka.ms/UnityPlugin) para começar.

### <a name="one-dev-question"></a>Uma pergunta sobre desenvolvimento

A série de vídeos de uma pergunta sobre desenvolvimento, há muito tempo desenvolvedores da Microsoft abrangem uma série de perguntas sobre o desenvolvimento do Windows, cultura de equipe e histórico. Veja as perguntas mais recentes que podemos respondeu!

Raymond Chen:

* [Como o kernel sabe quando reiniciar um driver de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [O que é a história por trás do objeto Burgermaster no Windows?](https://youtu.be/0TDSbyAIvX0)