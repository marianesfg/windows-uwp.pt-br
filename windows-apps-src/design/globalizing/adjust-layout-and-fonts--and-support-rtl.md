---
Description: Projete seu app de forma a dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda).
title: Ajustar layout e fontes e fornecer suporte para RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, localizabilidade, localização, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: e428dd068337ecd79992e8e27cd193bed112d9c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645301"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajustar layout e fontes e fornecer suporte para RTL
Projete seu app de forma a dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda). Direção de fluxo é a direção na qual o script é criado e exibido e os elementos de interface do usuário na página são visualizados por nós.

## <a name="layout-guidelines"></a>Diretrizes de layout
Idiomas como alemão e finlandês geralmente usam mais caracteres do que o inglês. Fontes do Extremo Oriente geralmente exigem mais altura. E idiomas, como árabe e hebraico, exigem que painéis de layout e elementos de texto sejam dispostos na ordem de leitura da direita para a esquerda (RTL).

Devido a essas variações nas métricas de texto traduzido, recomendamos que você não incorpore o posicionamento absoluto, larguras fixas ou alturas fixas na interface do usuário (IU). Em vez disso, aproveite os mecanismos de layout dinâmico que integram os elementos de interface do usuário do Windows. Por exemplo, controles de conteúdo (como botões), controles de itens (como modos de exibição de grade e lista), painéis de layout (como grades e stackpanels) redimensionam e refluem por padrão para adaptar o conteúdo automaticamente. A pseudotradução do aplicativo revelará todas as margens problemáticas em que os elementos de interface do usuário não acomodam o conteúdo adequadamente.

O layout dinâmico é a técnica recomendada e você poderá usá-la na maioria dos casos. Em vez de inserir tamanhos na marcação da interface do usuário, é melhor definir valores de layout como uma função de idioma. Veja a seguir um exemplo de como você pode expor uma propriedade Width no seu aplicativo como um recurso que os localizadores podem definir adequadamente por idioma. Primeiramente, no Arquivo de Recursos (.resw) do seu aplicativo, adicione um identificador de propriedade com o nome "TitleText.Width" (em vez de "TitleText", você pode usar qualquer nome que desejar). Em seguida, use **x:Uid** para associar um ou mais elementos da interface do usuário a este identificador de propriedade.

```xaml
<TextBlock x:Uid="TitleText">
```

Para obter mais informações sobre Arquivos de Recursos (.resw), identificadores de propriedade, e **x:Uid**, consulte [Localizar cadeias de caracteres em sua IU e manifesto do pacote de aplicativo](../../app-resources/localize-strings-ui-manifest.md).

## <a name="fonts"></a>Fontes
Use a classe de mapeamento de fonte [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) para acesso via programação à família de fontes, tamanho, peso e estilo recomendados para um idioma específico. A classe **LanguageFont** oferece acesso às informações corretas de fonte para várias categorias de conteúdo, incluindo cabeçalhos de interface do usuário, notificações, texto do corpo e fontes de corpo de documento editáveis pelo usuário.

## <a name="mirroring-images"></a>Espelhando imagens
Se o seu app tiver imagens que devem ser espelhadas (ou seja, a mesma imagem pode ser invertida) para RTL, você pode usar **FlowDirection**.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Se o seu app exige uma imagem diferente para inverter a imagem corretamente, você pode usar o sistema de gerenciamento de recursos com o qualificador `LayoutDirection` (consulte a seção LayoutDirection do [Personalizar os recursos de acordo com idioma, escala e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). O sistema escolhe uma imagem chamada `file.layoutdir-rtl.png` quando o idioma de tempo de execução do app (consulte [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md)) estiver definido como idioma da direita para a esquerda (RTL). Essa abordagem pode ser necessária quando alguma parte da imagem é invertida, mas outra parte não.

## <a name="handling-right-to-left-rtl-languages"></a>Processar idiomas da direita para a esquerda (RTL)
Quando o aplicativo é localizado para idiomas da direita para esquerda (RTL), use a propriedade [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection), e defina margens e preenchimento simétricos. Os painéis de layout, como [**Grade**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live), são dimensionados e invertidos automaticamente com o valor de **FlowDirection** que você definir.

Defina **FlowDirection** no painel de layout raiz (ou quadro) da sua página ou na própria página. Isso faz com que todos os controles contidos herdem essa propriedade.

> [!IMPORTANT]
> Entretanto, **FlowDirection** *não* é definido automaticamente com base no idioma de exibição selecionado do usuário nas configurações do Windows; nem muda dinamicamente quando o usuário altera o idioma de exibição. Por exemplo, se o usuário altera as configurações do Windows do inglês para árabe, a propriedade **FlowDirection** *não* muda automaticamente da esquerda para a direita para o contrário. Como o desenvolvedor do aplicativo, você precisa definir **FlowDirection** adequadamente para os idiomas em exibição no momento.

A técnica programática é usar a propriedade `LayoutDirection` do idioma de exibição preferencial do usuário para definir a propriedade [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (veja o exemplo de código abaixo). A maioria dos controles incluídos no Windows já usam **FlowDirection**. Se você estiver implementando um controle personalizado, é necessário usar **FlowDirection** para fazer alterações de layout apropriado para idiomas RTL e LTR.

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

A técnica acima transforma **FlowDirection** em uma função da propriedade `LayoutDirection` do idioma de exibição preferencial do usuário. Se você não quiser que essa lógica por algum motivo, é possível expor uma propriedade FlowDirection no aplicativo como um recurso que os localizadores podem definir apropriadamente para cada idioma traduzido.

Primeiramente, no Arquivo de Recursos (.resw) do seu app, adicione um identificador de propriedade com o nome "MainPage.FlowDirection" (em vez de "MainPage", você pode usar qualquer nome que desejar). Em seguida, use **x:Uid** para associar seu principal elemento de **Página** (ou o painel de layout raiz ou quadro) a este identificador de propriedade.

```xaml
<Page x:Uid="MainPage">
```

Em vez de uma única linha de código para todos os idiomas, isso depende do tradutor que "traduz" esse recurso de propriedade corretamente para cada idioma; portanto, lembre-se de que há a possibilidade de erro humano ao usar essa técnica.

## <a name="important-apis"></a>APIs Importantes
* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Tópicos relacionados
* [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](../../app-resources/localize-strings-ui-manifest.md)
* [Personalize seus recursos de idioma, escala e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Entender os idiomas de perfil do usuário e linguagens de manifesto do aplicativo](manage-language-and-region.md)