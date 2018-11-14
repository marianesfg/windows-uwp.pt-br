---
author: stevewhims
description: Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples.
title: Introdução ao C++/WinRT
ms.author: stwhi
ms.date: 10/19/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, introdução, ponto, de partida
ms.localizationpriority: medium
ms.openlocfilehash: 6cb8e18904f61976103689c8d83475ec248eb38b
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6282531"
---
# <a name="get-started-with-cwinrt"></a>Introdução ao C++/WinRT

Para você se familiarizar com o uso de [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), este tópico explica como um exemplo de código simples com base em um novo **aplicativo de Console do Windows (C++ c++ WinRT)** projeto. Este tópico também mostra como [Adicionar C++ c++ WinRT suporte para um projeto de aplicativo de área de trabalho do Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!IMPORTANT]
> Se você estiver usando o Visual Studio 2017 (versão 15.8.0 ou superior) e o SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803), em seguida, um recém-criado C + de direcionamento c++ WinRT projeto talvez não consiga compilar com o erro "*erro C3861: 'from_abi': identificador não encontrado*"e outros erros que se originam no *base.h*. A solução é qualquer destino uma posterior (mais compatível) versão do SDK do Windows, ou conjunto de propriedade do projeto **C/C++** > **idioma** > **modo de conformidade: não** (Além disso, se **/ permissivo-** aparece na propriedade do projeto ** C/C++** > **idioma** > de**linha de comando** em **Opções adicionais**, exclua-o).

## <a name="a-cwinrt-quick-start"></a>Início rápido do C++/WinRT

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Crie um novo projeto **Aplicativo de Console do Windows (C++/WinRT)**.

Edite `pch.h` e `main.cpp` para que tenha a aparência a seguir.

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

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modificar um projeto de aplicativo de área de trabalho do Windows para adicionar C++ c++ suporte WinRT

Esta seção mostra como você pode adicionar C++ c++ WinRT suporte para um projeto de aplicativo de área de trabalho do Windows que você possa ter. Se você não tiver um projeto de aplicativo de área de trabalho do Windows existente, em seguida, você pode seguir juntamente com estas etapas por uma criação primeira. Por exemplo, abra o Visual Studio e crie um **Visual C++** \> **Área de trabalho do Windows** \> projeto de**Aplicativo de área de trabalho do Windows** .

### <a name="set-project-properties"></a>Definir propriedades do projeto

Vá para projeto propriedade **Geral** \> **Versão do SDK do Windows**e selecione **Todas as configurações** e **Todas as plataformas**. Certifique-se de que a **Versão do SDK do Windows** é definida como 10.0.17134.0 (Windows 10, versão 1803) ou superior.

Confirme que você não será afetado por [por que meu projeto não será compilado?](/windows/uwp/cpp-and-winrt-apis/faq).

Porque C++ c++ /WinRT usa recursos do padrão C++ 17, defina a propriedade de projeto **C/C++** > **idioma** > **Padrão de linguagem C++** para *ISO C++ 17 padrão (/ /std: + + 17)*.

### <a name="the-precompiled-header"></a>O cabeçalho pré-compilado

Renomear o `stdafx.h` e `stdafx.cpp` para `pch.h` e `pch.cpp`, respectivamente. Defina a propriedade do projeto **C/C++** > **Cabeçalhos pré-compilados** > **Arquivo de cabeçalho pré-compilado** *pch. h*.

Localizar e substituir todos os `#include "stdafx.h"` com `#include "pch.h"`.

Em `pch.h`, inclua `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Vinculação

C++ c++ projeção de linguagem do WinRT depende de determinados (não membro) do Windows Runtime livres e pontos de entrada, que exigem para a biblioteca de lib [Windowsapp](/uwp/win32-and-com/win32-apis) . Esta seção descreve as três maneiras de satisfazer o vinculador.

A primeira opção é adicionar o Visual Studio todos C++ de projeto c++ WinRT MSBuild propriedades e destinos. Editar seu `.vcxproj` arquivo, localize `<PropertyGroup Label="Globals">` e, no grupo de propriedades, defina a propriedade `<CppWinRTEnabled>true</CppWinRTEnabled>`.

Como alternativa, você pode usar as configurações de associação de projeto para vincular explicitamente `WindowsApp.lib`.

Ou, você pode fazer isso no código-fonte (em `pch.h`, por exemplo) da seguinte maneira.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Você agora pode compilar e vincular e adicionar C++ c++ WinRT código ao seu projeto (por exemplo, o código mostrado o [A c++ c++ início rápido do WinRT](#a-cwinrt-quick-start) seção acima)

## <a name="important-apis"></a>APIs Importantes
* [Método syndicationclient:: Retrievefeedasync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propriedade SyndicationFeed. Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [struct WinRT:: HRESULT-error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Processamento de erros com C++/WinRT](error-handling.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md)
* [Mudar do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
* [Processamento de cadeia de caracteres em C++/WinRT](strings.md)
