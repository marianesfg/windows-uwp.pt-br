---
title: O que há de novo nos documentos do Windows em agosto de 2018 – desenvolva aplicativos UWP
description: Foram adicionados novos recursos, vídeos, exemplos e orientações para desenvolvedores para a documentação do desenvolvedor do Windows 10 de agosto de 2018.
keywords: o que há de novo, update, recursos, diretrizes para desenvolvedores, Windows 10, agosto
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616481"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>O que há de novo nos documentos de desenvolvedor do Windows em agosto de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Visões gerais de recursos, diretrizes para desenvolvedores e vídeos a seguir estavam disponíveis no mês de agosto.

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="design"></a>Criar

Os seguintes recursos foram adicionados para o Windows builds do Insider Preview, disponíveis por meio de [Windows Insider](https://insider.windows.com/) programa.

* O [biblioteca de interface do usuário do Windows](https://aka.ms/winui-docs) é um conjunto de pacotes do NuGet que fornecem elementos fichas de controles e outro usuário para aplicativos UWP. Esses pacotes também são compatíveis com versões anteriores do Windows 10, o aplicativo funciona mesmo se os usuários não tiverem a versão mais recente do sistema operacional.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), e [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fornecem controles de botão com recursos especializados para aprimorar a interface do usuário do seu aplicativo.

![Um botão de divisão para a seleção de cor de primeiro plano](../design/controls-and-patterns/images/split-button-rtb.png)

* Agora dá suporte a do NavigationView [principais navegação](../design/controls-and-patterns/navigationview.md), para casos em que seu aplicativo tem um número menor de opções de navegação e exigem mais espaço para o conteúdo do seu aplicativo.

* TreeView foi aprimorado para dar suporte a [vinculação de dados, modelos de item e arrastar e soltar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>PSF (estrutura de suporte do pacote)

A estrutura de suporte do pacote é um kit de software livre que ajuda você a aplicar correções para o seu aplicativo win32 quando você não tem acesso ao código-fonte, para que ele pode ser executado em um contêiner de MSIX.

Para obter mais informações, consulte [tempo de execução de aplicar correções a um pacote MSIX usando a estrutura de suporte do pacote](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="web-api-extensions"></a>Extensões de API da Web

Uma lista dos [extensões herdadas da API do Microsoft](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) foi adicionada a documentação do Mozilla Developer Network para desenvolvimento da web de vários navegadores. Essas extensões de API são exclusivos para o Internet Explorer ou Microsoft Edge e complementam as informações existentes sobre o suporte de compatibilidade e navegador nos documentos da web do MDN. Microsoft herdado [extensões CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) e [extensões JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) também estão disponíveis e você pode encontrar web avançados informações da API do MDN são exibidas diretamente na [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + / exemplos de código do WinRT

Adicionamos 250 [C + + c++ /CLI WinRT](../cpp-and-winrt-apis/index.md) listagens de tópicos de instruções nossos documentos, que acompanha C + existente de código c++ /CLI exemplos de código do CX.

### <a name="project-rome"></a>Project Rome

O [docs projeto Roma](https://docs.microsoft.com/windows/project-rome/) site tiver sido reorganizado em uma abordagem primeiro de recurso. Isso deve facilitar para os desenvolvedores para encontrar o que estão procurando e implementar recursos de sua preferência em várias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Plug-in do Xbox Live Unity

O plug-in Xbox Live para Unity contém suporte para adicionar a assinatura Xbox Live, estatísticas, listas de amigos, armazenamento em nuvem e placares de líderes ao seu título. [Assista ao vídeo](https://youtu.be/fVQZ-YgwNpY) para saber mais, em seguida, [Baixe o pacote do GitHub](https://aka.ms/UnityPlugin) para começar a usar.

### <a name="one-dev-question"></a>Uma pergunta de desenvolvimento

A série de vídeos de uma pergunta de desenvolvimento, os desenvolvedores da Microsoft há muito tempo abrangem uma série de perguntas sobre desenvolvimento Windows, a cultura da equipe e o histórico. Eis aqui as perguntas mais recente que podemos ter respondido!

Raymond Chen:

* [Como o kernel sabe quando reiniciar um driver de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [O que é a história por trás do objeto Burgermaster no Windows?](https://youtu.be/0TDSbyAIvX0)