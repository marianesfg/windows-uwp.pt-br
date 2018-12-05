---
Description: Ink tools described
title: Controles de escrita à tinta
label: Inking Controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: a76a03eaff3d831b48e7b86c0b70ff2cbae7360a
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8707268"
---
# <a name="inking-controls"></a>Controles de escrita à tinta



Há dois controles diferentes que facilitam a escrita à tinta em aplicativos da Plataforma Universal do Windows (UWP): [InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) e [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

O controle InkCanvas renderiza uma entrada à caneta como um traço de tinta (usando as configurações padrão de cor e espessura) ou um traço de apagar. Esse controle é uma sobreposição transparente que não inclui interfaces do usuário internas para alterar as propriedades de traço de tinta padrão.

> [!NOTE]
> InkCanvas pode ser configurado para dar suporte a uma funcionalidade semelhante para entrada por mouse e toque.

Como o controle InkCanvas não incluir suporte para alterar as configurações de traço de tinta padrão, ele pode combinado com um controle InkToolbar. O InkToolbar contém uma coleção personalizável e extensível de botões que ativam recursos relacionados à tinta em um InkCanvas associado.

Por padrão, o InkToolbar inclui botões para desenhar, apagar, realçar e exibir uma régua. Dependendo do recurso, outras configurações e comandos, como cor da tinta, espessura do traço, apagar toda a tinta, são fornecidos em um submenu.

> [!NOTE]
> InkToolbar dá suporte à entrada por caneta e mouse e pode ser configurado para reconhecer a entrada por toque.

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **APIs importantes**: [classe InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx), [classe InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx), [classe InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx), [Windows.UI.Input.Inking](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use o InkCanvas quando você precisar habilitar recursos básicos de escrita à tinta em seu aplicativo sem fornecer todas as configurações de tinta ao usuário.

Por padrão, os traços são renderizados como tinta ao usar a ponta da caneta (uma caneta esferográfica preta com espessura de 2 pixels) e como borracha ao usar a ponta da borracha. Se uma ponta de borracha não estiver presente, o InkCanvas poderá ser configurado para processar a entrada da ponta da caneta como um traço para apagar.

Emparelhe o InkCanvas com um InkToolbar a fim de oferecer uma interface do usuário para ativar recursos de tinta e configurar propriedades de tinta básicas, como tamanho do traço, cor e forma da ponta da caneta.

> [!NOTE] 
> Para obter uma personalização mais abrangente da renderização do traço de tinta em um InkCanvas, use o objeto [InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) subjacente.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/InkCanvas">abrir o aplicativo e ver o InkCanvas em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

O Microsoft Edge usa o InkCanvas e InkToolbar para **Anotações Web**.  
![InkCanvas é usado para tinta no Microsoft Edge](images/ink-tools-edge.png)

**Espaço de Trabalho do Windows Ink**

O InkCanvas e InkToolbar também são usados para **Bloco de esboços** e **Esboço da tela** no **Espaço de Trabalho do Windows Ink**.  
![InkToolbar no Espaço de Trabalho do Windows Ink](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>Criar um InkCanvas e InkToolbar

Adicionar um InkCanvas ao aplicativo requer apenas uma linha de marcação:

```xaml
<InkCanvas x:Name=“myInkCanvas”/>
```

> [!NOTE]
> Para obter uma personalização do InkCanvas detalhada usando o InkPresenter, consulte o artigo ["Interações com caneta em aplicativos UWP"](http://windowsstyleguide/input/pen-and-stylus-interactions/).

O controle InkToolbar deve ser usado em conjunto com um InkCanvas. A incorporação de um InkToolbar (com todas as ferramentas internas) ao seu aplicativo requer uma linha de marcação adicional:

 ```xaml
<InkToolbar TargetInkCanvas=“{x:Bind myInkCanvas}”/>
 ```

Isso exibe o InkToolbar a seguir:
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>Botões internos

O InkToolbar contém os seguintes botões internos:

**Canetas**

- Caneta esferográfica – desenha um traço sólido opaco com uma ponta de caneta circular. O tamanho do traço depende da pressão da caneta detectada.
- Lápis – desenha um traço de borda suave texturizado e semitransparente (útil para efeitos de sombra em camadas) com uma ponta de caneta circular. A cor do traço (escuridão) depende da pressão da caneta detectada.
- Marca-texto – desenha um traço semitransparente com uma ponta de caneta retangular.

Você pode personalizar os atributos de paleta de cores e tamanho (mín, máx, padrão) no menu suspenso de cada caneta.

**Ferramenta**

- Borracha – exclui qualquer traço de tinta tocado. Observe que o inteiro traço de tinta é excluído, não apenas a parte sob o traço da borracha.

**Alternância**

- Régua – mostra ou oculta a régua. Desenhar perto da borda da régua faz com que o traço de tinta se ajuste à régua.  
 ![Elemento visual de régua associado ao InkToolbar](images/inking-tools-ruler.png)

Embora essa seja a configuração padrão, você tem controle total sobre quais botões internos estão incluídos no InkToolbar para seu aplicativo.

### <a name="custom-buttons"></a>Botões personalizados

O InkToolbar consiste em dois grupos distintos de tipos de botões:

1. Um grupo de botões de "ferramentas" que contém os botões internos para desenhar, apagar e realçar. Canetas personalizadas e ferramentas são adicionadas aqui.
> [!NOTE]
> A seleção de recursos é mutuamente excludente.

2. Um grupo de botões de "alternância" que contém o botão de régua interno. As alternâncias personalizadas são adicionadas aqui.
> [!NOTE]
> Os recursos não são mutuamente excludentes e podem ser usados concomitantemente com outras ferramentas ativas.

Dependendo de seu aplicativo e da funcionalidade de escrita à tinta necessária, você pode adicionar qualquer um dos seguintes botões (associados aos seus recursos de tinta personalizados) ao InkToolbar:

- Caneta personalizada – uma caneta para a qual as propriedades de paleta de cores de tinta e ponta da caneta, como tamanho, rotação e forma, são definidas pelo aplicativo host.
- Ferramenta personalizada – uma ferramenta sem caneta, definida pelo aplicativo host.
- Alternância personalizada – define o estado de um recurso definido pelo aplicativo como ativado ou desativado. Quando ativado, o recurso funciona com a ferramenta ativa.

> [!NOTE]
> Não é possível alterar a ordem de exibição dos botões internos. A ordem de exibição padrão é: caneta esferográfica, lápis, marca-texto, borracha e régua. Canetas personalizadas são acrescentadas à última caneta padrão, botões de ferramenta personalizados são adicionados entre o último botão de caneta e o botão de borracha e botões de alternância personalizados são adicionados após o botão de régua. (Os botões personalizados são adicionados na ordem em que são especificados.)

Embora o InkToolbar possa ser um item de nível superior, ele normalmente é exposto por meio de um botão ou comando de "Escrita à tinta". Recomendamos usar o glifo EE56 da fonte Segoe MLD2 Assets como um ícone de nível superior.

## <a name="inktoolbar-interaction"></a>Interação do InkToolbar

Todos os botões de caneta e ferramenta internos contêm um submenu onde as propriedades da tinta e a forma e o tamanho da ponta da caneta podem ser definidos. Um "glifo de extensão" ![Glifo InkToolbar](images/ink-tools-glyph.png) é exibido no botão para indicar a existência do submenu.

O submenu é mostrado quando o botão de uma ferramenta ativa é selecionado novamente. Quando a cor ou o tamanho forem alterados, o submenu será ignorado automaticamente e a escrita à tinta poderá ser retomada. As canetas e ferramentas personalizadas podem usar o submenu padrão ou especificar um submenu personalizado.

A borracha também tem um submenu que fornece o comando **Apagar Toda a Tinta**.  
![InkToolbar com o submenu de borracha invocado](images/ink-tools-erase-all-ink.png)

 Para obter informações sobre personalização e extensibilidade, confira o [exemplo SimpleInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- O InkCanvas, e a escrita à tinta em geral, oferece a melhor experiência com uma caneta ativa. No entanto, é recomendável dar suporte à escrita à tinta com entrada de mouse e toque (inclusive caneta passiva), se exigido por seu aplicativo.
- Use um controle InkToolbar com InkCanvas para fornecer configurações e recursos de escrita à tinta básicos. O InkCanvas e InkToolbar podem ser personalizados de forma programática.
- O InkToolbar, e a escrita à tinta em geral, oferece a melhor experiência com uma caneta ativa. No entanto, a escrita à tinta com mouse e toque pode ter suporte, se exigido por seu aplicativo.
- Para dar suporte à escrita à tinta com entrada por toque, recomendamos usar o ícone ED5F da fonte Segoe MLD2 Assets para o botão de alternância, com uma dica de ferramenta "Escrita por toque".
- Se você fornecer seleção de traço, recomendamos usar o ícone EF20 da fonte Segoe MLD2 Assets para o botão de ferramenta, com uma dica de ferramenta "Ferramenta de seleção".
- Se for usar mais de um InkCanvas, recomendamos usar um único InkToolbar para controlar a escrita à tinta em telas.
- Para obter o melhor desempenho, recomendamos alterar o submenu padrão, em vez de criar um personalizado para ferramentas padrão e personalizadas.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- O [exemplo SimpleInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) - Demonstra 8 cenários de recursos de personalização e extensibilidade dos controles InkCanvas e InkToolbar. Cada cenário fornece orientação básica sobre situações comuns de escrita à tinta e implementações de controle.
- [Exemplo de ComplexInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) - Demonstra cenários de escrita à tinta mais avançados.
- [Exemplo de XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) -Ver todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Interações com caneta em aplicativos UWP](http://windowsstyleguide/input/pen-and-stylus-interactions/)
- [Reconhecer traços de tinta](http://windowsstyleguide/input/convert-ink-to-text/)
- [Armazenar e recuperar traços de tinta](http://windowsstyleguide/input/save-and-load-ink/)
