---
Description: Design effective help to be displayed reactively inside your app.
title: Diretrizes da criação de ajuda no aplicativo.
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: 4783d28e4da6c06df0d0676f4a7d28ef3995481a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8333684"
---
# <a name="in-app-help-pages"></a>Páginas de ajuda no aplicativo

Na maioria das vezes, é melhor que a ajuda seja exibida dentro do aplicativo e quando o usuário optar por exibi-la.

## <a name="when-to-use-in-app-help-pages"></a>Quando usar as páginas de ajuda no aplicativo

A ajuda no aplicativo deve ser o método padrão de exibição da ajuda para o usuário. Deve ser usada para obter ajuda que seja simples e direta e não introduza novo conteúdo para o usuário. Instruções, conselhos e dicas e truques são todos adequados para a ajuda no aplicativo.

Instruções complexas ou tutoriais não são fáceis de referenciar rapidamente e ocupam grandes quantidades de espaço. Portanto, devem ser hospedados externamente e não incorporados no próprio aplicativo.

Os usuários não devem precisar procurar a ajuda para obter instruções básicas ou para descobrir novos recursos. Se você precisar de ajuda que instrua os usuários, use a interface do usuário instrucional.

## <a name="types-of-in-app-help"></a>Tipos de ajuda no aplicativo

A ajuda no aplicativo pode ser fornecida de várias formas, mas todas elas seguem os mesmos princípios gerais de design e a usabilidade.

#### <a name="help-pages"></a>Páginas de ajuda

Ter uma página ou páginas separadas de ajuda em seu aplicativo é uma maneira rápida e fácil de exibir instruções úteis.

-   **Seja conciso:** uma biblioteca grande de tópicos da ajuda é complicada e inadequada para a ajuda no aplicativo.
-   **Seja consistente:** verifique se os usuários podem acessar as páginas da ajuda da mesma maneira em qualquer parte do aplicativo. Eles nunca devem precisar procurá-la.
-   **Os usuários examinam, não leem:** como a ajuda que um usuário procura pode estar na mesma página de outros tópicos da ajuda, certifique-se de que eles possam indicar rapidamente em qual eles precisam se concentrar.


#### <a name="popups"></a>Pop-ups

Os pop-ups permitem ajuda altamente contextual exibindo instruções e conselhos que são relevantes para a tarefa específica que o usuário está tentando.

-   **Concentre-se um problema:** o espaço é ainda mais restrito em um pop-up do que em uma página da ajuda. Os pop-ups da ajuda precisam fazer referência a uma tarefa específica para serem eficazes.
-   **Visibilidade é importante:** como os pop-ups da ajuda só podem ser exibidos em um local, certifique-se de que eles estejam claramente visíveis para o usuário sem serem obstrutivos. Se o usuário não os vir, ele pode sair do pop-up em busca de uma página da ajuda.
-   **Não use muitos recursos:** a ajuda não deve demorar nem ter carregamento lento. O uso de arquivos de áudio ou vídeos ou imagens de alta resolução em pop-ups é provavelmente mais frustrante do que útil para o usuário.

#### <a name="descriptions"></a>Descrições

Às vezes, pode ser útil fornecer mais informações sobre um recurso quando um usuário o inspeciona. As descrições são semelhantes à interface do usuário instrutiva, mas a principal diferença é que a interface do usuário instrucional tenta ensinar e treinar o usuário sobre recursos que ele não conhece, enquanto descrições detalhadas melhoram o entendimento de recursos do aplicativo nos quais o usuário já está interessado.

-   **Não ensine noções básicas:** pressuponha que o usuário já conheça os conceitos básicos de como usar o item que está sendo descrito. Esclarecer ou oferecer informações adicionais é útil. Informar o que o usuário já sabe não é.
-   **Descreva interações interessantes:** um dos melhores usos de descrições é fornecer instruções sobre como um recurso que o usuário já conhece pode interagir. Isso ajuda os usuários a saber mais sobre coisas já gostam de usar.
-   **Fique fora do caminho:** de forma muito semelhante à interface do usuário instrucional, as descrições precisam evitar interferir no aproveitamento do aplicativo pelo usuário.

## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes da ajuda do aplicativo](guidelines-for-app-help.md)
