---
author: Xansky
Description: Learn to develop accessible Windows 10 UWP apps that include keyboard navigation, color and contrast settings, and support for assistive technologies.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: Desenvolvendo aplicativos inclusivos do Windows 10
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2adb3c67c4c7c1d024cd969af15cc12baa424511
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7429532"
---
# <a name="developing-inclusive-windows-apps"></a>Desenvolvendo aplicativos inclusivos do Windows  

Este artigo discute como desenvolver aplicativos UWP (Plataforma Universal do Windows) acessíveis. Especificamente, ele presume que você entende como criar a hierarquia lógica para seu aplicativo. Aprenda a desenvolver aplicativos UWP para Windows 10 acessíveis que incluem navegação de teclado, configurações de cor e contraste, além de suporte para as tecnologias assistenciais.

Se você ainda não tiver feito isso, comece lendo [Criando software inclusivo](designing-inclusive-software.md).

Há três coisas que você deve fazer para se certificar de que seu aplicativo seja acessível:

1. Exponha seus elementos de interface do usuário para [acesso programático](#programmatic-access).
2. Certifique-se de que seu aplicativo dê suporte a [navegação de teclado](#keyboard-navigation) pessoas que não são capazes de usar um mouse ou uma tela touch.
3. Certifique-se de que seu aplicativo dê suporte a configurações [cor e contraste](#color-and-contrast) acessíveis.

## <a name="programmatic-access"></a>Acesso programático  
O acesso programático é essencial para a criação de acessibilidade em aplicativos. Isso é realizado por meio da definição do nome acessível (obrigatório) e a descrição (opcional) dos elementos de interface de usuário interativa e do conteúdo em seu aplicativo. Isso garante que os controles de interface do usuário sejam expostos à tecnologia assistencial (a), como leitores de tela (por exemplo, o Narrador) ou dispositivos de saída alternativos (como telas em Braille). Sem acesso programático, as APIs de tecnologia assistencial não conseguem interpretar informações corretamente, deixando o usuário incapaz de usar os produtos suficientemente ou forçando o AT a usar as interfaces de programação não documentadas ou técnicas nunca antes usadas como uma interface de acessibilidade. Quando os controles de interface do usuário são expostos à tecnologia assistencial, a AT é capaz de determinar quais ações e opções estão disponíveis para o usuário.  

Para saber mais sobre como tornar os elementos de interface do usuário do seu aplicativo disponíveis para as tecnologias assistenciais (AT), veja [Expor informações básicas de acessibilidade](basic-accessibility-information.md).

## <a name="keyboard-navigation"></a>Navegação de teclado  
Para usuários cegos ou que têm problemas de mobilidade, é extremamente importante ser capaz de navegar a interface do usuário com um teclado. No entanto, apenas os controles de interface do usuário que exigem interação do usuário à função devem ter o foco do teclado. Componentes que não exigem uma ação, como imagens estáticas, não precisam do foco do teclado.  

É importante lembrar que, ao contrário da navegação com um mouse ou toque, a navegação de teclado é linear. Ao considerar a navegação de teclado, pense em como o usuário interagirá com seu produto e qual será a lógica de navegação. Em culturas ocidentais, as pessoas leem da esquerda para a direita, de cima para baixo. Portanto, é prática comum seguir esse padrão para navegação de teclado.  

Ao projetar a navegação de teclado, examine sua interface do usuário e pense nestas perguntas:
* Como os controles são dispostos ou agrupados na interface do usuário?
* Há alguns grupos significativos de controles?
    * Em caso afirmativo, esses grupos contêm outro nível de grupos?
*   Entre os controles pares, a navegação deve ser feita por tabulação ou via navegação especial (como teclas de direção), ou ambos?

O objetivo é ajudar o usuário a entender como a interface do usuário é disposta e identificar os controles que são acionáveis. Se você estiver achando que existem muitas tabulações antes de o usuário concluir o loop de navegação, considere a possibilidade de agrupar os controles relacionados. Alguns controles relacionados, como um controle híbrido, talvez precisem ser abordados neste estágio de exploração inicial. Depois que você começar a desenvolver seu produto, é difícil fazer reformulações na navegação de teclado, portanto, planeje com cuidado e antecipadamente!  

Para saber mais sobre a navegação de teclado entre elementos de interface do usuário, veja [Acessibilidade de teclado](keyboard-accessibility.md).  

Além disso, o eBook [Software de engenharia para acessibilidade](https://www.microsoft.com/download/details.aspx?id=19262) tem um capítulo excelente sobre esse assunto intitulado _Projetando a hierarquia lógica_.

## <a name="color-and-contrast"></a>Cor e contraste  
Um dos recursos integrados de acessibilidade no Windows é o modo de alto contraste, o que aumenta o contraste de cor de texto e de imagens na tela do computador. Para algumas pessoas, aumentar o contraste das cores reduz a fadiga ocular e facilita a leitura. Ao verificar sua interface do usuário em alto contraste, você deseja verificar se os controles foram codificados consistentemente e com cores do sistema (não com cores embutidas em código) para garantir que os usuários possam ver todos os controles na tela que um usuário que não usa alto contraste veria.  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
Para saber mais sobre o uso de cores e recursos do sistema, veja [Recursos de tema XAML](../controls-and-patterns/xaml-theme-resources.md).

Desde que você não tenha substituído as cores do sistema, um aplicativo UWP dá suporte a temas de alto contraste por padrão. Quando o usuário decide que o sistema deve utilizar um tema de alto contraste das configurações de sistema ou ferramentas de acessibilidade, a estrutura usa automaticamente as cores e as configurações de estilo que produzem layout e renderização de alto contraste para controles e componentes na interface do usuário.   

Para saber mais, veja [Temas de alto contraste](high-contrast-themes.md).  

Se você decidiu usar seu próprio tema de cores em vez de cores do sistema, considere estas diretrizes:  

**Taxa de contraste de cor** – A Seção 508 atualizada do Americans with Disability Act, bem como outras leis, exige que o contraste de cor padrão entre o texto e seu plano de fundo deve ser 5:1. Para texto grande (tamanhos de fonte de 18 pontos ou 14 pontos e em negrito), o contraste padrão necessário é 3:1.  

**Combinações de cores** – Cerca de 7% dos homens (e menos de 1% das mulheres) têm alguma forma de deficiência para identificar cores. Os usuários com daltonismo têm problemas para diferenciar determinadas cores, portanto, é importante que não apenas a cor seja usada para transmitir status ou significado em um aplicativo. Assim como em imagens decorativas (como ícones ou planos de fundo), as combinações de cores devem ser escolhidas de forma que maximize a percepção da imagem por usuários daltônicos.  

## <a name="accessibility-checklist"></a>Lista de verificação de acessibilidade  
Veja a seguir uma versão abreviada da lista de verificação de acessibilidade:

1. Defina o nome acessível (obrigatório) e a descrição (opcional) dos elementos de interface de usuário interativa e do conteúdo em seu aplicativo.
2. Implemente a acessibilidade do teclado.
3. Verifique sua interface do usuário para garantir que o contraste do texto esteja adequado, que os elementos renderizem corretamente nos temas em alto contraste e que as cores estejam sendo usadas corretamente.
4. Execute ferramentas de acessibilidade, resolva problemas relatados e verifique a experiência de leitura da tela. (Veja o tópico Testes de acessibilidade)
5. Verifique se as configurações do manifesto do aplicativo seguem as diretrizes de acessibilidade.
6. Declare seu aplicativo como acessível na Microsoft Store. (Veja o tópico [Acessibilidade na loja](accessibility-in-the-store.md)).

Para obter mais detalhes, veja o tópico [Lista de verificação de acessibilidade](accessibility-checklist.md) inteiro.

## <a name="related-topics"></a>Tópicos relacionados  
* [Criando software inclusivo](designing-inclusive-software.md)  
* [Design inclusivo](http://design.microsoft.com/inclusive)
* [Práticas de acessibilidade a evitar](practices-to-avoid.md)
* [Software de engenharia para acessibilidade](https://www.microsoft.com/download/details.aspx?id=19262)
* [Hub de desenvolvedor de acessibilidade da Microsoft](https://msdn.microsoft.com/enable)
* [Acessibilidade](accessibility.md)
