---
author: Karl-Bridge-Microsoft
Description: "Crie aplicativos para a UWP (Plataforma Universal do Windows) com a experiência de interação exclusiva e diferenciada otimizada para toque, mas funcionalmente consistente com todos os tipos de dispositivos de entrada."
title: Diretrizes de design de toque
ms.assetid: 3250F729-4FDD-4AD4-B856-B8BA575C3375
label: Touch design guidelines
template: detail.hbs
redirect_url: https://msdn.microsoft.com/windows/uwp/input-and-devices/touch-interactions
ms.openlocfilehash: 28dfadf6010aed3fb2ed0d03b73f92631c17fcf4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="touch-design-guidelines"></a>Diretrizes de design de toque





Crie aplicativos para a UWP (Plataforma Universal do Windows) com a experiência de interação exclusiva e diferenciada otimizada para toque, mas funcionalmente consistente com todos os tipos de dispositivos de entrada.

## <a name="span-iddosanddontsspanspan-iddosanddontsspanspan-iddosanddontsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>O que fazer e o que não fazer


-   Aplicativos de design com interação por toque como o método de entrada primário esperado.
-   Fornecer feedback visual para interações de todos os tipos (toque, caneta, stylus, mouse etc.)
-   Otimize alvos ajustando tamanho do alvo de toque, geometria de contato, esfregar e balançar.
-   Otimize a acuidade por meio do uso de pontos de ajuste e "trilhos" direcionais.
-   Forneça dicas de ferramenta e manipuladores para auxiliar a melhorar a acuidade de toque de itens da interface do usuário com pouco espaçamento.
-   Não use interações cronometradas sempre que possível (exemplos de uso apropriado: tocar e segurar).
-   Não use o número de dedos usados para distinguir a manipulação sempre que possível.

## <a name="span-idadditionalusageguidancespanspan-idadditionalusageguidancespanspan-idadditionalusageguidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Diretrizes de uso adicional


Primeiro e mais importante, projete o seu aplicativo com a expectativa de que o toque será o método de entrada principal dos usuários. Se você usar os controles da plataforma, o suporte para touchpad, mouse e caneta não exigirá qualquer programação adicional, pois o Windows 8 o fornece gratuitamente.

Tenha em mente, porém, que uma interface do usuário otimizada para toque nem sempre é superior a uma interface do usuário tradicional. Ambas oferecem vantagens e desvantagens exclusivas de tecnologia e aplicativo. Na mudança para uma interface do usuário touch-first, é importante entender as diferenças essenciais entre a entrada por toque (incluindo touchpad), caneta, mouse e teclado. Não considere como certas e consumadas as propriedades de dispositivos de entrada tradicionais, pois o recurso de toque no Windows 8 é muito mais que uma simples emulação dessa funcionalidade.

Você encontrará ao longo das diretrizes a informação de que a entrada por toque requer uma abordagem diferente para o design IU.

**Compare os requisitos de interação por toque**

A tabela a seguir mostra algumas das diferenças entre os dispositivos de entrada, as quais você deve considerar ao projetar aplicativos da Windows Store otimizados para toque.

Interações de toque de fator Interações do mouse, teclado e caneta Precisão do toque A área de contato da ponta do dedo é maior do que uma coordenada x-y única, o que aumenta a probabilidade de ativações de comando acidentais.
O mouse e a caneta/stylus fornecem uma coordenada x-y precisa.
Igual ao mouse.
O formato da área de contato altera ao longo do movimento.
Os movimentos do mouse e traços da caneta/stylus fornecem uma coordenada x-y precisa. O foco do teclado é explícito.
Igual ao mouse.
Não há cursor do mouse para ajudar no direcionamento.
O cursor do mouse, o cursor da caneta/stylus e o foco do teclado, todos ajudarão no direcionamento.
Igual ao mouse.
Anatomia humana Os movimentos com as pontas dos dedos são imprecisos porque dificultam um movimento em linha reta com um ou mais dedos. Isso se deve à curvatura das articulações das mãos e ao número de articulações envolvidas no movimento.
É fácil realizar um movimento em linha reta com o mouse ou caneta/stylus porque a mão que os controla percorre uma distância física menor do que o cursor na tela.
Igual ao mouse.
Algumas áreas na superfície de toque de um dispositivo de vídeo podem ser inacessíveis devido à postura do dedo e à posição do punho do usuário sobre o dispositivo.
O mouse e a caneta podem atingir qualquer parte da tela, embora qualquer controle deva ser acessado pelo teclado por meio da ordem de tabulação.
A postura do dedo e a alça podem ser um problema.
Os objetos podem ser obscurecidos por uma ou mais pontas de dedos ou pela mão do usuário. Isso é conhecido como oclusão.
Os dispositivos de entrada indireta não causam oclusão.
Igual ao mouse.
Estado do objeto O toque usa um modelo de dois estados: a superfície de toque de um dispositivo de vídeo é tocada (on) ou não (off). Não há estado de foco que possa disparar comentários visuais adicionais.
Um mouse, caneta/stylus e teclado, todos expõem um modelo de três estados: acima (off), abaixo (on) (ativado), e focalizar (foco).

A focalização permite que os usuários explorem e aprendam usando as dicas de ferramentas associadas aos elementos da interface do usuário. Os efeitos de focalização e de foco podem retransmitir os objetos que são interativos e também ajudar no direcionamento.

Igual ao mouse.
Interação sofisticada Dá suporte a multitoque, pontos de entrada múltiplos (pontas dos dedos) em uma superfície de toque.
Suporta um ponto de entrada único.
Igual ao toque.
Suporta a manipulação direta de objetos através de gestos como tocar, arrastar, deslizar, apertar e girar.
Sem suporte para manipulação direta quando o mouse, caneta e teclado são dispositivos de entrada indiretos.
Igual ao mouse.
 

**Observação**  
A entrada indireta conta com os benefícios de mais de 25 anos de refinamento. Recursos como dicas de ferramentas disparadas por focalização foram projetadas para solucionar a exploração da interface do usuário especificamente para entradas por touchpad, mouse, caneta e teclado. Elementos da interface do usuário como esses foram reprojetados para uma experiência avançada fornecida pela entrada de toque, sem comprometer a experiência do usuário para outros dispositivos.

 

**Usar comentário por toque**

O feedback visual apropriado durante as interações com o aplicativo ajudam os usuários a reconhecer, aprender e adaptar a forma como suas interações são interpretadas pelo aplicativo e pelo Windows 8. O feedback visual pode indicar interações bem-sucedidas, retransmitir o status do sistema, aprimorar o sentido de controle, reduzir erros, ajudar os usuários a entenderem o sistema e os dispositivos de entrada, além de estimular a interação.

A resposta visual é importante quando o usuário recorre à entrada por toque em atividades que exigem exatidão e precisão com base no local. A exibição do feedback sempre que a entrada por toque for detectada ajudará o usuário a entender as regras de direcionamento personalizadas definidas pelo aplicativo e seus respectivos controles.

**Criar uma experiência de interação imersiva**

As técnicas a seguir aprimoram a experiência imersiva de aplicativos da Windows Store.

**Direcionamento**

O direcionamento é otimizado por meio de:

-   Tamanhos de destino de toque

    Diretrizes claras sobre tamanho garantem que os aplicativos proporcionem uma interface do usuário agradável com objetos e controles de direcionamento fácil e seguro.

-   Geometria de contato

    Toda a área de contato do dedo determina o objeto de destino mais provável.

-   Anulação

    Os itens em um grupo são redirecionados com facilidade arrastando o dedo entre eles (por exemplo, botões de opção). O item atual é ativado quando o toque é liberado.

-   Balanço

    Itens empacotados densamente (por exemplo, hiperlinks) são redirecionados com facilidade ao pressionar o dedo e, sem deslizar, balançá-lo para frente e para trás sobre os itens. Devido à oclusão, o item atual é identificado por uma dica de ferramenta ou pela barra de status e é ativado quando o toque é liberado.

**Precisão**

Crie interações soltas usando:

-   Pontos de alinhamento que facilitam a parada nos locais desejados quando os usuários interagem com o conteúdo.
-   "Trilhos" direcionais que ajudam no movimento panorâmico vertical ou horizontal, mesmo quando a mão se desloca em um leve arco. Para saber mais, veja [Diretrizes para movimento panorâmico](guidelines-for-panning.md).

**Oclusão**

Para evitar a oclusão de dedo e mão:

-   Dimensione e posicione a interface do usuário.

    Deixe os elementos da interface do usuário grandes o suficiente para que não sejam completamente cobertos por uma área de contato da ponta do dedo.

    Posicione os menus e pop-ups sobre a área de contato sempre que possível.

-   Dicas de ferramenta

    Mostre dicas de ferramentas quando um usuário mantém o dedo em contato com um objeto. Isso é útil para descrever a funcionalidade do objeto. O usuário pode retirar a ponta do dedo para sair do objeto e assim evitar a invocação da dica de ferramenta.

    Para objetos pequenos, desloque as dicas de ferramenta para que não fiquem cobertas pela área de contato da ponta do dedo. Isso é útil para direcionamento.

-   Alças para precisão

    Quando a precisão é necessária (por exemplo, seleção de texto), inclua alças de seleção que sejam deslocadas para aumentar a precisão. Para saber mais, veja [Diretrizes para seleção de texto e imagens (aplicativos do Tempo de Execução do Windows)](guidelines-for-textselection.md).

**Tempo**

Evite mudanças no modo temporizado para permitir manipulação direta. A manipulação direta simula o tratamento físico direto e em tempo real de um objeto. O objeto responde assim que o usuário move os dedos.

Uma interação em tempo limite, por outro lado, ocorre após uma interação por toque. As interações em tempo limite dependem normalmente de limites invisíveis como tempo, distância ou velocidade para determinar o comando a ser realizado. As interações em tempo limite não apresentam nenhum comentário visual até que o sistema execute a ação.

A manipulação direta oferece vários benefícios sobre as interações com tempo limite:

-   A resposta visual instantânea durante as interações faz com que os usuários se sintam conectados, confiantes e no controle.
-   As manipulações diretas tornam mais seguro explorar um sistema, pois são reversíveis: os usuários podem reverter facilmente suas ações de maneira lógica e intuitiva.
-   As interações que afetam diretamente os objetos e imitam as interações reais são mais intuitivas, passíveis de descoberta e memorizáveis. Elas não dependem de interações obscuras ou abstratas.
-   A interações em tempo limite podem ser difíceis de serem executadas, pois os usuários podem atingir limites arbitrários e invisíveis.

Além disso, as dicas a seguir são altamente recomendadas:

-   As manipulações não devem ser diferenciadas pelo número de dedos usados.
-   As interações devem permitir interações combinadas. Por exemplo, pinçar para aplicar zoom e, ao mesmo tempo, arrastar os dedos para fazer movimento panorâmico.
-   As interações não devem ser diferenciadas por tempo. A mesma interação deve ter o mesmo resultado, independentemente do tempo que leva para realizá-la. Ativações baseadas em tempo geram atrasos obrigatórios para os usuários e fogem de sua natureza imersiva de manipulação direta e percepção da capacidade de resposta do sistema.

    **Observação**  Uma exceção a isso ocorre quando você usa interações com tempo limite específicas para ajudar no aprendizado e na exploração (por exemplo, pressionar e segurar).

     

-   Descrições e indicações visuais adequadas têm um grande efeito sobre o uso das interações avançadas.

## <a name="span-idrelatedtopicsspanrelated-articles"></a><span id="related_topics"></span>Artigos relacionados

**Para desenvolvedores (XAML)**
* [Interações por toque](https://msdn.microsoft.com/library/windows/apps/mt185617)
* [Interações personalizadas do usuário](https://msdn.microsoft.com/library/windows/apps/mt185599)
 

 




