---
Description: Exiba várias partes do seu aplicativo em janelas separadas.
title: Mostrar vários modos de exibição para um aplicativo
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 275dc6ab7cdb310dff817a3e0017568ad2fed80c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317119"
---
# <a name="show-multiple-views-for-an-app"></a>Mostrar vários modos de exibição para um aplicativo

![Estrutura delineada mostrando um aplicativo com várias janelas](images/multi-view.gif)

Ajude os usuários a serem mais produtivos permitindo que eles exibam partes independentes do aplicativo em janelas separadas. Quando você cria várias janelas para um aplicativo, cada janela se comporta de maneira independente. A barra de tarefas mostra cada janela separadamente. Os usuários podem mover, redimensionar, mostrar e ocultar janelas do aplicativo de maneira independente e alternar janelas do aplicativo como se elas fossem aplicativos separados. Cada janela funciona no próprio thread.

> **APIs importantes**: [**ApplicationViewSwitcher**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher), [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="when-should-an-app-use-multiple-views"></a>Quando um aplicativo deve usar vários modos de exibição?
Há vários cenários que podem se beneficiar com vários modos de exibição. Veja aqui alguns exemplos:
 - Um aplicativo de email que permite aos usuários exibir uma lista de mensagens recebidas ao redigir um novo email
 - Um aplicativo de catálogo de endereços que permite aos usuários comparar as informações de contato de várias pessoas lado a lado
 - Um player de música que permite aos usuários ver o que está sendo reproduzido durante a navegação em uma lista de outras músicas disponíveis
 - Um aplicativo de anotações que permite aos usuários copiar informações de uma página de anotações para outra
 - Um app de leitura que permite aos usuários abrir vários artigos para leitura posterior, após a oportunidade de ver todos os títulos principais

Para criar instâncias separadas do app, consulte [Criar um aplicativo UWP de várias instâncias](../../launch-resume/multi-instance-uwp.md).

## <a name="what-is-a-view"></a>O que é um modo de exibição?

Modo de exibição do aplicativo é o emparelhamento 1:1 de um thread e uma janela que o aplicativo usa para exibir conteúdo. Ele é representado por um objeto [**Windows.ApplicationModel.Core.CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView).

Os modos de exibição são gerenciados pelo objeto [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication). Você chama [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) para criar um objeto[**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). O **CoreApplicationView** reúne um [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) e um [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) (armazenados nas propriedades [**CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) e [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher)). É possível pensar no **CoreApplicationView** como o objeto que o Windows Runtime usa para interagir com o sistema básico do Windows.

Você normalmente não trabalha diretamente com o [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). Em vez disso, o Windows Runtime fornece a classe [**ApplicationView**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView) no namespace [**Windows.UI.ViewManagement**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement). Essa classe fornece propriedades, métodos e eventos que você usa quando o aplicativo interage com o sistema de janelas. Para trabalhar com um **ApplicationView**, chame o método [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) estático, que obtém uma instância **ApplicationView** vinculada ao thread atual de **CoreApplicationView**.

Da mesma forma, a estrutura XAML encapsula o objeto [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) em um objeto [**Windows.UI.XAML.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window). Em um aplicativo XAML, você normalmente interage com o objeto **Window**, em vez de trabalhar diretamente com o **CoreWindow**.

## <a name="show-a-new-view"></a>Mostrar um novo modo de exibição

Embora cada layout de aplicativo seja exclusivo, é recomendável incluir um botão de "nova janela" em um local previsível, como o canto superior direito do conteúdo que pode ser aberto em uma nova janela. Considere também incluir uma opção de menu de contexto em "Abrir em uma nova janela".

Vamos examinar as etapas de criação de um novo modo de exibição. Aqui, o novo modo de exibição é iniciado em resposta a um clique do botão.

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**Para mostrar uma nova exibição**

1.  Chame [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) para criar uma nova janela e um thread para o conteúdo do modo de exibição.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Acompanhe a [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) do novo modo de exibição. Você pode usá-la para mostrar o modo de exibição mais tarde.

    Convém considerar a criação de uma infraestrutura no aplicativo para ajudar no controle dos modos de exibição criados. Consulte a classe `ViewLifetimeControl` na [Amostra MultipleViews](https://go.microsoft.com/fwlink/p/?LinkId=620574) para obter um exemplo.

    ```csharp
    int newViewId = 0;
    ```

3.  No novo thread, preencha a janela.

    Você usa o método [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para agendar o trabalho no thread da interface do usuário para o novo modo de exibição. Você usa uma [expressão lambda](https://go.microsoft.com/fwlink/p/?LinkId=389615) para passar uma função como um argumento para o método **RunAsync**. O trabalho feito na função lambda acontece no thread do novo modo de exibição.

    Em XAML, você normalmente adiciona um [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) à propriedade [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) de [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) e passa o **Frame** para um XAML [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) em que você definiu o conteúdo do aplicativo. Para obter mais informações, consulte [Navegação ponto a ponto entre duas páginas](../basics/navigate-between-two-pages.md).

    Depois que o novo [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) for preenchido, você deverá chamar o método [**Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate) de **Window** para mostrar o **Window** mais tarde. Esse trabalho acontece no thread do novo modo de exibição, logo, o novo **Window** é ativado.

    Por fim, obtenha a [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) do novo modo de exibição que você usa para mostrar o modo de exibição mais tarde. Mais uma vez, esse trabalho é feito no thread do novo modo de exibição, logo, [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) obtém a **Id** do novo modo de exibição.

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  Mostre o novo modo de exibição chamando [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync).

    Depois de criar um novo modo de exibição, você poderá mostrá-lo em uma nova janela chamando o método [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync). O parâmetro *viewId* desse método é um inteiro que identifica com exclusividade cada um dos modos de exibição no aplicativo. Você recupera a [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) do modo de exibição usando a propriedade **ApplicationView.Id** ou o método [**ApplicationView.GetApplicationViewIdForWindow**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow).

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>O modo de exibição principal


O primeiro modo de exibição criado quando o aplicativo é iniciado é chamado de *modo de exibição principal*. Esse modo de exibição é armazenado na propriedade [**CoreApplication.MainView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.mainview), e a propriedade [**IsMain**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) é verdadeira. Você não cria esse modo de exibição; ele é criado pelo aplicativo. O thread do modo de exibição principal funciona como o gerenciador para o aplicativo, e todos os eventos de ativação do aplicativo são fornecidos nesse thread.

Caso os modos de exibição secundários estejam abertos, a janela do modo de exibição principal pode permanecer oculta – por exemplo, clicando-se no botão de fechamento (x) na barra de título da janela –, mas o thread permanece ativo. Chamar [**Close**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.close) no [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) do modo de exibição principal causa a ocorrência de **InvalidOperationException**. (Use [ **exit** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.exit) para fechar o aplicativo.) Se o thread principal do modo de exibição for encerrada, o aplicativo é fechado.

## <a name="secondary-views"></a>Modos de exibição secundários


Outros modos de exibição, inclusive todos os modos de exibição que você cria chamando [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) no código do aplicativo, são modos de exibição secundários. Os modos de exibição principal e secundário são armazenados na coleção [**CoreApplication.Views**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.views). Normalmente, você cria modos de exibição secundários em resposta à ação de um usuário. Em alguns casos, o sistema cria modos de exibição secundários para o aplicativo.

> [!NOTE]
> Você pode usar o recurso *acesso atribuído* do Windows para executar um aplicativo em [modo de quiosque](https://docs.microsoft.com/windows/manage/set-up-a-device-for-anyone-to-use). Quando você faz isso, o sistema cria um modo de exibição secundário para apresentar a interface do usuário do aplicativo acima da tela de bloqueio. Os modos de exibição secundários criados pelo aplicativo não são permitidos, logo, caso você tente mostrar o próprio modo de exibição secundário no modo de quiosque, uma exceção é lançada.

## <a name="switch-from-one-view-to-another"></a>Alternar de um modo de exibição para outro

É recomendável oferecer uma maneira para o usuário navegar de uma janela secundária para sua janela pai. Para fazer isso, use o método [**ApplicationViewSwitcher.SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync). Você chama esse método no thread da janela da qual está alternando e passa a ID do modo de exibição da janela para a qual alternando.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Ao usar [**SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync), você pode escolher se deseja fechar a janela inicial e removê-la da barra de tarefas especificando o valor de [**ApplicationViewSwitchingOptions**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

* Forneça um ponto de entrada claro para o modo de exibição secundário utilizando o glifo "abrir nova janela".
* Comunique aos usuários a finalidade do modo de exibição secundário.
* Assegure que o aplicativo está totalmente funcional em um único modo de exibição e que os usuários abrirão um modo de exibição secundário apenas por conveniência.
* Não conte com o modo de exibição secundário para fornecer notificações ou outros elementos visuais transitórios.

## <a name="related-topics"></a>Tópicos relacionados

* [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
* [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
 
