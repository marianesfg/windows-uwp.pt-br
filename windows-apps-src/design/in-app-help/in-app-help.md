---
Description: Crie ajuda eficaz para ser exibida reativamente em seu aplicativo.
title: Diretrizes da criação de ajuda no aplicativo.
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: 4783d28e4da6c06df0d0676f4a7d28ef3995481a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610051"
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

-   **Ser conciso:** Uma grande biblioteca de tópicos da Ajuda é complicado e inadequada para Ajuda no aplicativo.
-   **Seja consistente:** Certifique-se de que os usuários podem acessar as páginas de Ajuda da mesma forma de qualquer parte de seu aplicativo. Eles nunca devem precisar procurá-la.
-   **Verificação de usuários, não lida:** Como a Ajuda de que um usuário está procurando pode ser na mesma página como outros tópicos de Ajuda, certifique-se de que eles possam perceber facilmente quais deles precisam se concentrar em.


#### <a name="popups"></a>Pop-ups

Os pop-ups permitem ajuda altamente contextual exibindo instruções e conselhos que são relevantes para a tarefa específica que o usuário está tentando.

-   **Concentre-se de um único problema:** Espaço é ainda mais restrita em um pop-up que uma página de Ajuda. Os pop-ups da ajuda precisam fazer referência a uma tarefa específica para serem eficazes.
-   **Visibilidade é importante:** Como pop-ups de Ajuda só podem ser exibidos de um local, certifique-se de que eles são claramente visíveis para o usuário sem ser obstrutivo. Se o usuário não os vir, ele pode sair do pop-up em busca de uma página da ajuda.
-   **Não use um número excessivo de recursos:** A Ajuda não deve ficar ou ser para carregar. O uso de arquivos de áudio ou vídeos ou imagens de alta resolução em pop-ups é provavelmente mais frustrante do que útil para o usuário.

#### <a name="descriptions"></a>Descrições

Às vezes, pode ser útil fornecer mais informações sobre um recurso quando um usuário o inspeciona. As descrições são semelhantes à interface do usuário instrutiva, mas a principal diferença é que a interface do usuário instrucional tenta ensinar e treinar o usuário sobre recursos que ele não conhece, enquanto descrições detalhadas melhoram o entendimento de recursos do aplicativo nos quais o usuário já está interessado.

-   **Não ensinam as Noções básicas:** Suponha que o usuário já conhece os fundamentos de como usar o item que está sendo descrito. Esclarecer ou oferecer informações adicionais é útil. Informar o que o usuário já sabe não é.
-   **Descreva interações interessantes:** Uma das melhores utilidades para obter descrições é instruir o usuário sobre como os recursos de que já conhecem sobre podem interagir. Isso ajuda os usuários a saber mais sobre coisas já gostam de usar.
-   **Fique fora do caminho:** Muito como das instruções da interface do usuário, as descrições precisam evitar interferência com o aproveitamento de um usuário do aplicativo.

## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes para obter ajuda sobre o aplicativo](guidelines-for-app-help.md)
