---
title: Estilo de escrita
description: Usar a voz e do tom certos é essencial para que o texto do seu aplicativo pareça uma parte natural do design.
keywords: UWP, Windows 10, texto, escrita, voz, tom, design, interface do usuário, experiência do usuário
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: aaaf13c455d3d05d5ccfac6b2bd61418f3e8e5bb
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63820519"
---
# <a name="writing-style"></a>Estilo de escrita

![imagem de cabeçalho](images/header-writing-style.gif)

A maneira de formular uma mensagem de erro, de escrever a documentação de ajuda e, até mesmo, o texto escolhido para um botão têm um grande impacto na usabilidade do seu aplicativo. O estilo de escrita pode fazer uma grande diferença entre uma experiência de usuário terrível e uma excelente.

## <a name="voice-and-tone-principles"></a>Princípios de voz e tom

Pesquisas indicam que as pessoas respondem melhor a um estilo de escrita amigável, útil e conciso. Como parte dessa pesquisa, a Microsoft desenvolveu três princípios de voz e tom, que aplicamos em todo o nosso conteúdo e são parte integral do Design Fluente.

### <a name="be-warm-and-relaxed"></a>Seja amigável e flexível

Acima de tudo, você não quer assustar o usuário. Seja informal, seja casual e não use termos que eles não entendem. Mesmo quando as coisas não funcionam, não culpe o usuário por nenhum problema. Em vez disso, o aplicativo deve levar a culpa e oferecer orientações que coloquem as ações do usuário em primeiro lugar.

### <a name="be-ready-to-lend-a-hand"></a>Esteja preparado para ajudar

Sempre mostre empatia em sua escrita. Concentre-se em explicar o que está acontecendo e fornecer as informações necessárias ao usuário, sem sobrecarregá-los com informações desnecessárias. Se possível, forneça sempre uma solução quando houver um problema.

### <a name="be-crisp-and-clear"></a>Seja claro e preciso

Na maioria das vezes, o texto não é o foco de um aplicativo. Ele existe para orientar as pessoas, para ensiná-las sobre o que está acontecendo e o que elas devem fazer em seguida. Não se esqueça disso ao escrever o texto em seu aplicativo e não assuma que os usuários lerão todas as palavras. Use uma linguagem familiar para seu público-alvo e verifique se é fácil de entender rapidamente.

## <a name="lead-with-whats-important"></a>Comece com o mais importante

Os usuários precisam ler e entender o texto rapidamente. Não enrole com apresentações desnecessárias. Dê mais visibilidade aos pontos principais e sempre apresente a ideia principal antes de adicionar mais elementos.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Select **filters** to add effects to your image.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        If you want to add visual effects or alterations to your image, select **filters.**
    :::column-end:::
:::row-end:::

## <a name="emphasize-action"></a>Enfatize a ação

Os aplicativos são definidos por ações. Os usuários executam essas ações quando usam o aplicativo e este responde com uma ação aos usuários. Verifique se o texto usa a *voz ativa* em todo o aplicativo. Descreva as funções e as pessoas com um papel ativo na execução das ações e não o contrário.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Restart the app to see your changes.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        The changes will be applied when the app is restarted.
    :::column-end:::
:::row-end:::

## <a name="short-and-sweet"></a>Curto e direto

Os usuários leem o texto e, frequentemente, ignoram totalmente os blocos maiores de palavras. Não sacrifique as informações necessárias e a apresentação, mas não use mais palavras do que o necessário. Às vezes, isso significa usar frases ou fragmentos menores. Outras vezes, isso significa ser mais seletivo com as palavras e a estrutura das frases maiores.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't upload the picture. If this happens again, try restarting the app. But don't worry — your picture will be waiting when you come back.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        An error occured, and we weren't able to upload the picture. Please try again, and if you encounter this problem again, you may need to restart the app. But don't worry — we've saved your work locally, and it'll be waiting for you when you come back.
    :::column-end:::
:::row-end:::

## <a name="style-conventions"></a>Convenções de estilo

Se você não se considera um escritor, pode ser assustador tentar implementar esses princípios e recomendações. Mas não se preocupe: uma linguagem simples e direta é uma ótima maneira de fornecer uma boa experiência ao usuário. Se ainda não tiver certeza sobre como estruturar suas palavras, veja algumas diretrizes úteis. E se precisar saber mais, confira o [Guia de estilo da Microsoft](https://docs.microsoft.com/style-guide/welcome/).

### <a name="addressing-the-user"></a>Comunique-se com o usuário

Fale diretamente com o usuário.
* Sempre se dirija ao usuário como "você".
* Use "nós" para fazer referência a sua própria perspectiva. Isso é reconfortante e ajuda o usuário a sentir-se como parte da experiência.
* Não use "eu" ou "meu" para se referir à perspectiva do aplicativo, mesmo se você for o único a criá-lo.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't save your file to that location.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="abbreviations"></a>Abreviações

 As abreviações podem ser úteis quando você precisa se referir a produtos, lugares ou conceitos técnicos várias vezes no aplicativo. Elas podem economizar espaço e parecem mais naturais, desde que o usuário consiga entendê-las.
* Não assuma que os usuários já estão familiarizados com as abreviações, mesmo se você acreditar que elas são comuns.
* Sempre explique o significado da nova abreviação na primeira vez que o usuário se deparar com ela.
* Não use abreviações muito semelhantes entre si.
* Não use abreviações se estiver traduzindo seu aplicativo ou se os usuários falam inglês como um segundo idioma.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        The Universal Windows Platform (UWP) design guidance is a resource to help you design and build beautiful, polished apps. With the design features that are included in every UWP app, you can build user interfaces (UI) that scale across a range of devices.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="contractions"></a>Contrações

As pessoas estão acostumadas com contrações e esperam vê-las. Ao evitar o uso delas, o aplicativo pode parecer muito formal ou até mesmo prolixo.
* Use contrações quando elas soarem naturais no texto.
* Não use contrações não naturais apenas para economizar espaço ou quando tornam suas palavras menos conversacionais.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        When you're happy with your image, select **save** to add it to your gallery. From there, you'll be able to share it with friends.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="periods"></a>Pontuação

 Terminar o texto com um ponto final implica que esse texto é uma frase completa. Use um ponto final para blocos maiores de texto e evite-os em textos menores do que uma frase completa.
* Use pontos finais para terminar frases completas em dicas de ferramentas, mensagens de erro e caixas de diálogo.
* Não use-os em textos de botões, botões de opção, rótulos ou caixas de seleção.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="capitalization"></a>Uso de maiúsculas

Letras maiúsculas são importantes, mas são fáceis de usar em excesso.
* Coloque nomes próprios em letras maiúsculas.
* Coloque em maiúsculas o início de cada cadeia de caracteres de texto em seu aplicativo: o início de cada frase, o rótulo e o título.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        * I forgot your password.
        * It won't accept password.
        * Someone else might be using my account.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="error-messages"></a>Mensagens de erro

Quando ocorre um erro em seu aplicativo, os usuários prestam atenção. Como os usuários podem se sentir confusos ou frustrados quando encontram uma mensagem de erro, esta é uma área em que um bom tom e uma boa voz podem ter um impacto significativo.

O mais importante é que sua mensagem de erro não culpe o usuário. Também é importante não sobrecarregá-los com informações que não entendem. Na maioria das vezes, um usuário que encontra um erro apenas deseja voltar às suas tarefas da forma mais rápida e fácil possível. Portanto, qualquer mensagem de erro que você escrever precisa:

* **Ser amigável e calma**, usando um tom de conversa e evitando termos e jargão técnico desconhecido.

* **Estar preparada para ajudar**, informando ao usuário da melhor forma sobre o que deu errado, informando-os sobre o que acontecerá em seguida e fornecendo uma solução que eles possam executar.

* **Ser clara e precisa**, eliminando informações em excesso.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end::: 

## <a name="dialogs"></a>Caixas de diálogo

:::row:::
    :::column:::
        Many of the same advice for writing error messages also applies when creating the text for any dialogs in your app. While dialogs are expected by the user, they still interrupt the normal flow of the app, and need to be helpful and concise so the user can get back to what they were doing.

        But most important is the "call and response" between the title of a dialog and its buttons. Make sure that your buttons are clear answers to the question posed by the title, and that their format is consistent across your app.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        1. I forgot my password
        2. It won't accept my password
        3. Someone else might be using my account
    :::column-end:::
:::row-end:::

## <a name="buttons"></a>Botões

:::row:::
    :::column:::
        Text on buttons needs to be concise enough that users can read it all at a glance and clear enough that the button's function is immediately obvious. The longest the text on a button should ever be is a couple short words, and many should be shorter than that.
        When writing the text for buttons, remember that every button represents an action. Be sure to use the *active voice* in button text, to use words that represent actions rather than reactions.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        * Install now
        * Share
    :::column-end:::
:::row-end:::

## <a name="spoken-experiences"></a>Experiências faladas

Os mesmos princípios gerais e recomendações se aplicam ao escrever texto para experiências faladas, como a Cortana. Nesses recursos, os princípios de escrita ideal são ainda mais importantes, pois você não pode fornecer aos usuários outros elementos de design visual para complementar as palavras faladas.

* **Seja amigável e calmo**, envolvendo os usuários com um tom de conversa. Mais do que em qualquer outra área, é essencial que uma experiência falada seja calorosa e acessível, e seja algo com que os usuários não tenham medo de conversar.

* **Esteja preparado para ajudar**, fornecendo sugestões alternativas quando o usuário pedir o impossível. Assim como em uma mensagem de erro, se algo deu errado e seu aplicativo não foi capaz de atender à solicitação, ele deverá oferecer ao usuário uma alternativa realista que ele possa tentar perguntar.

* **Seja claro e preciso**, mantendo sua linguagem simples. As experiências faladas não são adequadas para frases longas ou palavras complexas.

## <a name="accessibility-and-localization"></a>Acessibilidade e tradução

O aplicativo pode atingir um público mais amplo se ele for escrito pensando na tradução e na acessibilidade. Isso é algo que não pode ser feito somente por meio de texto, embora o uso de linguagem simples e amigável seja um ótimo começo. Para saber mais, confira nossa [visão geral de acessibilidade](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) e as [diretrizes de localização](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Esteja preparado para ajudar**, levando em consideração as diferentes experiências. Evite frases que não fazem sentido para um público internacional e não use palavras que façam suposições sobre o que o usuário pode ou não fazer.

* **Seja claro e preciso**, evitando palavras incomuns e especializadas quando não forem necessárias. Quanto mais simples for o texto, mais fácil será de traduzir.


## <a name="techniques-for-non-writers"></a>Técnicas para autores sem experiência

Você não precisa ser um escritor profissional e experiente para oferecer aos usuários uma boa experiência. Escolha palavras que pareçam confortáveis para você, pois também serão confortáveis aos outros usuários. Mas, às vezes, isso não é tão fácil quanto parece. Se você ficar sem saída, essas técnicas podem ajudar. 

* Imagine que você está falando com um amigo sobre seu aplicativo. Como você explicaria o aplicativo para ele? Como você faria para explicar sobre os recursos ou dar instruções? Melhor ainda, explique para uma pessoa que ainda não usou o aplicativo. 

* Pense em como você descreveria um aplicativo completamente diferente. Por exemplo, se você está criando um jogo, pense no que diria ou escreveria para descrever um aplicativo financeiro ou de notícias. O contraste entre o idioma e a estrutura usados pode oferecer mais ideias sobre as palavras adequadas para o que você está realmente escrevendo.

* Veja aplicativos semelhantes para buscar inspiração. 

Encontrar as palavras adequadas é um problema que muitas pessoas enfrentam, portanto, não sinta-se mal se não for fácil encontrar algo que pareça natural.
