---
Description: Crie uma interface do usuário das instruções (IU) que ensina os usuários a trabalhar com seu aplicativo UWP.
title: Diretrizes para criar uma interface do usuário instrucional
label: Instructional UI
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: b39507fb1333fdb642601c6b4828c3d160c6ceb5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656431"
---
# <a name="instructional-ui-guidelines"></a>Diretrizes da interface do usuário instrucional



Em algumas circunstâncias, pode ser útil ensinar o usuário sobre funções em seu aplicativo que podem não ser óbvias, como interações de toque específicas. Nesses casos, você precisa apresentar instruções para o usuário por meio da interface do usuário, para que ele possa usar esses recursos que talvez tenha ignorado.

## <a name="when-to-use-instructional-ui"></a>Quando usar a interface do usuário instrucional

A interface do usuário instrucional deve ser usada com cuidado. Quando usada em excesso, ela pode ser facilmente ignorada ou incomodar o usuário, tornando-se ineficaz.

A interface do usuário instrucional deve ser usada para ajudar o usuário a descobrir recursos importantes de seu aplicativo que não são óbvios, como gestos de toque ou configurações nos quais ele podem estar interessados. Também pode ser usada para informar os usuários sobre novos recursos ou alterações em seu aplicativo que, de outra maneira, podem ser ignorados.

A menos que seu aplicativo dependa de gestos de toque, interface do usuário instrucional não deve ser usada para ensinar os usuários os recursos fundamentais do seu aplicativo.

## <a name="principles-of-writing-instructional-ui"></a>Princípios de escrita da interface do usuário instrucional

Uma boa interface do usuário instrucional é relevante e educativa para o usuário e melhora a experiência do usuário. Ela deve ser:

-   **Simples:** Os usuários não querem sua experiência seja interrompida com informações complicadas
-   **Fácil de ser lembrado:** Os usuários não querem ver as mesmas instruções toda vez que ele tente uma tarefa, portanto, precisam de instruções seja algo que vai se lembrar.
-   **Imediatamente relevante:** Se a interface do usuário das instruções não ensinar a um usuário sobre algo que eles desejam fazer imediatamente, eles não tem um motivo para prestar atenção a ela.

Evite o uso excessivo de interface do usuário instrucional e certifique-se de escolher os tópicos certos. Não ensine:

-   **Recursos fundamentais:** Se um usuário precisar de instruções para usar seu aplicativo, considere o design de aplicativo mais intuitiva.
-   **Recursos óbvios:** Se um usuário pode descobrir um recurso por conta própria, sem a instrução, a interface do usuário das instruções apenas receberá da forma.
-   **Recursos complexos:** Interface do usuário das instruções deve ser conciso, e usuários interessados em recursos complexos são normalmente dispostos a busca de instruções e não precisam ser concedido a eles.

Evite ser inconveniente para o usuário com sua interface do usuário instrucional. Não:

-   **Obscuras informações importantes:** Interface do usuário das instruções nunca deve ser em termos de outros recursos de seu aplicativo.
-   **Forçar os usuários a participar:** Os usuários devem ser capazes de ignorar a interface do usuário das instruções e ainda o andamento por meio do aplicativo.
-   **Exibindo informações de repetição:** Não perseguir o usuário com a interface do usuário das instruções, mesmo se eles ignorá-la na primeira vez. Adicionar uma configuração para exibir a interface do usuário instrucional novamente é a melhor solução.

## <a name="examples-of-instructional-ui"></a>Exemplos de interface do usuário instrucional

Estas são algumas situações nas quais a interface do usuário instrucional pode ajudar seus usuários a aprender:

-   **Ajudando os usuários a descobrir interações de toque.** A captura de tela a seguir mostra uma interface do usuário instrucional ensinando um jogador a usar gestos de touch no jogo Cut the Rope.

    ![captura de tela de jogo mostrando mensagem da interface do usuário instrucional, "deslize para cortar a corda"](images/in-game-controls-3.png)

-   **Fazendo uma primeira impressão excelente.** Quando o Editor de Vídeos é iniciado pela primeira vez, a interface do usuário instrucional solicita que o usuário comece a criar vídeos sem obstruir sua experiência.

    ![tela de inicialização do aplicativo Editor de Vídeos](images/instructional-ui-movie.png)

-   **A orientação dos usuários para ir para a próxima etapa em uma tarefa complicada.** No aplicativo Windows Mail, uma dica na parte inferior da Caixa de Entrada direciona os usuários até as **Configurações** para acessar mensagens mais antigas.

    ![captura de tela cortada do aplicativo windows mail mostrando uma mensagem da interface do usuário instrucional](images/instructional-ui-mail-inbox.png)

    Quando o usuário clica na mensagem, o submenu **Configurações** do aplicativo aparece no lado direito da tela, permitindo a conclusão da tarefa. Estas capturas de tela mostram o aplicativo Mail antes e depois de um usuário clicar na mensagem da interface do usuário instrucional.

    | Antes                                                               | Depois                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![captura de tela do aplicativo windows mail](images/instructional-ui-mail.png) | ![captura de tela do aplicativo de email do windows com um submenu configurações estendido](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes para obter ajuda sobre o aplicativo](guidelines-for-app-help.md)
