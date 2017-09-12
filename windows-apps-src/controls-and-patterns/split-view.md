---
author: Jwmsft
title: "Modo divisão"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo."
label: Split view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 126fab3db9a0728626289788757f576648a43856
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="split-view-control"></a>Controle de modo de exibição dividido

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo.

> **APIs importantes**: [classe SplitView](https://msdn.microsoft.com/library/windows/apps/dn864360)

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

O controle de modo divisão pode ser usado para criar um [painel de navegação](navigationview.md). Para criar esse padrão, adicione um botão expandir/recolher (o botão "hambúrguer") e um modo de exibição de lista representando os itens de navegação.

O controle de modo divisão também pode ser usado para criar qualquer experiência de "gaveta" na qual os usuários podem abrir e fechar o painel complementar.

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



## <a name="related-topics"></a>Tópicos relacionados
* [Padrão do painel de navegação](navigationview.md)
* [Modo de exibição de lista](lists.md)
 

 
