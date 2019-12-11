---
title: Raio de canto
description: Saiba mais sobre os princípios de cantos arredondados, abordagens de design e opções de personalização.
ms.date: 10/08/2019
ms.topic: article
keywords: Windows 10, UWP, raio do canto, arredondado
ms.openlocfilehash: 84cd27bf8c65ed65a6ee2b0f044e0ffb3ef86bf0
ms.sourcegitcommit: 49af415e4eefea125c023b7071adaa5dc482e223
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74799920"
---
# <a name="corner-radius"></a>Raio de canto

Da versão 2.2 em diante da WinUI ([Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/)), o estilo padrão para muitos controles foi atualizado para usar cantos arredondados. Esses novos estilos destinam-se a evocar familiaridade e confiança e tornam a interface do usuário mais fácil para os usuários processarem visualmente.

Aqui estão dois controles de botão, os primeiros sem cantos arredondados e o segundo usando o novo estilo de canto arredondado.

![Botões com e sem cantos arredondados](images/rounded-corner/my-button.png)

Quando você instala o pacote NuGet para o WinUI 2.2 ou posterior, os novos estilos padrão são instalados para a plataforma e os controles da WinUI. Esses estilos são usados automaticamente quando você usa o WinUI 2.2 em seu aplicativo. Você não precisa executar nenhuma ação adicional para usar os novos estilos. No entanto, posteriormente neste artigo, mostraremos como personalizar os cantos arredondados se você precisar fazer isso.

> [!IMPORTANT]
> Alguns controles estão disponíveis na plataforma ([Windows.UI.XAML.Controls](/uwp/api/windows.ui.xaml.controls)) e em WinUI ([Microsoft.UI.XAML.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2)); por exemplo, **TreeView** ou **ColorPicker**. Ao usar o WinUI em seu aplicativo, você deve usar a versão WinUI do controle. O arredondamento de canto pode ser aplicado de forma inconsistente na versão da plataforma quando usado com WinUI.

> **APIs importantes**: [Propriedade Control.CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>Designs de controle padrão

Há três áreas dos controles em que os estilos de canto arredondados são usados: elementos retangulares, elementos de submenu e elementos de barra.

### <a name="corners-of-rectangle-ui-elements"></a>Cantos dos elementos de interface do usuário do retângulo

- Esses elementos de interface do usuário incluem controles básicos como botões que os usuários veem na tela sempre.
- O valor do raio padrão que usamos para esses elementos de interface do usuário é **2px**.

![Botão com cantos arredondados realçados](images/rounded-corner/button.png)

**Controles**

- AutoSuggestBox
- Botão
  - Botões de ContentDialog
- CalendarDatePicker
- CheckBox
  - Caixas de seleção múltipla TreeView
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>Cantos dos elementos de interface do usuário de submenu e sobreposição

- Eles podem ser elementos de interface do usuário transitórios que aparecem na tela temporariamente, como MenuFlyout, ou elementos que se sobrepõem a outra interface do usuário, como guias TabView.
- O valor do raio padrão que usamos para esses elementos de interface do usuário é **4px**.

![Exemplo de submenu](images/rounded-corner/flyout.png)

**Controles**

- CommandBarFlyout
- ContentDialog
- Submenu
- MenuFlyout
- Guias TabView
- TeachingTip
- ToolTip
- Parte do submenu (quando aberto)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>Elementos da barra

- Esses elementos da interface do usuário são formatados como barras ou linhas; por exemplo, ProgressBar.
- Os valores de raio padrão que usamos aqui são **2px**.

![Exemplo de barra de progresso](images/rounded-corner/bars.png)

**Controles**

- Indicador de seleção de NavigationView
- Indicador de seleção dinâmica
- ProgressBar
- ScrollBar (quando `IndicatorMode=TouchIndicator`)
- Controle deslizante
  - Controle deslizante de cor ColorPicker
  - Controle deslizante da barra de busca MediaTransportControls

## <a name="customization-options"></a>Opções de personalização

Os valores de raios de canto padrão que fornecemos não são imutáveis e há algumas maneiras de modificar facilmente a quantidade de arredondamento nos cantos. Isso pode ser feito por meio de dois recursos globais ou por meio da propriedade [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) diretamente no controle, dependendo do nível de granularidade de personalização desejado.

### <a name="when-not-to-round"></a>Quando não arredondar

Há casos em que o canto de um controle não deve ser arredondado, e não os arredondamos por padrão.

- Quando vários elementos da interface do usuário alojados dentro de um contêiner tocam uns nos outros, como as duas partes de um SplitButton. Não deve haver espaço quando eles entram em contato.

![SplitButton](images/rounded-corner/split-button-2.png)

- Quando um controle é armazenado dentro de outro contêiner, como a barra de um ScrollBar e botões que fazem parte do contêiner ScrollBar, que também faz parte de um ScrollViewer.

![ScrollBar](images/rounded-corner/scrollbar.png)

- Quando um elemento de interface do usuário do submenu é conectado a uma interface do usuário que invoca o submenu de um lado.

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>Retângulo de foco de teclado e sombra

Nosso design padrão não faz nenhum trabalho especial para arredondar os cantos do retângulo de foco do teclado ou da sombra de controle. Usar um valor de raio de canto superior não os quebrará de forma funcional; no entanto, é bom estar ciente disso para evitar falhas visuais indesejadas que poderiam ser introduzidas com um valor maior.

Aqui está um exemplo de como um raio de canto maior pode fazer com que a sombra pareça indesejável:

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>Cantos arredondados e desempenho

A renderização de cantos arredondados naturalmente usa mais potência de desenho do que renderizar cantos quadrados. Ao selecionar os valores de raio de canto padrão, nós não apenas consideramos os princípios de design, mas também temos cuidado para garantir que nossos controles padrão tenham um bom desempenho ao usá-los em seus aplicativos.

Ao pensar sobre o desempenho do aplicativo neste contexto, você deve considerar principalmente o tempo de carregamento da página e o tempo de inicialização do aplicativo. Considere que os cantos arredondados em uma superfície de interface do usuário maior têm um impacto maior no desempenho. Evite desenhar cantos arredondados em uma interface do usuário de aplicativo de tela inteira. Isso será um problema menor se a interface do usuário for exibida brevemente e depois que a página for carregada, como um ContentDialog.

### <a name="page-or-app-wide-cornerradius-changes"></a>Alterações de CornerRadius em todo o aplicativo ou página

Há dois recursos de aplicativo que controlam o raios de canto de todos os controles:

- `ControlCornerRadius` – o padrão é 2px.
- `OverlayCornerRadius` – o padrão é 4px.

Se você substituir o valor desses recursos em qualquer escopo, isso afetará todos os controles dentro desse escopo de forma adequada.

Isso significa que, se você quiser alterar o arredondamento de todos os controles em que o arredondamento pode ser aplicado, poderá definir os dois recursos no nível do aplicativo com os novos valores de CornerRadius como este:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">4</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
            </ResourceDictionary>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Como alternativa, se você quiser alterar o arredondamento de todos os controles dentro de um escopo específico, como em um nível de página ou contêiner, poderá seguir um padrão semelhante:

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> O recurso de `OverlayCornerRadius` deve ser definido no nível do aplicativo para entrar em vigor.
>
>Isso ocorre porque pop-ups e submenus são dinâmicos e criados no elemento raiz na árvore visual, assim, todos os recursos que eles usam também devem ser definidos lá. Caso contrário, eles estão fora do escopo.

### <a name="per-control-cornerradius-changes"></a>Alterações de CornerRadius por controle

Você pode modificar a propriedade [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) nos controles diretamente se quiser alterar o arredondamento de apenas um número selecionado de controles.

|Padrão | Propriedade modificada |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

Nem todos os cantos dos controles responderão à sua propriedade `CornerRadius` que está sendo modificada. Para garantir que o controle cujos cantos você deseja arredondar realmente responda à sua propriedade de `CornerRadius` da maneira esperada, primeiro verifique se os recursos `ControlCornerRadius` ou `OverlayCornerRadius` globais afetam o controle em questão. Se não afetarem, verifique se o controle que você deseja arredondar tem cantos. Muitos de nossos controles não renderizam bordas reais e, portanto, não podem usar adequadamente a propriedade `CornerRadius`.
