---
author: mijacobs
Description: "Ícones bons se harmonizam com a tipografia e com o restante da linguagem do design. Eles não misturam metáforas e comunicam apenas o que é necessário, com a máxima rapidez e simplicidade possível."
title: "Ícones"
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: e5e601bf3ff9d0b1518c86130a5b0fefbb86a773

---

# Ícones para aplicativos UWP

Ícones bons se harmonizam com a tipografia e com o restante da linguagem do design. Eles não misturam metáforas e comunicam apenas o que é necessário, com a máxima rapidez e simplicidade possível. 

## Rampas de tamanho de dimensionamento linear 

<table>
    <tr> 
        <td>16px x 16px</td>
        <td>24px x 24px</td>
        <td>32px x 32px</td>
        <td>48px x 48px</td>
    </tr>
    <tr> 
        <td>![Icons at 16x16 effective pixels](images/icons-16x16.png)</td>
        <td>![Icons at 24x24 effective pixels](images/icons-24x24.png)</td>
        <td>![Icons at 32x32 effective pixels](images/icons-32x32.png)</td>
        <td>![Icons at 48x48 effective pixels](images/icons-48x48.png)</td>
    </tr>
</table>

## Formas comuns

Os ícones geralmente devem maximizar o espaço fornecido com pouco preenchimento. Essas formas fornecem pontos de partida para dimensionamento de formas básicas. 

![Grade de 32px por 32px](images/icons-common-shapes.png)

Use a forma que corresponda à orientação do ícone e componha ao redor desses parâmetros básicos. Os ícones não necessariamente precisam preencher ou se encaixar completamente dentro da forma e podem ser ajustados conforme necessário para garantir um equilíbrio ideal. 

<table>
    <tr>
        <td>Círculo<td>
        <td>Quadrado</td>
        <td>Triângulo</td>
    </tr>
    <tr>
        <td>![A circle](images/icons-common-shapes-examples-1.png)<td>
        <td>![A square](images/icons-common-shapes-examples-2.png)</td>
        <td>![A triangle ](images/icons-common-shapes-examples-3.png)</td>
    </tr>
        <tr>
        <td>Retângulo horizontal<td>
        <td colspan="2">Retângulo vertical</td>        
        </tr>
    <tr>
        <td>![A horizontal rectangle](images/icons-common-shapes-examples-4.png)<td>
        <td colspan="2">![A vertical rectangle](images/icons-common-shapes-examples-5.png)</td>
         
    </tr>

</table>

## Ângulos

Além de usar a mesma grade e peso da linha, os ícones são construídos com elementos comuns. 

Usar somente esses ângulos na criação de formas cria consistência entre todos os nossos ícones e garante que os ícones sejam renderizados corretamente. 

Essas linhas podem ser combinadas, unidas, giradas e refletidas para criar ícones. 

<table>
    <tr>
        <td>**1:1**<br/>45 °</td>
        <td>**1:2**<br />26,57 ° (vertical)<br/>63,43 ° (horizontal)</td>
        <td>**1:3**<br/>18,43 ° (vertical)<br/>71,57 ° (horizontal)</td>
        <td>**1:4**<br/>14,04 ° (vertical)<br/>75,96 ° (horizontal)</td>
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
        <td>![A 1:1 angle example](images/icons-angles-examples-1.png)</td>
        <td>![A 1:2 angle example](images/icons-angles-examples-2.png)</td>
        <td>![A 1:3 angle example](images/icons-angles-examples-3.png)</td>
        <td>![A 1:4 angle example](images/icons-angles-examples-4.png)</td>
    </tr>
</table>

## Curvas

Linhas curvas são construídas a partir das seções de um círculo completo e não devem ser inclinadas, a menos que necessário para o ajuste na grade de pixels. 

<table>
    <tr>
        <td>1/4 de círculo</td>
        <td>1/8 de círculo</td>
    </tr>
    <tr>
        <td>![1/4 circle](images/icons-curves-14circle.png)</td>
        <td>![1/8 circle](images/icons-curves-18circle.png)</td>
    </tr>
    <tr>
        <td>![1/4 cirlce example](images/icons-curves-examples-1.png)</td>
        <td>![1/8 circle example](images/icons-curves-examples-2.png)</td>
    </tr>    
</table>

## Construção geométrica

É recomendável usar somente formas geométricas puras ao construir ícones.

![Ícone de guitarra com sobreposição geométrica ](images/icons-geometric-construction.png)

## Formas preenchidas 

Os ícones podem conter formas preenchidas quando necessário, mas eles não devem ser maiores que 4px em 32px × 32px. Os círculos preenchidos não devem ser maiores que 6px × 6px. 

![Preenchimento de 5px por 8px ](images/icons-filled-shapes.png)

## Selos

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
    <td>![Status badge ](images/icons-badge-common-states-1.png)</td>
    <td>![Action badge ](images/icons-badge-common-states-2.png)</td>
</tr>
</table>
<p></p>

### Cor do selo 

Os selos coloridos devem ser usados apenas para transmitir o estado de um ícone. As cores usadas nos selos de status transmitem mensagens emocionais específicas para o usuário. 

<table>
<tr><td>Verde - #128B44</td><td>Azul - #2C71B9</td><td>Amarelo - #FDC214</td></tr>
<tr><td>Positivo: feito, concluído </td><td>Neutro: ajuda, notificação </td><td>Advertência: alerta, aviso </td></tr>
<tr><td>![Green status](images/icons-color-inbadging-1.png)</td><td>![Blue status](images/icons-color-inbadging-2.png)</td>
<td>![Yellow status](images/icons-color-inbadging-3.png)</td></tr>
</table>
<p></p>

### Posição do selo

O padrão de posição de qualquer status ou ação é o canto inferior direito. Use as outras posições apenas quando o design não permitir isso. 

### Dimensionamento do selo

Os selos devem ser dimensionados com 10 a 18 px em uma grade de 32px x 32px. 

## Artigos relacionados

* [Diretrizes de ativos de bloco e ícone](../controls-and-patterns/tiles-and-notifications-app-assets.md)



<!--HONumber=Jun16_HO4-->


