---
description: Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão.
title: Tipos de dados C++ padrão e C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, tipos de dados
ms.localizationpriority: medium
ms.openlocfilehash: 7b0b529bbf397b76acb1eb589095a84f5c85745c
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332731"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Tipos de dados C++ padrão e C++/WinRT

Com [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão, incluindo alguns tipos de dados da biblioteca padrão C++. Você pode passar cadeias de caracteres padrão para APIs (consulte [cadeia de caracteres de manipulação no C++ c++ WinRT](strings.md)), e você pode passar inicializador listas e contêineres padrão para APIs que espera uma coleção semanticamente equivalente.

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
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
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

O C++/WinRT associa **std::vector** como um parâmetro de coleção do Windows Runtime. Assim, você pode passar um **std::vector&lt;winrt::hstring&gt;**, e ele será convertido na coleção do Windows Runtime apropriada do **winrt::hstring**. Há um detalhe adicional deve ter em mente se o computador chamado for assíncrono. Devido aos detalhes da implementação nesse caso, você precisará fornecer um rvalue, portanto, você deve fornecer uma cópia ou uma movimentação do vetor. No exemplo de código abaixo, movemos a propriedade do vetor para o objeto do tipo de parâmetro aceito pelo computador chamado assíncrono (e, em seguida, estamos cuidadosos para não acessar `vecH` novamente após movê-lo). Se você quiser saber mais sobre rvalues, consulte [categorias de valor e referências a elas](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Mas, você não pode passar um **std::vector&lt;std::wstring&gt;** em que uma coleção do Windows Runtime é esperada. Isso acontece porque, tendo feito a conversão na coleção do Windows Runtime apropriada de **std::wstring**, a linguagem C++ não forçará os parâmetros de tipo dessa coleção. Consequentemente, o exemplo de código a seguir não será compilado (e a solução é passar um **std:: Vector&lt;WinRT:: hstring&gt; ** em vez disso, conforme mostrado acima).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrizes brutos e intervalos de ponteiro
Tendo em mente que um tipo equivalente pode existir no futuro na Biblioteca Padrão C++, você também pode trabalhar diretamente com **array_view** se quiser ou precisar.

**array_view** tem os construtores de conversão de uma matriz de bruta e dentre uma variedade de **T&ast; ** (ponteiros para o tipo de elemento).

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

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt; ** e construções de iteração padrão
[**SyndicationFeed. Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) é um exemplo de uma API de tempo de execução do Windows que retorna uma coleção de tipo [**IVector&lt;T&gt; **](/uwp/api/windows.foundation.collections.ivector_t_) (projetado no C++ c++ WinRT como **winrt::Windows::Foundation::Collections::IVector&lt;T&gt; ** ). Você pode usar esse tipo com construções de iteração padrão, como baseado em intervalo `for`.

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Rotinas concomitantes de C++ com APIs assíncronas do Windows Runtime
Você pode continuar a usar a [Biblioteca de padrões paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) ao chamar APIs assíncronas do Windows Runtime. No entanto, em muitos casos, rotinas concomitantes de C++ fornecem um idioma eficiente e mais facilmente codificado para interagir com objetos assíncronos. Para obter mais informações e exemplos de código, consulte [simultaneidade e operações assíncronas com C++ c++ WinRT](concurrency.md).

## <a name="important-apis"></a>APIs Importantes
* [IVector&lt;T&gt; interface](/uwp/api/windows.foundation.collections.ivector_t_)
* [Modelo de struct winrt::array_view](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Tópicos relacionados
* [Processamento de cadeia de caracteres em C++/WinRT](strings.md)
