---
Description: Use o controle ParallaxView para adicionar profundidade e movimento ao seu aplicativo.
title: Use da Parallax para adicionar profundidade e movimentação ao seu aplicativo.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d659683d6871d9d48fd17b73c74477e7bd03e258
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970491"
---
# <a name="parallax"></a>Paralaxe

Paralaxe é um efeito visual onde os itens mais próximos ao visualizador se movem mais rápido do que itens no plano de fundo. A Paralaxe cria uma sensação de profundidade, perspectiva e movimento. Em um aplicativo UWP, você pode usar o controle ParallaxView para criar um efeito de paralaxe.  

> **APIs da biblioteca da interface do usuário do Windows:** [classe ParallaxView](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [Propriedade VerticalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [Propriedade HorizontalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **APIs de plataforma**: [classe ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [Propriedade VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [Propriedade HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ParallaxView">abrir o aplicativo e ver o ParallaxView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Paralaxe e o Sistema de Design Fluent

 O Sistema de Design Fluente ajuda a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. Paralaxe é um componente do Sistema de Design Fluent que acrescenta movimento, profundidade e escala ao seu aplicativo. Para saber mais, confira a [visão geral do design fluente](/windows/apps/fluent-design-system).

## <a name="how-it-works-in-a-user-interface"></a>Como ele funciona em uma interface de usuário

Em uma interface de usuário, você pode criar um efeito paralaxe movendo objetos diferentes em diferentes taxas quando a interface de usuário é rolada ou faz um movimento panorâmico. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Para demonstrar, vamos examinar duas camadas de conteúdo, uma lista e uma imagem de plano de fundo.  A lista é colocada sobre a imagem de fundo, o que já gera a ilusão de que a lista está mais perto para o visualizador.  Agora, para obter o efeito da Parallax, queremos que o objeto mais próximo de nós viaje "mais rápido" do que o objeto que está mais distante.  Conforme o usuário rola a interface, a lista se move a uma taxa mais rápida que a imagem de fundo, o que cria a ilusão de profundidade.

 ![Um exemplo de paralaxe com uma lista e uma imagem de fundo](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Usando o controle ParallaxView para criar um efeito paralaxe

Para criar um efeito paralaxe, use o controle [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview). Esse controle vincula a posição de rolagem de um elemento do primeiro plano, como uma lista, a um elemento de fundo, como uma imagem. Enquanto você navega através do elemento do primeiro plano, ele anima o elemento de fundo para criar um efeito paralaxe. 

Para usar o controle ParallaxView, você fornece um elemento de origem, um elemento de fundo e define as propriedades [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (para rolagem vertical) e/ou [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (para rolagem horizontal) como um valor maior que zero. 
* A propriedade de Origem recebe uma referência do elemento do primeiro plano. Para que o efeito paralaxe ocorra, o primeiro plano deve ser um [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) ou um elemento que contenha um ScrollViewer, como uma [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) ou uma [RichTextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Para definir o elemento de fundo, adicione esse elemento como um filho do controle ParallaxView. O elemento de fundo pode ser qualquer [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), como uma [Imagem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) ou um painel que contém outros elementos de interface do usuário. 

Para criar um efeito paralaxe, o ParallaxView deve estar atrás do elemento do primeiro plano. Os painéis [Grade](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) e [Tela](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) permitem que você coloque itens um sobre o outro, para que eles funcionem bem com o controle ParallaxView.  

Este exemplo cria um efeito paralaxe para uma lista:
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

O ParallaxView ajusta automaticamente o tamanho da imagem para que ela funcione para a operação da Parallax, de modo que você não precisa se preocupar com a rolagem da imagem da exibição.

## <a name="customizing-the-parallax-effect"></a>Personalizando o efeito paralaxe 

As propriedades VerticalShift e HorizontalShift permitem controlar o nível do efeito paralaxe.

* A propriedade VerticalShift especifica o quanto queremos que a tela de fundo se desloque verticalmente durante toda a operação de paralaxe. Um valor de 0 significa que o plano de fundo não se move.
* A propriedade HorizontalShift especifica o quanto queremos que a tela de fundo se desloque horizontalmente durante toda a operação de paralaxe. Um valor de 0 significa que o plano de fundo não se move.

Valores maiores criam um efeito mais dramático. 

Para obter uma lista completa das maneiras de personalizar a paralaxe, consulte a classe ParallaxView. 

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Use a paralaxe em listas com uma imagem de fundo
- Considere usar a paralaxe em ListViewItems quando ListViewItems contêm uma imagem
- Não use em todo lugar, o uso em excesso pode diminuir seu impacto

## <a name="related-articles"></a>Artigos relacionados

- [Classe ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Design Fluente para UWP](/windows/apps/fluent-design-system)
- [Ciência no sistema: Design Fluente e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
