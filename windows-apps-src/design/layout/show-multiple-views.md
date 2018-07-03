---
author: Jwmsft
Description: View multiple parts of your app in separate windows.
title: Mostrar vários modos de exibição de um aplicativo
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5bda75f71b1230974ecb4d136e5b40e85b7e5259
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843406"
---
# <a name="show-multiple-views-for-an-app"></a>Mostrar vários modos de exibição de um aplicativo

![Estrutura delineada mostrando um aplicativo com várias janelas](images/multi-view.gif)

Ajude os usuários a serem mais produtivos permitindo que eles exibam partes independentes do aplicativo em janelas separadas. Quando você cria várias janelas para um aplicativo, cada janela se comporta de maneira independente. A barra de tarefas mostra cada janela separadamente. Os usuários podem mover, redimensionar, mostrar e ocultar janelas do aplicativo de maneira independente e alternar janelas do aplicativo como se elas fossem aplicativos separados. Cada janela funciona no próprio thread.

> **APIs importantes**: [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094), [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

## <a name="when-should-an-app-use-multiple-views"></a>Quando um aplicativo deve usar vários modos de exibição?
Há vários cenários que podem se beneficiar com vários modos de exibição. Veja aqui alguns exemplos:
 - Um aplicativo de email que permite aos usuários exibir uma lista de mensagens recebidas ao redigir um novo email
 - Um aplicativo de catálogo de endereços que permite aos usuários comparar as informações de contato de várias pessoas lado a lado
 - Um player de música que permite aos usuários ver o que está sendo reproduzido durante a navegação em uma lista de outras músicas disponíveis
 - Um aplicativo de anotações que permite aos usuários copiar informações de uma página de anotações para outra
 - Um app de leitura que permite aos usuários abrir vários artigos para leitura posterior, após a oportunidade de ver todos os títulos principais

Para criar instâncias separadas do app, consulte [Criar um aplicativo UWP de várias instâncias](../../launch-resume/multi-instance-uwp.md).

## <a name="what-is-a-view"></a>O que é um modo de exibição?

Modo de exibição do aplicativo é o emparelhamento 1:1 de um thread e uma janela que o aplicativo usa para exibir conteúdo. Ele é representado por um objeto [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017).

Os modos de exibição são gerenciados pelo objeto [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). Você chama [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) para criar um objeto[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). O **CoreApplicationView** reúne um [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) e um [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) (armazenados nas propriedades [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) e [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264)). É possível pensar no **CoreApplicationView** como o objeto que o Windows Runtime usa para interagir com o sistema básico do Windows.

Você normalmente não trabalha diretamente com o [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). Em vez disso, o Windows Runtime fornece a classe [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658) no namespace [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295). Essa classe fornece propriedades, métodos e eventos que você usa quando o aplicativo interage com o sistema de janelas. Para trabalhar com um **ApplicationView**, chame o método [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) estático, que obtém uma instância **ApplicationView** vinculada ao thread atual de **CoreApplicationView**.

Da mesma forma, a estrutura XAML encapsula o objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) em um objeto [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041). Em um aplicativo XAML, você normalmente interage com o objeto **Window**, em vez de trabalhar diretamente com o **CoreWindow**.

## <a name="show-a-new-view"></a>Mostrar um novo modo de exibição

Embora cada layout de aplicativo seja exclusivo, é recomendável incluir um botão de "nova janela" em um local previsível, como o canto superior direito do conteúdo que pode ser aberto em uma nova janela. Considere também incluir uma opção de menu de contexto em "Abrir em uma nova janela".

Vamos examinar as etapas de criação de um novo modo de exibição. Aqui, o novo modo de exibição é iniciado em resposta a um clique de botão.

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

**Para mostrar um novo modo de exibição**

1.  Chame [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) para criar uma nova janela e um thread para o conteúdo do modo de exibição.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Acompanhe a [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) do novo modo de exibição. Você pode usá-la para mostrar o modo de exibição mais tarde.

    Convém considerar a criação de uma infraestrutura no aplicativo para ajudar no controle dos modos de exibição criados. Consulte a classe `ViewLifetimeControl` na [Amostra MultipleViews](http://go.microsoft.com/fwlink/p/?LinkId=620574) para obter um exemplo.

    ```csharp
    int newViewId = 0;
    ```

3.  No novo thread, preencha a janela.

    Você usa o método [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para agendar o trabalho no thread da interface do usuário para o novo modo de exibição. Você usa uma [expressão lambda](http://go.microsoft.com/fwlink/p/?LinkId=389615) para passar uma função como um argumento para o método **RunAsync**. O trabalho feito na função lambda acontece no thread do novo modo de exibição.

    Em XAML, você normalmente adiciona um [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) à propriedade [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) de [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) e passa o **Frame** para um XAML [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) em que você definiu o conteúdo do aplicativo. Para obter mais informações, consulte [Navegação ponto a ponto entre duas páginas](../basics/navigate-between-two-pages.md).

    Depois que o novo [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) for preenchido, você deverá chamar o método [**Activate**](https://msdn.microsoft.com/library/windows/apps/br209046) de **Window** para mostrar o **Window** mais tarde. Esse trabalho acontece no thread do novo modo de exibição, logo, o novo **Window** é ativado.

    Por fim, obtenha a [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) do novo modo de exibição que você usa para mostrar o modo de exibição mais tarde. Mais uma vez, esse trabalho é feito no thread do novo modo de exibição, logo, [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) obtém a **Id** do novo modo de exibição.

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

4.  Mostre o novo modo de exibição chamando [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101).

    Depois de criar um novo modo de exibição, você poderá mostrá-lo em uma nova janela chamando o método [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101). O parâmetro *viewId* desse método é um inteiro que identifica com exclusividade cada um dos modos de exibição no aplicativo. Você recupera a [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) do modo de exibição usando a propriedade **ApplicationView.Id** ou o método [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109).

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>O modo de exibição principal


O primeiro modo de exibição criado quando o aplicativo é iniciado é chamado de *modo de exibição principal*. Esse modo de exibição é armazenado na propriedade [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465), e a propriedade [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) é verdadeira. Você não cria esse modo de exibição; ele é criado pelo aplicativo. O thread do modo de exibição principal funciona como o gerenciador para o aplicativo, e todos os eventos de ativação do aplicativo são fornecidos nesse thread.

Caso os modos de exibição secundários estejam abertos, a janela do modo de exibição principal pode permanecer oculta – por exemplo, clicando-se no botão de fechamento (x) na barra de título da janela –, mas o thread permanece ativo. Chamar [**Close**](https://msdn.microsoft.com/library/windows/apps/br209049) no [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) do modo de exibição principal causa a ocorrência de **InvalidOperationException**. (Use [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327) para fechar seu aplicativo.) Se o thread do modo de exibição principal for encerrado, o aplicativo será fechado.

## <a name="secondary-views"></a>Modos de exibição secundários


Outros modos de exibição, inclusive todos os modos de exibição que você cria chamando [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) no código do aplicativo, são modos de exibição secundários. Os modos de exibição principal e secundário são armazenados na coleção [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861). Normalmente, você cria modos de exibição secundários em resposta à ação de um usuário. Em alguns casos, o sistema cria modos de exibição secundários para o aplicativo.

> [!NOTE]
> Você pode usar o recurso *acesso atribuído* do Windows para executar um aplicativo em [modo de quiosque](https://technet.microsoft.com/library/mt219050.aspx). Quando você faz isso, o sistema cria um modo de exibição secundário para apresentar a interface do usuário do aplicativo acima da tela de bloqueio. Os modos de exibição secundários criados pelo aplicativo não são permitidos, logo, caso você tente mostrar o próprio modo de exibição secundário no modo de quiosque, uma exceção é lançada.

## <a name="switch-from-one-view-to-another"></a>Alternar de um modo de exibição para outro

É recomendável oferecer uma maneira para o usuário navegar de uma janela secundária para sua janela pai. Para fazer isso, use o método [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097). Você chama esse método no thread da janela da qual está alternando e passa a ID do modo de exibição da janela para a qual alternando.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Ao usar [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097), você pode escolher se deseja fechar a janela inicial e removê-la da barra de tarefas especificando o valor de [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

* Forneça um ponto de entrada claro para o modo de exibição secundário utilizando o glifo "abrir nova janela".
* Comunique aos usuários a finalidade do modo de exibição secundário.
* Assegure que o aplicativo está totalmente funcional em um único modo de exibição e que os usuários abrirão um modo de exibição secundário apenas por conveniência.
* Não conte com o modo de exibição secundário para fornecer notificações ou outros elementos visuais transitórios.

## <a name="related-topics"></a>Tópicos relacionados

* [ApplicationViewSwitcher](https://msdn.microsoft.com/library/windows/apps/dn281094)
* [CreateNewView](https://msdn.microsoft.com/library/windows/apps/dn297278)
 
