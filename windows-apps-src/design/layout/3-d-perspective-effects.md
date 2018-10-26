---
author: Jwmsft
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: Efeitos da perspectiva 3D na interface do usuário XAML
description: Você pode aplicar efeitos 3D ao conteúdo em seus aplicativos do Windows Runtime usando transformações de perspectiva. Por exemplo, você pode criar a ilusão de que um objeto está sendo girado em sua direção ou para longe de você, como mostrado abaixo.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0b50ca4cb3f74c7c8e8bccb4fceb58e0151bcf2
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5543071"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>Efeitos da perspectiva 3D na interface do usuário XAML


Você pode aplicar efeitos 3D ao conteúdo em seus aplicativos do Windows Runtime usando transformações de perspectiva. Por exemplo, você pode criar a ilusão de que um objeto está sendo girado em sua direção ou para longe de você, como mostrado abaixo.

![Imagem com transformação de perspectiva](images/3dsimple.png)

Outro uso comum para transformações de perspectiva é organizar objetos em relação uns aos outros para criar um efeito 3D, como visto aqui.

![Empilhando objetos para criar um efeito 3D](images/3dstacking.png)

Além de criar efeitos 3D estáticos, você pode animar as propriedades da transformação de perspectiva para criar efeitos 3D em movimento.

Você acabou de ver transformações de perspectiva aplicadas a imagens, mas você pode aplicar esses efeitos a qualquer [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911), incluindo controles. Por exemplo, você pode aplicar um efeito 3D a um contêiner de controles inteiro, assim:

![Efeito 3D aplicado a um contêiner de elementos](images/skewedstackpanel.png)

Aqui está o código XAML usado para criar esta amostra:

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

Aqui, focamos nas propriedades do [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192), usado para girar e mover objetos em um espaço 3D. A próxima amostra permite que você teste essas propriedades e veja o efeito em um objeto.

## <a name="planeprojection-class"></a>Classe PlaneProjection

Você pode aplicar efeitos 3D a qualquer [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911), configurando a propriedade [**Projeção**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection) do UIElement usando um [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192). O **PlaneProjection** define como a transformação é renderizada no espaço. O próximo exemplo mostra um caso simples.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

Esta figura mostra como a imagem é renderizada. O eixo x, o eixo y e o eixo z são mostrados como linhas vermelhas. A imagem é girada 35 graus para trás em torno do eixo x usando a propriedade [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx).

![RotateX menos 35 graus.](images/3drotatexminus35.png)

A propriedade [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) gira em torno do eixo y do centro de rotação.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY menos 35 graus](images/3drotateyminus35.png)

A propriedade [**RotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationz) gira em torno do eixo z do centro de rotação (uma linha perpendicular ao plano do objeto).

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ menos 45 graus](images/3drotatezminus35.png)

As propriedades de rotação podem especificar um valor positivo ou negativo a ser girado em qualquer direção. O número absoluto pode ser maior que 360, que gira o objeto mais de uma rotação completa.

Você pode mover o centro de rotação usando as propriedades [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx), [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) e [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz). Por padrão, os eixos de rotação são executados diretamente através do centro do objeto, fazendo com que o mesmo gire ao redor do próprio centro. Mas se você mover os centros de rotação para a borda externa do objeto, ele girará em torno daquela borda. Os valores padrão para **CenterOfRotationX** e **CenterOfRotationY** são 0,5 e o valor padrão para **CenterOfRotationZ** é 0. Para **CenterOfRotationX** e **CenterOfRotationY** os valores entre 0 e 1 definem o ponto dinâmico em um local dentro do objeto. O valor 0 representa uma borda de objeto e o valor 1 representa a borda oposta. Valores fora desse intervalo são permitidos e moverão o centro da rotação de forma apropriada. Pelo eixo z do centro de rotação ser desenhado através do plano do objeto, você pode mover o centro de rotação para trás do objeto usando um número negativo e para a frente do objeto (em direção a você) usando um número positivo.

[**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) move o centro de rotação ao longo do eixo x, paralelo ao objeto, enquanto [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) move o centro de rotação ao longo do eixo y do objeto. As próximas ilustrações demonstram o uso de valores diferentes para **CenterOfRotationY**.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0,5" (padrão)**

![CenterOfRotationY igual a 0,5](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0,1"**

![CenterOfRotationY igual a 0,1](images/3dcenterofrotationy0point1.png)

Perceba como a imagem gira em torno do centro quando a propriedade [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) está definida para o valor padrão de 0,5 e gira próximo à borda superior quando está definida para 0,1. Pode-se ver um comportamento semelhante ao mudar a propriedade [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) para mover-se para onde a propriedade [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) gira o objeto.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0,5" (padrão)**

![CenterOfRotationX igual a 0,5](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0,9" (borda direita)**

![CenterOfRotationX igual a 0,9](images/3dcenterofrotationx0point9.png)

Usa o [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz) para posicionar o centro de rotação acima ou abaixo do plano do objeto. Dessa forma, você pode girar o objeto em torno do ponto de forma análoga a um planeta que orbita em torno de uma estrela.

## <a name="positioning-an-object"></a>Posicionando um objeto

Até agora, você aprendeu como girar um objeto no espaço. Você pode posicionar estes objetos girados no espaço em relação um ao outro usando estas propriedades:

-   [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx) move um objeto ao longo do eixo x do plano de um objeto rotacionado.
-   [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) move um objeto ao longo do eixo y do plano de um objeto rotacionado.
-   [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) move um objeto ao longo do eixo z do plano de um objeto rotacionado.
-   [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) move um objeto ao longo do eixo x alinhado à tela.
-   [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) move um objeto ao longo do eixo y alinhado à tela.
-   [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) move um objeto ao longo do eixo z alinhado à tela.

**Deslocamento local**

As propriedades [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx), [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) e [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) traduzem um objeto ao longo do respectivo eixo do plano do objeto depois de ele ter sido girado. Portanto, a rotação do objeto determina a direção que em que o objeto está traduzido. Para demonstrar este conceito, a próxima amostra anima o **LocalOffsetX** de 0 a 400 e o [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) de 0 a 65 graus.

Perceba que na amostra anterior o objeto está se movendo ao longo do seu próprio eixo x. Bem no início da animação, quando o valor [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) está próximo a zero (paralelo à tela), o objeto se move ao longo da tela na direção x, mas conforme ele gira em direção a você, ele se move ao longo do eixo x do plano do objeto na sua direção. Por outro lado, se você tivesse animado a propriedade **RotationY** para -65 graus, o objeto se curvaria na sua direção oposta.

[**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) funciona de maneira semelhante ao [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx), exceto por se mover ao longo dos eixos verticais. Então, mudar o [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) afeta a direção para a qual o **LocalOffsetY** move o objeto. Na próxima amostra, o **LocalOffsetY** é animado de 0 a 400 e o **RotationX** de 0 a 65 graus.

[**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) converte o objeto perpendicular ao plano do objeto como se um vetor fosse desenhado diretamente através do centro, de trás do objeto até você. Para demonstrar como o **LocalOffsetZ** funciona, a próxima amostra anima o **LocalOffsetZ** de 0 a 400 e o [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) de 0 a 65 graus.

Bem no início da animação, quando o valor [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) está próximo a zero (paralelo à tela), o objeto se move em direção a você, mas conforme a face dele gira para baixo, ele se move para baixo.

**Deslocamento global**

As propriedades [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx), [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) e [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) traduzem o objeto ao longo dos eixos em relação à tela. Ou seja, ao contrário das propriedades de deslocamento local, o eixo no qual o objeto é movido é independente de qualquer rotação aplicada ao objeto. Essas propriedades são úteis quando você quer apenas mover o objeto pelos eixos x, y ou z da tela sem se preocupar com a rotação aplicada ao objeto.

A próxima amostra anima o [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) de 0 a 400 e o [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) de 0 a 65 graus.

Perceba que, nesta amostra, o objeto não muda de direção conforme gira. Isso ocorre porque o objeto está sendo movido ao longo do eixo x da tela sem levar em consideração a sua rotação.

## <a name="positioning-an-object"></a>Posicionando um objeto

Você pode usar os tipos [**Matrix3DProjection**](https://msdn.microsoft.com/library/windows/apps/BR210128) e [**Matrix3D**](https://msdn.microsoft.com/library/windows/apps/BR243266) para cenários semi-3D mais complexos que são possíveis com o [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192). O **Matrix3DProjection** fornece uma matriz de transformação 3D completa para ser aplicada a qualquer [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911), para que você possa aplicar um modelo arbitrário de matrizes de transformação e de perspectiva aos elementos. Não se esqueça de que essas APIs são mínimas e, logo, se você usá-las, precisará escrever o código que cria corretamente as matrizes de transformação 3D. Por causa disso, é mais fácil usar o **PlaneProjection** para cenários 3D simples.
