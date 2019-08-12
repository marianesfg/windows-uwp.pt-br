---
Description: Um movimento proposital e bem projetado traz vida ao seu aplicativo e faz com que a experiência seja elaborada e refinada. Ajude os usuários a entenderem as alterações de contexto e vincule experiências com transições visuais.
title: Movimento e animação em aplicativos UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 31cf2134fb8f77809b75a5abf3e6980443452059
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867399"
---
# <a name="motion-for-uwp-apps"></a>Movimento para aplicativos UWP

![Ícone de movimento](../images/motion-2x.png)

O movimento fluente tem um propósito em seu aplicativo. Ele fornece comentários inteligentes com base no comportamento do usuário, mantém a interface do usuário ativa e guia a navegação do usuário no aplicativo. O movimento fluente gera uma conexão emocional entre um usuário e sua experiência digital. Aproveitamos uma base de movimento natural que o usuário já entende do mundo físico e ampliamos nosso sistema com base nisso.

## <a name="examples"></a>Exemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/category/Motion">abrir o aplicativo e ver o Movimento em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-motion-principles"></a>Princípios de movimento fluente

### <a name="physical"></a>Físico

Objetos em movimento apresentam comportamentos de objetos no mundo real. Um movimento fluente e dinâmico faz com que a experiência seja natural, criando conexões emocionais e adicionando personalidade.

![Exemplo de interface do usuário de movimento físico](images/Physical.gif)
> Quando você interage com a interface do usuário por meio de toque, o movimento da interface do usuário está diretamente relacionado à velocidade da interação. Como o toque é a manipulação direta, o objeto com o qual você interage afeta os objetos ao redor.

### <a name="functional"></a>Funcional

O movimento tem um propósito e uma convicção. Ele orienta o usuário pela complexidade e ajuda a estabelece a hierarquia. O movimento oferece a impressão de desempenho aprimorado e otimiza a experiência do usuário, ocultando a latência percebida.

![Exemplo de interface do usuário de movimento funcional](images/functional.gif)
> As transições de página têm finalidade específica. Elas fornecem dicas sobre como as páginas são relacionadas entre si. Elas se movem de forma considerada rápida até mesmo quando o desempenho não é ideal.

### <a name="continuous"></a>Contínuo

O movimento fluente de um ponto a outro naturalmente atrai a atenção e orienta o usuário. Com elegância, ele une uma tarefa do usuário, fazendo parecer mais amigável e consumível.

![Exemplo de interface do usuário de movimento contínuo](images/continuous3.gif)
> Objetos podem deslocar de cena em cena ou se transformar dentro de uma cena para fornecer continuidade e ajudar o usuário a manter o contexto.

### <a name="contextual"></a>Contextual

O movimento inteligente fornece comentários ao usuário de forma alinhada com a manipulação da interface do usuário. A interação está centralizada no usuário. O movimento parece apropriado ao fator forma e é projetado tendo em vista o cenário. Deve ser confortável para cada usuário.

![Exemplo de interface do usuário de movimento contextual](images/Contextual.gif)
> A animação deve se relacionar à interação do usuário. Um menu de contexto é implantado de um o ponto em que o usuário o ativa.

## <a name="motion-articles"></a>Artigos sobre movimento

:::row:::
    :::column:::
### <a name="timing-and-easingtiming-and-easingmd"></a>[Tempo e suavização](timing-and-easing.md)
O tempo e a suavização são elementos importantes que tornam o movimento natural para objetos que entram, saem ou se movem na interface do usuário.
    :::column-end:::
    :::column:::
### <a name="directionality-and-gravitydirectionality-and-gravitymd"></a>[Direcionalidade e gravidade](directionality-and-gravity.md)
Os sinais de direção ajudam a fornecer um modelo mental sólido da jornada do usuário nas experiências. O movimento direcional está sujeito a forças como gravidade, o que reforça a sensação natural da movimentação.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
### <a name="page-transitionspage-transitionsmd"></a>[Transições de página](page-transitions.md)
As transições de página navegam os usuários entre as páginas de um aplicativo, fornecendo comentários sobre o relacionamento entre as páginas. Elas ajudam os usuários a entender onde estão na hierarquia de navegação.
    :::column-end:::
    :::column:::
### <a name="connected-animationconnected-animationmd"></a>[Animação conectada](connected-animation.md)
As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente pela animação da transição de um elemento entre duas exibições diferentes.
    :::column-end:::
:::row-end:::
