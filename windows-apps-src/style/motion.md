---
author: mijacobs
Description: "Um movimento proposital e bem projetado traz vida ao seu aplicativo e faz com que a experiência seja elaborada e refinada. Ajude os usuários a entenderem as alterações de contexto e vincule experiências com transições visuais."
title: "Movimento e animação em apps UWP"
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.openlocfilehash: d24edf5eaacb65ca28f2f2727f441cb833141dcf
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="motion-for-uwp-apps"></a>Movimento para apps UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Um movimento proposital e bem projetado traz vida ao seu aplicativo e faz com que a experiência seja elaborada e refinada. O movimento ajuda os usuários a entenderem as alterações de contexto e onde elas estão dentro da hierarquia de navegação do seu aplicativo. Ele vincula experiências através de transições visuais. O movimento acrescenta uma sensação de ritmo e dimensionalidade à experiência.

## <a name="benefits-of-motion"></a>Benefícios do movimento

O movimento é mais do que fazer as coisas se moverem. O movimento é uma ferramenta para criar um ecossistema físico para o usuário viver e manipular através de uma variedade de tipos de entrada, como mouse, teclado, toque e caneta. A qualidade da experiência depende de quão bem o aplicativo responde ao usuário, e que tipo de personalidade a interface de usuário comunica.

Certifique-se de que o movimento tem um propósito em seu aplicativo. Os melhores aplicativos da Plataforma Universal do Windows (UWP) usam movimento para dar vida à interface de usuário. O movimento deve:

- Fornecer feedback baseado no comportamento do usuário.
- Ensinar ao usuário como interagir com a interface do usuário.
- Indicar como navegar a exibições anteriores ou para as próximas.

Conforme um usuário passa mais tempo dentro de seu aplicativo, ou conforme as tarefas em seu aplicativo se tornam mais sofisticadas, movimento de alta qualidade torna-se cada vez mais importante: ele pode ser utilizado para modificar o modo como o usuário percebe sua carga cognitiva e a facilidade de uso de seu aplicativo. O movimento possui muitos outros benefícios diretos:

- **O movimento oferece suporte à interação e à orientação.**

    O movimento é direcional: ele se move para frente e para trás, para dentro e para fora do conteúdo, deixando uma trilha de pistas mental sobre como o usuário chegou na exibição atual. As transições podem ajudar os usuários a aprender como operar novas aplicações desenhando analogias com tarefas que o usuário já é familiarizado.

- **O movimento pode dar a impressão de desempenho aprimorado.**

    Quando as velocidades da rede diminuem ou o sistema interrompe seu trabalho, as animações podem fazer com que a espera do usuário pareça menor. As animações podem ser utilizadas para permitir que o usuário saiba que o aplicativo está processando e não travado, e pode exibir passivamente novas informações sobre as quais o usuário pode estar interessado.

- **O movimento acrescenta personalidade.**

    O movimento é geralmente o segmento comum que comunica sua a personalidade de seus aplicativos conforme um usuário se move ao longo de uma experiência.

- **O movimento acrescenta elegância.**

    Um movimento fluido e dinâmico faz com que as experiências sejam naturais, criando conexões emocionais com a experiência.

## <a name="examples-of-motion"></a>Exemplos de movimento

Aqui estão alguns exemplos de movimento em um aplicativo.

Aqui, um aplicativo usa uma animação conectada para animar uma imagem conforme ela "continua" a fazer parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Animação Conectada](images/connected-animations/example.gif)

Aqui, um efeito de paralaxe visual move objetos diferentes em diferentes taxas quando a interface de usuário rola ou faz um movimento panorâmico para criar uma sensação de profundidade, perspectiva e movimento.

![Um exemplo de paralaxe com uma imagem de fundo e uma lista](images/_Parallax_v2.gif)


## <a name="types-of-motion"></a>Tipos de movimento

<table>
    <tr>
        <th align="left">Tipo de movimento</th>
        <th align="left">Descrição</th>
    </tr>
    <tr>
        <td>[Adicionar e excluir](motion-list.md)
        </td>
        <td>Animações de lista permitem inserir ou remover um ou vários itens de uma coleção, como um álbum de fotos ou uma lista de resultados de pesquisa.
        </td>
    </tr>
    <tr>
        <td>[Animação conectada](connected-animation.md)
        </td>
        <td>As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente através da animação da transição de um elemento entre duas exibições diferentes. Isso ajuda o usuário a manter o contexto e oferece continuidade entre os modos de exibição. Em uma animação conectada, um elemento parece "continuar" entre duas exibições durante uma alteração no conteúdo da interface de usuário, voando pela tela desde a sua localização na exibição de origem até seu destino na nova exibição. Isso enfatiza o conteúdo comum entre os modos de exibição e cria um efeito belo e dinâmico como parte de uma transição. 
        </td>
    </tr>
    <tr>
        <td>[Transição de conteúdo](content-transition-animations.md)
        </td>
        <td>As animações de transição de conteúdo permitem que você mude o conteúdo de uma área da tela enquanto mantém o recipiente ou o plano de fundo constantes. O novo conteúdo aparece. Se houver conteúdo existente para ser substituído, esse conteúdo desaparecerá.
        </td>
    </tr>
    <tr>
        <td>[Drill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation)
        </td>
        <td>Use uma drill-in quando um usuário navega para frente em uma hierarquia lógica, como de uma lista mestre para uma página de detalhes. Use uma animação drill-out quando um usuário navega para trás em uma hierarquia lógica, como de uma página de detalhes para uma página mestre.
        </td>
    </tr>
    <tr>
        <td>[Desaparecer](motion-fade.md)
        </td>
        <td>Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações comuns de desaparecimento são o fade-in e o fade-out.
        </td>
    </tr>
        <tr>
        <td>[Paralaxe](parallax.md)
        </td>
        <td>Um efeito de paralaxe visual ajuda a criar uma sensação de profundidade, perspectiva e movimento. Ele alcança este efeito movendo objetos diferentes em diferentes taxas quando a interface de usuário rola ou faz um movimento panorâmico.
        </td>
    </tr> 
    <tr>
        <td>[Pressione feedback](motion-pointer.md)
        </td>
        <td>As animações de apertar ponteiro fornecem aos usuários feedback visual quando o usuário toca em um item. A animação de ponteiro para baixo encolhe e inclina um pouco o item pressionado e aparece quando um item é tocado pela primeira vez. A animação de ponteiro para cima, que restaura o item à sua posição original, aparece quando o usuário solta o ponteiro.
        </td>
    </tr>
</table>