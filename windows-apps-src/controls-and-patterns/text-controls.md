---
author: Jwmsft
Description: Considere com que frequência lemos texto em nossas vidas diárias - em emails, livros, sinais de trânsito, preços em um cardápio, marcas de pressão dos pneus ou cartazes em postes.
title: Controles de texto
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Text controls
template: detail.hbs
---
# Controles de texto
Controles de texto consistem em caixas de entrada de texto, caixas de senha, caixas de sugestão automática e blocos de texto. A estrutura do XAML fornece vários controles para renderizar, inserir e editar texto, com um conjunto de propriedades para formatar o texto.

- Os controles para exibir texto somente leitura são [TextBlock](text-block.md) e [RichTextBlock](rich-text-block.md).
- Os controles de entrada e edição de texto são: [TextBox](text-block.md), [AutoSuggestBox](auto-suggest-box.md), [PasswordBox](password-box.md) e [RichEditBox](rich-edit-box.md). 


<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [**Classe AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)
-   [**Classe PasswordBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)
-   [**Classe RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)
-   [**Classe RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)
-   [**Classe TextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)
-   [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)

## Esse é o controle correto?

O controle de texto que você deverá usar dependerá do cenário. Use essas informações para selecionar o controle de texto correto para usar em seu aplicativo.

### Renderizar texto somente leitura

Use um **TextBlock** para exibir mais texto somente leitura em seu aplicativo. Você pode usá-lo para exibir texto de linha única ou de várias linhas, hiperlinks embutidos e texto com formatação, como negrito, itálico ou sublinhado.

TextBlock geralmente é mais fácil de usar e fornece melhor desempenho na renderização de texto do que RichTextBlock, por isso é o preferido para a maioria do texto da interface do usuário de aplicativo. Você pode facilmente acessar e usar o texto de um TextBlock em seu aplicativo, obtendo o valor da propriedade [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx). 

Também fornece muitas das mesmas opções de formatação para personalizar como o texto será renderizado. Embora você possa colocar quebras de linha no texto, TextBlock é projetado para exibir um único parágrafo e não oferece suporte a recuo de texto.

Use um **RichTextBlock** quando precisar de suporte para vários parágrafos, texto com várias colunas ou outros layouts de texto complexos ou elementos de interface do usuário embutidos como imagens. RichTextBlock fornece vários recursos para layout de texto avançado.

A propriedade de conteúdo de RichTextBlock é a propriedade [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), que oferece suporte a texto baseado em parágrafos por meio do elemento [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Ele não tem uma propriedade **Text** que você pode usar para acessar facilmente o conteúdo de texto do controle em seu aplicativo.  

### Entrada de texto

Use o controle **TextBox** para permitir que o usuário digite e edite texto não formatado, como em um formulário. Você pode usar a propriedade [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) para obter e definir o texto em um TextBox.

Você pode tornar um TextBox somente leitura, mas deve ser um estado temporário e condicional. Se o texto nunca for editável, considere usar um TextBlock.

Use o controle **PasswordBox** para receber uma senha ou outros dados particulares, como um número de CPF. Caixa de senha é uma caixa para entrada de texto que oculta os caracteres digitados nela, por uma questão de privacidade. Uma caixa de senha é semelhante a uma caixa para entrada de texto, exceto pelo fato de gerar marcadores no lugar do texto inserido. O caractere do marcador pode ser personalizado.

Use o controle **AutoSuggestBox** para mostrar ao usuário uma lista de sugestões das quais escolher ao digitar. Caixa de sugestão automática é uma caixa de entrada de texto que aciona uma lista de sugestões de pesquisa básica. Os termos sugeridos podem ser extraídos de uma combinação de termos de pesquisa populares e termos do histórico inseridos pelo usuário.

Você também deve usar um controle AutoSuggestBox para implementar uma caixa de pesquisa.

Use **RichEditBox** para exibir e editar arquivos Rich Text. Você não usa um RichEditBox para obter a entrada do usuário em seu aplicativo da maneira que você usa outras caixas de entrada de texto padrão. Em vez disso, você o usa para trabalhar com arquivos de texto que são separados de seu aplicativo. Em geral, você salva o texto inserido em um RichEditBox em um arquivo. rtf.

**A entrada de texto é a melhor opção?**

Há muitas maneiras de se obter a entrada do usuário em seu aplicativo. Estas perguntas ajudarão a responder se uma das caixas de entrada de texto padrão ou outro controle é a melhor opção para obter a entrada do usuário.

-   **É prático enumerar todos os valores válidos de forma eficiente?** Se sim, então considere usar um dos controles de seleção, como [caixa de seleção](checkbox.md), [lista suspensa](lists.md), caixa de listagem, [botão de opção](radio-button.md), [controle deslizante](slider.md), [botão de alternância](toggles.md), [seletor de data](date-and-time.md) ou seletor de hora.
-   **Há um conjunto razoavelmente pequeno de valores válidos?** Nesse caso, considere uma [lista suspensa](lists.md) ou uma caixa de listagem, especialmente se os valores tiverem mais do que alguns caracteres.
-   **Os dados válidos estão completamente irrestritos? Ou os dados válidos estão restritos apenas pelo formato (restrição de tamanho ou tipos de caractere)?** Em caso afirmativo, use um controle de entrada de texto. Você pode limitar o número de caracteres que podem ser inseridos e validar o formato no código do aplicativo.
-   **O valor representa um tipo de dados que tem um controle comum especializado?** Se sim, use o controle apropriado em vez de um controle de entrada de texto. Por exemplo, use um [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681), em vez de um controle de entrada de texto, para aceitar uma entrada de data.
-   Se os dados forem estritamente numéricos:
    -   **O valor que está sendo inserido é aproximado e/ou relativo a outra quantidade na mesma página?** Se for o caso, utilize um [controle deslizante](slider.md).
    -   **O usuário se beneficiaria com um feedback instantâneo sobre o efeito das alterações de configuração?** Se for caso, utilize um [controle deslizante](slider.md), possivelmente com um controle anexo.
    -   **Há alguma probabilidade de o valor inserido ser ajustado após o resultado ser observado, como volume ou brilho ser ajustado?** Se for o caso, utilize um [controle deslizante](slider.md).
    
## Exemplos

Caixa de texto

![Uma caixa de texto](images/text-box.png)

Caixa de sugestão automática

![Exemplo do controle de sugestão automática expandido](images/controls_autosuggest_expanded01.png)

Caixa de senha

![Foco da caixa de senha no estado de digitação texto](images/passwordbox-focus-typing.png)

## Criar um controle de texto

Consulte estes artigos para obter informações e exemplos específicos para cada controle de texto.

-   [**AutoSuggestBox**](auto-suggest-box.md)
-   [**PasswordBox**](password-box.md)
-   [**RichEditBox**](rich-edit-box.md)
-   [**RichTextBlock**](rich-text-block.md)
-   [**TextBlock**](text-block.md)
-   [**TextBox**](text-box.md)

## Diretrizes de fonte e estilo
Consulte estes artigos para obter diretrizes de fonte:

- [**Diretrizes de fonte**](fonts.md)
- [**Diretrizes e lista de ícones Segoe MDL2**](segoe-ui-symbol-font.md)


## Escolha o teclado correto para o controle de texto

**Aplica-se a:** TextBox, PasswordBox RichEditBox

Para ajudar os usuários a inserir dados usando o teclado virtual ou SIP (Soft Input Panel), você pode configurar o escopo de entrada do controle de texto para corresponder ao tipo de dado que se espera que o usuário insira.

>Dica Essas informações se aplicam somente ao SIP. Elas não se aplicam a teclados de hardware nem ao Teclado Virtual disponível nas opções de Facilidade de Acesso do Windows.

O teclado virtual pode ser usado para entrada de texto, quando o aplicativo é executado em um dispositivo com tela sensível ao toque. O teclado virtual é invocado quando o usuário toca em um campo de entrada editável, como um TextBox ou um RichEditBox. Você pode tornar a entrada de dados muito mais rápida e fácil para os usuários em seu aplicativo definindo o escopo de entrada do controle de texto para corresponder ao tipo de dados que o usuário deve inserir. O escopo de entrada oferece uma dica para o sistema sobre o tipo de entrada de texto esperado pelo controle, para que o sistema possa fornecer um layout de teclado virtual especializado para o tipo de entrada.

Por exemplo, se uma caixa de texto for usada somente para a inserção de um PIN de 4 dígitos, defina a propriedade [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) como **Number**. Isso informa o sistema para mostrar o layout do teclado numérico, facilitando a inserção do PIN.

>Importante  
>O escopo de entrada não faz com que qualquer validação de entrada seja executada, e não impede que o usuário forneça qualquer entrada por meio de um teclado de hardware ou de outro dispositivo de entrada. Você ainda é o responsável pela validação da entrada em seu código, conforme necessário.

Para obter mais informações, consulte [Usar o escopo de entrada para alterar o teclado virtual]().

## Fontes de cores

**Aplica-se a:** TextBlock, RichTextBlock, TextBox, RichEditBox

No Windows, as fontes podem incluir várias camadas coloridas para cada glifo. Por exemplo, a fonte Segoe UI Emoji define versões de cor do Emoticon e outros personagens Emoji. 

Os controles padrão e Rich Text dão suporte a fontes de cor de exibição. Por padrão, a propriedade **IsColorFontEnabled** é **true**, e fontes com essas camadas adicionais são renderizadas na cor. A fonte de cor padrão no sistema é Segoe UI Emoji e os controles voltarão para essa fonte para exibir os glifos em cores. 

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

O texto renderizado tem a aparência a seguir:

![Bloco de texto com fonte de cor](images/text-block-color-fonts.png)

Para saber mais, consulte a propriedade [**IsColorFontEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx).

## Diretrizes para separadores de linha e parágrafo

**Aplica-se a:** TextBlock, RichTextBlock, TextBox de várias linhas, RichEditBox

Use o caractere separador de linha (0x2028) e o caractere separador de parágrafo (0x2029) para dividir texto sem formatação. Uma nova linha é iniciada após cada separador de linha. Um novo parágrafo é iniciado após cada separador de parágrafo.

Não é necessário iniciar a primeira linha ou parágrafo em um arquivo com esses caracteres nem finalizar a última linha ou parágrafo com eles; fazer isso indica que há uma linha ou parágrafo vazio nesse local.

Seu aplicativo pode usar o separador de linha para indicar um final da linha não condicional. No entanto, separadores de linha não correspondem ao retorno de carro separado e caracteres de avanço de linha nem a uma combinação desses caracteres. Os separadores de linha devem ser processados separadamente de caracteres de avanço de linha e de retorno do carro.

Seu aplicativo pode inserir um separador de parágrafo entre parágrafos de texto. O uso desse separador permite a criação de arquivos de texto sem formatação que podem ser formatados com larguras de linha diferentes em sistemas operacionais diversos. O sistema de destino pode ignorar todos os separadores de linha e interromper parágrafos apenas nos separadores de parágrafo.



## Artigos relacionados

**Para designers**
- [**Diretrizes de fonte**](fonts.md)
- [**Diretrizes e lista de ícones Segoe MDL2**](segoe-ui-symbol-font.md)
- [Diretrizes para verificação ortográfica](spell-checking-and-prediction.md)
- [Adicionando pesquisa](https://msdn.microsoft.com/library/windows/apps/hh465231)

**Para desenvolvedores (XAML)**
- [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Classe Windows.UI.Xaml.Controls PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriedade String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)


<!--HONumber=May16_HO2-->


