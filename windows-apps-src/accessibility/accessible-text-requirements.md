---
author: Xansky
Description: "Este tópico descreve as práticas recomendadas para acessibilidade de texto em um aplicativo, garantindo que cores e telas de fundo satisfaçam o índice de contraste necessário."
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: "Requisitos de texto acessível"
label: Accessible text requirements
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 1307b4f70cf7ffed300f4254a7d92b67b5afd085

---

# Requisitos de texto acessível  




Este tópico descreve as práticas recomendadas para acessibilidade de texto em um aplicativo, garantindo que cores e telas de fundo satisfaçam o índice de contraste necessário. Este tópico também aborda as funções de Automação da Interface do Usuário da Microsoft que os elementos de texto em um aplicativo da Plataforma Universal do Windows (UWP) podem ter, e as práticas recomendadas para texto em elementos gráficos.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>
## Taxas de contraste  
Embora os usuários sempre tenham a opção de alternar para um modo de alto contraste, o design do seu aplicativo para texto deve considerar essa possibilidade como último recurso. Uma prática muito melhor é assegurar que o texto do seu aplicativo siga algumas diretrizes estabelecidas para o nível de contraste entre o texto e a tela de fundo. A avaliação do nível de contraste é baseada em técnicas determinísticas que não consideram a tonalidade de cor. Por exemplo, se você tiver texto vermelho sobre fundo verde, esse texto poderá não ser legível por alguém com daltonismo. Verificar e corrigir a taxa de contraste pode evitar esses tipos de problemas de acessibilidade.

As recomendações para contraste de texto são baseadas em um padrão de acessibilidade da Web, o [G18, para garantir que exista, no mínimo, uma relação de contraste de 4,5:1 entre o texto (e as imagens do texto) e a tela de fundo do texto](http://go.microsoft.com/fwlink/p/?linkid=221823). Essa orientação está na especificação *W3C Techniques for WCAG 2.0*.

Para ser considerado acessível, o texto visível precisa ter contraste de luminosidade mínimo de 4,5:1 em relação à tela de fundo. As exceções incluem logotipos e texto incidental (como o que faz parte de um componente de interface do usuário inativo).

Texto decorativo e que não expressa informações é excluído. Por exemplo, quando são usadas palavras aleatórias para criar uma tela de fundo, e as palavras podem ser reorganizadas ou substituídas sem alteração de significado, elas são consideradas decorativas e não precisam atender a esse critério.

Use as ferramentas de contraste de cores para verificar se a taxa de contraste de texto visível é aceitável. Consulte [Técnicas para WCAG 2.0 G18 (seção Recursos)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) sobre as ferramentas que podem testar as taxas de contraste.

> [!NOTE]
> Algumas das ferramentas listadas pelas Técnicas para WCAG 2.0 G18 não podem ser usadas de forma interativa com um aplicativo UWP. Talvez seja necessário inserir valores de cores da tela de fundo e de primeiro plano manualmente na ferramenta, ou fazer capturas de tela da interface do usuário do aplicativo e depois executar a ferramenta de índice de contraste na imagem de captura de tela.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>
## Funções de elementos de texto  
Um aplicativo UWP pode usar esses elementos padrão (usualmente chamados de *text elements* or *textedit controls*):

* [
              **TextBlock**
            ](https://msdn.microsoft.com/library/windows/apps/BR209652): a função é [**Texto**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
              **TextBox**
            ](https://msdn.microsoft.com/library/windows/apps/BR209683): a função é [**Editar**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
              **RichTextBlock**
            ](https://msdn.microsoft.com/library/windows/apps/BR227565) (e classe excedente [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow)): a função é [**Texto**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
              **RichEditBox**
            ](https://msdn.microsoft.com/library/windows/apps/BR227548): a função é [**Editar**](https://msdn.microsoft.com/library/windows/apps/BR209182)

Quando um controle reporta sua função como [**Editar**](https://msdn.microsoft.com/library/windows/apps/BR209182), tecnologias adaptativas supõem que haja formas de os usuários mudarem os valores. Então, se você colocar texto estático em um [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), reportando o papel e, portanto, a estrutura de forma errônea do aplicativo para o usuário de acessibilidade.

Nos modelos de texto de XAML, há dois elementos que são principalmente usados para texto estático, [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) e [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565). Nenhum deles é uma subclasse [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) e, assim, nenhum deles é focalizável no teclado ou pode aparecer na ordem da guia. Mas isso não significa que as tecnologias adaptativas não podem ou não conseguem lê-los. Os leitores de tela são normalmente concebidos para suportar vários modos de leitura de conteúdo em um aplicativo, incluindo um modo de leitura dedicado ou padrões de navegação que vão além do foco e da ordem de tabulação, como um “cursor virtual”. Então, não coloque o seu texto estático em contêineres focalizáveis para a sua ordem de guia leve o usuário até lá. Os usuários de tecnologia adaptativa esperam que qualquer coisa na ordem de guia seja interativa e se encontrarem o texto estático ali, ficarão confusos. Você deve testar isso por si próprio com o Narrador para ter uma noção da experiência do usuário com seu aplicativo ao usar um leitor de tela para examinar o texto estático do seu aplicativo.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>
## Texto em elementos gráficos  
Sempre que possível, evite incluir texto em um elemento gráfico. Por exemplo, qualquer texto que você inclua no arquivo de origem da imagem exibido no aplicativo como um elemento [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) não é automaticamente acessível e não pode ser lido por tecnologias adaptativas. Se você tiver que usar texto em elementos gráficos, assegure que o valor [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) que você fornecer como equivalente de "alt text" inclua o texto ou um resumo do significado do texto. Aplicam-se considerações semelhantes se você estiver criando caracteres de testo de vetores como parte de um [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) ou usando [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>
## Tamanho da fonte do texto  
Muitos leitores têm dificuldades de ler textos em aplicativos quando a fonte é muito pequena para ser lida. Primeiramente, você pode evitar esse problema deixando o texto na interface do usuário do seu aplicativo consideravelmente grande. As tecnologias adaptativas também fazem parte do Windows e permitem ao usuário alterar os tamanhos das visualizações dos aplicativos ou a exibição em geral.

* Alguns usuários alteram os valores de pontos por polegada (dpi) da exibição principal como uma opção de acessibilidade. Essa opção está disponível em **Ampliar itens da tela**, em **Facilidade de Acesso**, que redireciona para uma interface do usuário do **Painel de Controle** para **Aparência e Personalização** / **Tela**. As opções de dimensionamento que realmente estão disponíveis podem variar, pois dependem dos recursos de exibição do dispositivo.
* A ferramenta Lupa pode ampliar uma área selecionada da interface do usuário. No entanto, é dificultoso utilizar a ferramenta Lupa para leitura de texto.

<span id="Text_scale_factor"/>
<span id="text_scale_factor"/>
<span id="TEXT_SCALE_FACTOR"/>
## Fator de escala de texto  
Vários elementos e controles de texto têm uma propriedade [**IsTextScaleFactorEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.istextscalefactorenabled). Essa propriedade tem o valor **true** por padrão. Quando o valor é **true**, a configuração chamada **Dimensionamento de texto** no telefone (**Configurações &gt; Facilidade de acesso**) faz o tamanho do texto naquele elemento aumentar. O dimensionamento afetará textos com **FontSize** menor em um grau maior do que textos com **FontSize** maior. Mas você pode desabilitar o aumento automático definindo a propriedade **IsTextScaleFactorEnabled** de um elemento como **false**. Tente esta marcação, ajuste a configuração **Text size** no telefone e veja o que acontece com os **TextBlock**:

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Porém, não desabilite o aumento automático de forma rotineira, porque o dimensionamento de texto de interface do usuário de forma geral em todos os aplicativos é uma experiência de acessibilidade importante para os usuários e eles esperam que ele funcione no seu aplicativo também.

Você também pode usar o evento [**TextScaleFactorChanged**](https://msdn.microsoft.com/library/windows/apps/Dn633867) e a propriedade [**TextScaleFactor**](https://msdn.microsoft.com/library/windows/apps/Dn633866) para descobrir mudanças na configuração do **Tamanho do texto** no telefone. Veja como:

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

O valor do **TextScaleFactor** é o dobro no intervalo \[1,2\]. O menor texto está dimensionado para esse valor. Você pode usar o valor para dimensionar elementos gráficos para ajustar ao texto. Mas lembre-se de que nem todo texto é ampliado pelo mesmo fator. Em geral, quanto maior o texto for, menos ele é afetado pelo dimensionamento.

Esses tipos têm uma propriedade **IsTextScaleFactorEnabled**:  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [
              **Control**
            ](https://msdn.microsoft.com/library/windows/apps/BR209390) e classes derivadas
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [
              **TextElement**
            ](https://msdn.microsoft.com/library/windows/apps/BR209967) e classes derivadas

<span id="related_topics"/>
## Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Informações básicas de acessibilidade](basic-accessibility-information.md)
* [Amostra de exibição de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238579)
* [Amostra de edição de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=251417)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)



<!--HONumber=Jun16_HO4-->


