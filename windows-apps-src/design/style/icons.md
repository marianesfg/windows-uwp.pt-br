---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Ícones
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
ms.localizationpriority: medium
ms.openlocfilehash: 61157ad23eb55447137531922ea23fa0120e2b98
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653915"
---
# <a name="icons-for-uwp-apps"></a>Ícones para aplicativos UWP



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
        <td><img src="images/icons-16x16.png" alt="Icons at 16x16 effective pixels" /></td>
        <td><img src="images/icons-24x24.png" alt="Icons at 24x24 effective pixels" /></td>
        <td><img src="images/icons-32x32.png" alt="Icons at 32x32 effective pixels" /></td>
        <td><img src="images/icons-48x48.png" alt="Icons at 48x48 effective pixels" /></td>
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
        <td><img src="images/icons-common-shapes-examples-1.png" alt="A circle" /><td>
        <td><img src="images/icons-common-shapes-examples-2.png" alt="A square" /></td>
        <td><img src="images/icons-common-shapes-examples-3.png" alt="A triangle " /></td>
    </tr>
        <tr>
        <td>Retângulo horizontal<td>
        <td colspan="2">Retângulo vertical</td>        
        </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-4.png" alt="A horizontal rectangle" /><td>
        <td colspan="2"><img src="images/icons-common-shapes-examples-5.png" alt="A vertical rectangle" /></td>
         
    </tr>

</table>

## <a name="angles"></a>Ângulos

Além de usar a mesma grade e peso da linha, os ícones são construídos com elementos comuns. 

Usar somente esses ângulos na criação de formas cria consistência entre todos os nossos ícones e garante que os ícones sejam renderizados corretamente. 

Essas linhas podem ser combinadas, unidas, giradas e refletidas para criar ícones. 

<table>
    <tr>
        <td><b>1:1</b><br/>45 °</td>
        <td><b>1:2</b><br />26,57 ° (vertical)<br/>63,43 ° (horizontal)</td>
        <td><b>1:3</b><br/>18,43 ° (vertical)<br/>71,57° (horizontal)</td>
        <td><b>1:4</b><br/>14,04° (vertical)<br/>75,96 ° (horizontal)</td>
    </tr>
    <tr>
        
        <td><img src="images/icons-grid-1-1.png" alt="1:1" /></td>
        <td><img src="images/icons-grid-1-2.png" alt="1:2" /></td>
        <td><img src="images/icons-grid-1-3.png" alt="1:3" /></td>
        <td><img src="images/icons-grid-1-4.png" alt="1:4" /></td>
    </tr>  
</table>

<p>Veja alguns exemplos:</p>

<table>
    <tr>
        <td><img src="images/icons-angles-examples-1.png" alt="A 1:1 angle example" /></td>
        <td><img src="images/icons-angles-examples-2.png" alt="A 1:2 angle example" /></td>
        <td><img src="images/icons-angles-examples-3.png" alt="A 1:3 angle example" /></td>
        <td><img src="images/icons-angles-examples-4.png" alt="A 1:4 angle example" /></td>
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
        <td><img src="images/icons-curves-14circle.png" alt="1/4 circle" /></td>
        <td><img src="images/icons-curves-18circle.png" alt="1/8 circle" /></td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-examples-1.png" alt="1/4 cirlce example" /></td>
        <td><img src="images/icons-curves-examples-2.png" alt="1/8 circle example" /></td>
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
    <td><img src="images/icons-badge-common-states-1.png" alt="Status badge " /></td>
    <td><img src="images/icons-badge-common-states-2.png" alt="Action badge " /></td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>Cor do selo 

Os selos coloridos devem ser usados apenas para transmitir o estado de um ícone. As cores usadas nos selos de status transmitem mensagens emocionais específicas para o usuário. 

<table>
<tr><td>Verde - #128B44</td><td>Azul - #2C71B9</td><td>Amarelo - #FDC214</td></tr>
<tr><td>Positivo: feito, concluído </td><td>Neutro: ajuda, notificação </td><td>Advertência: alerta, aviso </td></tr>
<tr><td><img src="images/icons-color-inbadging-1.png" alt="Green status" /></td><td><img src="images/icons-color-inbadging-2.png" alt="Blue status" /></td>
<td><img src="images/icons-color-inbadging-3.png" alt="Yellow status" /></td></tr>
</table>
<p></p>

### <a name="badge-position"></a>Posição do selo

O padrão de posição de qualquer status ou ação é o canto inferior direito. Use as outras posições apenas quando o design não permitir isso. 

### <a name="badge-sizing"></a>Dimensionamento do selo

Os selos devem ser dimensionados com 10 a 18 px em uma grade de 32px x 32px. 

## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes de ativos de bloco e ícone](../shell/tiles-and-notifications/app-assets.md)
