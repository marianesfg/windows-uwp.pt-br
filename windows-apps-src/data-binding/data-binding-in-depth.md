---
author: stevewhims
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Vinculação de dados em detalhes
description: A vinculação de dados é uma maneira de a interface do usuário do seu aplicativo exibir dados e, opcionalmente, ficar em sincronia com esses dados.
ms.author: stwhi
ms.date: 10/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 906fb2d0d5d466f4fd691afd35ed96198929225c
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4432459"
---
# <a name="data-binding-in-depth"></a>Vinculação de dados em detalhes

**APIs Importantes**

-   [**Extensão de marcação {x:Bind}**](../xaml-platform/x-bind-markup-extension.md)
-   [**Classe Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

> [!NOTE]
> Este tópico descreve detalhadamente os recursos de vinculação de dados. Para obter uma introdução breve e prática, consulte [Visão geral da vinculação de dados](data-binding-quickstart.md).

A vinculação de dados é uma maneira de a interface do usuário do seu aplicativo exibir dados e, opcionalmente, ficar em sincronia com esses dados. A vinculação de dados permite separar a preocupação dos dados da preocupação da interface do usuário, e isso resulta em um modelo conceitual mais simples, bem como melhor legibilidade, capacidade de teste e capacidade de manutenção do seu aplicativo.

Você pode usar a vinculação de dados simplesmente para exibir os valores de uma fonte de dados quando a interface do usuário é mostrada primeiro, mas não para responder a alterações nesses valores. Este é um modo de associação chamada *única*, e funciona bem para um valor que não muda durante o tempo de execução. Como alternativa, você pode optar por "observar" os valores e atualizar a interface do usuário quando mudam. Isso mais é chamado *unidirecional*, e funciona bem para dados somente leitura. Por fim, você pode optar por observar e atualizar, para que as alterações que o usuário faz aos valores da interface do usuário sejam automaticamente enviadas de volta para a fonte de dados. Esse modo é chamado *bidirecional*, e funciona bem para dados de leitura / gravação. Aqui estão alguns exemplos.

-   Você pode usar o modo unidirecional para associar uma [**imagem**](https://msdn.microsoft.com/library/windows/apps/BR242752) a foto do usuário atual.
-   Você pode usar o modo unidirecional para associar um [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) a uma coleção de artigos de notícias em tempo real agrupados pela seção de jornal.
-   Você pode usar o modo bidirecional para associar um [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) ao nome do cliente em um formulário.

Independentemente do modo, existem dois tipos de associação, e ambos normalmente são declarados na marcação da interface do usuário. Você pode optar por usar a [extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou a [extensão de marcação {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). E você ainda pode usar uma combinação das duas no mesmo aplicativo, inclusive no mesmo elemento de interface do usuário. A {x:Bind} é nova para Windows 10 e tem um desempenho melhor. Todos os detalhes descritos neste tópico se aplicam a ambos os tipos de associação, a menos que explicitamente declarado o contrário.

**Aplicativos de amostra que demonstram {x:Bind}**

-   [Amostra de {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989).
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Amostra de noções básicas de interface do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=619992)

**Aplicativos de amostra que demonstram {Binding}**

-   Baixe o aplicativo [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Baixe o aplicativo [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

## <a name="every-binding-involves-these-pieces"></a>Cada associação envolve essas partes

-   Uma *origem de associação*. Essa é a fonte de dados para a associação e pode ser uma instância de qualquer classe que tem membros cujos valores você deseja exibir em sua interface do usuário.
-   Um *destino da associação*. Este é um [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) da [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) em sua interface do usuário que exibe os dados.
-   Um *objeto de associação*. É a parte que transfere os valores de dados da origem ao destino e, opcionalmente, do destino para a origem. O objeto de associação é criado no tempo de carregamento XAML da extensão de marcação [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).

Nas seções a seguir, vamos dar uma olhada na origem da associação, no destino da associação e no objeto de associação. E vincularemos as seções com o exemplo de associação do conteúdo de um botão a uma propriedade de string chamada **NextButtonText**, que pertence a uma classe chamada **HostViewModel**.

### <a name="binding-source"></a>Origem de associação

Consulte uma implementação muito rudimentar de uma classe que poderíamos usar como uma fonte de associação.

Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, adicionar novos itens de **Midl File (. idl)** para o projeto, chamado conforme mostrado no C++ c++ WinRT abaixo de listagem de exemplo de código. Substitua o conteúdo desses novos arquivos com o código de [MIDL 3.0](/uwp/midl-3/intro) mostrada na listagem, compile o projeto para gerar `HostViewModel.h` e `.cpp`e, em seguida, adicione o código para os arquivos gerados para corresponder a listagem. Para obter mais informações sobre esses arquivos gerados e como copiá-los em seu projeto, consulte [controles XAML; associar a C++ c++ WinRT propriedade](/windows/uwp/cpp-and-winrt-apis/binding-property).

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

Essa implementação de **HostViewModel**, e sua propriedade **NextButtonText**, só são apropriados para associação unidirecional. Mas associações unidirecionais e bidirecionais são extremamente comuns, e, nesses tipos de associação, a interface do usuário é atualizada automaticamente em resposta às alterações nos valores de dados de origem da associação. Para que esses tipos de associação funcionem corretamente, você precisa fazer sua fonte de associação "observável" para o objeto de associação. Assim, em nosso exemplo, se quisermos fazer associação unidirecional ou bidirecional à propriedade **NextButtonText**, todas as alterações que acontecem no tempo de execução para o valor da propriedade precisam ser feitas observáveis para o objeto de associação.

Uma maneira de fazer isso é derivar a classe que representa sua fonte de associação de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356), e expor um valor de dados por meio de um [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362). É assim que um [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) se torna observável. **FrameworkElements** são boas fontes de associação prontas para usar.

Uma maneira mais simples de tornar uma classe observável — e uma necessária para as classes que já têm uma classe base — é implementar [**System.ComponentModel.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx). Isso envolve apenas a implementação de um único evento denominado **PropertyChanged**. Um exemplo usando **HostViewModel** está abaixo.

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

Agora a propriedade **NextButtonText** é observável. Quando você cria uma associação unidirecional ou bidirecional com essa propriedade (mostraremos posteriormente), o objeto de associação resultante assina o evento **PropertyChanged**. Quando esse evento é acionado, o manipulador do objeto de associação recebe um argumento que contém o nome da propriedade que foi alterada. É assim que o objeto de associação sabe qual valor de propriedade usar e ler novamente.

Para que você não precise implementar o padrão mostrado acima várias vezes, se você estiver usando c# e em seguida, você pode simplesmente derivar de classe base **BindableBase** que você encontrará na amostra [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (na pasta "Comum"). Consulte um exemplo de como fica.

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> Para C++ c++ WinRT, qualquer classe de tempo de execução que você declara em seu aplicativo que deriva de uma classe base é conhecido como um *composable* classe. E há restrições ao redor composable classes. Para um aplicativo passar nos testes do [Kit de certificação de aplicativo do Windows](../debug-test-perf/windows-app-certification-kit.md) usados pelo Visual Studio e pela Microsoft Store para validar envios (e, portanto, para o aplicativo ser inserido com êxito na Microsoft Store), uma classe composable deve Por fim derive de uma classe base do Windows. Isso significa que a classe na raiz muito da hierarquia de herança deve ser um tipo originando do namespace. Se você precisar derivar uma classe de tempo de execução de uma classe base&mdash;por exemplo, para implementar uma classe **BindableBase** para todos os seus modelos de exibição derivar de&mdash;, em seguida, você pode derivar de [**DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).

Gerar o evento **PropertyChanged** com um argumento de [**String.Empty**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.empty.aspx) ou **null** indica que todas as propriedades não indexadas no objeto devem ser lidas novamente. Você pode gerar o evento para indicar que as propriedades no indexador do objeto foram alteradas com um argumento de "Item\[*indexer*\]" para indexadores específicos (em que *indexer* é o valor do índice), ou um valor de "Item\[\]" para todos os indexadores.

Uma origem de associação pode ser tratada como um único objeto cujas propriedades contêm dados ou como uma coleção de objetos. No código em c# e Visual Basic, você pode usar a associação unidirecional para um objeto que implementa [**List (Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) para exibir uma coleção que não muda em tempo de execução. Para uma coleção observável (observar quando itens são adicionados e removidos da coleção), use associação unidirecional a [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx). No código C++, você pode associar a [**Vector&lt;T&gt;**](https://msdn.microsoft.com/library/dn858385.aspx) para coleções observáveis e não observáveis. Para associar às suas próprias classes de coleção, use a diretriz da tabela a seguir.

|Cenário|C# e VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|Associar a um objeto.|Pode ser qualquer objeto.|Pode ser qualquer objeto.|O objeto deve ter [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) ou implementar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).|
|Obtenha as notificações de alteração de propriedade de um objeto associado.|Objeto deve implementar [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx).| Objeto deve implementar [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).|Objeto deve implementar [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).|
|Associar a uma coleção.| [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx)|[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)ou [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Consulte [controles de itens XAML; vincular a C++ c++ WinRT coleção](../cpp-and-winrt-apis/binding-collection.md) e [coleções com C++ c++ WinRT](../cpp-and-winrt-apis/collections.md).| [**Vetor&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx)|
|Obter as notificações de alteração de uma coleção associada.|[**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)|[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).|[**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/br226052.aspx)|
|Implemente uma coleção com suporte para associação.|Estenda [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) ou implemente [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx), [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/5y536ey6.aspx)(Of [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)), [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ienumerable.aspx) ou [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/9eekhta0.aspx)(Of **Object**). Não há suporte para a associação a **IList(Of T)** genérico e a **IEnumerable(Of T)**.|Implemente [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Consulte [controles de itens XAML; vincular a C++ c++ WinRT coleção](../cpp-and-winrt-apis/binding-collection.md) e [coleções com C++ c++ WinRT](../cpp-and-winrt-apis/collections.md).|Implemente [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; ou **IIterable**&lt;**IInspectable**\*&gt;. Não há suporte para a associação a **IVector&lt;T&gt;** genérico e **IIterable&lt;T&gt;**.|
| Implemente uma coleção que dá suporte a notificações de alteração de coleção. | Estenda [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) ou implemente [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) (não genérico) e [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx).|Implemente [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)ou [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).|Implemente [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) e [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).|
|Implemente uma coleção com suporte para carregamento incremental.|Estenda [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) ou implemente [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) (não genérico) e [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Além disso, implemente [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).|Implemente [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)ou [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Além disso, implemente [ **ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)|Implemente [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) e [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).|

Você pode associar controles de lista a fontes de dados arbitrariamente grandes e ainda assim obter um alto desempenho, usando o carregamento incremental. Por exemplo, você pode associar controles de lista a resultados de consulta de imagens do Bing sem precisar carregar todos os resultados de uma vez. Em vez disso, você carrega apenas alguns resultados imediatamente e carrega resultados adicionais conforme o necessário. Para dar suporte ao carregamento incremental, você deve implementar [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) em uma fonte de dados que dá suporte a notificações de alteração de coleção. Quando o mecanismo de vinculação de dados solicitar mais dados, a fonte de dados deverá fazer as solicitações apropriadas, integrar os resultados e, depois, enviar as notificações apropriadas para atualizar a interface do usuário.

### <a name="binding-target"></a>Destino da associação

Nos dois exemplos a seguir, a propriedade **Content** é o destino da associação e seu valor é definido como uma extensão de marcação que declara o objeto de associação. Primeiro [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) é mostrado e depois [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Declarar as associações na marcação é o caso comum (é conveniente, legível e editável). Porém, você pode evitar a marcação e criar obrigatoriamente (programaticamente) uma instância da classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), se precisar.

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

Se você estiver usando C++ c++ extensões de componente WinRT ou Visual C++ (C++ c++ /CX), em seguida, você precisará adicionar o atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) a qualquer classe de tempo de execução que você deseja usar com a extensão de marcação [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) .

> [!IMPORTANT]
> Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), o atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) estará disponível se você instalou o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809), ou posterior. Sem esse atributo, você precisará implementar as interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) e [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) para ser capaz de usar a extensão de marcação [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) .

### <a name="binding-object-declared-using-xbind"></a>Objeto de associação declarado usando {x:Bind}

Há uma etapa que precisamos fazer antes de criar nossa marcação [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783). Precisamos expor nossa classe de origem de associação da classe que representa nossa página de marcação. Fazemos isso adicionando uma propriedade (do tipo **HostViewModel** neste caso) à nossa classe de página **MainPage** .

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

Quando concluído, podemos dar uma olhada na marcação que declara o objeto de associação. O exemplo a seguir usa o mesmo destino da associação **Button.Content** que usamos na seção "Destino da associação" anteriormente, e mostra a vinculação à propriedade **HostViewModel.NextButtonText**.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Observe o valor que especificamos para **Path**. Esse valor é interpretado no contexto da página em si, e nesse caso, o caminho começa fazendo referência a propriedade **ViewModel** que acabamos para a página de **MainPage** . Essa propriedade retorna uma instância **HostViewModel** e, portanto, podemos especificar esse objeto para acessar a propriedade **HostViewModel.NextButtonText**. E podemos especificar **Mode**, para substituir o [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) padrão de unidirecional.

A propriedade [**Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path) dá suporte a várias opções de sintaxe para associação a propriedades aninhadas, propriedades anexadas e indexadores de inteiros e cadeias de caracteres. Para obter mais informações, consulte [Sintaxe do Property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586). A associação a indexadores de inteiros tem o mesmo efeito da associação a propriedades dinâmicas, mas sem implementar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). Para outras configurações, consulte [Extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783).

Para ilustrar que a propriedade **Hostviewmodel** é realmente observável, adicione um manipulador de eventos de **clique** ao botão e atualizar o valor de **Hostviewmodel**. Criar, executar e clique no botão para ver o valor do **conteúdo** do botão Atualizar.

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> As alterações para [**TextBox. Text**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text) são enviadas para uma fonte associada bidirecional quando [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) perde o foco e não após cada pressionamento de teclas do usuário.

**DataTemplate e x:DataType**

Dentro de um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (se usado como um modelo de item, um modelo de conteúdo ou um modelo de cabeçalho), o valor de **Path** não é interpretado no contexto da página, mas no contexto do objeto de dados que está sendo modelado. Ao usar {x:Bind} em um modelo de dados para que suas associações possam ser validadas (e um código eficiente gerado para elas) no momento da compilação, o **DataTemplate** precisa declarar o tipo de seu objeto de dados usando **x:DataType**. A amostra abaixo pode ser usada como o **ItemTemplate** de um controle de itens associado a uma coleção de objetos **SampleDataGroup**.

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Objetos de tipo fraco em seu caminho**

Por exemplo, considere que você tenha um tipo chamado SampleDataGroup, que implementa uma propriedade de cadeia de caracteres chamada Title. E você tem uma propriedade MainPage.SampleDataGroupAsObject, que é do tipo objeto, mas que, na verdade, retorna uma instância de SampleDataGroup. A associação `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` resultará em um erro de compilação porque a propriedade Title não foi encontrada no objeto de tipo. A solução para isso é adicionar uma conversão para a sintaxe do seu caminho como esta: `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`. Este é outro exemplo em que o elemento é declarado como objeto, mas é realmente um TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. E uma conversão soluciona o problema: `<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`.

**Se seus dados forem carregados de forma assíncrona**

O código para dar suporte a **{x:Bind}** é gerado no tempo de compilação nas classes parciais de suas páginas. Esses arquivos podem ser encontrados em sua pasta `obj`, com nomes como (para C#) `<view name>.g.cs`. O código gerado inclui um manipulador do evento [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706) de sua página e esse manipulador chama o método **Initialize** em uma classe gerada que representa as associações da sua página. **Initialize** chama **Update** para começar a movimentação de dados entre a origem da associação e o destino. **Loading** é disparado antes da primeira medida passar da página ou do controle de usuário. Portanto, se seus dados são carregados de forma assíncrona, talvez não estejam prontos no momento em que **Initialize** for chamado. Sendo assim, depois de carregar dados, você pode forçar a inicialização de associações únicas, chamando `this.Bindings.Update();`. Se você só precisa de associações únicas para dados carregados de forma assíncrona é muito mais barato inicializá-las assim que ter associações unidirecionais e ouvir as alterações. Se os seus dados não sofrerem mudanças detalhadas, e se é provável que sejam atualizados como parte de uma ação específica, então, você pode tornar suas associações únicas e forçar uma atualização manual a qualquer momento com uma chamada para **Update**.

> [!NOTE]
> **{x: Bind}** não é adequado para cenários de associação tardia, como navegar na estrutura de dicionário de um objeto JSON, nem digitação pato. "Digitação pato" é uma forma fraca de digitação com base em correspondências lexicais em nomes de propriedade (um, "se ele orienta, nada e grasnar como um pato, é um pato"). Com "Duck typing", uma vinculação à propriedade **idade** seria igualmente atendida com uma **pessoa** ou um **vinhos** objeto (supondo que esses tipos tinham uma propriedade de **idade** ). Para esses cenários, use a extensão de marcação **{Binding}** .

### <a name="binding-object-declared-using-binding"></a>Objeto de associação declarado usando {Binding}

Se você estiver usando C++ c++ extensões de componente WinRT ou Visual C++ (C++ c++ /CX), para usar a extensão de marcação [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) , você precisará adicionar o atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) a qualquer classe de tempo de execução que você deseja associar. Para usar [{x: Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), você não precisará desse atributo.

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), o atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) estará disponível se você instalou o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809), ou posterior. Sem esse atributo, você precisará implementar as interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) e [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) para ser capaz de usar a extensão de marcação [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) .

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) presume que, por padrão, você está associando ao [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) da sua página de marcação. Portanto, definiremos o **DataContext** de nossa página como uma instância de nossa classe de origem de associação (do tipo **HostViewModel**, nesse caso). O exemplo a seguir mostra a marcação que declara o objeto de associação. Usamos o mesmo destino da associação **Button.Content** usado na seção "Destino da associação" anteriormente e associamos à propriedade **HostViewModel.NextButtonText**.

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

Observe o valor que especificamos para **Path**. Esse valor é interpretado no contexto da página [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), que neste exemplo é definida como uma instância de **HostViewModel**. O caminho faz referência à propriedade **HostViewModel.NextButtonText**. Podemos omitir **Mode**, pois o padrão [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) de unidirecional funciona aqui.

O valor padrão de [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) para um elemento da interface do usuário é o valor herdado de seu pai. Você obviamente pode substituir esse padrão definindo **DataContext** explicitamente, que, por sua vez, é herdado pelos filhos por padrão. A definição de **DataContext** explicitamente em um elemento é útil quando você quer ter várias associações que usam a mesma origem.

Um objeto de associação tem uma propriedade **Source**, que assume como padrão o [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) do elemento de interface do usuário no qual a associação é declarada. Você pode substituir esse padrão definindo **Source**, **RelativeSource** ou **ElementName** explicitamente na associação (veja [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) para obter detalhes).

Dentro de um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) é automaticamente definido para o objeto de dados que está sendo modelado. O exemplo abaixo pode ser usado como o **ItemTemplate** de um controle de itens associado a uma coleção de qualquer tipo que tem propriedades de string chamadas **Title** e **Description**.

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> Por padrão, as alterações [**TextBox. Text**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text) são enviadas para uma fonte associada bidirecional quando [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) perde o foco. Para fazer com que as alterações sejam enviadas após cada pressionamento de teclas do usuário, defina **UpdateSourceTrigger** como **PropertyChanged** na associação na marcação. Você também pode assumir completamente o controle de quando as alterações são enviadas para a origem definindo **UpdateSourceTrigger** como **Explicit**. Você manipula eventos na caixa de texto (geralmente [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683)), chama [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.getbindingexpression) no destino para obter um objeto [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.aspx) e, finalmente, chama [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.updatesource.aspx) para atualizar de forma programática a fonte de dados.

A propriedade [**Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path) dá suporte a várias opções de sintaxe para associação a propriedades aninhadas, propriedades anexadas e indexadores de inteiros e cadeias de caracteres. Para obter mais informações, consulte [Sintaxe do Property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586). A associação a indexadores de inteiros tem o mesmo efeito da associação a propriedades dinâmicas, mas sem implementar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). A propriedade [**ElementName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.elementname) é útil para associação de elementos. A propriedade [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.relativesource) tem vários usos, um deles é como uma alternativa mais poderosa à associação de modelo dentro de um [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). Para outras configurações, consulte [Extensão de marcação {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) e a classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820).

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>E se a origem e o destino não forem do mesmo tipo?

Se desejar controlar a visibilidade de um elemento de interface do usuário com base no valor de uma propriedade booleana, ou se você desejar renderizar um elemento de interface do usuário com uma cor que é uma função de um intervalo ou tendência de um valor numérico, ou se quiser exibir um valor de data e/ou hora em uma propriedade de elemento de interface do usuário que espera uma string, precisará converter valores de um tipo em outro. Haverá casos em que a solução ideal é expor outra propriedade do tipo certo de sua classe de origem de associação e manter a lógica de conversão encapsulada e testada lá. Mas isso não é flexível nem escalável quando você tem números grandes, ou combinações grandes, das propriedades de origem e destino. Nesse caso, você tem algumas opções:

* Se estiver usando {x:Bind}, você poderá vincular-se diretamente a uma função para fazer essa conversão
* Ou poderá especificar um conversor de valor que é um objeto criado para executar a conversão 

## <a name="value-converters"></a>Conversores de valor

Consulte um conversor de valor, adequado para uma associação única ou unidirecional, que converte um valor [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx) em um valor de string que contém o mês. A classe implementa [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

E aqui está como consumir esse conversor de valor em sua marcação de objeto de associação.

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

O mecanismo de associação chamará os métodos [**Convert**](https://msdn.microsoft.com/library/windows/apps/hh701934) e [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/hh701938) se o parâmetro [**Converter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converter) for definido para a associação. Quando os dados forem repassados da origem, o mecanismo de associação chamará **Convert** e repassará os dados retornados ao destino. Quando os dados forem repassados do destino (para uma associação bidirecional), o mecanismo de associação chamará **ConvertBack** e repassará os dados retornados à origem.

O conversor também tem parâmetros opcionais: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterlanguage), que permite especificar o idioma que será usado na conversão, e [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterparameter), que permite repassar um parâmetro para a lógica de conversão. Para obter um exemplo de uso de um parâmetro de conversor, consulte [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

> [!NOTE]
> Se houver um erro na conversão, não emita uma exceção. Em vez disso, retorne [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyproperty.unsetvalue), que interromperá a transferência de dados.

Para exibir um valor padrão a ser usado sempre que a origem da associação não puder ser resolvida, defina a propriedade **FallbackValue** no objeto de associação na marcação. Isso é útil para lidar com erros de conversão e formatação. E também para associar propriedades da origem que talvez não existam em todos os objetos em uma coleção associada de tipos heterogêneos.

Se você associar um controle de texto a um valor que não seja uma cadeia, o mecanismo de vinculação de dados converterá o valor a uma cadeia. Se o valor for um tipo de referência, o mecanismo de vinculação de dados recuperará o valor da cadeia chamando [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) ou [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) se disponível e, caso não estejam, chamará [**Object.ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx). Observe, porém, que esse mecanismo ignora qualquer implementação de **ToString** que oculte a implementação da classe base. Implementações de subclasse devem substituir o método **ToString** da classe base em vez disso. Da mesma forma, nos idiomas nativos, todos os objetos gerenciados parecem implementar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) e [**IStringable**](https://msdn.microsoft.com/library/Dn302135). Porém, todas as chamadas para **GetStringRepresentation** e **IStringable.ToString** são roteadas para **Object.ToString** ou para uma substituição desse método e nunca para uma nova implementação de **ToString** que oculte a implementação da classe base.

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um booliano integrado para conversor de Visibilidade. O conversor mapeia **true** para o valor de enumeração **Visible** e **false** para **Collapsed** para que você possa associar uma propriedade de Visibilidade a um booleano sem criar um conversor. Para usar o conversor integrado, a versão do SDK de alvo mínimo do seu aplicativo deve ser 14393 ou posterior. Você não poderá usá-lo se seu aplicativo for voltado para versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

## <a name="function-binding-in-xbind"></a>Associação de função em {x:Bind}

{x:Bind} permite que a etapa final em um caminho de associação seja uma função. Isso pode ser usado para realizar conversões e realizar associações que dependem de mais de uma propriedade. Consulte [ **funções em x: Bind**](function-bindings.md)

<span id="resource-dictionaries-with-x-bind"/>

## <a name="resource-dictionaries-with-xbind"></a>Dicionários de recursos com {x:Bind}

A [Extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) depende de geração do código e, assim, precisa de um arquivo de código que contém um construtor que chama **InitializeComponent** (para inicializar o código gerado). Você reutiliza o dicionário de recursos instanciando seu tipo (para que **InitializeComponent** seja chamado) em vez de fazer referência a seu nome de arquivo. Consulte um exemplo do que fazer se você tiver um dicionário de recursos existente e desejar usar {x:Bind} nele.

TemplatesResourceDictionary.xaml

```xaml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xaml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## <a name="event-binding-and-icommand"></a>Associação de eventos e ICommand

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) dá suporte a um recurso chamado de associação de eventos. Com esse recurso, você pode especificar o manipulador para um evento usando uma associação, que é uma opção adicional sobre a manipulação de eventos com um método no arquivo de código. Digamos que você tenha uma propriedade **RootFrame** em sua classe **MainPage**.

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

Em seguida, você pode associar um evento **Click** de um botão a um método no objeto **Frame** retornado pela propriedade **RootFrame** como este. Observe que nós também associamos a propriedade **IsEnabled** do botão a outro membro do mesmo **Frame**.

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

Métodos sobrecarregados não podem ser usados para manipular um evento com essa técnica. Além disso, se o método que manipula o evento tem parâmetros, eles devem ser atribuíveis dos tipos de todos os parâmetros do evento, respectivamente. Nesse caso, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) não é sobrecarregado e não tem parâmetros (mas ainda seria válido mesmo se tivesse dois parâmetros **object**). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) é sobrecarregado, de modo que não podemos usar esse método com essa técnica.

A técnica de associação de eventos é semelhante a implementar e consumir comandos (um comando é uma propriedade que retorna um objeto que implementa a interface [**ICommand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.icommand.aspx)). [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) e [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) funcionam com comandos. Para que você não precise implementar o padrão de comando várias vezes, pode usar a classe auxiliar **DelegateCommand** que encontrará na amostra [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (na pasta "Comum").

## <a name="binding-to-a-collection-of-folders-or-files"></a>Associando a uma coleção de pastas ou arquivos

Você pode usar as APIs do namespace [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) para recuperar dados de pastas e arquivos. No entanto, os diversos métodos **GetFilesAsync**, **GetFoldersAsync** e **GetItemsAsync** não retornam valores adequados para a associação a controles de lista. Em vez disso, você deve associar os valores de retorno dos métodos [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428) e [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) da classe [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501). A amostra de código a seguir, da [amostra de StorageDataSource e GetVirtualizedFilesVector](http://go.microsoft.com/fwlink/p/?linkid=228621), mostra o padrão de uso típico. Lembre-se de declarar a funcionalidade **picturesLibrary** em seu manifesto de pacote do aplicativo e confirmar que há imagens na sua pasta de biblioteca de imagens.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var library = Windows.Storage.KnownFolders.PicturesLibrary;
    var queryOptions = new Windows.Storage.Search.QueryOptions();
    queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
    queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

    var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

    var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
        fileQuery,
        Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
        190,
        Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
        false
        );

    var dataSource = fif.GetVirtualizedFilesVector();
    this.PicturesListView.ItemsSource = dataSource;
}
```

Você normalmente usará essa abordagem para criar um modo de exibição somente leitura das informações de arquivos e de pastas. Você pode criar associações bidirecionais às propriedades de arquivo e pasta para, por exemplo, permitir que os usuários classifiquem uma música em um modo de exibição de músicas. Porém, as alterações não serão persistidas até que você chame o método **SavePropertiesAsync** apropriado (por exemplo, [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Você deve confirmar as alterações quando o item perde o foco porque isso ativa uma redefinição de seleção.

Observe que a associação bidirecional que usa essa técnica funciona apenas em locais indexados, por exemplo, em Música. Você pode determinar se um local é indexado chamando o método [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627).

Observe também que um vetor virtualizado pode retornar **null** para alguns itens antes de ele preencher seu valor. Por exemplo, você deve verificar se há **null** antes de usar o valor [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) de um limite de controle de lista para um vetor virtualizado ou use [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768).

## <a name="binding-to-data-grouped-by-a-key"></a>Associação a dados agrupados por uma chave

Se você usar uma coleção simples de itens (livros, por exemplo, representados por uma classe de **BookSku** ) e agrupar os itens usando uma propriedade comum como uma chave (a propriedade de **booksku. AuthorName** , por exemplo), o resultado será chamado de dados agrupados. Quando você agrupa os dados, não é mais uma coleção simples. Dados agrupados são uma coleção de objetos de grupo, onde cada objeto de grupo tem

- uma chave, e
- uma coleção de itens cuja propriedade corresponde a essa chave.

Para aproveitar o exemplo de livros novamente, o resultado de agrupar os livros pelo nome do autor resulta em uma coleção de grupos de nome do autor onde cada grupo tem

- uma chave, que é um nome do autor, e
- uma coleção do s **BookSku**cuja propriedade **AuthorName** coincide com a chave do grupo.

Em geral, para exibir uma coleção, você associa [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) de um controle de itens (como [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) diretamente a uma propriedade que retorna uma coleção. Se for uma coleção simples de itens, você não precisa fazer nada especial. Mas se for uma coleção de objetos de grupo (como ao associar a dados agrupados), você precisa dos serviços de um objeto intermediário chamado [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) que fica entre o controle de itens e a origem da associação. Você associa **CollectionViewSource** à propriedade que retorna dados agrupados, e associa o controle de itens a **CollectionViewSource**. Uma vantagem extra de um **CollectionViewSource** é que ele acompanha o item atual e, assim, você pode manter mais de um controle de itens em sincronia, associando todos ao mesmo **CollectionViewSource**. Você também pode acessar o item atual de forma programática usando a propriedade [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) do objeto retornado pela propriedade [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.view).

Para ativar o recurso de agrupamento de um [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), defina [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.issourcegrouped) como **true**. Se você também precisa definir a propriedade [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.itemspath) depende exatamente de como você cria seus objetos de grupo. Há duas maneiras de criar um objeto de grupo: o padrão "é-um-grupo" e o padrão "tem-um-grupo". No padrão "é-um-grupo", o objeto de grupo é derivado de um tipo de coleção (por exemplo, **List&lt;T&gt;**), portanto, o objeto de grupo, na verdade, é o grupo de itens. Com esse padrão, você não precisa definir **ItemsPath**. No padrão "tem-um-grupo", o objeto de grupo tem uma ou mais propriedades de um tipo de coleção (como **List&lt;T&gt;**), portanto, o grupo "tem um" grupo de itens na forma de uma propriedade (ou vários grupos de itens na forma de várias propriedades). Com esse padrão, é necessário definir **ItemsPath** como o nome da propriedade que contém o grupo de itens.

O exemplo a seguir ilustra o padrão "tem-um-grupo". A classe page tem uma propriedade chamada [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), que retorna uma instância de nosso modelo de exibição. O [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) associa à propriedade **Authors** do modelo de exibição (**Authors** é a coleção de objetos de grupo) e também especifica que é a propriedade **Author.BookSkus** que contém os itens agrupados. Por fim, o [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) está associado a **CollectionViewSource**, e tem seu estilo de grupo definido para que possa renderizar os itens em grupos.

```csharp
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

Você pode implementar o padrão "é-um-grupo" de uma de duas maneiras. Uma maneira é criar sua própria classe de grupo. Derive a classe de **List&lt;T&gt;** (onde *T* é o tipo dos itens). Por exemplo, `public class Author : List<BookSku>`. A segunda maneira é usar uma expressão [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) para criar dinamicamente objetos de grupo (e uma classe de grupo) de valores de propriedade dos itens **BookSku**. Essa abordagem (manter somente uma lista simples de itens e agrupá-los em tempo real) é típica de um aplicativo que acessa dados de um serviço de nuvem. Você tem a flexibilidade para agrupar livros por autor ou por gênero (por exemplo) sem a necessidade de classes de grupos especiais, como **Author** e **Genre**.

O exemplo a seguir ilustra o padrão "é-um-grupo" usando [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Desta vez, agrupamos livros por gênero, exibido com o nome do gênero nos cabeçalhos de grupo. Isso é indicado pelo caminho de propriedade "Key" na referência ao valor [**Key**](https://msdn.microsoft.com/library/windows/apps/bb343251.aspx) do grupo.

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

Lembre-se de que, ao usar [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) com modelos de dados, precisamos indicar o tipo associado definindo um valor **x:DataType**. Se o tipo for genérico, não poderemos expressar isso na marcação e, portanto, precisamos usar [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) no modelo de cabeçalho de estilo de grupo.

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

Um controle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) é uma ótima maneira para que os usuários vejam e naveguem por dados agrupados. O aplicativo de amostra [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) ilustra como usar o **SemanticZoom**. No aplicativo, você pode exibir uma lista de livros agrupados por autor (a exibição ampliada) ou pode reduzir o zoom para ver uma lista de atalhos de autores (a exibição reduzida). A lista de atalhos proporciona uma navegação mais rápida do que rolar pela lista de livros. Os modos de exibição ampliada e reduzida são, na verdade, controles **ListView** ou **GridView** associados à mesma **CollectionViewSource**.

![Uma ilustração de um SemanticZoom](images/sezo.png)

Quando você associa a dados hierárquicos — como subcategorias nas categorias — pode optar por exibir os níveis hierárquicos em sua interface do usuário com uma série de controles de itens. Uma seleção em um controle de itens determina o conteúdo dos controles de itens subsequentes. Você pode manter as listas sincronizadas associando cada uma delas à sua própria [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) e associando as instâncias **CollectionViewSource** em uma cadeia. Isso é chamado de modo de exibição mestre/detalhado (ou lista/detalhes). Para saber mais, consulte [Como associar a dados hierárquicos e criar um modo de exibição mestre/detalhado](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>Diagnosticar e depurar problemas de vinculação de dados

Sua marcação de associação contém os nomes de propriedades (e, para C#, às vezes, campos e métodos). Portanto, quando você renomeia uma propriedade, também precisa alterar qualquer associação que faz referência a ela. Esquecer de fazer isso leva a um exemplo típico de um bug de vinculação de dados, e seu aplicativo não será compilado ou não será executado corretamente.

Os objetos de associação criados por [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) e [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) são em grande parte funcionalmente equivalentes. Mas {x:Bind} tem informações de tipo para a origem da associação, e gera código-fonte no momento da compilação. Com {x:Bind}, você obtém o mesmo tipo de detecção de problemas que obtém com o resto do seu código. Isso inclui validação de tempo de compilação das expressões de associação e depuração definindo pontos de interrupção no código-fonte gerado como a classe parcial para sua página. Essas classes podem ser encontradas nos arquivos na pasta `obj`, com nomes como (para C#) `<view name>.g.cs`). Se você tiver um problema com uma associação, ative **Interromper Exceções sem Tratamento** no depurador do Microsoft Visual Studio. O depurador interromperá a execução nesse ponto e, em seguida, você pode depurar o que deu errado. O código gerado por {x:Bind} segue o mesmo padrão para cada parte do gráfico de nós de origem de associação, e você pode usar as informações da janela **Pilha de Chamadas** para ajudar a determinar a sequência de chamadas que levaram ao problema.

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) não tem informações de tipo para a origem da associação. Mas, quando você executa o aplicativo com o depurador anexado, os possíveis erros de associação aparecem na janela **Saída** do Visual Studio.

## <a name="creating-bindings-in-code"></a>Criando associações em código

**Observação**  Esta seção só se aplica a [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), pois você não pode criar associações [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) no código. No entanto, alguns dos mesmos benefícios de {x:Bind} podem ser obtidos com [**DependencyObject.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx), o que permite que você se registre para receber notificações de alteração em qualquer propriedade de dependência.

Você também pode conectar elementos da interface de usuário aos dados usando código de procedimentos em vez de XAML. Para fazer isso, crie um novo objeto [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), defina as propriedades apropriadas, depois chame [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257.aspx) ou [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244376.aspx). A criação de associações de forma programática é útil quando você deseja escolher os valores da propriedade de associação no tempo de execução ou compartilhar uma única associação entre vários controles. Observe, porém, que você não pode alterar os valores da propriedade de associação depois que chamar **SetBinding**.

O exemplo a seguir mostra como implementar uma associação em código.

```xaml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## <a name="xbind-and-binding-feature-comparison"></a>Comparação de recursos de {x:Bind} e {Binding}

| Recurso | {x:Bind} | {Binding} | Observações |
|---------|----------|-----------|-------|
| O caminho é a propriedade padrão | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Propriedade de caminho | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | Em x:Bind, caminho raiz fica na página por padrão, não no DataContext. | 
| Indexador | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Associa ao item especificado na coleção. Somente indexações baseadas em números inteiros são permitidas. | 
| Propriedades anexadas | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Propriedades anexadas são especificadas usando parênteses. Se a propriedade não for declarada em um namespace XAML, então será necessário prefixá-la com um namespace xml, que deve ser mapeado para um namespace de código no cabeçalho do documento. | 
| Transmissão | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Não necessário. | As conversões são especificadas usando parênteses. Se a propriedade não for declarada em um namespace XAML, então será necessário prefixá-la com um namespace xml, que deve ser mapeado para um namespace de código no cabeçalho do documento. | 
| Conversor | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Os conversores devem ser declarados na raiz de Page/ResourceDictionary, ou em App.xaml. | 
| ConversorParameter, ConversorLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Os conversores devem ser declarados na raiz de Page/ResourceDictionary, ou em App.xaml. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Usado quando a folha da expressão de associação é nula. Use aspas simples para um valor de string. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Usado quando alguma parte do caminho para a associação (exceto a folha) é nula. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Com {x:Bind}, você está associando a um campo; Caminho fica na raiz na página por padrão e, assim, qualquer elemento nomeado pode ser acessado por meio de seu campo. | 
| RelativeFonte: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | Com {x:Bind}, dê um nome para o elemento e use seu nome no caminho. | 
| RelativeFonte: TemplatedParent | Não necessário | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | Com {x: Bind} TargetType em ControlTemplate indica vinculação ao pai do modelo. Para {Binding} associação de modelo Regular pode ser usada nos modelos de controle para a maioria dos usos. Mas use TemplatedParent onde você precisa usar um conversor ou uma associação bidirecional.< | 
| Source | Não necessário | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | Para {x: Bind}, você pode usar diretamente o elemento nomeado, use uma propriedade ou um caminho estático. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Modo pode ser OneTime, OneWay ou TwoWay. {x:Bind} usa como padrão OneTime; {Binding} OneWay. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger pode ser Padrão, LostFocus ou PropertyChanged. {x:Bind} não oferece suporte a UpdateSourceTrigger=Explicit. {x:Bind} usa o comportamento PropertyChanged para todos os casos, exceto TextBox.Text, onde ele usa o comportamento LostFocus. | 


