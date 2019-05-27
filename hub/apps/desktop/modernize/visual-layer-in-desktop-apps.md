---
title: Modernize seu aplicativo da área de trabalho usando a camada Visual
description: Use a camada Visual para aprimorar a interface do usuário do seu aplicativo da área de trabalho do .NET ou Win32.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215172"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Usando a camada Visual nos aplicativos da área de trabalho

Agora você pode usar as APIs da UWP em aplicativos de área de trabalho não UWP para aprimorar a aparência, a aparência e a funcionalidade do seu WPF, Windows Forms, e C++ aplicativos do Win32 e tirar proveito dos recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de UWP.

Para muitos cenários, você pode usar [Ilhas XAML](xaml-islands.md) para adicionar controles XAML modernos ao seu aplicativo. No entanto, quando você precisar criar experiências personalizadas que vão além dos controles internos, você pode acessar a APIs de camada Visual.

A camada Visual fornece um API de modo retido para gráficos, efeitos e animações de alto desempenho. É a base para a interface do usuário em dispositivos Windows 10. Controles UWP XAML são criados na camada Visual, e permite que muitos aspectos do [Fluent Design System](/windows/uwp/design/fluent-design-system/index), como a luz, profundidade, movimento, Material e escala.

![Interface do usuário criada com a camada visual](images/visual-layer-interop/pull-to-animate.gif)

> _Interface do usuário criada com a camada visual_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Criar uma interface do usuário visualmente atraentes em qualquer aplicativo do Windows

Camada Visual permite que você crie experiências envolventes usando composição leve de conteúdo personalizado desenhado (visuais) e aplicar animações poderosas, efeitos e manipulações nesses objetos em seu aplicativo. Camada Visual não substitui qualquer estrutura de interface do usuário existente; em vez disso, ele é um suplemento valioso para essas estruturas.

Você pode usar a camada Visual para dar uma aparência exclusiva de seu aplicativo e estabelecer uma identidade que o diferencia dos outros aplicativos. Ele também permite que os princípios de Design Fluent, que são projetados para tornar seus aplicativos mais fáceis de usar, de desenho mais envolvimento dos usuários. Por exemplo, você pode usá-lo para criar visuais e transições de tela animadas que mostram as relações entre itens na tela.

## <a name="visual-layer-features"></a>Recursos de camada Visual

### <a name="brushes"></a>Pincéis

[Pincéis de composição](/windows/uwp/composition/composition-brushes) permitem que você pinte objetos de interface do usuário com cores sólidas, gradientes, imagens, vídeos, efeitos complexos e muito mais.

![Um ovo gerado com o criador de Material](images/visual-layer-interop/egg.gif)

> _Um ovo criado com o [aplicativo de demonstração do criador de Material](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Efeitos

[Efeitos de composição](/windows/uwp/composition/composition-effects) incluem a luz e sombra uma lista dos efeitos de filtro. Eles podem ser animados, personalizados e encadeados e aplicados diretamente aos visuais. O SceneLightingEffect pode ser combinado com a iluminação de composição para criar a atmosfera, profundidade e materiais.

![As luzes e material](images/visual-layer-interop/light-interop.gif)

> _Luzes e material demonstradas a [Galeria de exemplos de composição de interface do usuário do Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animações

[Animações de composição](/windows/uwp/composition/composition-animation) executados diretamente no processo do compositor, independente do thread da interface do usuário. Isso garante a suavidade e escala, para que você possa executar grandes números de animações simultâneas, explícitas. Além de animações de quadro-chave familiares para fazer alterações de propriedade ao longo do tempo, você pode usar expressões para definir as relações de matemática entre as diferentes propriedades, incluindo a entrada do usuário. Animações orientado por entrada permitem a criação da interface do usuário dinâmica e fluida responde à entrada do usuário, que pode resultar em maior engajamento do usuário.

![Interface do usuário criada com a camada visual](images/visual-layer-interop/swipe-scroller.gif)

> _Movimento demonstradas a [Galeria de exemplos de composição de interface do usuário do Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Mantenha sua base de código existente e adotar incrementalmente

O código em seus aplicativos existentes representa um investimento significativo que você não deseja perder. Você pode migrar _Ilhas_ de conteúdo para usar a camada Visual e manter o restante da interface do usuário em sua estrutura existente. Isso significa que você pode fazer atualizações significativas e aprimoramentos para o seu aplicativo da interface do usuário sem precisar fazer grandes alterações em seu código existente base.

## <a name="samples-and-tutorials"></a>Exemplos e tutoriais

Saiba como usar a camada Visual em seus aplicativos experimentando os nossos exemplos. Esse exemplos e tutoriais ajudam a começar a usar a camada Visual e mostrar como funcionam os recursos.

### <a name="win32"></a>Win32

- [Usando a camada Visual com o Win32](using-the-visual-layer-with-win32.md) tutorial
  - [Exemplo de composição Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Exemplo de vetores de Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Exemplo de superfícies virtual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Exemplo de captura de tela](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [Usando a camada Visual com o Windows Forms](using-the-visual-layer-with-windows-forms.md) tutorial
  - [Exemplo de composição Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Exemplo de integração de camada Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [Usando a camada Visual com o WPF](using-the-visual-layer-with-wpf.md) tutorial
  - [Exemplo de composição Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Exemplo de integração de camada Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Exemplo de captura de tela](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Limitações

Embora muitos recursos de camada Visual funcionam da mesma forma quando hospedado em um aplicativo de área de trabalho, como fazem em um aplicativo UWP, alguns recursos têm limitações. Aqui estão algumas das limitações a serem consideradas:

- Contam com cadeias de efeito [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) para as descrições de efeito. O [pacote do NuGet Win2D](https://www.nuget.org/packages/Win2D.uwp) não tem suporte em aplicativos da área de trabalho, portanto, você precisará recompilá-lo do [código-fonte](https://github.com/Microsoft/Win2D).
- Para o teste de clique, você precisa fazer cálculos de limites percorrendo a árvore visual por conta própria. Isso é o mesmo que a camada Visual na UWP, exceto nesse caso, não há nenhum elemento XAML, que você pode associar facilmente a para teste de clique.
- Camada Visual não tem um primitivo para renderização de texto.
- Quando duas tecnologias de interface do usuário diferentes são usadas juntos, como o WPF e camada Visual, eles estão responsável por desenhar seus próprios pixels na tela, e eles não podem compartilhar pixels. Como resultado, o conteúdo da camada Visual sempre é renderizado sobre outros tipos de conteúdo da interface do usuário. (Isso é conhecido como o _espaço aéreo_ problema.) Talvez você precise fazer codificação extra e testes para garantir que seu conteúdo de camada Visual é redimensionado com o host da interface do usuário e não occlude outros tipos de conteúdo.
- Conteúdo hospedado em um aplicativo de desktop não automaticamente redimensionar ou dimensionar o DPI. Etapas adicionais podem necessárias para garantir que seu conteúdo lida com alterações DPI. (Consulte os tutoriais específicos de plataforma para obter mais informações.)

## <a name="additional-resources"></a>Recursos adicionais

- [Camada visual](/windows/uwp/composition/visual-layer)
- [Composição visual](/windows/uwp/composition/composition-visual-tree)
- [Pincéis de composição](/windows/uwp/composition/composition-brushes)
- [Efeitos de composição](/windows/uwp/composition/composition-effects)
- [Animações de composição](/windows/uwp/composition/composition-animation)

Referência de API

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)