---
Description: Fornece uma lista de verificação para ajudar você a garantir que o seu aplicativo da Plataforma Universal do Windows (UWP) esteja acessível.
title: Lista de verificação de acessibilidade
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
label: Checklist
template: detail.hbs
---

Lista de verificação de acessibilidade
===================================================================================

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Fornece uma lista de verificação para ajudar você a garantir que o seu aplicativo da Plataforma Universal do Windows (UWP) está acessível.

Aqui nós fornecemos uma lista de verificação que você pode usar para garantir que seu aplicativo é acessível.

1.  Defina o nome acessível (obrigatório) e a descrição (opcional) dos elementos de interface de usuário interativa e do conteúdo em seu aplicativo.

    Um nome acessível é uma cadeia de caracteres de texto descritiva e curta que um leitor de usa para anunciar um elemento de interface do usuário. Alguns elementos de interface do usuário como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) and [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) promovem o conteúdo de texto como o nome acessível padrão; consulte [Basic accessibility information](basic-accessibility-information.md#name_from_inner_text).

    Você deve definir o nome acessível de forma explicita para imagens ou outros controles que não promovem o conteúdo do texto interno como um nome acessível implícito. Você deve usar rótulos para elementos de formulário para que o texto do rótulo possa ser usado como um destino [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) no modelo de Automação da Interface do Usuário da Microsoft para correlacionar rótulos e entradas. Se você deseja fornecer mais diretrizes de interface do usuário para os usuários além das que são geralmente incluídas no nome acessível, dicas e descrições acessíveis ajudam os usuários a entender a interface do usuário.

    Para saber mais, consulte [Nome acessível](basic-accessibility-information.md#accessible_name) e [Descrição acessível.](basic-accessibility-information.md).

2.  Implementar acessibilidade do teclado


    -   Test the default tab index order for a UI. Adjust the tab index order if necessary, which may require enabling or disabling certain controls, or changing the default values of [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) on some of the UI elements.
    -   Use controls that support arrow-key navigation for composite elements. For default controls, the arrow-key navigation is typically already implemented.
    -   Use controls that support keyboard activation. For default controls, particularly those that support the UI Automation [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) pattern, keyboard activation is typically available; check the documentation for that control.
    -   Set access keys or implement accelerator keys for specific parts of the UI that support interaction.
    -   For any custom controls that you use in your UI, verify that you have implemented these controls with correct [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) support for activation, and defined overrides for key handling as needed to support activation, traversal and access or accelerator keys.

    For more info, see [Keyboard interactions](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Verifique sua interface do usuário para garantir que o contraste do texto esteja adequado, que os elementos renderizem corretamente nos temas em alto contraste e que as cores estejam sendo usadas corretamente.

    -   Use as opções de exibição do sistema que ajustam o valor de pontos por polegada (dpi) da exibição, e garanta que a interface de usuário de seu aplicativo seja dimensionada corretamente quando o valor de dpi mudar. (Alguns usuários alteram os valores de dpi como uma opção de acessibilidade; isso está disponível em **Facilidade de Acesso**).
    -   Use uma ferramenta de análise de cor para verificar se a taxa de contraste visual do texto é pelo menos 4.5:1.
    -   Mude para um tema de alto contraste e veja se é possível ler e usar a interface do usuário de seu aplicativo.
    -   A interface do usuário não deve usar as cores como única forma de transmitir informações.

    Para obter mais informações, consulte [Temas de alto contraste](high-contrast-themes.md) e [Requisitos de texto acessível](accessible-text-requirements.md).

4.  Execute ferramentas de acessibilidade, resolva problemas relatados e verifique a experiência de leitura da tela.

    Use ferramentas como o [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) para verificar o acesso programático, execute ferramentas diagnósticas como o [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) para descobrir erros comuns e verifique a experiência de leitura da tela com o Narrador.

    Para obter mais informações, consulte [Testes de acessibilidade](accessibility-testing.md).

5.  Verifique se as configurações do manifesto do aplicativo seguem as diretrizes de acessibilidade.

6.  Declare seu aplicativo como acessível na Windows Store.

    Se você implementou o suporte de acessibilidade de linha base, declarar o seu aplicativo como acessível na Windows Store pode ajudá-lo a chegar a mais clientes e obter boas classificações adicionais.

    Para obter mais informações, consulte [Accessibility in the Store](accessibility-in-the-store.md).

<span id="related_topics"></span>Tópicos relacionados
-----------------------------------------------

* [Acessibilidade](accessibility.md)
* [Design de acessibilidade](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Práticas que devem ser evitadas](practices-to-avoid.md)
 

 





<!--HONumber=Mar16_HO3-->


