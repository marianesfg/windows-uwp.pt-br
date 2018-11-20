---
author: stevewhims
Description: Design your app to provide bidirectional text support (BiDi) so that you can combine script from left-to-right (LTR) and right-to-left (RTL) writing systems, which generally contain different types of alphabets.
title: Projetar seu app para texto bidirecional
template: detail.hbs
ms.author: stwhi
ms.date: 11/10/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 24e4c5dfce4aa3e773ab8c334ca732ac5ed53030
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7417122"
---
# <a name="design-your-app-for-bidirectional-text"></a>Projetar seu app para texto bidirecional

Projete seu app para dar suporte a texto bidirecional (BiDi) para que você possa combinar scripts dos sistemas de escrita da direita para a esquerda (RTL) e da esquerda para a direita (LTR), que geralmente contêm diferentes tipos de alfabeto.

Os sistemas de escrita da direita para a esquerda, como os usados no Oriente Médio, na Ásia Central e do Sul e na África, possuem requisitos de design exclusivos. Esses sistemas de escrita exigem suporte a texto bidirecional (BiDi). O suporte bidirecional é a capacidade de inserir e exibir o layout do texto na ordem da direita para a esquerda (RTL) ou da esquerda para a direita (LTR).

Um total de nove idiomas bidirecionais são incluídos com o Windows.
- Dois idiomas totalmente traduzidos. Árabe e hebraico.
- Sete Language Interface Packs para mercados emergentes. Persa, urdu, dari, curdo central, sindhi, panjabi (Paquistão) e uigur.

Este tópico contém a filosofia do design bidirecional do Windows e estudos de caso que mostram considerações do design bidirecional.

## <a name="bidi-design-elements"></a>Elementos de design bidirecional

Quatro elementos influenciam as decisões de design bidirecional no Windows.

- **Espelhamento da interface do usuário**. O fluxo de interface do usuário permite que o conteúdo da direita para a esquerda seja apresentado em seu layout nativo. O design de interface do usuário parece local aos mercados bidirecionais.
- **Consistência em experiência do usuário**. O design parece natural na orientação da direita para a esquerda. Os elementos de interface do usuário compartilham uma direção de layout consistente e aparecem como o usuário os espera.
- **Otimização de toque**. Similar à experiência do usuário por toque na interface do usuário não espelhada, os elementos são fáceis de acessar e parecem naturais à interação por toque.
- **Suporte a texto misto**. O suporte à direção de texto permite a ótima apresentação de texto misto (texto em inglês nas compilações bidirecionais e vice versa).

## <a name="feature-design-overview"></a>Visão geral do projeto de recurso

O Windows oferece suporte aos quatro elementos de design bidirecional. Vamos olhar para algumas das principais características relevantes no Windows e fornecer algum contexto sobre como elas afetam o seu app.

### <a name="navigate-in-the-direction-that-feels-natural"></a>Navegue na direção em que se sente natural

Windows ajusta a direção da grade tipográfica para que ela flua da direita para a esquerda, significando que o primeiro bloco na grade é colocado no canto superior direito e o último bloco na parte inferior esquerda. Isso corresponde ao padrão RTL de publicações impressas, como livros e revistas, em que o padrão de leitura sempre começa no canto superior direito e caminha para a esquerda.

![Menu Iniciar bidirecional](images/56283_BIDI_01_startscreen_resized.png)
![Menu Iniciar bidirecional com botões](images/56283_BIDI_02_startscreen_charm_resized.png)

Para preservar um fluxo consistente da interface do usuário, o conteúdo nos blocos retêm um layout da direita para a esquerda, o que significa que o nome e o logotipo do app são posicionados no canto direito inferior do bloco, independentemente do idioma da interface do usuário do app.

#### <a name="bidi-tile"></a>Bloco bidirecional

![Bloco bidirecional](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Bloco em inglês

![Bloco em inglês](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Obtenha notificações de bloco que são lidas corretamente

Os blocos possuem suporte a texto misto. A região de notificação tem flexibilidade interna para ajustar o alinhamento do texto com base no idioma de notificação.  Quando um app envia árabe, hebraico ou outras notificações de idioma bidirecional, o texto é alinhado à direita. E quando chega uma notificação em inglês (ou outro da esquerda para a direita) chega de notificação, ele é alinhado à esquerda.

![Notificações de bloco](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Uma experiência de usuário da direita para a esquerda, de fácil toque e consistente.

Todos os elementos da interface do usuário do Windows se encaixam na orientação da direita para a esquerda. Os botões e menus suspensos foram posicionados na borda esquerda da tela para não ficarem sobre os resultados da pesquisa nem reduzirem a otimização de toque. Eles podem ser acessados facilmente com os polegares.

![Captura de tela bidirecional](images/56286_BIDI_05_search_flyout_resized.png)
![Captura de tela bidirecional](images/56286_BIDI_06_print_flyout_resized.png)

![Captura de tela bidirecional](images/56286_BIDI_07_settings_flyout_resized.png)
![Captura de tela bidirecional](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Entrada de texto em qualquer direção

O Windows oferece um teclado virtual limpo na tela, livre de distrações. Em idiomas bidirecionais, há uma chave de controle de direção de texto para que a direção de entrada de texto possa ser alternada, conforme necessário.

![Teclado virtual para o idioma bidirecional](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Use qualquer app em qualquer idioma

Instale e use seus aplicativos favoritos em qualquer idioma. Os aplicativos aparecem e funcionam como fariam em versões não bidirecionais do Windows. Os elementos nos aplicativos sempre são colocados em uma posição consistente e previsível.

![App em inglês mostrando conteúdo bidirecional](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Parênteses são exibidos corretamente

Com a introdução do Algoritmo de Parêntese Bidirecional (BPA), os parênteses emparelhados sempre são exibidos corretamente, independentemente das propriedades do idioma ou do alinhamento do texto.

#### <a name="incorrect-parentheses"></a>Parênteses incorretos

![App bidirecional com parênteses incorretos](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Parênteses corretos

![App bidirecional com parênteses corretos](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Tipografia

O Windows usa a fonte Segoe UI para todos os idiomas bidirecionais. Essa fonte foi totalmente adaptada à IU do Windows.

![Fonte Segoe UI para idioma bidirecional](images/56290_BIDI_13_start_screen_segoe.png)
![fonte Segoe UI para idioma bidirecional](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>Estudo de caso 1: um app de música bidirecional

### <a name="overview"></a>Visão geral

Os aplicativos multimídia são um desafio de design muito interessante, porque os controles de mídia geralmente têm um layout da esquerda para a direita semelhante ao dos idiomas não bidirecionais.

![Controles de mídia da esquerda para a direita](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Controles de mídia da direita para a esquerda](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Estabelecendo a direcionalidade de interface do usuário

É importante reter o fluxo da interface do usuário da direita para a esquerda para que se tenha um design consistente em mercados bidirecionais. A adição de elementos que têm fluxo da esquerda para a direita nesse contexto é difícil, pois alguns elementos de navegação, como o botão voltar, podem contradizer a orientação direcional do botão voltar nos controles de áudio.

![Página de controle do app de Música](images/56292_BIDI_16_app_layout_callouts_resized.png)

Este app de música retém a grade com orientação da direita para a esquerda. Isso dá ao app uma aparência bem natural para os usuários que já navegam nessa direção na IU do Windows. O fluxo é retido, garantindo que os principais elementos não sejam só ordenados da direita para a esquerda, mas também alinhados corretamente nos cabeçalhos de seção para manter o fluxo da interface do usuário.

![Página do álbum do app de música](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Manipulação de texto

A biografia do artista na captura de tela acima é alinhada à esquerda, enquanto as partes de texto relacionadas de outro artista, como nomes de álbuns e faixas, mantém o alinhamento à direita. O campo de biografia é um elemento de texto bastante grande que é mal lido quando está alinhado à direita simplesmente porque é difícil acompanhar as linhas durante a leitura de um bloco de texto maior. Em geral, qualquer elemento de texto com mais de duas ou três linhas que contenham cinco ou mais palavras pode ser considerado para exceções de alinhamento similar, em que o alinhamento do bloco de texto é oposto ao do layout geral da direção do aplicativo.

A manipulação de alinhamento no app pode parecer simples, mas muitas vezes expõe alguns dos limites inerentes aos mecanismos de renderização em termos de posicionamento de caracteres neutros em cadeias de caracteres bidirecionais. Por exemplo, a seguinte cadeia de caracteres pode ser exibida de forma diferente com base no alinhamento.

| | Cadeia de caracteres em inglês da esquerda para a direita (LTR) | Cadeia de caracteres em hebraico da direita para a esquerda (RTL) |
| -------------- | ------------------- | ------------------- |
| **Alinhamento à esquerda** | Olá, Mundo! | בוקר טוב! |
| **Alinhamento à direita** | !Olá, Mundo | !בוקר טוב |

Para garantir que as informações do artista sejam exibidas corretamente através do app de música, a equipe de desenvolvimento separou as propriedades de layout de texto do alinhamento. Em outras palavras, as informações de artistas podem ser muitas vezes exibidas com alinhamento à direita, mas o ajuste do layout de cadeias de caracteres é definido com base no processamento personalizado em segundo plano. O processamento em segundo plano determina a melhor configuração do layout direcional, com base no conteúdo da cadeia de caracteres.

![Página do artista do app de música](images/56292_BIDI_18_app_layout_callouts_resized.png)

Por exemplo: sem o processamento do layout personalizado da cadeia de caracteres, o nome do artista "The Contoso Band". apareceria como ".The Contoso Band".

### <a name="specialized-string-direction-preprocessing"></a>Pré-processamento da direção de cadeia de caracteres especializada

Quando o app contata o servidor em busca de metadados de mídia, ele pré-processa cada cadeia de caracteres antes de exibi-la ao usuário. Durante o pré-processamento, o app também faz uma transformação para tornar a direção do texto consistente. Para fazer isso, ele verifica se há caracteres neutros nas extremidades da cadeia de caracteres. Além disso, se a direção do texto da cadeia de caracteres for oposta à direção do app definida nas configurações de idioma do Windows, ele acrescenta (e às vezes antecede) marcadores de direção Unicode. A função de transformação se parece com esta.

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

Os caracteres Unicode adicionados são de largura igual a zero para que não afetem o espaçamento das cadeias de caracteres. Este código carrega uma penalidade de desempenho em potencial, já que detectar a direção de uma cadeia de caracteres requer o exame da cadeia de caracteres até que um caractere não neutro seja encontrado. Cada caractere que é primeiro verificado para neutralidade é comparado com vários intervalos Unicode, por isso, não é uma verificação trivial.

## <a name="case-study-2-a-bidi-mail-app"></a>Estudo de caso 2: um aplicativo de email bidirecional

### <a name="overview"></a>Visão geral

Em termos de requisitos de layout de interface do usuário, um cliente de email é bastante simples de projetar. O app de Email no Windows é espelhado por padrão. De uma perspectiva de manipulação de texto onde o app de email deve ter uma exibição de texto mais robusta e recursos de composição para acomodar cenários de texto misto.

### <a name="establishing-ui-directionality"></a>Estabelecendo a direcionalidade de interface do usuário

O layout da interface do usuário para o app de Email é espelhado. Os três painéis foram reorientados para que o painel de pastas seja posicionado na borda direita da tela, seguido pelo painel de lista de itens de email à esquerda e depois pelo painel de composição de email.

![App de email espelhado](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Itens adicionais foram reorientados para corresponder ao fluxo geral da interface do usuário e à otimização de toque. Isso inclui a barra de aplicativos e os ícones para redigir, responder e excluir.

![Aplicativo de email espelhado com barra de aplicativos](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Manipulação de texto

#### <a name="ui"></a>Interface do Usuário

O alinhamento de texto na interface do usuário geralmente é à direita. Isso inclui o painel de pastas e o painel de itens. O painel de itens é limitado a duas linhas de texto (Endereço e Título). Isso é importante para reter o alinhamento da direita para a esquerda, sem introduzir um bloco de texto cuja leitura seria difícil quando a direção do conteúdo é oposta ao fluxo de direção da interface do usuário.

#### <a name="text-editing"></a>Edição de texto

A edição de texto requer a capacidade de redigir da direita para a esquerda e da esquerda para a direita. Além disso, o layout da composição deve ser preservado usando um formato adequado, &mdash;como rich text&mdash;, que tem a capacidade de salvar informações de direção.

![App de email da esquerda para a direita](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![App de email da direita para a esquerda](images/56294_BIDI_22_email_orientation_RtL_resized.png)
