---
Description: This article is an overview of the concepts and technologies related to accessibility scenarios for Universal Windows Platform (UWP) apps.
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: Visão geral de acessibilidade
label: Accessibility overview
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50e6c68841440120b783713ef0a591e39a7c7eec
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8348280"
---
# <a name="accessibility-overview"></a>Visão geral de acessibilidade  




Este artigo é uma visão geral dos conceitos e tecnologias relacionados a cenários de acessibilidade de aplicativos da Plataforma Universal do Windows (UWP).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>

## <a name="accessibility-and-your-app"></a>Acessibilidade e seu aplicativo  
Há muitas deficiências ou problemas possíveis, incluindo limites de movimento, visão, percepção de cores, audição, fala, cognição e alfabetização. Entretanto, você pode atender à maioria das exigências seguindo as diretrizes oferecidas aqui. Isso significa fornecer:

* Suporte a interações de teclado e leitores de tela.
* Suporte a personalização do usuário, como fonte, ajuste de zoom (ampliação), cor e configurações de alto contraste.
* Alternativas ou suplementos para partes de sua interface do usuário.

Os controles do XAML fornecem suporte interno a teclado e suporte a tecnologias adaptativas, como leitores de tela, que aproveitam as estruturas de acessibilidade que já dão suporte a aplicativos UWP, HTML e outras tecnologias de interface de usuário. Esse suporte interno permite um nível básico de acessibilidade que você pode personalizar com muito pouco trabalho, definindo apenas algumas propriedades. Se você estiver criando seus próprios controles e componentes personalizados de XAML, também poderá adicionar suporte semelhante a esses controles usando o conceito de um *par de automatização*.

Além disso, a vinculação de dados, o estilo e os recursos de modelos facilitam a implementação do suporte para mudanças dinâmicas para as configurações de exibição e texto para interfaces de usuário alternativas.

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>

## <a name="ui-automation"></a>Automação de Interface de Usuário  
O suporte à acessibilidade vem principalmente do suporte integrado à estrutura de Automação da IU da Microsoft. Esse suporte é fornecido através de classes base e do comportamento nativo da implementação de classe para tipos de controle, e uma representação da interface da API do provedor de Automação da Interface do Usuário. Cada classe de controle usa os conceitos de pares e padrões de automação da Automação da Interface do Usuário para relatar a função do controle e o conteúdo para os clientes de Automação da Interface do Usuário. O aplicativo é tratado como janela principal pela Automação da Interface do Usuário e pela Estrutura de Automação da IU todo conteúdo relevante à acessibilidade dentro da janela do aplicativo está disponível para um cliente de Automação da Interface do Usuário. Para saber mais sobre Automação da Interface do Usuário, consulte [UI Automation Overview](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>

## <a name="assistive-technology"></a>Tecnologia adaptativa  
Muitas necessidades de acessibilidade do usuário são atendidas por produtos de tecnologia adaptativa instalados pelo usuário ou por ferramentas e configurações fornecidas pelo sistema operacional. Isso inclui funcionalidades como leitores de tela, ampliação de tela e configurações de alto contraste.

Produtos de tecnologia adaptativa incluem uma ampla variedade de softwares e hardwares. Esses produtos funcionam por meio de estruturas de interface e acessibilidade de teclado padrão que relatam informações sobre o conteúdo e a estrutura de uma interface de usuário para leitores de tela e outras tecnologias adaptativas. Exemplos de produtos de tecnologia adaptativa:

* O Teclado Virtual, que permite às pessoas usar um ponteiro no lugar de um teclado para digitar um texto.
* Software de reconhecimento de voz, que converte as palavras faladas em texto digitado.
* Leitores de tela, que convertem o texto em palavras faladas ou outras formas, como o Braille.
* O leitor de tela Narrador, que é especificamente parte do Windows. O Narrador possui um modo de toque que pode executar tarefas de leitura de tela ao processar gestos de toque quando não houver um teclado disponível.
* Programas ou configurações que ajustam a exibição ou suas áreas, por exemplo, temas de alto contraste, configurações de pontos por polegada (dpi) da exibição ou a ferramenta Lupa.

Os aplicativos que têm um bom suporte a leitor de telas e teclado costumam funcionar bem com diversos produtos de tecnologia adaptativa. Em muitos casos, um aplicativo UWP funciona com esses produtos sem a modificação adicional de informações ou estrutura. Mas convém modificar algumas configurações para obter a melhor experiência de acessibilidade ou para implementar suporte adicional.

Algumas das opções que você pode usar para testar os cenários de acessibilidade básica com tecnologias assistenciais são listadas em [Testes de acessibilidade](accessibility-testing.md).

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>

## <a name="screen-reader-support-and-basic-accessibility-information"></a>Suporte a leitores de tela e informações básicas de acessibilidade  
Os leitores de tela fornecem acesso ao texto em um aplicativo transformando-o em algum outro formato, como um idioma falado ou Braille. O comportamento exato de um leitor de tela depende do software e da configuração do usuário nele.

Por exemplo, alguns leitores de tela leem a interface do usuário do aplicativo inteira quando o usuário executa ou alterna para o aplicativo que está sendo visualizado, permitindo ao usuário receber todo o conteúdo informativo disponível antes de tentar navegar nele. Alguns leitores de telas também leem o texto associado a um controle individual quando ele recebe foco durante a navegação por tabulação. Isso permite que os usuários se orientem conforme navegam pelos controles de entrada de um aplicativo. O Narrador é um exemplo de leitor de tela que fornece os dois comportamentos, dependendo da escolha do usuário.

A informação mais importante que um leitor de tela ou qualquer outra tecnologia adaptativa precisa para ajudar os usuários a entender ou navegar um aplicativo é um *nome acessível* para partes do elementos do aplicativo. Em muitos casos, um controle ou elemento já tem um nome acessível que é calculado a partir de outros valores de propriedade que você forneceu. O caso mais comum em que você pode usar um nome já calculado é com um elemento que suporta e exibe o texto interno. Para outros elementos, você pode precisar contabilizar outras maneiras de fornecer um nome acessível seguindo as práticas recomendadas para a estrutura do elemento. E, às vezes, você precisa fornecer um nome que se destina explicitamente a ser o nome acessível para acessibilidade do aplicativo. Para obter uma lista de quantos desses valores calculados funcionam em elementos comuns de interface do usuário e para saber mais sobre os nomes acessíveis em geral, consulte [Informações básicas de acessibilidade](basic-accessibility-information.md).

Existem várias outras propriedades de automação disponíveis (incluindo as propriedades de teclado descritas na próxima seção). Contudo, nem todos os leitores de tela dão suporte a todas as propriedades de automação. Em geral, você deve definir todas as propriedades e teste de automação apropriadas para dar o suporte mais amplo possível para os leitores de tela.

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>

## <a name="keyboard-support"></a>Suporte ao teclado  
Para fornecer um bom suporte ao teclado, você precisa assegurar que todas as partes de seu aplicativo possam ser usadas com teclado. Se o seu aplicativo usa basicamente os controles padrão e não usa controles personalizados, você já avançou bastante. O modelo de controle XAML básico fornece suporte interno a teclado, incluindo navegação da guia, entrada de texto, e suporte específico de controle. Os elementos que servem como contêineres de layout (como painéis) usam a ordem do layout para estabelecer uma ordem de tabulação padrão. Essa ordem costuma ser a ordem de guia correta a se usar em uma representação acessível de interface do usuário. Se você usa os controles [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) para exibir dados, eles fornecem navegação interna por setas. Ou se você usa um controle [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265), ele já lida com a barra de espaço e a tecla enter para ativação de botões.

Para saber mais sobre todos os aspectos de suporte a teclado, incluindo a ordem de tabulação e a ativação ou navegação baseada em teclas, consulte [Acessibilidade de teclado](keyboard-accessibility.md).

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>

## <a name="media-and-captioning"></a>Mídia e legendas  
Você costuma exibir mídia audiovisual através de um objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926). Você pode usar APIs do **MediaElement** para controlar a reprodução de mídia. Por questões de acessibilidade, forneça controles que permitam que os usuários reproduzam, pausem e interrompam a mídia quando necessário. Às vezes, a mídia inclui componentes adicionais que são destinados à acessibilidade, como legendagem ou faixas de áudio alternativas que incluem descrições narrativas.

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>

## <a name="accessible-text"></a>Texto acessível  
Três aspectos principais do texto são relevantes para a acessibilidade:

* As ferramentas devem determinar se o texto deve ser lido como parte de uma passagem de sequência de guias ou apenas como parte de uma representação geral do documento. Você pode ajudar a controlar essa determinação escolhendo o elemento adequado para exibir o texto ou ajustando as propriedades desses elementos de texto. Cada elemento de texto tem uma finalidade específica, e essa finalidade geralmente tem uma função correspondente de Automação da Interface do Usuário. Usar o elemento errado pode resultar no relatório da função errada para a Automação da Interface do Usuário e na criação de uma experiência confusa para um usuário de tecnologia adaptativa.
* Muitos usuários têm limitações de visão que tornam difícil para eles lerem o texto, a menos que haja contraste adequado no contexto. A maneira na qual isso afeta o usuário não é intuitiva para os designers de aplicativo que não tem essa limitação de visão. Por exemplo, para usuários daltônicos, opções erradas de cores na concepção podem impedir que alguns usuários sejam capazes de ler o texto. As recomendações de acessibilidade que originalmente foram feitas para o conteúdo da Web definem os padrões de contraste que também podem evitar esses problemas em aplicativos. Para obter mais informações, consulte [Requisitos de texto acessível](accessible-text-requirements.md).
* Muitos usuários têm dificuldade em ler texto muito pequeno. Primeiramente, você pode evitar esse problema deixando o texto na interface do usuário do seu aplicativo consideravelmente grande. No entanto, isso é um desafio para os aplicativos que exibem grandes quantidades de texto ou os textos intercalados com outros elementos visuais. Nesses casos, verifique se o aplicativo interage corretamente com os recursos do sistema que podem aumentar a visualização na tela, de modo que todos os textos contidos no aplicativo sejam ampliados com ele. (Alguns usuários alteram os valores de dpi como uma opção de acessibilidade. Essa opção está disponível em **Ampliar itens da tela**, em **Facilidade de Acesso**, que redireciona para uma interface do usuário do **Painel de Controle** para **Aparência e Personalização** / **Tela**).

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>

## <a name="supporting-high-contrast-themes"></a>Oferecendo suporte a temas de alto contraste  
Os controles de IU usam uma representação visual definida como parte de um dicionário de recursos XAML de temas. Um ou mais desses temas é usado especificamente quando o sistema é definido para alto contraste. Quando o usuário alterna para alto contraste, ao consultar o tema adequado de um dicionário de recursos dinamicamente, todos os controles da interface do usuário também usarão um tema de alto contraste adequado. Apenas verifique se você não desabilitou os temas especificando um estilo explícito ou usando outra técnica de estilização que impede que os temas de alto contraste sejam carregados e substituam as mudanças de estilo. Para obter mais informações, consulte [Temas de alto contraste](high-contrast-themes.md).

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>

## <a name="design-for-alternative-ui"></a>Design de interface do usuário alternativa  
Ao projetar seus aplicativos, leve em consideração como eles podem ser usados por pessoas com mobilidade, visão e audição limitadas. Como os produtos de tecnologia adaptativa fazem amplo uso de interfaces do usuário padrão, é particularmente importante fornecer um bom suporte ao teclado e a leitores de tela mesmo se você não fizer outros ajustes de acessibilidade.

Em muitos casos, você pode transportar informações essenciais usando várias técnicas para ampliar a sua audiência. Por exemplo, você pode destacar informações usando informações de ícone e de cor para ajudar aos usuários que são cegos e exibir alertas visuais com efeitos sonoros para ajudar aos usuários que possuem deficiência de audição.

Se necessário, você pode fornecer elementos de interface do usuário acessíveis e alternativos que removem completamente os elementos e animações não essenciais e fornecem outras simplificações para dinamizar a experiência do usuário. O exemplo de código a seguir demonstra como exibir uma instância [**UserControl**](https://msdn.microsoft.com/library/windows/apps/BR227647) em um lugar de outra, de acordo com a configuração do usuário.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">

  <CheckBox x:Name="ShowAccessibleUICheckBox" Click="ShowAccessibleUICheckBox_Click">
    Show Accessible UI
  </CheckBox>

  <UserControl x:Name="ContentBlock">
    <local:ContentPage/>
  </UserControl>

</StackPanel>
```

Visual Basic
```vb
Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If
End Sub
```

C#
```csharp
private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}
```

<span id="Verification_and_publishing"/>
<span id="verification_and_publishing"/>
<span id="VERIFICATION_AND_PUBLISHING"/>

## <a name="verification-and-publishing"></a>Verificação e publicação  
Para saber mais sobre as declarações de acessibilidade e a publicação do seu aplicativo, consulte [Accessibility in the Store](accessibility-in-the-store.md).

> [!NOTE]
> Declarar o aplicativo como acessível somente é relevante para a Microsoft Store.

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>

## <a name="assistive-technology-support-in-custom-controls"></a>Suporte a tecnologia adaptativa em controles personalizados  
Quando você cria um controle personalizado, recomendamos que também implemente ou estenda uma ou mais subclasses [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) para fornecer suporte à acessibilidade. Em alguns casos, desde que você use a mesma classe de par que foi usada pela classe de controle de base, o suporte de automação para sua classe derivada é adequado em um nível básico. Entretanto, você deve testar isso e implementar um par ainda é uma prática recomendada, dessa forma, o par pode relatar corretamente o nome da classe de sua nova classe de controle. Implementar um par de automação personalizado envolve algumas etapas. Para saber mais, consulte [Custom automation peers](custom-automation-peers.md).

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>

## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>Suporte a tecnologia adaptativa em aplicativos que dão suporte à interoperabilidade de XAML/Microsoft DirectX  
O conteúdo do Microsoft DirectX hospedado em uma interface do usuário XAML (usando [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/Dn252834) or [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/Hh702041)) não é acessível por padrão. [XAML SwapChainPanel DirectX interop sample](http://go.microsoft.com/fwlink/p/?LinkID=309155) mostra como criar pares de Automação da Interface do Usuário para o conteúdo DirectX hospedado. Essa técnica deixa o conteúdo hospedado acessível na Automação da Interface do Usuário.

## <a name="related-topics"></a>Tópicos relacionados  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179)
* [Design de acessibilidade](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Acessibilidade](accessibility.md)
* [Introdução ao Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
