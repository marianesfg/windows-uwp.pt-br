---
author: Karl-Bridge-Microsoft
Description: "Este tópico descreve o uso da geometria de contato por área de toque e fornece as práticas recomendadas de direcionamento em aplicativos do Windows Runtime."
title: "Seleção por área touch"
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 50a285b484f7e9ed7b349921c3460bd7c9c81603

---

# Diretrizes de direcionamento

A seleção por área touch no Windows usa a área de contato total de cada dedo detectado por um digitalizador de toque. O conjunto maior e mais complexo de dados de entrada relatados pelo digitalizador é usado para aumentar a precisão ao determinar o destino desejado (ou mais provável) pelo usuário.

**APIs importantes**

-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)



Este tópico descreve o uso da geometria de contato para seleção por área de toque e fornece as práticas recomendadas de direcionamento em aplicativos UWP.

## Medidas e dimensionamento


Para manter-se consistente entre diferentes tamanhos de tela e densidades de pixels, todos os tamanhos desejados são apresentados em unidades físicas (milímetros). As unidades físicas podem ser convertidas para pixels ao usar a seguinte equação:

Pixels = densidade de pixels × medida

O exemplo a seguir usa essa fórmula para calcular o tamanho do pixel de um destino de 9 mm em uma exibição de 135 PPI (pixels por polegada) em um nível de ajuste predefinido de 1x:

Pixels = 135 PPI × 9 mm

Pixels = 135 PPI × (0,03937 polegadas por mm × 9 mm)

Pixels = 135 PPI × 0,35433 polegadas

Pixels = 48 pixels

Esse resultado deve ser ajustado de acordo com cada nível de ajuste predefinido pelo sistema.

## Limites


Os limites de distância e tempo podem ser usados para determinar o resultado de uma interação.

Por exemplo, quando um toque é detectado, ele é registrado se o objeto for arrastado em menos de 2,7 mm do ponto de toque e o dedo for levantado em 0,1 segundo ou menos depois do toque. Mover o dedo além desse limite de 2,7 mm faz com que o objeto seja arrastado e selecionado ou movido (para saber mais, veja [Diretrizes de deslizamento transversal](guidelines-for-cross-slide.md)). Dependendo do seu aplicativo, segurar o dedo por mais de 0,1 segundo pode fazer com que o sistema faça uma interação de autorrevelação (para saber mais, veja [Diretrizes de comentários visuais](guidelines-for-visualfeedback.md#selfreveal)).

## Tamanhos do alvo


Em geral, defina o tamanho do alvo de toque como 9 mm ou maior (48x48 pixels em uma tela de 135 PPI em um nível de ajuste predefinido de 1,0x). Evite usar alvos de toque que tenham menos de 7 mm.

O diagrama a seguir mostra como o tamanho do destino normalmente é uma combinação do destino visual, do tamanho do destino real e de qualquer área de preenchimento entre o destino real e outros destinos possíveis.

![diagrama mostrando os tamanhos recomendados para o destino visual, o destino real e o preenchimento.](images/targeting-size.png)

A tabela a seguir lista os tamanhos mínimos e recomendados para os componentes de um alvo de toque.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Componente alvo</th>
<th align="left">Tamanho mínimo</th>
<th align="left">Tamanho recomendado</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Preenchimento</td>
<td align="left">2 mm</td>
<td align="left">Não aplicável.</td>
</tr>
<tr class="even">
<td align="left">Tamanho visual do destino</td>
<td align="left">&lt; 60% do tamanho real</td>
<td align="left">90-100% do tamanho real
<p>A maioria dos usuários não vai perceber que um destino visual pode ser tocado, se ele for menor que 4,2 mm quadrados (60% do tamanho do destino mínimo recomendado de 7 mm).</p></td>
</tr>
<tr class="odd">
<td align="left">Tamanho real do alvo</td>
<td align="left">7 mm</td>
<td align="left">Maior ou igual a 9 mm (48 x 48 px em 1x)</td>
</tr>
<tr class="even">
<td align="left">Tamanho total do alvo</td>
<td align="left">11 x 11 mm (aproximadamente 60 px: três unidades de grade de 20 px em 1x)</td>
<td align="left">13,5 x 13,5 mm (72 x 72 px em 1x)
<p>Isto implica que o tamanho do destino real e o preenchimento combinados devem ser maiores que os seus mínimos respectivos.</p></td>
</tr>
</tbody>
</table>

 

Essas recomendações de tamanho de destino podem ser ajustadas de acordo com determinado cenário. Algumas das considerações incluídas nessas recomendações são:

-   Frequência de toques: considere tornar os destinos pressionados repetidas vezes ou com frequência maiores que o tamanho mínimo.
-   Consequência do erro: destinos com consequências graves quando tocados por engano devem ter preenchimento maior e ser colocados mais distantes da extremidade da área de conteúdo. Isso vale principalmente para destinos que são tocados com frequência.
-   Posição na área de conteúdo
-   Fator forma e tamanho da tela
-   Postura do dedo
-   Visualizações por toque
-   Hardware e digitalizadores de toque

## Assistência de direcionamento


O Windows oferece assistência de direcionamento para suportar cenários onde as recomendações de tamanho ou de preenchimento mínimos apresentados aqui não são aplicáveis, por exemplo, hiperlinks em uma página da Web, controles de calendário, listas suspensas e caixas de combinação ou seleção de texto.

Essas melhorias na plataforma de direcionamento e os comportamentos da interface do usuário funcionam junto com a resposta visual (interface do usuário de desambiguização) para aumentar a precisão e a confiança do usuário. Para obter mais informações, consulte [Diretrizes de resposta visual](guidelines-for-visualfeedback.md).

Se um elemento tocável deve ser menor que o tamanho mínimo de alvo recomendado, as seguintes técnicas podem ser usadas para minimizar os problemas de direcionamento resultantes.

## Conector


O compartilhamento é uma indicação visual (um conector de um ponto de contato até o retângulo delimitador de um objeto) usada para indicar ao usuário que ele está conectado e interagindo com um objeto, mesmo que o contato de entrada não esteja diretamente em contato com o objeto. Isso pode ocorrer quando:

-   Um contato de toque foi detectado primeiro dentro de algum limite de proximidade com um objeto, e esse objeto foi identificado como o destino mais provável do contato.
-   Um contato de toque foi movido para fora de um objeto, mas ainda está dentro do limite de proximidade.

Esse recurso não está para exposto para desenvolvedores de aplicativos da Windows Store que usam o JavaScript.

## Esfregar


Esfregar significa tocar em qualquer lugar dentro de um campo de alvos e deslizar para selecionar o alvo desejado sem tirar o dedo até que esteja sobre o alvo desejado. Isso também é conhecido como "ativação sem largar", onde o objeto que é ativado é aquele que foi tocado por último quando o dedo foi suspenso da tela.

Siga estas diretrizes para criar interações de esfregar:

-   Esfregar é usado em conjunto com a IU de desambiguidade. Para obter mais informações, consulte [Diretrizes de resposta visual](guidelines-for-visualfeedback.md).
-   O tamanho mínimo recomendado para o destino de toque de esfregar é de 20 px (3,75 mm em 1x).
-   Esfregar tem prioridade quando realizada em uma área de movimento panorâmico, como uma página da Web.
-   Os destinos de esfregar devem ficar juntos.
-   Uma ação é cancelada quando o usuário arrasta o dedo para fora do destino de esfregar.
-   O conector de um destino de esfregar é especificado quando as ações realizadas pelo destino não são destrutivas, como alternar entre datas no calendário.
-   O conector é especificado em uma única direção, horizontal ou vertical.

## Artigos relacionados


**Exemplos**
* [Amostra de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemplo do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo-morto**
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de teste de toque](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: amostra de gestos no Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: amostra de manipulações e gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Amostra de entrada por toque do DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 







<!--HONumber=Jun16_HO5-->


