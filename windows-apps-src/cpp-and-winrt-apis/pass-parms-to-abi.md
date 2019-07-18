---
description: O C++/WinRT simplifica a passagem de parâmetros para o limite do ABI, fornecendo conversões automáticas para casos comuns.
title: Passagem de parâmetros para o limite do ABI
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, passar, parâmetros, ABI
ms.localizationpriority: medium
ms.openlocfilehash: c1e172fc4dbd5b865add1828a98dc1a030d5dc6f
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844348"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>Passagem de parâmetros para o limite do ABI

Com os tipos no namespace **winrt::param**, o C++/WinRT simplifica a passagem de parâmetros para o limite do ABI fornecendo conversões automáticas para casos comuns. É possível ver mais detalhes e exemplos de código em [Manipulação de cadeia de caracteres](/windows/uwp/cpp-and-winrt-apis/strings) e [Tipos de dados C++ padrão e C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types).

> [!IMPORTANT]
> Não use os tipos no namespace **winrt::param** por conta própria. Eles têm a finalidade de beneficiar a projeção.

Muitos tipos são fornecidos em versões síncronas e assíncronas. O C++/WinRT usa a versão síncrona quando você está passando um parâmetro para um método síncrono e usa a versão assíncrona quando você está passando um parâmetro para um método assíncrono. A versão assíncrona tem etapas extras para tornar mais difícil para o chamador executar uma mutação da coleção antes que a operação seja concluída. No entanto, observe que nenhuma das variantes protege a coleção de sofrer uma mutação por outro thread. Impedir isso é sua responsabilidade.

## <a name="string-parameters"></a>Parâmetros de cadeia de caracteres

**winrt::param::hstring** simplifica a passagem de parâmetros para APIs que utilizam **HSTRING**.

|Tipos que você pode passar|Observações|
|-|-|
|`{}`|Passa uma cadeia de caracteres vazia.|
|**winrt::hstring**||
|**std::wstring_view**|Para literais, você pode escrever `L"Name"sv`. A exibição precisa ter um terminador nulo após o final.|
|**std::wstring**|-|
|**wchar_t const\***|Uma cadeia de caracteres terminada em nulo.|

`nullptr` não é permitido. Em vez disso, use `{}`.

O compilador sabe como avaliar `wcslen` em literais de cadeia de caracteres em tempo de compilação. Portanto, para literais, `L"Name"sv` e `L"Name"` são equivalentes.

Observe que objetos **std::wstring_view** não são terminados em nulo, mas C++/WinRT exige que o caractere após o final da cadeia de caracteres seja nulo. Se você passar uma **std::wstring_view** não terminada em nulo, o processo será encerrado.

## <a name="iterable-parameters"></a>Parâmetros que pode ser iterados

**winrt::param::iterable\<T\>** e **winrt::param::async_iterable\<T\>** simplificam a passagem de parâmetros para APIs que usam **IIterable\<T\>** .

Coleções do Windows Runtime já são **IIterable**.

|Tipos que você pode passar|Sync|Async|Observações|
|-|-|-|-|
| `nullptr` | Sim | Sim | Você precisa verificar se o método subjacente dá suporte a `nullptr`.|
| **IIterable\<T\>** | Sim | Sim | Ou qualquer coisa que possa ser convertida nele.|
| **std::vector\<T\> const&** | Sim | Não ||
| **std::vector\<T\>&&** | Sim | Sim | O conteúdo é movido para o iterador para evitar a mutação.|
| **std::initializer_list\<T\>** | Sim | Sim | A versão assíncrona copia os itens.|
| **std::initializer_list\<U\>** | Sim | Não | Deve ser possível converter **U** em **T**.|
| `{ ForwardIt begin, ForwardIt end }` | Sim | Não | Deve ser possível converter `*begin` em **T**.|

Observe que **IIterable\<U\>** e **std::vector\<U\>** não são permitidos, mesmo que **U** possa ser convertido em **T**. Para **std::vector\<U\>** , você pode usar a versão com iterador duplo (mais detalhes abaixo).

Em alguns casos, o objeto que você tem pode implementar o **IIterable** que deseja. Por exemplo, o **IVectorView\<StorageFile\>** produzido por [**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) implementa **IIterable<StorageFile>** . Mas ele também implementa **IIterable<IStorageItem>** , você só precisa pedir por ele explicitamente.

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

Em outros casos, você pode usar a versão com iterador duplo.

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

O iterador duplo funciona de maneira mais geral no caso em que você tem uma coleção que não se encaixa em nenhum dos cenários acima, desde que você possa iterar nele e produzir coisas que possam ser convertidas em **T**. Nós o usamos acima para iterar em um vetor de tipos derivados. Aqui, o usamos para iterar em um não vetor de tipos derivados.

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

A implementação de [**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany) é mais eficiente quando o iterador é um `RandomAcessIt`. Caso contrário, ela faz várias passagens pelo intervalo.

|Tipos que você pode passar|Sync|Async|Observações|
|-|-|-|-|
| `nullptr` | Sim | Sim | Você precisa verificar se o método subjacente dá suporte a `nullptr`.|
| **IIterable\<IKeyValuePair\<K, V\>\>** | Sim | Sim | Ou qualquer coisa que possa ser convertida nele.|
| **std::map\<K, V\> const&** | Sim | Não ||
| **std::map\<K, V\>&&** | Sim | Sim | O conteúdo é movido para o iterador para evitar a mutação.|
| **std::unordered_map\<K, V\> const&** | Sim | Não ||
| **std::unordered_map\<K, V\>&&** | Sim | Sim | O conteúdo é movido para o iterador para evitar a mutação.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Sim | Sim | Os tipos **K** e **V** precisam ser exatamente correspondentes. As chaves não podem ser duplicadas. A versão assíncrona copia os itens.|
| `{ ForwardIt begin, ForwardIt end }` | Sim | Não | Deve ser possível converter `begin->first` e `begin->second` em **K** e **V**, respectivamente.|

## <a name="vector-view-parameters"></a>Parâmetros de exibição de vetor

**winrt::param::vector_view\<T\>** e **winrt::param::async_vector_view\<T\>** simplificam a passagem de parâmetros para APIs que usam **IVectorView\<T\>** .

Você pode usar [**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) para obter um **IVectorView** de um **IVector**.

|Tipos que você pode passar|Sync|Async|Observações|
|-|-|-|-|
| `nullptr` | Sim | Sim | Você precisa verificar se o método subjacente dá suporte a `nullptr`.|
| **IVectorView\<T\>** | Sim | Sim | Ou qualquer coisa que possa ser convertida nele.|
| **std::vector\<T\>const&** | Sim | Não ||
| **std::vector\<T\>&&** | Sim | Sim | O conteúdo é movido para a exibição para evitar a mutação.|
| **std::initializer_list\<T\>** | Sim | Sim | O tipo deve ser idêntico ao digitado anteriormente. A versão assíncrona copia os itens.|
| `{ ForwardIt begin, ForwardIt end }` | Sim | Não | Deve ser possível converter `*begin` em **T**.|

A versão com iterador duplo pode ser usada para criar exibições de vetor usando coisas que não se encaixam nos requisitos para serem passadas diretamente. No entanto, observe que, como vetores dão suporte ao acesso aleatório, é recomendável passar um `RandomAcessIter`.

## <a name="map-view-parameters"></a>Parâmetros de exibição de mapa

**winrt::param::map_view\<T\>** e **winrt::param::async_map_view\<T\>** simplificam a passagem de parâmetros para APIs que usam **IMapView\<T\>** .

Você pode usar **IMap::GetView** para obter um **IMapView** de um **IMap**.

|Tipos que você pode passar|Sync|Async|Observações|
|-|-|-|-|
| `nullptr` | Sim | Sim | Você precisa verificar se o método subjacente dá suporte a `nullptr`.|
| **IMapView\<K, V\>** | Sim | Sim | Ou qualquer coisa que possa ser convertida nele.|
| **std::map\<K, V\> const&** | Sim | Não ||
| **std::map\<K, V\>&&** | Sim | Sim | O conteúdo é movido para a exibição para evitar a mutação.|
| **std::unordered_map\<K, V\> const&**  | Sim | Não ||
| **std::unordered_map\<K, V\>&&** | Sim | Sim | O conteúdo é movido para a exibição para evitar a mutação.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Sim | Sim | As versões síncrona e assíncrona copiam os itens. Você não pode duplicar chaves.|

## <a name="vector-parameters"></a>Parâmetros de vetor

**winrt::param::vector\<T\>** simplifica a passagem de parâmetros para APIs que usam **IVector\<T\>** .

|Tipos que você pode passar|Observações|
|-|-|
| `nullptr` | Você precisa verificar se o método subjacente dá suporte a `nullptr`.|
| **IVector\<T\>** | Ou qualquer coisa que possa ser convertida nele.|
| **std::vector\<T\>&&** | O conteúdo é movido para o parâmetro para evitar a mutação. Os resultados não são movidos de volta.|
| **std::initializer_list\<T\>** | O conteúdo é copiado para o parâmetro para evitar a mutação.|

Se o método gerar uma mutação do vetor, a única maneira de observar essa mutação será passar um **IVector** diretamente. Se você passar um **std::vector**, o método gerará uma mutação da cópia, e não do original.

## <a name="map-parameters"></a>Parâmetros de mapa

**winrt::param::map\<T\>** simplifica a passagem de parâmetros para APIs que usam **IMap\<T\>** .

|Tipos que você pode passar|Observações|
|-|-|
| `nullptr` | Você precisa verificar se o método subjacente dá suporte a `nullptr`.|
| **IMap\<T\>** | Ou qualquer coisa que possa ser convertida nele.|
| **std::map\<K, V\>&&** | O conteúdo é movido para o parâmetro para evitar a mutação. Os resultados não são movidos de volta.|
| **std::unordered_map\<K, V\>&&** | O conteúdo é movido para o parâmetro para evitar a mutação. Os resultados não são movidos de volta.|
| **std::initializer_list\<std::pair\<K, V\>\>** | O conteúdo é copiado para o parâmetro para evitar a mutação.|

Se o método gerar uma mutação do mapa, a única maneira de observar essa mutação será passar um **IMap** diretamente. Se você passar um **std::map** ou **std::unordered_map**, o método gerará uma mutação da cópia, e não do original.

## <a name="array-parameters"></a>Parâmetros de matriz

O **winrt::array_view\<T\>** não está no namespace **winrt::param**, mas é usado para parâmetros que são matrizes de estilo C &mdash;também conhecidas como *matrizes compatíveis*.

|Tipos que você pode passar|Observações|
|-|-|
| `{}` | Uma matriz vazia.|
| **array** | Uma matriz de C compatível (ou seja, `C array[N];`), em que **C** pode ser convertido em **T** e `sizeof(C) == sizeof(T)`. |
| **std::array<C, N>** | Uma **std::array** C++ de **C**, em que **C** pode ser convertido em **T** e `sizeof(C) == sizeof(T)`. |
| **std::vector<C>** | Um **std::vector** C++ de **C**, em que **C** pode ser convertido em **T** e `sizeof(C) == sizeof(T)`. |
| `{ T*, T* }` | Um par de ponteiros representa o intervalo [início, fim).|
| **std::initializer_list\<T\>** ||