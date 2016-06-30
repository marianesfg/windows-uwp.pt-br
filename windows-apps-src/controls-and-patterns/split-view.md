---
author: Jwmsft
title: "Modo divisão"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo."
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 391bfdbbf09474ad707dbbf306d4997825fa8386

---

# Diretrizes para o controle SplitView



**APIs importantes**

-   [**Classe SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objeto SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo. A área de conteúdo está sempre visível. O painel pode ser expandido e recolhido ou ficar em estado aberto, e pode apresentar-se do lado esquerdo ou direito da janela de um aplicativo. O painel tem quatro modos:

-   **Sobreposição**

    O painel ficará oculto até que seja aberto. Quando aberto, o painel se sobrepõe à área de conteúdo.

-   **Embutido**

    O painel está sempre visível e não sobrepõe-se à área de conteúdo. As áreas do painel e de conteúdo dividem o espaço real disponível na tela.

-   **CompactOverlay**

    Uma parte estreita do painel está sempre visível, com largura suficiente para mostrar os ícones. A largura do painel padrão fechado é 48px, que pode ser modificada com `CompactPaneLength`. Se o painel estiver aberto, ele ficará sobreposto à área de conteúdo.

-   **CompactInline**

    Uma parte estreita do painel está sempre visível, com largura suficiente para mostrar os ícones. A largura do painel padrão fechado é 48px, que pode ser modificada com `CompactPaneLength`. Se o painel estiver aberto, ele reduzirá o espaço disponível para o conteúdo, empurrando o conteúdo do seu jeito.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>Esse é o controle correto?

O controle de modo divisão pode ser usado para criar um [painel de navegação](nav-pane.md). Para criar esse padrão, adicione um botão expandir/recolher (o botão "hambúrguer") e um modo de exibição de lista representando os itens de navegação.

O controle de modo divisão também pode ser usado para criar qualquer experiência de "gaveta" na qual os usuários podem abrir e fechar o painel complementar.

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Exemplos

O controle de modo divisão em sua forma padrão é um contêiner básico. Veja a seguir um exemplo do aplicativo Microsoft Edge usando SplitView para mostrar seu Hub.

![Exemplo de modo de exibição de divisão do Microsoft Edge](images/split_view_Edge.png)



## <span id="related_topics"></span>Tópicos relacionados


* [Padrão do painel de navegação](nav-pane.md)
* [Exibição de lista](lists.md)
 

 



<!--HONumber=Jun16_HO4-->


