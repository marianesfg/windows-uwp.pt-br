---
Description: Estas diretrizes descrevem como criar conteúdo da ajuda eficaz para seu aplicativo.
title: Diretrizes da ajuda do aplicativo
label: Guidelines for app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: c3e73f9b-4839-4804-b379-c95b0ca4fbe8
ms.localizationpriority: medium
ms.openlocfilehash: bd2174c6bbfb84a3ea6c6956e1d0b02ed5c9be33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621361"
---
# <a name="guidelines-for-app-help"></a>Diretrizes da ajuda do aplicativo



Os aplicativos podem ser complexos e o fornecimento de ajuda eficaz pode melhorar consideravelmente a experiência dos usuários. Nem todos os aplicativos precisam fornecer ajuda para seus usuários e o tipo de ajuda que deve ser fornecido pode variar bastante, dependendo do aplicativo.

Se você decidir fornecer ajuda, siga estas diretrizes ao criá-la. Ajuda que não seja útil pode ser pior que nenhuma ajuda.

## <a name="intuitive-design"></a>Design intuitivo

Mesmo que o conteúdo da ajuda seja útil, seu aplicativo não pode confiar nele para proporcionar uma boa experiência para o usuário. Se não puder descobrir e usar as funções essenciais de seu aplicativo imediatamente, o usuário não usará seu aplicativo. A quantidade ou a qualidade da ajuda não irá alterar essa primeira impressão.

Um design intuitivo e amigável é a primeira etapa para criar ajuda útil. Além de manter o usuário envolvido por tempo suficiente para usar os recursos mais avançados, a ajuda também fornece conhecimento das funções principais do aplicativo, nas quais eles podem se basear ao continuar a usar o aplicativo e aprender.

## <a name="general-instructions"></a>Instruções gerais

Um usuário não irá procurar o conteúdo da ajuda, a menos que ele já tenha um problema, portanto, a ajuda precisa fornecer uma resposta rápida e eficiente para esse problema. Se a ajuda não for prontamente útil e se for muito complicada, os usuários provavelmente a ignorarão.

Toda ajuda, qualquer que seja, deve seguir estes princípios:

-   **Fácil de entender:** Ajuda que confunde o usuário é pior do que não há ajuda.

-   **Simples:** Usuários que desejam ajudam deseja limpar respostas apresentadas diretamente para eles.

-   **Relevante:** Os usuários não desejam precisará procurar por seu problema específico. Eles querem uma ajuda mais relevante apresentada diretamente para eles (isso é chamado de "Ajuda contextual") ou querem uma interface fácil de navegar.

-   **Direto:** Quando um usuário procura ajuda, ele deseja ver a Ajuda. Se seu aplicativo incluir páginas para relatório de bugs, envio de comentários, exibição dos termos de serviço ou funções semelhantes, é bom que sua ajuda tenha links para essas páginas. Mas eles devem ser incluídos como algo secundário na página principal da ajuda e não como itens de igual ou maior importância.

-   **Consistente:** Não importa o tipo de Ajuda ainda é uma parte do seu aplicativo e deve ser tratada como qualquer outra parte da interface do usuário. Os mesmos princípios de design de usabilidade, acessibilidade e estilo usados em todo o restante de seu aplicativo também devem estar presentes na ajuda que você oferece.

## <a name="types-of-help"></a>Tipos de ajuda

Há três categorias principais de conteúdo de ajuda, cada uma com pontos fortes e adequação variáveis para diferentes objetivos. Use qualquer combinação deles em seu aplicativo, de acordo com suas necessidades.

#### <a name="instructional-ui"></a>Interface do usuário instrucional

Normalmente, os usuários devem poder usar todas as funções principais do seu aplicativo sem nenhuma instrução. Mas, às vezes, seu aplicativo irá depender do uso de um gesto específico ou poderá conter recursos secundários que não são imediatamente óbvios. Nesse caso, a interface do usuário instrucional deve ser usada para instruir os usuários com informações sobre como realizar tarefas específicas.

[Consulte as diretrizes para das instruções da interface do usuário](instructional-ui.md)

#### <a name="in-app-help"></a>Ajuda no aplicativo

O método padrão da apresentação da ajuda é exibi-la dentro do aplicativo mediante solicitação do usuário. Há várias maneiras de apresentar isso, como em páginas da ajuda ou como descrições informativas. Esse método é ideal para a ajuda de uso geral que responde diretamente às perguntas de um usuário sem complexidade.

[Consulte as diretrizes para obter ajuda no aplicativo](in-app-help.md)

#### <a name="external-help"></a>Ajuda externa

Para tutoriais detalhados, funções avançadas ou bibliotecas de tópicos da ajuda muito grandes para caber em seu aplicativo, links para páginas da web externas são ideais. Se possível, esses links devem ser usados com moderação, uma vez que removem o usuário da experiência do aplicativo.

[Consulte as diretrizes para ajuda externo](external-help.md)


