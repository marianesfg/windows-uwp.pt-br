---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: Ajustar layout e fontes e fornecer suporte a RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, localizabilidade, localização, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: cf3a2d781dc916fbda9a9d6386dee4e2e6144873
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673973"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajustar layout e fontes e fornecer suporte a RTL

Projete seu app de forma a dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda). Direção de fluxo é a direção na qual o script é criado e exibido e os elementos de interface do usuário na página são visualizados por nós.

## <a name="layout-guidelines"></a>Diretrizes de layout

Idiomas como alemão e finlandês geralmente usam mais caracteres do que o inglês. Fontes do Extremo Oriente geralmente exigem mais altura. E idiomas, como árabe e hebraico, exigem que painéis de layout e elementos de texto sejam dispostos na ordem de leitura da direita para a esquerda (RTL).

Devido ao comprimento variável do texto traduzido, você deve usar mecanismos dinâmicos de layout da IU em vez de posicionamento absoluto, larguras fixas ou alturas fixas. Pseudolocalizar seu app revelará todas as margens problemáticas em que os elementos de interface do usuário não acomodam o conteúdo adequadamente.

No que se refere a idiomas escritos da direita para a esquerda (RTL), use a propriedade [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection), e defina margens e preenchimento simétricos. Os painéis de layout, como [**Grade**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live), são dimensionados e invertidos automaticamente com o valor de **FlowDirection** que você definir.

Veja a seguir um exemplo de como você pode expor uma propriedade FlowDirection no seu app como um recurso que os localizadores podem definir adequadamente.

Primeiramente, no Arquivo de Recursos (.resw) do seu app, adicione um identificador de propriedade com o nome "MainPage.FlowDirection" (em vez de "MainPage", você pode usar qualquer nome que desejar).

Em seguida, use **x:Uid** para associar seu principal elemento de **Página** a este identificador de propriedade.

```xaml
<Page x:Uid="MainPage">
```

Para obter mais informações sobre Arquivos de Recursos (.resw), identificadores de propriedade, e **x:Uid**, consulte [Localizar cadeias de caracteres em sua IU e manifesto do pacote de aplicativo](../../app-resources/localize-strings-ui-manifest.md).

Você deve evitar definir os valores absolutos de layout em qualquer elemento de interface do usuário com base no idioma. Entretanto, se for absolutamente inevitável, você pode criar um identificador de propriedade do formulário "TitleText.Width".

```xaml
<TextBlock x:Uid="TitleText">
```

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

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>Práticas recomendadas para lidar com idiomas escritos da direita para a esquerda (RTL)

Quando seu app está traduzido para idiomas escritos da direita para a esquerda (RTL), use APIs para definir a direção de texto padrão para o painel de layout raiz da sua Página. Isso faz com que todos os controles contidos no painel raiz respondam adequadamente à direção de texto padrão. Quando mais de um idioma for compatível, use `LayoutDirection` para o idioma de tempo de execução do app superior a fim de definir a propriedade [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consulte o exemplo de código abaixo). A maioria dos controles incluídos no Windows já usam **FlowDirection**. Se você estiver implementando controles personalizados, eles deverão usar **FlowDirection** para fazer alterações de layout apropriado para idiomas RTL e LTR.

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

### <a name="rtl-faq"></a>PERGUNTAS FREQUENTES SOBRE RTL 

**P:** O **FlowDirection** é definido automaticamente com base na seleção de idioma atual? Por exemplo, se eu selecionar inglês, ele exibirá da esquerda para a direita e, se eu selecionar árabe, ele exibirá da direita para a esquerda?

> **R:** O **FlowDirection** não leva em conta o idioma. Você define **FlowDirection** adequadamente para o idioma que você está exibindo atualmente. Veja o código de exemplo acima.

**P:** Não estou muito familiarizado com a localização. Os recursos já contêm direção de fluxo? É possível determinar a direção do fluxo do idioma atual?

> **R:** se você estiver usando as práticas recomendadas atuais, os recursos não contêm direção de fluxo diretamente. Você deve determinar a direção de fluxo para o idioma atual. Veja aqui duas maneiras de fazer isso.
> 
> A maneira preferida é usar **LayoutDirection** para o principal idioma preferencial para definir a propriedade **FlowDirection** de RootFrame. Todos os controles de RootFrame herdam FlowDirection do RootFrame.
> 
> Outra maneira é definir o FlowDirection no arquivo .resw para os idiomas RTL que você está traduzindo. Por exemplo, você pode ter um arquivo resw árabe e um arquivo resw hebraico. Nesses arquivos, você pode usar X:UID para definir a FlowDirection. No entanto, esse método é mais propenso a erros que o método programático.

## <a name="important-apis"></a>APIs Importantes

* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Tópicos relacionados

* [Localizar as cadeias de caracteres em seu IU e o manifesto do pacote do aplicativo](../../app-resources/localize-strings-ui-manifest.md)
* [Personalizar os recursos de acordo com idioma, escala e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md)