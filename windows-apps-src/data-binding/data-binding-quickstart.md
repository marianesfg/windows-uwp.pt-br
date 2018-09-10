---
author: mcleblanc
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: Visão geral da associação de dados
description: Este tópico mostra como associar um controle (ou outro elemento da interface do usuário) a um único item ou um controle de itens a uma coleção de itens em um aplicativo da Plataforma Universal do Windows (UWP).
ms.author: markl
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e070acdfcc6dded12ae48cc34fe46f099da6401
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3821060"
---
# <a name="data-binding-overview"></a>Visão geral da associação de dados

Este tópico mostra como associar um controle (ou outro elemento da interface do usuário) a um único item ou um controle de itens a uma coleção de itens em um aplicativo UWP (Plataforma Universal do Windows). Este tópico também mostra como controlar a renderização de itens, implementar uma exibição de detalhes com base em uma seleção e converter dados para exibição. Para saber mais detalhes, consulte [Vinculação de dados em detalhes](data-binding-in-depth.md).

## <a name="prerequisites"></a>Pré-requisitos

Este tópico pressupõe que você saiba como criar um aplicativo UWP básico. Para obter instruções sobre como criar seu primeiro aplicativo UWP, consulte [Introdução aos aplicativos do Windows](https://developer.microsoft.com/windows/getstarted).

## <a name="create-the-project"></a>Criar o projeto

Crie um novo projeto **Aplicativo em branco (universal do Windows)**. Nomeie-o como "Início rápido".

## <a name="binding-to-a-single-item"></a>Associação a um único item

Cada associação consiste em um destino da associação e uma origem de associação. Normalmente, o destino é uma propriedade de um controle ou outro elemento de interface do usuário, e a origem é uma propriedade de uma instância de classe (um modelo de dados ou um modelo de exibição). Este exemplo mostra como associar um controle a um único item. O destino é a propriedade **Text** de um **TextBlock**. A origem é uma instância de uma classe simples denominada **Recording** que representa uma gravação de áudio. Primeiro, vamos examinar a classe.

Se você estiver usando c#, em seguida, adicione uma nova classe ao seu projeto e nomeie-o `Recording.cs`.

Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, adicionar novos itens de **Midl File (. idl)** para o projeto, chamado conforme mostrado no C++ c++ WinRT abaixo de listagem de exemplo de código. Substitua o conteúdo desses novos arquivos com o código de [MIDL 3.0](/uwp/midl-3/intro) mostrada na listagem, compile o projeto para gerar `Recording.h` e `.cpp` e `RecordingViewModel.h` e `.cpp`e, em seguida, adicione o código para os arquivos gerados para corresponder a listagem. Para obter mais informações sobre esses arquivos gerados e como copiá-los em seu projeto, consulte [controles XAML; associar a C++ c++ WinRT propriedade](/windows/uwp/cpp-and-winrt-apis/binding-property).

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }
        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }
        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }
    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cppwinrt
// Recording.idl
namespace Quickstart
{
    runtimeclass Recording : Windows.UI.Xaml.DependencyObject
    {
        Recording(String artistName, String compositionName, Windows.Globalization.Calendar releaseDateTime);
        String ArtistName{ get; };
        String CompositionName{ get; };
        Windows.Globalization.Calendar ReleaseDateTime{ get; };
        String OneLineSummary{ get; };
    }
}

// RecordingViewModel.idl
import "Recording.idl";

namespace Quickstart
{
    runtimeclass RecordingViewModel : Windows.UI.Xaml.DependencyObject
    {
        RecordingViewModel();
        Quickstart.Recording DefaultRecording{ get; };
    }
}

// Recording.h
// Add these fields:
...
#include <sstream>
...
private:
    std::wstring m_artistName;
    std::wstring m_compositionName;
    Windows::Globalization::Calendar m_releaseDateTime;
...

// Recording.cpp
// Implement like this:
...
Recording::Recording(hstring const& artistName, hstring const& compositionName, Windows::Globalization::Calendar const& releaseDateTime) :
    m_artistName{ artistName.c_str() },
    m_compositionName{ compositionName.c_str() },
    m_releaseDateTime{ releaseDateTime } {}

hstring Recording::ArtistName(){ return hstring{ m_artistName }; }
hstring Recording::CompositionName(){ return hstring{ m_compositionName }; }
Windows::Globalization::Calendar Recording::ReleaseDateTime(){ return m_releaseDateTime; }

hstring Recording::OneLineSummary()
{
    std::wstringstream wstringstream;
    wstringstream << m_compositionName.c_str();
    wstringstream << L" by " << m_artistName.c_str();
    wstringstream << L", released: " << m_releaseDateTime.MonthAsNumericString().c_str();
    wstringstream << L"/" << m_releaseDateTime.DayAsString().c_str();
    wstringstream << L"/" << m_releaseDateTime.YearAsString().c_str();
    return hstring{ wstringstream.str().c_str() };
}
...

// RecordingViewModel.h
// Add this field:
...
#include "Recording.h"
...
private:
    Quickstart::Recording m_defaultRecording{ nullptr };
...

// RecordingViewModel.cpp
// Implement like this:
...
Quickstart::Recording RecordingViewModel::DefaultRecording()
{
    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Month(1);
    releaseDateTime.Day(1);
    releaseDateTime.Year(1761);
    m_defaultRecording = winrt::make<Recording>(L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime);
    return m_defaultRecording;
}
...
```

```cpp
#include <sstream>
namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;
    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}
        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }
        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }
        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }
        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c_str());
            }
        }
    };
    public ref class RecordingViewModel sealed
    {
    private:
        Recording ^ defaultRecording;
    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            releaseDateTime->Year = 1761;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }
        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}
```

Em seguida, exponha a classe de origem de associação na classe que representa a página de marcação. Fazemos isso adicionando uma propriedade do tipo **RecordingViewModel** a **MainPage**.

Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, primeira atualização `MainPage.idl`. Compile o projeto para gerar `MainPage.h` e `.cpp`e mesclar as alterações nesses arquivos gerados aqueles em seu projeto.

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }
        public RecordingViewModel ViewModel{ get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
// Add this property:
import "RecordingViewModel.idl";
...
RecordingViewModel ViewModel{ get; };
...

// MainPage.h
// Add this property and this field:
...
#include "RecordingViewModel.h"
...
    Quickstart::RecordingViewModel ViewModel();

private:
    Quickstart::RecordingViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();
    m_viewModel = winrt::make<RecordingViewModel>();
}
Quickstart::RecordingViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

```cpp
namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel ^ viewModel;
    public:
        MainPage()
        {
            InitializeComponent();
            this->viewModel = ref new RecordingViewModel();
        }
        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}
```

A última parte é associar um **TextBlock** à propriedade **ViewModel.DefaultRecording.OneLiner**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, você precisará remover a função de **MainPage:: clickHandler** na ordem para o projeto compilar.

Consulte o resultado.

![Associando um bloco de texto](images/xaml-databinding0.png)

## <a name="binding-to-a-collection-of-items"></a>Associação a uma coleção de itens

Um cenário comum é associar a uma coleção de objetos comerciais. No C# e no Visual Basic, a classe genérica [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) é uma boa escolha de coleção para a vinculação de dados, porque ela implementa as interfaces [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) e [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Essas interfaces fornecem notificação de alteração para associações quando itens são adicionados ou removidos ou uma propriedade da lista muda. Se você quiser que os controles associados sejam atualizados com alterações em propriedades de objetos na coleção, o objeto comercial também deve implementar **INotifyPropertyChanged**. Para obter mais informações, consulte [Vinculação de dados em detalhes](data-binding-in-depth.md).

Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, você pode ler sobre a associação a uma coleção observável no [controles de itens XAML; vincular a C++ c++ WinRT coleção](/windows/uwp/cpp-and-winrt-apis/binding-collection).

O exemplo seguinte associa [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) a uma coleção de objetos `Recording`. Vamos começar adicionando a coleção ao nosso modelo de exibição. Basta adicionar esses novos membros à classe **RecordingViewModel**.

```csharp
public class RecordingViewModel
{
    ...
        private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
    public ObservableCollection<Recording> Recordings{ get{ return this.recordings; } }
        public RecordingViewModel()
    {
        this.recordings.Add(new Recording(){ ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
        this.recordings.Add(new Recording(){ ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
        this.recordings.Add(new Recording(){ ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
    }
}
```

```cpp
public ref class RecordingViewModel sealed
{
private:
    ...
    Windows::Foundation::Collections::IVector<Recording^>^ recordings;
public:
    RecordingViewModel()
    {
        ...
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Month = 7;
        releaseDateTime->Day = 8;
        releaseDateTime->Year = 1748;
        Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Month = 2;
        releaseDateTime->Day = 11;
        releaseDateTime->Year = 1805;
        recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Month = 12;
        releaseDateTime->Day = 3;
        releaseDateTime->Year = 1737;
        recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
        this->Recordings->Append(recording);
    }
    ...
    property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
    {
        Windows::Foundation::Collections::IVector<Recording^>^ get()
        {
            if (this->recordings == nullptr)
            {
                this->recordings = ref new Platform::Collections::Vector<Recording^>();
            }
            return this->recordings;
        };
    }
};
```

E, em seguida, associar uma [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) à propriedade **ViewModel.Recordings**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Ainda não fornecemos um modelo de dados para a classe **Recording**, portanto o melhor que a estrutura da IU pode fazer é chamar [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) para cada item na [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). A implementação padrão do **ToString** é retornar o nome do tipo.

![Associando um modo de exibição de lista](images/xaml-databinding1.png)

Para corrigir isso, podemos substituir [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) para retornar o valor de **OneLineSummary**, ou podemos fornecer um modelo de dados. A opção de modelo de dados é mais comum e possivelmente mais flexível. Você especifica um modelo de dados usando a propriedade [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369) de um controle de conteúdo ou a propriedade [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830) de um controle de itens. Aqui estão duas maneiras de criar um modelo de dados para **Recording** com uma ilustração do resultado.

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <TextBlock Text="{x:Bind OneLineSummary}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![Associando um modo de exibição de lista](images/xaml-databinding2.png)

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Margin="6">
                <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                <StackPanel>
                    <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                    <TextBlock Text="{x:Bind CompositionName}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![Associando um modo de exibição de lista](images/xaml-databinding3.png)

Para obter mais informações sobre a sintaxe XAML, consulte [Criar uma interface do usuário com XAML](https://msdn.microsoft.com/library/windows/apps/Mt228349). Para obter mais informações sobre o layout de controle, consulte [Definir layouts com XAML](https://msdn.microsoft.com/library/windows/apps/Mt228350).

## <a name="adding-a-details-view"></a>Adicionando uma visão de detalhes

Você pode optar por exibir todos os detalhes dos objetos **Recording** nos itens [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). Mas isso ocupa muito espaço. Em vez disso, você pode mostrar apenas dados suficientes no item para identificá-lo e, em seguida, quando o usuário fizer uma seleção, pode exibir todos os detalhes do item selecionado em um componente separado da interface do usuário conhecido como o modo de exibição de detalhes. Além disso, esse esquema também é conhecido como um modo de exibição mestre/detalhado, ou um modo de exibição de lista/detalhes.

Há duas maneiras de lidar com isso. Você pode associar o modo de exibição de detalhes à propriedade [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) do [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). Ou você pode usar um [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833): associar **ListView** e o modo de exibição de detalhes a **CollectionViewSource** (que cuidará do item atualmente selecionado para você). Ambas as técnicas são mostradas abaixo, e ambas geram os mesmos resultados mostrados na ilustração.

> [!NOTE]
> Até agora neste tópico só usamos a [extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), mas ambas as técnicas mostradas abaixo requerem a [extensão de marcação {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) mais flexível (mas menos eficiente).


Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, você precisará implementar o [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) e [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) interfaces para ser capaz de usar a extensão de marcação {Binding}.

Se você estiver usando extensões de componente do Visual C++ (C++/CX), como vamos usar [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), você precisará adicionar o atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) à classe **Recording**.

Primeiro, a técnica [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770).
```cpp
[Windows::UI::Xaml::Data::Bindable]
public ref class Recording sealed
{
    ...
};
```

A única outra alteração necessária é na marcação.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

Para a técnica [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), primeiro adicione um **CollectionViewSource** como um recurso de página.

```xml
<Page.Resources>
    <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
</Page.Resources>
```

E, em seguida, ajuste as associações no [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) (que não precisa mais ser chamado) e no modo de exibição de detalhes para usar o [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833). Observe que, associando o modo de exibição de detalhes diretamente a **CollectionViewSource**, você está indicando que deseja associar ao item atual em associações em que o caminho não pode ser encontrado na coleção em si. Não é necessário especificar a propriedade **CurrentItem** como o caminho para a associação, embora você possa fazer isso se houver ambiguidade.

```xml
...

<ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">

...

<StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
...
```

E aqui está o resultado idêntico em cada caso.

![Associando um modo de exibição de lista](images/xaml-databinding4.png)

## <a name="formatting-or-converting-data-values-for-display"></a>Formatação ou conversão de valores de dados para exibição

Há um pequeno problema com a renderização acima. A propriedade **ReleaseDateTime** não é apenas uma data, é um [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx), de forma que está sendo exibida com precisão maior do que precisamos. Uma solução é adicionar uma propriedade de string à classe **Recording** que retorna `this.ReleaseDateTime.ToString("d")`. Chamar essa propriedade **ReleaseDate** indica que ela retorna uma data, não uma data e hora. Chamar **ReleaseDateAsString** indica ainda mais que ela retorna uma string.

Uma solução mais flexível é usar algo conhecido como um conversor de valor. Consulte um exemplo de como criar seu próprio conversor de valor. Adicione este código ao seu arquivo de código-fonte Recording.cs.

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

Agora podemos adicionar uma instância de **StringFormatter** como um recurso de página e usá-lo em nossa associação. Nós passamos a string de formato da marcação para o conversor de marcação para o máximo em flexibilidade de formatação.

```xml
<Page.Resources>
    <local:StringFormatter x:Key="StringFormatterValueConverter"/>
</Page.Resources>
...

<TextBlock Text="{Binding ReleaseDateTime,
    Converter={StaticResource StringFormatterValueConverter},
    ConverterParameter=Released: \{0:d\}}"/>

...
```

Consulte o resultado.

![exibindo uma data com formatação personalizada](images/xaml-databinding5.png)

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um booleano integrado para conversor de Visibilidade. O conversor mapeia **true** para o valor de enumeração **Visible** e **false** para **Collapsed** para que você possa associar uma propriedade de Visibilidade a um booleano sem criar um conversor. Para usar o conversor integrado, a versão do SDK de alvo mínimo do seu aplicativo deve ser 14393 ou posterior. Você não poderá usá-lo se seu aplicativo for voltado para versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

## <a name="see-also"></a>Consulte também
- [Vinculação de dados](index.md)
