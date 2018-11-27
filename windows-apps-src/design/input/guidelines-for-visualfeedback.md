---
Description: Use visual feedback to show users when their interactions with a UWP app are detected, interpreted, and handled.
title: Feedback visual
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: feedback visual, feedback de foco, feedback de toque, visualização de contato, entrada, interação
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b043ec71eb7d5883a1b22c4f0d8f43824034d454
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7827820"
---
# <a name="guidelines-for-visual-feedback"></a>Diretrizes de feedback visual

Use o feedback visual para mostrar aos usuários quando suas interações são detectadas, interpretadas e manipuladas. O feedback visual poderá ajudar os usuários incentivando a interação. Ele indica o sucesso da interação, o que oferece ao usuário uma sensação de controle. Ele também retransmite o status do sistema e reduz os erros.

> **APIs importantes**:  [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)

## <a name="recommendations"></a>Recomendações

- Tente limitar modificações de um modelo de controle para aqueles diretamente relacionados à sua intenção de design, pois grandes alterações podem afetar o desempenho e a acessibilidade do controle e de seu aplicativo. 
    - Consulte [estilos XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) para obter mais informações sobre como personalizar as propriedades de um controle, incluindo propriedades de estado visual.
    - Consulte a [Classe UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) para obter mais detalhes sobre como fazer alterações em um modelo de controle
    - Considere criar seu próprio controle de modelo personalizado se precisar fazer alterações significativas em um modelo de controle. Para obter um exemplo de um controle de modelo personalizado, consulte o [exemplo de Controle de edição personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- Não use visualizações de toque em situações em que possam interferir com o uso do aplicativo. Para obter mais informações, consulte [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969).
- Não exiba comentários a menos que seja absolutamente necessário. Mantenha a interface do usuário clara e organizada sem mostrar o feedback visual, a menos que você esteja agregando valor que não esteja disponível em nenhum outro lugar.
- Tente não personalizar os comportamentos do feedback visual dos gestos internos do Windows em excesso, porque isso pode criar uma experiência inconsistente e confusa para o usuário.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicionais

As visualizações por contato são especialmente críticas para as interações por toque que exigem exatidão e precisão. Por exemplo, seu app deve indicar claramente o local de um toque para permitir que um usuário saiba se errou o seu destino, o quanto errou e quais ajustes deve fazer.

Usar os controles da plataforma padrão XAML disponíveis garante que o aplicativo funcione corretamente em todos os dispositivos e em situações de entrada. Se o seu aplicativo permitir interações personalizadas que exijam comentários personalizados, você deverá garantir que os comentários sejam adequados, que eles se propaguem em dispositivos de entrada e que não distraiam o usuário da tarefa. Isso pode ser um problema específico em aplicativos de jogos ou de desenho, em que o feedback visual pode entrar em conflito ou até mesmo obscurecer uma interface do usuário essencial.

> [!Important]
> Não recomendamos mudar o comportamento da interação dos gestos internos.

**Comentários em todos os dispositivos**

O feedback visual geralmente depende do dispositivo de entrada (toque, touchpad, mouse, caneta, teclado, etc.). Por exemplo, o feedback interno de um mouse geralmente envolve movimentar e mudar o cursor, enquanto o toque e a caneta exigem visualizações de contato e entrada de teclado e a navegação usa retângulos de foco e destaque.

Use [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) para configurar o comportamento de feedback para gestos da plataforma.

Se você estiver personalizando a interface do usuário de feedback, lembre-se de fornecer feedback que dê suporte e seja adequado a todos os modos de entrada.

Estes são alguns exemplos de visualizações internas de contato do Windows.

| ![Comentários do toque](images/TouchFeedback.png) | ![Comentários do mouse](images/MouseFeedback.png) | ![Comentários da caneta](images/PenFeedback.png) | ![Comentários do teclado](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualização por toque | Visualização por mouse/touchpad | Visualização por caneta | Visualização por teclado |

## <a name="high-visibility-focus-visuals"></a>Elementos visuais de foco de alta visibilidade

Todos os aplicativos do Windows ostentam elementos visual de foco mais definido em torno de controles interativos do aplicativo. Esses novos elementos visuais de foco são totalmente personalizáveis, assim como desativáveis quando necessário.

Para a **experiência de 10 pés** típica do uso de Xbox e programa de TV, o Windows oferece suporte ao **Foco de revelação**, um efeito de iluminação que anima a borda de elementos de foco, como um botão, quando estão em foco pela entrada do gamepad ou teclado. Para obter mais informações, consulte [Projetar para Xbox e TV](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus).

## <a name="color-branding--customizing"></a>Personalização e identidade visual de cores

**Propriedades da borda**

Há duas partes nos elementos visuais de foco de alta visibilidade: a borda principal e a borda secundária. A borda principal apresenta espessura de **2px** e é moldada em torno da parte *externa* da borda secundária. A borda secundária apresenta espessura de **1px** e é moldada em torno da parte *interna* da borda secundária.
![Linhas vermelhas dos elementos visuais de foco de alta visibilidade](images/FocusRectRedlines.png)

Para alterar a espessura de qualquer tipo de borda (principal ou secundária), use **FocusVisualPrimaryThickness** ou **FocusVisualSecondaryThickness**, respectivamente:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Espessuras de margem do elemento visual de foco de alta visibilidade](images/FocusMargin.png)

A margem é uma propriedade do tipo [**Thickness**](https://msdn.microsoft.com/library/system.windows.thickness); portanto, a margem pode ser personalizada para aparecer somente em determinados lados do controle. Veja abaixo: ![Apenas a espessura da margem do elemento visual de foco de alta visibilidade inferior](images/FocusThicknessSide.png)

A margem é o espaço entre os limites do controle visual e o início da *borda secundária* dos elementos visuais de foco. A margem padrão está **1px** além dos limites de controle. Você pode editar essa margem em uma base por controle. Basta alterar a propriedade **FocusVisualMargin**:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Diferenças de margem dos elementos visuais de foco de alta visibilidade](images/FocusPlusMinusMargin.png)

*Uma margem negativa colocará a borda além do centro do controle, e uma margem positiva moverá a borda para mais perto do centro do controle.*

Para desativar totalmente os elementos visuais de foco no controle, basta desabilitar **UseSystemFocusVisuals**:
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

A espessura, a margem ou a opção do desenvolvedor do aplicativo de ter ou não os elementos visuais de foco são determinadas com base no controle.

**Propriedades de cor**

Há somente duas propriedades de cor para os elementos visuais de foco: a cor da borda principal e a cor da borda secundária. Essas cores de borda dos elementos visuais podem ser alteradas por controle no nível da página e globalmente no nível do aplicativo:

Para usar a identidade visual dos elementos visuais de foco em todo o aplicativo, substitua os pincéis do sistema:
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![Alterações de cor dos elementos visuais de foco de alta visibilidade](images/FocusRectColorChanges.png)

Para alterar as cores em uma base por controle, edite as propriedades dos elementos visuais de foco no controle desejado:
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>Artigos relacionados

**Para designers**
* [Diretrizes de movimento panorâmico](guidelines-for-panning.md)

**Para desenvolvedores**
* [Interações personalizadas do usuário](https://msdn.microsoft.com/library/windows/apps/mt185599)

**Exemplos**
* [Amostra de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo-morto**
* [Entrada: amostra de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de teste de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: amostra de gestos no Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: amostra de manipulações e gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Amostra de entrada por toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 
