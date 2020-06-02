---
description: Este tópico descreve os detalhes técnicos envolvidos na portabilidade do código-fonte em um projeto [C#](/visualstudio/get-started/csharp) para o equivalente no [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Mover do C# para C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, compatibilizar, migrar, C#
ms.localizationpriority: medium
ms.openlocfilehash: 38ad2d4f2b0af65424e6d9fa50f2c21b626e1914
ms.sourcegitcommit: 3125d5e2e32831481790266f44967851585888b3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172827"
---
# <a name="move-to-cwinrt-from-c"></a>Mover do C# para C++/WinRT

Este tópico descreve os detalhes técnicos envolvidos na portabilidade do código-fonte em um projeto [C#](/visualstudio/get-started/csharp) para o equivalente no [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

Para obter um estudo de caso de portabilidade de um dos exemplos de aplicativo UWP (Plataforma Universal do Windows), confira o tópico complementar [Como portar a amostra de área de transferência para o C++/WinRT do C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp). Você pode adquirir prática e experiência em portabilidade seguindo o passo a passo e fazendo a portabilidade da amostra por conta própria durante as etapas.

## <a name="how-to-prepare-and-what-to-expect"></a>Como se preparar e o que esperar

O estudo de caso [Como portar a amostra de área de transferência para o C++/WinRT do C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) ilustra exemplos dos tipos de decisões de design de software que você tomará ao portar um projeto para o C++/WinRT. Portanto, é uma boa ideia se preparar para a portabilidade adquirindo uma compreensão substancial de como o código existente funciona. Dessa forma, você obterá uma boa visão geral da funcionalidade do aplicativo e da estrutura do código, e as decisões que você tomará sempre o conduzirão para frente e na direção certa.

Em relação a quais tipos de portabilidade devem ser esperados, é possível agrupá-los em quatro categorias.

- [**Portar a projeção de linguagem**](#port-the-language-projection). O WinRT (Windows Runtime) é *projetado* em várias linguagens de programação. Cada uma dessas projeções de linguagem foi criada para dar um aspecto idiomático à linguagem de programação em questão. Quanto ao C#, alguns tipos do Windows Runtime foram projetados como tipos .NET. Por exemplo, você converterá [**System.Collections.Generic.IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1) novamente em [**Windows.Foundation.Collections.IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1). Também em C#, algumas operações do Windows Runtime foram projetadas como recursos da linguagem C# convenientes. Um exemplo é que, em C#, você usa a sintaxe do operador `+=` para registrar um delegado de processamento de eventos. Portanto, você converterá recursos de linguagem como esse novamente na operação fundamental que está sendo executada (registro de evento, neste exemplo).
- [**Portar a sintaxe da linguagem**](#port-language-syntax). Muitas dessas alterações são transformações mecânicas simples, com a substituição de um símbolo por outro. Por exemplo, alteração do ponto (`.`) para dois-pontos (`::`).
- [**Portar o procedimento da linguagem**](#port-language-procedure). Algumas dessas podem ser alterações simples e repetitivas (como `myObject.MyProperty` para `myObject.MyProperty()`). Outras precisam de alterações mais profundas (por exemplo, portar um procedimento que envolve o uso de **System.Text.StringBuilder** para um que envolve o uso de **std::wostringstream**).
- [**Tarefas relacionadas à portabilidade que são específicas para o C++/WinRT**](#porting-related-tasks-that-are-specific-to-cwinrt). Alguns detalhes do Windows Runtime são resolvidos implicitamente pelo C#, nos bastidores. Esses detalhes são realizados explicitamente no C++/WinRT. Um exemplo disso é que você usa um arquivo `.idl` para definir as classes de runtime.

O restante deste tópico é estruturado de acordo com essa taxonomia.

## <a name="port-the-language-projection"></a>Portar a projeção de linguagem

||C#|C++/WinRT|Veja também|
|-|-|-|-|
|Objeto não tipado|`object` ou [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[Como portar o método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Namespaces de projeção|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|Tamanho de uma coleção|`collection.Count`|`collection.Size()`|[Como portar o método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Coleção somente leitura|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[Como portar o método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Delegado do manipulador de eventos como membro de classe|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[Como portar o método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Revogar delegado do manipulador de eventos|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[Como portar o método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Contêiner associativo|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|Acesso de membro de vetor|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>Registrar/revogar um manipulador de eventos

No C++/WinRT, há várias opções sintáticas para registrar/revogar um delegado de manipulador de eventos, conforme descrito em [Processar eventos usando delegados no C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). Confira também [Como portar o método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications).

Às vezes, por exemplo, quando um destinatário de evento (um objeto que processa um evento) está prestes a ser destruído, o ideal é revogar um manipulador de eventos para que a origem do evento (o objeto que gera o evento) não chame um objeto destruído. Veja [Revogar um delegado registrado](/windows/uwp/cpp-and-winrt-apis/handle-events#revoke-a-registered-delegate). Em casos como esse, crie uma variável de membro **event_token** para os manipuladores de eventos. Para obter um exemplo, confira [Como portar o método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications).

Você também pode registrar um manipulador de eventos na marcação XAML.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

No C#, o método **OpenButton_Click** pode ser privado e o XAML ainda poderá conectá-lo ao evento [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) gerado por *OpenButton*.

No C++/WinRT, o método **OpenButton_Click** precisará ser público no [tipo de implementação](/windows/uwp/cpp-and-winrt-apis/author-apis) *se você desejar registrá-lo na marcação XAML*. Se você registrar um manipulador de eventos somente no código imperativo, o manipulador de eventos não precisará ser público.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Como alternativa, você pode tornar o registro da página XAML um amigo do tipo de implementação e o **OpenButton_Click** privado.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Um último cenário é quando o projeto C# que você está portando se *associa* ao manipulador de eventos da marcação (para obter mais informações sobre esse cenário, confira [Funções em x:Bind](/windows/uwp/data-binding/function-bindings)).

```xaml
<Button x:Name="OpenButton" Click="{x:Bind OpenButton_Click}" />
```

Você pode alterar essa marcação para o `Click="OpenButton_Click"` mais simples. Ou, caso prefira, você pode manter essa marcação como está. Tudo o que você precisa fazer para dar suporte a ela é declarar o manipulador de eventos em IDL.

```idl
void OpenButton_Click(Object sender, Windows.UI.Xaml.RoutedEventArgs e);
```

> [!NOTE]
> Declare a função como `void` mesmo se você *implementá-la* como [Disparar e esquecer](/windows/uwp/cpp-and-winrt-apis/concurrency-2#fire-and-forget).

## <a name="port-language-syntax"></a>Portar a sintaxe da linguagem

||C#|C++/WinRT|Veja também|
|-|-|-|-|
|Modificadores de acesso|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[Como portar o método **Button_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#button_click)|
|Ação assíncrona|`async Task ...`|`IAsyncAction ...`||
|Operação assíncrona|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|Método do tipo disparar e esquecer (implica uma operação assíncrona)|`async void ...`|`winrt::fire_and_forget ...`|[Como portar o método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Espera cooperativa|`await ...`|`co_await ...`|[Como portar o método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Acessar uma constante enumerada|`E.Value`|`E::Value`|[Como portar o método **DisplayChangedFormats**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats)|
|Separador de namespace|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[Como portar o método **UpdateStatus**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|Declaração de parâmetro para um método|`MyType`|`MyType const&`|[Passagem de parâmetro](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|Declaração de parâmetro para um método assíncrono|`MyType`|`MyType`|[Passagem de parâmetro](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|Chamar um método estático|`T.Method()`|`T::Method()`||
|Cadeias de caracteres|`string` ou **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[Processamento da cadeia de caracteres em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings)|
|Cadeia de caracteres literal|`"a string literal"`|`L"a string literal"`|[Como portar o construtor, **Current** e **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Literal de cadeia de caracteres textual/bruta|`@"verbatim string literal"`|`LR"(raw string literal)"`|[Como portar o método **DisplayToast**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast)|
|Acessar um membro de dados|`this.variable`|`this->variable`||
|Diretiva using|`using A.B.C;`|`using namespace A::B::C;`|[Como portar o construtor, **Current** e **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Tipo inferido (ou deduzido)|`var`|`auto`|[Como portar o método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|

> [!NOTE]
> Se um arquivo de cabeçalho não contiver uma diretiva `using namespace` para determinado namespace, você precisará qualificar totalmente todos os nomes de tipos desse namespace ou, pelo menos, qualificá-los suficientemente para que o compilador os encontre. Para obter um exemplo, confira [Como portar o método **DisplayToast**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast).

### <a name="porting-classes-and-members"></a>Como portar classes e membros

Você precisará decidir, para cada tipo C#, se desejará portá-lo para um tipo do Windows Runtime ou para uma classe/um struct/uma numeração C++ normal. Para obter mais informações e exemplos detalhados que ilustram como tomar essas decisões, confira [Como portar a amostra de área de transferência para o C++/WinRT do C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp).

Uma propriedade C# normalmente se torna uma função de acessador, uma função de modificador e um membro de dados de backup. Para obter mais informações e um exemplo, confira [Como portar a propriedade **IsClipboardContentChangedEnabled**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#isclipboardcontentchangedenabled).

Para campos não estáticos, torne-os membros de dados do [tipo de implementação](/windows/uwp/cpp-and-winrt-apis/author-apis).

Um campo estático C# torna-se um acessador estático e/ou uma função de modificador C++/WinRT. Para obter mais informações e um exemplo, confira [Como portar o construtor, **Current** e **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name).

Para as funções de membro, novamente, você precisará decidir para cada uma delas se pertencem ou não à IDL ou se elas são uma função de membro pública ou privada do tipo de implementação. Para obter mais informações e exemplos de como tomar essa decisão, confira [IDL para o tipo **MainPage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type).

### <a name="porting-xaml-markup-and-asset-files"></a>Como portar a marcação XAML e os arquivos de ativos

No caso de [Como portar a amostra de área de transferência para o C++/WinRT do C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp), pudemos usar *a mesma* marcação XAML (incluindo os recursos) e os arquivos de ativos em todo o projeto C# e C++/WinRT. Em alguns casos, as edições na marcação serão necessárias para conseguir isso. Confira [Copiar o XAML e os estilos necessários para concluir a portabilidade de **MainPage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage).

## <a name="port-language-procedure"></a>Portar o procedimento de linguagem

||C#|C++/WinRT|Veja também|
|-|-|-|-|
|Gerenciamento do tempo de vida em um método assíncrono|N/D|`auto lifetime{ get_strong() };` ou<br>`auto lifetime = get_strong();`|[Como portar o método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Descarte|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[Como portar o método **CopyImage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copyimage)|
|Objeto de constructo|`new MyType(args)`|`MyType{ args }` ou<br>`MyType(args)`|[Como portar a propriedade **Scenarios**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios)|
|Criar uma referência não inicializada|`MyType myObject;`|`MyType myObject{ nullptr };` ou<br>`MyType myObject = nullptr;`|[Como portar o construtor, **Current** e **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Objeto de constructo em variável com argumentos|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` ou <br>`auto myObject{ MyType(args) };` ou <br>`auto myObject = MyType{ args };` ou <br>`auto myObject = MyType(args);` ou <br>`MyType myObject{ args };` ou <br>`MyType myObject(args);`|[Como portar o método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|Objeto de constructo em variável sem argumentos|`var myObject = new T();`|`MyType myObject;`|[Como portar o método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Inicialização abreviada de objeto|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|Operação de vetor em massa|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[Como portar o método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Iterar na coleção|`foreach (var v in c)`|`for (auto&& v : c)`|[Como portar o método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Capturar uma exceção|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[Como portar o método **PasteButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Detalhes da exceção|`ex.Message`|`ex.message()`|[Como portar o método **PasteButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Obtém um valor da propriedade|`myObject.MyProperty`|`myObject.MyProperty()`|[Como portar o método **NotifyUser**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser)|
|Definir um valor da propriedade|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|Incrementar um valor da propriedade|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>Para cadeias de caracteres, alterne para um construtor||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|Cadeia de caracteres de linguagem para a cadeia de caracteres do Windows Runtime|N/D|`winrt::hstring{ s }`||
|Construção da cadeia de caracteres|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[Construção da cadeia de caracteres](#string-building)|
|Interpolação de cadeia de caracteres|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) e/ou [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[Como portar o método **OnNavigatedTo**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto)|
|Cadeia de caracteres vazia para comparação|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[Como portar o método **UpdateStatus**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|Operações de dicionário|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|Conversão de tipo (geração em caso de falha)|`(MyType)v`|`v.as<MyType>()`|[Como portar o método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|Conversão de tipo (nula em caso de falha)|`v as MyType`|`v.try_as<MyType>()`|[Como portar o método **PasteButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Elementos XAML com x:Name são propriedades|`MyNamedElement`|`MyNamedElement()`|[Como portar o construtor, **Current** e **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Alternar para o thread da IU|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** ou [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[Como portar o método **NotifyUser**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser) e [Como portar o método **HistoryAndRoaming**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#historyandroaming)|

As seções a seguir fornecem mais detalhes em relação a alguns dos itens da tabela.

### <a name="tostring"></a>ToString()

Os tipos C# fornecem o método [Object.ToString](/dotnet/api/system.object.tostring).

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

O C++/ WinRT não fornece diretamente esse recurso, mas você poderá usar alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

O C++/WinRT também dá suporte a [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) em um número limitado de tipos. Você precisará adicionar sobrecargas para os tipos adicionais que deseja converter em cadeia de caracteres.

| Language | Converter int em cadeia de caracteres | Converter uma enumeração em cadeia de caracteres |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

No caso da conversão de uma enumeração em cadeia de caracteres, você precisará fornecer a implementação de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Em geral, essas conversões em cadeia de caracteres são consumidas implicitamente pela vinculação de dados.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Essas associações executarão **winrt::to_hstring** da propriedade associada. No caso do segundo exemplo (o **StatusEnum**), você precisará fornecer sua própria sobrecarga de **winrt::to_hstring**; caso contrário, obterá um erro do compilador.

Confira também [Como portar o método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click).

### <a name="string-building"></a>Construção da cadeia de caracteres

Para a construção de cadeia de caracteres, o C# tem um tipo [**StringBuilder**](/dotnet/api/system.text.stringbuilder) interno.

| | C# | C++/WinRT |
|-|-|-|
| Construção da cadeia de caracteres | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| Acrescentar uma cadeia de caracteres do Windows Runtime preservando os valores nulos | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| Adicionar uma nova linha |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| Acessar o resultado | `s = builder.ToString();` | `ws = builder.str();` |

Confira também [Como portar o método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring) e [Como portar o método **DisplayChangedFormats**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats).

## <a name="porting-related-tasks-that-are-specific-to-cwinrt"></a>Tarefas relacionadas à portabilidade que são específicas para o C++/WinRT

### <a name="define-your-runtime-classes-in-idl"></a>Definir as classes de runtime na IDL

Confira [IDL para o tipo **MainPage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type) e [Consolidar os arquivos `.idl`](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#consolidate-your-idl-files).

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>Incluir os arquivos de cabeçalho do namespace C++/WinRT do Windows de que você precisa

No C++/WinRT, sempre que desejar usar um tipo de um namespace do Windows, inclua o arquivo de cabeçalho do namespace C++/WinRT do Windows correspondente. Para obter um exemplo, confira [Como portar o método **NotifyUser**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser).

### <a name="boxing-and-unboxing"></a>Conversão boxing e unboxing

O C# faz a conversão boxing automática de escalares em objetos. O C++/WinRT exige que você chame explicitamente a função [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value). Ambas as linguagens exigem que você faça a conversão unboxing explicitamente. Confira [Conversão boxing e unboxing com o C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

Nas tabelas a seguir, usaremos estas definições.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Operação | C# | C++/WinRT|
|-|-|-|
| Conversão boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Conversão unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

O C++/CX e o C# gerarão exceções se você tentar fazer a conversão unboxing de um ponteiro nulo em um tipo de valor. O C++/WinRT considera isso um erro de programação e falha. No C++/WinRT, use a função [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) se desejar lidar com o caso em que o objeto não é do tipo que você pensou que fosse.

| Cenário | C# | C++/WinRT|
|-|-|-|
| Fazer a conversão unboxing de um inteiro conhecido |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Se o for nulo | `System.NullReferenceException` | Falha |
| Se o não for um int convertido | `System.InvalidCastException` | Falha |
| Fazer a conversão unboxing de int, se for nulo, usar fallback; falhar, em qualquer outra situação | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Fazer a conversão unboxing de int, se possível; usar fallback para qualquer outra situação | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

Para obter um exemplo, confira [Como portar o método **OnNavigatedTo**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto) e [Como portar o método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click).

#### <a name="boxing-and-unboxing-a-string"></a>Conversão boxing e unboxing de uma cadeia de caracteres

Uma cadeia de caracteres é, de algumas maneiras, um tipo de valor e, de outras, um tipo de referência. O C# e o C++/WinRT tratam as cadeias de caracteres de maneira diferente.

O tipo do ABI [**HSTRING**](/windows/win32/winrt/hstring) é um ponteiro para uma cadeia de caracteres de contagem de referências. Mas ele não é derivado de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), portanto, não é tecnicamente um *objeto*. Além disso, um **HSTRING** nulo representa a cadeia de caracteres vazia. É feita a conversão boxing de coisas não derivadas de **IInspectable** por meio do encapsulamento delas dentro de um [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_). Além disso, o Windows Runtime fornece uma implementação padrão na forma do objeto [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (tipos personalizados são relatados como [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

O C# representa uma cadeia de caracteres do Windows Runtime como um tipo de referência, enquanto o C++/WinRT projeta uma cadeia de caracteres como um tipo de valor. Isso significa que uma cadeia de caracteres nula convertida pode ter diferentes representações, dependendo do procedimento adotado.

| Comportamento | C# | C++/WinRT|
|-|-|-|
| Declarações | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| Categoria de tipo de cadeia de caracteres | Tipo de referência | Tipo de valor |
| projetos **HSTRING** nulos como | `""` | `hstring{}` |
| São nulos e idênticos `""`? | Não | Sim |
| Validade de nulo | `s = null;`<br>`s.Length` gera NullReferenceException | `s = hstring{};`<br>`s.size() == 0` (válido) |
| Se você atribuir uma cadeia de caracteres nula ao objeto | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Se você atribuir `""` ao objeto | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Conversões boxing e unboxing básicas.

| Operação | C# | C++/WinRT|
|-|-|-|
| Fazer a conversão boxing de uma cadeia de caracteres | `o = s;`<br>A cadeia de caracteres vazia se torna um objeto não nulo. | `o = box_value(s);`<br>A cadeia de caracteres vazia se torna um objeto não nulo. |
| Fazer a conversão unboxing de uma cadeia de caracteres conhecida | `s = (string)o;`<br>O objeto nulo torna-se uma cadeia de caracteres nula.<br>InvalidCastException se não for uma cadeia de caracteres. | `s = unbox_value<hstring>(o);`<br>O objeto nulo falha.<br>Falha se não for uma cadeia de caracteres. |
| Fazer a conversão unboxing de uma possível cadeia de caracteres | `s = o as string;`<br>Objeto nulo ou não cadeia de caracteres se torna uma cadeia de caracteres nula.<br><br>OU<br><br>`s = o as string ?? fallback;`<br>Nulo ou não cadeia de caracteres se torna fallback.<br>Cadeia de caracteres vazia preservada. | `s = unbox_value_or<hstring>(o, fallback);`<br>Nulo ou não cadeia de caracteres se torna fallback.<br>Cadeia de caracteres vazia preservada. |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>Como disponibilizar uma classe para a extensão de marcação {Binding}

Se você pretende usar a extensão de marcação {Binding} para associar dados ao tipo de dados, confira [Objeto de associação declarado usando {Binding}](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

### <a name="consuming-objects-from-xaml-markup"></a>Como consumir objetos por meio da marcação XAML

Em um projeto C#, você pode consumir membros privados e elementos nomeados por meio da marcação XAML. Mas no C++/WinRT, todas as entidades consumidas com o uso da [**extensão de marcação XAML {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) precisam ser expostas publicamente em IDL.

Além disso, a associação a um booliano exibe `true` ou `false` no C#, mas mostra **Windows.Foundation.IReference`1\<Boolean\>** no C++/WinRT.

Para obter mais informações e exemplos de código, confira [Como consumir objetos para marcação](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

### <a name="making-a-data-source-available-to-xaml-markup"></a>Como disponibilizar uma fonte de dados para marcação XAML

No C++/WinRT versão 2.0.190530.8 e posterior, [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) cria um vetor observável que dá suporte a **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** e **IObservableVector\<IInspectable\>** . Para obter um exemplo, confira [Como portar a propriedade **Scenarios**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios).

Você pode criar o **arquivo MIDL (.idl)** desta forma (confira também [Como fatorar classes de runtime em arquivos MIDL (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Depois, implemente-o desta forma.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Para obter informações, confira [Controles de itens XAML; associação a uma coleção do C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection) e [Coleções com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/collections).

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Como disponibilizar uma fonte de dados para marcação XAML (antes do C++/WinRT 2.0.190530.8)

A vinculação de dados XAML exige que uma fonte de itens implemente **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** , bem como uma das combinações de interfaces a seguir.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** e **INotifyCollectionChanged**
- **IBindableVector** e **IBindableObservableVector**
- **IBindableVector** por si só (não responderá a alterações)
- **IVector\<IInspectable\>**
- **IBindableIterable** (iterará e salvará elementos em uma coleção particular)

Uma interface genérica, como **IVector\<T\>** , não pode ser detectada em runtime. Cada **IVector\<T\>** tem um IID (identificador de interface) diferente, que é uma função de **T**. Qualquer desenvolvedor pode expandir o conjunto de **T** de forma arbitrária e, portanto, é evidente que o código de associação XAML nunca pode saber o conjunto completo a ser consultado. Essa restrição não é um problema para o C# porque cada objeto CLR que implementa **IEnumerable\<T\>** implementa **IEnumerable** automaticamente. No nível da ABI, isso significa que cada objeto que implementa **IObservableVector\<T\>** implementa **IObservableVector\<IInspectable\>** automaticamente.

O C++/WinRT não oferece essa garantia. Se uma classe de runtime do C++/WinRT implementa **IObservableVector\<T\>** , não podemos pressupor que uma implementação de **IObservableVector\<IInspectable\>** também seja fornecida.

Consequentemente, veja abaixo como o exemplo anterior precisará ser examinado.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Veja também a implementação.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Caso você precise acessar objetos em *m_bookSkus*, precisará executar o QI novamente para **Bookstore::BookSku**.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>Classes derivadas

Para derivação de uma classe de runtime, a classe base precisa ser *combinável*. O C# não exige que você execute nenhuma etapa especial para tornar suas classes combináveis, ao contrário do C++/WinRT. Use a [palavra-chave não selada](/uwp/midl-3/intro#base-classes) para indicar que deseja que a classe seja utilizável como uma classe base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

Na classe do cabeçalho do [tipo de implementação](/windows/uwp/cpp-and-winrt-apis/author-apis), inclua o arquivo de cabeçalho da classe base antes de incluir o cabeçalho gerado automaticamente da classe derivada. Caso contrário, você receberá erros como "uso ilícito deste tipo como uma expressão".

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>APIs importantes
* [namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [Tutoriais do C#](/visualstudio/get-started/csharp)
* [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)
* [Vinculação de dados em detalhes](/windows/uwp/data-binding/data-binding-in-depth)