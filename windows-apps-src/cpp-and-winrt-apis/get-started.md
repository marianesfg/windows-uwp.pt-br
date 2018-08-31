---
author: stevewhims
description: Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples.
title: Introdução ao C++/WinRT
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, introdução, ponto, de partida
ms.localizationpriority: medium
ms.openlocfilehash: 13aa1e61a2d81cfa7faed0236551dad41bd00057
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3230544"
---
# <a name="get-started-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Introdução ao [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples.

## <a name="a-cwinrt-quick-start"></a>Início rápido do C++/WinRT
> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Crie um novo projeto **Aplicativo de Console do Windows (C++/WinRT)**. Edite `pch.h` e `main.cpp` para que tenha a aparência a seguir.

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
...
```

```cppwinrt
// main.cpp
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Vamos usar o exemplo de código acima detalhadamente e explicar o que está acontecendo em cada parte.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
```

Os cabeçalhos incluídos fazem parte do SDK, na pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. O Visual Studio inclui esse caminho na macro *IncludePath*. Os cabeçalhos contêm APIs do Windows projetadas para C++/WinRT. Em outras palavras, para cada tipo de Windows, o C++/WinRT define um equivalente C++ amigável (chamado de *tipo projetado*). Um tipo projetado tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace C++ **winrt**. Essas inclusões no cabeçalho pré-compilado reduz o tempo de compilação adicional.

> [!IMPORTANT]
> Sempre que desejar usar um tipo de namespace do Windows, inclua o arquivo de cabeçalho do namespace C++/WinRT correspondente, como mostrado. O cabeçalho *correspondente* é aquele com o mesmo nome do tipo de namespace. Por exemplo, para usar a projeção de C++/WinRT para a classe de tempo de execução [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

As diretivas de `using namespace` são opcionais, mas convenientes. O padrão mostrado acima para essas diretivas (permitindo que a pesquisa de nome não procure por nada no namespace **winrt**) é adequada para quando você estiver começando a um novo projeto e C++/WinRT for a única projeção de linguagem que estiver usando no projeto. Por outro lado, se estiver misturando o código C++/WinRT com [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e/ou o código ABI (interface binária de aplicativo) do SDK (você está fazendo a portabilidade de ou a interoperabilidade com um ou ambos os modelos), veja os tópicos [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md), [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md) e [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

A chamada para **winrt::init_apartment** inicializa COM; por padrão, em um multithreaded apartment.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Aloque dois objetos em pilha: eles representam o uri do blog do Windows e um cliente de sindicalização. Construímos o uri com um literal de cadeia de caracteres simples (veja [Processamento da cadeia de caracteres em C++/WinRT](strings.md) para ver outras formas de trabalhar com cadeias de caracteres).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) é um exemplo de uma função assíncrona do Windows Runtime. O exemplo de código recebe um objeto de operação assíncrona do **RetrieveFeedAsync** e chama **get** nesse objeto para bloquear o thread de chamada e aguardar os resultados (no caso, um feed de sindicalização). Para saber mais sobre simultaneidade e técnicas de não bloqueio, consulte [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) é um intervalo, definido pelos iteradores retornados pelas funções **begin** e **end** (ou por suas variantes constantes, reversas e constante-reversas). Por isso, você pode enumerar **itens** com uma instrução `for` baseada em intervalos ou a função de modelo **for_each**.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Veja o texto do título do feed como um objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (mais detalhes em [Processamento de cadeia de caracteres em C++/WinRT](strings.md)). Em seguida, **hstring** é enviado pela função **c_str**, que reflete o padrão usado com cadeias de caracteres da biblioteca padrão C++.

Como você pode ver, o C++/WinRT incentiva o uso de expressões C++ modernas e semelhantes a classes, como `syndicationItem.Title().Text()`. Esse é um estilo de programação diferente e mais limpo em contraposição à programação COM tradicional. Não é necessário inicializar diretamente o COM. Trabalhe com ponteiros COM.

Nem tampouco é necessário processar os códigos de retorno de HRESULT. O C++/WinRT converte os HRESULTs de erro em exceções, como [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) para oferecer um estilo de programação moderno e natural. Para saber mais sobre o processamento de erros e ver exemplos de código, consulte [Processamento de erros com C++/WinRT](error-handling.md).

## <a name="important-apis"></a>APIs Importantes
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Processamento de erros com C++/WinRT](error-handling.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md)
* [Mudar do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
* [Processamento de cadeia de caracteres em C++/WinRT](strings.md)
