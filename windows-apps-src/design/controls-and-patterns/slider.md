---
Description: Lets the user set a value in a given range.
title: Controles deslizantes
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e13f28d9b82bca04d108bac818faa873567d77f7
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194660"
---
# <a name="sliders"></a>Controles deslizantes

 

Controle deslizante é um controle que permite que o usuário selecione em uma lista de valores movendo um controle de posição paralelo a uma faixa.

> **APIs importantes**: [classe Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), [propriedade Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx), [evento ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)

![Um controle deslizante](images/controls/slider.png)


## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use um controle deslizante quando quiser que os usuários tenham condições de estabelecer valores contíguos definidos (como volume ou brilho) ou um intervalo de valores distintos (como configurações de resolução de tela).

Um controle deslizante é uma boa opção quando você sabe que os usuários imaginam o valor como uma quantidade relativa, e não como um valor numérico. Por exemplo, os usuários pensam em definir o volume de áudio como baixo ou médio, e não em definir o valor como 2 ou 5.

Não use um controle deslizante para configurações binárias. Use um [botão de alternância](toggles.md) em seu lugar.

Veja a seguir alguns fatores adicionais que você deve considerar ao decidir se irá usar um controle deslizante:

-   **A configuração parece uma quantidade relativa?** Se não parecer, use os [botões de opção](radio-button.md) ou uma [caixa de listagem](lists.md).
-   **A configuração é um valor numérico exato conhecido?** Se sim, use uma [caixa de texto](text-box.md).
-   **O usuário se beneficiaria com um feedback instantâneo sobre o efeito das alterações de configuração?** Se sim, use um controle deslizante. Por exemplo, os usuários podem escolher uma cor com mais facilidade vendo imediatamente o efeito das alterações nos valores de matiz, saturação ou luminosidade.
-   **A configuração tem um intervalo de quatro ou mais valores?** Se não parecer, use os [botões de opção](radio-button.md).
-   **O usuário pode alterar o valor?** Controles deslizantes são destinados para interação do usuário. Se um usuário não puder alterar o valor, use texto somente leitura.

Se você estiver decidindo entre um controle deslizante e uma caixa de texto numérico, use uma caixa de texto numérico se:

-   O espaço na tela for restrito.
-   For provável que o usuário irá preferir usar o teclado.

Use um controle deslizante se:

-   Os usuários forem se beneficiar com um feedback instantâneo.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Slider">abrir o aplicativo e ver o Slider em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Um controle deslizante para controlar o volume no Windows Phone.

![Um controle deslizante para controlar o volume no Windows Phone](images/control-examples/slider-phone.png)

Um controle deslizante para alterar o tamanho do texto nas configurações de exibição do Windows.

![Um controle deslizante para alterar o tamanho do texto nas configurações de exibição do Windows](images/control-examples/slider-display-settings.png)

## <a name="create-a-slider"></a>Criar um controle deslizante

Aqui está como usar um controle deslizante em XAML.

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

Consulte como criar um controle deslizante em código.

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

Você obtém e define o valor do controle deslizante pela propriedade propriedade [Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx). Para responder às alterações de valor, você pode usar a vinculação de dados para associar à propriedade Value ou manipular o evento [ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx).

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## <a name="recommendations"></a>Recomendações

-   Dimensione o controle de modo que os usuários possam definir com facilidade o valor que desejam. Para configurações com valores distintos, certifique-se de que o usuário pode facilmente selecionar qualquer valor utilizando o mouse. Confirme que os pontos de extremidade do controle deslizante sempre estejam dentro dos limites de uma visualização.
-   Forneça feedback imediato enquanto ou após um usuário faz uma seleção (quando for prático). Por exemplo, o controle de volume do Windows emite um som para indicar o volume de áudio selecionado.
-   Use rótulos para mostrar o intervalo de valores. Exceção: se o controle deslizante estiver orientado verticalmente, e o rótulo superior for Máximo, Alto, Mais ou equivalente, você poderá omitir os outros rótulos pois o significado está claro.
-   Desative todos os rótulos ou elementos visuais de feedback associados ao desabilitar o controle deslizante.
-   Considere a direção do texto ao configurar a direção de fluxo e/ou orientação de seu controle deslizante. O script flui da esquerda para direita em alguns idiomas e, da direita para esquerda em outros.
-   Não use um controle deslizante como indicador de progresso.
-   Não altere o tamanho padrão da miniatura de controle deslizante.
-   Não crie um controle deslizante contínuo se o intervalo de valores for grande e os usuários provavelmente selecionarão um entre diversos valores representativos de dentro do intervalo. Em vez disso, utilize os valores como as únicas etapas permitidas. Por exemplo, se o valor para tempo pode ficar acima de um mês mas os usuários só precisam escolher entre 1 minuto, 1 hora, 1 dia ou 1 mês, crie um controle deslizante com quatro pontos de etapa.

## <a name="additional-usage-guidance"></a>Diretriz de uso adicional

### <a name="choosing-the-right-layout-horizontal-or-vertical"></a>Escolhendo o layout correto: horizontal ou vertical

Você pode definir o layout do seu controle deslizante horizontal ou verticalmente. Use estas diretrizes para determinar qual layout usar.

-   Use uma orientação natural. Por exemplo, se o controle deslizante representar um valor real que normalmente é mostrado na vertical (como temperatura), use uma orientação vertical.
-   Se o controle for usado para buscas dentro de uma mídia, como em um aplicativo de vídeo, use uma orientação horizontal.
-   Ao usar um controle deslizante em uma página que pode fazer um movimento panorâmico em uma direção (horizontal ou vertical), use uma orientação diferente para o controle deslizante do que a direção panorâmica. Caso contrário, os usuários poderão passar o dedo sobre o controle deslizante e alterar seu valor acidentalmente ao tentarem um movimento panorâmico na página.
-   Se ainda não tiver certeza sobre qual orientação usar, utilize a que melhor se ajusta ao layout da sua página.

### <a name="range-direction"></a>Direção do intervalo

A direção do intervalo é a direção em que você move o controle deslizante ao escorregar do seu valor atual para o seu valor máximo.

-   Para um controle deslizante vertical, coloque o maior valor no topo do controle deslizante, independentemente da direção de leitura. Por exemplo, para um controle deslizante de volume, sempre coloque a configuração de volume máximo no topo do controle deslizante. Para outros tipos de valores (como dias da semana), siga a direção de leitura da página.
-   Para estilos horizontais, coloque o valor inferior no lado esquerdo do controle deslizante para um layout de página da esquerda para a direita e no lado direito para um layout de página da direita para a esquerda.
-   A única exceção para a diretriz anterior é no caso de barras de busca de mídia: sempre coloque o valor inferior no lado esquerdo do controle deslizante.

### <a name="steps-and-tick-marks"></a>Etapas e marcas de escala

-   Use pontos de etapa se não quiser que o controle deslizante permita valores arbitrários entre mín e máx. Por exemplo, se você usar um controle deslizante para especificar o número de entradas de cinema a comprar, não permita valores de ponto flutuante. Forneça a ele um valor de etapa igual a 1.
-   Se você especificar etapas (também conhecidas como pontos de alinhamento), verifique se a etapa final se alinha ao valor máximo do controle deslizante.
-   Use marcas de escala quando quiser mostrar aos usuários o local de valores importantes ou significantes. Por exemplo, um controle deslizante que controla o zoom pode ter marcas de escala para 50%, 100% e 200%.
-   Mostre marcas de escala quando os usuários precisarem saber o valor aproximado da configuração.
-   Mostre marcas de escala e um rótulo de valor quando os usuários precisarem saber o valor exato da configuração escolhida, sem interagir com o controle. Caso contrário, eles poderão usar a dica de ferramenta de valor para ver o valor exato.
-   Sempre mostre marcas de escala quando pontos de etapa não forem óbvios. Por exemplo, se o controle deslizante tiver 200 pixels de largura e 200 pontos de ajuste, você poderá ocultar as marcas de escala, pois os usuários não observarão o comportamento de ajuste. Mas, se houver apenas 10 pontos de ajuste, mostre as marcas de escala.

### <a name="labels"></a>Rótulos

-   **Rótulos de controle deslizante**

    O rótulo de controle deslizante indica a função do controle deslizante.

    -   Use um rótulo sem pontuação final (essa é a convenção para todos os rótulos de controle).
    -   Posicione rótulos acima do controle deslizante quando ele estiver em um formato que coloque a maioria dos seus rótulos acima dos seus controles.
    -   Posicione rótulos nas laterais quando o controle deslizante estiver em um formato que coloque a maioria dos seus rótulos na lateral dos seus controles.
    -   Evite colocar rótulos abaixo do controle deslizante porque o dedo do usuário pode ocultar o rótulo ao tocar no controle deslizante.
-   **Rótulos de intervalo**

    Rótulos de intervalo, ou de preenchimento, descrevem os valores mínimo e máximo do controle deslizante.

    -   Rotule as duas extremidades do intervalo do controle deslizante, a menos que uma orientação vertical torne isso desnecessário.
    -   Use apenas uma palavra, se possível, para cada rótulo.
    -   Não use pontuação final.
    -   Certifique-se de que esses rótulos sejam descritivos e paralelos. Exemplos: Máximo/Mínimo, Mais/Menos, Baixo/Alto (altura), Baixo/Alto (som).
-   **Rótulos de valor**

    Um rótulo de valor exibe o valor atual do controle deslizante.

    -   Se você precisar de um rótulo de valor, mostre-o abaixo do controle deslizante.
    -   Centralize o texto em relação ao controle e inclua as unidades (como pixels).
    -   Como o elevador do controle deslizante está coberto durante o deslizamento, considere a possibilidade de mostrar o valor atual de outro modo, com um rótulo ou outro elemento visual. O tamanho de texto da configuração de um controle deslizante pode produzir algum texto de exemplo à direita do controle deslizante, a seu lado.

### <a name="appearance-and-interaction"></a>Aparência e interação

Um controle deslizante é composto de uma faixa e um elevador. A faixa é uma barra (que pode opcionalmente exibir diversos estilos de marcas de escala) representando o intervalo de valores que podem ser inseridos. O elevador é um seletor, que o usuário pode posicionar tocando na faixa ou deslizando o seletor para frente ou para trás.

Um controle deslizante tem uma área grande de alvo de toque. Para manter a acessibilidade ao toque, um controle deslizante deve ser posicionado suficientemente longe da borda da tela.

Quando você está projetando um controle deslizante personalizado, considere meios de apresentar todas as informações necessárias ao usuário com o mínimo de desorganização possível. Utilize um rótulo de valor se um usuário precisa saber as unidades para entender a configuração; encontre meios criativos de representar esses valores graficamente. Um controle deslizante que controla o volume, por exemplo, pode exibir o gráfico de uma caixa acústica sem ondas sonoras na extremidade mínima do controle deslizante e uma caixa acústica com ondas sonoras na extremidade máxima desse controle.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados
- [Switches de alternância](toggles.md)
- [Classe Slider](https://msdn.microsoft.com/library/windows/apps/br209614)
