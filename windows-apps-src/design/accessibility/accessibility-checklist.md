---
author: Xansky
Description: Provides a checklist to help you ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Lista de verificação de acessibilidade
label: Accessibility checklist
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf9c3c4d803c4e5c2bc369449b83cf711bda6f99
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6673987"
---
# <a name="accessibility-checklist"></a>Lista de verificação de acessibilidade



Fornece uma lista de verificação para ajudar você a garantir que seu aplicativo da Plataforma Universal do Windows (UWP) seja acessível.

Aqui nós fornecemos uma lista de verificação que você pode usar para garantir que seu aplicativo é acessível.

1.  Defina o nome acessível (obrigatório) e a descrição (opcional) dos elementos de interface de usuário interativa e do conteúdo em seu aplicativo.

    Um nome acessível é uma cadeia de caracteres de texto descritiva e curta que um leitor de usa para anunciar um elemento de interface do usuário. Alguns elementos de interface do usuário como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) and [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) promovem o conteúdo de texto como o nome acessível padrão; consulte [Basic accessibility information](basic-accessibility-information.md#name_from_inner_text).

    Você deve definir o nome acessível de forma explicita para imagens ou outros controles que não promovem o conteúdo do texto interno como um nome acessível implícito. Você deve usar rótulos para elementos de formulário para que o texto do rótulo possa ser usado como um destino [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) no modelo de Automação da Interface do Usuário da Microsoft para correlacionar rótulos e entradas. Se você deseja fornecer mais diretrizes de interface do usuário para os usuários além das que são geralmente incluídas no nome acessível, dicas e descrições acessíveis ajudam os usuários a entender a interface do usuário.

    Para saber mais, consulte [Nome acessível](basic-accessibility-information.md#accessible_name) e [Descrição acessível.](basic-accessibility-information.md).

2.  Implementar a acessibilidade do teclado:

    * Teste a ordem do índice de tabulação padrão para uma interface do usuário. Ajuste a ordem do índice de tabulação, se necessário, que pode exigir a habilitação ou desabilitação de determinados controles ou a alteração dos valores padrão de [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) em alguns dos elementos de interface do usuário.
    * Use controles que ofereçam suporte à navegação por teclas de direção para elementos compostos. Para os controles padrão, a navegação por teclas de direção normalmente já está implementada.
    * Use controles que ofereçam suporte à ativação do teclado. Para os controles padrão, especialmente aqueles que dão suporte ao padrão [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) de Automação de Interface do Usuário, a ativação do teclado está normalmente disponível. Verifique a documentação do controle.
    * Defina as teclas de acesso ou implemente as teclas de aceleração para partes específicas da interface do usuário que ofereçam suporte à interação.
    * Para todos os controles personalizados que você usa na sua interface do usuário, verifique se que você implementou esses controles com o suporte a [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) correto para ativação e se definiu as substituições para o tratamento de chaves, conforme necessário para oferecer suporte a ativação, passagem e acesso ou chaves do acelerador.

    Para obter mais informações, consulte [Interações por teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Observe a interface do usuário para confirmar se o contraste do texto está adequado, se os elementos são renderizados corretamente nos temas de alto contraste e se as cores são usadas corretamente.

    * Use as opções de exibição do sistema que ajustam o valor de pontos por polegada (dpi) da exibição, e garanta que a interface de usuário de seu aplicativo seja dimensionada corretamente quando o valor de dpi mudar. (Alguns usuários alteram os valores de dpi como uma opção de acessibilidade; isso está disponível em **Facilidade de Acesso**).
    * Use uma ferramenta de análise de cor para verificar se a taxa de contraste visual do texto é pelo menos 4.5:1.
    * Mude para um tema de alto contraste e veja se é possível ler e usar a interface do usuário de seu aplicativo.
    * A interface do usuário não deve usar as cores como única forma de transmitir informações.

    Para obter mais informações, consulte [Temas de alto contraste](high-contrast-themes.md) e [Requisitos de texto acessível](accessible-text-requirements.md).

4.  Execute ferramentas de acessibilidade, resolva problemas relatados e verifique a experiência de leitura da tela.

    Use ferramentas como o [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) para verificar o acesso programático, execute ferramentas diagnósticas como o [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) para descobrir erros comuns e verifique a experiência de leitura da tela com o Narrador.

    Para obter mais informações, consulte [Testes de acessibilidade](accessibility-testing.md).

5.  Verifique se as configurações do manifesto do aplicativo seguem as diretrizes de acessibilidade.

6.  Declare seu aplicativo como acessível na Microsoft Store.

    Se você implementou o suporte de acessibilidade de linha base, declarar o seu aplicativo como acessível na Microsoft Store pode ajudá-lo a chegar a mais clientes e obter boas classificações adicionais.

    Para obter mais informações, consulte [Accessibility in the Store](accessibility-in-the-store.md).

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Design de acessibilidade](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Práticas que devem ser evitadas](practices-to-avoid.md) 
