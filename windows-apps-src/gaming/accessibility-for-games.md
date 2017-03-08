---
author: joannaleecy
title: "Tronando os jogos acessíveis"
description: "Aprenda a criar jogos acessíveis. Use o princípio do design de jogo inclusivo para alcançar a acessibilidade do jogo."
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, acessibilidade, jogos
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 74a3e3bf3b4f614a112bedd8a2777f8d641237a9
ms.lasthandoff: 02/07/2017

---
#  <a name="making-games-accessible"></a>Tronando os jogos acessíveis

A acessibilidade pode capacitar cada pessoa e cada organização do planeta a alcançar mais, e isso se aplica a fazer jogos mais acessíveis também. Este artigo foi escrito para desenvolvedores de jogos, mais especificamente designers, produtores e gerentes de jogos. Ele apresenta uma visão geral das diretrizes de acessibilidade jogo derivada de diversas organizações (listadas na seção de referência abaixo) e o princípio do design de jogo inclusivo para a criação de jogos mais acessíveis.

##  <a name="why-make-games-accessible"></a>Por que criar jogos acessíveis?

### <a name="increased-gamer-base"></a>Base maior de jogadores

No nível mais básico, a justificativa comercial da acessibilidade é simples:

Número de usuários que podem jogar o jogo x grandiosidade do jogo = vendas do jogo

Se tiver feito um jogo incrível muito complicado ou intrincado e que apenas algumas pessoas podem jogá-lo, você limitará as vendas. Da mesma forma, se tiver feito um jogo que não possa ser jogado por pessoas com deficiências física, sensorial ou cognitiva, você perderá vendas em potencial. Considerando que, por exemplo, [19% da população apenas nos Estados Unidos têm alguma forma de deficiência](http://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), isso pode ter um grande impacto na receita do título. 

Para mais justificativas comerciais, consulte [Criando videogames acessíveis](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### <a name="better-games"></a>Jogos melhores

A criação de um jogo mais acessível pode acabar criar um jogo melhor. 

Um exemplo é a legenda em jogos. No passado, jogos raramente eram compatíveis com legendas ou legendagem oculta para diálogos de jogos. Hoje em dia, espera-se que os jogos tenham legendas e legendagem oculta. Essa mudança não foi determinada por jogadores com deficiências. Em vez disso, ela foi motivada por jogadores que simplesmente preferiam jogar com legendas porque elas melhoram a experiência dos jogos. Os jogadores ativam legendas e legendagem oculta quando estão jogando com muito ruído de fundo, estão enfrentando dificuldade para ouvir vozes com diversos efeitos sonoros ou sons ambiente sendo executados ao mesmo tempo, ou quando simplesmente precisam manter o volume baixo para evitar incomodar outras pessoas. As legendas e a legendagem oculta não apenas ajudaram jogadores a terem uma experiência de jogos melhor, mas também permitem que pessoas com deficiências auditivas joguem também.

O remapeamento do controlador é outro recurso que lentamente está se tornando um padrão para o setor de jogos por motivos semelhantes. Os jogadores aproveitam a personalização das experiências de jogos. O que a maioria das pessoas não percebe é que a capacidade de remapear botões em um dispositivo de entrada é realmente um recurso de acessibilidade que foi concebido para tornar um jogo jogável para pessoas com diversos tipos de deficiências motoras

Por fim, o processo de concepção usado para tornar o jogo mais acessível geralmente resultará em um jogo melhor porque você projetou uma experiência mais amigável e personalizável para os jogadores aproveitarem.

### <a name="a-social-space"></a>Um espaço social

Jogos são uma forma de entretenimento e podem proporcionar horas de alegria. Para alguns, jogos não são apenas uma forma de entretenimento, e sim uma fuga de uma cama de hospital, de dores crônicas ou de uma ansiedade social debilitante. Os jogadores são transportados para um mundo onde se tornam os personagens principais do videogame. Por meio dos jogos, eles podem criar e participar de um espaço social próprio que proporcione a distração das batalhas do dia a dia que acompanham as deficiências e que ofereça uma oportunidade para se comunicar com pessoas com as quais não poderiam interagir de outra forma.

##  <a name="is-the-game-you-are-making-today-accessible"></a>O jogo que você está criando hoje é acessível?

Se você estiver pensando em deixar o jogo acessível pela primeira vez, aqui estão algumas perguntas para se fazer:

* Você consegue terminar o jogo usando uma única mão? 
* Uma pessoa média pode ser capaz de pegar o jogo e jogar?
* Você consegue efetivamente jogar o jogo em um monitor pequeno ou sentado a uma certa distância da TV?
* Você dá suporte a mais de um tipo de dispositivo de entrada que possa ser usado para reproduzir todo o jogo?
* Você consegue jogar o jogo sem som?
* Você consegue jogar o jogo com o monitor em preto e branco?

Se a maioria das respostas foi não, ou você não as saiba, é hora de dar um passo adiante e colocar acessibilidade no jogo.

## <a name="defining-disability"></a>Definindo deficiência

Deficiência é definida como uma "incompatibilidade entre as necessidades do indivíduo e o serviço, produto ou ambiente oferecidos". ([Vídeo inclusivo](https://www.microsoft.com/design/inclusive), Microsoft.com.) Isso significa que qualquer pessoa pode ter uma deficiência e que essa pode ser uma condição de curto prazo ou situacional. Imagine quais desafios jogadores com essas condições podem ter ao jogar o jogo e pense em como o jogo pode ser mais bem projetado para eles. Veja a seguir algumas deficiências que devem ser levadas em consideração:

### <a name="vision"></a>Visão

*   Condições médicas, a longo prazo, como glaucoma, catarata, daltonismo, miopia e retinopatia diabética
*   Condições de curto prazo, situacionais, como um tamanho de monitor ou tela pequeno, uma tela de baixa resolução ou reflexo de tela por causa de fontes de luz brilhantes em um monitor
        
### <a name="hearing"></a>Audição

* Condições médicas, a longo prazo, como surdez completa ou perda de audição parcial por causa de doenças ou genética
* Condições de curto prazo, situacionais, como ruído de fundo excessivo ou volume restrito para evitar incomodar outras pessoas
        
### <a name="motor"></a>Motora

* Condições médicas, a longo prazo, como mal de Parkinson, esclerose lateral amiotrófica (ELA), artrite e distrofia muscular
* Condições de curto prazo, situacionais, como uma mão machucada, segurando uma bebida ou segurando uma criança em um braço
  
### <a name="cognitive"></a>Cognitiva

* Condições médicas, a longo prazo, como dislexia, epilepsia, transtorno do déficit de atenção com hiperatividade (ADHD), demência e amnésia
* Condições de curto prazo, situacionais, como privação de sono, ou distrações temporárias como sirene de um veículo de emergência passando próximo à residência

### <a name="speech"></a>Fala

* Condições médicas, a longo prazo, como danos à corda vocal, disartria e apraxia
* Condições de curto prazo, situacionais, como trabalho odontológico, ou comer e beber


## <a name="how-to-make-games-more-accessible"></a>Como criar jogos mais acessíveis?

### <a name="design-shift-inclusive-game-design-approach"></a>Mudança de design: abordagem do design de jogo inclusivo

O design inclusivo se concentra na criação de produtos e serviços mais acessíveis para um conjunto mais amplo de consumidores, inclusive pessoas com deficiências.

Para serem bem-sucedidos, os designers de jogos atuais precisam pensar além da criação de jogos divertidos para um público-alvo pequeno e segmentado. Os designers de jogos precisam estar cientes de como as decisões de design afetam a acessibilidade geral do jogo, a jogabilidade do jogo para o público-alvo em potencial geral, inclusive pessoas com deficiências.

Assim, os paradigmas do design de jogo tradicional devem mudar para abranger o conceito do design de jogo inclusivo. O design de jogo inclusivo significa ir além do design de jogo básico de criar diversão para o público-alvo, criar personas adicionais ou modificadas para incluir um conjunto mais amplo de jogadores. 

Essa etapa extra ajuda a identificar lacunas no design original. Identificando lacunas, você pode iterar o conceito do design original e melhorá-lo. Quando você reserva um tempo para ser mais inclusivo no processo de design do jogo, o jogo final se torna mais acessível.

### <a name="empower-gamers-give-gamers-options"></a>Capacitar jogadores: dar opções aos jogadores

Acessibilidade é uma questão de opções. Dê aos jogadores as opções para personalizarem a experiência dos jogos. Se já tiver uma base de fãs grandes, você poderá ter uma parte significativa do público-alvo que não queira que a experiência mude de maneira alguma. Tudo bem. Dê aos jogadores a capacidade de ativar e desativar esses recursos, além de torná-los configuráveis individualmente.

### <a name="innovate-be-creative"></a>Inovar: ser criativo

Há muitas maneiras criativas de melhorar a acessibilidade do jogo. Coloque o chapéu da criatividade e aprenda com outros jogos acessíveis por aí. Se você já tiver um jogo existente, aprenda a identificar recursos dos jogos atuais que possam ser melhorados, mantendo a mecânica básica do jogo e a experiência conforme projetada. Conforme mencionado acima, acessibilidade em jogos é uma questão de dar aos jogadores opções para personalizarem a experiência dos jogos.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Evangelizar: fazer da acessibilidade uma prioridade no estúdio de jogos

Como o desenvolvimento de jogos está sempre seguindo um cronograma apertado, priorizar a acessibilidade ajudará a torná-la um processo mais fácil. Uma maneira é criar desde o início tendo a acessibilidade em mente. Compartilhe o conhecimento sobre acessibilidade com a equipe e compartilhe as justificativas comerciais.

### <a name="review-constantly-evaluate-your-game"></a>Revisar: avaliar constantemente o jogo

Durante o desenvolvimento, você pode introduzir um processo de revisão para garantir que em todas as etapas do caminho você esteja pensando em acessibilidade. Faça uma lista de verificação como a mostrada abaixo para ajudar a equipe a avaliar constantemente se o que você está criando é acessível ou não.

| Lista de verificação                                         | Recursos de acessibilidade                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Cinemática no jogo                                | Tem legendas, testadas para fotossensibilidade                                                                           |
| Arte geral (gráficos 2D e 3D)              | Cores e opções amigáveis a daltônicos, não dependentes inteiramente da cor para identificação, mas que usam formas e padrões também|
| Tela inicial, menu de configurações e outros menus       | Capacidade de ler em voz alta, capacidade de se lembrar das configurações, método de entrada de controle do comando alternativo, tamanho da fonte da interface do usuário ajustável  |
| Jogo                                          | Níveis de dificuldade amplamente ajustáveis, legendas, boas respostas visual e de áudio para o jogador                           |
| Tela HUD                                       | Posição de tela ajustável, tamanho da fonte ajustável, opção amigável para daltônicos                                                  |        
| Entrada de controle                                     | Controles mapeáveis para dispositivo de entrada, suporte ao controlador personalizado, entrada simplificada para jogo permitida                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Testar e iterar: ouvir comentários dos jogadores

Ao organizar sessões de testes, convide testadores com deficiências para as quais o jogo foi projetado e peça para eles jogarem o jogo. Observe como eles jogam e ouça os comentários. Descubra quais alterações precisam ser feitas para melhorar o jogo.

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Anunciar: dizer ao mundo que o jogo é acessível

Os consumidores desejarão saber o jogo pode ser jogado por jogadores com deficiências. Diga claramente a acessibilidade do jogo no site e na embalagem do jogo para garantir que os consumidores saibam o que esperar quando comprarem o jogo. Não se esqueça de deixar o site e todos os canais de vendas do jogo acessíveis também. Mais importante: entre em contato com a comunidade de jogos de acessibilidade e a informe sobre o jogo.

## <a name="game-accessibility-features"></a>Recursos de acessibilidade do jogo

Esta seção descreve alguns recursos que podem tornar o jogo mais acessível. Esses recursos são derivados de diretrizes extraídas das [Diretrizes de acessibilidade de jogo](http://gameaccessibilityguidelines.com/), que representam as descobertas de um grupo colaborativo de estúdios, especialistas e acadêmicos. Para obter mais informações, consulte [Diretrizes de acessibilidade de jogo](http://gameaccessibilityguidelines.com/). 

### <a name="colorblind-friendly-graphics-and-user-interface"></a>Gráficos e interface de usuário amigáveis para daltônicos

A retina do olho tem dois tipos de células sensíveis à luz: os cones para ver onde há luz e os bastonetes para enxergar em condições de pouca luz. Há três tipos de cones (vermelhos, verdes e azuis) que nos permitem ver cores corretamente. O daltonismo ocorre quando um ou mais desses três tipos de cone de luz não está funcionando conforme o esperado. O grau de daltonismo pode variar desde uma percepção de cores quase normal com sensibilidade reduzida à luz vermelha, verde ou azul até uma incapacidade completa de perceber as cores vermelho, verde ou azul. Como é menos comum ter sensibilidade reduzida à luz azul, durante o design para daltônicos, a seleção das cores é voltada para pessoas que não enxergam as cores vermelho ou verde:
 
  + Use combinações de cores que possam ser diferenciadas por pessoas com daltonismo para vermelho/verde:
  
    * Cores que pareçam semelhantes: todos os tons de vermelho e verde, inclusive marrom e laranja
    * Cores que se destacam: azul e amarelo
    
  + Não conte exclusivamente com cores para distinguir objetos do jogo; use formas e padrões também
  
### <a name="closed-captioning-and-subtitles"></a>Legendagem oculta e legendas

Durante o design das legendas ocultas e das legendas do jogo, o objetivo é fornecer legendas legíveis como uma opção de maneira que o jogo também possa ser curtido sem áudio. Deve ser possível ter componentes do jogo como diálogos, áudios e efeitos sonoros exibidos como texto na tela.

Aqui estão algumas diretrizes básicas a serem consideradas durante o design de legendas ocultas e legendas:

*   Selecione uma fonte legível simples.
*   Selecione um tamanho de fonte suficientemente grande pense em ter uma opção de tamanho de fonte ajustável para mais flexibilidade. (O tamanho de fonte ideal depende do tamanho da tela, da distância da tela na exibição etc.)
*   Crie alto contraste entre as cores de fundo e da fonte. (Para obter mais informações, consulte [Informações sobre o índice de contraste](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Exiba sentenças curtas na tela. (Lembre-se não revelar o jogo exibindo o texto antes do evento ocorrer.)
*   Diferencie o que está fazendo o som ou quem está falando. (Exemplo: "Daniel: Olá!")
*   Dê a opção para ativar e desativar legendas ocultas e legendas. (Recurso adicional: capacidade de selecionar quantas informações sobre o som são exibidas com base na importância.)

### <a name="sound-feedback"></a>Retorno sonoro

O som fornece um retorno para o jogador, além de um retorno visual. Um bom design de áudio de jogo pode melhorar a acessibilidade para jogadores com deficiência visual. Veja a seguir algumas diretrizes que devem ser levadas em consideração:

*   Use indicações de áudio 3D para fornecer informações espaciais adicionais.
* Separe os controles de volume de música, fala e efeitos sonoros.
*   Projete uma fala que dê informações significativas para os jogadores. (Exemplo: "Inimigos se aproximando" x "Inimigos estão entrando pela porta de trás".)
*   Certifique-se de que a fala esteja em uma velocidade razoável e ofereça um controle de velocidade para melhor acessibilidade.

### <a name="fully-mappable-controls"></a>Controles totalmente mapeáveis

Há empresas e organizações, como a [Special Effect](http://www.specialeffect.org.uk/), que projetam controladores de jogos personalizados que podem ser usados com diversos sistemas de jogos como Windows e Xbox One. Essa personalização permite que pessoas com diferentes formas de deficiências joguem jogos que não conseguiriam de outra forma. Para obter mais informações sobre as pessoas que agora são capazes de executar jogos de maneira independente por causa de controladores personalizados, consulte [quem eles ajudaram](http://www.specialeffect.org.uk/who-we-helped).

Como um desenvolvedor de jogos, você pode deixar o jogo mais acessível, permitindo controles totalmente mapeáveis, de maneira que os jogadores tenham a opção de conectar os controladores personalizados e remapear as teclas de acordo com as necessidades.

Os controladores do Xbox One e do Xbox Elite padrão oferecem a personalização dos controladores para jogos de precisão. Para obter mais informações, consulte [Xbox One](http://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) e [Xbox Elite](http://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Seleção mais ampla de níveis de dificuldade

Videogames oferecem entretenimento. O desafio para desenvolvedores de jogos é ajustar o nível de dificuldade de maneira que o jogador enfrente o desafio adequado. Em primeiro lugar, nem todos os jogadores têm o mesmo nível de habilidade e funcionalidade, logo, projetar uma seleção mais ampla de opções de dificuldade aumenta a chance de dar aos jogadores o nível certo de desafio. Ao mesmo tempo, essa seleção mais ampla também torna o jogo mais acessível porque ele pode permitir que mais pessoas com deficiências joguem o jogo. Lembre-se: os jogadores desejam superar desafios de um jogo e ser recompensados por isso. Eles não querem um jogo em que não possam vencer.

Ajustar o nível de dificuldade do jogo é um processo delicado. Se for muito fácil, os jogadores poderão ficar entediados. Se for muito difícil, os jogadores poderão desistir e deixar de jogar desse ponto em diante. O processo de equilíbrio é arte e ciência. Há muitas maneiras de deixar um nível de jogo com o nível de desafio certo. Alguns jogos oferecem entradas simplificadas, como uma opção de jogo com pressionamento de botão único para a opção de jogo, uma opção de retroceder e repetir para deixar o jogo mais permissivo, ou menos inimigos e mais fracos para facilitar o avanço depois de diversas tentativas.

### <a name="photosensitivity-epilepsy-testing"></a>Teste de epilepsia para fotossensibilidade

Epilepsia fotossensível (PSE) é uma condição na qual ataques são ocasionados por estímulos visuais como exposições a luzes piscantes ou a determinadas formas e padrões visuais em movimento. Isso ocorre em aproximadamente três por cento das pessoas e é mais comum em crianças e adolescentes.

Há vários fatores que podem causar uma reação fotossensível durante a execução de vídeo games, inclusive a duração do jogo, a frequência do piscante, a intensidade da luz, contraste do segundo plano e da luz, além da distância entre a tela e o jogador e o comprimento de onda da luz.

Como um desenvolvedor, aqui estão algumas dicas para desenvolver um jogo a fim de incluir jogadores que tenham a tendência de epilepsia fotossensível:

*   Evite luzes piscantes com uma frequência de 5 a 30 piscadas por segundo (Hertz) porque luzes piscando nessa faixa têm mais probabilidade de causar convulsões.
*   Use um sistema automatizado para verificar o jogo em busca de estímulos que possam causar epilepsia fotossensível. (Exemplo: [Harding Flash and Pattern Analyzer (FPA) G2](http://www.hardingfpa.com/harding-fpa-for-games/) desenvolvido pela Cambridge Research System Ltd e pelo Professor Graham Harding.) 
*   Projete tendo em vista pausas entre níveis de jogos, incentivando jogadores a fazer uma pausa do jogo ininterrupto.

## <a name="other-accessibility-resources"></a>Outros recursos de acessibilidade

Aqui estão alguns sites externos que fornecem informações adicionais sobre acessibilidade em jogos.

### <a name="game-accessibility-guidelines"></a>Diretrizes de acessibilidade em jogos
* [Diretrizes de acessibilidade em jogos](http://gameaccessibilityguidelines.com/)
* [Diretrizes da AbleGamers Foundation](http://www.includification.com/)
* [Projetar jogos acessíveis universalmente (UA)](http://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Controles de entrada personalizados
* [Efeito especial](http://www.specialeffect.org.uk/)
* [Guerreiro engajado](http://www.warfighterengaged.org/)

## <a name="references-used"></a>Referências usadas
* [Diretrizes de acessibilidade em jogos](http://gameaccessibilityguidelines.com/)
* [Diretrizes da AbleGamers Foundation](http://www.includification.com/)
* [Color Blind Awareness, uma empresa de interesse comunitário](http://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Como fazer boas legendas – um artigo do blog Gamasutra por Ian Hamilton](http://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Programa Innovation for All](http://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)

## <a name="related-links"></a>Links relacionados
* [Design inclusivo](https://www.microsoft.com/design/inclusive)
* [Hub de desenvolvedor de acessibilidade da Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Desenvolvendo apps UWP acessíveis](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [Livro eletrônico sobre software de engenharia para acessibilidade](https://www.microsoft.com/download/details.aspx?id=19262)

