---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: "Animações de composição"
description: "Muitas propriedades de objeto e efeito de composição podem ser animadas usando animações de quadro chave e expressão permitindo que as propriedades de um elemento de interface do usuário mudem ao longo do tempo ou com base em um cálculo."
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 85b2e5006e8f2b9d2d2f78bab044032210011f41
ms.lasthandoff: 02/07/2017

---
# <a name="composition-animations"></a>Animações de composição

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A API do WinRT Windows.UI.Composition permite criar, animar, transformar e tratar os objetos do compositor em uma camada de API unificada. As animações de composição fornecem uma maneira avançada e eficiente de executar animações na interface do usuário do app. Elas foram projetadas desde o início para garantir que suas animações sejam executadas em 60 FPS independente do thread da interface do usuário e para oferecer a você a flexibilidade para criar experiências incríveis usando não só propriedades de tempo, mas também de entrada e outras propriedades, para ativar as animações.
Este tópico fornece uma visão geral da funcionalidade disponível que permite animar propriedades do objeto Composition.
Este documento presume que você esteja familiarizado com as noções básicas sobre a estrutura de Camada Visual. Para obter mais informações, [veja aqui](./composition-visual-tree.md). Existem dois tipos de animações de composição: **animações de quadro-chave** e **animações de expressão**  

![Tipos de animação](./images/composition-animation-types.png)  
   
 
## <a name="types-of-composition-animations"></a>Tipos de animações de composição
**Animações de quadro-chave** proporcionam experiências de animação tradicionais, *quadro por quadro* controladas por tempo. Os desenvolvedores podem definir explicitamente *pontos de controle* descrevendo os valores nos quais uma propriedade de animação precisa estar nos pontos específicos, na linha do tempo de animação. O mais importante é que você consegue usar funções de easing (também chamadas de interpoladores) para descrever como fazer a transição entre esses pontos de controle.  

**Animações implícitas** são um tipo de animação que permite que os desenvolvedores definam animações individuais reutilizáveis ou uma série de animações separadamente da lógica de apps importantes. Animações implícitas permitem que os desenvolvedores criem *modelos* de animação e vinculem-os a gatilhos. Esses gatilhos são alterações de propriedade resultantes de atribuições explícitas. Os desenvolvedores podem definir um modelo como uma única animação ou um grupo de animação. Os grupos de animação são uma coleção de modelos de animação que podem ser iniciados juntos explicitamente ou com um gatilho. Animações implícitas eliminam a necessidade de criar KeyFrameAnimations explícitas sempre que você deseja alterar o valor de uma propriedade e vê-la animada.

**Animações de expressão** são um tipo de animação introduzido na Camada Visual com a atualização de novembro do Windows 10 (compilação 10586). A ideia por trás das animações de expressão é que um desenvolvedor pode criar relacionamentos matemáticos entre as propriedades [visuais](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) e valores distintos que serão avaliados e atualizados a cada quadro. Os desenvolvedores podem fazer referência a propriedades de objetos de Composição ou conjuntos de propriedades, usar auxiliares de função matemática e até mesmo fazer referência à entrada para gerar esses relacionamentos matemáticos. As expressões tornam as experiências como cabeçalhos paralaxe e fixos possíveis e fáceis na plataforma Windows.  

## <a name="why-composition-animations"></a>Por que animações de composição?
**Desempenho**  
 Ao criar aplicativos universais Windows, a maior parte do código do desenvolvedor é executada no thread da interface de usuário. Para garantir que as animações sejam executadas perfeitamente em todas as diferentes categorias de dispositivo, o sistema executa os cálculos de animação e o trabalho em um thread independente para manter 60 FPS. Isso significa que os desenvolvedores podem contar com o sistema para fornecer animações perfeitas enquanto seus apps realizam outras operações complexas para experiências de usuário avançado.    
 
**Possibilidades**  
A meta das animações de composição na Camada visual é facilitar a criação de interfaces do usuário bonitas. Queremos fornecer aos desenvolvedores diversos tipos de animações que tornam mais fácil criar suas ideias incríveis.
 
   

**Modelagem de texto**  
 Todas as animações de composição na Camada Visual são modelos – isso significa que os desenvolvedores podem usar uma animação em vários objetos sem precisar criar animações separadas. Isso permite que os desenvolvedores usem a mesma animação e ajuste as propriedades ou os parâmetros para atender a outras necessidades sem se preocupar em obstruir os usos anteriores.  

Você pode conferir nossas discussões sobre //BUILD para [Animações de expressão](https://channel9.msdn.com/events/Build/2016/P486), [Experiências interativas](https://channel9.msdn.com/Events/Build/2016/P405), [Animações implícitas](https://channel9.msdn.com/events/Build/2016/P484) e [Animações conectadas](https://channel9.msdn.com/events/Build/2016/P485) para ver alguns exemplos do que é possível.

Você também pode conferir o [GitHub Composition](http://go.microsoft.com/fwlink/?LinkID=789439) para obter exemplos de como usar as APIs e alguns exemplos de fidelidade superiores das APIs em ação.
 
## <a name="what-can-you-animate-with-composition-animations"></a>O que você pode animar com animações de composição?
As animações de composição podem ser aplicadas à maioria dos objetos de composição como [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) e **InsetClip**. Você também pode aplicar animações de composição a efeitos de composição e conjuntos de propriedades. **Ao escolher o que animar, anote o tipo – use isso para determinar qual tipo de animação de quadro-chave criar ou para qual tipo sua expressão deverá ser resolvida.**  
 
### <a name="visual"></a>Visual
|Propriedades visuais animáveis|    Tipo|
|------|------|
|AnchorPoint|    Vector2|
|CenterPoint|    Vector3|
|Deslocamento|    Vector3|
|Opacidade|    Escalar|
|Orientação|    Quatérnion|
|RotationAngle|    Escalar|
|RotationAngleInDegrees|    Escalar|
|RotationAxis|    Vector3|
|Scale|    Vector3|
|Tamanho|    Vector2|
|TransformMatrix *|    Matrix4x4|
*Se você quiser animar a propriedade TransformMatrix toda como uma Matrix4x4, precisará usar uma ExpressionAnimation para fazer isso. Caso contrário, você poderá se concentrar células individuais da matriz e usar KeyFrame ou ExpressionAnimation.  

### <a name="insetclip"></a>InsetClip
|Propriedades InsetClip animáveis|    Tipo|
|-------------------------------|-------|
|BottomInset|    Escalar|
|LeftInset|    Escalar|
|RightInset|    Escalar|
|TopInset|    Escalar|

## <a name="visual-sub-channel-properties"></a>Propriedades do subcanal Visual
Além de poder animar propriedades do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx), você também pode direcionar os componentes do *subcanal* dessas propriedades para animações. Por exemplo, digamos que você simplesmente deseje animar o deslocamento X de um [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) em vez do deslocamento inteiro. A animação pode direcionar a propriedade Deslocamento Vector3 ou o componente X Escalar da propriedade Deslocamento. Além de poder direcionar um componente de subcanal individual de uma propriedade, você também pode direcionar vários componentes. Por exemplo, você pode direcionar o componente X e Y de Escala.

|Propriedades do subcanal Visual animável|    Tipo|
|----------------------------------------|------|
|AnchorPoint.x, y|Escalar|
|AnchorPoint.xy|Vector2|
|CenterPoint.x, y, z|Escalar|
|CenterPoint.xy, xz, yz|Vector2|
|Offset.x, y, z|Escalar|
|Offset.XY, xz, yz|Vector2|
|RotationAxis.x, y, z|Escalar|
|RotationAxis.xy, xz, yz|Vector2|
|Scale.x, y, z|Escalar|
|Scale.XY, xz, yz|Vector2|
|Size.x, y|Escalar|
|Size.xy|Vector2|
|TransformMatrix._11... TransformMatrix._NN,|Escalar|
|TransformMatrix._11_12 ... TransformMatrix._NN_NN|Vector2|
|TransformMatrix._11_12_13 ... TransformMatrix._NN_NN_NN|Vector3|
|TransformMatrix._11_12_13_14|Vector4|
|Cor*|    Cores (Windows.UI)|

* Animar o subcanal de cor da propriedade Pincel é um pouco diferente. Você anexa StartAnimation() ao Visual.Brush e declara a propriedade para animar no parâmetro como "Cor". (Mais detalhes sobre como animar cor são abordados posteriormente)

## <a name="property-sets-and-effects"></a>Conjuntos de Propriedades e Efeitos
Além de animar propriedades de [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) de Composição e InsetClip, você também pode animar propriedades em um PropertySet ou um Efeito. Para conjuntos de propriedades, você pode definir uma propriedade e armazená-la em um Conjunto de Propriedades de Composição – essa propriedade pode ser o destino de uma animação posteriormente (e também ser referenciada simultaneamente em outra). Isso será discutido em mais detalhes nas seções a seguir.  

Para efeitos, você pode definir efeitos gráficos usando as APIs de Efeitos de Composição (veja aqui para obter a [Visão geral de Efeitos](./composition-effects.md). Além de definir Efeitos, você também pode animar os valores de propriedade do Efeito. Isso é feito direcionando-se o componente de propriedades da propriedade Pincel em sprites visuais.

## <a name="quick-formula-getting-started-with-composition-animations"></a>Fórmula rápida: introdução a Animações de Composição
Antes de entrar em detalhes sobre como construir e usar os diferentes tipos de animações, veja a seguir uma fórmula rápida e de alta nível para reunir as Animações de Composição.  
1.    Decida qual propriedade, propriedade de subcanal ou Efeito você deseja animar - anote o tipo.  
2.    Crie um novo objeto para a sua animação – isso será uma animação de quadro-chave ou Expressão.  
    *  Para animações de quadro-chave, certifique-se de criar um tipo de animação de quadro-chave que corresponda ao tipo de propriedade que você deseja animar.  
    *  Há apenas um único tipo de Animação de expressão.  
3.    Defina o conteúdo para animação – Insira seus quadros-chave ou a cadeia de caracteres de expressão  
    *  Para animações de quadro-chave, certifique-se de que o valor de seus quadros-chave sejam do mesmo tipo que a propriedade que você deseja animar.  
    *  Para Animações de expressão, certifique-se de que sua cadeia de caracteres de expressão seja resolvida para o mesmo tipo que a propriedade que você deseja animar.  
4.    Inicie sua animação no [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) cuja propriedade você deseja animar – chame StartAnimation e inclua como parâmetros: o nome da propriedade que você deseja animar (na forma de cadeia de caracteres) e o objeto para a sua animação.  

```cs
// KeyFrame Animation Example to target Opacity property
// Step 2 - Create your animation object
var animation = _compositor.CreateScalarKeyFrameAnimation();
// Step 3 - Define Content
animation.InsertKeyFrameAnimation(1.0f, 0.2f); 
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", animation); 
  
// Expression Animation Example to target Opacity property
// Step 2 - Create your animation object
var expression = _compositor.CreateExpressionAnimation(); 
// Step 3 - Define Content (you can also define the string as part of the expression object
// declaration)
expression.Expression = "targetVisual.Offset.X / windowWidth";
expression.SetReferenceParameter("targetVisual", _target);
expression.SetScalarParameter("windowWidth", _xSizeWindow);
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", expression);

```

## <a name="using-keyframe-animations"></a>Usando Animações de quadro-chave
Animações de quadro-chave são animações baseadas em tempo que usam um ou mais quadros-chave para especificar como o valor animado deve mudar ao longo do tempo. Os quadros representam marcadores ou pontos de controle, permitindo que você defina qual deve ser o valor animado em um momento específico.  
 
### <a name="creating-your-animation-and-defining-keyframes"></a>Criando sua animação e definindo quadros-chave
Para construir uma animação de quadro-chave, use um o método de construtor do objeto Compositor que se correlaciona com o tipo da propriedade que você deseja animar. Os diferentes tipos de animação de quadro-chave são:
*    ColorKeyFrameAnimation
*    QuaternionKeyFrameAnimation
*    ScalarKeyFrameAnimation
*    Vector2KeyFrameAnimation
*    Vector3KeyFrameAnimation
*    Vector4KeyFrameAnimation  

Um exemplo que cria uma animação de quadro-chave Vector3:     
```cs
var animation = _compositor.CreateVector3KeyFrameAnimation(); 
```

Cada animação de quadro-chave é construída inserindo segmentos individuais de quadro-chave que definem dois componentes (com um terceiro opcional)  
*    Tempo: estado do progresso normalizado do quadro-chave entre 0,0 – 1,0
*    Valor: valor específico do valor da animação no estado de tempo
*    (Opcional) Função de easing: função para descrever a interpolação entre o quadro-chave anterior e o atual (explicada posteriormente neste tópico)  

Um exemplo que insere um quadro-chave no ponto intermediário da animação:
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

**Observação:** ao animar cor com Animações de quadro-chave, há alguns itens adicionais a serem lembrados:
1.    Você anexa StartAnimation ao Visual.Brush em vez de [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx), com **Cor** como o parâmetro de propriedade que você deseja animar.
2.    O componente "value" do quadro-chave é definido pelo objeto Cores do namespace Windows. UI.
3.    Você tem a opção de definir o espaço de cores pelo qual a interpolação passará definindo a propriedade InterpolationColorSpace. Os valores possíveis incluem:
    *    CompositionColorSpace.Rgb
    *    CompositionColorSpace.Hsl


## <a name="keyframe-animation-properties"></a>Propriedades da animação de quadro-chave
Depois de definir sua animação de quadro-chave e os quadros-chave individuais, você pode definir várias propriedades da animação:
*    DelayTime – tempo antes do início de uma animação depois que StartAnimation() é chamado
*    Duration – duração da animação
*    IterationBehavior – comportamento de contagem ou de repetição infinita de uma animação
*    IterationCount – número de vezes finitas que uma animação de quadro-chave será repetida
*    Contagem de quadros-chave – leitura do número de quadros chave em determinada animação de quadro-chave
*    StopBehavior – especifica o comportamento de um valor de propriedade de animação quando StopAnimation é chamado  
*   Direção – especifica a direção da animação para reprodução  

Um exemplo que define a duração da animação como 5 segundos:  
```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

## <a name="easing-functions"></a>Funções de easing
Funções de easing (CompositionEasingFunction) indicam como valores intermediários avançam do valor de quadro-chave anterior para o valor de quadro-chave atual. Se você não fornecer uma função de easing para o quadro-chave, uma curva padrão será usada.  
Há dois tipos de funções de easing com suporte:
*    Linear
*    Bézier cúbica  
*   Etapa  

Béziers cúbicas são uma função paramétrica frequentemente usada para descrever curvas suaves que podem ser dimensionadas. Ao usar com animações de quadro-chave de composição, você define dois pontos de controle que são objetos Vector2. Esses pontos de controle são usados para definir a forma da curva. É recomendável usar sites semelhantes como [isso](http://cubic-bezier.com/#0,-0.01,.48,.99) para visualizar como os dois pontos de controle formam a curva de uma Bézier cúbica.

Para criar uma função de easing, utilize o método de construtor fora de seu objeto Compositor. Dois exemplos abaixo que criam uma função de easing Linear e uma função de easing da Bézier cúbica.    
```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
var step = _compositor.CreateStepEasingFunction();
```
Para adicionar sua função de easing ao seu quadro-chave, basta adicionar o terceiro parâmetro ao quadro-chave ao inseri-lo na animação.   
Um exemplo que adiciona uma função de easing easeIn com o quadro-chave:  
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

## <a name="starting-and-stopping-keyframe-animations"></a>Iniciando e interrompendo animações de quadro-chave
Depois de definir a animação e quadros-chave, você estará pronto para vincular sua animação. Ao iniciar sua animação, você especifica o [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) a ser animado, a propriedade de destino a ser animada e uma referência para a animação. Você pode fazer isso chamando a função StartAnimation(). Lembre-se de que chamar StartAnimation() em uma propriedade desconectará e removerá todas as animações em execução anteriormente.  
**Observação:** a referência para a propriedade que você escolher para animar estará na forma de uma cadeia de caracteres.  

Um exemplo que define e inicia uma animação na propriedade Deslocamento do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx):  
```cs
targetVisual.StartAnimation("Offset", animation);
```  

Se você quiser direcionar uma propriedade de subcanal, adicione o subcanal à cadeia de caracteres que define a propriedade que você deseja animar. Nos exemplos acima, a sintaxe seria alterada para StartAnimation("Offset.X, animation2), em que animation2 é uma ScalarKeyFrameAnimation.  

Depois de iniciar a animação, você também pode interrompê-la antes do término. Para tal, use a função StopAnimation().  
Um exemplo que interrompe uma animação na propriedade Deslocamento do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx):    
```cs
targetVisual.StopAnimation("Offset");
```

Você também pode definir o comportamento da animação quando ela é interrompida explicitamente. Para fazer isso, defina a propriedade Interromper Comportamento da sua animação. Existem três opções:
*    LeaveCurrentValue: a animação marcará o valor da propriedade animada para ser o último valor calculado da animação
*    SetToFinalValue: a animação marcará o valor da propriedade animada para ser o valor do quadro-chave final
*    SetToInitialValue: a animação marcará o valor da propriedade animada para ser o valor do primeiro quadro-chave  

Um exemplo que define a propriedade StopBehavior para uma animação de quadro-chave:  
```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

## <a name="animation-completion-events"></a>Eventos de conclusão de animação
Com animações de quadro-chave, os desenvolvedores podem usar lotes de animações a serem agregadas quando uma animação selecionada (ou grupo de animações) forem concluídas. Somente eventos de conclusão de animação de quadro-chave podem ser enviados em lote. Expressões não têm um fim definido, portanto, elas não acionam um evento de conclusão. Se uma animação de expressão for iniciada dentro de um lote, ela será executada conforme o esperado e não afetará quando o lote for acionado.    

Um evento de conclusão em lote será acionado quando todas as animações do lote forem concluídas. O tempo que leva para o evento do lote ser disparado depende da animação mais longa ou mais atrasada no lote.
A agregação dos estados de término é útil quando você precisa saber quando grupos de animações selecionadas serão concluídos para agendar outro trabalho.  

Os lotes serão descartados depois que o evento de conclusão for disparado. Você também pode chamar Dispose() a qualquer momento para liberar o recurso antes. Será possível descartar o objeto de lote manualmente se uma animação em lote for finalizada antes e você não quiser selecionar o evento de conclusão. Se uma animação for interrompida ou cancelada, o evento de conclusão será acionado e considerado no lote no qual ele foi definido. Isso é demonstrado no exemplo SDK Animation_Batch no [Windows/Composition GitHub](http://go.microsoft.com/fwlink/p/?LinkId=789439).  
 
## <a name="scoped-batches"></a>Lotes analisados
Para agregar um grupo específico de animações ou direcionar o evento de conclusão de uma única animação, você cria um lote analisado.    
```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
``` 
Depois de criar um lote analisado, todas as animações iniciadas serão agregadas até que o lote seja explicitamente suspenso ou encerrado usando a função Suspend ou End.    

Chamar a função Suspend interrompe a agregação dos estados de término da animação até Resume ser chamado. Isso permite que você exclua explicitamente o conteúdo de um determinado lote.  

No exemplo a seguir, a animação que direciona a propriedade Deslocamento de VisualA não será incluída no lote:  
```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

Para concluir o lote, você deve chamar End(). Sem uma chamada de término, o lote permanecerá aberto coletando objetos indefinidamente.  
 
O seguinte trecho de código e diagrama abaixo mostram um exemplo de como o lote agregará animações para rastrear estados finais. Observe que, neste exemplo, as Animações 1, 3 e 4 terão estados finais acompanhados por este lote, mas a Animação 2 não.  
```cs
myScopedBatch.End();
CompositionScopedBatch myScopedBatch =     _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
// Start Animation1
[…]
myScopedBatch.Suspend();
// Start Animation2 
[…]
myScopedBatch.Resume();
// Start Animation3
[…]
// Start Animation4
[…]
myScopedBatch.End();
```  
![O lote analisado contém a animação 1, 3 e 4, enquanto a animação 2 é excluída.](./images/composition-scopedbatch.png)
 
## <a name="batching-a-single-animations-completion-event"></a>Envio em lote de um evento de conclusão de uma única animação
Se você quiser saber quando uma única animação termina, precisará criar um lote analisado que inclua apenas a animação que você está direcionando. Por exemplo:  
```cs
CompositionScopedBatch myScopedBatch =     _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

## <a name="retrieving-a-batchs-completion-event"></a>Recuperando o evento de conclusão de um lote

Ao enviar em lote uma ou várias animações, você recuperará o evento de conclusão do lote da mesma maneira. Registre o método de manipulação de eventos para o evento Completed do lote de destino.  

```cs
myScopedBatch.Completed += OnBatchCompleted;
``` 

## <a name="batch-states"></a>Estados do lote
Há duas propriedades que você pode usar para determinar o estado de um lote existente; IsActive e IsEnded.  

A propriedade IsActive retornará true se um lote de destino for aberto para animações de agregação. IsActive retornará false quando um lote for suspenso ou encerrado.   

A propriedade IsEnded retornará true quando você não conseguir adicionar uma animação a esse lote específico. Um lote será encerrado quando você chamar explicitamente End() para um lote específico.  
 
## <a name="using-expression-animations"></a>Usando animações de expressão
Animações de expressão são um novo tipo de animação que a equipe de composição introduziu com a atualização de novembro para Windows 10 (10586). Em um nível alto, animações de expressão são baseadas em uma equação/relacionamento matemático entre valores distintos e referências a outras propriedades de objeto de composição. Ao contrário das animações de quadro-chave que usam uma função de interpolador (Bézier cúbica, Quadrante, Quintic, etc.) para descrever como o valor muda ao longo do tempo, as animações de expressão usam uma equação matemática para definir como o valor animado é calculado em cada quadro. É importante ressaltar que animações de expressão não têm uma duração definida – uma vez iniciadas, elas serão executadas e usarão a equação matemática para determinar o valor da propriedade de animação até que sejam explicitamente interrompidas.

**Então por que as animações de expressão são úteis?** O consumo de energia real das animações de expressão vem de sua capacidade de criar um relacionamento matemático que inclua referências a parâmetros ou propriedades em outros objetos. Isso significa que você pode ter uma equação que faça referência a valores de propriedades em outros objetos de composição, variáveis locais ou até mesmo valores compartilhados em conjuntos de propriedades de composição. Devido a esse modelo de referência e porque a equação é avaliada a cada quadro, se os valores que definem uma equação mudarem, a saída da equação também será alterada. Isso abre possibilidades maiores além das tradicionais animações de quadro-chave onde os valores devem ser distintos e predefinidos. Por exemplo, experiências como cabeçalhos fixos e paralaxe podem ser facilmente descrita usando as animações de expressão.

**Observação:** usamos os termos "Expressão" ou "Cadeia de caracteres de expressão" como referência à sua equação matemática que define o objeto de animação de expressão.

## <a name="creating-and-attaching-your-expression-animation"></a>Criando e anexando sua animação de expressão
Antes de abordarmos a sintaxe de criação de animações de expressão, há alguns princípios fundamentais a serem mencionados:  
*    Animações de expressão usam uma equação matemática definida para determinar o valor da propriedade de animação a cada quadro.
*    A equação matemática é inserida na expressão como uma cadeia de caracteres.
*    O resultado da equação matemática deve ser resolvido para o mesmo tipo que a propriedade que você pretende animar. Se eles não corresponderem, você receberá um erro quando a expressão for calculada. Se a equação for resolvida para Nan (número/0), o sistema usará o último valor calculado anteriormente.
*    Animações de expressão têm uma *vida útil infinita* – elas continuarão a ser executadas até serem interrompidas.  

Para criar uma animação de expressão, basta usar o construtor de seu objeto de composição, onde você define sua expressão matemática.  
 
Um exemplo do construtor no qual é definida uma expressão muito básica que soma dois valores escalares (descreveremos expressões mais complicadas na próxima seção):  
```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```
Semelhante a animações de quadro-chave, depois de definir sua animação de expressão, você precisa anexá-la ao Visual e declarar a propriedade que deseja que a animação anime. A seguir, vamos continuar com o exemplo acima e anexar nossa animação de expressão à propriedade Opacidade do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) (um tipo escalar):  
```cs
targetVisual.StartAnimation("Opacity", expression);
```

## <a name="components-of-your-expression-string"></a>Componentes da cadeia de caracteres de expressão
O exemplo na seção anterior demonstrou dois valores escalares simples sendo adicionados. Embora seja um exemplo válido de expressões, ele não demonstra totalmente o que pode ser feito com expressões. Uma coisa a observar sobre o exemplo acima é que, como eles são valores distintos, em cada quadro a equação será resolvida como 0,5 e nunca muda durante todo o ciclo de vida da animação. O potencial real das expressões vem de definição de uma relação matemática em que os valores podem mudam periodicamente ou o tempo todo.  
 
Vamos examinar as diferentes partes que compõem esses tipos de expressões.  

### <a name="operators-precedence-and-associativity"></a>Operadores, precedência e capacidade de associação
A cadeia de expressão dá suporte ao uso de operadores típicos que devem descrever relações matemáticas entre diferentes componentes da equação:  

|Categoria|    Operadores|
|--------|-----------|
|Unários|    -|
|Multiplicativos|    * /|
|Aditivos|    + -|
|Mod| %|  

Da mesma forma, quando a expressão é avaliada, ela seguirá a precedência do operador e associação conforme definido na especificação da linguagem C#. Em outras palavras, ela seguirá a ordem básica das operações.  

No exemplo a seguir, quando avaliados, os parênteses serão resolvidos primeiro antes do restante da equação com base na ordem das operações:  
```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

### <a name="property-parameters"></a>Parâmetros de propriedade
Parâmetros de propriedade são um dos componentes mais sofisticados das animações de expressão. Na cadeia de caracteres de expressão, você pode fazer referência a valores de propriedades de outros objetos, como [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) de composição, conjunto de propriedades de composição ou outros objetos C#.   

Para usá-los em uma cadeia de caracteres de expressão, basta definir as referências como parâmetros para a animação de expressão. Isso é feito por meio do mapeamento da cadeia de caracteres usada na expressão para o objeto real. Isso permite que o sistema, ao avaliar a equação, saiba o que inspecionar para calcular o valor. Existem diferentes tipos de parâmetros que correspondem ao tipo do objeto que você deseja incluir na equação:  

|Tipo|    Função para criar o parâmetro|
|----|------------------------------|
|Escalar|    SetScalarParameter(String ref, Scalar obj)|
|Vetor|    SetVector2Parameter(String ref, Vector2 obj)<br/>SetVector3Parameter(String ref, Vector3 obj)<br/>SetVector4Parameter(String ref, Vector4 obj)|
|Matrix|    SetMatrix3x2Parameter(String ref, Matrix3x2 obj)<br/>SetMatrix4x4Parameter(String ref, Matrix4x4 obj)|
|Quatérnion|    SetQuaternionParameter(String ref, Quaternion obj)|
|Cor|    SetColorParameter(String ref, Color obj)|
|CompositionObject|    SetReferenceParameter(cadeia de caracteres ref, Composition object obj)|
|booliano| SetBooleanParameter (ref. de cadeia de caracteres, objeto booliano)|  

No exemplo a seguir, criamos uma animação de expressão que faz referência ao deslocamento de dois outros elementos [visuais](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) de composição e um objeto System.Numerics Vector3 básico.  
```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

Além disso, você pode referenciar um valor em um conjunto de propriedades de uma expressão usando o mesmo modelo descrito acima. Conjuntos de propriedades de composição são uma maneira útil de armazenar dados usados por animações e são úteis para a criação de dados compartilháveis, reutilizáveis que não estão vinculados ao tempo de vida de todos os outros objetos de composição. Valores de conjunto de propriedades podem ser referenciados em uma expressão semelhante às outras referências de propriedade. (Conjuntos de propriedades são abordados com mais detalhes em uma seção posterior)  

Podemos modificar o exemplo diretamente acima, de forma que um conjunto de propriedades seja usado para definir o commonOffset em vez de uma variável local:
```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

Por fim, ao fazer referência a propriedades de outros objetos, também é possível fazer referência às propriedades de subcanal na cadeia de caracteres de expressão ou como parte do parâmetro de referência.  
 
No exemplo a seguir, fazemos referência ao subcanal x de propriedades Deslocamento de dois [Visuais](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) – um na cadeia de caracteres de expressão em si e o outro ao criar a referência de parâmetro.
Observe que, ao referenciar o componente X do deslocamento, podemos alterar o tipo de parâmetro para um parâmetro Escalar em vez de um Vector3 como no exemplo anterior:  
```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

### <a name="expression-helper-functions-and-constructors"></a>Funções auxiliares de expressão e construtores
Além de ter acesso a operadores e parâmetros de propriedade, você pode aproveitar uma lista de funções matemáticas para usar em suas expressões. Essas funções são fornecidas para realizar cálculos e operações em tipos diferentes, que você faria de forma semelhante com objetos System.Numerics.  

O exemplo a seguir cria uma expressão voltada para escalares que aproveita a função de auxiliar Clamp:  
```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

Além de uma lista de funções auxiliares, você também pode usar métodos internos do construtor dentro de uma cadeia de expressão que gerarão uma instância desse tipo com base nos parâmetros fornecidos.  

O exemplo a seguir cria uma expressão que define um novo Vector3 na cadeia de caracteres de expressão:  
```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

Você pode localizar a lista completa de funções auxiliares e construtores na seção Apêndice ou para cada tipo na lista a seguir:  
*    [Escalar](#scalar)
*    [Vector2](#vector2)
*    [Vector3](#vector3)
*    [Matrix3x2](#matrix3x2)
*    [Matrix4x4](#matrix4x4)
*    [Quatérnion](#quaternion)
*    [Cor](#color)  

### <a name="expression-keywords"></a>Palavras-chave de expressão
Você pode tirar proveito das "palavras-chave" especiais que são tratadas de forma diferente quando a cadeia de expressão é avaliada. Como elas são consideradas "palavras-chave", não podem ser usadas como parte do parâmetro de cadeia de caracteres das suas referências de propriedade.  
 
|Palavra-chave|    Descrição|
|-------|--------------|
|This.StartingValue| Fornece uma referência ao valor original inicial da propriedade que está sendo animada.|
|This.CurrentValue|    Fornece uma referência ao valor atualmente "conhecido" da propriedade|
|Pi| Fornece uma referência de palavra-chave ao valor de PI|

O exemplo a seguir demonstra o uso da palavra-chave this.StartingValue:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

### <a name="expressions-with-conditionals"></a>Expressões com condicionais
Além de dar suporte a relações matemáticas usando operadores, referências de propriedade, funções e construtores, você também pode criar uma expressão que contém um operador ternário:  
```
(condition ? ifTrue_expression : ifFalse_expression)
```

Instruções condicionais permitem que você escreva expressões de forma que, com base em uma determinada condição, diferentes relações matemáticas sejam usadas pelo sistema para calcular o valor da propriedade de animação. Operadores ternários podem ser aninhados como as expressões para as instruções true ou false.  

Os seguintes operadores condicionais são compatíveis com a instrução condicional: 
*    Igual a (==)
*    Diferente de (!=)
*    Menor que (<)
*    Menor que ou igual a (<=)
*    Maior que (>)
*    Maior que ou igual a (>=)  

As seguintes conjunções são aceitas como operadores ou funções na instrução condicional:
*    Not: ! / Not(bool1)
*    And: && / And(bool1, bool2)
*    Or: || / Or(bool1, bool2)  

Aqui está um exemplo de animação de expressão que usa uma condicional.  
```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

## <a name="expression-keyframes"></a>Quadros-chave de expressão
Anteriormente neste documento, descrevemos como criar animações de quadro-chave e apresentamos a você as animações de expressão e todos os elementos que você pode usar para formar a cadeia de expressão. E se você quisesse o poder das animações de expressão, mas desejasse a interpolação de tempo fornecida por animações de quadro-chave? A resposta é quadros-chave de expressão!  

Em vez de definir um valor distinto para cada ponto de controle na animação de quadro-chave, você pode ter o valor de ser uma cadeia de caracteres de expressão. Nessa situação, o sistema usará a cadeia de caracteres de expressão para calcular o valor da propriedade de animação que deve estar em determinado ponto na linha do tempo. Em seguida, o sistema simplesmente interpolará para esse valor como uma animação de quadro-chave normal.    

Você não precisa criar animações especiais para usar quadros-chave de expressão – basta inserir um ExpressionKeyFrame em sua animação de quadro-chave padrão, fornecer o tempo e sua cadeia de caracteres de expressão como o valor. O exemplo a seguir demonstra isso, usando uma cadeia de caracteres de expressão como o valor para um dos quadros-chave:   
```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

## <a name="expression-sample"></a>Exemplo de expressão
O código a seguir mostra um exemplo de configuração de uma animação de expressão para uma experiência paralaxe básica que extrai os valores de entrada do Visualizador de Rolagem.
```cs
// Get scrollviewer
ScrollViewer myScrollViewer = ThumbnailList.GetFirstDescendantOfType<ScrollViewer>();
_scrollProperties = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);

// Setup the expression
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 *     ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5     * ItemHeight))";
```

## <a name="animating-with-property-sets"></a>Animando com conjuntos de propriedades
Os conjuntos de propriedades de composição fornecem a capacidade de armazenar valores que podem ser compartilhados entre várias animações e não estão vinculados ao tempo de vida de outro objeto de composição. Os conjuntos de propriedades são extremamente úteis para armazenar valores comuns e, em seguida, fazer referência a eles com facilidade em animações. Você também pode usar conjuntos de propriedades para armazenar dados com base na lógica de app para ativar uma expressão.  

Para criar um conjunto de propriedades, use o método construtor de seu objeto Compositor:  
```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Depois de criar seu conjunto de propriedades, você pode adicionar uma propriedade e um valor a ele:  
```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

Semelhante ao que vimos anteriormente, podemos fazer referência a esse valor de conjunto de propriedades de uma animação de expressão:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.target.Offset + sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
targetVisual.StartAnimation("Offset", expression);
```

Valores de conjunto de propriedades também podem ser animados. Isso é feito anexando a animação ao objeto PropertySet e, em seguida, fazendo referência ao nome da propriedade na cadeia de caracteres. Abaixo, animamos a propriedade NewOffset no conjunto de propriedades usando uma animação de quadro-chave.  
```cs
var keyFrameAnimation = _compositor.CreateVector3KeyFrameAnimation()
keyFrameAnimation.InsertKeyFrame(0.5f, new Vector3(25.0f, 68.0f, 0.0f);
keyFrameAnimation.InsertKeyFrame(1.0f, new Vector3(89.0f, 125.0f, 0.0f);
_sharedProperties.StartAnimation("NewOffset", keyFrameAnimation);
```


Você deve estar se perguntando se esse código for executado em um app, o que acontecerá com o valor da propriedade animada à qual a animação de expressão está anexada. Nessa situação, a expressão inicialmente geraria um valor, no entanto, uma vez iniciada a animação de quadro-chave para animar a propriedade no conjunto de propriedades, o valor da expressão será atualizado também, uma vez que a equação é calculada a cada quadro. Essa é a vantagem dos conjuntos de propriedades com animações de quadro-chave e expressão!  
 
## <a name="manipulationpropertyset"></a>ManipulationPropertySet
Além de utilizar conjuntos de propriedades de composição, um desenvolvedor também é capaz de obter acesso a ManipulationPropertySet, que permite o acesso a propriedades fora de um ScrollViewer XAML. Essas propriedades podem ser usadas e referenciadas em uma animação de expressão para ativar experiências como Parallax e cabeçalhos fixos. Observação: você pode capturar o ScrollViewer de qualquer controle XAML (ListView, GridView etc.) que tenha conteúdo rolável e usar esse ScrollViewer para obter o ManipulationPropertySet para esses controles roláveis.  

Em sua expressão, é possível referenciar as seguintes propriedades do Visualizador de Rolagem:  

|Propriedade| Tipo|  
|--------|-----|  
|Translation| Vector3|  
|Pan| Vector3|  
|Scale| Vector3|  
|CenterPoint| Vector3|  
|Matrix| Matrix4x4|  

Para obter uma referência para ManipulationPropertySet, utilize o método GetScrollViewerManipulationPropertySet de ElementCompositionPreview.  
```csharp
CompositionPropertySet manipPropSet = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScroller);
```

Quando você tiver uma referência para esse conjunto de propriedades, poderá referenciar propriedades do Visualizador de Rolagem que são encontradas no conjunto de propriedades. A primeira etapa é criar o parâmetro de referência.  
```csharp
ExpressionAnimation exp = compositor.CreateExpressionAnimation();
exp.SetReferenceParameter("ScrollManipulation", manipPropSet);
```

Depois de configurar o parâmetro de referência, você poderá referenciar as propriedades ManipulationPropertySet na expressão.  
```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```

## <a name="using-implicit-animations"></a>Usando animações implícitas  
As animações são uma ótima maneira para você descrever um comportamento aos seus usuários. Há várias maneiras de animar o seu conteúdo, mas todos os métodos abordados até agora exigem que você *inicie* explicitamente sua animação. Embora isso permita que você tenha controle total para definir quando uma animação será iniciada, torna-se difícil de gerenciar quando uma animação é necessária sempre que um valor de propriedade for alterado. Isso ocorre com bastante frequência quando apps são separados da "personalidade" do app que define as animações da "lógica" do app que define os principais componentes e a infraestrutura do app. Animações implícitas fornecem uma maneira mais fácil e mais limpa de definir a animação separadamente da lógica principal do app. Você pode vincular essas animações para executar gatilhos de alteração de propriedade específica.

### <a name="setting-up-your-implicitanimationcollection"></a>Configurando seu ImplicitAnimationCollection  
Animações implícitas são definidas por outros objetos **CompositionAnimation** (**KeyFrameAnimation** ou **ExpressionAnimation**). **ImplicitAnimationCollection** representa o conjunto de objetos **CompositionAnimation** que serão iniciados quando o *gatilho* de alteração de propriedades for atendido. Note que, ao definir animações, certifique-se de definir a propriedade **Target**, isso define a propriedade [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) a qual a animação se destinará quando ela for iniciada. A propriedade **Target** só pode ser uma propriedade [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) que pode ser animada.
No trecho de código abaixo, um único **Vector3KeyFrameAnimation** é criado e definido como parte de **ImplicitAnimationCollection**. **ImplicitAnimationCollection** é então anexado à propriedade **ImplicitAnimation** do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx), de forma que, quando o gatilho for atendido, a animação será iniciada.  
```csharp
Vector3KeyFrameAnimation animation = _compositor.CreateVector3KeyFrameAnimation();
animation.DelayTime =  TimeSpan.FromMilliseconds(index);
animation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");
animation.Target = "Offset";
ImplicitAnimationCollection implicitAnimationCollection = compositor.CreateImplicitAnimationCollection();

visual.ImplicitAnimations = implicitAnimationCollection;
```


### <a name="triggering-when-the-implicitanimation-starts"></a>Disparando quando ImplicitAnimation iniciar  
Gatilho é o termo usado para descrever quando as animações começarão implicitamente. No momento os gatilhos são definidos como alterações em qualquer uma das propriedades animáveis em [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) – essas alterações ocorrem por conjuntos explícitos na propriedade. Por exemplo, ao colocar um gatilho de **Deslocamento** em **ImplicitAnimationCollection** e associar uma animação a ele, as atualizações para **Deslocamento** do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) direcionado acionará a animação para seu novo valor usando a animação na coleção.  
No exemplo acima, podemos adicionar esta linha adicional para definir o gatilho para a propriedade **Offset** do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) de destino.  
```csharp
implicitAnimationCollection["Offset"] = animation;
```  
Observe que um **ImplicitAnimationCollection** pode ter vários gatilhos. Isso significa que a animação implícita ou o grupo de animações pode iniciar as alterações nas propriedades diferentes. No exemplo acima, o desenvolvedor pode potencialmente adicionar um gatilho para outras propriedades como Opacidade.  
###<a name="thisfinalvalue"></a>this.FinalValue     
No primeiro exemplo implícito, nós usamos um ExpressionKeyFrame para o KeyFrame "1.0" e atribuímos a expressão  **this.FinalValue** a ele. **this.FinalValue** é uma palavra-chave reservada no idioma da expressão que fornece comportamento diferenciado para animações implícitas. **this.FinalValue** associa o valor definido na propriedade da API à animação. Isso ajuda na criação de modelos reais. **this.FinalValue** não é útil em animações explícitas uma vez que a propriedade da API é definida instantaneamente enquanto no caso de animações implícitas ela é adiada.  
 
## <a name="using-animation-groups"></a>Usando grupos de animação  
O **CompositionAnimationGroup** fornece uma maneira fácil para os desenvolvedores agruparem uma lista de animações que podem ser usadas com animações implícitas ou explícitas.   
### <a name="creating-and-populating-animation-groups"></a>Criando e preenchendo grupos de animação  
O método **CreateAnimationGroup** do objeto Compositor permite que os desenvolvedores criem um grupo de animação:  
```sharp
CompositionAnimationGroup animationGroup = _compositor.CreateAnimationGroup();
animationGroup.Add(animationA);
animationGroup.Add(animationB);
```   
Depois que o grupo for criado, animações individuais poderão ser adicionadas ao grupo de animação. Lembre-se de que você não precisa iniciar explicitamente as animações individuais – todas elas serão iniciadas quando **StartAnimationGroup** for chamado para o cenário explícito ou quando o gatilho for atendido para o implícito.  
Observação: certifique-se de que as animações que são adicionadas ao grupo tenham a propriedade **Target** definida. Isso definirá qual propriedade do [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) alvo elas animarão.

### <a name="using-animation-groups-with-implicit-animations"></a>Usando grupos de animação com animações implícitas  
Os desenvolvedores podem criar animações implícitas de forma que, quando um gatilho for atendido, um conjunto de animações na forma de um grupo de animação será iniciado. Nesse caso, defina o grupo de animação como o conjunto de animações que é iniciado quando o gatilho é atendido.  
```csharp
implicitAnimationCollection["Offset"] = animationGroup;
```   
### <a name="using-animation-groups-with-explicit-animations"></a>Usando grupos de animação com animações explícitas  
Os desenvolvedores podem criar animações explícitas de forma que as animações individuais adicionadas sejam iniciadas quando **StartAnimationGroup** for chamado. Observe que nesta chamada de **StartAnimation**, não há nenhuma propriedade direcionada para o grupo uma vez que as animações individuais podem estar direcionadas a propriedades diferentes. Certifique-se de que a propriedade de destino para cada animação seja definida.  
```csharp
visual.StartAnimationGourp(AnimationGroup);
```  

### <a name="e2e-sample"></a>Amostra de E2E 
Este exemplo mostra como animar a propriedade Offset implicitamente quando um novo valor for definido.  
```csharp 
class PropertyAnimation
{
    PropertyAnimation(Compositor compositor, SpriteVisual heroVisual, SpriteVisual listVisual)
    {
        // Define ImplicitAnimationCollection
        ImplicitAnimationCollection implicitAnimations = 
        compositor.CreateImplicitAnimationCollection();

        // Trigger animation when the “Offset” property changes.
        implicitAnimations["Offset"] = CreateAnimation(compositor);

        // Assign ImplicitAnimations to a visual. Unlike Visual.Children,    
        // ImplicitAnimations can be shared by multiple visuals so that they 
        // share the same implicit animation behavior (same as Visual.Clip).
        heroVisual.ImplicitAnimations = implicitAnimations;

        // ImplicitAnimations can be shared among visuals 
        listVisual.ImplicitAnimations = implicitAnimations;

        listVisual.Offset = new Vector3(20f, 20f, 20f);
    }

    Vector3KeyFrameAnimation CreateAnimation(Compositor compositor)
    {
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        animation.InsertExpressionKeyFrame(0f, "this.StartingValue");
        animation.InsertExpressionKeyFrame(1f, "this.FinalValue");
        animation.Target = “Offset”;
        animation.Duration = TimeSpan.FromSeconds(0.25);
        return animation;
    }
}
```   

 
 
## <a name="appendix"></a>Apêndice
### <a name="expression-functions-by-structure-type"></a>Funções de expressão por tipo de estrutura
### <a name="scalar"></a>Escalar  

|Operações de construtor e função| Descrição|  
|-----------------------------------|--------------|  
|Abs(Float value)| Retorna um Float que representa o valor absoluto do parâmetro float|  
|Clamp(Float value, Float min, Float max)| Retorna um valor flutuante que é maior do que o mínimo e menor do que o máximo ou o mínimo se o valor for menor do que o mínimo ou máximo se o valor for maior do que o máximo|  
|Max (Float value1, Float value2)| Retorna o maior flutuante entre value1 e value2.|  
|Min (Float value1, Float value2)| Retorna o menor flutuante entre value1 e value2.|  
|Lerp(Float value1, Float value2, Float progress)| Retorna um flutuante que representa o cálculo de interpolação linear calculada entre os dois valores escalares com base no andamento (Observação: o andamento é entre 0,0 e 1,0)|  
|Slerp(Float value1, Float value2, Float progress)|    Retorna um flutuante que representa a interpolação esférica calculada entre os dois valores flutuantes com base no andamento (Observação: o andamento é entre 0,0 e 1,0)|  
|Mod(Float value1, Float value2)| Retorna o flutuante resultante da divisão de value1 e value2|  
|Ceil(Float value)| Retorna o parâmetro Float arredondado para o próximo número inteiro maior|  
|Floor(Float value)| Retorna o parâmetro Float para o próximo número inteiro menor|  
|Sqrt(Float value)|    Retorna a raiz quadrada do parâmetro Float|  
|Square(Float value)| Retorna o quadrado do parâmetro Float|  
|Sin(Float value1)| Retorna o Sin do parâmetro Float|
|Asin(Float value2)| Retorna o ArcSin do parâmetro Float|
|Cos(Float value1)| Retorna o Cos do parâmetro Float|
|ACos(Float value2)| Retorna o ArcCos do parâmetro Float|
|Tan(Float value1)| Retorna o Tan do parâmetro Float|
|ATan(Float value2)| Retorna o ArcTan do parâmetro Float|
|Round(Float value)| Retorna o parâmetro Float arredondado para o número inteiro mais próximo|
|Log10(Float value)| Retorna o resultado de Log (base 10) do parâmetro Float|
|Ln(Float value)| Retorna o resultado de Log Natural do parâmetro Float|
|Pow(Float value, Float power)|    Retorna o resultado do parâmetro Float elevado a uma potência específica|
|ToDegrees(Float radians)| Retorna o parâmetro Float convertido em graus|
|ToRadians (graus Float)| Retorna o parâmetro Float convertido em radianos|

### <a name="vector2"></a>Vector2  

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|Abs (Vector2 value)|    Retorna um Vector2 com valor absoluto aplicado a cada componente|
|Clamp (Vector2 value1, Vector2 min, Vector2 max)|    Retorna um Vector2 que contém os valores vinculados para cada componente respectivo|
|Max (Vector2 value1, Vector2 value2)|    Retorna um Vector2 que executou um Máx em cada um dos componentes correspondentes de value1 e value2|
|Min (Vector2 value1, Vector2 value2)|    Retorna um Vector2 que executou um Mín em cada um dos componentes correspondentes de value1 e value2|
|Scale(Vector2 value, Float factor)|    Retorna um Vector2 com cada componente do vetor multiplicado pelo fator de dimensionamento.|
|Transform(Vector2 value, Matrix3x2 matrix)|    Retorna um Vector2 resultante da transformação linear entre um Vector2 e um Matrix3x2 (também conhecido como multiplicação de um vetor por uma matriz).|
|Lerp(Vector2 value1, Vector2 value2, Float progress)|    Retorna um Vector2 que representa o cálculo de interpolação linear calculada entre os dois valores Vector2 com base no andamento (Observação: o andamento é entre 0,0 e 1,0)|
|Length(Vector2 value)|    Retorna um valor flutuante que representa o comprimento/magnitude do Vector2|
|LengthSquared (Vector2)|    Retorna um valor flutuante que representa o quadrado do comprimento/magnitude de um Vector2|
|Distance(Vector2 value1, Vector2 value2)|    Retorna um valor flutuante que representa a distância entre dois valores de Vector2|
|DistanceSquared (value1 Vector2, value2 Vector2)|    Retorna um valor flutuante que representa o quadrado da distância entre dois valores de Vector2|
|Normalize(Vector2 value)|    Retorna um Vector2 que representa o vetor unitário do parâmetro onde todos os componentes foram normalizados|
|Vector2 (Float x, Float y)|    Constroi um Vector2 usando dois parâmetros Float|

### <a name="vector3"></a>Vector3  

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|Abs (Vector3 value)|    Retorna um Vector3 com valor absoluto aplicado a cada componente|
|Clamp (Vector3 value1, Vector3 min, Vector3 max)|    Retorna um Vector3 que contém os valores vinculados para cada componente respectivo|
|Max (Vector3 value1, Vector3 value2)|    Retorna um Vector3 que executou um Máx em cada um dos componentes correspondentes de value1 e value2|
|Min (Vector3 value1, Vector3 value2)|    Retorna um Vector3 que executou um Mín em cada um dos componentes correspondentes de value1 e value2|
|Scale(Vector3 value, Float factor)|    Retorna um Vector3 com cada componente do vetor multiplicado pelo fator de dimensionamento.|
|Lerp(Vector3 value1, Vector3 value2, Float progress)|    Retorna um Vector3 que representa o cálculo de interpolação linear calculada entre os dois valores Vector3 com base no andamento (Observação: o andamento é entre 0,0 e 1,0)|
|Length(Vector3 value)|    Retorna um valor flutuante que representa o comprimento/magnitude do Vector3|
|LengthSquared(Vector3)|    Retorna um valor flutuante que representa o quadrado do comprimento/magnitude de um Vector3|
|Distance(Vector3 value1, Vector3 value2)|    Retorna um valor flutuante que representa a distância entre dois valores de Vector3|
|DistanceSquared(Vector3 value1, Vector3 value2)|    Retorna um valor flutuante que representa o quadrado da distância entre dois valores de Vector3|
|Normalize(Vector3 value)|    Retorna um Vector3 que representa o vetor unitário do parâmetro onde todos os componentes foram normalizados|
|Vector3(Float x, Float y, Float z)|    Constroi um Vector3 usando três parâmetros Float|

### <a name="vector4"></a>Vector4  

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|Abs (Vector4 value)|    Retorna um Vector3 com valor absoluto aplicado a cada componente|
|Clamp (Vector4 value1, Vector4 min, Vector4 max)|    Retorna um Vector4 que contém os valores vinculados para cada componente respectivo|
|Max (Vector4 value1 Vector4 value2)|    Retorna um Vector4 que executou um Máx em cada um dos componentes correspondentes de value1 e value2|
|Min (Vector4 value1 Vector4 value2)|    Retorna um Vector4 que executou um Mín em cada um dos componentes correspondentes de value1 e value2|
|Scale(Vector3 value, Float factor)|    Retorna um Vector3 com cada componente do vetor multiplicado pelo fator de dimensionamento.|
|Transform(Vector4 value, Matrix4x4 matrix)|    Retorna um Vector4 resultante da transformação linear entre um Vector4 e um Matrix4x4 (também conhecido como multiplicação de um vetor por uma matriz).|
|Lerp(Vector4 value1, Vector4 value2, Float progress)|    Retorna um Vector4 que representa o cálculo de interpolação linear calculada entre os dois valores Vector4 com base no andamento (Observação: o andamento é entre 0,0 e 1,0)|
|Length(Vector4 value)|    Retorna um valor flutuante que representa o comprimento/magnitude do Vector4|
|LengthSquared(Vector4)|    Retorna um valor flutuante que representa o quadrado do comprimento/magnitude de um Vector4|
|Distance(Vector4 value1, Vector4 value2)|    Retorna um valor flutuante que representa a distância entre dois valores de Vector4|
|DistanceSquared(Vector4 value1, Vector4 value2)|    Retorna um valor flutuante que representa o quadrado da distância entre dois valores de Vector4|
|Normalize(Vector4 value)|    Retorna um Vector4 que representa o vetor unitário do parâmetro onde todos os componentes foram normalizados|
|Vector4(Float x, Float y, Float z, Float w)|     Constroi um Vector4 usando quatro parâmetros Float|

### <a name="matrix3x2"></a>Matrix3x2  

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|Scale(Matrix3x2 value, Float factor)|    Retorna um Matrix3x2 com cada componente da matriz multiplicado pelo fator de dimensionamento.|
|Inverse(Matrix 3x2 value)|    Retorna um objeto Matrix3x2 que representa a matriz recíproca|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|    Constroi uma Matrix3x2 usando 6 parâmetros Float|
|Matrix3x2.CreateFromScale(Vector2 scale)|    Constrói uma Matrix3x2 de uma escala que representa Vector2<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(Vector2 translation)|    Constrói uma Matrix3x2 de uma conversão que representa Vector2<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|  
|Matrix3x2.CreateSkew(Float x, Float y, Vector2 centerpoint)| Constrói uma Matrix3x2 de dois Float e um Vector2 que representa a inclinação<br/>\[1.0, Tan(y),<br/>Tan(x), 1.0,<br/>-centerpoint.Y * Tan(x), -centerpoint.X * Tan(y)\]|  
|Matrix3x2.CreateRotation(Float radians)| Constrói uma Matrix3x2 de uma rotação em radianos<br/>\[Cos(radians), Sin(radians),<br/>-Sin(radians), Cos(radians),<br/>0.0, 0.0 \]|   
|Matrix3x2.CreateTranslation(Vector2 translation)| Mesmo que CreateFromTranslation|      
|Matrix3x2.CreateScale(Vector2 scale)| Mesmo que CreateFromScale|    

    
### <a name="matrix4x4"></a>Matrix4x4  

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|Scale(Matrix4x4 value, Float factor)|    Retorna uma Matrix4x4 com cada componente da matriz multiplicado pelo fator de dimensionamento.|
|Inverse(Matrix4x4)|    Retorna um objeto Matrix4x4 que representa a matriz recíproca|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>       Float M31, Float M32, Float M33, Float M34,<br/>       Float M41, Float M42, Float M43, Float M44)|    Constroi uma Matrix4x4 usando 16 parâmetros Float|
|Matrix4x4.CreateFromScale(Vector3 scale)|    Constrói uma Matrix4x4 de uma escala que representa Vector3<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(Vector3 translation)|    Constrói uma Matrix4x4 de uma conversão que representa Vector3<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(Vector3 axis, Float angle)|    Constroi uma Matrix4x4 de um eixo Vector3 e Float que representa um ângulo|
|Matrix4x4(Matrix3x2 matrix)| Constroi uma Matrix4x4 usando uma Matrix3x2<br/>\[matrix.11, matrix.12, 0, 0,<br/>matrix.21, matrix.22, 0, 0,<br/>0, 0, 1, 0,<br/>matrix.31, matrix.32, 0, 1\]|  
|Matrix4x4.CreateTranslation(Vector3 translation)| Mesmo que CreateFromTranslation|  
|Matrix4x4.CreateScale(Vector3 scale)| Mesmo que CreateFromScale|  


### <a name="quaternion"></a>Quatérnion  

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|Slerp(Quaternion value1, Quaternion value2, Float progress)|    Retorna um Quatérnion que representa a interpolação esférica calculada entre os dois valores flutuantes Quatérnion com base no andamento (Observação: o andamento é entre 0,0 e 1,0)|
|Concatenate(Quaternion value1 Quaternion value2)|    Retorna um Quatérnion que representa uma concatenação de dois Quatérnions (também conhecido como um Quatérnion que representa duas rotações individuais combinadas)|
|Length(Quaternion value)|    Retorna um valor flutuante que representa o comprimento/magnitude do Quatérnion.|
|LengthSquared(Quaternion)|    Retorna um valor flutuante que representa o quadrado do comprimento/magnitude de um Quatérnion|
|Normalize(Quaternion value)|    Retorna um Quatérnion cujos componentes foram normalizados|
|Quaternion.CreateFromAxisAngle(Vector3 axis, Scalar angle)|    Constroi um Quatérnion de um eixo Vector3 e um escalar que representa um ângulo|
|Quaternion(Float x, Float y, Float z, Float w)|    Constrói um Quatérnion de quatro valores flutuantes|

### <a name="color"></a>Cor

|Operações de construtor e função|    Descrição|
|-----------------------------------|--------------|
|ColorLerp(Color colorTo, Color colorFrom, Float progress)|    Retorna um objeto de cor que representa o valor calculado de interpolação linear entre dois objetos de cor com base em um andamento determinado. (Observação: o progresso é entre 0,0 e 1,0)|
|ColorLerpRGB(Color colorTo, Color colorFrom, Float progress)|    Retorna um objeto de cor que representa o valor calculado de interpolação linear entre dois objetos com base em um andamento determinado no espaço de cores RGB.|
|ColorLerpHSL(Color colorTo, Color colorFrom, Float progress)|    Retorna um objeto de cor que representa o valor calculado de interpolação linear entre dois objetos com base em um andamento determinado no espaço de cores HSL.|
|ColorArgb(Float a, Float r, Float g, Float b)|    Constrói um objeto que representa a cor definida por componentes ARGB|
|ColorHsl(Float h, Float s, Float l)|    Constrói um objeto que representa a cor definida por componentes HSL (Observação: matiz definido de 0 e 2pi)|





