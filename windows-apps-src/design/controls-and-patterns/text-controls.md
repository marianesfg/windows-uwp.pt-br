---
author: Jwmsft
Description: Consider how often we read text in our daily lives - in email, a book, a road sign, the prices on a menu, tire pressure markings, or posters on a street pole.
title: Controles de texto
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Text controls
template: detail.hbs
ms.author: jimwalk
ms.date: 10/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2cd5f7e7022f246fce3d08286fe77c74503ddc5d
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5471369"
---
# <a name="text-controls"></a>Controles de texto

Controles de texto consistem em caixas de entrada de texto, caixas de senha, caixas de sugestão automática e blocos de texto. A estrutura do XAML fornece vários controles para renderizar, inserir e editar texto, com um conjunto de propriedades para formatar o texto.

- Os controles para exibir texto somente leitura são [TextBlock](text-block.md) e [RichTextBlock](rich-text-block.md).
- Os controles de entrada e edição de texto são: [TextBox](text-box.md), [RichEditBox](rich-edit-box.md), [AutoSuggestBox](auto-suggest-box.md)e [PasswordBox](password-box.md).

> **APIs importantes**: [classe TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [classe RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [classe TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx), [classe RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx), [classe AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx), [classe PasswordBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

O controle de texto que você deverá usar dependerá do cenário. Use essas informações para selecionar o controle de texto correto para usar em seu aplicativo.

### <a name="render-read-only-text"></a>Renderizar texto somente leitura

Use um **TextBlock** para exibir mais texto somente leitura em seu aplicativo. Você pode usá-lo para exibir texto de linha única ou de várias linhas, hiperlinks embutidos e texto com formatação, como negrito, itálico ou sublinhado.

TextBlock geralmente é mais fácil de usar e fornece melhor desempenho na renderização de texto do que RichTextBlock, por isso é o preferido para a maioria do texto da interface do usuário de aplicativo. Você pode facilmente acessar e usar o texto de um TextBlock em seu aplicativo, obtendo o valor da propriedade [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx).

Também fornece muitas das mesmas opções de formatação para personalizar como o texto será renderizado. Embora você possa colocar quebras de linha no texto, TextBlock é projetado para exibir um único parágrafo e não oferece suporte a recuo de texto.

Use um **RichTextBlock** quando precisar de suporte para vários parágrafos, texto com várias colunas ou outros layouts de texto complexos ou elementos de interface do usuário embutidos como imagens. RichTextBlock fornece vários recursos para layout de texto avançado.

A propriedade de conteúdo de RichTextBlock é a propriedade [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), que oferece suporte a texto baseado em parágrafos por meio do elemento [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Ele não tem uma propriedade **Text** que você pode usar para acessar facilmente o conteúdo de texto do controle em seu aplicativo.  

### <a name="text-input"></a>Entrada de texto

Use o controle **TextBox** para permitir que o usuário digite e edite texto não formatado, como em um formulário. Você pode usar a propriedade [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) para obter e definir o texto em um TextBox.

Você pode tornar um TextBox somente leitura, mas deve ser um estado temporário e condicional. Se o texto nunca for editável, considere usar um TextBlock.

Use o controle **PasswordBox** para receber uma senha ou outros dados particulares, como um número de CPF. Caixa de senha é uma caixa para entrada de texto que oculta os caracteres digitados nela, por uma questão de privacidade. Uma caixa de senha é semelhante a uma caixa para entrada de texto, exceto pelo fato de gerar marcadores no lugar do texto inserido. O caractere do marcador pode ser personalizado.

Use o controle **AutoSuggestBox** para mostrar ao usuário uma lista de sugestões das quais escolher ao digitar. Caixa de sugestão automática é uma caixa de entrada de texto que aciona uma lista de sugestões de pesquisa básica. Os termos sugeridos podem ser extraídos de uma combinação de termos de pesquisa populares e termos do histórico inseridos pelo usuário.

Você também deve usar um controle AutoSuggestBox para implementar uma caixa de pesquisa.

Use **RichEditBox** para exibir e editar arquivos Rich Text. Você não usa um RichEditBox para obter a entrada do usuário no aplicativo da maneira que você usa outras caixas de entrada de texto padrão. Em vez disso, você o usa para trabalhar com arquivos de texto que são separados de seu aplicativo. Em geral, você salva o texto inserido em um RichEditBox em um arquivo. rtf.

**A entrada de texto é a melhor opção?**

Há muitas maneiras de se obter a entrada do usuário em seu aplicativo. Estas perguntas ajudarão a responder se uma das caixas de entrada de texto padrão ou outro controle é a melhor opção para obter a entrada do usuário.

-   **É prático enumerar todos os valores válidos de forma eficiente?** Se sim, então considere usar um dos controles de seleção, como [caixa de seleção](checkbox.md), [lista suspensa](lists.md), caixa de listagem, [botão de opção](radio-button.md), [controle deslizante](slider.md), [switch de alternância](toggles.md), [seletor de data](date-and-time.md) ou seletor de hora.
-   **Há um conjunto razoavelmente pequeno de valores válidos?** Nesse caso, considere uma [lista suspensa](lists.md) ou uma caixa de listagem, especialmente se os valores tiverem mais do que alguns caracteres.
-   **Os dados válidos estão completamente irrestritos? Ou os dados válidos estão restritos apenas pelo formato (restrição de tamanho ou tipos de caractere)?** Em caso afirmativo, use um controle de entrada de texto. Você pode limitar o número de caracteres que podem ser inseridos e validar o formato no código do aplicativo.
-   **O valor representa um tipo de dados que tem um controle comum especializado?** Se sim, use o controle apropriado em vez de um controle de entrada de texto. Por exemplo, use um [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681), em vez de um controle de entrada de texto, para aceitar uma entrada de data.
-   Se os dados forem estritamente numéricos:
    -   **O valor que está sendo inserido é aproximado e/ou relativo a outra quantidade na mesma página?** Se for o caso, utilize um [controle deslizante](slider.md).
    -   **O usuário se beneficiaria com um feedback instantâneo sobre o efeito das alterações de configuração?** Se for caso, utilize um [controle deslizante](slider.md), possivelmente com um controle anexo.
    -   **Há alguma probabilidade de o valor inserido ser ajustado após o resultado ser observado, como volume ou brilho ser ajustado?** Se for o caso, utilize um [controle deslizante](slider.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/category/Text">abrir o aplicativo e ver os controles de texto em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Caixa de texto

![Uma caixa de texto](images/text-box.png)

Caixa de sugestão automática

![Exemplo do controle de sugestão automática expandido](images/controls_autosuggest_expanded01.png)

Caixa de senha

![Foco da caixa de senha no estado de digitação texto](images/passwordbox-focus-typing.png)

## <a name="create-a-text-control"></a>Criar um controle de texto

Consulte estes artigos para obter informações e exemplos específicos para cada controle de texto.

-   [AutoSuggestBox](auto-suggest-box.md)
-   [PasswordBox](password-box.md)
-   [RichEditBox](rich-edit-box.md)
-   [RichTextBlock](rich-text-block.md)
-   [TextBlock](text-block.md)
-   [TextBox](text-box.md)

## <a name="font-and-style-guidelines"></a>Diretrizes de fonte e estilo
Consulte estes artigos para obter diretrizes de fonte:

- [Diretrizes de tipografia](../style/typography.md)
- [Diretrizes e lista de ícones Segoe MDL2](../style/segoe-ui-symbol-font.md)

## <a name="pen-input"></a>Entrada à caneta

**Aplica-se a:** TextBox, RichEditBox, AutoSuggestBox

A partir do Windows 10, versão 1803, caixas de entrada de texto XAML têm suporte incorporado para caneta de entrada usando [Windows Ink](../input/pen-and-stylus-interactions.md). Quando um usuário toca em um texto de entrada de caixa usando uma caneta do Windows, as transformações de caixa de texto para permitir que o usuário escrever diretamente nele com uma caneta, em vez de abrir um painel de entrada separado.

![Caixa de texto se expande quando tocado com caneta](images/handwritingview/handwritingview2.gif)

Para obter mais informações, consulte [a entrada de texto com o modo de exibição de manuscrito](text-handwriting-view.md).

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Escolher o teclado correto para seu controle de texto

**Aplica-se a:** TextBox, PasswordBox RichEditBox

Para ajudar os usuários a inserir dados usando o teclado virtual ou SIP (Soft Input Panel), você pode configurar o escopo de entrada do controle de texto para corresponder ao tipo de dado que se espera que o usuário insira.

>Dica Essas informações se aplicam somente ao SIP. Elas não se aplicam a teclados de hardware nem ao Teclado Virtual disponível nas opções de Facilidade de Acesso do Windows.

O teclado virtual pode ser usado para entrada de texto, quando o aplicativo é executado em um dispositivo com tela sensível ao toque. O teclado virtual é invocado quando o usuário toca em um campo de entrada editável, como um TextBox ou um RichEditBox. Você pode tornar a entrada de dados muito mais rápida e fácil para os usuários em seu aplicativo definindo o escopo de entrada do controle de texto para corresponder ao tipo de dados que o usuário deve inserir. O escopo de entrada oferece uma dica para o sistema sobre o tipo de entrada de texto esperado pelo controle, para que o sistema possa fornecer um layout de teclado virtual especializado para o tipo de entrada.

Por exemplo, se uma caixa de texto for usada somente para a inserção de um PIN de 4 dígitos, defina a propriedade [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) como **Number**. Isso informa o sistema para mostrar o layout do teclado numérico, facilitando a inserção do PIN.

>Importante  
>O escopo de entrada não faz com que qualquer validação de entrada seja executada, e não impede que o usuário forneça qualquer entrada por meio de um teclado de hardware ou de outro dispositivo de entrada. Você ainda é o responsável pela validação da entrada em seu código, conforme necessário.

Para obter mais informações, consulte [Usar o escopo de entrada para alterar o teclado virtual](https://msdn.microsoft.com/library/windows/apps/mt280229).

## <a name="color-fonts"></a>Fontes de cores

**Aplica-se a:** TextBlock, RichTextBlock, TextBox, RichEditBox

No Windows, as fontes podem incluir várias camadas coloridas para cada glifo. Por exemplo, a fonte Segoe UI Emoji define versões de cor do Emoticon e outros personagens Emoji.

Os controles padrão e Rich Text dão suporte a fontes de cor de exibição. Por padrão, a propriedade **IsColorFontEnabled** é **true**, e fontes com essas camadas adicionais são renderizadas na cor. A fonte de cor padrão no sistema é Segoe UI Emoji e os controles voltarão para essa fonte para exibir os glifos em cores.

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

O texto renderizado tem a aparência a seguir:

![Bloco de texto com fonte de cor](images/text-block-color-fonts.png)

Para saber mais, consulte a propriedade [IsColorFontEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx).

## <a name="guidelines-for-line-and-paragraph-separators"></a>Diretrizes para separadores de linha e parágrafo

**Aplica-se a:** TextBlock, RichTextBlock, TextBox de várias linhas, RichEditBox

Use o caractere separador de linha (0x2028) e o caractere separador de parágrafo (0x2029) para dividir texto sem formatação. Uma nova linha é iniciada após cada separador de linha. Um novo parágrafo é iniciado após cada separador de parágrafo.

Não é necessário iniciar a primeira linha ou parágrafo em um arquivo com esses caracteres nem finalizar a última linha ou parágrafo com eles; fazer isso indica que há uma linha ou parágrafo vazio nesse local.

Seu aplicativo pode usar o separador de linha para indicar um final da linha não condicional. No entanto, separadores de linha não correspondem ao retorno de carro separado e caracteres de avanço de linha nem a uma combinação desses caracteres. Os separadores de linha devem ser processados separadamente de caracteres de avanço de linha e de retorno do carro.

Seu aplicativo pode inserir um separador de parágrafo entre parágrafos de texto. O uso desse separador permite a criação de arquivos de texto sem formatação que podem ser formatados com larguras de linha diferentes em sistemas operacionais diversos. O sistema de destino pode ignorar todos os separadores de linha e interromper parágrafos apenas nos separadores de parágrafo.

## <a name="guidelines-for-spell-checking"></a>Diretrizes para verificação ortográfica

**Aplica-se a:** TextBox, RichEditBox

Durante a edição e a entrada de texto, a verificação ortográfica informa o usuário que uma palavra está com grafia incorreta realçando-a com uma linha ondulada vermelha e fornece uma maneira de o usuário corrigir o erro de ortografia.

Veja aqui um exemplo do verificador ortográfico interno:

![o verificador ortográfico interno](images/spellchecking.png)

Use a verificação ortográfica com controles de entrada de texto para estas duas finalidades:

-   **Para corrigir erros ortográficos automaticamente**

    O mecanismo de verificação ortográfico corrige automaticamente palavras incorretas quando tem certeza sobre a correção. Por exemplo, o mecanismo altera automaticamente "teh" para "the".

-   **Para mostrar grafias alternativas**

    Quando o mecanismo de verificação ortográfica não tem certeza sobre as correções, ele adiciona uma linha vermelha abaixo da palavra incorreta e exibe as alternativas em um menu de contexto quando você toca ou clica com o botão direito do mouse na palavra.

-   Use a verificação ortográfica para ajudar os usuários a inserir palavras ou frases em controles de entrada de texto. A verificação ortográfica funciona com entradas por touch, mouse e teclado.
-   Não use a verificação ortográfica quando uma palavra provavelmente não estará no dicionário ou se os usuários não a valorizarão. Por exemplo, não a ative se a caixa de texto se destinar a capturar um número de telefone ou nome.
-   Não desabilite a verificação ortográfica só porque o atual mecanismo de correção ortográfica não oferece suporte ao idioma do seu aplicativo. Quando o verificador ortográfico não oferece suporte a um idioma, ele não faz nada, então não há nenhum mal em deixar a opção habilitada. Além disso, alguns usuários podem usar um IME para inserir outro idioma em seu aplicativo, e pode haver suporte para esse idioma. Por exemplo, ao criar um aplicativo em japonês, mesmo que o mecanismo de verificação ortográfica não possa reconhecer no momento esse idioma, não desative a verificação ortográfica. O usuário pode alternar para um IME em inglês e digitar inglês no aplicativo; se a verificação ortográfica estiver habilitada, a ortografia em inglês será verificada.

Para controles TextBox e RichEditBox, a verificação ortográfica permanece ativada por padrão. Você pode desabilitá-la definindo a propriedade **IsSpellCheckEnabled** como **false**.

## <a name="related-articles"></a>Artigos relacionados

**Para designers**
- [Diretrizes de tipografia](../style/typography.md)
- [Diretrizes e lista de ícones Segoe MDL2](../style/segoe-ui-symbol-font.md)
- [Adicionando pesquisa](https://msdn.microsoft.com/library/windows/apps/hh465231)

**Para desenvolvedores (XAML)**
- [Classe TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe Windows.UI.Xaml.Controls PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriedade String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)
