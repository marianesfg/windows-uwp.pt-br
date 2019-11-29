---
description: A prática de definição da interface do usuário na forma de marcação XAML declarativa traduz extremamente bem dos aplicativos Universal 8.1 para aplicativos da Plataforma Universal do Windows (UWP).
title: Portabilidade do Windows Runtime 8.x XAML e da interface do usuário para a UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19e754fd6a52880c7bc636818acaeda815f9da16
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259102"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Portabilidade do Windows Runtime 8.x XAML e da interface do usuário para a UWP


O tópico anterior foi [Solução de problemas](w8x-to-uwp-troubleshooting.md).

A prática de definição da interface do usuário na forma de marcação XAML declarativa traduz extremamente bem dos aplicativos Universal 8.1 para aplicativos da Plataforma Universal do Windows (UWP). Você descobrirá que a maior parte da sua marcação é compatível, embora talvez seja necessário fazer alguns ajustes nas chaves de recurso do sistema ou nos modelos personalizados que você está usando. O código imperativo em seus modelos de exibição exigirão pouca ou nenhuma alteração. Muito, ou grande parte, do código de sua camada de apresentação que manipula elementos da interface do usuário deve ser simples de portar.

## <a name="imperative-code"></a>Código imperativo

Se você só quiser chegar ao estágio onde o seu projeto é compilado, poderá comentar ou apagar qualquer código não essencial. Em seguida, itere, um problema por vez, e consulte os seguintes tópicos desta seção (e o tópico anterior: [Solução de problemas](w8x-to-uwp-troubleshooting.md)), até que todos os problemas de compilação e do runtime sejam corrigidos e a portabilidade seja concluída.

## <a name="adaptiveresponsive-ui"></a>Interface do usuário responsiva/adaptável

Como seu aplicativo pode potencialmente ser executado em uma ampla variedade de dispositivos, cada um com seu próprio tamanho da tela e resolução, você vai querer ir além das etapas mínimas para portar seu aplicativo. Você também vai querer adaptar a interface do usuário para ter a melhor aparência possível nesses dispositivos. Você pode usar o recurso adaptável do Gerenciador de Estado Visual para detectar dinamicamente o tamanho da janela e mudar o layout em resposta, e um exemplo de como fazer isso é mostrado na seção [Interface do usuário adaptável](w8x-to-uwp-case-study-bookstore2.md) no tópico do estudo de caso Bookstore2.

## <a name="back-button-handling"></a>Manipulação do botão Voltar

Para aplicativos Universal 8,1, Windows Runtime aplicativos 8. x e aplicativos da Windows Phone Store têm abordagens diferentes para a interface do usuário mostrada e os eventos que você manipula para o botão voltar. Mas, para aplicativos do Windows 10, você pode usar uma única abordagem em seu aplicativo. Em dispositivos móveis, o botão é fornecido a você como um botão capacitivo no dispositivo ou como um botão no shell. Em um dispositivo desktop, você adiciona um botão para elementos visuais cromados do seu aplicativo sempre que a navegação regressiva for possível dentro do aplicativo, e isso aparece na barra de título para aplicativos de janela ou na barra de tarefas para o modo Tablet. O evento do botão Voltar é um conceito universal em todas as famílias de dispositivos, e os botões implementados no hardware ou no software acionam o mesmo evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested).

O exemplo abaixo funciona para todas as famílias de dispositivos e é bom para casos em que o mesmo processamento se aplica a todas as páginas, e você não precisa confirmar a navegação (por exemplo, para avisar sobre alterações não salvas).

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

Há também uma abordagem única para sair de forma programática do aplicativo usada em todas as famílias de dispositivos.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>Botões

Você não precisa alterar nenhum código que se integre com os botões, mas você precisa adicionar alguma interface do usuário ao seu aplicativo para colocar a barra de botões, que não faz parte do shell do Windows 10. Um aplicativo universal 8,1 em execução no Windows 10 tem sua própria interface do usuário substituta fornecida pelo Chrome renderizado pelo sistema na barra de título do aplicativo.

## <a name="controls-and-control-styles-and-templates"></a>Controles, estilos e modelos de controle

Um aplicativo universal 8,1 em execução no Windows 10 manterá a aparência e o comportamento de 8,1 em relação aos controles. Mas, quando você porta esse aplicativo para um aplicativo do Windows 10, há algumas diferenças na aparência e no comportamento que devem ser considerados. A arquitetura e o design dos controles são essencialmente inalterados para aplicativos do Windows 10, portanto, as alterações estão em grande parte da linguagem de design, da simplificação e das melhorias de usabilidade.

**Observe**   o estado visual do PointerOver é relevante em estilos/modelos personalizados em aplicativos do Windows 10 e em aplicativos Windows Runtime 8. x, mas não em aplicativos da loja Windows Phone. Por esse motivo (e por causa das chaves de recurso do sistema com suporte para aplicativos do Windows 10), recomendamos que você reutilize os estilos/modelos personalizados de seus aplicativos Windows Runtime 8. x quando estiver portando seu aplicativo para o Windows 10.
Se você quiser ter certeza de que seus estilos/modelos personalizados estão usando o conjunto mais recente de Estados visuais e que são beneficiando de melhorias de desempenho feitas nos estilos/modelos padrão, edite uma cópia do novo modelo padrão do Windows 10 e aplique novamente a sua personalização. Um exemplo de uma melhoria no desempenho é que qualquer controle **Border** que anteriormente contornava um **ContentPresenter** ou um painel foi removido e um elemento filho agora renderiza a borda.

Veja alguns exemplos mais específicos de mudanças nos controles.

| Nome do controle | Alteração |
|--------------|--------|
| **AppBar**   | Se você estiver usando o controle **AppBar** (em vez disso, o[**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) é recomendado), ele não estará oculto por padrão em um aplicativo do Windows 10. É possível controlar isso usando a propriedade [**AppBar.ClosedDisplayMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode). |
| **AppBar**, [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Em um aplicativo do Windows 10, **AppBar** e [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) têm um botão **Ver mais** (as reticências). |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Em um aplicativo Windows Runtime 8. x, os comandos secundários de um [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) estão sempre visíveis. Em um aplicativo da Windows Phone Store e em um aplicativo do Windows 10, o não aparece até que a barra de comandos seja aberta. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Para um aplicativo da Loja do Windows Phone, o valor de [**CommandBar.IsSticky**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.issticky) não afeta se a barra é light dismiss. Para um aplicativo do Windows 10, se **Isadesivoy** estiver definido como true, o **CommandBar** desconsiderará um gesto de descarte claro. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Em um aplicativo do Windows 10, [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) não manipula os eventos [**EdgeGesture. Completed**](https://docs.microsoft.com/uwp/api/windows.ui.input.edgegesture.completed) ou [**UIElement. RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped) . Ele também não reage a um toque nem a um passar de dedo para cima. Você ainda tem a opção de manipular esses eventos e definir o [**IsOpen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.isopen). |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [ **timepeguer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Examine a aparência do seu aplicativo com as alterações visuais em [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) e [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker). Para um aplicativo do Windows 10 em execução em um dispositivo móvel, esses controles não navegam mais para uma página de seleção, mas sim usam um popup quaisquer dispensáveis. |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [ **timepeguer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Em um aplicativo do Windows 10, você não pode colocar [](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) ou timechecker dentro de uma interrupção. Se você quiser que esses controles sejam exibidos em um controle de tipo pop-up, você pode usar [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) e [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout). |
| **GridView**, **ListView** | Para **GridView**/**ListView**, consulte [Alterações na GridView e ListView](#gridview-and-listview-changes). |
| [**82801ER**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Em um aplicativo da Loja do Windows Phone, um controle [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) encapsula da última seção à primeira. Em um aplicativo Windows Runtime 8. x e em um aplicativo do Windows 10, as seções do Hub não são encapsuladas. |
| [**82801ER**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Em um aplicativo da Loja do Windows Phone, uma imagem de plano de fundo do controle [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) move no paralaxe em relação às seções do hub. Em um aplicativo Windows Runtime 8. x e em um aplicativo do Windows 10, o da Parallax não é usado. |
| [**82801ER**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)  | Em um aplicativo universal 8.1, a propriedade [**HubSection.IsHeaderInteractive**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive) faz com que o cabeçalho da seção, e um glifo de divisa renderizado ao lado dele, se tornem interativos. Em um aplicativo do Windows 10, há uma "ver mais" interativa ao lado do cabeçalho, mas o cabeçalho em si não é interativo. **IsHeaderInteractive** ainda determina se a interação gera o evento [**Hub.SectionHeaderClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick). |
| **MessageDialog** | Caso você esteja usando **MessageDialog**, leve em consideração usar o [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), que é mais flexível. Além disso, consulte o exemplo [Noções básicas de interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics). |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** e **PickerFlyout** são preteridos para um aplicativo do Windows 10. Para um menu suspenso de seleção única, use [**MenuFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout); para obter experiências mais complexas, use [**Flyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout). |
| [**PasswordBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | A propriedade [**PasswordBox. IsPasswordRevealButtonEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled) é preterida em um aplicativo do Windows 10 e a configuração não tem nenhum efeito. Em vez disso, use [**PasswordBox. PasswordRevealMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) , que assume a **inspeção** (na qual um glifo ocular é exibido, como em um aplicativo Windows Runtime 8. x). Além disso, consulte [Diretrizes para caixas de senha](https://docs.microsoft.com/windows/uwp/controls-and-patterns/password-box). |
| [**Pivô**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) | O controle [**Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) agora é universal e não tem mais o uso limitado a dispositivos móveis. |
| [**SearchBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | Embora [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) seja implementado na família de dispositivos Universal, não é totalmente funcional em dispositivos móveis. Veja [SearchBox preterido em favor do AutoSuggestBox](#searchbox-deprecated-in-favor-of-autosuggestbox). |
| **SemanticZoom** | Para **SemanticZoom**, consulte [alterações em SemanticZoom changes](#semanticzoom-changes). |
| [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | Algumas propriedades padrão do [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) mudaram. [**HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) é **auto**, [**VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) é **auto**e [**zoommode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) está **desabilitado**. Caso os novos valores padrão não sejam apropriados para o aplicativo, você pode alterá-los em um estilo ou como valores locais no próprio controle.  |
| [**Texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Em um aplicativo Windows Runtime 8. x, a verificação ortográfica está desativada por padrão para uma [**caixa de texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Em um aplicativo da Windows Phone Store e em um aplicativo do Windows 10, ele fica ativado por padrão. |
| [**Texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | O tamanho de fonte padrão para um [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) mudou de 11 para 15. |
| [**Texto**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | O valor padrão de [**TextBox.TextReadingOrder**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) foi alterado de **Default** para **DetectFromContent**. Se não for esta a sua vontade, use **UseFlowDirection**. **Default** é preterido. |
| Vários | A cor de destaque se aplica a aplicativos da Windows Phone Store e a aplicativos do Windows 10, mas não a Windows Runtime aplicativos 8. x.  |

Para saber mais sobre controles do aplicativo UWP, consulte [Controles por função](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function), [Lista de controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/) e [Diretrizes para controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index).

##  <a name="design-language-in-windows10"></a>Linguagem de design no Windows 10

Há algumas diferenças pequenas, mas importantes, na linguagem de design entre aplicativos do Universal 8,1 e do Windows 10. Para saber todos os detalhes, consulte [Design](https://developer.microsoft.com/en-us/windows/apps/design). Apesar das alterações na linguagem de design, nossos princípios de design permanecem consistentes: atenção aos detalhes, mas sempre buscando simplicidade por meio da concentração no conteúdo, e não no cromo, reduzindo drasticamente elementos visuais e permanecendo autêntico ao domínio digital; usar a hierarquia visual especialmente com tipografia; projetar em uma grade e dar vida às suas experiências com animações suaves.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>Pixels efetivos, distância exibição e fatores de escala

Antes, os pixels de exibição eram a maneira de abstrair o tamanho e o layout dos elementos da interface do usuário do tamanho físico e da resolução reais dos dispositivos. Agora os pixels de exibição evoluíram para pixels efetivos. Esta é uma explicação do termo, o que significa e o valor adicional que oferece.

O termo "resolução" refere-se a uma medida da densidade de pixels e não, como comumente se pensa, uma contagem de pixels. "Resolução efetiva" é a maneira como os pixels físicos que compõem uma imagem ou um glifo são resolvidos para o olho dadas as diferenças na distância de exibição e no tamanho físico de pixels do dispositivo (a densidade de pixels sendo a recíproca do tamanho físico de pixels). A resolução efetiva é uma boa métrica para criar uma experiência porque é centrada no usuário. Ao compreender todos os fatores e controlar o tamanho dos elementos da interface do usuário, você poderá melhorar a experiência do usuário.

Os diferentes dispositivos têm um número diferente de pixels efetivos de largura, desde 320 epx para os dispositivos menores até 1024 epx para um monitor de tamanho moderado, e muito além disso para larguras muito maiores. Você só precisa continuar a usar elementos de dimensionamento automático e painéis de layout dinâmicos como você sempre fez. Também haverá alguns casos em que você definirá as propriedades dos elementos da interface do usuário para um tamanho fixo na marcação XAML. Um fator de escala é aplicado automaticamente ao aplicativo dependendo do dispositivo em que é executado e das configurações de exibição feitas pelo usuário. E esse fator de escala serve para que qualquer elemento da interface do usuário com um tamanho fixo continue apresentando um alvo de toque (e leitura) de tamanho mais ou menos constante para o usuário em diversos tamanhos de tela. E com o layout dinâmico, sua interface do usuário não vai simplesmente ser dimensionada para caber em dispositivos diferentes. Em vez disso, ela fará o que for necessário para ajustar a quantidade adequada de conteúdo ao espaço disponível.

Para que seu aplicativo tenha a melhor experiência em todas as telas, recomendamos que você crie cada ativo de bitmap em um intervalo de tamanhos, cada um adequado a um fator de escala específico. O fornecimento de ativos em escala de 100%, 200% e 400% (nessa ordem de prioridade) trará excelentes resultados na maioria dos casos em todos os fatores de escala intermediários.

**Observe**  se, por qualquer motivo, você não puder criar ativos em mais de um tamanho, crie ativos de 100% de escala. No Microsoft Visual Studio, o modelo de projeto padrão para aplicativos UWP fornece ativos de identidade visual (logotipos e imagens do bloco) em um único tamanho, mas eles não estão na escala de 100%. Quando criar ativos para seu próprio aplicativo, siga as diretrizes nesta seção e forneça tamanhos de 100%, 200% e 400%, além de usar pacotes de ativos.

Caso você tenha uma arte complexa, convém fornecer os ativos em ainda mais tamanhos. Caso você seja um principiante na arte vetorial, é relativamente fácil gerar ativos de alta qualidade em qualquer fator de escala.

Não recomendamos que você tente dar suporte a todos os fatores de escala, mas a lista completa de fatores de escala para aplicativos do Windows 10 é 100%, 125%, 150%, 200%, 250%, 300% e 400%. Se você o fizer, a Loja selecionará os ativos de tamanho correto para cada dispositivo, e somente esses ativos serão baixados. A Loja seleciona os ativos a serem baixados com base no DPI do dispositivo. Você pode reutilizar os ativos do seu aplicativo Windows Runtime 8. x em fatores de escala, como 140% e 220%, mas seu aplicativo será executado em um dos novos fatores de escala e, portanto, um dimensionamento de bitmap será inevitável. Teste seu aplicativo em uma variedade de dispositivos para ver se você está satisfeito com os resultados no seu caso.

Você pode estar reutilizando a marcação XAML de um aplicativo Windows Runtime 8. x em que valores de dimensão literais são usados na marcação (talvez para dimensionar formas ou outros elementos, talvez para tipografia). Mas, em alguns casos, um fator de escala maior é usado em um dispositivo para um aplicativo do Windows 10 do que para um aplicativo universal 8,1 (por exemplo, 150% é usado onde 140% estava antes e 200% é usado onde o 180%). Portanto, se você descobrir que esses valores literais agora são muito grandes no Windows 10, tente multiplica-los por 0,8. Para obter mais informações, consulte [Design responsivo 101 para aplicativos UWP](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="gridview-and-listview-changes"></a>Alterações em ListView e GridView

Foram feitas várias alterações nos setters de estilo padrão de [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) para fazer o controle rolar verticalmente (em vez de horizontalmente, como o padrão anterior). Se você tiver editado uma cópia do estilo padrão no projeto, a cópia não terá essas alterações, logo, você precisará fazê-las manualmente. Aqui está uma lista das alterações.

-   O setter para [**ScrollViewer.HorizontalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) foi alterado de **Auto** para **Disabled**.
-   O setter para [**ScrollViewer.VerticalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) foi alterado de **Disabled** para **Auto**.
-   O setter para [**ScrollViewer.HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) foi alterado de **Enabled** para **Disabled**.
-   O setter para [**ScrollViewer.VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) foi alterado de **Disabled** para **Enabled**.
-   No setter para [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel), o valor de [**ItemsWrapGrid.Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation) foi alterado de **Vertical** para **Horizontal**.

Se essa última alteração (a alteração em **Orientation**) parecer contraditória, lembre-se de que estamos nos referindo a uma grade de encapsulamento. A grade de encapsulamento orientada horizontalmente (o novo valor) é semelhante a um sistema de escrita onde o texto flui horizontalmente e se quebra para a próxima linha no final de uma página. Uma página de texto assim rola verticalmente. Inversamente, uma grade de encapsulamento orientada verticalmente (o valor anterior) é semelhante a um sistema de escrita em que o texto flui na vertical e, portanto, rola na horizontal.

Aqui estão os aspectos de [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) e [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) que têm alterações ou não têm suporte no Windows 10.

-   A propriedade [**IsSwipeEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled) (Windows Runtime apenas aplicativos 8. x) não tem suporte para aplicativos do Windows 10. A API ainda está presente, mas sua definição não tem efeito. Todos os gestos de seleção anteriores têm suporte, exceto o de passar o dedo para baixo (que não tem suporte porque os dados mostram que ele não é detectável) e o clique com o botão direito do mouse (que é reservado para mostrar um menu de contexto).
-   A propriedade [**reordenemode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode) (somente aplicativos da Windows Phone Store) não tem suporte para aplicativos do Windows 10. A API ainda está presente, mas sua definição não tem efeito. Em vez disso, defina [**AllowDrop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop) e [**CanReorderItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) como true em **GridView** ou **ListView**, e o usuário poderá reordenar usando um gesto de pressionar e manter pressionado (ou clicar e arrastar).
-   Ao desenvolver para o Windows 10, use [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) em vez de [**GridViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter) em seu estilo de contêiner de item, tanto para [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) quanto para [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). Se editar uma cópia dos estilos de contêiner do item padrão, você terá o tipo correto.
-   Os visuais de seleção foram alterados para um aplicativo do Windows 10. Caso você defina [**SelectionMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) como **Multiple**, por padrão, uma caixa de seleção é renderizada para cada item. A configuração padrão para itens **ListView** significa que a caixa de seleção é disposta embutida ao lado do item e, consequentemente, o espaço ocupado pelo restante do item será ligeiramente reduzido e deslocado. Para itens **GridView**, a caixa de seleção é sobreposta acima do item por padrão. Mas, em ambos os casos, você pode controlar o layout (embutido ou sobreposto) das caixas de seleção (com a propriedade [**CheckMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode)) e se elas são exibidas (com a propriedade [**SelectionCheckMarkVisualEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled)) no elemento [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) dentro do estilo de contêiner do item, como no exemplo abaixo.
-   No Windows 10, o evento [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) é gerado duas vezes por item durante a virtualização da interface do usuário: uma vez para a recuperação e uma vez para a reutilização. Se o valor de [**InRecycleQueue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) for **true** e você não tiver nenhum trabalho de recuperação especial para fazer, você poderá sair do manipulador de eventos imediatamente com a garantia de que ele será reinserido quando o mesmo item for reutilizado (no momento em que **InRecycleQueue** será **false**).

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![um listviewitempresenter com caixa de seleção embutida](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

Um ListViewItemPresenter com caixa de seleção embutida

![um listviewitempresenter com uma caixa de seleção sobreposta](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

Um ListViewItemPresenter com uma caixa de seleção sobreposta

-   Com a remoção dos gestos de passar o dedo para baixo e clique direito com o botão direito do mouse para seleção (pelos motivos indicados acima), o modelo de interação foi alterado, e uma consequência disso é que os eventos [**ItemClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) e [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) deixam de ser mutuamente excludentes. Para seu aplicativo do Windows 10, examine seus cenários e decida se deseja adotar o modelo de interação "seleção" ou "invocar". Para obter detalhes, consulte [Como alterar o modo de interação](https://docs.microsoft.com/previous-versions/windows/apps/hh780625(v=win.10)).
-   Há algumas alterações nas propriedades que você usa para definir o estilo de [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter). As novas propriedades são [**CheckBoxBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush), [**PressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground), [**SelectedPressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground) e [**FocusSecondaryBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush). As propriedades ignoradas para um aplicativo do Windows 10 são [**preenchimentos**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding) (use [**ContentMargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin) em vez disso), [**CheckHintBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush), [**CheckSelectingBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush), [**PointerOverBackgroundMargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin), [**ReorderHintOffset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset), [**SelectedBorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness)e [**SelectedPointerOverBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush).

Esta tabela descreve as alterações feitas nos estados visuais e nos grupos de estados visuais nos modelos de controle [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) e [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem).

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [indisponível]       |
|                     | Desabilitado                |                   | [indisponível]       |
|                     | [indisponível]           |                   | PointerOverSelected |
|                     | [indisponível]           |                   | Selected            |
|                     | [indisponível]           |                   | PressedSelected     |
| [indisponível]       |                         | DisabledStates    |                     |
|                     | [indisponível]           |                   | Desabilitado            |
|                     | [indisponível]           |                   | Habilitado             |
| SelectionHintStates |                         | [indisponível]     |                     |
|                     | VerticalSelectionHint   |                   | [indisponível]       |
|                     | HorizontalSelectionHint |                   | [indisponível]       |
|                     | NoSelectionHint         |                   | [indisponível]       |
| [indisponível]       |                         | MultiSelectStates |                     |
|                     | [indisponível]           |                   | MultiSelectDisabled |
|                     | [indisponível]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [indisponível]     |                     |
|                     | Unselecting             |                   | [indisponível]       |
|                     | Unselected              |                   | [indisponível]       |
|                     | UnselectedPointerOver   |                   | [indisponível]       |
|                     | UnselectedSwiping       |                   | [indisponível]       |
|                     | Selecting               |                   | [indisponível]       |
|                     | Selected                |                   | [indisponível]       |
|                     | SelectedSwiping         |                   | [indisponível]       |
|                     | SelectedUnfocused       |                   | [indisponível]       |

Caso você tenha um modelo de controle [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) ou [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) personalizado, examine-o considerando as alterações acima. Recomendamos que você recomece editando uma cópia do novo modelo padrão e reaplicando a personalização a ele. Se, por qualquer motivo, você não puder fazer isso e precisar editar o modelo existente, aqui estão algumas diretrizes gerais sobre como fazer isso.

-   Adicione o novo grupo de estados visuais MultiSelectStates.
-   Adicione o novo estado visual MultiSelectDisabled.
-   Adicione o novo estado visual MultiSelectEnabled.
-   Adicione o novo grupo de estados visuais DisabledStates.
-   Adicione o novo estado visual Enabled.
-   No grupo de estados visuais CommonStates, remova o estado visual PointerOverPressed.
-   Mova o estado visual Disabled para o grupo de estados visuais DisabledStates.
-   Adicione o novo estado visual PointerOverSelected.
-   Adicione o novo estado visual PressedSelected.
-   Remova o grupo de estados visuais SelectedHintStates.
-   No grupo de estados visuais SelectionStates, mova o estado visual Selected para o grupo de estados visuais CommonStates.
-   Remova todo o grupo de estados visuais SelectionStates.

## <a name="localization-and-globalization"></a>Localização e globalização

Você pode reutilizar os arquivos Resources.resw do seu projeto Universal 8.1 em seu projeto de aplicativo UWP. Depois de copiar o arquivo, adicione-o ao projeto e defina **Ação de Compilação** como **PRIResource** e **Copiar para Diretório de Saída** como **Não copiar**. O tópico [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) descreve como carregar recursos específicos à família de dispositivos com base no fator de seleção de recurso de família de dispositivos.

## <a name="play-to"></a>Reproduzir em

As APIs no namespace [**Windows. Media. playto**](https://docs.microsoft.com/uwp/api/Windows.Media.PlayTo) são preteridas para aplicativos do Windows 10 em favor das APIs [**Windows. Media. elenco**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting) .

## <a name="resource-keys-and-textblock-style-sizes"></a>Chaves de recurso e tamanhos de estilo TextBlock

A linguagem de design evoluiu para o Windows 10 e, consequentemente, alguns estilos de sistema foram alterados. Em alguns casos, você desejará revisitar os designs visuais dos modos de exibição de maneira que eles fiquem em harmonia com as propriedades de estilo que foram alteradas.

Em outros casos, as chaves de recurso deixam de ser compatíveis. O editor de marcação XAML no Visual Studio realça referências a chaves de recurso que não podem ser resolvidas. Por exemplo, o editor de marcação XAML sublinhará uma referência à chave de estilo `ListViewItemTextBlockStyle` com uma linha ondulada vermelha. Se isso não for corrigido, o aplicativo será encerrado imediatamente quando você tentar implantá-lo no emulador ou no dispositivo. Portanto, é importante estar atento à correção da marcação XAML. E você descobrirá que o Visual Studio é uma ótima ferramenta para identificar esses problemas.

Para chaves que ainda têm suporte, as mudanças na linguagem de design significam que as propriedades definidas por alguns estilos foram alteradas. Por exemplo, `TitleTextBlockStyle` define **FontSize** como 14.667 PX em um aplicativo Windows Runtime 8. x e 18.14 PX em um aplicativo da loja Windows Phone. Porém, o mesmo estilo define **FontSize** para um 24px muito maior em um aplicativo do Windows 10. Revise os designs e layouts e use os estilos apropriados nos locais corretos. Para obter mais informações, consulte [Diretrizes para fontes](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) e [Crie aplicativos UWP](https://developer.microsoft.com/en-us/windows/apps/design).

Esta é uma lista completa das chaves que não têm suporte.

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>SearchBox preterido em favor do AutoSuggestBox

Embora [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) seja implementado na família de dispositivos Universal, não é totalmente funcional em dispositivos móveis. Use [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) para a sua experiência de pesquisa universal. Aqui está como normalmente implementar uma experiência de pesquisa com **AutoSuggestBox**.

Depois que o usuário começa a digitar, o **TextChanged** evento é acionado, com um motivo de **UserInput**. Você então preenche a lista de sugestões e define o **ItemsSource** do [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox). Conforme o usuário navega na lista, o evento **SuggestionChosen** é acionado (e, se você tiver definido **TextMemberDisplayPath**, a caixa de texto é preenchida automaticamente com a propriedade especificada). Quando o usuário envia uma opção com a tecla Enter, o **QuerySubmitted** evento é gerado, no ponto em que você pode executar uma ação naquela sugestão (neste caso, provavelmente navegar para outra página com mais detalhes sobre o conteúdo especificado). Observe que as propriedades **LinguisticDetails** e **Language** do **SearchBoxQuerySubmittedEventArgs** não são mais suportadas (há APIs equivalentes para dar suporte a essa funcionalidade). E para **KeyModifiers** não há mais suporte.

[**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) também tem suporte para IMEs (Input Method Editors). E, caso queira mostrar um ícone de "localizar", você pode fazer isso também (interagir com o ícone fará o evento **QuerySubmitted** ser gerado).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

Além disso, consulte [Exemplo de portabilidade AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox).

## <a name="semanticzoom-changes"></a>Alterações de SemanticZoom

O gesto de reduzir do [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) convergiu no modelo do Windows Phone, que é tocar ou clicar no cabeçalho de um grupo (portanto, em computadores desktop, a funcionalidade do botão de subtração para reduzir não é mais exibida). Agora temos o mesmo comportamento consistente gratuitamente em todos os dispositivos. Uma diferença estética do modelo do Windows Phone é que a exibição reduzida (a lista de atalhos) substitui a exibição ampliada em vez de se sobrepor a ela. Por isso, você pode remover qualquer tela de fundo semiopaca das exibições reduzidas.

Em um aplicativo da Windows Phone Store, a exibição com zoom se expande para o tamanho da tela. Em um aplicativo Windows Runtime 8. x e em um aplicativo do Windows 10, o tamanho do modo de exibição com zoom é restrito aos limites do controle **SemanticZoom** .

Em um aplicativo da Loja do Windows Phone, o conteúdo atrás do modo de exibição reduzida (em ordem z) aparece se a exibição reduzida tiver qualquer transparência no seu plano de fundo. Em um aplicativo Windows Runtime 8. x e em um aplicativo do Windows 10, nada é visível por trás da exibição com zoom.

Em um aplicativo Windows Runtime 8. x, quando o aplicativo é desativado e reativado, a exibição com zoom é ignorada (se ela estava sendo mostrada) e a exibição com zoom é mostrada em vez disso. Em um aplicativo da Windows Phone Store e em um aplicativo do Windows 10, a exibição com zoom continuará mostrando se ele estava sendo mostrado.

Em um aplicativo da Windows Phone Store e em um aplicativo do Windows 10, a exibição com zoom é ignorada quando o botão voltar é pressionado. Para um aplicativo Windows Runtime 8. x, não há nenhum processamento de botão voltar interno, portanto, a pergunta não se aplica.

## <a name="settings"></a>Configurações

A classe **SettingsPane** 8. x não é apropriada para Windows Runtime o Windows 10. Em vez disso, além de criar uma página de configurações, você deve oferecer aos usuários uma forma de acessá-la dentro do aplicativo. Recomendamos expor essa página de configurações do aplicativo no nível superior, como o último item fixo no painel de navegação, mas aqui está todo o conjunto de opções.

-   Painel de navegação. As configurações devem ser o último item na lista de opções de navegação e fixadas na parte inferior.
-   Appbar/barra de ferramentas (dentro de um modo de exibição de guias ou de um layout dinâmico). As configurações devem ser o último item no submenu de menu de appbar ou da barra de ferramentas. Não é recomendável que as configurações sejam um dos itens de nível superior dentro da navegação.
-   Hub. As configurações devem estar localizadas dentro do submenu de menu (pode ser no menu da barra de aplicativos ou no menu da barra de ferramentas dentro do layout do Hub).

Também não é recomendável esconder as configurações dentro de um painel de detalhes mestre.

A página de configurações deve preencher toda a janela do aplicativo, e a página de configurações também é onde Sobre e Comentários devem estar. Para obter uma diretriz sobre o design da página de configurações, consulte [Diretrizes para configurações de aplicativo](https://docs.microsoft.com/windows/uwp/app-settings/guidelines-for-app-settings).

## <a name="text"></a>Texto

Texto (ou tipografia) é um aspecto importante de um aplicativo UWP e, durante a portabilidade, convém revisitar os designs visuais dos modos de exibição de maneira que eles fiquem em harmonia com a nova linguagem de design. Use estas ilustrações para encontrar os estilos de sistema  **TextBlock** da UWP (Plataforma Universal do Windows). Encontre os que correspondem aos Windows Phone estilos do Silverlight que você usou. Como alternativa, você pode criar seus próprios estilos universais e copiar as propriedades dos estilos de sistema Windows Phone Silverlight para eles.

![estilos de sistema textblock para aplicativos do windows 10](images/label-uwp10stylegallery.png) <br/>Estilos TextBlock do sistema para aplicativos do Windows 10

Nos aplicativos Windows Runtime 8. x e Windows Phone aplicativos da loja, a família de fontes padrão é a interface do usuário global. Em um aplicativo do Windows 10, a família de fontes padrão é Segoe UI. Como resultado, as métricas de fonte em seu aplicativo podem parecer diferentes. Se quiser reproduzir a aparência de seu texto 8.1, você pode definir suas próprias métricas usando propriedades como [**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) e [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy).

Nos aplicativos Windows Runtime 8. x e Windows Phone aplicativos da loja, o idioma padrão do texto é definido como o idioma da compilação ou para en-US. Em um aplicativo do Windows 10, o idioma padrão é definido como o idioma superior do aplicativo (fallback de fonte). Você pode definir [**FrameworkElement.Language**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.language) explicitamente, mas desfrutará de um melhor comportamento de fallback se não definir um valor para essa propriedade.

Para obter mais informações, consulte [Diretrizes para fontes](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) e [Crie aplicativos UWP](https://developer.microsoft.com/). Além disso, consulte a seção [Controles](#controls-and-control-styles-and-templates) acima para saber as alterações feitas em controles de texto.

## <a name="theme-changes"></a>Alterações de tema

Para um aplicativo Universal 8.1, o tema padrão é escuro. Para dispositivos Windows 10, o tema padrão foi alterado, mas você pode controlar o tema usado pela declaração de um tema solicitado em app. XAML. Por exemplo, para usar um tema escuro em todos os dispositivos, adicione `RequestedTheme="Dark"` ao elemento Application raiz.

## <a name="tiles-and-toasts"></a>Blocos e notificações do sistema

Para blocos e notificações, os modelos que você está usando no momento continuarão a funcionar em seu aplicativo do Windows 10. Porém, existem novos modelos adaptáveis disponíveis para você usar, e eles estão descritos em [Notificações, blocos, notificações do sistema e selos](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications).

Anteriormente, em computadores desktop, uma notificação do sistema era uma mensagem transitória. Ela desaparecia, e não podia mais ser recuperada, depois de ser perdida ou ignorada. No Windows Phone, se uma notificação do sistema for ignorada ou temporariamente dispensada, ela irá para a Central de Ações. Agora, a Central de Ações não é mais limitada à família de dispositivos móveis.

Para enviar uma notificação do sistema, não há mais necessidade de declarar uma funcionalidade.

## <a name="window-size"></a>Tamanho da janela

Para um aplicativo Universal 8.1, o elemento de manifesto do aplicativo [**ApplicationView**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) é usado para declarar uma largura mínima de janela. Em seu aplicativo UWP, você pode especificar um tamanho mínimo (largura e altura) com código imperativo. O tamanho mínimo padrão é 500 x 320 epx, que também é o menor tamanho mínimo aceito. O maior tamanho mínimo aceito é 500 x 500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

O próximo tópico é [Portabilidade para E/S, dispositivo e modelo de aplicativo](w8x-to-uwp-input-and-sensors.md).

