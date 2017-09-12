---
author: mijacobs
Description: "Ícones bons se harmonizam com a tipografia e com o restante da linguagem do design. Eles não misturam metáforas e comunicam apenas o que é necessário, com a máxima rapidez e simplicidade possível."
title: "Ícones"
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.openlocfilehash: 225f00a8a0147310938229eff5a5f136e19e406b
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="icons-for-uwp-apps"></a>Ícones para aplicativos UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Ícones bons se harmonizam com a tipografia e com o restante da linguagem do design. Eles não misturam metáforas e comunicam apenas o que é necessário, com a máxima rapidez e simplicidade possível. 

## <a name="linear-scaling-size-ramps"></a>Rampas de tamanho de dimensionamento linear 

<table>
    <tr> 
        <td>16px x 16px</td>
        <td>24px x 24px</td>
        <td>32px x 32px</td>
        <td>48px x 48px</td>
    </tr>
    <tr> 
        <td>![Ícones de 16 x 16 pixels efetivos](images/icons-16x16.png)</td>
        <td>![Ícones de 24 x 24 pixels efetivos](images/icons-24x24.png)</td>
        <td>![Ícones de 32 x 32 pixels efetivos](images/icons-32x32.png)</td>
        <td>![Ícones de 48 x 48 pixels efetivos](images/icons-48x48.png)</td>
    </tr>
</table>

## <a name="common-shapes"></a>Formas comuns

Os ícones geralmente devem maximizar o espaço fornecido com pouco preenchimento. Essas formas fornecem pontos de partida para dimensionamento de formas básicas. 

![Grade de 32px por 32px](images/icons-common-shapes.png)

Use a forma que corresponda à orientação do ícone e componha ao redor desses parâmetros básicos. Os ícones não necessariamente precisam preencher ou se encaixar completamente dentro da forma e podem ser ajustados conforme necessário para garantir um equilíbrio ideal. 

<table class="uwpd-noborder">
    <tr>
        <td>Círculo<td>
        <td>Quadrado</td>
        <td>Triângulo</td>
    </tr>
    <tr>
        <td>![Um círculo](images/icons-common-shapes-examples-1.png)<td>
        <td>![Um quadrado](images/icons-common-shapes-examples-2.png)</td>
        <td>![Um triângulo ](images/icons-common-shapes-examples-3.png)</td>
    </tr>
        <tr>
        <td>Retângulo horizontal<td>
        <td colspan="2">Retângulo vertical</td>        
        </tr>
    <tr>
        <td>![Um retângulo horizontal](images/icons-common-shapes-examples-4.png)<td>
        <td colspan="2">![Um retângulo vertical](images/icons-common-shapes-examples-5.png)</td>
         
    </tr>

</table>

## <a name="angles"></a>Ângulos

Além de usar a mesma grade e peso da linha, os ícones são construídos com elementos comuns. 

Usar somente esses ângulos na criação de formas cria consistência entre todos os nossos ícones e garante que os ícones sejam renderizados corretamente. 

Essas linhas podem ser combinadas, unidas, giradas e refletidas para criar ícones. 

<table>
    <tr>
        <td>**1:1**<br/>45 °</td>
        <td>**1:2**<br />26,57 ° (vertical)<br/>63,43 ° (horizontal)</td>
        <td>**1:3**<br/>18,43 ° (vertical)<br/>71,57° (horizontal)</td>
        <td>**1:4**<br/>14,04° (vertical)<br/>75,96° (horizontal)</td>
    </tr>
    <tr>
        
        <td>![1:1](images/icons-grid-1-1.png)</td>
        <td>![1:2](images/icons-grid-1-2.png)</td>
        <td>![1:3](images/icons-grid-1-3.png)</td>
        <td>![1:4](images/icons-grid-1-4.png)</td>
    </tr>  
</table>

<p>Veja alguns exemplos:</p>

<table>
    <tr>
        <td>![Um exemplo de ângulo de 1:1](images/icons-angles-examples-1.png)</td>
        <td>![Um exemplo de ângulo de 1:2](images/icons-angles-examples-2.png)</td>
        <td>![Um exemplo de ângulo de 1:3](images/icons-angles-examples-3.png)</td>
        <td>![Um exemplo de ângulo de 1:4](images/icons-angles-examples-4.png)</td>
    </tr>
</table>

## <a name="curves"></a>Curvas

Linhas curvas são construídas a partir das seções de um círculo completo e não devem ser inclinadas, a menos que necessário para o ajuste na grade de pixels. 

<table>
    <tr>
        <td>1/4 de círculo</td>
        <td>1/8 de círculo</td>
    </tr>
    <tr>
        <td>![1/4 de círculo](images/icons-curves-14circle.png)</td>
        <td>![1/8 de círculo](images/icons-curves-18circle.png)</td>
    </tr>
    <tr>
        <td>![Exemplo de 1/4 de círculo](images/icons-curves-examples-1.png)</td>
        <td>![Exemplo de 1/8 de círculo](images/icons-curves-examples-2.png)</td>
    </tr>    
</table>

## <a name="geometric-construction"></a>Construção geométrica

É recomendável usar somente formas geométricas puras ao construir ícones.

![Ícone de guitarra com sobreposição geométrica ](images/icons-geometric-construction.png)

## <a name="filled-shapes"></a>Formas preenchidas 

Os ícones podem conter formas preenchidas quando necessário, mas eles não devem ser maiores que 4px em 32px × 32px. Os círculos preenchidos não devem ser maiores que 6px × 6px. 

![Preenchimento de 5px por 8px ](images/icons-filled-shapes.png)

## <a name="badges"></a>Selos

Um "selo" é um termo genérico usado para descrever um elemento adicionado a um ícone que não deve ser integrado com o elemento de ícone base. Elas geralmente transmitem outras partes de informações sobre o ícone como o status ou a ação. Outros termos comuns incluem: sobreposição, anotação ou modificador. 

![Selo de status ](images/icons-badge-status.png)

![Selo de ação ](images/icons-badge-action.png)

Os selos de status utilizam um objeto preenchido colorido que está sobre o ícone, enquanto os selos de ação são integrados ao ícone no mesmo estilo monocromático e peso de linha.

<table>
<tr>
    <td>Selos de status comuns</td>
    <td>Selos de ações comuns</td>
</tr>
<tr>
    <td>![Selo de status ](images/icons-badge-common-states-1.png)</td>
    <td>![Selo de ação ](images/icons-badge-common-states-2.png)</td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>Cor do selo 

Os selos coloridos devem ser usados apenas para transmitir o estado de um ícone. As cores usadas nos selos de status transmitem mensagens emocionais específicas para o usuário. 

<table>
<tr><td>Verde - #128B44</td><td>Azul - #2C71B9</td><td>Amarelo - #FDC214</td></tr>
<tr><td>Positivo: feito, concluído </td><td>Neutro: ajuda, notificação </td><td>Advertência: alerta, aviso </td></tr>
<tr><td>![Status verde](images/icons-color-inbadging-1.png)</td><td>![Status azul](images/icons-color-inbadging-2.png)</td>
<td>![Status amarelo](images/icons-color-inbadging-3.png)</td></tr>
</table>
<p></p>

### <a name="badge-position"></a>Posição do selo

O padrão de posição de qualquer status ou ação é o canto inferior direito. Use as outras posições apenas quando o design não permitir isso. 

### <a name="badge-sizing"></a>Dimensionamento do selo

Os selos devem ser dimensionados com 10 a 18 px em uma grade de 32px x 32px. 

## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes de ativos de bloco e ícone](../controls-and-patterns/tiles-and-notifications-app-assets.md)
