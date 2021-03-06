---
description: Descreve as etapas necessárias para garantir que seu aplicativo do Windows seja utilizável quando um tema de alto contraste estiver ativo.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Temas de alto contraste
template: detail.hbs
ms.date: 09/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ce3fe9ea96f4b4ce2f541fb5f7a9682a0dee5e0e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234035"
---
# <a name="high-contrast-themes"></a>Temas de alto contraste  

O Windows dá suporte para temas de alto contraste do sistema operacional e dos aplicativos que os usuários podem optar por habilitar. Os temas de alto contraste usam uma pequena paleta de cores contrastantes que deixa a interface mais fácil de ver.

![Calculadora mostrada no tema claro e no tema Preto em Alto Contraste.](images/high-contrast-calculators.png)

*Calculadora mostrada no tema claro e no tema Preto em Alto Contraste.*

Você pode mudar para um tema de alto contraste usando *Configurações > Facilidade de acesso > Alto contraste*.

> [!NOTE]
> Não confunda temas de alto contraste com temas claros e escuros, que permitem uma paleta bem maior de cores que não é considerada de alto contraste. Para mais temas claros e escuros, consulte o artigo sobre [cor](../style/color.md).

Embora os controles comuns venham com suporte completo para alto contraste de graça, tenha cuidado ao personalizar sua interface do usuário. O bug de alto contraste mais comum é causado pela codificação de uma cor em um controle embutido.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Quando a cor `#E6E6E6` é definida embutida no primeiro exemplo, a grade mantém essa cor de fundo em todos os temas. Se o usuário mudar para o tema Preto em Alto Contraste, ele vai esperar que seu aplicativo tenha uma tela de fundo preta. Como `#E6E6E6` é praticamente branco, alguns usuários podem não conseguir interagir com seu aplicativo.

No segundo exemplo, a [**extensão de marcação {ThemeResource}**](../../xaml-platform/themeresource-markup-extension.md) é usada para fazer referência a uma cor na coleção de [**ThemeDictionaries**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries), uma propriedade dedicada de um elemento [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). O **ThemeDictionaries** permite que o XAML troque automaticamente as cores por você com base no tema atual do usuário.

## <a name="theme-dictionaries"></a>Dicionários de temas

Quando você precisar alterar uma cor padrão do sistema, crie uma coleção de ThemeDictionaries para seu aplicativo.

1. Comece criando o caminho adequado, caso ainda não exista. Em app. XAML, crie uma coleção **ThemeDictionaries** , incluindo **Default** e **HighContrast** no mínimo.
2. Em **Default**, crie o tipo de [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) de que você precisa, geralmente um **SolidColorBrush**. Dê a ele um nome de *x:Key* específico para o que ele está sendo usado.
3. Atribua a **Color** desejada para ele.
4. Copie esse **Brush** em **HighContrast**.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

A última etapa é determinar a cor que será usada em alto contraste, o que é explicado na próxima seção.

> [!NOTE]
> **HighContrast** não é o único nome de chave disponível. Também há **HighContrastBlack**, **HighContrastWhite**e **HighContrastCustom**. Na maioria dos casos, **HighContrast** é tudo o que você precisa.

## <a name="high-contrast-colors"></a>Cores de alto contraste

Na página *Configurações > Facilidade de acesso > Alto contraste*, há 4 temas de alto contraste por padrão. 


![Configurações de alto contraste](images/high-contrast-settings.png)  

*Depois que o usuário seleciona uma opção, a página mostra uma visualização.*  

![Recursos de alto contraste](images/high-contrast-resources.png)  

*Cada amostra de cor na visualização pode ser clicada para alterar seu valor. Cada amostra também é mapeada diretamente para um recurso de cor XAML.*  

Cada recurso **SystemColor*Color** é uma variável que atualiza a cor automaticamente quando o usuário muda os temas de alto contraste. As diretrizes a seguir explicam onde e quando usar cada recurso.

Recurso | Uso |
|--------|-------|
**SystemColorWindowTextColor** | Corpo do texto, títulos, listas; qualquer texto que não permita interação |
| **SystemColorHotlightColor** | Hiperlinks |
| **SystemColorGrayTextColor** | Interface do usuário desabilitada |
| **SystemColorHighlightTextColor** | Cor do primeiro plano do texto ou da interface do usuário que está em andamento, selecionado(a) ou interagindo |
| **SystemColorHighlightColor** | Cor da tela de fundo do texto ou da interface do usuário que está em andamento, selecionado(a) ou interagindo |
| **SystemColorButtonTextColor** | Cor do primeiro plano para botões; qualquer interface do usuário que permita interação |
| **SystemColorButtonFaceColor** | Cor da tela de fundo para botões; qualquer interface do usuário que permita interação |
| **SystemColorWindowColor** | Tela de fundo de páginas, painéis, pop-ups e barras |

Muitas vezes é útil procurar aplicativos existentes, Iniciar ou os controles comuns para ver como outras pessoas resolveram problemas de design de alto contraste similares aos seus.

**O que fazer**

* Respeitar os pares de tela de fundo/primeiro plano sempre que possível.
* Testar os 4 temas de alto contraste enquanto seu aplicativo estiver em execução. O usuário não deve ter de reiniciar seu aplicativo quando mudar de tema.
* Ser consistente.

**O que não fazer**

* Codificar uma cor do tema **HighContrast**; use os recursos **SystemColor*Color**.
* Escolher um recurso de cor por estética. Lembre-se de que elas mudam com o tema!
* Não use **SystemColorGrayTextColor** para o corpo do texto que é secundário ou atua como uma dica.


Para continuar o exemplo anterior, você precisa escolher um recurso para **BrandedPageBackgroundBrush**. Como o nome indica que ele será usado para uma tela de fundo, **SystemColorWindowColor** é uma boa opção.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Posteriormente em seu aplicativo, você pode definir a tela de fundo.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Observe como a ** \{ mera \} ** é usada duas vezes, uma vez para fazer referência a **SystemColorWindowColor** e novamente para fazer referência a **BrandedPageBackgroundBrush**. Ambos são necessários para seu aplicativo aplicar o tema corretamente no tempo de execução. Esse é um bom momento para testar a funcionalidade em seu aplicativo. A tela de fundo da grade será atualizada automaticamente quando você mudar para um tema de alto contraste. Ela também será atualizada ao alternar entre temas de alto contraste diferentes.

## <a name="when-to-use-borders"></a>Quando usar bordas

Páginas, painéis, pop-ups e barras devem usar **SystemColorWindowColor** para a tela de fundo em alto contraste. Adicione uma borda somente de alto contraste quando necessário para preservar limites importantes na interface do usuário.

![Um painel de navegação separado do restante da página](images/high-contrast-actions-content.png)  

*O painel de navegação e a página compartilham a mesma cor da tela de fundo em alto contraste. Uma borda de alto contraste para dividi-las é essencial.*


## <a name="list-items"></a>Itens de lista

Em alto contraste, os itens em [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) têm a tela de fundo definida como **SystemColorHighlightColor** quando são focalizados, pressionados ou selecionados. Itens de lista complexos normalmente têm um bug em que o conteúdo do item de lista não inverte sua cor quando focalizado, pressionado ou selecionado. Isso torna o item impossível de ler.

![Lista simples no tema claro e no tema Preto em Alto Contraste](images/high-contrast-list1.png)

*Uma lista simples no tema claro (à esquerda) e Alto Contraste tema preto (à direita). O segundo item está selecionado; Observe como a cor do texto é invertida em alto contraste.*


### <a name="list-items-with-colored-text"></a>Itens de lista com texto colorido

Um culpado é a definição de TextBlock.Foreground do [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) de ListView. Normalmente, isso é feito para estabelecer a hierarquia visual. A propriedade Foreground é definida no [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem), e os TextBlocks no DataTemplate herdam a cor de primeiro plano correta quando o item é focalizado, pressionado ou selecionado. No entanto, definir a propriedade Foreground interrompe a herança.

![Lista complexa no tema claro e no tema Preto em Alto Contraste](images/high-contrast-list2.png)

*Lista complexa no tema claro (esquerda) e Alto Contraste tema preto (à direita). Em alto contraste, a segunda linha do item selecionado falhou em Inverter.*  

Você pode contornar isso definindo a propriedade Foreground condicionalmente por meio de um estilo que esteja em uma coleção de **ThemeDictionaries**. Como a propriedade **Foreground** não é definida por **SecondaryBodyTextBlockStyle** em **HighContrast**, sua cor será invertida corretamente.

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>Detectando o alto contraste

Você pode verificar programaticamente se o tema atual é um tema de alto contraste usando membros da classe [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings).

> [!NOTE]
> Certifique-se de chamar o construtor **AccessibilitySettings** de um escopo onde o aplicativo é inicializado e o conteúdo já esteja sendo exibido.

## <a name="related-topics"></a>Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Amostra de configurações e contraste da interface do usuário](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [Amostra de acessibilidade XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [Amostra de alto contraste XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)
