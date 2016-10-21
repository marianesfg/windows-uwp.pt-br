---
author: DelfCo
Description: "Desenvolva seu aplicativo para dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda)."
title: Ajustar layout e fontes e fornecer suporte para RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 5255da14ccdd0aed3852c41fa662de63a7160fba
ms.openlocfilehash: b45029156a28afdb37d7ac1402d1e6ae845b0e63

---

# Ajustar layout e fontes e fornecer suporte para RTL





Desenvolva seu aplicativo para dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda).

## Diretrizes de layout


Alguns idiomas, como alemão e finlandês, exigem mais espaço de texto do que o inglês. As fontes para alguns idiomas, como japonês, exigem mais altura. E alguns idiomas, como árabe e hebraico, exigem que o layout do texto e o layout do aplicativo tenham a ordem de leitura da direita para a esquerda.

Use mecanismos de layout flexíveis em vez de posicionamento absoluto, larguras fixas ou alturas fixas. Quando necessário, determinados elementos de interface do usuário podem ser ajustados com base no idioma.

### XAML

Especifique um **Uid** para um elemento:

```XML
<TextBlock x:Uid="Block1">
```

Verifique se o arquivo ResW do aplicativo tem um recurso para Block1.Width, que você pode definir para cada idioma que traduz.

Para aplicativos da Windows Store em C++, C# ou Visual Basic, use a propriedade [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) com preenchimento e margens simétricos, para permitir a localização para outras direções de layout.

Controles de layout XAML como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) são dimensionados e invertidos automaticamente com a propriedade [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716). Exponha sua própria propriedade **FlowDirection** no seu aplicativo como um recurso para tradutores.

Especifique um **Uid** para a página principal do seu aplicativo:

```XML
<Page x:Uid="MainPage">
```

Assegure-se de que o arquivo **ResW** do aplicativo tenha um recurso para MainPage.FlowDirection, que você possa definir para cada idioma em que traduzi-lo.

### HTML

Para os aplicativos da Windows Store em JavaScript, use mecanismos de layout de [folhas de estilo em cascata (CSS)](https://msdn.microsoft.com/library/ms531209), como [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) e [–ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section). Use preenchimento e margens simétricos para permitir a localização para várias direções de layout.

Seu aplicativo também pode usar o seletor de pseudoclasse [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) para ajustar propriedades CSS, como a largura em determinados elementos com base no idioma do aplicativo. Para ativar esse recurso, o Host de Aplicativo define o atributo **lang** do elemento raiz para o idioma do aplicativo.

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

Os aplicativos da Windows Store em JavaScript que usam as folhas estilo ui-light.css ou ui-dark.css têm a direção do layout do corpo definida automaticamente, com base no idioma do aplicativo. O CSS seguinte está em ui-light e ui-dark.css, e você não precisa escrevê-lo.

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

Isso significa que a maioria dos layouts de aplicativo são definidos corretamente quando o sistema usa um idioma da direita para a esquerda.

Como os controles [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782) seu aplicativo pode usar o seletos de pseudoclasse [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) para ajustar as propriedades físicas de CSS, como **margin** and **padding**. Você não precisa ajustar as propriedades lógicas de CSS que usam palavras-chave como **after** e **before**.

Não use a propriedade ou atributo **align** em HTML. Em vez disso, use a propriedade **direction** ara controlar o alinhamento de componentes específicos.

Use a propriedade [**writing-mode**](https://msdn.microsoft.com/library/ms531187) para dar suporte a layouts de texto vertical no CSS.

## Espelhando imagens


### XAML

Se o seu aplicativo tiver imagens que devem ser espelhadas (ou seja, a mesma imagem pode ser invertida) para RTL, você pode aplicar a propriedade [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716).

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### HTML

Se o seu aplicativo tiver imagens que devem ser espelhadas (ou seja, a mesma imagem pode ser invertida), você pode usar as transformações do CSS para espelhar suas imagens no momento da renderização adicionando uma classe .mirrorable aos seus elementos e adicionando a seguinte classe de CSS:

```CSS
.mirrorable { transform: scaleX(-1); }
```

**Para XAML e HTML:** se o seu aplicativo exige uma imagem diferente para invertê-la corretamente, você pode usar o sistema de gerenciamento de recursos com o [qualificador layoutdir](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324). O sistema escolhe uma imagem chamada file.layoutdir-rtl.png quando o [idioma do aplicativo](manage-language-and-region.md) é definido como um idioma da direita para a esquerda (RTL) Essa abordagem pode ser necessária quando alguma parte da imagem é invertida, mas outra parte não.

## Fontes


**Para XAML e HTML:** use as APIs de mapeamento de fonte [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) para acessar via programação a família de fontes, tamanho, peso e estilo recomendados para um idioma específico. O objeto **LanguageFont** oferece acesso às informações corretas de fonte para várias categorias de conteúdo, incluindo cabeçalhos de interface do usuário, notificações, texto do corpo e fontes de corpo de documento editáveis pelo usuário.

### HTML

Os aplicativos da Windows Store em JavaScript que usam as folhas estilo ui-light.css ou ui-dark.css têm a fonte definida automaticamente como a mais adequada, com base no idioma do aplicativo. O host de aplicativo define o atributo **lang** do elemento raiz para o idioma do aplicativo.

Os aplicativos que exibem vários idiomas em uma única página devem definir o atributo **lang** para a seção em cada idioma. O seletor de pseudoclasse [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) seleciona a fonte correta para cada seção da página.

## Práticas recomendadas para lidar com idiomas escritos da direita para a esquerda (RTL)

Quando seu aplicativo está traduzido para idiomas escritos da direita para a esquerda, use APIs para definir a direção de texto padrão para o RootFrame. Isso fará com que todos os controles contidos no RootFrame respondam adequadamente à direção de texto padrão.  Quando mais de um idioma for compatível, use LayoutDirection para o idioma preferencial superior para definir a propriedade FlowDirection. A maioria dos controles incluídos no Windows já usam FlowDirection. Se você estiver implementando controles personalizados, eles deverão usar FlowDirection para fazer alterações de layout apropriado para idiomas RTL e LTR.

C#
```csharp    
// For bidirectional languages, determine flow direction for RootFrame and all derived UI.

    string resourceFlowDirection = ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
    if (resourceFlowDirection == "LTR")
    {
       RootFrame.FlowDirection = FlowDirection.LeftToRight;
    }
    else
    {
       RootFrame.FlowDirection = FlowDirection.RightToLeft;
    }
```
C++:
```cpp
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### PERGUNTAS FREQUENTES SOBRE RTL 

<dl>
  <dt> <p><b>P:</b> O <b>FlowDirection</b> é definido automaticamente com base na seleção de idioma atual? Por exemplo, se eu selecionar inglês, ele exibirá da esquerda para a direita e, se eu selecionar árabe, ele exibirá da direita para a esquerda?</p></dt>

  <dd><p><b>R: O </b> <b>FlowDirection</b> não leva em conta o idioma. Você define <b>FlowDirection</b> adequadamente para o idioma que você está exibindo atualmente. Veja o código de exemplo acima.</p></dd> 

  <dt> <p><b>P:</b> Não estou muito familiarizado com a localização. Os recursos já contêm direção de fluxo? É possível determinar a direção do fluxo do idioma atual?</p></dt>

  <dd> <p><b>R:</b> se você estiver usando as práticas recomendadas atuais, os recursos não contêm direção de fluxo diretamente. Você deve determinar a direção de fluxo para o idioma atual. Veja aqui duas maneiras de fazer isso: </p>
   <p>A maneira preferida é usar LayoutDirection para o principal idioma preferencial para definir a propriedade FlowDirection de RootFrame. Todos os controles de RootFrame herdam FlowDirection do RootFrame.</p>
   <p>Outra maneira é definir o FlowDirection no arquivo resw para idiomas RTL para os quais você está localizando. Por exemplo, você pode ter um arquivo resw árabe e um arquivo resw hebraico. Nesses arquivos, você pode usar X:UID para definir a FlowDirection. No entanto, esse método é mais propenso a erros que o método programático.</p></dd>
</dl>


## Tópicos relacionados
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)



<!--HONumber=Aug16_HO3-->


