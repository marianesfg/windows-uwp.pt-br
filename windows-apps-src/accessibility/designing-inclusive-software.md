---
author: Xansky
Description: Saiba mais sobre como evoluir o design inclusivo com aplicativos da Plataforma Universal do Windows (UWP) para o Windows 10.  Crie e compile software inclusivo com a acessibilidade em mente.
ms.assetid: A6393A57-53F2-4F06-89AF-0D806FD76DB0
title: Projetando software inclusivo no Windows 10
label: Designing inclusive software
template: detail.hbs
ms.sourcegitcommit: ea4d413e0b2ade1429d255afbc6a1a73ea308051
ms.openlocfilehash: 6f1c0663034f81bb0ddfe42c04fbe60562b45c1c

---

# Projetando software inclusivo para Windows 10  

Saiba mais sobre como evoluir o design inclusivo com aplicativos da Plataforma Universal do Windows (UWP) para o Windows 10.  Crie e compile software inclusivo com a acessibilidade em mente.

Na Microsoft, estamos desenvolvendo nossos princípios de design e práticas recomendadas. Eles mostram a aparência, a função e o comportamento das nossas experiências. Estamos elevando a nossa perspectiva.

Essa nova filosofia de design é chamada de design inclusivo. A ideia é criar um software com todas as pessoas em mente desde o começo. Isso é o oposto de enxergar a acessibilidade como uma tecnologia você inclui no final do processo de desenvolvimento para satisfazer alguns pequenos grupos de usuários.

"Definimos deficiência como uma incompatibilidade entre as necessidades do indivíduo e o serviço, produto ou ambiente oferecidos. Qualquer pessoa pode sofrer uma deficiência. É uma característica humana comum a ser eliminada".  \- do vídeo [Inclusivo](https://www.microsoft.com/en-us/design/inclusive)  

O design inclusivo cria produtos melhores para todos. Trata-se de considerar a gama completa de diversidade humana. Considere os recortes de meio-fio que você encontra na maioria das calçadas das esquinas. Eles foram claramente pensados para serem usados por pessoas em cadeiras de rodas. Mas agora quase todo mundo os usa, inclusive pessoas com carrinhos de bebê, ciclistas, esqueitistas. Até mesmo pedestres geralmente os usam porque estão disponíveis e proporcionam uma melhor experiência. O controle remoto da televisão pode ser considerado uma tecnologia assistencial (AT) para alguém com limitações físicas. No entanto hoje é praticamente impossível comprar uma televisão sem um. Antes de as crianças aprenderem a amarrar os sapatos, elas podem usar sapatos com fechamento de velcro. Sapatos que são fáceis de calçar e tirar geralmente são preferenciais em culturas nas quais eles são removidos antes de entrar em uma casa. Eles também são melhores para pessoas com problemas de destreza como artrite ou até mesmo um pulso temporariamente quebrado.

## Princípios de design inclusivo  
Os quatro princípios seguintes estão orientando a mudança da Microsoft para o design inclusivo:

**Pense universal**: vamos nos concentrar no que unifica as pessoas — motivações, relações e habilidades humanas. Isso nos ajuda a considerar o impacto social mais amplo do nosso trabalho. O resultado é uma experiência que tem uma diversidade de maneiras para todas as pessoas participarem.

**Personalize**: em seguida, nós nos desafiamos a criar conexões emocionais. Interações de humanos com humanos podem inspirar melhor a interação de humanos com a tecnologia. Circunstâncias únicas de uma pessoa podem melhorar um design para todos. O resultado é uma experiência que parece ter sido criada para uma pessoa.

**Mantenha a simplicidade**: começamos com a simplicidade como o unificador principal. Quando reduzimos a desordem as pessoas sabem o que fazer em seguida. Eles são inspirados a seguir em frente em espaços que são limpos, luminosos e abertos. O resultado é uma experiência que seja sincera e atemporal.

**Crie alegria**: experiências alegres evocam admiração e descoberta. Às vezes, é mágico. Às vezes, é um detalhe que está correto. Criamos esses momentos para que pareçam uma mudança bem-vinda no ritmo. O resultado é uma experiência que tem impulso e fluxo.

## Usuários de design inclusivo  
Há essencialmente dois tipos de usuários de tecnologia assistencial (AT):

1. Aqueles que precisam dela por deficiências ou comprometimentos, condições relacionadas à idade ou condições temporárias (como mobilidade limitada por um membro quebrado)  
2. Aqueles que a utilizam por preferência, para uma experiência de computação mais confortável ou conveniente

A maioria dos usuários de computador (54%) conhece alguma forma de tecnologia assistencial e 44% dos usuários de computador usa alguma forma dela, mas muitos deles não está usando a AT que poderia beneficiá-los (Forrester 2004).  

Um estudo de 2003-2004 contratado pela Microsoft e realizado pela Forrester Research descobriu que mais da metade &mdash;57%&mdash; dos usuários de computador nos Estados Unidos entre as idades de 18 e 64 anos poderiam se beneficiar da tecnologia assistencial. A maioria desses usuários não se identificou como tendo uma deficiência ou comprometimento, mas expressou determinadas dificuldades relacionadas a comprometimentos ou deficiências ao usar um computador. A Forrester (2003) também descobriu o seguinte número de usuários com estas dificuldades específicas: um em quatro tem uma dificuldade visual. Um em quatro sente dor nos pulsos ou mãos. Uma em cinco tem dificuldade de audição.  

Além de deficiências permanentes, a gravidade e os tipos de dificuldades que uma pessoa tem podem variar durante a vida dela. Não existe um ser humano normal. Nossas habilidades estão sempre mudando. Margaret Meade disse, "Somos todos únicos. Sermos todos únicos nos torna iguais".  

A Microsoft se dedica à realização de pesquisas de ciência da computação e de engenharia de software com metas para aperfeiçoar a experiência de computação e inventar novas tecnologias de computação. Veja [Projetos de desenvolvimento e pesquisa atuais da Microsoft Research](https://www.microsoft.com/enable/microsoft/research.aspx) destinados a tornar o computador mais acessível e fácil de ver, ouvir e interagir.  

## Etapas de design prático  
Se você estiver disposto, esta seção é para você. Ela descreve as etapas de design prático a serem consideradas ao implementar o design inclusivo para seu aplicativo.  

### Descreva o público-alvo  
Defina os usuários em potencial do seu aplicativo. Avalie cuidadosamente todas as suas habilidades e características diferentes. Por exemplo, idade, sexo, idioma, usuários surdos ou com deficiências auditivas, deficiências visuais, habilidades cognitivas, estilo de aprendizagem, restrições de mobilidade e assim por diante. Seu design atende às necessidades individuais deles?  

### Converse com seres humanos reais com necessidades específicas  
Conheça usuários em potencial com características diversas. Verifique se que você está considerando todas as necessidades deles ao criar seu aplicativo. Por exemplo, a Microsoft descobriu que usuários com deficiência auditiva estavam desativando as notificações do sistema em seus consoles do Xbox. Quando perguntamos aos usuários com deficiência auditiva reais sobre isso, aprendemos que as notificações do sistema estavam obscurecendo uma seção de legendas. A correção foi exibir a notificação do sistema ligeiramente acima na tela. Essa foi uma solução simples que não era necessariamente óbvia para os dados de telemetria que inicialmente revelaram o comportamento.  

### Escolha uma estrutura de desenvolvimento com sabedoria  
No estágio de design, a estrutura de desenvolvimento que você usará (isto é, UWP, Win32, Web) é essencial para o desenvolvimento de seu produto. Se você tiver o luxo de escolher a sua estrutura, pense sobre quanto esforço levará para criar seus controles dentro da estrutura. Quais são as propriedades de acessibilidade padrão ou internas que vêm com ele? Quais são os controles que você precisará personalizar? Ao escolher sua estrutura, você está essencialmente escolhendo quanto dos controles de acessibilidade você receberá "gratuitamente" (ou seja, o quanto dos controles já estão incluídos) e quantos exigirão custos de desenvolvimento adicionais devido às personalizações de controle.   

Use controles padrão do Windows sempre que possível. Esses controles já estão habilitados com a tecnologia necessária para interface com tecnologias assistenciais.

### Criar uma hierarquia lógica para seus controles  
Depois de ter a sua estrutura, crie uma hierarquia lógica para mapear seus controles. A hierarquia lógica do seu aplicativo inclui o layout e a ordem de tabulação dos controles. Quando os programas de tecnologia assistencial (AT), como leitores de tela, leem a sua interface do usuário, a apresentação visual não é suficiente; você deve fornecer uma alternativa programática que faça sentido de forma estrutural para os usuários. Uma hierarquia lógica pode ajudá-lo a fazer isso. É uma maneira de estudar o layout da sua interface do usuário e a estruturação de cada elemento para que os usuários possam entendê-la. Uma hierarquia lógica é usada principalmente para:  

1.  Fornecer contexto de programas para a ordem lógica (leitura) dos elementos da interface do usuário  
2.  Identificar limites claros entre os controles padrão e os controles personalizados na interface do usuário  
3.  Determinar como partes da interface do usuário interagem juntas  

Uma hierarquia lógica é uma ótima maneira de lidar com quaisquer possíveis problemas de usabilidade. Se não pode estruturar a interface do usuário de maneira relativamente simples, talvez você possa ter problemas com a usabilidade. Uma representação lógica de uma caixa de diálogo simples não deve resultar em páginas de diagramas. Para hierarquias lógicas que se tornam muito profundas ou muito amplas, talvez seja necessário reprojetar sua interface do usuário. Para saber mais, baixe o eBook [Engenharia de software para acessibilidade](https://www.microsoft.com/en-us/download/details.aspx?id=19262).  

### Crie configurações visuais apropriadas da interface do usuário  
Ao criar a interface do usuário visual, verifique se o seu produto tem uma configuração de alto contraste, usa as fontes padrão do sistema e as opções de suavização, é dimensionada corretamente para as configurações de tela de pontos por polegada (dpi), tem texto padrão com pelo menos uma taxa de contraste de 5:1 com a tela de fundo e tem combinações de cores que serão fáceis para os usuários com daltonismo diferenciar.  

#### Configuração de alto contraste  
Um dos recursos integrados de acessibilidade no Windows é o modo de alto contraste, que aumenta o contraste de cor de texto e imagens. Para algumas pessoas, aumentar o contraste das cores reduz a fadiga ocular e facilita a leitura. Ao verificar sua interface do usuário no modo de alto contraste, você deseja verificar se os controles, tais como links, foram codificados consistentemente e com cores do sistema (não com cores embutidas em código) para garantir que os usuários possam ver todos os controles na tela que um usuário que não usa alto contraste veria.  

#### Configurações de fonte do sistema  
Para garantir a legibilidade e minimizar quaisquer distorções inesperadas no texto, certifique-se de que seu produto sempre use as fontes padrão do sistema e use as opções de suavização. Se o seu produto usa fontes personalizadas, os usuários podem enfrentar distrações e questões de legibilidade significativas ao personalizarem a apresentação de sua interface do usuário (com o uso de um leitor de tela ou usando diferentes estilos de fonte para exibir sua interface do usuário, por exemplo).  

#### Resoluções de alto DPI  
Para usuários com deficiência visual, é importante ter uma interface do usuário escalonável. Interfaces de usuário que não sejam dimensionadas corretamente em resoluções de alto DPI podem fazer com que componentes importantes se sobreponham ou ocultem outros componentes, e podem se tornar inacessíveis.  

#### Taxa de contraste de cor  
A Seção 508 atualizada do Americans with Disability Act (ADA), bem como outras leis, exige que o contraste de cor padrão entre o texto e seu plano de fundo deve ser 5:1. Para textos grandes (tamanhos de fonte de 18 pontos ou 14 pontos e em negrito), o contraste padrão necessário é 3:1.  

#### Combinações de cores  
Cerca de 7% dos homens (e menos de 1% das mulheres) têm alguma forma de deficiência para identificar cores. Os usuários com daltonismo têm problemas para diferenciar determinadas cores, portanto, é importante que não apenas a cor seja usada para transmitir status ou significado em um aplicativo. Assim como em imagens decorativas (como ícones ou planos de fundo), as combinações de cores devem ser escolhidas de forma que maximize a percepção da imagem por usuários daltônicos. Se o seu design usa essas recomendações de cor desde o início, seu aplicativo já estará dando passos significativos para ser inclusivo.  

## Resumo &mdash; sete etapas para design inclusivo  
Em resumo, siga essas sete etapas para garantir que o seu software seja inclusivo.  
1.  Decida se o design inclusivo é um aspecto importante para o seu software. Se for, aprenda e aprecie como ele permite que os usuários reais vivam, trabalhem e joguem, para ajudar a orientar o seu design.  
2.  Ao criar soluções para suas necessidades, use controles fornecidos por sua estrutura (controles padrão) tanto quanto possível e evite qualquer esforço desnecessário e os custos de controles personalizados.  
3.  Crie uma hierarquia lógica para seu produto, observando onde estão os controles padrão, os controles personalizados e o foco do teclado na interface do usuário.  
4.  Crie configurações úteis do sistema (como navegação de teclado, alto contraste e alto dpi) em seu produto.  
5.  Implemente seu design, usando o [hub de desenvolvedor de acessibilidade Microsoft](https://developer.microsoft.com/en-us/windows/accessible-apps) e a especificação de acessibilidade da estrutura como um ponto de referência.  
6.  Teste seu produto com os usuários com necessidades especiais para garantir que eles poderão aproveitar as técnicas de design inclusivo implementadas nele.  
7.  Ofereça seu produto final e documente sua implementação para quem possa trabalhar no projeto depois de você.  

## Tópicos relacionados  
* [Design inclusivo](http://design.microsoft.com/inclusive)
* [Software de engenharia para acessibilidade](https://www.microsoft.com/en-us/download/details.aspx?id=19262)
* [Hub de desenvolvedor de acessibilidade da Microsoft](https://developer.microsoft.com/en-us/windows/accessible-apps)
* [Desenvolvendo aplicativos inclusivos do Windows](developing-inclusive-windows-apps.md)  



<!--HONumber=Jun16_HO4-->


