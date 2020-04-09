---
title: Modernizar seu aplicativo da área de trabalho usando a Camada visual
description: Use a Camada visual para aprimorar a interface do usuário de seu aplicativo da área de trabalho Win32 ou .NET.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215172"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Usar a Camada visual em aplicativos da área de trabalho

Agora, você pode usar as APIs da UWP em aplicativos da área de trabalho não UWP para aprimorar a aparência e a funcionalidade de seus aplicativos WPF, Windows Forms e Win32 C++, além de tirar proveito dos recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio da UWP.

Para muitos cenários, você pode usar [ilhas XAML](xaml-islands.md) para adicionar controles XAML modernos ao seu aplicativo. No entanto, quando você precisa criar experiências personalizadas que vão além dos controles internos, é possível acessar as APIs da Camada visual.

A Camada visual fornece uma API de modo retido de alto desempenho para elementos gráficos, efeitos e animações. Ela é a base para a interface do usuário em dispositivos Windows 10. Os controles XAML da UWP são criados na Camada visual e permitem muitos aspectos do [Sistema de Design Fluente](/windows/uwp/design/fluent-design-system/index), como Iluminação, Profundidade, Movimento, Material e Escala.

![Interface do usuário criada com a Camada visual](images/visual-layer-interop/pull-to-animate.gif)

> _Interface do usuário criada com a Camada visual_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Criar uma interface do usuário visualmente atraente em qualquer aplicativo do Windows

A Camada visual permite que você crie experiências envolventes usando a composição leve de conteúdo desenhado personalizado (visuais) e a aplicação de animações, efeitos e manipulações avançadas nesses objetos em seu aplicativo. A Camada visual não substitui nenhuma estrutura da IU existente; em vez disso, é um complemento valioso para essas estruturas.

Você pode usar a Camada visual para dar ao seu aplicativo uma aparência única e estabelecer uma identidade que o diferencie de outros aplicativos. Ela também habilita os princípios de Design Fluente, projetados para facilitar o uso de seus aplicativos, gerando um maior envolvimento dos usuários. Por exemplo, você pode usar isso para criar indicações visuais e transições de tela animadas que mostram as relações entre os itens na tela.

## <a name="visual-layer-features"></a>Recursos da Camada visual

### <a name="brushes"></a>Pincéis

Os [pincéis de composição](/windows/uwp/composition/composition-brushes) permitem que você pinte objetos da interface do usuário com cores sólidas, gradientes, imagens, vídeos, efeitos complexos e muito mais.

![Um ovo criado com o Criador de Materiais](images/visual-layer-interop/egg.gif)

> _Um ovo criado com o [aplicativo de demonstração do Criador de Materiais](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Efeitos

Os [Efeitos de composição](/windows/uwp/composition/composition-effects) incluem luz, sombra e uma lista de efeitos de filtro. Eles podem ser animados, personalizados, encadeados e aplicados diretamente aos visuais. O SceneLightingEffect pode ser combinado com a iluminação de composição para criar atmosfera, profundidade e materiais.

![Luzes e materiais](images/visual-layer-interop/light-interop.gif)

> _Luzes e materiais demonstrados na [Galeria de exemplos de Composição da interface do usuário do Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animações

As [Animações de composição](/windows/uwp/composition/composition-animation) são executadas diretamente no processo do compositor, independentemente do thread de interface do usuário. Isso garante simplicidade e escala, para que você possa executar grandes quantidades de animações simultâneas e explícitas. Além das animações conhecidas do KeyFrame para conduzir as alterações de propriedade ao longo do tempo, você pode usar expressões para configurar relações matemáticas entre diferentes propriedades, incluindo entrada do usuário. As animações controladas por entrada permitem criar uma interface do usuário que responde de forma dinâmica e fluida à entrada dos usuários, o que pode resultar em uma maior participação deles.

![Interface do usuário criada com a camada visual](images/visual-layer-interop/swipe-scroller.gif)

> _Movimento demonstrado na [Galeria de exemplos de Composição da interface do usuário do Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Manter sua base de código existente e adotar de forma incremental

O código em seus aplicativos existentes representa um investimento significativo que você não quer perder. Você pode migrar _ilhas_ de conteúdo para usar a Camada visual e manter o restante da interface do usuário em sua estrutura existente. Isso significa que você pode fazer atualizações e aprimoramentos significativos em sua interface do usuário do aplicativo sem a necessidade de fazer grandes alterações em sua base de código existente.

## <a name="samples-and-tutorials"></a>Exemplos e tutoriais

Saiba como usar a Camada visual em seus aplicativos experimentando nossos exemplos. Estes exemplos e tutoriais ajudam você a começar a usar a Camada visual e mostram como os recursos funcionam.

### <a name="win32"></a>Win32

- Tutorial [Usar a Camada visual com Win32](using-the-visual-layer-with-win32.md)
  - [Exemplo de HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Exemplo de HelloVectors](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Exemplo de Superfícies virtuais](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Exemplo de Captura de tela](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- Tutorial [Usar a Camada visual com o Windows Forms](using-the-visual-layer-with-windows-forms.md)
  - [Exemplo de HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Exemplo de integração de Camada visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- Tutorial [Usar a Camada visual com WPF](using-the-visual-layer-with-wpf.md)
  - [Exemplo de HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Exemplo de integração de Camada visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Exemplo de Captura de tela](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Limitações

Embora muitos recursos da Camada visual funcionem da mesma forma quando hospedados em um aplicativo da área de trabalho ou em um aplicativo UWP, alguns recursos apresentam limitações. Aqui estão algumas das limitações a serem consideradas:

- As cadeias de efeito dependem do [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) para as descrições de efeito. O [pacote NuGet do Win2D](https://www.nuget.org/packages/Win2D.uwp) não é compatível com aplicativos da área de trabalho, portanto, você precisará recompilá-lo do [código-fonte](https://github.com/Microsoft/Win2D).
- Para fazer testes de clique, você precisa calcular os limites percorrendo a árvore visual por conta própria. Isso é o mesmo que a Camada visual na UWP, exceto que, neste caso, não há nenhum elemento XAML que você possa associar facilmente para testes de clique.
- A Camada visual não tem um primitivo para renderizar o texto.
- Quando duas tecnologias de interface do usuário diferentes são usadas juntas, como o WPF e a Camada visual, cada uma delas é responsável por desenhar seus próprios pixels na tela e não podem compartilhar pixels. Como resultado, o conteúdo da Camada visual sempre é renderizado em cima de outro conteúdo da interface do usuário. (Isso é conhecido como problema de _espaço aéreo_.) Talvez seja necessário fazer codificação e teste adicionais para garantir que o conteúdo da Camada visual seja redimensionado com a interface do usuário do host e não oculte outro conteúdo.
- O conteúdo hospedado em um aplicativo da área de trabalho não é redimensionado ou dimensionado automaticamente para DPI. Podem ser necessárias etapas adicionais para garantir que o conteúdo lide com as alterações de DPI. (Confira os tutoriais específicos da plataforma para obter mais informações.)

## <a name="additional-resources"></a>Recursos adicionais

- [Camada visual](/windows/uwp/composition/visual-layer)
- [Visual de composição](/windows/uwp/composition/composition-visual-tree)
- [Pincéis de composição](/windows/uwp/composition/composition-brushes)
- [Efeitos de composição](/windows/uwp/composition/composition-effects)
- [Animações de composição](/windows/uwp/composition/composition-animation)

Referência de API

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)