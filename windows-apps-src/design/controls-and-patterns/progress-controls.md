---
author: QuinnRadich
Description: A progress control provides feedback to the user that a long-running operation is underway.
title: Diretrizes de controles de progresso
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d92005dca87d1be0cf9fddd0a28402497ab56595
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4213216"
---
# <a name="progress-controls"></a>Controles de progresso

 

Um controle de progresso oferece feedback ao usuário que uma operação de execução longa está em andamento. Isso pode significar que o usuário não pode interagir com o aplicativo quando o indicador de progresso está visível e também pode indicar a duração do tempo de espera, dependendo do indicador usado.

> **APIs importantes**: [classe ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx), [propriedade IsIndeterminate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx), [classe ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx), [propriedade IsActive](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

## <a name="types-of-progress"></a>Tipo de progresso

Há dois controles para mostrar ao usuário que uma operação está em andamento – um ProgressBar ou um ProgressRing.

-   O estado *determinado* do ProgressBar mostra a porcentagem de conclusão de uma tarefa. Isso deve ser usado durante uma operação cuja duração é conhecida, mas seu progresso não deve bloquear a interação do usuário com o aplicativo.
-   O estado *indeterminado* do ProgressBar mostra que uma operação está em andamento, não bloqueia a interação do usuário com o aplicativo e o tempo de conclusão é desconhecido.
-   O ProgressRing tem apenas um estado *indeterminado* e deve ser usado quando qualquer outra interação do usuário é bloqueada até que a operação seja concluída.

Além disso, um controle de progresso é somente leitura, não é interativo. Isso significa que o usuário não pode invocar nem usar esses controles diretamente.

![Estados do ProgressBar](images/ProgressBar_TwoStates.png)

*De cima para baixo - um ProgressBar indeterminado e um ProgressBar determinado*

![Estado do ProgressRing](images/ProgressRing_SingleState.png)

*Um ProgressRing indeterminado*

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> ou <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>Quando usar cada controle

Nem sempre é óbvio qual controle ou estado (determinado vs indeterminado) usar ao tentar mostrar que algo está acontecendo. Às vezes, uma tarefa é tão óbvia que não requer um controle de progresso – e, às vezes, mesmo se um controle de progresso é usado, uma linha de texto ainda é necessária para explicar ao usuário qual operação está em andamento.

### <a name="progressbar"></a>ProgressBar
-   **O controle tem uma duração definida ou um fim previsível?**

    Use um ProgressBar determinado e, em seguida, atualize a porcentagem ou o valor corretamente.

-   **O usuário pode continuar sem precisar monitorar o andamento da operação?**

    Quando um ProgressBar está em uso, a interação é não modal, o que geralmente significa que o usuário não está bloqueado pela conclusão da operação e pode continuar a usar o aplicativo de outras maneiras, até que esse aspecto seja concluído.

-   **Palavras-chave**

    Se a sua operação se enquadrar nessas palavras-chave, ou se você estiver mostrando texto junto com a operação de progresso que corresponda a essas palavras-chave, considere o uso de um ProgressBar:

    - *Carregando...*
    - *Recuperando*
    - *Trabalhando...*

### <a name="progressring"></a>ProgressRing

-   **A operação fará com que o usuário espere para continuar?**

    Se uma operação exige que toda a interação com o aplicativo ou uma parte dela aguarde até que ela seja concluída, o ProgressRing é a melhor opção. O controle ProgressRing é usado para interações modais, o que significa que o usuário é bloqueado até que o ProgressRing desapareça.

-   **O aplicativo está aguardando que o usuário execute uma tarefa?**

    Em caso afirmativo, use um ProgressRing, pois ele indica um tempo de espera desconhecido para o usuário.

-   **Palavras-chave**

    Se a sua operação se enquadrar nessas palavras-chave, ou se você estiver mostrando texto junto com a operação de progresso que corresponda a essas palavras-chave, considere o uso de um ProgressRing:

    - *Atualizando*
    - *Entrando...*
    - *Conectando...*

### <a name="no-progress-indication-necessary"></a>Nenhuma indicação de progresso necessária
-   **O usuário precisa saber que algo está acontecendo?**

    Por exemplo, se o aplicativo está baixando alguma coisa em tela de fundo, e o usuário não iniciou a transferência, o usuário não precisa necessariamente saber disso.

-   **A operação é uma atividade em tela de fundo que não bloqueia a atividade do usuário e é de interesse mínimo (porém existente) para o usuário?**

    Use texto quando seu aplicativo estiver executando tarefas que não precisam estar visíveis o tempo todo, mas cujo status você ainda precisa mostrar.

-   **Importa ao usuário apenas a conclusão da operação?**

    Às vezes, é melhor mostrar um aviso somente quando a operação é concluída ou fornecer um sinal visual de que a operação foi concluída imediatamente e executar os toques finais em segundo plano.

## <a name="progress-controls-best-practices"></a>Práticas recomendadas de controles de progresso

Às vezes, é melhor ver algumas representações visuais de quando e onde usar esses diferentes controles de progresso:

**ProgressBar - determinado**

![Exemplo de ProgressBar determinado](images/PB_DeterminateExample.png)

O primeiro exemplo é o ProgressBar determinado. Quando a duração da operação é conhecida, ao instalar, baixar, configurar, etc., é melhor usar um ProgressBar determinado.

**ProgressBar - indeterminado**

![Exemplo de ProgressBar indeterminado](images/PB_IndeterminateExample.png)

Quando não se sabe o tempo que operação levará, use um ProgressBar indeterminado. ProgressBars indeterminados também são bons no preenchimento de uma lista virtualizada e na criação de uma transição visual suave entre um ProgressBar indeterminado para um determinado.

-   **A operação é em uma coleção virtualizada?**

    Em caso afirmativo, não coloque um indicador de progresso em cada item da lista conforme eles aparecem. Em vez disso, use um ProgressBar e coloque-o na parte superior da coleção de itens que estão sendo carregados, para mostrar que os itens estão sendo procurados.

**ProgressRing - indeterminado**

![Exemplo de ProgressRing indeterminado](images/PR_IndeterminateExample.png)

O ProgressRing indeterminado é usado quando toda a interação do usuário com o aplicativo é interrompida ou o aplicativo está esperando a entrada do usuário para continuar. O exemplo "entrando..." acima é um cenário perfeito para o ProgressRing, o usuário não pode continuar a usar o aplicativo até que a entrada seja concluída.

## <a name="customizing-a-progress-control"></a>Personalizando um controle de progresso

Os dois controles de progresso são bastante simples, mas alguns recursos visuais dos controles não são óbvios de personalizar.

**Dimensionando o ProgressRing**

O ProgressRing pode ter o tamanho que você quiser, mas o mínimo deve ser 20x20epx. Para redimensionar um ProgressRing, é necessário definir sua altura e largura. Se apenas a largura ou a altura for definida, o controle assumirá o tamanho mínimo (20x20epx) – por outro lado, se a altura e a largura forem definidas como dois tamanhos diferentes, será considerado o tamanho menor.
Para garantir que o ProgressRing esteja correto para suas necessidades, defina a altura e a largura como o mesmo valor:

```XAML
<ProgressRing Height="100" Width="100"/>
```

Para tornar seu ProgressRing visível e animá-lo, você deve definir a propriedade IsActive como true:

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**Colorindo os controles de progresso**

Por padrão, a cor principal dos controles de progresso é definida como a cor de destaque do sistema. Para substituir esse pincel, basta alterar a propriedade de primeiro plano em qualquer um dos controles.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

A alteração da cor de primeiro plano do ProgressRing alterará as cores dos pontos. A propriedade de primeiro plano do ProgressBar alterará a cor de preenchimento da barra – para alterar a parte não preenchida da barra, basta substituir a propriedade de plano de fundo.

**Mostrando um cursor de espera**

Às vezes, é melhor mostrar apenas um breve cursor de espera quando o aplicativo ou a operação precisa de tempo para pensar, e você precisa indicar ao usuário que ele não deverá interagir com o aplicativo ou a área onde o cursor de espera estiver visível até o cursor de espera desaparecer.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Classe ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529)
- [Classe ProgressRing](https://msdn.microsoft.com/library/windows/apps/br227538)

**Para desenvolvedores (XAML)**
- [Adicionando controles de progresso](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [Como criar uma barra de progresso indeterminado personalizada para Windows Phone](http://go.microsoft.com/fwlink/p/?LinkID=392426)
