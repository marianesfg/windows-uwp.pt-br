---
title: Modo divisão
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo.
label: Split view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e5df29f4ce91bd272fd921b23b5fc05b6655d4c0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081535"
---
# <a name="split-view-control"></a>Controle de modo de exibição dividido

Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo.

> **APIs importantes**: [Classe SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView)

Veja a seguir um exemplo do aplicativo Microsoft Edge usando SplitView para mostrar seu Hub.

![Exemplo de modo de exibição de divisão do Microsoft Edge](images/split_view_Edge.png)


 A área de conteúdo da exibição dividida está sempre visível. O painel pode ser expandido e recolhido ou ficar em estado aberto, e pode apresentar-se do lado esquerdo ou direito da janela de um aplicativo. O painel tem quatro modos:

-   **Sobreposição**

    O painel ficará oculto até que seja aberto. Quando aberto, o painel se sobrepõe à área de conteúdo.

-   **Embutido**

    O painel está sempre visível e não sobrepõe-se à área de conteúdo. As áreas do painel e de conteúdo dividem o espaço real disponível na tela.

-   **CompactOverlay**

    Uma parte estreita do painel está sempre visível, com largura suficiente para mostrar os ícones. A largura do painel padrão fechado é 48px, que pode ser modificada com `CompactPaneLength`. Se o painel estiver aberto, ele ficará sobreposto à área de conteúdo.

-   **CompactInline**

    Uma parte estreita do painel está sempre visível, com largura suficiente para mostrar os ícones. A largura do painel padrão fechado é 48px, que pode ser modificada com `CompactPaneLength`. Se o painel estiver aberto, ele reduzirá o espaço disponível para o conteúdo, empurrando o conteúdo do seu jeito.

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

O controle de modo divisão também pode ser usado para criar qualquer experiência de "gaveta" na qual os usuários podem abrir e fechar o painel complementar. Por exemplo, você pode usar SplitView para criar o padrão [mestre/detalhes](master-details.md).

Se você quiser criar um menu de navegação com um botão expandir/recolher e uma lista de itens de navegação, use o controle [NavigationView](navigationview.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/SplitView">abrir o aplicativo e ver o SplitView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-split-view"></a>Criar um modo de exibição dividido

Eis um controle SplitView com um painel aberto sendo exibido embutido ao lado do conteúdo.
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) – confira todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados
- [Padrão do painel de navegação](navigationview.md)
- [Modo de exibição de lista](lists.md)
- [Mestre/detalhes](master-details.md)