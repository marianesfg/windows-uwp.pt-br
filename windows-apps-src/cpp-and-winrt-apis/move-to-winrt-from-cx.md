---
description: Este tópico mostra como fazer a portabilidade do código C++/CX para seu equivalente no C++/WinRT.
title: Mover do C++/CX para C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, portabilidade, migrar, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 92088906078a3a705e5fae052a50fc914561c77c
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393457"
---
# <a name="move-to-cwinrt-from-ccx"></a>Mover do C++/CX para C++/WinRT

Este tópico mostra como fazer a portabilidade do código em um projeto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) para seu equivalente no [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Estratégias de portabilidade

É possível fazer a portabilidade gradualmente do código C++/CX para C++/WinRT, caso você queira. Os códigos C++/CX e C++/WinRT podem coexistir no mesmo projeto, com exceção do suporte ao compilador XAML e dos componentes do Windows Runtime. Para essas duas exceções, é preciso definir se o escopo será o C++/CX ou C++/WinRT dentro do mesmo projeto.

> [!IMPORTANT]
> Se seu projeto cria um aplicativo XAML, um fluxo de trabalho recomendável é primeiro criar um novo projeto no Visual Studio usando um dos modelos de projeto C++/WinRT (confira o [Suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Depois, comece a copiar o código-fonte e a marcação do projeto C++/CX. Você pode adicionar novas páginas XAML em **Projeto** \> **Adicionar Novo Item...** \>**Visual C++**  > **Página em Branco (C++/WinRT)** .
>
> Como alternativa, é possível usar um componente do Windows Runtime para fatorar o código do projeto XAML C++/CX à medida que você faz a portabilidade. Mova o máximo do código C++/CX que conseguir para um componente e, em seguida, altere o projeto XAML para C++/WinRT. Ou deixe o projeto XAML como C++/CX, crie um novo componente C++/WinRT e comece a fazer a portabilidade do código C++/CX do projeto XAML para dentro do componente. Você também pode ter um projeto de componente C++/CX em conjunto com um projeto de componente C++/WinRT dentro da mesma solução, fazer referência a ambos no seu projeto de aplicativo e realizar a portabilidade gradual de um para o outro. Confira [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md) para obter mais detalhes sobre como usar as duas projeções de linguagem no mesmo projeto.

> [!NOTE]
> O [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e o SDK do Windows declaram tipos no namespace raiz **Windows**. Um tipo do Windows projetado no C++/WinRT tem o mesmo nome totalmente qualificado como o tipo do Windows, mas ele é colocado no namespace C++ **winrt**. Esses namespaces distintos permitem que você faça a portabilidade do C++/CX para o C++/WinRT em seu próprio ritmo.

Tendo em mente as exceções mencionadas acima, a primeira etapa da portabilidade de um projeto C++/CX para C++/WinRT é adicionar manualmente o suporte ao C++/WinRT (confira [Suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para fazer isso, instale o [pacote NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) em seu projeto. Abra o projeto no Visual Studio, clique em **Projeto** \> **Gerenciar Pacotes NuGet...** \> **Procurar**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e clique em **Instalar** para instalar o pacote do projeto. Um efeito dessa alteração é que o suporte para C++/CX é desativado no projeto. É recomendado deixar o suporte desativado para que as mensagens de build possam ajudar a encontrar (e transferir) todas as dependências no C++/CX; ou você pode ativar o suporte (nas propriedades do projeto, **C/C++** \>**Geral**\>**Consumir a extensão do Tempo de Execução do Windows**\>**Sim (/ZW)** ) e fazer a portabilidade de forma gradual.

Como alternativa, adicione manualmente a propriedade a seguir ao arquivo `.vcxproj` usando a página de propriedades do projeto C++/WinRT no Visual Studio. Para obter uma lista de opções de personalização semelhantes (que ajustam o comportamento da ferramenta `cppwinrt.exe`), confira o [arquivo Leiame](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) do pacote NuGet Microsoft.Windows.CppWinRT.

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

Em seguida, verifique se a propriedade do projeto **Geral** \> **Versão da Plataforma de Destino** está definida como 10.0.17134.0 (Windows 10, versão 1803) ou posterior.

No arquivo de cabeçalho pré-compilado (normalmente, `pch.h`), inclua `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Se você incluir qualquer cabeçalho de API C++/WinRT projetado do Windows (por exemplo, `winrt/Windows.Foundation.h`), não será necessário incluir explicitamente `winrt/base.h` dessa forma, pois será incluído automaticamente.

Caso o projeto também use os tipos da [WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows)](/cpp/windows/windows-runtime-cpp-template-library-wrl), confira como [mover para o C++/WinRT a partir da WRL](move-to-winrt-from-wrl.md).

## <a name="file-naming-conventions"></a>Convenções de nomenclatura de arquivo

### <a name="xaml-markup-files"></a>Arquivos de marcação XAML

| | C++/CX | C++/WinRT |
| - | - | - |
| **Arquivos XAML do desenvolvedor** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl (veja abaixo) |
| **Arquivos XAML gerados** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

Observe que o C++/WinRT remove o `.xaml` dos nomes de arquivo `*.h` e `*.cpp`.

O C++/WinRT adiciona um arquivo de desenvolvedor adicional, o **arquivo MIDL (.idl)** . O C++/CX gera de forma automática esse arquivo internamente, adicionando a ele todos os membros públicos e protegidos. No C++/WinRT, você mesmo adiciona e cria o arquivo. Para obter mais detalhes, exemplos de código e um passo a passo da criação de IDL, confira [Controles XAML; associação a uma propriedade do C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property).

Confira também [Como fatorar classes de tempo de execução em arquivos MIDL (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)

### <a name="runtime-classes"></a>Classes de tempo de execução

O C++/CX não impõe restrições quanto aos nomes dos arquivos de cabeçalho; é comum colocar várias definições de classe de tempo de execução em um único arquivo de cabeçalho, especialmente para classes pequenas. Mas o C++/WinRT exige que cada classe de tempo de execução tenha seu próprio arquivo de cabeçalho com o mesmo nome de classe. 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

Menos comum (mas ainda válido) no C++/CX é usar arquivos de cabeçalho nomeados de forma diferente para controles personalizados XAML. Você precisará renomear esse arquivo de cabeçalho para que ele corresponda ao nome de classe.

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>Requisitos do arquivo de cabeçalho

O C++/CX não exige que você inclua nenhum arquivo de cabeçalho especial, pois ele gera de forma automática arquivos de cabeçalho com base em arquivos `.winmd` internamente. No C++/CX, é comum usar diretivas `using` para namespaces consumidos por nome.

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

A diretiva `using namespace Windows::Media::Playback` nos permite escrever `MediaPlaybackItem` sem um prefixo de namespace. Também alteramos o namespace `Windows.Media.Core`, porque `item->VideoTracks->GetAt(0)` retorna um **Windows.Media.Core.VideoTrack**. Mas não precisamos digitar o nome **VideoTrack** em nenhum lugar, portanto, não precisamos de uma diretiva `using Windows.Media.Core`.

Mas o C++/WinRT exige que você inclua um arquivo de cabeçalho correspondente a cada namespace consumido, mesmo que você não o nomeie.

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

Por outro lado, embora o evento **MediaPlaybackItem.AudioTracksChanged** seja do tipo **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** , não precisamos incluir `winrt/Windows.Foundation.Collections.h` porque não usamos esse evento.

O C++/WinRT também exige que você inclua arquivos de cabeçalho para namespaces consumidos pela marcação XAML.

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

O uso da classe **Rectangle** significa que você precisa adicionar essa inclusão.

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

Se você esquecer um arquivo de cabeçalho, tudo será compilado corretamente, mas você obterá erros de vinculador porque as classes `consume_` estão ausentes.

## <a name="parameter-passing"></a>Passagem de parâmetro

Ao escrever o código-fonte do C++/CX, você passa os tipos C++/CX como parâmetros de função como referência de circunflexo (\^).

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

No C++/WinRT, para funções síncronas, você precisa usar parâmetros `const&` por padrão. Isso evita cópia e sobrecarga encaixada. Entretanto, as corrotinas precisam usar passar-por-valor para garantir a captura pelo valor e evitar problemas de tempo de vida (para ver mais detalhes, confira [Operações assíncronas e simultaneidade com C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Um objeto C++/WinRT é fundamentalmente um valor que mantém um ponteiro da interface para o objeto do Tempo de Execução do Windows subjacente. Quando você copia um objeto C++/WinRT, o compilador copia o ponteiro da interface encapsulado, aumentando sua contagem de referência. A consequente destruição da cópia envolve reduzir a contagem de referência. Portanto, apenas incorre na sobrecarga de uma cópia quando necessário.

## <a name="variable-and-field-references"></a>Referências de variáveis e campo

Ao escrever o código-fonte do C++/CX, você usa as variáveis de circunflexo (\^) para fazer referência a objetos do Tempo de Execução do Windows e de operador seta (-&gt;) para desreferenciar uma variável desse tipo.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Ao fazer a portabilidade para o código C++/WinRT equivalente, você gastar um bom tempo removendo os circunflexos e alterando o operador seta (-&gt;) para o operador ponto (.). Os tipos projetados de C++/ WinRT são valores, e não ponteiros.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

O construtor padrão para uma referência de ângulo de visão do C++/CX inicializa-o como nulo. Aqui está um exemplo de código C++/CX em que criamos uma variável/campo do tipo correto, mas que não foi inicializada. Em outras palavras, ela não se refere inicialmente a um **TextBlock**, e pretendemos atribuir a referência mais tarde.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Para o equivalente no C++/WinRT, confira [Inicialização atrasada](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Propriedades

A extensões de linguagem C++/CX incluem o conceito de propriedades. Ao escrever o código-fonte do C++/CX, você pode acessar uma propriedade como se fosse um campo. O C++ padrão não tem o conceito de propriedade, portanto, no C++/WinRT, você pode obter e definir funções.

Nos exemplos a seguir, **XboxUserId**, **UserState**, **PresenceDeviceRecords** e **Size** são propriedades.

### <a name="retrieving-a-value-from-a-property"></a>Recuperar o valor de uma propriedade

Veja como obter o valor de uma propriedade no C++/CX.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

O código-fonte do C++/WinRT equivalente chama uma função com o mesmo nome da propriedade, mas sem parâmetros.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Observe que a função **PresenceDeviceRecords** retorna um objeto do Tempo de Execução do Windows com uma função **Size**. Como o objeto retornado também é um tipo projetado do C++/WinRT, desreferenciamos usando o operador ponto para chamar **Size**.

### <a name="setting-a-property-to-a-new-value"></a>Definir uma propriedade como um novo valor

Uma propriedade é definida com um novo valor de forma semelhante. Primeiro, no C++/CX.

```cppcx
record->UserState = newValue;
```

Para fazer o equivalente no C++/WinRT, chame uma função com o mesmo nome da propriedade e passe um argumento.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Criar a instância de uma classe

Trabalhe com o objeto do C++/CX por meio de um identificador, conhecido como uma referência de circunflexo (\^). Crie um novo objeto por meio da palavra-chave `ref new`, que, por sua vez, chama [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para ativar uma nova instância da classe do tempo de execução.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Um objeto do C++/WinRT é um valor; portanto, você pode alocá-lo na pilha ou como um campo de um objeto. *Nunca* use `ref new` (nem `new`) para alocar um objeto do C++/WinRT. Nos bastidores, **RoActivateInstance** ainda está sendo chamado.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Se um recurso foi dispendioso para inicializar, é comum atrasar a inicialização até que ela seja realmente necessária. Como já mencionado, o construtor padrão para uma referência de ângulo de visão do C++/CX inicializa-o como nulo.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

O mesmo código portado para o C++/WinRT. Observe o uso do construtor **std::nullptr_t**. Para obter mais informações sobre esse construtor, confira [Inicialização atrasada](consume-apis.md#delayed-initialization).

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="how-the-default-constructor-affects-collections"></a>Como o construtor padrão afeta as coleções

Os tipos de coleção C++ usam o construtor padrão, o que pode resultar na construção de um objeto indesejado.

| Cenário | C++/CX | C++/WinRT (incorreto) | C++/WinRT (correto) |
| - | - | - | - |
| Variável local, inicialmente vazia | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| Variável de membro, inicialmente vazia | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| Variável global, inicialmente vazia | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| Vetor de referências vazias | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| Definir um valor em um mapa | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| Matriz de referências vazias | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| Emparelhar | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

Não há nenhum atalho para a criação de uma matriz de referências vazias. Você precisa repetir `nullptr` para cada elemento da matriz. Se você tiver poucos, os extras serão construídos por padrão.

### <a name="more-about-the-stdmap-example"></a>Mais sobre o exemplo de **std::map**

O operador subscrito `[]` para **std::map** se comporta desta forma.

- Se a chave for encontrada no mapa, retorne uma referência ao valor existente (que poderá ser substituído).
- Se a chave não for encontrada no mapa, crie uma entrada no mapa que consiste na chave (movida, se móvel) e *um valor construído padrão* e retorne uma referência ao valor (que você poderá então substituir).

Em outras palavras, o operador `[]` sempre cria uma entrada no mapa. Isso é diferente de C#, Java e JavaScript.

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Converter de uma classe base de tempo de execução para uma derivada

É comum ter uma referência à base que você sabe que se refere a um objeto de um tipo derivado. No C++/CX, use `dynamic_cast` para *converter* a referência à base em uma referência para derivado. O `dynamic_cast` é apenas uma chamada oculta para [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Aqui está um exemplo típico em que &mdash;você está manipulando um evento de alteração de propriedade de dependência e deseja converter de **DependencyObject** de volta para o tipo real que possui a propriedade da dependência.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

O código C++/WinRT equivalente substitui o `dynamic_cast` por uma chamada para a função [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function), que encapsula **QueryInterface**. Em vez disso, você também tem a opção de chamar [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), que gerará uma exceção se a consulta para a interface necessária (a interface padrão do tipo que você está solicitando) não for retornada. Veja aqui um exemplo de código C++/WinRT.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="derived-classes"></a>Classes derivadas

Para derivação de uma classe de tempo de execução, a classe base precisa ser *combinável*. O C++/CX não exige que você execute nenhuma etapa especial para tornar suas classes combináveis, ao contrário do C++/WinRT. Use a [palavra-chave não selada](/uwp/midl-3/intro#base-classes) para indicar que deseja que a classe seja utilizável como uma classe base.

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

Na classe do cabeçalho de implementação, você precisa incluir o arquivo de cabeçalho da classe base antes de incluir o cabeçalho gerado automaticamente da classe derivada. Caso contrário, você receberá erros como "uso ilícito deste tipo como uma expressão".

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

## <a name="event-handling-with-a-delegate"></a>Processamento de eventos com um delegado

Veja um exemplo típico de processamento de eventos no C++/CX usando uma função lambda como delegado nesse caso.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Esse é o equivalente no C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Em vez de uma função lambda, você pode optar por implementar o delegado como uma função livre como uma função de ponteiro para membro. Para obter mais informações, veja como [manipular eventos usando delegados no C++/WinRT](handle-events.md).

Se estiver fazendo a portabilidade de uma base de código C++/CX em que os eventos e delegados são usados internamente (e não em binários), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) ajudará a replicar esse padrão no C++/WinRT. Confira também [Delegados parametrizados, sinais simples e retornos de chamada dentro de um projeto](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Revogar um delegado

No C++/CX, use o operador `-=` para revogar o registro de um evento anterior.

```cppcx
myButton->Click -= token;
```

Esse é o equivalente no C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Para obter mais informações e opções, confira [Revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate).

## <a name="boxing-and-unboxing"></a>Conversão boxing e unboxing

O C++/CX faz a conversão boxing automática de escalares em objetos. O C++/WinRT exige que você chame explicitamente a função [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value). Ambas as linguagens exigem que você faça a conversão unboxing explicitamente. Confira [Conversão boxing e unboxing com o C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

Nas tabelas a seguir, usaremos estas definições.

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| Operação | C++/CX | C++/WinRT|
|-|-|-|-|
| Conversão boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Conversão unboxing | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

O C++/CX e o C# gerarão exceções se você tentar fazer a conversão unboxing de um ponteiro nulo em um tipo de valor. O C++/WinRT considera isso um erro de programação e falha. No C++/WinRT, use a função [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) se desejar lidar com o caso em que o objeto não é do tipo que você pensou que fosse.

| Cenário | C++/CX | C++/WinRT|
|-|-|-|-|
| Fazer a conversão unboxing de um inteiro conhecido | `i = (int)o;` | `i = unbox_value<int>(o);` |
| Se o for nulo | `Platform::NullReferenceException` | Falha |
| Se o não for um int convertido | `Platform::InvalidCastException` | Falha |
| Fazer a conversão unboxing de int, se for nulo, usar fallback; falhar, em qualquer outra situação | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Fazer a conversão unboxing de int, se possível; usar fallback para qualquer outra situação | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Conversão boxing e unboxing de uma cadeia de caracteres

Uma cadeia de caracteres é, de algumas maneiras, um tipo de valor e, de outras, um tipo de referência. O C++/CX e o C++/WinRT tratam as cadeias de caracteres de maneira diferente.

O tipo do ABI [**HSTRING**](/windows/win32/winrt/hstring) é um ponteiro para uma cadeia de caracteres de contagem de referências. Mas ele não é derivado de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), portanto, não é tecnicamente um *objeto*. Além disso, um **HSTRING** nulo representa a cadeia de caracteres vazia. A conversão boxing de itens não derivados de **IInspectable** é feita encapsulando-os dentro de um [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_) e o Windows Runtime fornece uma implementação padrão na forma do objeto [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (os tipos personalizados são relatados como [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

O C++/CX representa uma cadeia de caracteres do Windows Runtime como um tipo de referência, enquanto o C++/WinRT projeta uma cadeia de caracteres como um tipo de valor. Isso significa que uma cadeia de caracteres nula convertida pode ter diferentes representações, dependendo do procedimento adotado.

Além disso, o C++/CX permite desreferenciar uma **String^** nula, caso em que se ela comporta como a cadeia de caracteres `""`.

| Operação | C++/CX | C++/WinRT|
|-|-|-|
| Categoria de tipo de cadeia de caracteres | Tipo de referência | Tipo de valor |
| projetos **HSTRING** nulos como | `(String^)nullptr` | `hstring{}` |
| São nulos e idênticos `""`? | Sim | Sim |
| Validade de nulo | `s = nullptr;`<br>`s->Length == 0` (válido) | `s = nullptr;`<br>`s.size() == 0` (válido) |
| Fazer a conversão boxing de uma cadeia de caracteres | `o = s;` | `o = box_value(s);` |
| Se `s` for `null` | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Se `s` for `""` | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr;` |
| Fazer a conversão boxing de uma cadeia de caracteres preservando nulo | `o = s;` | `o = s.empty() ? nullptr : box_value(s);` |
| Fazer a conversão boxing forçada de uma cadeia de caracteres | `o = PropertyValue::CreateString(s);` | `o = box_value(s);` |
| Fazer a conversão unboxing de uma cadeia de caracteres conhecida | `s = (String^)o;` | `s = unbox_value<hstring>(o);` |
| Se `o` for nulo | `s == nullptr; // equivalent to ""` | Falha |
| Se `o` não for uma cadeia de caracteres convertida | `Platform::InvalidCastException` | Falha |
| Fazer a conversão unboxing da cadeia de caracteres, se for nulo, usar fallback; falhar, em qualquer outra situação | `s = o ? (String^)o : fallback;` | `s = o ? unbox_value<hstring>(o) : fallback;` |
| Fazer a conversão unboxing da cadeia de caracteres, se possível; usar fallback para qualquer outra situação | `auto box = dynamic_cast<IBox<String^>^>(o);`<br>`s = box ? box->Value : fallback;` | `s = unbox_value_or<hstring>(o, fallback);` |

Nos dois casos de *conversão unboxing com fallback* acima, é possível que seja feita a conversão boxing forçada de uma cadeia de caracteres nula, caso em que o fallback não será usado. O valor resultante será uma cadeia de caracteres vazia porque isso é o que estava na caixa.

## <a name="concurrency-and-asynchronous-operations"></a>Simultaneidade e operações assíncronas

A PPL (Biblioteca de Padrões Paralelos) ([**concurrency::task**](/cpp/parallel/concrt/reference/task-class), por exemplo) foi atualizada para dar suporte às referências de ângulo de visão do C++/CX.

Para o C++/WinRT, você deverá usar corrotinas e `co_await`. Para saber mais e obter exemplos de código, confira [Simultaneidade e operações assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

## <a name="consuming-objects-from-xaml-markup"></a>Como consumir objetos por meio da marcação XAML

Em um projeto C++/CX, você pode consumir membros privados e elementos nomeados da marcação XAML. Mas no C++/WinRT, todas as entidades consumidas com o uso da [**extensão de marcação XAML {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) precisam ser expostas publicamente em IDL.

Além disso, a associação a um booliano exibe `true` ou `false` no C++/CX, mas mostra **Windows.Foundation.IReference`1\<Boolean\>** no C++/WinRT.

Para obter mais informações e exemplos de código, confira [Como consumir objetos para marcação](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Mapear os tipos **Platform** do C++/CX para tipos do C++/WinRT

O C++/CX fornece vários tipos de dados no namespace **Platform**. Esses tipos não são do C++ padrão, portanto, podem ser usados somente ao habilitar as extensões de linguagem do Tempo de Execução do Windows (propriedade do projeto do Visual Studio **C/C++**  > **Geral** > **Consumir extensão do Tempo de Execução do Windows** > **Sim (/ZW)** ). A tabela abaixo ajuda a fazer a portabilidade dos tipos **Platform** para seus equivalentes no C++/WinRT. Depois de fazer isso, como o C++/WinRT é o C++ padrão, você poderá desativar a opção `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | Confira [Fazer a portabilidade de **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagile_ref"></a>Fazer a portabilidade de **Platform::Agile\^** para **winrt::agile_ref**

O tipo **Platform:: Agile\^** no C++/CX representa uma classe do Windows Runtime que pode ser acessada de qualquer thread. O equivalente do C++/WinRT é [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

No C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

No C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Fazer a portabilidade de **Platform::Array\^**

As opções incluem o uso de uma lista de inicializadores, um **std::array** ou um **std::vector**. Para obter mais informações e exemplos de código, confira [Listas de inicializadores padrão](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) e [Matrizes e vetores padrão](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresult_error"></a>Fazer a portabilidade de **Platform::Exception\^** para **winrt::hresult_error**

O tipo **Platform::Exception\^** é produzido no C++/CX quando uma API do Tempo de Execução do Windows retorna um HRESULT diferente de S\_OK. O equivalente do C++/WinRT é [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Para fazer a portabilidade para C++/WinRT, altere todos os códigos que usam **Platform::Exception\^** para usarem **winrt::hresult_error**.

No C++/CX.

```cppcx
catch (Platform::Exception^ ex)
```

No C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

O C++/WinRT fornece essas classes de exceção.

| Tipo de exceção | Classe base | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | chamar [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Observe que cada classe (por meio da classe base **hresult_error**) fornece uma função [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function), que retorna o HRESULT do erro, e uma função [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function), que retorna a representação da cadeia de caracteres desse HRESULT.

Veja um exemplo de geração de uma exceção no C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

E o equivalente no C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Fazer a portabilidade de **Platform::Object\^** para **winrt::Windows::Foundation::IInspectable**

Como todos os tipos do C++/WinRT, **winrt::Windows::Foundation::IInspectable** é um tipo de valor. Veja como inicializar uma variável desse tipo como null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Fazer a portabilidade de **Platform::String\^** para **winrt::hstring**

**Platform::String\^** é equivalente ao tipo ABI de HSTRING do Tempo de Execução do Windows. Para o C++/WinRT, o equivalente é [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Porém, com o C++/WinRT, você pode chamar as APIs do Tempo de Execução do Windows usando tipos de cadeia de caracteres longas da Biblioteca Padrão do C++, como **std::wstring**, e/ou literais de cadeias de caracteres longas. Para obter mais detalhes e exemplos de código, confira [Processamento da cadeia de caracteres em C++/WinRT](strings.md).

Com o C++/CX, você pode acessar a propriedade [**Platform::String::Data**](https://docs.microsoft.com/cpp/cppcx/platform-string-class?view=vs-2019#data) para recuperar a cadeia de caracteres como uma matriz **const wchar_t\*** C-style (por exemplo, para passá-lá a **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Para fazer o mesmo com C++/WinRT, você pode usar a função [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) para obter uma versão de cadeia de caracteres C-style terminada em null, assim como de **std::wstring**.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Quando se trata de implementar APIs que recebem ou retornam cadeias de caracteres, normalmente você altera qualquer código do C++/CX que usa **Platform::String\^** para usar **winrt::hstring**.

Veja um exemplo de uma API do C++/CX que recebe a cadeia de caracteres.

```cppcx
void LogWrapLine(Platform::String^ str);
```

Para o C++/WinRT, é possível declarar a API no [MIDL 3.0](/uwp/midl-3) dessa forma.

```idl
// LogType.idl
void LogWrapLine(String str);
```

A cadeia de ferramentas do C++/WinRT gera o código-fonte semelhante a este.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

Os tipos C++/CX fornecem o método [Object::ToString](/cpp/cppcx/platform-object-class#tostring).

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

O C++/ WinRT não fornece diretamente esse recurso, mas você poderá usar alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

O C++/WinRT também dá suporte a [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) em um número limitado de tipos. Você precisará adicionar sobrecargas para os tipos adicionais que deseja converter em cadeia de caracteres.

| Idioma | Converter int em cadeia de caracteres | Converter uma enumeração em cadeia de caracteres |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
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

#### <a name="string-building"></a>Construção da cadeia de caracteres

O C++/CX e o C++/WinRT fazem o adiamento para o **std::wstringstream** padrão para a construção da cadeia de caracteres.

| | C++/CX | C++/WinRT |
|-|-|-|
| Acrescentar cadeia de caracteres, preservando nulos | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| Acrescentar cadeia de caracteres, parar no primeiro nulo | `stream << s->Data();` | `stream << s.c_str();` |
| Extrair resultado | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>Mais exemplos

Nos exemplos abaixo, *ws* é uma variável do tipo **std::wstring**. Além disso, embora o C++/CX possa construir uma **Platform::String** com base em uma cadeia de caracteres de 8 bits, o C++/WinRT não faz isso.

| Operação | C++/CX | C++/WinRT |
| - | - | - |
| Construir uma cadeia de caracteres com base no literal | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| Fazer a conversão de **std::wstring**, preservando nulos | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| Fazer a conversão de **std::wstring**, parar no primeiro nulo | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| Fazer a conversão em **std::wstring**, preservando nulos | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| Fazer a conversão em **std::wstring**, parar no primeiro nulo | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| Passar o literal para o método | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| Passar **std::wstring** para o método | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

## <a name="important-apis"></a>APIs Importantes
* [Modelo de struct winrt::delegate](/uwp/cpp-ref-for-winrt/delegate)
* [struct winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Criar eventos em C++/WinRT](author-events.md)
* [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Manipular eventos usando delegados em C++/WinRT](handle-events.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Referência de linguagem IDL 3.0 da Microsoft](/uwp/midl-3)
* [Mudar do WRL para o C++/WinRT](move-to-winrt-from-wrl.md)
* [Processamento da cadeia de caracteres em C++/WinRT](strings.md)
