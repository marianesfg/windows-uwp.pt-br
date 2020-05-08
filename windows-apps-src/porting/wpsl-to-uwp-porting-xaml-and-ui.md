---
description: A prática de definição da interface do usuário na forma de marcação XAML declarativa é convertida extremamente bem dos aplicativos Windows Phone Silverlight para aplicativos UWP (Plataforma Universal do Windows).
title: Fazendo a portabilidade de XAML e da interface do usuário do Windows Phone Silverlight para a UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9f6f7bddba5b6e457cb6ece4aa811f02ba3ebe8a
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730342"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>Fazendo a portabilidade de XAML e da interface do usuário do Windows Phone Silverlight para a UWP



O tópico anterior foi [Solução de problemas](wpsl-to-uwp-troubleshooting.md).

A prática de definição da interface do usuário na forma de marcação XAML declarativa é convertida extremamente bem dos aplicativos Windows Phone Silverlight para aplicativos UWP (Plataforma Universal do Windows). Você descobrirá que grandes seções da sua marcação serão compatíveis assim que tiver atualizado as referências de chave de recurso do sistema, alterado alguns nomes de tipo de elemento e alterado "clr-namespace" para "using". Muito do código imperativo na sua camada de apresentação (modelos de exibição e o código que manipula os elementos da interface do usuário) também serão simples de portar.

## <a name="a-first-look-at-the-xaml-markup"></a>Primeira análise da marcação XAML

O tópico anterior mostrou como copiar os arquivos XAML e code-behind para seu novo projeto do Visual Studio do Windows 10. Um dos primeiros problemas que você pode notar realçado no designer XAML do Visual Studio é que o elemento `PhoneApplicationPage` na raiz do arquivo XAML não é válido para um projeto da Plataforma Universal do Windows (UWP). No tópico anterior, você salvou uma cópia dos arquivos XAML gerados pelo Visual Studio quando ele criou o projeto do Windows 10. Se abrir essa versão de MainPage.xaml, você verá que há na raiz o tipo [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), que é o namespace [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls). Assim, você pode alterar todos os elementos `<phone:PhoneApplicationPage>` para `<Page>` (não se esqueça da sintaxe de elemento de propriedade) e excluir a declaração `xmlns:phone`.

Para uma abordagem mais geral para encontrar o tipo da UWP que corresponda a um tipo do Windows Phone Silverlight, consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md).

## <a name="xaml-namespace-prefix-declarations"></a>Declarações de prefixo de namespace XAML


Se você usar instâncias de tipos personalizados nos modos de exibição (talvez uma instância de modelo de exibição ou um conversor de valor) terá declarações de prefixo de namespace XAML em sua marcação XAML. A sintaxe deles difere entre o Windows Phone Silverlight e a UWP. Estes são alguns exemplos:

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

Altere "clr-namespace" para "using" e exclua todos os tokens de assembly e ponto-e-vírgulas (o assembly será deduzido). O resultado será semelhante a este:

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

Você pode ter um recurso cujo tipo seja definido pelo sistema:

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

Na UWP, omita a declaração do prefixo "System" e use o (já declarado) prefixo "x":

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>Código imperativo


Seus modelos de exibição são um lugar onde há código imperativo que referencia tipos de interface do usuário. Outro local é qualquer arquivo code-behind que manipule diretamente elementos da interface do usuário. Por exemplo, você poderia descobrir que uma linha de código como esta ainda não compila:


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** está no namespace **System.Windows.Media.Imaging** no Windows Phone Silverlight, e usar uma diretiva no mesmo arquivo permite que **BitmapImage** seja usado sem qualificação de namespace como no trecho acima. Em casos assim, é possível clicar com o botão direito do mouse no nome do tipo (**BitmapImage**) no Visual Studio e usar o comando **Resolver** no menu de contexto para adicionar uma nova diretiva de namespace ao arquivo. Nesse caso, o namespace [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) é adicionado, que é onde o tipo está na UWP. É possível remover a diretiva de uso de **System.Windows.Media.Imaging**, e bastará fazer isso para portar código como no trecho acima. Quando terminar, você terá removido todos os namespaces do Windows Phone Silverlight.

Em casos simples como este, onde você está mapeando os tipos em um namespace antigo para os mesmos tipos em um novo, é possível usar o comando **Localizar e Substituir** do Visual Studio para fazer alterações em massa em seu código-fonte. O comando **Resolver** é uma ótima maneira de descobrir o novo namespace do tipo. Como outro exemplo, você pode substituir todos os "System.Windows" por "Windows.UI.Xaml". Essencialmente, isso portará todas as diretivas using e todos os nomes de tipo totalmente qualificados que fazem referência a esse namespace.

Assim que todas as diretivas using antigas forem removidas e as novas adicionadas, você poderá usar o comando **Organizar Usos** do Visual Studio para classificar suas diretivas e remover as não utilizadas.

Às vezes, corrigir o código imperativo será tão secundário quanto alterar um tipo de parâmetro. Em outras ocasiões, você precisará usar APIs Windows Runtime em vez de APIs .NET para aplicativos Windows Runtime 8. x. Para identificar quais APIs têm suporte, use o restante deste guia de portação em combinação com o [.net para Windows Runtime visão geral dos aplicativos 8. x](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)) e a [referência de Windows Runtime](https://docs.microsoft.com/uwp/api/).

E, se você só quiser chegar ao estágio onde o seu projeto é compilado, poderá comentar ou apagar qualquer código não essencial. Em seguida, itere, um problema por vez, e consulte os seguintes tópicos desta seção (e o tópico anterior: [Solução de problemas](wpsl-to-uwp-troubleshooting.md)), até que todos os problemas de compilação e do tempo de execução sejam corrigidos e a portabilidade seja concluída.

## <a name="adaptiveresponsive-ui"></a>Interface do usuário responsiva/adaptável

Como seu aplicativo do Windows 10 pode potencialmente ser executado em uma ampla variedade de dispositivos (cada um com seu próprio tamanho de tela e resolução), você vai querer ir além das etapas mínimas para portar seu aplicativo e adaptar a interface do usuário para ter a melhor aparência nesses dispositivos. Você pode usar o recurso adaptável do Gerenciador de Estado Visual para detectar dinamicamente o tamanho da janela e mudar o layout em resposta, e um exemplo de como fazer isso é mostrado na seção [Interface do usuário adaptável](wpsl-to-uwp-case-study-bookstore2.md) no tópico do estudo de caso Bookstore2.

## <a name="alarms-and-reminders"></a>Alarmes e lembretes

O código que usa as classes **Alarm** ou **Reminder** deve ser portado para usar a classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para criar e registrar uma tarefa em segundo plano e exibir uma notificação do sistema no momento relevante. Consulte [Processamento em segundo plano](wpsl-to-uwp-business-and-data.md) e [Notificações do sistema](#toasts).

## <a name="animation"></a>Animação

Como uma alternativa preferencial para animações de quadro chave e animações de/para, a biblioteca de animação da UWP está disponível para aplicativos UWP. Essas animações foram projetadas e ajustadas para serem executadas sem problemas, para terem uma ótima aparência e para fazer seu aplicativo parecer integrado ao Windows da mesma forma que os aplicativos nativos. Consulte [Guia de início rápido: animando sua interface do usuário usando animações de biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10)).

Caso use animações de quadro chave ou animações de/para nos aplicativos UWP, talvez você queira compreender a distinção entre animações independentes e dependentes introduzidas pela nova plataforma. Consulte [Otimizar animações e mídia](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media). As animações executadas no thread da interface do usuário (as que animam propriedades de layout, por exemplo) são conhecidas como animações dependentes e, quando executadas na nova plataforma, não terão efeito, a menos que você faça uma destas duas coisas. É possível redirecioná-las para animar propriedades diferentes, como [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), o que as torna independentes. Ou é possível definir `EnableDependentAnimation="True"` no elemento de animação para confirmar a intenção de executar uma animação sem garantia de execução tranquila. Se usar o Blend for Visual Studio para criar novas animações, essa propriedade será definida para você onde for necessário.

## <a name="back-button-handling"></a>Manipulação do botão Voltar

Em um aplicativo do Windows 10, você pode usar uma única abordagem para manipular o botão Voltar, e ele funcionará em todos os dispositivos. Em dispositivos móveis, o botão é fornecido a você como um botão capacitivo no dispositivo ou como um botão no shell. Em um dispositivo desktop, você adiciona um botão para elementos visuais cromados do seu aplicativo sempre que a navegação regressiva for possível dentro do aplicativo, e isso aparece na barra de título para aplicativos de janela ou na barra de tarefas para o modo Tablet. O evento do botão Voltar é um conceito universal em todas as famílias de dispositivos, e os botões implementados no hardware ou no software acionam o mesmo evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested).

O exemplo a seguir funciona para todas as famílias de dispositivos e é bom para casos em que o mesmo processamento se aplica a todas as páginas, e você não precisa confirmar a navegação (por exemplo, para avisar sobre as alterações não salvas).

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
        // but it will have no effect. Such device families provide a back button UI for you.
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

## <a name="binding-and-compiled-bindings-with-xbind"></a>Associação e associações compiladas com {x:Bind}

O tópico de associação inclui:

-   Associando um elemento da interface do usuário a "dados" (ou seja, às propriedades e aos comandos de um modelo de exibição)
-   Associando um elemento da interface do usuário a outro elemento da interface do usuário
-   Escrevendo um modelo de exibição observável (ou seja, ele gera notificações quando um valor de propriedade é alterado e quando a disponibilidade de um comando é alterada)

Todos esses aspectos ainda têm amplo suporte, mas há diferenças de namespace. Por exemplo, **System.Windows.Data.Binding** é mapeado para [**Windows.UI.Xaml.Data.Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding), **System.ComponentModel.INotifyPropertyChanged** é mapeado para [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) e **System.Collections.Specialized.INotifyPropertyChanged** é mapeado para [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged).

As barras de aplicativos e os botões da barra de aplicativos Windows Phone Silverlight não podem ser associados assim como em um aplicativo UWP. Você pode ter código imperativo que construa sua barra de aplicativos e seus botões, que vincule-os às propriedades e às cadeias de caracteres localizadas e que manipule seus eventos. Nesse caso, agora você tem a opção de portar esse código imperativo substituindo-o pela marcação declarativa associada a propriedades e a comandos, e por referências a recurso estático, tornando seu aplicativo cada vez mais seguir e de manutenção mais fácil. Você pode usar o Visual Studio ou o Blend for Visual Studio para associar botões da barra de aplicativos UWP e criar estilos para eles, assim como qualquer outro elemento XAML. Observe que, em um aplicativo UWP, os nomes de tipo usados são [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) e [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton).

Atualmente, os recursos relacionados à associação de aplicativos UWP têm as seguintes limitações:

-   Não há suporte interno para validação de entrada de dados e para as interfaces [**IDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.idataerrorinfo) e [**INotifyDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifydataerrorinfo).
-   A classe [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) não inclui as propriedades de formatação estendidas disponíveis no Windows Phone Silverlight. Entretanto, você ainda pode implementar [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) para oferecer formatação personalizada.
-   Os métodos [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) obtêm as cadeias de caracteres de idiomas como parâmetros em vez de objetos [**CultureInfo**](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo).
-   A classe [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) não fornece suporte interno para classificação e filtragem, e o agrupamento funciona de modo diferente. Para obter mais informações, consulte [Associação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth) e [Exemplo de vinculação de dados](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

Embora os mesmos recursos de associação ainda tenham amplo suporte, o Windows 10 oferece a opção de um mecanismo de associação novo e com melhor desempenho chamado de associações compiladas, que usam a extensão de marcação {x:Bind}. Consulte [Vinculação de dados: impulsione o desempenho de seu aplicativo por meio de novas melhorias na vinculação de dados XAML](https://channel9.msdn.com/Events/Build/2015/3-635) e o [Exemplo x:Bind](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

## <a name="binding-an-image-to-a-view-model"></a>Associando uma imagem a um modelo de exibição

Você pode associar a propriedade [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) a qualquer propriedade de um modelo de exibição que seja do tipo [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource). Aqui está uma implementação típica dessa propriedade em um aplicativo Windows Phone Silverlight:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Em um aplicativo UWP, você usa o [esquema de URI](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)) ms-appx. Para que você possa manter o restante de seu código da mesma forma, use uma sobrecarga diferente do construtor **System.Uri** para colocar o esquema de URI ms-appx em um URI de base e acrescentar o resto do caminho nele. Dessa forma:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

Dessa forma, o restante do modelo de exibição, os valores de caminho na propriedade de caminho de imagem e as associações na marcação XAML poderão se manter exatamente iguais.

## <a name="controls-and-control-stylestemplates"></a>Controles e estilos/modelos de controles

Os aplicativos Windows Phone Silverlight usam controles definidos nos namespaces **Microsoft.Phone.Controls** e **System.Windows.Controls**. Aplicativos UWP XAML usam controles definidos no namespace [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls). A arquitetura e o design de controles XAML na UWP são praticamente os mesmos dos controles do Windows Phone Silverlight. Porém, algumas alterações foram feitas para melhorar o conjunto de controles disponíveis e unificá-los aos aplicativos do Windows. Veja exemplos específicos.

| Nome do controle | Alterar |
|--------------|--------|
| ApplicationBar | A propriedade [Page.TopAppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.topappbar). |
| ApplicationBarIconButton | O equivalente UWP é a propriedade [Glyph](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon.glyph). PrimaryCommands é a propriedade de conteúdo de CommandBar. O analisador XAML interpreta o xml interno de um elemento como o valor de sua propriedade de conteúdo. |
| ApplicationBarMenuItem | O equivalente UWP é a propriedade [AppBarButton.Label](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.label) definida para o texto do item de menu. |
| ContextMenu (no kit de ferramentas do Windows Phone) | Para um submenu de seleção única, use [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout). |
| Classe ControlTiltEffect.TiltEffect | As animações da biblioteca de animações UWP são inseridas nos estilos padrão dos controles comuns. Veja [Animando ações de ponteiro](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10)). |
| LongListSelector com dados agrupados | O LongListSelector do Windows Phone Silverlight funciona de duas maneiras, que podem ser usadas em conjunto. Primeiro, ele é capaz de exibir dados agrupados por uma chave, por exemplo, uma lista de nomes agrupados pela letra inicial. Segundo, é capaz de aplicar "zoom" entre duas exibições semânticas: a lista agrupada de itens (por exemplo, nomes) e uma lista somente com as próprias chaves de grupo (por exemplo, letras iniciais). Com a UWP, você pode exibir dados agrupados com as [Diretrizes de controles de exibição de grade e de lista](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists). |
| LongListSelector com dados simples | Por motivos de desempenho, no caso de listas muito longas, é recomendável LongListSelector em vez de uma caixa de listagem do Windows Phone Silverlight mesmo para dados simples não agrupados. Em um aplicativo UWP, [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) são preferíveis para listas longas de itens, sejam dados receptivos a agrupamento ou não. |
| Panorama | O Windows Phone controle de panorama do Silverlight mapeia as [diretrizes para controles de Hub em aplicativos Windows Runtime 8. x](https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub) e diretrizes para o controle Hub. <br/> Observe que um controle de Panorama encapsula da última seção à primeira, e sua imagem da tela de fundo se move na paralaxe em relação às seções. As seções [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) não encapsulam, e a paralaxe não é usada. |
| Dinâmico | O equivalente da UWP para o controle de Pivô do Windows Phone Silverlight é [Windows.UI.Xaml.Controls.Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot). Ele está disponível para todas as famílias de dispositivos. |

**Observe**    que o estado visual do PointerOver é relevante em estilos/modelos personalizados em aplicativos do Windows 10, mas não em Windows Phone aplicativos do Silverlight. Há outros motivos por que seus estilos/modelos personalizados existentes podem não ser adequados aos aplicativos do Windows 10, incluindo as chaves de recurso do sistema que você está usando, as alterações nos conjuntos de estados visuais usados e as melhorias de desempenho nos estilos/modelos padrão do Windows 10. Recomendamos que você edite uma cópia nova de um modelo padrão do controle para o Windows 10 e reaplique a personalização de estilo e modelo a ela.

Para saber mais sobre controles da UWP, consulte [Controles por função](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function), [Lista de controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/) e [Diretrizes para controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index).

##  <a name="design-language-in-windows10"></a>Linguagem de design no Windows 10

Existem algumas diferenças na linguagem de design entre aplicativos Windows Phone Silverlight e aplicativos do Windows 10. Para saber todos os detalhes, consulte [Design](https://developer.microsoft.com/windows/apps/design). Apesar das alterações na linguagem de design, nossos princípios de design permanecem consistentes: atenção aos detalhes, mas sempre buscando simplicidade por meio da concentração no conteúdo, e não no cromo, reduzindo drasticamente elementos visuais e permanecendo autêntico ao domínio digital; usar a hierarquia visual especialmente com tipografia; projetar em uma grade e dar vida às suas experiências com animações suaves.

## <a name="localization-and-globalization"></a>Localização e globalização

Para cadeias de caracteres localizadas, você pode reutilizar o arquivo .resx do projeto do Windows Phone Silverlight no projeto do aplicativo UWP. Copie o arquivo, adicione-o ao projeto e renomeie-o para Resources.resw de forma que o mecanismo de pesquisa o encontre por padrão. Defina **Ação de Compilação** como **PRIResource** e **Copiar para Diretório de Saída** como **Não copiar**. Você pode usar as cadeias de caracteres na marcação especificando o atributo **X:UID** em seus elementos XAML. Consulte [Guia de início rápido: usando recursos de cadeia de caracteres](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10)).

Os aplicativos Windows Phone Silverlight usam a classe **CultureInfo** para ajudar a globalizar um aplicativo. Os aplicativos UWP usam MRT (Modern Resource Technology), que permite o carregamento dinâmico de recursos do aplicativo (localização, escala e tema) no tempo de execução e na superfície de design do Visual Studio. Para saber mais, veja [Diretrizes para arquivos, dados e globalização](https://docs.microsoft.com/windows/uwp/design/usability/index).

O tópico [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) descreve como carregar recursos específicos à família de dispositivos com base no fator de seleção de recurso de família de dispositivos.

## <a name="media-and-graphics"></a>Mídia e elementos gráficos

Ao ler sobre elementos gráficos e mídia da UWP, tenha em mente que os princípios de design do Windows incentivam uma redução intensa de tudo que seja supérfluo, incluindo desordem e complexidade gráfica. O design do Windows é tipificado por elementos visuais, tipografia e movimento claros e limpos. Se o aplicativo seguir os mesmos princípios, ele parecerá mais como os aplicativos internos.

O Windows Phone Silverlight tem um tipo **RadialGradientBrush** que não está presente na UWP, embora outros tipos [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) estejam. Em alguns casos, você poderá obter um efeito semelhante com um bitmap. É possível [criar um pincel de gradiente radial](https://docs.microsoft.com/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush) com Direct2D em um [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) e em UWP XAML C++.

O Windows Phone Silverlight tem a propriedade **System.Windows.UIElement.OpacityMask**, mas essa propriedade não é um membro do tipo [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) da UWP. Em alguns casos, você poderá obter um efeito semelhante com um bitmap. E é possível [criar uma máscara de opacidade](https://docs.microsoft.com/windows/desktop/Direct2D/opacity-masks-overview) com Direct2D em um aplicativo UWP [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) e XAML C++. Porém, um caso de uso comum para **OpacityMask** é usar um único bitmap que se adapta a temas claros e escuros. Para gráficos vetoriais, é possível usar pincéis do sistema com reconhecimento de tema (como os gráficos de pizza ilustrados abaixo). Porém, para criar um bitmap com reconhecimento de tema (como as marcas de seleção ilustradas abaixo), isso requer uma abordagem diferente.

![um bitmap com reconhecimento de tema](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

Em um aplicativo do Windows Phone Silverlight, a técnica é usar uma máscara alfa (na forma de um bitmap) como o **OpacityMask** para um **Rectangle** preenchido com o pincel de primeiro plano:

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

A maneira mais simples de portar isso para um aplicativo UWP é usar um [**BitmapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon) da seguinte maneira:

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

Aqui, o\_check-in do winrt. png é uma máscara alfa na forma de um bitmap\_assim como wpsl check. png é, e ele poderia ser muito bem o mesmo arquivo. No entanto, talvez você queira fornecer vários tamanhos diferentes de\_check-in do winrt. png a ser usado para fatores de dimensionamento diferentes. Para saber mais sobre isso e para obter uma explicação sobre as alterações nos valores **Largura** e **Altura**, veja [Pixels de exibição ou efetivos, distância de exibição e fatores de escala](#view-or-effective-pixels-viewing-distance-and-scale-factors) neste tópico.

Uma abordagem mais geral, que é apropriada caso haja diferenças entre o formulário de temas claro e escuro de um bitmap, é usar dois ativos de imagem - um com um primeiro plano escuro (para tema claro) e um com um primeiro plano claro (para tema escuro). Para obter mais detalhes sobre como nomear esse conjunto de ativos de bitmap, consulte [personalizar seus recursos para o idioma, a escala e outros qualificadores](../app-resources/tailor-resources-lang-scale-contrast.md). Depois que um conjunto de arquivos de imagem for nomeado corretamente, você poderá se referir a eles no resumo usando o nome raiz deles, desta forma:

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

No Windows Phone Silverlight, a propriedade **UIElement.Clip** pode ser qualquer forma que você possa expressar com uma **Geometry**, e normalmente é serializada na marcação XAML na minilinguagem **StreamGeometry**. Na UWP, o tipo da propriedade [**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) é [**RectangleGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry), portanto, você só pode recortar uma região retangular. Permitir que um retângulo seja definido usando minilinguagem seria muito permissivo. Assim, para fazer a portabilidade de uma região de recorte na marcação, substitua a sintaxe do atributo **Clip** e transformá-la em uma sintaxe de elemento de propriedade semelhante à seguinte:

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

Observe que você pode [usar geometria arbitrária como uma máscara em uma camada](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-layers-overview) com Direct2D em um aplicativo UWP em [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) e em XAML C++.

## <a name="navigation"></a>Navegação

Ao navegar até uma página em um aplicativo do Windows Phone Silverlight, você usa um esquema de endereçamento de URI (Uniform Resource Identifier):

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

Em um aplicativo UWP, você chama o método [**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) e especifica o tipo da página de destino (como definido pelo atributo **x:Class** da definição de marcação XAML da página):


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

Defina a página de inicialização para um aplicativo do Windows Phone Silverlight em WMAppManifest. xml:

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

Em um aplicativo UWP, você pode usar código imperativo para definir a página de inicialização. Aqui está um código em App.xaml.cs que ilustra como:

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

O mapeamento de URI e a navegação de fragmento são técnicas de navegação de URI e, portanto, não são aplicáveis à navegação da UWP, que não se baseia em URIs. O mapeamento de URI existe em resposta à natureza fracamente tipada de identificação de uma página de destino com uma cadeia de caracteres de URI, o que leva a problemas de fragilidade e de capacidade de manutenção caso a página seja movida para uma pasta diferente e, consequentemente, para um caminho relativo diferente. Os aplicativos UWP usam navegação baseada em tipo, que é fortemente tipada e verificada por compilador e não tem o problema que é resolvido pelo mapeamento de URI. O caso de uso para a navegação de fragmento é passar algum contexto para a página de destino de forma que a página possa fazer com que um fragmento específico do seu conteúdo seja rolado para exibição ou, caso contrário, exibido. O mesmo objetivo pode ser obtido passando um parâmetro de navegação quando você chama o método [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate).

Para obter mais informações, consulte [Navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics).

## <a name="resource-key-reference"></a>Referência de chave de recurso

A linguagem de design evoluiu para o Windows 10 e, consequentemente, determinados estilos de sistema foram alterados, e muitas chaves de recurso do sistema foram removidas ou renomeadas. O editor de marcação XAML no Visual Studio realça referências a chaves de recurso que não podem ser resolvidas. Por exemplo, o editor de marcação XAML sublinhará uma referência à chave de estilo `PhoneTextNormalStyle` com uma linha ondulada vermelha. Se isso não for corrigido, o aplicativo será encerrado imediatamente quando você tentar implantá-lo no emulador ou no dispositivo. Portanto, é importante estar atento à correção da marcação XAML. E você descobrirá que o Visual Studio é uma ótima ferramenta para identificar esses problemas.

Além disso, consulte [Texto](#text) abaixo.

## <a name="status-bar-system-tray"></a>Barra de status (bandeja do sistema)

A bandeja do sistema (definida na marcação XAML com `shell:SystemTray.IsVisible`) agora é chamada de barra de status e é mostrada por padrão. Você pode controlar sua visibilidade no código imperativo chamando os métodos [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.showasync) e [**HideAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.hideasync).

## <a name="text"></a>Texto

Texto (ou tipografia) é um aspecto importante de um aplicativo UWP e, durante a portabilidade, convém revisitar os designs visuais dos modos de exibição de maneira que eles fiquem em harmonia com a nova linguagem de design. Use estas ilustrações para encontrar os estilos de sistema **TextBlock** da UWP que estão disponíveis. Encontre aqueles que correspondam aos estilos do Windows Phone Silverlight que você usou. Como alternativa, você pode criar seus próprios estilos universais e copiar as propriedades dos estilos de sistema do Windows Phone Silverlight para eles.

![estilos de sistema textblock para aplicativos do windows 10](images/label-uwp10stylegallery.png)

Estilos de sistema TextBlock para aplicativos do Windows 10

Em um aplicativo Windows Phone Silverlight, a família de fontes padrão é Segoe WP. Em um aplicativo do Windows 10, a família de fontes padrão é a Segoe UI. Como resultado, as métricas de fonte em seu aplicativo podem parecer diferentes. Se deseja reproduzir a aparência do seu texto do Windows Phone Silverlight, você pode definir suas próprias métricas usando propriedades como [**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) e [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy). Para obter mais informações, consulte [Diretrizes para fontes](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) e [Crie aplicativos UWP](https://developer.microsoft.com/windows/apps/design).

## <a name="theme-changes"></a>Alterações de tema

Para um aplicativo Windows Phone Silverlight, o tema padrão é escuro. Para dispositivos Windows 10, o tema padrão mudou, mas você pode controlar o tema usado declarando um tema solicitado em App.xaml. Por exemplo, para usar um tema escuro em todos os dispositivos, adicione `RequestedTheme="Dark"` ao elemento Application raiz.

## <a name="tiles"></a>Tiles

Os blocos dos aplicativos UWP têm comportamentos semelhantes aos Blocos Dinâmicos dos aplicativos Windows Phone Silverlight, embora haja algumas diferenças. Por exemplo, o código que chama o método **Microsoft.Phone.Shell.ShellTile.Create** para criar blocos secundários deve ser portado para chamar [**SecondaryTile.RequestCreateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync). Aqui está um exemplo de antes e depois, primeiro a versão do Windows Phone Silverlight:


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

E o equivalente da UWP:

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

O código que atualiza um bloco com o método **Microsoft.Phone.Shell.ShellTile.Update**, ou a classe **Microsoft.Phone.Shell.ShellTileSchedule**, deve ser portado para usar as classes [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager), [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater), [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) e/ou as classes [**ScheduledTileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification).

Para saber mais sobre blocos, notificações do sistema, selos, faixas e notificações, consulte [Criando blocos](https://docs.microsoft.com/previous-versions/windows/apps/hh868260(v=win.10)) e [Trabalhando com blocos, selos e notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)). Para obter informações específicas sobre tamanhos de ativos visuais usados para Blocos da UWP, consulte [Ativos visuais de bloco e de notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh781198(v=win.10)).

## <a name="toasts"></a>Notificações do sistema

O código que exibe uma notificação do sistema com a classe **Microsoft.Phone.Shell.ShellToast** deve ser portado para usar as classes [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager), [**ToastNotifier**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier), [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) e/ou as classes [**ScheduledToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification). Observe que, em dispositivos móveis, o termo voltado para o consumidor para "notificação do sistema" é "faixa".

Consulte [Trabalhando com blocos, selos e notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>Pixels de exibição ou efetivos, distância de exibição e fatores de escala

Aplicativos Windows Phone Silverlight e aplicativos do Windows 10 são diferentes na forma como abstraem o tamanho e o layout de elementos da interface do usuário do tamanho físico e da resolução reais de dispositivos. Um aplicativo Windows Phone Silverlight usa pixels de exibição para fazer isso. Com o Windows 10, o conceito de pixels de exibição foi refinado para o conceito de pixels efetivos. Aqui está uma explicação sobre esse termo, seu significado, e o valor extra que ele oferece.

O termo "resolução" refere-se a uma medida da densidade de pixels e não, como comumente se pensa, uma contagem de pixels. "Resolução efetiva" é a maneira como os pixels físicos que compõem uma imagem ou um glifo são resolvidos para o olho dadas as diferenças na distância de exibição e no tamanho físico de pixels do dispositivo (a densidade de pixels sendo a recíproca do tamanho físico de pixels). A resolução efetiva é uma boa métrica para criar uma experiência porque é centrada no usuário. Ao compreender todos os fatores e controlar o tamanho dos elementos da interface do usuário, você poderá melhorar a experiência do usuário.

Para um aplicativo Windows Phone Silverlight, todas as telas de telefone têm exatamente 480 pixels de exibição de largura, sem exceção, independentemente de quantos pixels físicos a tela tenha ou de qual seja sua densidade de pixels ou seu tamanho físico. Isso significa que um elemento **Image** com `Width="48"` será exatamente um décimo da largura da tela de qualquer telefone que possa executar o aplicativo Windows Phone Silverlight.

Para um aplicativo do Windows 10, *não* se aplica o caso em que todos os dispositivos têm um número fixo de pixels efetivos de largura. Isso é provavelmente óbvio, devido à ampla variedade de dispositivos em que um aplicativo UWP pode ser executado. Os diferentes dispositivos têm um número diferente de pixels efetivos de largura, desde 320 epx para os dispositivos menores até 1024 epx para um monitor de tamanho moderado, e muito além disso para larguras muito maiores. Você só precisa continuar a usar elementos de dimensionamento automático e painéis de layout dinâmicos como você sempre fez. Também haverá alguns casos em que você definirá as propriedades dos elementos da interface do usuário para um tamanho fixo na marcação XAML. Um fator de escala é aplicado automaticamente ao aplicativo dependendo do dispositivo em que é executado e das configurações de exibição feitas pelo usuário. E esse fator de escala serve para que qualquer elemento da interface do usuário com um tamanho fixo continue apresentando um alvo de toque (e leitura) de tamanho mais ou menos constante para o usuário em diversos tamanhos de tela. E, com o layout dinâmico, a interface do usuário não será simplesmente dimensionada opticamente em dispositivos diferentes. Em vez disso, ela fará o que for necessário para ajustar a quantidade apropriada de conteúdo ao espaço disponível.

Uma vez que anteriormente a largura fixa em pixels de exibição de uma tela do tamanho de um telefone era 480, e esse valor agora é normalmente menor em pixels efetivos, uma regra geral é multiplicar qualquer dimensão na marcação do aplicativo Windows Phone Silverlight por um fator de 0,8.

Para que seu aplicativo tenha a melhor experiência em todas as telas, recomendamos que você crie cada ativo de bitmap em um intervalo de tamanhos, cada um adequado a um fator de escala específico. O fornecimento de ativos em escala de 100%, 200% e 400% (nessa ordem de prioridade) trará excelentes resultados na maioria dos casos em todos os fatores de escala intermediários.

**Observação**  se, por qualquer motivo, você não puder criar ativos em mais de um tamanho, crie ativos de 100% de escala. No Microsoft Visual Studio, o modelo de projeto padrão para aplicativos UWP fornece ativos de identidade visual (logotipos e imagens do bloco) em um único tamanho, mas eles não estão na escala de 100%. Quando criar ativos para seu próprio aplicativo, siga as diretrizes nesta seção e forneça tamanhos de 100%, 200% e 400%, além de usar pacotes de ativos.

Caso você tenha uma arte complexa, convém fornecer os ativos em ainda mais tamanhos. Caso você seja um principiante na arte vetorial, é relativamente fácil gerar ativos de alta qualidade em qualquer fator de escala.

Não é recomendável que você tente dar suporte a todos os fatores de escala, mas a lista completa de fatores de escala para aplicativos do Windows 10 é 100%, 125%, 150%, 200%, 250%, 300% e 400%. Se você o fizer, a Loja selecionará os ativos de tamanho correto para cada dispositivo, e somente esses ativos serão baixados. A Loja seleciona os ativos a serem baixados com base no DPI do dispositivo.

Para obter mais informações, consulte [Design responsivo 101 para aplicativos UWP](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="window-size"></a>Tamanho da janela

Em seu aplicativo UWP, você pode especificar um tamanho mínimo (largura e altura) com código imperativo. O tamanho mínimo padrão é 500 x 320 epx, que também é o menor tamanho mínimo aceito. O maior tamanho mínimo aceito é 500 x 500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

O próximo tópico é [Portabilidade para E/S, dispositivo e modelo de aplicativo](wpsl-to-uwp-input-and-sensors.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md)
