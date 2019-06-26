---
title: Novidades nos documentos do Windows de agosto de 2018 – Desenvolver aplicativos UWP
description: Novos recursos, vídeos, amostras e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor Windows 10 de agosto de 2018.
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, agosto
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63780238"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novidades nos documentos de desenvolvedor Windows de agosto de 2018

A Documentação do Desenvolvedor Windows recebe atualizações constantes com informações sobre novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As visões gerais de recursos, as diretrizes para desenvolvedores e os vídeos a seguir foram disponibilizados em agosto.

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="design"></a>Criar

Os seguintes recursos foram adicionados aos builds do Windows Insider Preview, disponíveis pelo programa [Windows Insider](https://insider.windows.com/).

* A [biblioteca de interface do usuário do Windows](https://aka.ms/winui-docs) é um conjunto de pacotes NuGet que fornecem controles e outros elementos de interface do usuário para aplicativos UWP. Esses pacotes também são compatíveis com versões anteriores do Windows 10 para que seu aplicativo funcione mesmo que os usuários não tenham a versão do sistema operacional mais recente.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) e [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fornecem controles de botão com recursos especializados para aprimorar a interface do usuário do seu aplicativo.

![Um botão de divisão para a seleção de cor de primeiro plano](../design/controls-and-patterns/images/split-button-rtb.png)

* O NavigationView agora é compatível com a [Navegação principal](../design/controls-and-patterns/navigationview.md), para casos em que seu aplicativo tem um número menor de opções de navegação e que exigem mais espaço para o conteúdo do seu aplicativo.

* O TreeView foi aprimorado para ser compatível com [vinculação de dados, modelos de item e arrastar e soltar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>PSF (estrutura de suporte do pacote)

A estrutura de suporte do pacote é um kit open-source que ajuda a aplicar correções a seu aplicativo win32 quando não há acesso ao código-fonte para que ele possa ser executado em um contêiner do MSIX.

Para obter mais informações, consulte [Aplicar correções no tempo de execução para um pacote do MSIX usando a Estrutura de Suporte do Pacote](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Diretrizes do desenvolvedor

### <a name="web-api-extensions"></a>Extensões de API Web

Uma lista de [extensões herdadas de API da Microsoft](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) foi adicionada à documentação do Mozilla Developer Network para desenvolvimento para a Web em vários navegadores. Essas extensões de API são exclusivas do Internet Explorer ou do Microsoft Edge e complementam as informações existentes sobre o suporte de compatibilidade e navegador nos documentos da Web do MDN. As [extensões CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) e as [extensões JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) herdadas da Microsoft também estão disponíveis e você pode encontrar informações avançadas da API Web no MDN expostas diretamente no [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>Exemplos de código C++/WinRT

Adicionamos 250 listagens de código [C++/WinRT](../cpp-and-winrt-apis/index.md) aos tópicos em nossos documentos, acompanhando os exemplos de código C++/CX existentes.

### <a name="project-rome"></a>Project Rome

O site te [documentos do Project Rome](https://docs.microsoft.com/windows/project-rome/) foi reorganizado em uma abordagem que mostra primeiro os recursos. Isso deve facilitar para os desenvolvedores encontrar o que estão procurando e implementar recursos de sua preferência em várias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Plug-in Unity do Xbox Live

O plug-in do Xbox Live para Unity contém suporte para adicionar a assinatura do Xbox Live, bem como estatísticas, listas de amigos, armazenamento em nuvem e placares de líderes ao seu título. [Assista ao vídeo](https://youtu.be/fVQZ-YgwNpY) para saber mais, em seguida, [baixe o pacote do GitHub](https://aka.ms/UnityPlugin) para começar.

### <a name="one-dev-question"></a>Uma pergunta sobre desenvolvimento

Na série de vídeos Uma pergunta sobre desenvolvimento, os desenvolvedores experientes da Microsoft respondem a uma série de perguntas sobre desenvolvimento, cultura de equipe e história do Windows. Aqui estão as perguntas mais recentes que podemos ter respondido!

Raymond Chen:

* [Como o kernel sabe quando reiniciar um driver de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Qual é a história por trás do objeto Burgermaster no Windows?](https://youtu.be/0TDSbyAIvX0)