---
title: Modo divisão
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo.
label: Modo divisão
template: detail.hbs
---

# Diretrizes para o controle de modo divisão


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**Classe SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objeto SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Um controle de modo divisão tem um painel que pode ser expandido/recolhido e uma área de conteúdo. A área de conteúdo está sempre visível. O painel pode ser expandido e recolhido ou ficar em estado aberto, e pode apresentar-se do lado esquerdo ou direito da janela de um aplicativo. O painel tem três modos:

-   **Sobreposição**

    O painel ficará oculto até que seja aberto. Quando aberto, o painel se sobrepõe à área de conteúdo.

-   **Embutido**

    O painel está sempre visível e não sobrepõe-se à área de conteúdo. As áreas do painel e de conteúdo dividem o espaço real disponível na tela.

-   **Compacto**

    Neste modo, o painel está sempre visível, com largura suficiente para mostrar os ícones (normalmente 48 epx de largura). O painel e a área de conteúdo dividem o espaço real disponível na tela. Embora o modo compacto padrão não se sobreponha à área de conteúdo, ele pode se transformar em um painel mais largo para mostrar mais conteúdo, se sobrepondo à área de conteúdo.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>Este é o controle correto?


O controle de modo divisão pode ser usado para criar um [padrão de painel de navegação](nav-pane.md). Para criar esse padrão, adicione um botão expandir/recolher (o botão "hambúrguer") e um modo de exibição de lista ao controle de modo divisão.

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Exemplos


O controle de modo divisão em sua forma padrão é um contêiner básico. Com um botão e uma exibição de lista adicionados, o controle de modo divisão está pronto como menu de navegação. Estes são exemplos do modo divisão como um menu de navegação, nos modos expandido e compacto.

![um exemplo de um menu de modo divisão no modo de sobreposição e o modo compacto](images/controls-splitview-menu01.png)
## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Recomendações


-   Ao usar o modo divisão em um menu de navegação, recomendamos colocar controles de navegação no painel que permitam o acesso a outras áreas do aplicativo. O uso do painel de navegação fornece uma experiência de usuário consistente. Além disso, essa implementação de menu pode ajudar o usuário a familiarizar-se com todas as partes de um aplicativo, fornecer acesso rápido à home page do aplicativo e incentivar os usuários a explorar mais áreas dele.

\[Este artigo contém informações que são específicas dos aplicativos UWP (Plataforma Universal do Windows) e do Windows 10. Para obter as diretrizes do Windows 8.1, baixe o [PDF de diretrizes do Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743).\]

## <span id="related_topics"></span>Tópicos relacionados


* [Padrão do painel de navegação](nav-pane.md)
* [Exibição de lista](lists.md)
 

 






<!--HONumber=Mar16_HO1-->


