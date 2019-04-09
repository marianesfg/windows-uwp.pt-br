---
description: Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão.
title: Tipos de dados C++ padrão e C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, tipos de dados
ms.localizationpriority: medium
ms.openlocfilehash: 44de7b61264f8e0e04d1de6d2b1101844656f28b
ms.sourcegitcommit: 99271798fe53d9768fc52b21366de05268cadcb0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58221452"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Tipos de dados C++ padrão e C++/WinRT

Com o [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), você pode chamar APIs de tempo de execução do Windows usando o padrão C++ tipos de dados, incluindo algumas C++ tipos de dados da biblioteca padrão. Você pode passar cadeias de caracteres padrão para as APIs (consulte [cadeia de caracteres de manipulação em C++/WinRT](strings.md)), e você pode passar o inicializador de listas e contêineres padrão para APIs que esperam uma coleção semanticamente equivalente.

## <a name="standard-initializer-lists"></a>Listas de inicializadores padrão
Uma lista de inicializadores (**std:: initializer_list**) é um constructo da Biblioteca Padrão C++. Você pode usar listas de inicializadores ao chamar determinados construtores e métodos do Windows Runtime. Por exemplo, você pode chamar [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes) com um.

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

Há duas partes envolvidas na realização desse trabalho. Primeiro, o método **DataWriter::WriteBytes** usa um parâmetro do tipo [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**WinRT::array_view** é um personalizado C++/WinRT tipo que representa com segurança uma série contígua de valores (ele é definido no C++biblioteca base /WinRT, que é `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Segundo, **winrt::array_view** tem um construtor de lista de inicializadores.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

Em muitos casos, você pode escolher se deseja ou não estar atento **winrt::array_view** em sua programação. Se você optar por *não* ficar ciente dele, não haverá nenhum código a ser alterado se um tipo equivalente aparecer na Biblioteca Padrão C++.

Você pode passar uma lista de inicializadores para uma API do Windows Runtime que espera um parâmetro de coleção. Use **StorageItemContentProperties::RetrievePropertiesAsync**, por exemplo.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Você pode chamar essa API com uma lista de inicializadores como esta.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

Dois fatores estão em jogo aqui. Primeiro, o receptor constrói uma **std:: Vector** da lista do inicializador (Esse receptor da chamada é assíncrona, portanto, é capaz de ter esse objeto, o que ele deve). Segundo, C++/WinRT associa de forma transparente (e sem introduzir cópias) o **std:: Vector** como um parâmetro de coleção do Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Vetores e matrizes padrão
[**WinRT::array_view** ](/uwp/cpp-ref-for-winrt/array-view) também tem construtores de conversão de **std:: Vector** e **std:: array**.

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

Assim, você também pode chamar **DataWriter::WriteBytes** com um **std::vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

Ou com um **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

O C++/WinRT associa **std::vector** como um parâmetro de coleção do Windows Runtime. Assim, você pode passar um **std::vector&lt;winrt::hstring&gt;**, e ele será convertido na coleção do Windows Runtime apropriada do **winrt::hstring**. Há um detalhe adicional para se ter em mente, se o receptor da chamada é assíncrona. Devido aos detalhes de implementação de nesse caso, você precisará fornecer um rvalue, portanto você deve fornecer uma cópia ou um movimento do vetor. No exemplo de código abaixo, estamos mova a propriedade do vetor para o objeto de parâmetro aceita pelo receptor async (e, em seguida, estamos cuidadosos para não acessar `vecH` novamente após movê-lo). Se você quiser saber mais sobre rvalues, consulte [categorias de valor e as referências a eles](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Mas, você não pode passar um **std::vector&lt;std::wstring&gt;** em que uma coleção do Windows Runtime é esperada. Isso acontece porque, tendo feito a conversão na coleção do Windows Runtime apropriada de **std::wstring**, a linguagem C++ não forçará os parâmetros de tipo dessa coleção. Consequentemente, o exemplo de código a seguir não será compilado (e a solução é passar uma **std:: Vector&lt;winrt::hstring&gt;**  em vez disso, conforme mostrado acima).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrizes brutos e intervalos de ponteiro
Tendo em mente a ressalva de que um tipo equivalente pode existir em futuras do C++ biblioteca padrão, você também pode trabalhar diretamente com **winrt::array_view** se você optar por, ou precisar.

**WinRT::array_view** tenha construtores de conversão de uma matriz bruta e de um intervalo de **T&ast;**  (ponteiros para o tipo de elemento).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Operadores e funções winrt::array_view
Um host de construtores, operadores, funções e iteradores são implementados para **winrt::array_view**. Um **winrt::array_view** é um intervalo, portanto, você pode usá-la com base no intervalo `for`, ou com **std::for_each**.

Para obter mais exemplos e informações, consulte o tópico de referência de API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;**  e construções de iteração padrão
[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) é um exemplo de uma API de tempo de execução do Windows que retorna uma coleção do tipo [ **IVector&lt;T&gt;**  ](/uwp/api/windows.foundation.collections.ivector_t_) (projetado em C++/WinRT como **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;**). Você pode usar esse tipo com construções de iteração padrão, como baseado em intervalo `for`.

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Corrotinas do C++ com o Windows Runtime APIs assíncronas
Você pode continuar a usar o [biblioteca de padrões paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) ao chamar o Windows Runtime APIs assíncronas. No entanto, em muitos casos, as corrotinas do C++ fornecem um eficiente e mais facilmente com código de idioma para interagir com objetos assíncronos. Para obter mais informações e exemplos de código, consulte [simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

## <a name="important-apis"></a>APIs Importantes
* [IVector&lt;T&gt; interface](/uwp/api/windows.foundation.collections.ivector_t_)
* [WinRT::array_view struct modelo](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Tópicos relacionados
* [Cadeia de caracteres de manipulação em C++/WinRT](strings.md)
