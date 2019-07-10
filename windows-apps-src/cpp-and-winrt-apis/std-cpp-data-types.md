---
description: Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão.
title: Tipos de dados C++ e C++/WinRT padrão
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, dados, tipos
ms.localizationpriority: medium
ms.openlocfilehash: 83d2c0c2c544d63d2806dc71bfc367613d34e23a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64745284"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Tipos de dados C++ e C++/WinRT padrão

Com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), é possível chamar APIs do Windows Runtime usando tipos de dados C++ padrão, incluindo alguns tipos de dados da Biblioteca Padrão do C++. É possível passar cadeias de caracteres padrão para as APIs (confira [Processamento da cadeia de caracteres em C++/WinRT](strings.md)) e passar as listas de inicializadores e contêineres padrão para APIs que esperam uma coleção semanticamente equivalente.

## <a name="standard-initializer-lists"></a>Listas de inicializadores padrão
Uma lista de inicializadores (**std::initializer_list**) é um constructo da Biblioteca Padrão do C++. Use listas de inicializadores para chamar determinados construtores e métodos do Windows Runtime. Por exemplo, chame com [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes) com uma lista dessas.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

Há duas partes envolvidas ao executar essa tarefa. Primeiro, o método **DataWriter::WriteBytes** usa um parâmetro do tipo [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**winrt::array_view** é um tipo C++/WinRT personalizado que representa com segurança uma série contígua de valores (ele é definido na biblioteca base do C++/WinRT, que é `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Segundo, **winrt::array_view** tem um construtor de lista de inicializadores.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

Em muitos casos, é possível escolher se deseja ou não ficar ciente de **winrt::array_view** durante a programação. Se optar por *não* ficar ciente, não haverá nenhum código para alterar se e quando um tipo equivalente aparecer na Biblioteca Padrão do C++.

É possível passar uma lista de inicializadores para uma API do Windows Runtime que espera um parâmetro de coleção. Pegue **StorageItemContentProperties::RetrievePropertiesAsync**, por exemplo.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Chame essa API com uma lista de inicializadores como esta.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

Existem dois fatores em jogo aqui. Primeiro, o computador chamado constrói um **std::vector** a partir da lista de inicializadores (esse computador chamado é assíncrono, portanto, pode ser proprietário desse objeto, e deve ser). Segundo, o C++/WinRT associa **std:: Vector** de forma transparente (e sem introduzir cópias) como um parâmetro de coleção do Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Vetores e matrizes padrão
[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) também tem os construtores de conversão de **std::vector** e **std::array**.

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

Então, é possível chamar **DataWriter::WriteBytes** com um **std::vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

Ou com um **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

O C++/WinRT associa **std::vector** como um parâmetro de coleção do Windows Runtime. Assim, é possível passar um **std::vector&lt;winrt::hstring&gt;** , e ele é convertido na coleção apropriada do Windows Runtime de **winrt::hstring**. Há um detalhe adicional a considerar se o computador chamado for assíncrono. Devido aos detalhes de implementação nesse caso, é preciso fornecer um rvalue, portanto é necessário fornecer uma cópia ou um movimento do vetor. No exemplo de código abaixo, movemos a propriedade do vetor para o objeto do tipo de parâmetro aceito pelo computador chamado assíncrono (e tomamos cuidado para não acessar novamente `vecH` após movê-lo). Se quiser saber mais sobre rvalues, confira [Categorias de valor e referências a elas](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Entretanto, não é possível passar um **std::vector&lt;std::wstring&gt;** quando se espera uma coleção do Windows Runtime. Isso acontece porque, ao converter para a coleção apropriada do Windows Runtime de **std::wstring**, a linguagem C++ não forçará os parâmetros de tipo dessa coleção. Consequentemente, o exemplo de código a seguir não será compilado (e, em vez disso, a solução é passar um **std::vector&lt;winrt::hstring&gt;** , como mostrado acima).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrizes brutas e intervalos de ponteiro
Lembrando que um tipo equivalente pode existir no futuro na Biblioteca Padrão do C++, também é possível trabalhar diretamente com **winrt::array_view** se quiser ou precisar.

O **winrt::array_view** tem os construtores de conversão de uma matriz bruta e de um intervalo de **T&ast;** (ponteiros para o tipo de elemento).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Operadores e funções de winrt::array_view
Uma série de construtores, operadores, funções e iteradores foram implementados para **winrt::hstring**. Um **winrt::array_view** é um intervalo, então é possível usá-lo com `for` baseado em intervalo ou com **std::for_each**.

Para saber mais e obter exemplos, confira o tópico de referência de API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>O **IVector&lt;T&gt;** e os constructos de iteração padrão
O [**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) é um exemplo de uma API do Windows Runtime que retorna uma coleção do tipo [**IVector&lt;T&gt;** ](/uwp/api/windows.foundation.collections.ivector_t_) (projetado no C++/WinRT como **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;** ). Use esse tipo com constructos de iteração padrão, como `for` baseado em intervalo.

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Corrotinas C++ com APIs assíncronas do Windows Runtime
Você pode continuar a usar a [PPL (biblioteca de padrões paralelos)](/cpp/parallel/concrt/parallel-patterns-library-ppl) ao chamar as APIs assíncronas do Windows Runtime. No entanto, em muitos casos, as corrotinas do C++ fornecem uma linguagem eficiente e mais fácil de codificar para interagir com objetos assíncronos. Para saber mais e obter exemplos de código, confira [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

## <a name="important-apis"></a>APIs Importantes
* Interface [IVector&lt;T&gt;](/uwp/api/windows.foundation.collections.ivector_t_)
* [Modelo de struct winrt::array_view](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Tópicos relacionados
* [Processamento da cadeia de caracteres em C++/WinRT](strings.md)
