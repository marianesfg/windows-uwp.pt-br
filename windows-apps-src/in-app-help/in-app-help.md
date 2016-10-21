---
author: QuinnRadich
Description: Crie ajuda eficaz para ser exibida reativamente em seu aplicativo.
title: "Diretrizes da criação de ajuda no aplicativo."
label: In-app help
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0d13ca6850df1b575eddebdb36b345423392664f
ms.openlocfilehash: 0c606e3b5be2889e83efce77d4c98ca646fef725

---

# Páginas de ajuda no aplicativo

Na maioria das vezes, é melhor que a ajuda seja exibida dentro do aplicativo e quando o usuário optar por exibi-la. Considere as diretrizes a seguir ao criar a ajuda no aplicativo.

## <span id="when_to_use_in_app_help"></span><span id="WHEN_TO_USE_IN_APP_HELP"></span>Quando usar as páginas de ajuda no aplicativo

A ajuda no aplicativo deve ser o método padrão de exibição da ajuda para o usuário. Deve ser usada para obter ajuda que seja simples e direta e não introduza novo conteúdo para o usuário. Instruções, conselhos e dicas e truques são todos adequados para a ajuda no aplicativo.

Instruções ou tutoriais complexos não são fáceis de consultar rapidamente e ocupam muito espaço. Portanto, esses materiais devem ser hospedados externamente e não incorporados no próprio aplicativo.

Os usuários não devem precisar procurar a ajuda para obter instruções básicas ou para descobrir novos recursos. Se você precisar de ajuda que instrua os usuários, use a interface do usuário instrucional.

## <span id="types_of_in_app_help"></span><span id="TYPES_OF_IN_APP_HELP"></span>Tipos de ajuda no aplicativo

A ajuda no aplicativo pode ser fornecida de várias formas, mas todas elas seguem os mesmos princípios gerais de design e a usabilidade.

### <span id="help_pages"></span><span id="HELP_PAGES"></span>Páginas de ajuda

Ter uma página ou páginas separadas de ajuda em seu aplicativo é uma maneira rápida e fácil de exibir instruções úteis:

-   **Seja conciso:** uma biblioteca grande de tópicos da ajuda é complicada e inadequada para a ajuda no aplicativo.
-   **Seja consistente:** verifique se os usuários podem acessar as páginas da ajuda da mesma maneira em qualquer parte do aplicativo. Eles nunca devem precisar procurá-la.
-   **Tenha em mente que os usuários examinam, eles não leem:** como a ajuda que um usuário está procurando pode estar na mesma página que outros tópicos da ajuda, verifique se podem perceber em qual tópico eles precisam se concentrar.


### <span id="popups"></span><span id="POPUPS"></span>Pop-ups

Os pop-ups permitem ajuda altamente contextual exibindo instruções e conselhos que são relevantes para a tarefa específica que o usuário está tentando:

-   **Concentre-se um problema:** o espaço é ainda mais restrito em um pop-up do que em uma página da ajuda. Os pop-ups da ajuda precisam fazer referência a uma tarefa específica para serem eficazes.
-   **Visibilidade é importante:** como os pop-ups da ajuda só podem ser exibidos em um local, verifique se eles estão claramente visíveis para o usuário sem serem obstrutivos. Se o usuário não os vir, ele pode sair do pop-up em busca de uma página da ajuda.
-   **Não use muitos recursos:** a ajuda não deve demorar nem ter carregamento lento. O uso de arquivos de áudio ou vídeos ou imagens de alta resolução em pop-ups é provavelmente mais frustrante do que útil para o usuário.

### <span id="descriptions"></span><span id="DESCRIPTIONS"></span>Descrições

Às vezes, pode ser útil fornecer mais informações sobre um recurso quando um usuário o inspeciona. As descrições são semelhantes à interface do usuário instrutiva, mas a principal diferença é que a interface do usuário instrucional tenta ensinar e treinar o usuário sobre recursos que ele não conhece, enquanto descrições detalhadas melhoram o entendimento de recursos do aplicativo nos quais o usuário já está interessado:

-   **Não ensine noções básicas:** pressuponha que o usuário já conheça os conceitos básicos de como usar o item que está sendo descrito. Esclarecer ou oferecer informações adicionais é útil. Informar o que o usuário já sabe não é.
-   **Descreva interações interessantes:** um dos melhores usos de descrições é fornecer instruções sobre como um recurso que o usuário já conhece pode interagir. Isso ajuda os usuários a saber mais sobre coisas já gostam de usar.
-   **Fique fora do caminho:** de forma muito semelhante à interface do usuário instrucional, as descrições precisam evitar interferir no aproveitamento do aplicativo pelo usuário.

## <span id="related_topics"></span>Artigos relacionados

* [Diretrizes da ajuda do aplicativo](guidelines-for-app-help.md)



<!--HONumber=Aug16_HO3-->


