---
Description: Procedimentos de teste a serem seguidos para garantir que seu aplicativo do Windows esteja acessível.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: Testes de acessibilidade
label: Accessibility testing
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1144f6edfab73069a8645e464a2aa8de3e5e2ed6
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854752"
---
# <a name="accessibility-testing"></a>Testes de acessibilidade  

Procedimentos de teste a serem seguidos para garantir que seu aplicativo do Windows esteja acessível.

## <a name="run-accessibility-testing-tools"></a>Executar as ferramentas de teste de acessibilidade

O Software Development Kit do Windows (SDK do Windows) inclui várias ferramentas de teste, como [**AccScope**](/windows/desktop/WinAuto/accscope), [**Inspect**](/windows/desktop/WinAuto/inspect-objects) e [**UI Accessibility Checker**](/windows/desktop/WinAuto/ui-accessibility-checker). Essas ferramentas podem ajudar você a identificar a acessibilidade do seu aplicativo. Lembre-se de verificar todos os cenários e elementos da interface do usuário do aplicativo.

Você pode iniciar as ferramentas de teste de acessibilidade a partir de um prompt de comando do Microsoft Visual Studio ou de uma pasta de ferramentas do SDK do Windows (o subdiretório bin de onde o SDK do Windows está instalado na sua máquina de desenvolvimento).
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
### <a name="accscope"></a>**AccScope**  

A ferramenta [**AccScope**](/windows/desktop/WinAuto/accscope) permite que os desenvolvedores e testadores avaliem a acessibilidade de seus aplicativos durante o desenvolvimento e o design do aplicativo, potencialmente em fases de protótipos anteriores, em vez das fases de teste posteriores do ciclo de desenvolvimento de um aplicativo. Isso é requisitado, especialmente, para testar cenários de acessibilidade do Narrador com o seu aplicativo.

### <a name="inspect"></a>**Inspecionar**  

[**Inspecionar**](/windows/desktop/WinAuto/inspect-objects) permite selecionar qualquer elemento de interface do usuário e exibir seus dados de acessibilidade. Você pode visualizar as propriedades e padrões de controle Automação da Interface do Usuário da Microsoft e testar a estrutura navegacional dos elementos de automação na árvore de automação da IU. Use **inspecionar** ao desenvolver a interface do usuário para verificar como os atributos de acessibilidade são expostos na automação da interface do usuário. Em alguns casos, os atributos vêm do suporte à Automação da Interface do Usuário que já está implementado para os controles XAML padrão. Em outros casos, os atributos são provenientes de valores específicos que você definiu em sua marcação XAML, como propriedades anexadas [**AutomationProperties**](/uwp/api/windows.ui.xaml.automation.automationproperties) .

A imagem a seguir mostra a ferramenta [**Inspect**](/windows/desktop/WinAuto/inspect-objects) consultando as propriedades de Automação da Interface do Usuário no elemento do menu **Edit** no Bloco de Notas.

![Captura de tela da ferramenta Inspect.](./images/inspect.png)

### <a name="ui-accessibility-checker"></a>UI Accessibility Checker

O **UI Accessibility Checker (AccChecker)** ajuda você a descobrir problemas de acessibilidade durante o tempo de execução. Quando a sua interface do usuário estiver completa e funcional, use o **AccChecker** para testar diferentes situações, verificar a exatidão da acessibilidade do tempo de execução e descobrir problemas no tempo de execução. Você pode executar o **AccChecker** em interface do usuário ou comandar o modo de linha. Para executar a ferramenta do modo de interface do usuário, abra o diretório do **AccChecker** no diretório bin do SDK do Windows, execute acccheckui.exe e clique no menu **Ajuda**.

### <a name="ui-automation-verify"></a>Verificação da Automação da Interface do Usuário

O **UI Automation Verify (UIA Verify)** é uma estrutura automatizada de testes e verificação para implementações da Automação da Interface do Usuário. O **UIA Verify** pode integrar-se no código de teste e conduzir um teste automático e regular ou e verificações específicas de situações de Automação da Interface do Usuário. Para executar o **UIA Verify**, execute VisualUIAVerifyNative.exe do subdiretório UIAVerify.

### <a name="accessible-event-watcher"></a>Accessible Event Watcher

[              O **Accessible Event Watcher (AccEvent)**](/windows/desktop/WinAuto/accessible-event-watcher) testa se os elementos de interface do usuário de um aplicativo disparam eventos adequados da Automação da Interface do Usuário e do Microsoft Active Accessibility quando ocorrem mudanças na interface do usuário. As alterações na interface do usuário podem ocorrer quando o foco muda ou quando um elemento de interface do usuário é chamado, selecionado ou sofre uma mudança de estado ou propriedade.

> [!NOTE]
> A maioria das ferramentas de teste de acessibilidade mencionadas na documentação é executada em um computador, não em um telefone. Você pode executar algumas das ferramentas enquanto desenvolve e utiliza um emulador, mas a maioria dessas ferramentas não pode expor a árvore de Automação da IU dentro do emulador.

## <a name="test-keyboard-accessibility"></a>Testar a acessibilidade do teclado

A melhor maneira de testar a acessibilidade do seu teclado é desconectar o mouse ou usar o Teclado Virtual se você estiver usando um dispositivo tablet. Teste a navegação da acessibilidade de teclado usando a tecla _Tab_. Você deverá ser capaz de alternar entre todos os elementos interativos da interface do usuário usando a tecla _Tab_. Para elementos compostos da interface do usuário, verifique se é possível navegar entre partes de elementos usando as teclas de seta. Por exemplo, você deve ser capaz de navegar nas listas de itens com as teclas do teclado. Finalmente, certifique-se de que é possível invocar todos os elementos interativos da interface do usuário com o teclado assim que esses elementos obtêm foco, geralmente usando Enter ou a tecla de espaço.

## <a name="verify-the-contrast-ratio-of-visible-text"></a>Verificar o contraste do texto visível

Use as ferramentas de contraste de cores para verificar se a taxa de contraste de texto visível é aceitável. As exceções incluem elementos da interface do usuário inativos e logotipos ou texto decorativo que não transmita informações e possa ser rearranjado sem alterar o significado. Consulte [Requisitos de texto acessível](accessible-text-requirements.md) para obter mais informações sobre a taxa de contraste e as exceções. Consulte [Técnicas para WCAG 2.0 G18 (seção Recursos)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) sobre as ferramentas que podem testar taxas de contraste.

> [!NOTE]
> Algumas das ferramentas listadas pelas Técnicas para WCAG 2.0 G18 não podem ser usadas de forma interativa com um aplicativo UWP. Talvez você precise inserir valores de cor da tela de fundo e de primeiro plano manualmente na ferramenta, fazer capturas de tela da interface do usuário do aplicativo e depois executar a ferramenta de taxa de contraste na imagem da captura de tela. Ou então executar a ferramenta enquanto você abre arquivos de bitmap de origem em um programa de edição de imagens, não enquanto a imagem é carregada pelo aplicativo.

## <a name="verify-your-app-in-high-contrast"></a>Verificar se o aplicativo está em alto contraste

Use o aplicativo com um tema de alto contraste ativo para verificar se todos os elementos da interface do usuário são exibidos corretamente. Todo o texto deve ser legível e todas as imagens devem ser nítidas. Ajuste os recursos de dicionário XAML ou os modelos de controle para corrigir quaisquer problemas de tema provenientes dos controles. Nos casos em que os problemas de alto contraste proeminentes não são provenientes de temas ou controles (como a partir de arquivos de imagem), forneça versões separadas para serem usadas quando um tema de alto contraste está ativo.

## <a name="verify-your-app-with-display-settings"></a>Verificar o aplicativo com configurações de exibição  

Use as opções de exibição do sistema que ajustam o valor de pontos por polegada (dpi) da exibição, e garanta que a interface de usuário de seu aplicativo seja dimensionada corretamente quando o valor de dpi mudar. (Alguns usuários alteram os valores de dpi como uma opção de acessibilidade, isso está disponível em **Facilidade de Acesso**, bem como em propriedades de vídeo). Se você encontrar problemas, siga as [Diretrizes de experiência do usuário para layout e dimensionamento](https://developer.microsoft.com/windows/apps/design) e forneça recursos adicionais para diferentes fatores dimensionamento.

## <a name="verify-main-app-scenarios-by-using-narrator"></a>Verifique os cenários do aplicativo principal usando o Narrator.

Use o Narrador para testar a experiência de leitura da tela do seu aplicativo.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**Use estas etapas para testar o seu aplicativo usando o Narrador com um mouse e teclado:**

1. Inicie o Narrador pressionando a tecla de logotipo do _Windows + Ctrl + Enter_. Em versões anteriores à versão 1607 do Windows 10, use _Tecla de logotipo do Windows + Enter_ para iniciar o Narrador.
2. Navegue pelo aplicativo com o teclado usando a tecla _Tab_, as teclas direcionais e _Caps Lock + teclas direcionais_.
3. Enquanto navega no seu aplicativo, ouça o Narrador lendo os elementos da sua interface do usuário e verifique o seguinte:
    - Para cada controle, verifique se o Narrador lê todo o conteúdo visível. Verifique também se o Narrador lê o nome de cada controle, seu estado aplicável (marcado, selecionado e assim por diante) e o tipo do controle (botão, caixa de seleção, item de lista e assim por diante).
    - Se o elemento for interativo, verifique se é possível usar o Narrador para invocar a sua ação pressionando a tecla _Caps Lock + Enter_.
    - Para cada tabela, garanta que o Narrador leia corretamente o nome, a descrição (se disponível) e os cabeçalhos de linha e coluna.
4. Pressione _Caps Lock + Shift + Enter_ para pesquisar o aplicativo e verificar se todos os controles aparecem na lista de pesquisa e se os nomes desses controles estão localizados e são legíveis.
5. Desligue seu monitor e tente realizar os cenários do aplicativo principal usando apenas o teclado e o Narrador. Para ver a lista completa de comandos e atalhos do Narrador, pressione _Caps Lock + F1_.

A partir da versão 1607 do Windows 10, apresentamos um novo modo de desenvolvedor no Narrador. Ative o modo de desenvolvedor quando o Narrator já estiver em execução pressionando _Control + Caps Lock + F12_. Quando o modo de desenvolvedor estiver habilitado, a tela será mascarada e realçará apenas os objetos acessíveis e o texto associado que é exposto por meio de programação para o Narrador. Isso proporciona a você uma boa representação visual das informações que são expostas para o Narrador.

**Use estas etapas para testar seu aplicativo usando o modo de toque do Narrador:**

> [!NOTE]
> O Narrador entra automaticamente no modo de toque em dispositivos que oferecem suporte para mais de 4 contatos. O Narrador não oferece suporte para cenários com vários monitores ou digitalizadores de toque múltiplo na tela principal.

1. Familiarize-se com a interface do usuário e explore o layout.
    - **Navegue pela interface do usuário usando gestos de passar o dedo.** Deslize o dedo para a esquerda ou direita para mover entre itens e para cima ou para baixo para mudar a categoria de itens navegados. As categorias incluem todos os itens, links, tabelas, cabeçalhos e assim por diante. A navegação com gestos simples de passar o dedo é semelhante à navegação com _Caps Lock + Seta_.
    - **Use gestos de tabulação para navegar por elementos focalizáveis.** Deslizar três dedos para a direita ou esquerda equivale a navegar com _Tab_ e _Shift + Tab_ em um teclado.
    - **Investigue espacialmente a interface do usuário com um único dedo.** Arraste um único dedo para cima e para baixo, ou para a esquerda e para a direita, para que o Narrador leia os itens sob seu dedo. É possível usar o mouse como alternativa, pois ele usa a mesma lógica de teste por pressionamento que o gesto de arrastar um único dedo.
    - **Leia a janela inteira e todos os seus conteúdos deslizando três dedos para cima**. Isso equivale a usar _Caps Lock + W_.

    Se houver um elemento importante da interface do usuário que você não consiga acessar, talvez exista um problema de acessibilidade.

2. Interaja com um controle para testar suas ações principais e secundárias e seu comportamento de rolagem.

    Ações principais incluem ativar um botão, inserir um sinal de intercalação de texto e definir o foco no controle. Ações secundárias incluem selecionar um item de lista ou expandir um botão que oferece várias opções.

    - Para testar uma ação principal: toque duas vezes ou pressione com um dedo e toque com o outro.
    - Para testar uma ação secundária: toque três vezes ou pressione com um dedo e toque duas vezes com o outro.
    - Para testar o comportamento de rolagem: use gestões de passar dois dedos para rolar na direção desejada.

    Alguns controles fornecem ações adicionais. Para exibir a lista completa, basta um único toque com quatro dedos.

    Se um controle responder ao mouse ou teclado, mas não responder à interação touch principal, ele pode precisar implementar padrões de controle [UI Automation](/windows/desktop/WinAuto/entry-uiauto-win32) adicionais.

Você também deve considerar o uso da ferramenta [**AccScope**](/windows/desktop/WinAuto/accscope) para testar os cenários de acessibilidade do Narrador com o seu aplicativo. O [**tópico da ferramenta AccScope**](/windows/desktop/WinAuto/accscope) descreve como configurar **AccScope** para testar cenários do Narrador.

## <a name="examine-the-ui-automation-representation-for-your-app"></a>Examine a representação de Automação da Interface do Usuário de seu aplicativo

Várias das ferramentas de teste de Automação da Interface do Usuário mencionadas antes fornecem uma forma de visualizar seu aplicativo não considerando a aparência dele. Em vez disso, elas representam o aplicativo como uma estrutura de elementos de automação da IU. É assim que os clientes de Automação da Interface do Usuário, principalmente tecnologias adaptativas, interagirão com o seu aplicativo em cenários de acessibilidade.

A ferramenta [**AccScope**](/windows/desktop/WinAuto/accscope) fornece uma exibição particularmente interessante do seu aplicativo porque é possível ver os elementos de automação da IU como representação visual ou como lista. Se você usar a visualização, será possível analisar as peças de maneira que poderá correlacionar com a aparência visual da interface de usuário do aplicativo. Você mesmo pode testar a acessibilidade de seus primeiros protótipos da interface do usuário antes de atribuir toda a lógica à interface do usuário, certificando-se de que a interação visual e a navegação de acessibilidade do cenário de seu aplicativo está em equilíbrio.

Um aspecto que você pode testar é se há elementos aparecendo na exibição do elemento de Automação da IU que você não quer que apareçam lá. Se você encontrar elementos que queria omitir da exibição, ou, de forma contrária, se houver elementos faltando, você pode usar a propriedade anexada XAML [**AutomationProperties.AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty) para ajustar a forma como os controles XAML aparecem nas exibições de acessibilidade. Depois de ter verificado as exibições de acessibilidade básicas, é uma boa oportunidade de verificar novamente as sequências de guia ou navegação espacial habilitadas pelas teclas de setas para garantir que os usuários possam chegar a todas as partes que interativas e expostas na exibição de controle.

## <a name="related-topics"></a>Tópicos relacionados

- [Acessibilidade](accessibility.md)
- [Práticas que devem ser evitadas](practices-to-avoid.md)
- [Automação da Interface de Usuário](/windows/desktop/WinAuto/entry-uiauto-win32)
- [Acessibilidade no Windows](https://www.microsoft.com/accessibility/)
- [Introdução ao Narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
