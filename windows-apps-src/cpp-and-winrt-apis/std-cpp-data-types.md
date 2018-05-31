---
author: stevewhims
description: Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão.
title: Tipos de dados C++ padrão e C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, tipos de dados
ms.localizationpriority: medium
ms.openlocfilehash: ccf79b1ec21688d9573e62777def8f15295c3fca
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832070"
---
# <a name="standard-c-data-types-and-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Tipos de dados C++ padrão e [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão, incluindo alguns tipos de dados da Biblioteca Padrão C++.

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
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

Há duas partes envolvidas na realização desse trabalho. Primeiro, o método **DataWriter::WriteBytes** usa um parâmetro do tipo [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 **array_view** é um tipo C++/WinRT personalizado que representa com segurança uma série de valores contígua (ele é definido na biblioteca base C++/WinRT, que é `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Segundo, **array_view** tem um construtor de lista de inicializadores.

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

Em muitos casos, você pode escolher se deseja ou não ficar ciente do **array_view** na sua programação. Se você optar por *não* ficar ciente dele, não haverá nenhum código a ser alterado se um tipo equivalente aparecer na Biblioteca Padrão C++.

Você pode passar uma lista de inicializadores para uma API do Windows Runtime que espera um parâmetro de coleção. Use **StorageItemContentProperties::RetrievePropertiesAsync**, por exemplo.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Você pode chamar essa API com uma lista de inicializadores como esta.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" });
}
```

Dois fatores estão em jogo aqui. Primeiro, o computador chamado constrói um **std::vector** a partir da lista de inicializadores (esse computador chamado é assíncrono, portanto, pode ser proprietário desse objeto, e deve ser). Segundo, C++/WinRT associa de forma transparente (e sem introduzir cópias) o **std:: Vector** como um parâmetro de coleção do Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Vetores e matrizes padrão
**array_view** também tem os construtores de conversão de **std::vector** e **std::array**.

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

Assim, você também pode chamar **DataWriter::WriteBytes** com um **std::vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

Ou com um **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

O C++/WinRT associa **std::vector** como um parâmetro de coleção do Windows Runtime. Assim, você pode passar um **std::vector&lt;winrt::hstring&gt;**, e ele será convertido na coleção do Windows Runtime apropriada do **winrt::hstring**. Se o computador chamado for assíncrono, você deverá copiar ou mover o vetor. No exemplo de código a seguir, movemos a propriedade do vetor para o computador chamado assíncrono.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<winrt::hstring> const& vecH)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH));
}
```

Mas, você não pode passar um **std::vector&lt;std::wstring&gt;** em que uma coleção do Windows Runtime é esperada. Isso acontece porque, tendo feito a conversão na coleção do Windows Runtime apropriada de **std::wstring**, a linguagem C++ não forçará os parâmetros de tipo dessa coleção. Consequentemente, o exemplo de código a seguir não será compilado.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)); // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrizes brutos e intervalos de ponteiro
Tendo em mente que um tipo equivalente pode existir no futuro na Biblioteca Padrão C++, você também pode trabalhar diretamente com **array_view** se quiser ou precisar.

**array_view** tem os construtores de conversão de uma matriz bruta e de um intervalo de **T*** (ponteiros para o tipo de elemento).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Operadores e funções winrt::array_view
Uma série de construtores, operadores, funções e iteradores são implementados para **array_view**. Um **array_view** é um intervalo, para que você possa usá-lo com `for` baseado em intervalo ou **std::for_each**.

Para obter mais exemplos e informações, consulte o tópico de referência de API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="important-apis"></a>APIs importantes
* [Modelo de struct winrt::array_view](/uwp/cpp-ref-for-winrt/array-view)
