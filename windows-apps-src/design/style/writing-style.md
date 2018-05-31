---
author: QuinnRadich
title: Estilo de escrita
description: O uso da voz e do tom corretos é essencial para que o texto do seu aplicativo seja uma parte natural do design.
keywords: UWP, Windows 10, texto, escrita, voz, tom, design, interface do usuário, experiência do usuário
ms.author: quradic
ms.date: 1/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d38f22f896e31a925f07c5d6dffd357c211b3336
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2018
ms.locfileid: "1573233"
---
# <a name="writing-style"></a>Estilo de escrita

A maneira como você formula uma mensagem de erro, como você escreve a documentação de ajuda e até mesmo o texto que você escolhe para um botão têm um grande impacto sobre a usabilidade do seu aplicativo. 

O estilo de escrita pode fazer uma grande diferença entre uma experiência de usuário terrível...

![Tela da morte azul do Windows 7](images/writing/bluescreen_old.png)

... e uma melhor.

![Tela da morte azul do Windows 10](images/writing/bluescreen_new.png)


## <a name="writing-with-a-natural-voice-and-tone"></a>Escrever com uma voz e tom natural

As pesquisas mostram que os usuários respondem melhor a um estilo de escrita amigável, útil e conciso. 

## <a name="be-friendly"></a>Seja amigável

Acima de tudo, você não quer assustar o usuário. Seja informal, seja casual e não use termos que eles não entendem. Mesmo quando coisas não funcionam, não culpe o usuário por todos os problemas. O aplicativo deve assumir a responsabilidade e oferecer orientação que coloca as ações do usuário em primeiro lugar.

![Antes: ocorreu um erro e não foi possível carregar sua imagem. Se as tentativas subsequentes também falharem, talvez seja necessário reiniciar o aplicativo. Depois: não foi possível enviar sua imagem devido a um erro. Tente novamente. Se ainda não funcionar, tente reiniciar o aplicativo.](images/writing/friendly_example.png)

## <a name="be-helpful"></a>Ofereça ajuda

Sempre concentre-se em explicar o que está acontecendo e fornecer as informações relevantes que o usuário precisa, sem sobrecarregá-los com informações desnecessárias.

![Antes: não é possível conectar à rede. Depois: o dispositivo está conectado a uma rede, mas ela não está conectada à Internet. Tente reiniciar seu roteador ou modem. Se estiver usando um hotspot público, verifique se você está conectado.](images/writing/helpful_example.png)

## <a name="be-clear-and-concise"></a>Seja clara e conciso

Na maioria das vezes, o texto não é o foco de um aplicativo. Ele existe para orientar os usuários, para ensiná-los sobre o que está acontecendo e o que eles devem fazer em seguida. Não perca o foco ao escrever o texto em seu aplicativo. Use uma linguagem conhecida pelo público-alvo. Em geral, isso significa escolher palavras comuns e conversacionais, mas os aplicativos para usos especializados terão seus próprios padrões. Os usuários nunca devem se perguntar o que o texto significa, portanto, favoreça a simplicidade se você não tiver certeza de qual tom usar.

![Antes: Ocorreu um erro e não conseguimos salvar seu arquivo. O local pode não estar acessível para este dispositivo ou talvez não haja espaço suficiente. Depois: Não foi possível salvar seu arquivo neste local. Tente novamente com outro local.](images/writing/concise_example.png)

## <a name="writing-tips"></a>Dicas de escrita

Existem várias maneiras de implementar esses princípios gerais no espaço limitado que o aplicativo oferece. Estas são algumas estratégias que achamos úteis para colocar esses princípios em ação:

### <a name="lead-with-whats-important"></a>Comece com o mais importante

Os usuários devem ser capazes de ler e entender o texto rapidamente. Não preencha com apresentações desnecessárias. Dê mais espaço aos pontos-chave e sempre apresente a ideia principal antes de adicionar mais elementos.

**Ideal:** selecione "filtros" para adicionar efeitos à imagem.

**Inadequado:** se você quiser adicionar efeitos visuais ou fazer alterações na imagem, selecione "filtros".

### <a name="emphasize-action"></a>Enfatizar a ação

Os aplicativos são definidos por ações. Os usuários agem enquanto usam o aplicativo e este age respondendo aos usuários. Verifique se o texto usa a *voz ativa* no aplicativo inteiro. Na descrição, as funções e os usuários devem ser descritos com um papel ativo na execução das ações e não o contrário.

**Ideal:** reinicie o aplicativo para ver as alterações.

**Inadequado:** as alterações serão aplicadas quando o aplicativo for reiniciado.

### <a name="short-and-sweet"></a>Curto e direto

Os usuários leem o texto e frequentemente ignoram blocos maiores de palavras inteiramente. Não sacrifique informações necessárias e a apresentação, mas não use mais palavras do que o necessário. Às vezes, isso significa depender de frases ou fragmentos menores. Outras vezes, isso significa ser mais seletivo com as palavras e a estrutura das frases.

**Ideal:** não foi possível carregar a foto. Se ocorrer novamente, tente reiniciar o aplicativo. Mas não se preocupe, sua foto estará esperando por você.

**Inadequado:** Ocorreu um erro e não conseguimos carregar a imagem. Tente novamente e, se você encontrar esse problema novamente, talvez seja necessário reiniciar o aplicativo. Mas não se preocupe, salvamos o trabalho localmente e ele estará esperando por você quando voltar.

## <a name="style-conventions"></a>Convenções de estilo

Se você não se considera um escritor, pode ser assustador tentar implementar esses princípios e recomendações. Mas não se preocupe: uma linguagem simples e direta é uma ótima maneira de fornecer uma boa experiência do usuário. Se você ainda não tiver certeza sobre como estruturar suas palavras, veja algumas diretrizes úteis.

### <a name="addressing-the-user"></a>Abordar o usuário

Fale diretamente com o usuário.

* Sempre se dirija ao usuário como "você".

* Use "nós" para fazer referência a sua própria perspectiva. Isso é reconfortante e ajuda o usuário a sentir-se como parte da experiência.

* Não use "eu" ou "meu" para se referir à perspectiva do aplicativo, mesmo se você for o único a criá-lo.

> Não foi possível salvar seu arquivo nesse local.

### <a name="abbreviations"></a>Abreviações

As abreviações podem ser úteis quando você precisa fazer referência a produtos, locais ou conceitos técnicos várias vezes no aplicativo. Eles podem economizar espaço e parecem mais naturais, desde que o usuário consiga entendê-los.

* Não pressuponha que os usuários já estejam familiarizados com as abreviações, mesmo se você acha que elas são comuns.

* Sempre explique o significado na nova abreviação na primeira vez que o usuário se deparar com ela.

* Não use abreviações muito semelhantes entre si.

> As orientações de design da Plataforma Universal do Windows (UWP) são um recurso que ajuda você a projetar e criar aplicativos visualmente belos e refinados. Com os recursos de design que estão incluídos em cada aplicativo UWP, você pode criar interfaces do usuário (UI) que sejam dimensionadas em uma ampla variedade de dispositivos.

### <a name="contractions"></a>Contrações

As pessoas estão acostumadas com contrações e esperam vê-las. Ao evitar o uso delas, o aplicativo pode parecer muito formal ou até mesmo prolixo.

* Use contrações quando soarem naturais no texto.

* Não use contrações não naturais apenas para economizar espaço ou quando tornam suas palavras menos conversacionais.

> Quando estiver satisfeito com sua imagem, pressione "Salvar" para adicioná-la à galeria. Lá, você poderá compartilhá-la com amigos.

### <a name="periods"></a>Períodos

Encerrar o texto com um ponto implica em que esse texto é uma frase completa. Use um ponto para blocos maiores de texto e evite-os em texto menor do que uma frase completa.

* Use pontos para encerrar frases completas em dicas de ferramentas, mensagens de erro e caixas de diálogo.

* Não encerre o texto de botões, botões de opção, rótulos ou caixas de seleção com um ponto.

> **Você não está conectado**
> * Verifique se os cabos de rede estão conectados.
> * Verifique se você não está no modo avião.
> * Veja se o switch sem fio está ligado.
> * Reinicie o roteador.

### <a name="capitalization"></a>Uso de maiúsculas

Letras maiúsculas são importantes, mas são fáceis de usar em excesso.

* Coloque os substantivos em letras maiúsculas.

* Coloque em maiúsculas o início de cada cadeia de caracteres de texto em seu aplicativo: o início de cada frase, o rótulo e o título.

> **Qual parte causa mais problemas?**
> * Esqueci minha senha
> * Ele não aceita minha senha
> * Alguém pode estar usando minha conta

## <a name="error-messages"></a>Mensagens de erro

Quando algo ocorre de errado em seu aplicativo, os usuários prestam atenção. Como os usuários podem se sentir confusos ou frustrados quando encontram uma mensagem de erro, elas são uma área onde um bom tom e uma boa voz podem ter um impacto significativo.

Mais do que qualquer outra coisa, é importante que sua mensagem de erro não culpe o usuário. Mas também é importante não sobrecarregá-los com informações que não entendem. Na maioria das vezes, um usuário que encontra um erro apenas deseja voltar às suas tarefas da forma mais rápida e fácil possível. Portanto, qualquer mensagem de erro que você escrever deve:

* **Ser amigável** ao assumir a responsabilidade pelo ocorrido, evitando termos desconhecidos e jargão técnico.

* **Ser útil** informando ao usuário da melhor forma sobre o que saiu errado, informando-os sobre o que acontecerá em seguida e fornecendo uma solução realista.

* **Ser claro e conciso** eliminando informações em excesso.

![Exemplo de uma mensagem de erro ideal](images/writing/connection_error.png)

## <a name="buttons"></a>Botões

O texto nos botões deve ser conciso o suficiente para que os usuários possam ler tudo em uma olhada e de forma clara o suficiente para que a função do botão seja imediatamente óbvia. O texto em um botão nunca deve ter mais do que algumas palavras curtas, e muitos devem ser menores do que isso.

Ao escrever o texto para os botões, lembre-se de que cada um representa uma ação. Certifique-se de usar a *voz ativa* no texto do botão, de usar palavras que representam ações em vez de reações.

![Exemplo de um botão ideal](images/writing/install_button.png)

## <a name="dialogs"></a>Caixas de diálogo

Muitos dos mesmos conselhos para escrever mensagens de erro também se aplicam à criação do texto para todas as caixas de diálogo do aplicativo. Embora as caixas de diálogo sejam esperadas pelo usuário, elas ainda interrompem o fluxo normal do aplicativo e devem ser úteis e concisas para que o usuário possa voltar ao que estava fazendo.

Mas o mais importante é a "chamada e a resposta" entre o título de uma caixa de diálogo e seus botões. Certifique-se de que os botões oferecem respostas claras para a pergunta apresentada pelo título e que seu formato seja consistente com o aplicativo.

![Exemplo de uma caixa de diálogo ideal](images/writing/password_dialog.png)

## <a name="spoken-experiences"></a>Experiências faladas

Os mesmos princípios gerais e as recomendações se aplicam ao escrever texto para experiências faladas, como a Cortana. Nesses recursos, os princípios de escrita ideal são ainda mais importantes, pois você não pode fornecer aos usuários outros elementos de design visual para complementar as palavras faladas.

* **Seja amigável** ao interagir com o usuário usando um tom de conversa. Mais do que em qualquer outra área, é essencial que uma experiência falada seja calorosa e acessível, sendo algo que os usuários não tenham medo ao conversar.

* **Seja útil** ao fornecer sugestões alternativas quando o usuário solicita o impossível. Assim como em uma mensagem de erro, se algo deu errado e seu aplicativo não for capaz de atender à solicitação, ele deverá oferecer ao usuário uma alternativa realista para perguntar.

* **Ser claro e conciso** mantendo sua linguagem simples. As experiências faladas não são adequadas para frases longas ou palavras complexas.

## <a name="accessibility-and-localization"></a>Acessibilidade e localização

O aplicativo pode atingir um público mais amplo se ele for escrito pensando na localização e na acessibilidade. Isso é algo que não pode ser feito somente por meio de texto, embora o uso de linguagem simples e amigável seja um ótimo começo. Para obter mais informações, consulte nossa [visão geral de acessibilidade](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) e as [diretrizes de localização](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Seja útil e amigável** ao considerar diferentes experiências. Evite frases que podem não fazer sentido para um público internacional e não use palavras que fazem suposições sobre o que o usuário pode ou não fazer.

* **Seja claro e conciso** evitando palavras incomuns e especializadas quando não forem necessárias. Quanto mais simples for o texto, mais fácil será de localizar.


## <a name="techniques-for-non-writers"></a>Técnicas para autores sem experiência

Você não precisa ser um escritor profissional e experiente para oferecer aos usuários uma boa experiência. Escolha palavras que pareçam confortáveis para você, pois também serão confortáveis para o usuário. Mas, às vezes, isso não é tão fácil quanto parece. Se você ficar sem saída, essas técnicas podem ajudar. 

* Imagine que você está falando para um amigo sobre seu aplicativo. Como você explicaria o aplicativo para eles? Como você faria para explicar sobre os recursos ou dar instruções? Melhor ainda, explique para uma pessoa que ainda não tenha usado o aplicativo. 

* Pense como você descreveria um aplicativo completamente diferente. Por exemplo, se você estiver criando um jogo, pense no que você diria ou escreveria para descrever um aplicativo financeiro ou de notícias. O contraste entre o idioma e a estrutura que você usa pode oferecer mais ideias sobre as palavras adequadas para o que você realmente estiver escrevendo.

* Veja aplicativos semelhantes para obter inspiração. 

A busca pelas palavras adequadas é um problema que muitas pessoas enfrentam, portanto, não sinta-se mal caso não use uma ideia que não parece natural.