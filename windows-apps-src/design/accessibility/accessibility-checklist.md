---
Description: Fornece uma lista de verificação para ajudar você a garantir que seu aplicativo da Plataforma Universal do Windows (UWP) seja acessível.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Lista de verificação de acessibilidade
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c9ff9760b3ae9b852fe1ae1b86d1cc48e49c5dd4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602441"
---
# <a name="accessibility-checklist"></a>Lista de verificação de acessibilidade

Fornece uma lista de verificação para ajudar você a garantir que seu aplicativo da Plataforma Universal do Windows (UWP) seja acessível.

Aqui nós fornecemos uma lista de verificação que você pode usar para garantir que seu aplicativo é acessível.

1. Defina o nome acessível (obrigatório) e a descrição (opcional) dos elementos de interface de usuário interativa e do conteúdo em seu aplicativo.

    Um nome acessível é uma cadeia de caracteres de texto curta e descritiva que o leitor de tela usa para anunciar um elemento de interface do usuário. Alguns elementos de interface do usuário como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) and [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) promovem o conteúdo de texto como o nome acessível padrão; consulte [Basic accessibility information](basic-accessibility-information.md#name_from_inner_text).

    Você deve definir o nome acessível de forma explicita para imagens ou outros controles que não promovem o conteúdo do texto interno como um nome acessível implícito. Você deve usar rótulos para elementos de formulário para que o texto do rótulo possa ser usado como um destino [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) no modelo de Automação da Interface do Usuário da Microsoft para correlacionar rótulos e entradas. Se você deseja fornecer mais diretrizes de interface do usuário para os usuários além das que são geralmente incluídas no nome acessível, dicas e descrições acessíveis ajudam os usuários a entender a interface do usuário.

    Para saber mais, consulte [Nome acessível](basic-accessibility-information.md#accessible_name) e [Descrição acessível](basic-accessibility-information.md).

2. Implementar a acessibilidade do teclado:

    * Teste a ordem do índice de tabulação padrão para uma interface do usuário. Ajuste a ordem do índice de tabulação, se necessário, que pode exigir a habilitação ou desabilitação de determinados controles ou a alteração dos valores padrão de [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) em alguns dos elementos de interface do usuário.
    * Use controles que ofereçam suporte à navegação por teclas de direção para elementos compostos. Para os controles padrão, a navegação por teclas de direção normalmente já está implementada.
    * Use controles que ofereçam suporte à ativação do teclado. Para os controles padrão, especialmente aqueles que dão suporte ao padrão [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) de Automação de Interface do Usuário, a ativação do teclado está normalmente disponível. Verifique a documentação do controle.
    * Defina as teclas de acesso ou implemente as teclas de aceleração para partes específicas da interface do usuário que ofereçam suporte à interação.
    * Para todos os controles personalizados que você usa na sua interface do usuário, verifique se que você implementou esses controles com o suporte a [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) correto para ativação e se definiu as substituições para o tratamento de chaves, conforme necessário para oferecer suporte a ativação, passagem e acesso ou chaves do acelerador.

    Para obter mais informações, consulte [Interações por teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3. Verifique se o texto é um tamanho legível

    * Windows incluem várias ferramentas de acessibilidade e configurações que os usuários podem aproveitar e ajustar a suas próprias necessidades e preferências para a leitura de texto. Como por exemplo:
        * A ferramenta de Lente de aumento, que amplia uma área selecionada de interface do usuário. Você deve garantir que o layout do texto em seu aplicativo não o torna difícil de usar a Lente de aumento para leitura.
        * Configurações globais de escala e a resolução no **Configurações -> sistema -> Exibir -> escala e layout**. Exatamente quais opções de dimensionamento estão disponíveis podem variar conforme isso depende dos recursos do dispositivo de vídeo.
        * As configurações de tamanho do texto no **Configurações -> a facilidade de acesso -> exibição**. Ajustar a **tornar o texto maior** configuração para especificar apenas o tamanho do texto para oferecer suporte a controles em todos os aplicativos e telas (suportam a todos os controles de texto UWP o dimensionamento experiência sem nenhuma personalização ou modelagem de texto).
        > [!NOTE]
        > O **tornar tudo maior** configuração permite que um usuário especificar o tamanho preferido para texto e aplicativos em geral, na tela principal somente.

4. Observe a interface do usuário para confirmar se o contraste do texto está adequado, se os elementos são renderizados corretamente nos temas de alto contraste e se as cores são usadas corretamente.

    * Use uma ferramenta de análise de cor para verificar se a taxa de contraste visual do texto é pelo menos 4.5:1.
    * Mude para um tema de alto contraste e veja se é possível ler e usar a interface do usuário de seu aplicativo.
    * A interface do usuário não deve usar as cores como única forma de transmitir informações.

    Para obter mais informações, consulte [Temas de alto contraste](high-contrast-themes.md) e [Requisitos de texto acessível](accessible-text-requirements.md).

5. Execute ferramentas de acessibilidade, resolva problemas relatados e verifique a experiência de leitura da tela.

    Use ferramentas como o [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) para verificar o acesso programático, execute ferramentas diagnósticas como o [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) para descobrir erros comuns e verifique a experiência de leitura da tela com o Narrador.

    Para obter mais informações, consulte [Testes de acessibilidade](accessibility-testing.md).

6. Verifique se as configurações do manifesto do aplicativo seguem as diretrizes de acessibilidade.

7. Declare seu aplicativo como acessível na Microsoft Store.

    Se você implementou o suporte de acessibilidade de linha base, declarar o seu aplicativo como acessível na Microsoft Store pode ajudá-lo a chegar a mais clientes e obter boas classificações adicionais.

    Para obter mais informações, consulte [Accessibility in the Store](accessibility-in-the-store.md).

## <a name="related-topics"></a>Tópicos relacionados  

* [Requisitos de texto acessível](accessible-text-requirements.md)
* [Escala de texto](../input/text-scaling.md)
* [Acessibilidade](accessibility.md)
* [Design para acessibilidade](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Práticas a serem evitadas](practices-to-avoid.md)
