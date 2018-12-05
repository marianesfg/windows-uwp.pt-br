---
Description: Create Universal Windows Platform (UWP) apps with intuitive and distinctive user interaction experiences that are optimized for touch but are functionally consistent across input devices.
title: Interações por toque
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: toque, ponteiro, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b662a7689f0b0b24fc3f70a9fbc143d4268d2cb8
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8693748"
---
# <a name="touch-interactions"></a>Interações por toque


Projete o seu app com a expectativa de que o toque será o método de entrada principal dos usuários. Se você usar os controles UWP, o suporte para touchpad, mouse e caneta/stylus não exigirá nenhuma programação adicional, pois os aplicativos UWP o fornecem gratuitamente.

Tenha em mente, porém, que uma interface do usuário otimizada para toque nem sempre é superior a uma interface do usuário tradicional. Ambas oferecem vantagens e desvantagens exclusivas de tecnologia e aplicativo. Na mudança para uma interface do usuário touch-first, é importante entender as diferenças essenciais entre a entrada por toque (incluindo touchpad), caneta, mouse e teclado.

> **APIs importantes**: [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994), [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)


Muitos dispositivos têm telas multi-touch que dão suporte ao uso de um ou mais dedos (ou contatos por toque) como entrada. Os contatos por toque, e seu movimento, são interpretados como gestos e manipulações de toque para dar suporte a várias interações do usuário.

A Plataforma Universal do Windows (UWP) inclui diversos mecanismos diferentes para manipulação de entrada por toque, permitindo criar uma experiência imersiva que os usuários podem explorar com confiança. Aqui, abordamos os aspectos básicos do uso da entrada por toque em um aplicativo UWP.

Interações por toque requerem três coisas:

-   Uma tela sensível ao toque.
-   O contato direto (ou proximidade, se o monitor tiver sensores de proximidade e der suporte à detecção por movimento) de um ou mais dedos na tela.
-   Movimento de contatos por toque (ou a ausência deles, com base em um limite de tempo).

Os dados de entrada fornecidos pelo sensor de toque podem ser:

-   Interpretados como um gesto físico para a manipulação direta de um ou mais elementos de interface do usuário (como movimento panorâmico, giro, redimensionamento ou mudanças). Por outro lado, a interação com um elemento por meio de sua janela de propriedades, caixa de diálogo ou outra funcionalidade da interface do usuário é considerada uma manipulação indireta.
-   Reconhecidos como um método de entrada alternativo, como o mouse ou a caneta.
-   Usados para complementar ou modificar aspectos de outros métodos de entrada, como borrar um traço de tinta desenhado com uma caneta.

A entrada por toque geralmente envolve a manipulação direta de um elemento na tela. O elemento responde imediatamente a qualquer contato por toque dentro de sua área de teste, e reage de acordo com os movimentos subsequentes dos contatos por toque, incluindo a remoção.

Os gestos e as interações de toque personalizados devem ser projetados com cuidado. Eles devem ser intuitivos, ágeis e detectáveis, e devem permitir que os usuários explorem o aplicativo com confiança.

Certifique-se de que a funcionalidade do aplicativo seja exposta de forma consistente em cada tipo de dispositivo de entrada com suporte. Se necessário, use algum modo de entrada indireta, como entrada de texto para interações de teclado, ou funcionalidades da interface do usuário para mouse e caneta.

Lembre-se de que os mecanismos de entrada tradicionais (como mouse e teclado) são familiares e atraentes para muitos usuários. Eles podem oferecer a velocidade, a precisão e a resposta tátil que o toque talvez não possa.

O fornecimento de experiências únicas e diferenciadas de interação para todos os dispositivos dará suporte à mais ampla gama de recursos e preferências, alcançará o público mais amplo possível e atrairá mais clientes para o seu aplicativo.

## <a name="compare-touch-interaction-requirements"></a>Compare os requisitos de interação por toque

A tabela a seguir mostra algumas das diferenças entre os dispositivos de entrada, as quais você deve considerar ao projetar aplicativos UWP otimizados para toque.

<table>
<tbody><tr><th>Fator</th><th>Interações por toque</th><th>Interações de mouse, teclado e caneta/stylus</th><th>Touchpad</th></tr>
<tr><td rowspan="3">Precisão</td><td>A área de contato da ponta do dedo é maior do que uma coordenada x-y única, o que aumenta a probabilidade de ativações de comando acidentais.</td><td>O mouse e a caneta/stylus fornecem uma coordenada x-y precisa.</td><td>Igual ao mouse.</td></tr>
<tr><td>O formato da área de contato altera ao longo do movimento.  </td><td>Os movimentos do mouse e traços da caneta/stylus fornecem uma coordenada x-y precisa. O foco do teclado é explícito.</td><td>Igual ao mouse.</td></tr>
<tr><td>Não há cursor do mouse para ajudar no direcionamento.</td><td>O cursor do mouse, o cursor da caneta/stylus e o foco do teclado, todos ajudarão no direcionamento.</td><td>Igual ao mouse.</td></tr>
<tr><td rowspan="3">Anatomia humana</td><td>Movimentos com as pontas dos dedos são imprecisos porque dificultam um movimento em linha reta usando um ou mais dedos. Isso se deve à curvatura das articulações das mãos e ao número de articulações envolvidas no movimento.</td><td>É fácil realizar um movimento em linha reta com o mouse ou caneta/stylus porque a mão que os controla percorre uma distância física menor do que o cursor na tela.</td><td>Igual ao mouse.</td></tr>
<tr><td>Algumas áreas na superfície de toque de um dispositivo de vídeo podem ser inacessíveis devido à postura do dedo e à posição do punho do usuário sobre o dispositivo.</td><td>O mouse e a caneta podem atingir qualquer parte da tela, embora qualquer controle deva ser acessado pelo teclado por meio da ordem de tabulação. </td><td>A postura do dedo e a alça podem ser um problema.</td></tr>
<tr><td>Os objetos podem ser obscurecidos por uma ou mais pontas de dedos ou pela mão do usuário. Isso é conhecido como oclusão.</td><td>Os dispositivos de entrada indireta não causam oclusão.</td><td>Igual ao mouse.</td></tr>
<tr><td>Estado do objeto</td><td>O toque usa um modelo de dois estados: a superfície de toque de um dispositivo de vídeo é tocada (on) ou não (off). Não há estado de foco que possa disparar feedback visual adicional.</td><td>
<p>Um mouse, caneta/stylus e teclado, todos expõem um modelo de três estados: acima (off), abaixo (on) (ativado), e focalizar (foco).</p>
<p>O estado focalizar permite que os usuários explorem e aprendam usando as dicas de ferramentas associadas aos elementos IU. Os efeitos de focalizar e foco podem retransmitir os objetos que são interativos e também ajudar no direcionamento. 
</p>
</td><td>Igual ao mouse.</td></tr>
<tr><td rowspan="2">Interação sofisticada</td><td>Suporta multitoque, pontos de entrada múltiplos (pontas dos dedos) em uma superfície de toque.</td><td>Suporta um ponto de entrada único.</td><td>Igual ao toque.</td></tr>
<tr><td>Suporta a manipulação direta de objetos através de gestos como tocar, arrastar, deslizar, apertar e girar.</td><td>Sem suporte para manipulação direta quando o mouse, caneta e teclado são dispositivos de entrada indiretos.</td><td>Igual ao mouse.</td></tr>
</tbody></table>



**Observação**  entrada indireta teve a vantagem de mais de 25 anos de refinamento. Recursos como dicas de ferramentas disparadas por focalização foram projetadas para solucionar a exploração da interface do usuário especificamente para entradas por touchpad, mouse, caneta e teclado. Elementos da interface do usuário como esses foram reprojetados para uma experiência avançada fornecida pela entrada de toque, sem comprometer a experiência do usuário para outros dispositivos.

 

## <a name="use-touch-feedback"></a>Usar comentário por toque

O feedback visual apropriado durante as interações com o seu aplicativo ajuda os usuários a reconhecer, aprender e se adaptar a como suas interações são interpretadas pelo aplicativo e o Windowsplatform. O feedback visual pode indicar interações bem-sucedidas, retransmitir o status do sistema, aprimorar o sentido de controle, reduzir erros, ajudar os usuários a entenderem o sistema e os dispositivos de entrada, além de estimular a interação.

A resposta visual é importante quando o usuário recorre à entrada por toque em atividades que exigem exatidão e precisão com base no local. A exibição do feedback sempre que a entrada por toque for detectada ajudará o usuário a entender as regras de direcionamento personalizadas definidas pelo aplicativo e seus respectivos controles.


## <a name="targeting"></a>Direcionamento

O direcionamento é otimizado por meio de:

-   Tamanhos de destino de toque

    Diretrizes claras sobre tamanho garantem que os aplicativos proporcionem uma interface do usuário agradável com objetos e controles de direcionamento fácil e seguro.

-   Geometria de contato

    Toda a área de contato do dedo determina o objeto de destino mais provável.

-   Anulação

    Os itens em um grupo são redirecionados com facilidade arrastando o dedo entre eles (por exemplo, botões de opção). O item atual é ativado quando o toque é liberado.

-   Balanço

    Itens empacotados densamente (por exemplo, hiperlinks) são redirecionados com facilidade ao pressionar o dedo e, sem deslizar, balançá-lo para frente e para trás sobre os itens. Devido à oclusão, o item atual é identificado por uma dica de ferramenta ou pela barra de status e é ativado quando o toque é liberado.

## <a name="accuracy"></a>Precisão

Crie interações soltas usando:

-   Pontos de alinhamento que facilitam a parada nos locais desejados quando os usuários interagem com o conteúdo.
-   "Trilhos" direcionais que ajudam no movimento panorâmico vertical ou horizontal, mesmo quando a mão se desloca em um leve arco. Para obter mais informações, consulte [Diretrizes para movimento panorâmico](guidelines-for-panning.md).

## <a name="occlusion"></a>Oclusão

Para evitar a oclusão de dedo e mão:

-   Dimensione e posicione a interface do usuário.

    Deixe os elementos da interface do usuário grandes o suficiente para que não sejam completamente cobertos por uma área de contato da ponta do dedo.

    Posicione os menus e pop-ups sobre a área de contato sempre que possível.

-   Dicas de ferramenta

    Mostre dicas de ferramentas quando um usuário mantém o dedo em contato com um objeto. Isso é útil para descrever a funcionalidade do objeto. O usuário pode retirar a ponta do dedo para sair do objeto e assim evitar a invocação da dica de ferramenta.

    Para objetos pequenos, desloque as dicas de ferramenta para que não fiquem cobertas pela área de contato da ponta do dedo. Isso é útil para direcionamento.

-   Alças para precisão

    Quando a precisão é necessária (por exemplo, seleção de texto), inclua alças de seleção que sejam deslocadas para aumentar a precisão. Para saber mais, veja [Diretrizes para seleção de texto e imagens (aplicativos do Tempo de Execução do Windows)](guidelines-for-textselection.md).

## <a name="timing"></a>Tempo

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

    **Observação**uma exceção a isso ocorre quando você usa as interações específicas para ajudar no aprendizado e na exploração (por exemplo, pressionar e segurar).

     

-   Descrições e indicações visuais adequadas têm um grande efeito sobre o uso das interações avançadas.


## <a name="app-views"></a>Modos de exibição do aplicativo


Ajuste a experiência de interação do usuário nas configurações de movimento panorâmico/rolagem e zoom dos modos de exibição do aplicativo. O modo de exibição do aplicativo indica como o usuário acessa e manipula o aplicativo e seu conteúdo. Os modos de exibição também oferecem comportamentos, como inércia, salto de limite de conteúdo e pontos de alinhamento.

As configurações de movimento panorâmico e rolagem do controle [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) determinam como os usuários navegam em um único modo de exibição quando o respectivo conteúdo não cabe no visor. Um único modo de exibição pode ser, por exemplo, a página de uma revista ou de um livro, a estrutura de pastas de um computador, uma biblioteca de documentos ou um álbum de fotos.

As configurações de zoom aplicam-se ao zoom óptico (permitido pelo controle [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)) e ao controle [**Semantic Zoom**](https://msdn.microsoft.com/library/windows/apps/hh702601). O Zoom Semântico é uma técnica otimizada para toque para apresentação e navegação em grandes conjuntos de dados ou conteúdos relacionados dentro de um único modo de exibição. Ele funciona com o uso de dois modos distintos de classificação ou níveis de zoom. Análogo ao movimento panorâmico e à rolagem em um modo de exibição único. O movimento panorâmico e a rolagem podem ser usados em conjunto com o Zoom Semântico.

Use modos de exibição do aplicativo e eventos para modificar os comportamentos de movimento panorâmico/rolagem e zoom. Isso pode oferecer uma experiência de interação mais tranquila do que seria possível por meio da manipulação de eventos de ponteiro e gestos.

Para saber mais sobre modos de exibição do aplicativo, consulte [Controles, layouts e texto](https://msdn.microsoft.com/library/windows/apps/mt228348).

## <a name="custom-touch-interactions"></a>Interações por toque personalizadas


Caso você implemente o próprio suporte à interação, tenha em mente que os usuários esperam uma experiência intuitiva que envolva a interação direta com os elementos da interface do usuário do aplicativo. Recomendamos que você modele suas interações personalizadas nas bibliotecas de controles da plataforma para manter os elementos consistentes e detectáveis. Os controles nessas bibliotecas oferecem a experiência de interação completa do usuário, incluindo interações padrão, efeitos físicos animados, feedback visual e acessibilidade. Crie interações personalizadas somente se houver uma exigência clara e bem-definida e se as interações básicas não derem suporte ao seu cenário.

Para oferecer suporte a toque personalizado, você pode manipular vários eventos [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911). Esses eventos são agrupados em três níveis de abstração.

-   Os eventos de gesto estático são disparados depois que uma interação é concluída. Os eventos de gesto incluem [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985), [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922), [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) e [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928).

    Você pode desabilitar os eventos de gesto em elementos específicos definindo [**IsTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208939), [**IsDoubleTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208931), [**IsRightTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208937) e [**IsHoldingEnabled**](https://msdn.microsoft.com/library/windows/apps/br208935) como **false**.

-   Os eventos de ponteiro, como [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) e [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970), fornecem detalhes de baixo nível para cada contato por toque, incluindo o movimento do ponteiro e a capacidade de diferenciar eventos de pressionamento e liberação.

    Um ponteiro é um tipo genérico de entrada com um mecanismo de evento unificado. Ele exibe informações básicas, como a posição na tela, na fonte de entrada ativa, que pode ser touch, touchpad, mouse ou caneta.

-   Os eventos de gesto de manipulação, como [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950), indicam uma interação constante. Eles começam a ser acionados quando o usuário toca em um elemento, e continuam até o usuário retirar o dedo ou a manipulação ser cancelada.

    Os eventos de manipulação incluem interações multi-touch, como zoom, panorâmica ou giro, além de interações que usam dados de inércia e velocidade, como arrastar. As informações fornecidas pelos eventos de manipulação não identificam a forma de interação que foi realizada, mas incluem dados como posição, delta de conversão e velocidade. Você pode usar esses dados de toque para determinar o tipo de interação que deve ser realizado.

Aqui está o conjunto básico de gestos de toque aceitos pela UWP.

| Nome           | Tipo                 | Descrição                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| Toque            | Gesto estático       | Um dedo toca na tela e levanta.                                            |
| Pressionar e segurar | Gesto estático       | Um dedo toca a tela e permanece no local.                                      |
| Deslizar          | Gesto de manipulação | Um ou mais dedos tocam a tela e movem-se na mesma direção.                   |
| Passar o dedo          | Gesto de manipulação | Um ou mais dedos tocam a tela e movem-se por uma curta distância na mesma direção.  |
| Virar           | Gesto de manipulação | Dois ou mais dedos tocam a tela e movem-se em um arco em sentido horário ou anti-horário. |
| Pinçar          | Gesto de manipulação | Dois ou mais dedos tocam a tela e aproximam-se.                         |
| Ampliar        | Gesto de manipulação | Dois ou mais dedos tocam a tela e distanciam-se.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>Eventos de gesto


Para obter detalhes sobre controles individuais, consulte [Lista de controles](https://msdn.microsoft.com/library/windows/apps/mt185406).

## <a name="pointer-events"></a>Eventos de ponteiro


Os eventos de ponteiro são gerados por uma variedade de fontes de entrada ativa, incluindo touch, touchpad, caneta e mouse (eles substituem os eventos de mouse tradicionais).

Os eventos de ponteiro são baseados em um único ponto de entrada (dedos, caneta, cursor do mouse) e não dão suporte a interações baseadas na velocidade.

Aqui está uma lista de eventos de ponteiro e o argumento do evento relacionado.

| Evento ou classe                                                       | Descrição                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)             | Ocorre quando um único dedo toca na tela.               |
| [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)           | Ocorre quando esse mesmo contato por toque é levantado.                |
| [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)                 | Ocorre quando o ponteiro é arrastado pela tela.         |
| [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)             | Ocorre quando um ponteiro entra na área de teste de clique de um elemento. |
| [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)               | Ocorre quando um ponteiro sai da área de teste de clique de um elemento.  |
| [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)           | Ocorre quando um contato por toque é perdido de forma anormal.               |
| [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)     | Ocorre quando uma captura do ponteiro é obtida por outro elemento.    |
| [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)   | Ocorre quando o valor delta de uma roda do mouse é alterado.         |
| [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) | Fornece dados para todos os eventos de ponteiro.                         |

 

O exemplo a seguir mostra como usar os eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) e [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) para manipular uma interação por toque em um objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

Primeiro, um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) denominado `touchRectangle` é criado na linguagem XAML.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
Em seguida, os ouvintes dos eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) e [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) são especificados.

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

Por fim, o manipulador de eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) aumenta os valores de [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) do [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), enquanto os manipuladores de eventos [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) e [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) definem **Height** e **Width** como seus valores iniciais.

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>Eventos de manipulação


Use eventos de manipulação se precisar dar suporte a interações com vários dedos no seu aplicativo, ou interações que exijam dados de velocidade.

Você pode usar eventos de manipulação para detectar interações como arrastar, ampliar e segurar.

Veja a seguir uma lista de eventos de manipulação e argumentos de eventos relacionados.

| Evento ou classe                                                                                               | Descrição                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**Evento ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951)                                   | Ocorre quando o processador de manipulação é criado.                                                                                  |
| [**Evento ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)                                     | Ocorre quando um dispositivo de entrada inicia uma manipulação no [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911).                                            |
| [**Evento ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)                                         | Ocorre quando o dispositivo de entrada muda de posição durante a manipulação.                                                                      |
| [**Evento ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/hh702425)                | Ocorre quando o dispositivo de entrada perde contato com o objeto [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) durante a manipulação e a inércia começa. |
| [**Evento ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)                                 | Ocorre quando uma manipulação e inércia no [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) são concluídas.                                          |
| [**ManipulationStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702132)               | Fornece dados ao evento [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951).                                         |
| [**ManipulationStartedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702101)                 | Fornece dados ao evento [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950).                                           |
| [**Classe ManipulationDeltaRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702051)                     | Fornece dados ao evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946).                                               |
| [**Classe ManipulationInertiaStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702074) | Fornece dados ao evento [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947).                           |
| [**ManipulationVelocities**](https://msdn.microsoft.com/library/windows/apps/br242032)                                              | Descreve a velocidade de ocorrência de manipuladores.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702035)             | Fornece dados ao evento [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945).                                       |

 

Um gesto consiste em uma série de eventos de manipulação. Cada gesto começa com um evento [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950), como quando um usuário toca a tela.

Em seguida, um ou mais eventos [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) são disparados. Por exemplo, se você toca na tela e, em seguida, arrasta o dedo pela tela. Finalmente, um evento [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) é gerado quando a interação é concluída.

**Observação**se você não tiver um monitor de tela sensível ao toque, você pode testar seu código de eventos de manipulação no simulador usando uma interface de roda do mouse e mouse.

 

O exemplo a seguir mostra como usar os eventos [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) para manipular uma interação de deslizamento em um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) e movê-lo pela tela.

Primeiro, um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) denominado `touchRectangle` é criado em XAML com uma [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e uma [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de 200.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

Em seguida, uma [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027) global denominada `dragTranslation` é criada para converter o [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Um ouvinte de evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) é especificado no **Rectangle** e `dragTranslation` é adicionado à [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980) do **Rectangle**.

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

Finalmente, no manipulador de eventos [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946), a posição de [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) é atualizada com [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027) na propriedade [**Delta**](https://msdn.microsoft.com/library/windows/apps/hh702058).

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>Eventos roteados


Todos os eventos de ponteiro, gesto e manipulação mencionados aqui são implementados como *eventos roteados*. Isso significa que o evento provavelmente pode ser manipulado por objetos que não sejam o que originalmente disparou o evento. Os pais sucessivos em uma árvore de objetos, como os contêineres pai de um [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) ou a [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) raiz do seu aplicativo, podem optar por manipular esses eventos mesmo que o elemento original não faça isso. Reciprocamente, qualquer objeto que manipule o evento pode marcar o evento como manipulado para que ele não atinja mais nenhum elemento pai. Para saber mais sobre o conceito de evento roteado e como ele influencia na forma como você grava manipuladores para eventos roteados, consulte [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/hh758286).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Aplicativos de design com interação por toque como o método de entrada primário esperado.
-   Fornecer feedback visual para interações de todos os tipos (toque, caneta, stylus, mouse etc.)
-   Otimize alvos ajustando tamanho do alvo de toque, geometria de contato, esfregar e balançar.
-   Otimize a acuidade por meio do uso de pontos de ajuste e "trilhos" direcionais.
-   Forneça dicas de ferramenta e manipuladores para auxiliar a melhorar a acuidade de toque de itens da interface do usuário com pouco espaçamento.
-   Não use interações cronometradas sempre que possível (exemplos de uso apropriado: tocar e segurar).
-   Não use o número de dedos usados para distinguir a manipulação sempre que possível.


## <a name="related-articles"></a>Artigos relacionados

* [Identificar entrada do ponteiro](handle-pointer-input.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)

**Exemplos**

* [Amostra de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo morto**

* [Entrada: amostra de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: gestos e interações com o GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 




