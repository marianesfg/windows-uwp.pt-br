---
author: QuinnRadich
title: Estilo de escrita
description: O uso da voz e do tom corretos é essencial para que o texto do seu aplicativo seja uma parte natural do design.
keywords: UWP, Windows 10, texto, escrita, voz, tom, design, interface do usuário, experiência do usuário
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8022b3bb5ca312be259c554f46dc9f432ea3caeb
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6052071"
---
# <a name="writing-style"></a>Estilo de escrita

![imagem de cabeçalho](images/header-writing-style.gif)

A maneira como você formula uma mensagem de erro, como você escreve a documentação de ajuda e até mesmo o texto que você escolhe para um botão têm um grande impacto sobre a usabilidade do seu aplicativo. O estilo de escrita pode fazer uma grande diferença entre uma experiência de usuário terrível e uma excelente.

## <a name="voice-and-tone-principles"></a>Princípios de voz e tom

As pesquisas mostram que as pessoas respondem melhor a um estilo de escrita amigável, útil e conciso. Como parte dessa pesquisa, a Microsoft desenvolveu três princípios de voz e tom que se aplicam a todos o nosso conteúdo e são parte integral do Design fluente.

### <a name="be-warm-and-relaxed"></a>Seja amigável e flexível

Acima de tudo, você não quer assustar o usuário. Seja informal, seja casual e não use termos que eles não entendem. Mesmo quando coisas não funcionam, não culpe o usuário por todos os problemas. O aplicativo deve assumir a responsabilidade e oferecer orientação que coloca as ações do usuário em primeiro lugar.

### <a name="be-ready-to-lend-a-hand"></a>Esteja preparado para ajudar

Sempre mostre empatia em sua escrita. Concentre-se em explicar o que está acontecendo e fornecer as informações que o usuário precisa, sem sobrecarregá-los com informações desnecessárias. Se possível, forneça sempre uma solução quando há um problema.

### <a name="be-crisp-and-clear"></a>Seja claro e preciso

Na maioria das vezes, o texto não é o foco de um aplicativo. Ele existe para orientar as pessoas, para ensiná-las sobre o que está acontecendo e o que elas devem fazer em seguida. Não perca o controle disso ao escrever o texto em seu aplicativo e não considerar que os usuários vão ler cada palavra. Use uma linguagem familiar para seu público-alvo e verifique se é fácil de entender a primeira vista.

## <a name="lead-with-whats-important"></a>Comece com o mais importante

Os usuários devem ser capazes de ler e entender o texto rapidamente. Não preencha com apresentações desnecessárias. Dê mais espaço aos pontos-chave e sempre apresente a ideia principal antes de adicionar mais elementos.

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

## <a name="emphasize-action"></a>Enfatizar a ação

Os aplicativos são definidos por ações. Os usuários agem quando usam o aplicativo e este age respondendo ao usuário. Verifique se o texto usa a *voz ativa* no aplicativo inteiro. Na descrição, as funções e as pessoas devem ser descritas com um papel ativo na execução das ações e não o contrário.

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

Os usuários leem o texto e frequentemente ignoram blocos maiores de palavras inteiramente. Não sacrifique informações necessárias e a apresentação, mas não use mais palavras do que o necessário. Às vezes, isso significa depender de frases ou fragmentos menores. Outras vezes, isso significa ser mais seletivo com as palavras e a estrutura das frases.

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

Se você não se considera um escritor, pode ser assustador tentar implementar esses princípios e recomendações. Mas não se preocupe: uma linguagem simples e direta é uma ótima maneira de fornecer uma boa experiência do usuário. Se você ainda não tiver certeza sobre como estruturar suas palavras, veja algumas diretrizes úteis. Se quiser obter mais informações, confira o [Guia de estilo da Microsoft](https://docs.microsoft.com/style-guide/welcome/).

### <a name="addressing-the-user"></a>Abordar o usuário

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

 As abreviações podem ser úteis quando você precisa fazer referência a produtos, locais ou conceitos técnicos várias vezes no aplicativo. Eles podem economizar espaço e parecem mais naturais, desde que o usuário consiga entendê-los.
* Não pressuponha que os usuários já estejam familiarizados com as abreviações, mesmo se você acha que elas são comuns.
* Sempre explique o significado na nova abreviação na primeira vez que o usuário se deparar com ela.
* Não use abreviações muito semelhantes entre si.
* Não use abreviações se estiver localizando seu aplicativo ou se os usuários falam inglês como um segundo idioma.

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
* Use contrações quando soarem naturais no texto.
* Não use contrações não naturais apenas para economizar espaço ou quando tornam suas palavras menos conversacionais.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        When you're happy with your image, select **save** to add it to your gallery. From there, you'll be able to share it with friends.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="periods"></a>Períodos

 Encerrar o texto com um ponto implica em que esse texto é uma frase completa. Use um ponto para blocos maiores de texto e evite-os em texto menor do que uma frase completa.
* Use pontos para encerrar frases completas em dicas de ferramentas, mensagens de erro e caixas de diálogo.
* Não encerre o texto de botões, botões de opção, rótulos ou caixas de seleção com um ponto.

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
* Coloque os substantivos em letras maiúsculas.
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

Quando algo ocorre de errado em seu aplicativo, os usuários prestam atenção. Como os usuários podem se sentir confusos ou frustrados quando encontram uma mensagem de erro, elas são uma área onde um bom tom e uma boa voz podem ter um impacto significativo.

Mais do que qualquer outra coisa, é importante que sua mensagem de erro não culpe o usuário. Mas também é importante não sobrecarregá-los com informações que não entendem. Na maioria das vezes, um usuário que encontra um erro apenas deseja voltar às suas tarefas da forma mais rápida e fácil possível. Portanto, qualquer mensagem de erro que você escrever deve:

* **Seja amigável e calmo** usando um tom de conversa e evitando termos e jargão técnico desconhecido.

* **Esteja preparado para ajudar** informando ao usuário da melhor forma sobre o que saiu errado, informando-os sobre o que acontecerá em seguida e fornecendo uma solução realista.

* **Seja claro e preciso** eliminando informações em excesso.

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

Os mesmos princípios gerais e as recomendações se aplicam ao escrever texto para experiências faladas, como a Cortana. Nesses recursos, os princípios de escrita ideal são ainda mais importantes, pois você não pode fornecer aos usuários outros elementos de design visual para complementar as palavras faladas.

* **Seja amigável e calmo** ao envolver os usuários com um tom de conversa. Mais do que em qualquer outra área, é essencial que uma experiência falada seja calorosa e acessível, sendo algo que os usuários não tenham medo ao conversar.

* **Esteja preparado para ajudar** ao fornecer sugestões alternativas quando o usuário pedir o impossível. Assim como em uma mensagem de erro, se algo deu errado e seu aplicativo não for capaz de atender à solicitação, ele deverá oferecer ao usuário uma alternativa realista para perguntar.

* **Ser claro e preciso** mantendo sua linguagem simples. As experiências faladas não são adequadas para frases longas ou palavras complexas.

## <a name="accessibility-and-localization"></a>Acessibilidade e localização

O aplicativo pode atingir um público mais amplo se ele for escrito pensando na localização e na acessibilidade. Isso é algo que não pode ser feito somente por meio de texto, embora o uso de linguagem simples e amigável seja um ótimo começo. Para obter mais informações, consulte nossa [visão geral de acessibilidade](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) e as [diretrizes de localização](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Esteja preparado para ajudar** ao considerar diferentes experiências. Evite frases que podem não fazer sentido para um público internacional e não use palavras que fazem suposições sobre o que o usuário pode ou não fazer.

* **Seja claro e preciso** evitando palavras incomuns e especializadas quando não forem necessárias. Quanto mais simples for o texto, mais fácil será de localizar.


## <a name="techniques-for-non-writers"></a>Técnicas para autores sem experiência

Você não precisa ser um escritor profissional e experiente para oferecer aos usuários uma boa experiência. Escolha palavras que pareçam confortáveis para você, pois também serão confortáveis outros usuários. Mas, às vezes, isso não é tão fácil quanto parece. Se você ficar sem saída, essas técnicas podem ajudar. 

* Imagine que você está falando para um amigo sobre seu aplicativo. Como você explicaria o aplicativo para eles? Como você faria para explicar sobre os recursos ou dar instruções? Melhor ainda, explique para uma pessoa que ainda não tenha usado o aplicativo. 

* Pense como você descreveria um aplicativo completamente diferente. Por exemplo, se você estiver criando um jogo, pense no que você diria ou escreveria para descrever um aplicativo financeiro ou de notícias. O contraste entre o idioma e a estrutura que você usa pode oferecer mais ideias sobre as palavras adequadas para o que você realmente estiver escrevendo.

* Veja aplicativos semelhantes para obter inspiração. 

A busca pelas palavras adequadas é um problema que muitas pessoas enfrentam, portanto, não sinta-se mal caso não use uma ideia que não parece natural.