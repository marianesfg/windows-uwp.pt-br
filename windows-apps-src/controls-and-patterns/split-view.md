---
author: Jwmsft
title: "Modo divisão"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo."
label: Split view
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b4f0c84c4fdf273e7ddf2c16e3323ad0c4e52359
ms.lasthandoff: 02/07/2017

---
# <a name="split-view-control"></a>Controle de modo de exibição dividido

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe SplitView**](https://msdn.microsoft.com/library/windows/apps/dn864360)</li>
</ul>
</div>

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

O controle de modo divisão pode ser usado para criar um [painel de navegação](nav-pane.md). Para criar esse padrão, adicione um botão expandir/recolher (o botão "hambúrguer") e um modo de exibição de lista representando os itens de navegação.

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
* [Padrão do painel de navegação](nav-pane.md)
* [Modo de exibição de lista](lists.md)
 

 

