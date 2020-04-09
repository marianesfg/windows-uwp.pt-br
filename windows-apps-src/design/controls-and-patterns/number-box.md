---
Description: Numberbox é um controle que pode ser usado para exibir e editar números.
title: Caixa de número
template: detail.hbs
ms.date: 11/27/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5307448b6209228cd8c4550a739c6db15091ba54
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081666"
---
# <a name="number-box"></a>Caixa de número

Representa um controle que pode ser usado para exibir e editar números. Isso dá suporte para validação, passo a passo incremental e computação de cálculos embutidos de equações básicas, como multiplicação, divisão, adição e subtração.

![Exemplo de um menu de contexto típico](images/contextmenu_rs2_icons.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | O controle **NumberBox** está incluído como parte da Biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para saber obter mais informações, incluindo instruções de instalação, confira a [visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

**APIs da Biblioteca de interface do usuário do Windows:** [Classe NumberBox](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.NumberBox)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Você pode usar um controle NumberBox para capturar e exibir a entrada matemática. Se você precisar de uma caixa de texto editável que aceite mais que números, use o controle [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Se você precisar de uma caixa de texto editável que aceite senhas ou outra entrada confidencial, confira [PasswordBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox). Se você precisar de uma caixa de texto para inserir termos de pesquisa, confira [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox). Se você precisar inserir ou editar texto formatado, confira [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TextBox">abri-lo e ver a NumberBox em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>Criar uma NumberBox simples

Aqui está o XAML para uma NumberBox básica que demonstra a aparência padrão. Use [x:Bind](/windows/uwp/xaml-platform/x-bind-markup-extension#property-path) para garantir que os dados exibidos para o usuário permaneçam em sincronia com os dados armazenados em seu aplicativo. 


```xaml
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```
![Um campo de entrada em foco mostrando 0.](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>Como rotular a NumberBox

Use `Header` ou `PlaceholderText` se a finalidade da NumberBox não estiver clara. `Header` ficará visível, independentemente de a NumberBox ter ou não um valor. 

```xaml
<NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Uma leitura de cabeçalho "Inserir expressão:" acima de uma NumberBox.](images/numberbox-header.png)

`PlaceholderText` é exibido dentro da NumberBox e só aparecerá quando `Value` for definido como NaN ou quando a entrada for desmarcada pelo usuário.

```xaml
<NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Uma NumberBox que contém texto de espaço reservado que indica "A + B".](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>Habilitar o suporte de cálculo

Definir a propriedade `AcceptsExpression` como true permite que a NumberBox avalie expressões básicas embutidas como multiplicação, divisão, adição e subtração usando a ordem padrão de operações. A avaliação é disparada quando ocorre perda de foco ou quando o usuário pressiona a tecla "Enter". Depois que uma expressão é avaliada, a forma original da expressão não é preservada.

XAML
```xaml
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>Passo a passo de incremento e decremento

Use a propriedade `SmallChange` para configurar o quanto o valor dentro de uma NumberBox é alterado quando o NumberBox está em foco e o usuário:

- rola
- pressiona a tecla de seta para cima
- pressiona a tecla de seta para baixo

Use a propriedade `LargeChange` para configurar o quanto o valor dentro de uma NumberBox é alterado quando a NumberBox está em foco e o usuário pressiona a tecla PageUp ou PageDown. 

Use a propriedade `SpinButtonPlacementMode` para habilitar botões que podem ser clicados para incrementar ou decrementar o valor na NumberBox pelo valor especificado pela propriedade `SmallChange`. Esses botões serão desabilitados se um valor máximo ou mínimo for superado com outra etapa. 

Defina `SpinButtonPlacementMode` como `Inline` para permitir que os botões apareçam ao lado do controle. 

XAML
```XAML
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![Uma NumberBox com um botão de seta para baixo e botão de seta para cima ao lado.](images/numberbox-spinbutton-inline.png)

Defina `SpinButtonPlacementMode` como `Compact` para permitir que os botões apareçam como um submenu somente quando a NumberBox estiver em foco.  

XAML
```XAML
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![Uma NumberBox com um pequeno ícone dentro mostra uma seta apontando para cima e uma seta apontando para baixo.](images/numberbox-spinbutton-compact-non-visible.png)

![Uma NumberBox com um botão de seta para baixo e botão de seta para cima flutuando para fora para o lado em uma camada com privilégios elevados.](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>Como habilitar a validação de entrada

A definição de `ValidationMode` como `InvalidInputOverwritten` permitirá que a NumberBox substitua a entrada inválida que não seja numérica nem legalmente formulada com o último valor válido quando a avaliação for disparada mediante perda de foco ou pressionamento da tecla "Enter".

XAML
```XAML
<NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

Configurar `ValidationMode` como `Disabled` permite que a validação de entrada personalizada seja configurada.  

Em relação a pontos decimais e vírgulas, a formatação usada por um usuário será substituída pela formatação configurada para a NumberBox. Um erro de validação de entrada não será disparado. 

### <a name="formatting-input"></a>Como formatar a entrada 

A [formatação de números](/uwp/api/windows.globalization.numberformatting) pode ser usada para formatar o valor de uma Numberbox configurando uma instância de uma classe de formatação e atribuindo-a à propriedade `NumberFormatter`. Decimais, moeda, percentual e números significativos são algumas das classes de formatação de número disponíveis. Observe que o arredondamento também é definido pelas propriedades de formatação de número.

Aqui está um exemplo de como usar DecimalFormatter para formatar um valor de NumberBox para ter um dígito inteiro, dois dígitos de fração e arredondar para o 0,25 mais próximo:  

XAML
```XAML
<NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

C#
```C#
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![NumberBox mostrando um valor de 0,00.](images/numberbox-formatted.png)

Em relação a pontos decimais e vírgulas, a formatação usada por um usuário será substituída pela formatação configurada para a NumberBox. Um erro de validação de entrada não será disparado. 

## <a name="remarks"></a>Comentários

### <a name="input-scope"></a>Escopo de entrada

`Number` será usado para o [escopo de entrada](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue). Esse escopo de entrada destina-se a trabalhar com dígitos de 0-9. Isso pode ser substituído, mas tipos de InputScope alternativos não terão suporte explícito. 

### <a name="not-a-number"></a>Não é um número

Quando a entrada de uma NumberBox é removida, `Value` é definido como `NaN` para indicar que nenhum valor numérico está presente. 

### <a name="expression-evaluation"></a>Avaliação de expressão 

A NumberBox usa a notação de infixo para avaliar expressões. Em ordem de precedência, os operadores permitidos são:

* ^
* */
* +-

Observe que os parênteses podem ser usados para substituir as regras de precedência. 

## <a name="recommendations"></a>Recomendações

* `Text` e `Value` facilitam a captura do valor de uma NumberBox como uma cadeia de caracteres ou um Double sem necessidade de converter o valor entre tipos. Ao alterar programaticamente o valor de uma NumberBox, é recomendável fazer isso por meio da propriedade `Value`. `Value` substituirá `Text` na configuração inicial. Após a configuração inicial, as alterações a uma serão propagadas para a outra, mas fazer alterações programáticas por meio de `Value` ajuda a evitar qualquer mal-entendido conceitual de que NumberBox aceitará caracteres não numéricos por meio de `Text`.  
* Use `Header` ou `PlaceholderText` para informar aos usuários de que a NumberBox aceita apenas caracteres numéricos como entrada. A representação por extenso de números, como "um", não será resolvida para um valor aceito. 
