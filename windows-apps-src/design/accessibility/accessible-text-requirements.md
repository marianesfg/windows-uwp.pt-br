---
Description: Este tópico descreve as práticas recomendadas para acessibilidade de texto em um aplicativo, garantindo que cores e telas de fundo satisfaçam o índice de contraste necessário.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Requisitos de texto acessível
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b566b1960435a856b82a7be605ef5e1f7ba86e2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257789"
---
# <a name="accessible-text-requirements"></a>Requisitos de texto acessível  




Este tópico descreve as práticas recomendadas para acessibilidade de texto em um aplicativo, garantindo que cores e telas de fundo satisfaçam o índice de contraste necessário. Este tópico também aborda as funções de Automação da Interface do Usuário da Microsoft que os elementos de texto em um aplicativo da Plataforma Universal do Windows (UWP) podem ter, e as práticas recomendadas para texto em elementos gráficos.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Taxas de contraste  
Embora os usuários sempre tenham a opção de alternar para um modo de alto contraste, o design do seu aplicativo para texto deve considerar essa possibilidade como último recurso. Uma prática muito melhor é assegurar que o texto do seu aplicativo siga algumas diretrizes estabelecidas para o nível de contraste entre o texto e a tela de fundo. A avaliação do nível de contraste é baseada em técnicas determinísticas que não consideram a tonalidade de cor. Por exemplo, se você tiver texto vermelho sobre fundo verde, esse texto poderá não ser legível por alguém com daltonismo. Verificar e corrigir a taxa de contraste pode evitar esses tipos de problemas de acessibilidade.

As recomendações para contraste de texto são baseadas em um padrão de acessibilidade da Web, o [G18, para garantir que exista, no mínimo, uma relação de contraste de 4,5:1 entre o texto (e as imagens do texto) e a tela de fundo do texto](https://www.w3.org/TR/WCAG20-TECHS/G18.html). Essa orientação está na especificação *W3C Techniques for WCAG 2.0*.

Para ser considerado acessível, o texto visível precisa ter contraste de luminosidade mínimo de 4,5:1 em relação à tela de fundo. As exceções incluem logotipos e texto incidental (como o que faz parte de um componente de interface do usuário inativo).

Texto decorativo e que não expressa informações é excluído. Por exemplo, quando são usadas palavras aleatórias para criar uma tela de fundo, e as palavras podem ser reorganizadas ou substituídas sem alteração de significado, elas são consideradas decorativas e não precisam atender a esse critério.

Use as ferramentas de contraste de cores para verificar se a taxa de contraste de texto visível é aceitável. Consulte [Técnicas para WCAG 2.0 G18 (seção Recursos)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) sobre as ferramentas que podem testar taxas de contraste.

> [!NOTE]
> Algumas das ferramentas listadas pelas Técnicas para WCAG 2.0 G18 não podem ser usadas de forma interativa com um aplicativo UWP. Talvez seja necessário inserir valores de cores da tela de fundo e de primeiro plano manualmente na ferramenta, ou fazer capturas de tela da interface do usuário do aplicativo e depois executar a ferramenta de índice de contraste na imagem de captura de tela.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Funções de elementos de texto  
Um aplicativo UWP pode usar esses elementos padrão (usualmente chamados de *text elements* or *textedit controls*):

* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock): role é [ **texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**Caixa de texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox): a função é [ **editada**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) (e [**RichTextBlockOverflow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)da classe overflow): a função é [**texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox): a função é [ **editada**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)

Quando um controle reporta sua função como [**Editar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType), tecnologias adaptativas supõem que haja formas de os usuários mudarem os valores. Então, se você colocar texto estático em um [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), reportando o papel e, portanto, a estrutura de forma errônea do aplicativo para o usuário de acessibilidade.

Nos modelos de texto de XAML, há dois elementos que são principalmente usados para texto estático, [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) e [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock). Nenhum deles é uma subclasse [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) e, assim, nenhum deles é focalizável no teclado ou pode aparecer na ordem da guia. Mas isso não significa que as tecnologias adaptativas não podem ou não conseguem lê-los. Os leitores de tela são normalmente concebidos para suportar vários modos de leitura de conteúdo em um aplicativo, incluindo um modo de leitura dedicado ou padrões de navegação que vão além do foco e da ordem de tabulação, como um “cursor virtual”. Então, não coloque o seu texto estático em contêineres focalizáveis para a sua ordem de guia leve o usuário até lá. Os usuários de tecnologia adaptativa esperam que qualquer coisa na ordem de guia seja interativa e se encontrarem o texto estático ali, ficarão confusos. Você deve testar isso com o Narrador para ter uma noção da experiência do usuário com seu aplicativo ao usar um leitor de tela para examinar o texto estático do seu aplicativo.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Acessibilidade de sugestão automática  
Quando um usuário digita em um campo de entrada e uma lista de sugestões é exibida, esse tipo de cenário é chamado de sugestão automática. Isso é comum na linha **Para:** de um email, na caixa de pesquisa da Cortana no Windows, no campo de entrada de URL no Microsoft Edge, no campo de entrada de localização no aplicativo Clima, etc. Se você estiver usando um XAML [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) ou os controles HTML intrínsecos, essa experiência já estará disponível para você por padrão. Para tornar essa experiência acessível, o campo de entrada e a lista devem estar associados. Isso é explicado na seção [Implementando a sugestão automática](#implementing_auto-suggest) seção.

O Narrador foi atualizado para tornar esse tipo de experiência acessível por meio de um modo de sugestões especial. Em um nível alto, quando o campo de edição e a lista estão conectados corretamente, o usuário final:

* Saberá que a lista está presente e quando ela é fechada
* Saberá quantas sugestões estão disponíveis
* Conhecerá o item selecionado, se houver
* Poderá mover o foco do Narrador para a lista
* Poderá navegar por uma sugestão com todos os outros modos de leitura

![lista de sugestões](images/autosuggest-list.png)<br/>
_Exemplo de uma lista de sugestões_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>Implementando a sugestão automática  
Para tornar essa experiência acessível, o campo de entrada e a lista devem ser associados na árvore de UIA. Essa associação é feita com a propriedade [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) em aplicativos de desktop ou com a propriedade [ControlledPeers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) em aplicativos UWP.

Em um nível alto, há dois tipos de experiências de sugestão automática.

**Seleção padrão**  
Se uma seleção padrão é feita na lista, o Narrador procura um evento [**UIA_SelectionItem_ElementSelectedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids) em um aplicativo de desktop, ou o evento [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) para ser gerado em um aplicativo UWP. Sempre que a seleção é alterada, quando o usuário digita outra letra e as sugestões foram atualizadas ou quando um usuário navega pela lista, o evento **ElementSelected** deve ser disparado.

![lista com uma seleção padrão](images/autosuggest-default-selection.png)<br/>
_Exemplo em que há uma seleção padrão_

**Nenhuma seleção padrão**  
Se não houver nenhuma seleção padrão, como na caixa de local do aplicativo Clima, o Narrador procurará o evento [**UIA_LayoutInvalidatedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids) de área de trabalho ou o evento [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) UWP para ser disparado na lista sempre que a lista for atualizada.

Lista de ![sem seleção padrão](images/autosuggest-no-default-selection.png)<br/>
_Exemplo em que não há nenhuma seleção padrão_

### <a name="xaml-implementation"></a>Implementação de XAML  
Se você estiver usando [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) de XAML padrão, tudo já estará disponível para você. Se estiver criando sua própria experiência de sugestão automática com um [**TextBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox) e uma lista, você precisará definir a lista como [**AutomationProperties.ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) no **TextBox**. Você deverá disparar o evento **AutomationPropertyChanged** para a propriedade [**ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) sempre que adicionar ou remover essa propriedade e também disparar seu próprio evento [**SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) ou [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) dependendo do tipo de cenário, que foi explicado anteriormente neste artigo.

### <a name="html-implementation"></a>Implementação de HTML  
Se você estiver usando os controles intrínsecos em HTML, a implementação de UIA já foi mapeada para você. Veja a seguir um exemplo de implementação que já está disponível para você:

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 Se você estiver criando seus próprios controles, deverá configurar seus próprios controles ARIA, que são explicados nos padrões W3C.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>Texto em elementos gráficos

Sempre que possível, evite incluir texto em um elemento gráfico. Por exemplo, qualquer texto que você inclua no arquivo de origem da imagem exibido no aplicativo como um elemento [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) não é automaticamente acessível e não pode ser lido por tecnologias adaptativas. Se você tiver que usar texto em elementos gráficos, assegure que o valor [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) que você fornecer como equivalente de "alt text" inclua o texto ou um resumo do significado do texto. Aplicam-se considerações semelhantes se você estiver criando caracteres de testo de vetores como parte de um [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) ou usando [**Glyphs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Glyphs).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>Tamanho e escala da fonte do texto

Os usuários podem ter dificuldade de ler texto em um aplicativo quando as fontes usam são simplesmente muito pequenas, portanto, certifique-se de que qualquer texto em seu aplicativo seja um tamanho razoável em primeiro lugar.

Depois de ter feito o óbvio, o Windows inclui várias ferramentas e configurações de acessibilidade que os usuários podem aproveitar e ajustar às suas próprias necessidades e preferências para ler o texto. São elas:

* A ferramenta Lupa, que amplia uma área selecionada da interface do usuário. Você deve garantir que o layout do texto em seu aplicativo não torna difícil usar a lupa para leitura.
* Configurações globais de escala e resolução em **configurações – > sistema-> exibição-> escala e layout**. Exatamente quais opções de dimensionamento estão disponíveis podem variar, pois isso depende dos recursos do dispositivo de vídeo.
* Configurações de tamanho de texto em **configurações-> facilidade de acesso-> exibição**. Ajuste a configuração **tornar texto maior** para especificar apenas o tamanho do texto em controles de suporte em todos os aplicativos e telas (todos os controles de texto UWP dão suporte à experiência de dimensionamento de texto sem qualquer personalização ou modelagem). 
> [!NOTE]
> A configuração **tornar tudo maior** permite que um usuário especifique seu tamanho preferido para texto e aplicativos em geral somente na tela principal.

Vários elementos e controles de texto têm uma propriedade [**IsTextScaleFactorEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled). Essa propriedade tem o valor **true** por padrão. Quando **true**, o tamanho do texto nesse elemento pode ser dimensionado. O dimensionamento afeta o texto que tem um **FontSize** pequeno para um grau maior do que afeta o texto que tem um **FontSize**grande. Você pode desabilitar o redimensionamento automático definindo a propriedade **IsTextScaleFactorEnabled** de um elemento como **false**. 

Consulte [dimensionamento de texto](https://docs.microsoft.com/windows/uwp/design/input/text-scaling) para obter mais detalhes.

Adicione a marcação a seguir a um aplicativo e execute-o. Ajuste a configuração de **tamanho do texto** e veja o que acontece com cada **TextBlock**.

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Não recomendamos que você desabilite o dimensionamento de texto como dimensionar o texto da interface do usuário universalmente em todos os aplicativos é uma experiência de acessibilidade importante para os usuários.

Você também pode usar o evento [**TextScaleFactorChanged**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) e a propriedade [**TextScaleFactor**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) para descobrir mudanças na configuração do **Tamanho do texto** no telefone. Veja como:

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

O valor de **TextScaleFactor** é um duplo no intervalo \[1, 2,25\]. O menor texto está dimensionado para esse valor. Você pode usar o valor para dimensionar elementos gráficos para ajustar ao texto. Mas lembre-se de que nem todo texto é ampliado pelo mesmo fator. Em geral, quanto maior o texto for, menos ele é afetado pelo dimensionamento.

Esses tipos têm uma propriedade **IsTextScaleFactorEnabled**:  
* [**ContentPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**Controle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) e classes derivadas
* [**FontIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**TextElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.TextElement) e classes derivadas

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  

* [Dimensionamento de texto](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)
* [Acessibilidade](accessibility.md)
* [Informações básicas de acessibilidade](basic-accessibility-information.md)
* [Exemplo de exibição de texto XAML](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)
* [Exemplo de edição de texto XAML](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
* [Exemplo de acessibilidade XAML](https://code.msdn.microsoft.com/windowsapps/XAML-accessibility-sample-d63e820d) 
