---
Description: Use comentários visuais para mostrar os usuários quando suas interações com um aplicativo do Windows forem detectadas, interpretadas e manipuladas.
title: Feedback visual
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: feedback visual, feedback de foco, feedback de toque, visualização de contato, entrada, interação
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fcb6945c488bc1b715c339fa39949ea62bdb2a12
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970071"
---
# <a name="guidelines-for-visual-feedback"></a>Diretrizes de feedback visual

Use o feedback visual para mostrar aos usuários quando suas interações são detectadas, interpretadas e manipuladas. O feedback visual poderá ajudar os usuários incentivando a interação. Ele indica o sucesso da interação, o que oferece ao usuário uma sensação de controle. Ele também retransmite o status do sistema e reduz os erros.

> **APIs importantes**:  [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>Recomendações

- Tente limitar modificações de um modelo de controle para aqueles diretamente relacionados à sua intenção de design, pois grandes alterações podem afetar o desempenho e a acessibilidade do controle e de seu aplicativo. 
    - Consulte [estilos XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) para obter mais informações sobre como personalizar as propriedades de um controle, incluindo propriedades de estado visual.
    - Consulte a [Classe UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) para obter mais detalhes sobre como fazer alterações em um modelo de controle
    - Considere criar seu próprio controle de modelo personalizado se precisar fazer alterações significativas em um modelo de controle. Para obter um exemplo de um controle de modelo personalizado, consulte o [exemplo de Controle de edição personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- Não use visualizações de toque em situações em que possam interferir com o uso do aplicativo. Para obter mais informações, consulte [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback).
- Não exiba comentários a menos que seja absolutamente necessário. Mantenha a interface do usuário clara e organizada sem mostrar o feedback visual, a menos que você esteja agregando valor que não esteja disponível em nenhum outro lugar.
- Tente não personalizar os comportamentos do feedback visual dos gestos internos do Windows em excesso, porque isso pode criar uma experiência inconsistente e confusa para o usuário.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional

As visualizações por contato são especialmente críticas para as interações por toque que exigem exatidão e precisão. Por exemplo, seu app deve indicar claramente o local de um toque para permitir que um usuário saiba se errou o seu destino, o quanto errou e quais ajustes deve fazer.

Usar os controles da plataforma padrão XAML disponíveis garante que o aplicativo funcione corretamente em todos os dispositivos e em situações de entrada. Se o seu aplicativo permitir interações personalizadas que exijam comentários personalizados, você deverá garantir que os comentários sejam adequados, que eles se propaguem em dispositivos de entrada e que não distraiam o usuário da tarefa. Isso pode ser um problema específico em aplicativos de jogos ou de desenho, em que o feedback visual pode entrar em conflito ou até mesmo obscurecer uma interface do usuário essencial.

> [!Important]
> Não recomendamos mudar o comportamento da interação dos gestos internos.

**Comentários em todos os dispositivos**

O feedback visual geralmente depende do dispositivo de entrada (toque, touchpad, mouse, caneta, teclado, etc.). Por exemplo, o feedback interno de um mouse geralmente envolve movimentar e mudar o cursor, enquanto o toque e a caneta exigem visualizações de contato e entrada de teclado e a navegação usa retângulos de foco e destaque.

Use [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback) para configurar o comportamento de feedback para gestos da plataforma.

Se você estiver personalizando a interface do usuário de feedback, lembre-se de fornecer feedback que dê suporte e seja adequado a todos os modos de entrada.

Estes são alguns exemplos de visualizações internas de contato do Windows.

| ![Comentários do toque](images/TouchFeedback.png) | ![Comentários do mouse](images/MouseFeedback.png) | ![Comentários da caneta](images/PenFeedback.png) | ![Comentários do teclado](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualização por toque | Visualização por mouse/touchpad | Visualização por caneta | Visualização por teclado |

## <a name="high-visibility-focus-visuals"></a>Elementos visuais de foco de alta visibilidade

Todos os aplicativos do Windows ostentam elementos visual de foco mais definido em torno de controles interativos do aplicativo. Esses novos elementos visuais de foco são totalmente personalizáveis, assim como desativáveis quando necessário.

Para a **experiência de 10 pés** típica do uso de Xbox e programa de TV, o Windows oferece suporte ao **Foco de revelação**, um efeito de iluminação que anima a borda de elementos de foco, como um botão, quando estão em foco pela entrada do gamepad ou teclado. Para obter mais informações, consulte [Projetar para Xbox e TV](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus).

## <a name="color-branding--customizing"></a>Personalização e identidade visual de cores

### <a name="border-properties"></a>Propriedades da borda

Há duas partes nos elementos visuais de foco de alta visibilidade: a borda principal e a borda secundária. A borda principal tem espessura de **2px** e é traçada em torno da parte *externa* da borda secundária. A borda secundária tem espessura de **1px** e é traçada em torno da parte *interna* da borda principal.
![Linhas vermelhas dos elementos visuais de foco de alta visibilidade](images/FocusRectRedlines.png)

Para alterar a espessura de qualquer tipo de borda (principal ou secundária), use **FocusVisualPrimaryThickness** ou **FocusVisualSecondaryThickness**, respectivamente:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Espessuras de margem do elemento visual de foco de alta visibilidade](images/FocusMargin.png)

A margem é uma propriedade do tipo [**Thickness**](https://docs.microsoft.com/dotnet/api/system.windows.thickness); portanto, a margem pode ser personalizada para aparecer somente em determinados lados do controle. Veja abaixo: ![foco na alta visibilidade de margens visuais de margem Visual somente inferior](images/FocusThicknessSide.png)

A margem é o espaço entre os limites visuais do controle e o início da *borda secundária*dos visuais de foco. A margem padrão é **1px** fora dos limites do controle. Você pode editar essa margem em uma base por controle, alterando a propriedade **FocusVisualMargin** :
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

### <a name="color-properties"></a>Propriedades de cor

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

### <a name="for-designers"></a>Para designers

- [Diretrizes de movimento panorâmico](guidelines-for-panning.md)

### <a name="for-developers"></a>Para desenvolvedores

- [Interações personalizadas do usuário](https://docs.microsoft.com/windows/uwp/design/layout/index)

### <a name="samples"></a>Exemplos

- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Amostra de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemplos de arquivo-morto

- [Entrada: amostra de eventos de entrada do usuário XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: amostra de funcionalidades do dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: amostra de teste de hit de toque](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemplo de rolagem, panorâmica e zoom do XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: amostra de tinta simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: amostra de gestos no Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: exemplo de manipulações e gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Amostra de entrada por toque do DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 
