---
Description: Fornece uma lista de verificação para ajudá-lo a garantir que seu aplicativo do Windows esteja acessível.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Lista de verificação de acessibilidade
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c7775c2d6c9e579e14c9f607fa0a09b665dbb24b
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969757"
---
# <a name="accessibility-checklist"></a>Lista de verificação de acessibilidade

Fornece uma lista de verificação para ajudá-lo a garantir que seu aplicativo do Windows esteja acessível.

Aqui nós fornecemos uma lista de verificação que você pode usar para garantir que seu aplicativo é acessível.

1. Defina o nome acessível (obrigatório) e a descrição (opcional) dos elementos de interface de usuário interativa e do conteúdo em seu aplicativo.

    Um nome acessível é uma cadeia de caracteres de texto curta e descritiva que o leitor de tela usa para anunciar um elemento de interface do usuário. Alguns elementos de interface do usuário como [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) and [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) promovem o conteúdo de texto como o nome acessível padrão; consulte [Basic accessibility information](basic-accessibility-information.md#name_from_inner_text).

    Você deve definir o nome acessível de forma explicita para imagens ou outros controles que não promovem o conteúdo do texto interno como um nome acessível implícito. Você deve usar rótulos para elementos de formulário para que o texto do rótulo possa ser usado como um destino [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) no modelo de Automação da Interface do Usuário da Microsoft para correlacionar rótulos e entradas. Se você deseja fornecer mais diretrizes de interface do usuário para os usuários além das que são geralmente incluídas no nome acessível, dicas e descrições acessíveis ajudam os usuários a entender a interface do usuário.

    Para saber mais, consulte [Nome acessível](basic-accessibility-information.md#accessible_name) e [Descrição acessível.](basic-accessibility-information.md).

2. Implementar a acessibilidade do teclado:

    * Teste a ordem do índice de tabulação padrão para uma interface do usuário. Ajuste a ordem do índice de tabulação, se necessário, que pode exigir a habilitação ou desabilitação de determinados controles ou a alteração dos valores padrão de [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) em alguns dos elementos de interface do usuário.
    * Use controles que ofereçam suporte à navegação por teclas de direção para elementos compostos. Para os controles padrão, a navegação por teclas de direção normalmente já está implementada.
    * Use controles que ofereçam suporte à ativação do teclado. Para os controles padrão, especialmente aqueles que dão suporte ao padrão [**Invoke**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) de Automação de Interface do Usuário, a ativação do teclado está normalmente disponível. Verifique a documentação do controle.
    * Defina as teclas de acesso ou implemente as teclas de aceleração para partes específicas da interface do usuário que ofereçam suporte à interação.
    * Para todos os controles personalizados que você usa na sua interface do usuário, verifique se que você implementou esses controles com o suporte a [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) correto para ativação e se definiu as substituições para o tratamento de chaves, conforme necessário para oferecer suporte a ativação, passagem e acesso ou chaves do acelerador.

    Para obter mais informações, consulte [Interações por teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions).

3. Garantir que o texto seja um tamanho legível

    * O Windows inclui várias ferramentas de acessibilidade e configurações que os usuários podem aproveitar e ajustar às suas próprias necessidades e preferências para ler o texto. Eles incluem:
        * A ferramenta Lupa, que amplia uma área selecionada da interface do usuário. Você deve garantir que o layout do texto em seu aplicativo não torna difícil usar a lupa para leitura.
        * Configurações globais de escala e resolução em **configurações – >sistema->exibição->escala e layout**. Exatamente quais opções de dimensionamento estão disponíveis podem variar, pois isso depende dos recursos do dispositivo de vídeo.
        * Configurações de tamanho de texto em **configurações->facilidade de acesso->exibição**. Ajuste a configuração **tornar texto maior** para especificar apenas o tamanho do texto em controles de suporte em todos os aplicativos e telas (todos os controles de texto UWP dão suporte à experiência de dimensionamento de texto sem qualquer personalização ou modelagem).
        > [!NOTE]
        > A configuração **tornar tudo maior** permite que um usuário especifique seu tamanho preferido para texto e aplicativos em geral somente na tela principal.

4. Verifique sua interface do usuário para garantir que o contraste do texto esteja adequado, que os elementos renderizem corretamente nos temas em alto contraste e que as cores estejam sendo usadas corretamente.

    * Use uma ferramenta de análise de cor para verificar se a taxa de contraste visual do texto é pelo menos 4.5:1.
    * Mude para um tema de alto contraste e veja se é possível ler e usar a interface do usuário de seu aplicativo.
    * A interface do usuário não deve usar as cores como única forma de transmitir informações.

    Para obter mais informações, consulte [Temas de alto contraste](high-contrast-themes.md) e [Requisitos de texto acessível](accessible-text-requirements.md).

5. Execute ferramentas de acessibilidade, resolva problemas relatados e verifique a experiência de leitura da tela.

    Use ferramentas como o [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) para verificar o acesso programático, execute ferramentas diagnósticas como o [**AccChecker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker) para descobrir erros comuns e verifique a experiência de leitura da tela com o Narrador.

    Para obter mais informações, consulte [Testes de acessibilidade](accessibility-testing.md).

6. Verifique se as configurações do manifesto do aplicativo seguem as diretrizes de acessibilidade.

7. Declare seu aplicativo como acessível na Microsoft Store.

    Se você implementou o suporte de acessibilidade de linha base, declarar o seu aplicativo como acessível na Microsoft Store pode ajudá-lo a chegar a mais clientes e obter boas classificações adicionais.

    Para obter mais informações, consulte [Accessibility in the Store](accessibility-in-the-store.md).

## <a name="related-topics"></a>Tópicos relacionados  

* [Requisitos de texto acessível](accessible-text-requirements.md)
* [Colocar texto em escala](../input/text-scaling.md)
* [Acessibilidade](accessibility.md)
* [Design de acessibilidade](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [Práticas que devem ser evitadas](practices-to-avoid.md)
