---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: animações de composição
description: muitas propriedades de objeto e efeito de composição podem ser animadas usando animações de quadro chave e expressão permitindo que as propriedades de um elemento de interface do usuário mudem ao longo do tempo ou com base em um cálculo.
---
# Animações de composição

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Muitas propriedades de objeto e efeito de composição podem ser animadas usando animações de quadro chave e expressão permitindo que as propriedades de um elemento de interface do usuário mudem ao longo do tempo ou com base em um cálculo. Existem dois tipos de animações: animações de quadro chave, representadas pela classe [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830), e animações de expressão, representadas pela classe [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817).

-   [Propriedades animáveis](./composition-animation.md#animatable_properties)
-   [Animações de quadro chave](./composition-animation.md#keyframe-animations)
    -   [Criando sua animação e quadros chave](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [Propriedades do quadro chave](./composition-animation.md#keyframe-properties)
    -   [Funções de easing](./composition-animation.md#easing-functions)
    -   [Iniciando e interrompendo animações de quadro chave](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [Eventos de conclusão de animação](./composition-animation.md#animation-completion-events)
-   [Animações de expressão](./composition-animation.md#expression-animations)
    -   [Criando sua expressão](./composition-animation.md#creating-your-expression)
    -   [Conjuntos de propriedades](./composition-animation.md#property-sets)
    -   [Quadros chave de expressão](./composition-animation.md#expression_keyframes)

## Propriedades animáveis

As propriedades a seguir são animáveis, associando-as a uma animação de quadro chave ou de expressão:

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## Animações de quadro chave

Animações de quadro chave são animações baseadas em tempo que usam um ou mais quadros chave para especificar como o valor animado deve mudar ao longo do tempo. Os quadros representam marcadores, permitindo que você defina qual deve ser o valor animado em um momento específico.

### Criando sua animação e quadros chave

Para construir uma animação de quadro chave, use um dos métodos de construtor da classe Compositor que se correlaciona ao tipo de estrutura da propriedade que você deseja animar.

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

Por exemplo, o trecho a seguir cria uma animação de quadro chave Vector3:

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

Cada quadro chave é construído chamando o método InsertKeyFrame de uma KeyFrameAnimation e especificando dois componentes e, opcionalmente, um terceiro componente:

-   Tempo: estado do progresso normalizado do quadro chave entre 0.0 – 1.0
-   Valor: valor específico do valor da animação no estado de tempo
-   (Opcional) Função de easing: função para descrever a interpolação entre o quadro chave anterior e o atual (explicada posteriormente)

Por exemplo, o trecho a seguir insere um quadro chave em uma Vector3KeyFrameAnimation que será acionada na metade da animação:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### Propriedades do quadro chave

Depois de definir sua animação de quadro chave e os quadros chave individuais, você pode definir várias propriedades da animação:

-   DelayTime – tempo antes do início de uma animação depois que StartAnimation() é chamado
-   Duration – duração da animação
-   IterationBehavior – comportamento de contagem ou de repetição infinita de uma animação
-   IterationCount – número de vezes finitas que uma animação de quadro chave será repetida
-   KeyFrame Count – número de quadros chave em determinada KeyFrameAnimation
-   StopBehavior – especifica o comportamento de um valor de propriedade de animação quando StopAnimation é chamado.

Por exemplo, o trecho a seguir define a duração da animação para cinco segundos:

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### Funções de easing

Uma função de easing de quadro, CompositionEasingFunction, indica como valores intermediários avançam do valor de quadro chave anterior para o valor de quadro chave atual. Se você não fornecer uma função de easing para o quadro chave, uma curva padrão será usada.

Há dois tipos de funções de easing com suporte:

-   Linear ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   Cubic Bezier ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

Para criar uma função de easing, use o método [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) ou [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) da classe [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761):

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

Observação: uma ferramenta útil para gerar os pontos para sua função Cubic Bezier pode ser encontrada aqui.

Para adicionar sua função de easing ao seu quadro chave, basta adicionar o terceiro parâmetro ao quadro chave ao inseri-lo na animação:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### Iniciando e interrompendo animações de quadro chave

Depois de definir sua animação, os quadros chave e as propriedades, você está pronto para conectar a animação à propriedade que deseja animar. Você conecta sua animação à propriedade que pretende animar chamando [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840) no objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) ao qual a propriedade pertence.

A sintaxe geral e um exemplo são:

```cs
targetVisual.StartAnimation(“Offset”, animation);
```

Depois de iniciar a animação, você também pode interrompê-la e desconectá-la. Isso é feito usando o método [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) e especificando a propriedade que você deseja parar de animar.

Por exemplo:

```cs
targetVisual.StopAnimation(“Offset”);
```

### Eventos de conclusão de animação

As animações de quadro chave têm um final definido, ao contrário das animações de expressão, que são condicionais. Os desenvolvedores podem determinar quando a animação de grupos ou de um único quadro chave é concluída usando lotes. Os lotes podem ser criados ou recuperados, dependendo do tipo de objeto de lote, e são usados para agregar o estado final das animações. Os lotes analisados são criados enquanto os lotes de confirmação são recuperados, o uso desses lotes diferentes será descrito posteriormente.

Um evento de conclusão em lote será acionado quando todas as animações do lote forem concluídas. O tempo que leva para o evento de conclusão do lote ser disparado depende da animação mais longa ou mais atrasada no lote. A agregação dos estados de término é útil quando você precisa saber quando grupos de animações selecionadas serão concluídos para agendar outro trabalho.

### Lotes analisados

Para agregar um grupo específico ou uma única animação, você precisa primeiro criar um lote analisado chamando [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425).

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

Depois de criar um lote analisado, todas as animações iniciadas agregam-se até que o lote seja explicitamente suspenso ou encerrado usando [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) ou [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844).

Chamar [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) interrompe a agregação dos estados de término da animação até [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847) ser chamado. Isso permite que você exclua explicitamente o conteúdo de um determinado lote.

```cs
myScopedBatch.Suspend();
```

Para concluir o lote, você deve chamar [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Sem uma chamada de término, o lote permanecerá aberto coletando objetos para sempre.

```cs
myScopedBatch.End();
```

Se você quiser saber quando uma única animação termina, crie um lote analisado, inicie a animação e encerre o lote.

Por exemplo:

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation(“Opacity”, myAnimation);
myScopedBatch.End();
```

### Lotes de confirmação

Um lote de confirmação é um lote que é criado implicitamente durante o ciclo de confirmação. O lote de confirmação atual pode ser recuperado chamando [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430) a qualquer momento durante o ciclo de confirmação. O ciclo de confirmação é definido como o tempo entre as atualizações do compositor. As atualizações são colocadas em fila até que o sistema esteja pronto para processar as alterações e desenhar bits na tela. Os títulos não controlam quando as confirmações acontecem. O lote de confirmação agregará todos os objetos no ciclo de confirmação, aqueles antes e depois de GetCommitBatch ser chamado. Há apenas um lote de confirmação por ciclo de confirmação, e você não precisa chamar [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) explicitamente no lote de confirmação.

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### Estados do lote

Para determinar se um lote está aberto para agregar animações, você pode usar a propriedade **IsActive**. Se a propriedade **IsEnded** retornar true, você não poderá adicionar uma animação a esse lote específico. Os lotes analisados são “encerrados” quando tiverem sido encerrados explicitamente chamando o método [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Os lotes de confirmação são encerrados quando o ciclo de confirmação atual estiver encerrado.

## Animações de expressão

Animações de expressão são animações que usam uma expressão matemática para especificar como o valor animado deve ser calculado para cada quadro. A linguagem de expressão propriamente dita pode referenciar propriedades de outros objetos de composição. Ao contrário das animações de quadro chave, as expressões não são baseadas em tempo e são processadas a cada quadro (se necessário). As expressões podem ser úteis para descrever experiências do usuário mais complexas que podem ser processadas pelo mecanismo de composição, como cabeçalhos fixos e paralaxe.

### Criando sua expressão

Para criar sua expressão, chame [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002) em seu objeto Compositor e especifique a expressão que você deseja usar:

```cs
var expression = _compositor.CreateExpressionAnimation(“INSERT_EXPRESSION_STRING”)
```

### Operadores, precedência e capacidade de associação

A linguagem de expressão aceita os seguintes operadores e segue a precedência do operador e a capacidade de associação conforme definido na especificação da linguagem C#:

-   Unários (-)
-   Multiplicativos (\* /)
-   Aditivos (-+)

A linguagem de expressão também dá suporte ao conceito de operações matemáticas por componente. Somente há suporte para esses operadores de acesso de membro (x.y) em tipos "primitivos" (vetores e matrizes). Por exemplo:

```cs
Offset.x + 5.0
```

### Parâmetros de propriedade

Um dos componentes eficientes da linguagem de expressão é a capacidade de usar parâmetros como variáveis na expressão. A cadeia de caracteres de expressão pode conter parâmetros que são substituídos por um valor constante ou que são referências ao valor da propriedade de outro objeto quando calculado. Por exemplo:

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

Neste exemplo, ChildOffset e ParentOffset são parâmetros que representam a propriedade de deslocamento de dois elementos visuais. Você define parâmetros usando os métodos **Set\*Parameter** da classe [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708):

-   Para criar um parâmetro de vetor, use [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx), [**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) ou [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter).
-   Para criar um parâmetro de matriz, use [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx), ou [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter).
-   Para criar um parâmetro escalar, use [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter).
-   Para criar um parâmetro de cor, use [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter).
-   Para criar um parâmetro de quatérnion, use [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter).
-   Para criar um parâmetro de referência, use [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter).

Na cadeia de caracteres da expressão acima, precisamos criar dois parâmetros para definir os dois elementos visuais:

```cs
Expression.SetReferenceParameter(“ChildVisual”, childVisual);
Expression.SetReferenceParameter(“ParentVisual”, parentVisual);
```

### Funções auxiliares de expressão

Além de ter acesso a operadores e parâmetros de propriedade, você também tem acesso à biblioteca completa de funções auxiliares da linguagem de expressão. Você pode encontrar a lista completa de funções auxiliares na seção de comentários da classe [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817), mas aqui estão algumas:

-   Max (Scalar value1, Scalar value2)
-   Scale (Vector3 value, Scalar factor)
-   Transform(Vector2 value, Matrix 3x2 matrix)

Este é um exemplo mais complexo de cadeia de caracteres de expressão que usa a função auxiliar Clamp:

```cs
Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)
```

### Iniciando e interrompendo sua animação de expressão

Para iniciar e interromper suas animações de expressão, você usa as mesmas funções ([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840), [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)) das animações de quadro chave. Observação: as animações de expressão continuarão em execução até que sejam interrompidas usando **StopAnimation** – ao contrário das animações de quadro chave, elas não têm uma duração finita.

### Conjuntos de propriedades

Além de poder referenciar propriedades de outros objetos Composition, você também pode criar e armazenar valores de propriedade próprios que podem ser usados em várias animações. Os conjuntos de propriedades são representados pela classe [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). Os conjuntos de propriedades permitem que você crie uma propriedade com um valor e referenciá-la mais tarde em uma expressão ou até mesmo associá-la como o destino de uma animação de quadro chave.

Para criar um conjunto de propriedades, use o método [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) da classe Compositor. Por exemplo:

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Depois de criar seu conjunto de propriedades, você pode adicionar uma propriedade e um valor a ele usando um dos métodos **Insert\*** do [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). Por exemplo:

```cs
_sharedProperties.InsertVector3(“NewOffset”, offset);
```

Depois de criar sua animação de expressão, você pode referenciar propriedades do conjunto de propriedades na cadeia de caracteres de expressão com o uso de um parâmetro de referência. Por exemplo:

```cs
var expression = _compositor.CreateExpressionAnimation(“sharedProperties.NewOffset”);
expression.SetReferenceParameter(“sharedProperties”, _sharedProperties);
```

### Quadros chave de expressão

Anteriormente neste documento, descrevemos como você cria animações de quadro chave e seus respectivos quadros chave. Além de criar o quadro chave tradicional com um tempo normalizado e o componente de valor, você também pode definir os quadros chave da expressão.

Em vez de definir um valor explícito para um determinado tempo no quadro chave, podemos usar a sintaxe da expressão para descrever como o valor deve ser calculado nesse ponto específico na linha do tempo do quadro chave. Você pode combinar quadros chave de expressão com quadros chave básicos em sua animação de quadro chave.

### Construindo quadros chave de expressão

Semelhante aos quadros chave tradicionais, os quadros chave de expressão são construídos especificando 2 componentes:

-   Tempo: estado do tempo normalizado do quadro chave entre 0.0 – 1.0
-   Valor: a cadeia de caracteres de expressão usada para descrever como o valor deve ser calculado

Por exemplo, o trecho a seguir usa uma combinação de quadros chave regulares e de expressão:

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, “VisualBOffset.X / VisualAOffset.Y”);
animation.InsertKeyFrame(1.00f, 0.8f);
```

### Usando valores atuais e iniciais

A linguagem de expressão, é possível fazer referência ao valor atual e inicial de uma animação. Esses valores são prefixados na linguagem de expressão com "this":

-   this.CurrentValue
-   this.StartingValue

Um exemplo de uso desses valos em um quadro chave de expressão:

```cs
animation.InsertExpressionKeyFrame(0.0f, “this.CurrentValue + delta”);
```

### Expressões condicionais

Além de oferecer suporte a expressões matemáticas básicas, você também pode definir uma expressão condicional usando um operador ternário:

```cs
(condition ? first_expression : second_expression)
```

Nas expressões propriamente ditas, os seguintes operadores condicionais são permitidos na instrução condicional:

-   Igual a (==)
-   Diferente de (!=)
-   Menor que (<)
-   Menor que ou igual a (<=)
-   Maior que (>)
-   Maior que ou igual a (>=)

Por fim, as seguintes conjunções são aceitas como operadores ou funções na instrução condicional:

-   Not: ! / Not(bool1)
-   And: && / And(bool1, bool2)
-   Or: || / Or(bool1, bool2)

O trecho a seguir mostra um exemplo do uso de condicionais em uma expressão:

```cs
var expression = _compositor.CreateExpressionAnimation(“target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f”);
```

 

 






<!--HONumber=Mar16_HO1-->


