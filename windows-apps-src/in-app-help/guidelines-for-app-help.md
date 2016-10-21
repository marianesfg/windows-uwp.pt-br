---
author: QuinnRadich
Description: "Estas diretrizes descrevem como criar conteúdo da ajuda eficaz para seu aplicativo."
title: Diretrizes da ajuda do aplicativo
label: Guidelines for app help
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0aa2db498ab7ec25839da259dd0026b0a7cd2b13
ms.openlocfilehash: 0ca099b0acfcba16d62c6158c7a829adaf475891

---

# Diretrizes da ajuda do aplicativo

\[ Atualizado para aplicativos da Plataforma Universal do Windows (UWP) no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Os aplicativos podem ser complexos e o fornecimento de ajuda eficaz pode melhorar consideravelmente a experiência dos usuários. Nem todos os aplicativos precisam fornecer ajuda para seus usuários, e o tipo de ajuda fornecido pode variar bastante, dependendo do aplicativo e de como as pessoas o usarão.

Se você decidir fornecer ajuda, considere as diretrizes descritas aqui. E, lembre-se, uma ajuda que não seja útil pode ser pior que nenhuma ajuda.

## <span id="intuitive_design"></span><span id="INTUITIVE_DESIGN"></span>Design intuitivo

Mesmo que o conteúdo da ajuda seja útil, seu aplicativo não pode confiar nele para proporcionar uma boa experiência para o usuário. Se não puder descobrir e usar as funções essenciais de seu aplicativo imediatamente, a pessoa não usará seu aplicativo. Nenhum conteúdo de ajuda mudará essa primeira impressão.

Um design intuitivo e amigável é a primeira etapa para criar ajuda útil. Além de manter o usuário envolvido por tempo suficiente para usar os recursos mais avançados, a ajuda também fornece o conhecimento das funções principais do aplicativo, nas quais eles podem se basear conforme o uso.

## <span id="general_instructions"></span><span id="GENERAL_INSTRUCTIONS"></span>Instruções gerais

Um usuário não irá procurar o conteúdo da ajuda, a menos que ele já tenha um problema, portanto, a ajuda precisa fornecer uma resposta rápida e eficiente para esse problema. Se a ajuda não for prontamente útil e se for muito complicada, as pessoas provavelmente a ignorarão.

Toda ajuda, qualquer que seja, deve seguir estes princípios básicos:

-   **Fácil de entender:** a ajuda que confunde o usuário é pior que nenhuma ajuda.

-   **Simples:** os usuários que procuram a ajuda querem respostas claras apresentadas diretamente a eles.

-   **Relevante:** os usuários não querem ter a necessidade de procurar seu problema específico. Eles querem uma ajuda mais relevante apresentada em primeiro lugar (isso é chamado de "ajuda contextual") ou querem uma interface fácil de navegar.

-   **Direta:** quando um usuário procura a ajuda, eles querem ver a ajuda. Se seu aplicativo contiver páginas para relatar bugs, enviar comentários, ver os termos de serviço ou funções semelhantes, elas devem ser incluídas como links ou notas de rodapé na página principal da ajuda, e não como itens de importância.

-   **Consistente:** independentemente do seu tipo, a ajuda ainda faz parte do seu aplicativo e deve ser tratada como qualquer outra parte da interface do usuário. Os mesmos princípios de design de usabilidade, acessibilidade e estilo usados em todo o restante de seu aplicativo também devem estar presentes na ajuda que você oferece.

## <span id="types_of_help"></span><span id="TYPES_OF_HELP"></span>Tipos de ajuda

Há três categorias principais de conteúdo de ajuda, cada uma com pontos fortes e adequação variáveis para diferentes objetivos. Você pode usar qualquer combinação deles em seu aplicativo, de acordo com suas necessidades.

### <span id="instructional_ui"></span><span id="INSTRUCTIONAL_UI"></span>Interface do usuário instrucional

O ideal é que os usuários possam usar todas as funções principais de seu aplicativo sem instrução, mas, ocasionalmente, seu aplicativo pode depender do uso de um gesto específico ou pode haver recursos secundários de seu aplicativo que não sejam imediatamente óbvios. Nesses casos, a interface do usuário instrucional deve ser usada para instruir os usuários com informações sobre como realizar tarefas específicas.

[Consulte Diretrizes da interface do usuário instrucional](instructional-ui.md)

### <span id="in_app_help"></span><span id="IN_APP_HELP"></span>Ajuda no aplicativo

O método padrão da apresentação da ajuda é exibi-la dentro do aplicativo mediante solicitação do usuário. Há várias maneiras de implementar isso, como páginas da ajuda ou descrições informativas. Esse método é ideal para a ajuda de uso geral que responde diretamente às perguntas de um usuário sem complexidade.

[Consulte Diretrizes de ajuda no aplicativo](in-app-help.md)

### <span id="external_help"></span><span id="EXTERNAL_HELP"></span>Ajuda externa

Para tutoriais detalhados, funções avançadas ou bibliotecas de tópicos da ajuda muito grandes para caber em seu aplicativo, links para páginas da web externas são ideais. Se possível, esses links devem ser usados com moderação, uma vez que removem o usuário da experiência do aplicativo.

[Consulte Diretrizes de ajuda externa](external-help.md)

\[Este artigo contém informações que são específicas dos aplicativos UWP (Plataforma Universal do Windows) e do Windows 10. Para obter as diretrizes do Windows 8.1, baixe o [PDF de diretrizes do Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743).\]



<!--HONumber=Aug16_HO3-->


