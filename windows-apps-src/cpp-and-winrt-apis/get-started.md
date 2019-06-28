---
description: Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples.
title: Introdução ao C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, introdução, ponto de partida
ms.localizationpriority: medium
ms.openlocfilehash: 64104124a6342da3f6963c61bafc871838fd00f6
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66721680"
---
# <a name="get-started-with-cwinrt"></a>Introdução ao C++/WinRT

Para você se familiarizar com o uso de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), este tópico apresenta um exemplo de código simples com base em um novo projeto de **Aplicativo de Console do Windows (C++/WinRT)** . Este tópico também mostra como [adicionar suporte ao C++/WinRT a um projeto de aplicativo de área de trabalho do Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!NOTE]
> Embora a recomendação seja de que você desenvolva usando as últimas versões do Visual Studio e do SDK do Windows, se estiver usando o Visual Studio 2017 (versão 15.8.0 ou superior) e visando o SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803), haverá falha de compilação de um projeto do C++/WinRT recentemente criado com o erro "*erro C3861: 'from_abi': identificador não encontrado*" e com outros erros que são originados em *base.h*. A solução é visar uma versão posterior (mais compatível) do SDK do Windows ou definir a propriedade do projeto **C/C++**  > **Language** > **Conformance mode: No** (além disso, se **/permissive-** for exibido na propriedade do projeto **C/C++**  > **Language** > **Command Line** em **Additional Options**, exclua-o).

## <a name="a-cwinrt-quick-start"></a>Um início rápido do C++/WinRT

> [!NOTE]
> Para obter informações sobre como instalar e usar a VSIX (Extensão do Visual Studio) para C++/WinRT e o pacote NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Crie um novo projeto de **Aplicativo de Console do Windows (C++/WinRT)** .

Edite `pch.h` e `main.cpp` para que tenha a aparência a seguir.

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
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
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

Com as configurações de projeto padrão, os cabeçalhos incluídos são originados no SDK do Windows, dentro da pasta`%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. O Visual Studio inclui esse caminho em sua macro *IncludePath*. Mas não há dependência estrita sobre o SDK do Windows, pois seu projeto (por meio da ferramenta `cppwinrt.exe`) gera os mesmos cabeçalhos na pasta *$(GeneratedFilesDir)* do seu projeto. Eles serão carregados dessa pasta se não puderem ser encontrados em outro lugar, ou se você alterar as configurações do projeto.

Os cabeçalhos contêm APIs do Windows projetadas em C++/WinRT. Em outras palavras, para cada tipo de Windows, o C++/WinRT define um equivalente amigável de C++ (chamado de *tipo projetado*). Um tipo projetado tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace **winrt** de C++. Colocar essas inclusões no cabeçalho pré-compilado reduz os tempos incrementais da compilação.

> [!IMPORTANT]
> Sempre que desejar usar um tipo de um namespace do Windows, inclua o arquivo de cabeçalho do namespace do Windows para C++/WinRT correspondente, como mostrado acima. O cabeçalho *correspondente* é aquele com o mesmo nome do namespace do tipo. Por exemplo, para usar a projeção de C++/WinRT para a classe de tempo de execução [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), `#include <winrt/Windows.Foundation.Collections.h>`. Se você incluir `winrt/Windows.Foundation.Collections.h`, *também* não será preciso incluir `winrt/Windows.Foundation.h`. Cada cabeçalho de projeção de C++/WinRT inclui automaticamente o arquivo de cabeçalho do namespace pai para que você não *precise* inclui-lo explicitamente. Embora, se você o fizer, não existirá erro.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

As diretivas `using namespace` são opcionais, mas convenientes. O padrão mostrado acima para essas diretivas (permitindo a pesquisa de nome não qualificado para qualquer item no namespace **winrt**) é adequado para quando você estiver iniciando um novo projeto e C++/WinRT for a única projeção de linguagem que está sendo usada dentro desse projeto. Por outro lado, se você estiver misturando código de C++/WinRT com [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e/ou código ABI (interface binária de aplicativo) do SDK (você está fazendo a portabilidade de ou a interoperabilidade com um ou ambos os modelos), veja os tópicos [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md), [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md) e [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

A chamada a **winrt::init_apartment** inicializa o thread no Windows Runtime; por padrão, em um apartment multi-threaded. A chamada também inicializa o COM.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Aloque dois objetos em pilha: eles representam o uri do blog do Windows e um cliente de sindicalização. Construímos o uri com um literal de cadeia de caracteres simples (veja [Processamento da cadeia de caracteres em C++/WinRT](strings.md) para ver outras formas de trabalhar com cadeias de caracteres).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) é um exemplo de uma função assíncrona do Windows Runtime. O exemplo de código recebe um objeto de operação assíncrona do **RetrieveFeedAsync** e chama **get** nesse objeto para bloquear o thread de chamada e aguardar os resultados (no caso, um feed de sindicalização). Para saber mais sobre simultaneidade e técnicas de não bloqueio, confira [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) é um intervalo, definido pelos iteradores retornados pelas funções **begin** e **end** (ou por suas variantes constantes, reversas e constante-reversas). Por isso, você pode enumerar **itens** com uma instrução `for` baseada em intervalo ou com a função de modelo **std::for_each**. Sempre que você iterar em uma coleção do Windows Runtime como essa, você precisará `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Veja o texto do título do feed como um objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (mais detalhes em [Processamento de cadeia de caracteres em C++/WinRT](strings.md)). Em seguida, **hstring** é enviado pela função **c_str**, que reflete o padrão usado com cadeias de caracteres da biblioteca padrão C++.

Como você pode ver, o C++/WinRT incentiva o uso de expressões C++ modernas e semelhantes a classes, como `syndicationItem.Title().Text()`. Esse é um estilo de programação diferente e mais limpo em contraposição à programação COM tradicional. Não é necessário inicializar diretamente o COM. Trabalhe com ponteiros COM.

Nem tampouco é necessário processar os códigos de retorno de HRESULT. O C++/WinRT converte os HRESULTs de erro em exceções, como [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) para oferecer um estilo de programação moderno e natural. Para saber mais sobre o processamento de erros e ver exemplos de código, confira [Processamento de erros com C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modificar um projeto de aplicativo de Área de Trabalho do Windows para adicionar suporte ao C++/WinRT

Esta seção mostra como é possível adicionar suporte ao C++/WinRT a um projeto de aplicativo de Área de Trabalho do Windows que você possa ter. Se você não tiver um projeto de aplicativo de Área de Trabalho do Windows existente, siga estas etapas para criar um. Por exemplo, abra o Visual Studio e crie um projeto **Visual C++** \> **Área de Trabalho do Windows** \> **Aplicativo de Área de Trabalho do Windows**.

Se desejar, você pode instalar a [VSIX (Extensão do Visual Studio) para C++/WinRT](https://aka.ms/cppwinrt/vsix) e o pacote NuGet. Para obter detalhes, confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Definir propriedades do projeto

Vá para a propriedade do projeto **Geral** \> **Versão do SDK do Windows** e selecione **Todas as Configurações** e **Todas as Plataformas**. Certifique-se de que a **Versão do SDK do Windows** esteja definida para 10.0.17134.0 (Windows 10, versão 1803) ou superior.

Confirme que você não será afetado por [Por que meu novo projeto não será compilado?](/windows/uwp/cpp-and-winrt-apis/faq).

Como o C++/WinRT usa recursos do padrão C++17, defina a propriedade do projeto **C/C++**  > **Linguagem** > **Padrão da Linguagem C++** para *Padrão ISO C++17 (/std:c++17)* .

### <a name="the-precompiled-header"></a>O cabeçalho pré-compilado

O modelo de projeto padrão cria um cabeçalho pré-compilado para você, denominado `framework.h` ou `stdafx.h`. Renomeie-o para `pch.h`. Se você tiver um arquivo `stdafx.cpp`, renomeie-o para `pch.cpp`. Defina a propriedade do projeto **C/C++**  > **Cabeçalhos Pré-compilados** > **Cabeçalho Pré-compilado** para *Criar (/Yc)* e **Arquivo de Cabeçalho Pré-compilado** para *pch.h*.

Localize e substitua todo o `#include "framework.h"` (ou `#include "stdafx.h"`) por `#include "pch.h"`.

Em `pch.h`, inclua `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Vinculação

A projeção de linguagem de C++/WinRT depende de determinadas funções livres (não membros) do Windows Runtime, que exigem vinculação à biblioteca [WindowsApp.lib](/uwp/win32-and-com/win32-apis). Esta seção descreve três maneiras de atender ao vinculador.

A primeira opção é adicionar ao seu projeto do Visual Studio todos os destinos e propriedades MSBuild de C++/WinRT. Para isso, instale o [pacote NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) em seu projeto. Abra o projeto no Visual Studio, clique em **Projeto** \> **Gerenciar Pacotes NuGet...** \> **Procurar**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e clique em **Instalar** para instalar o pacote para o projeto.

Você também pode usar as configurações de link do projeto para vincular `WindowsApp.lib` explicitamente. Ou você pode fazê-lo no código-fonte (em `pch.h`, por exemplo) como se segue.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Agora, você pode compilar e vincular, bem como adicionar código de C++/WinRT ao seu projeto (por exemplo, código semelhante a esse mostrado na seção [Um início rápido do C++/WinRT](#a-cwinrt-quick-start), acima).

## <a name="important-apis"></a>APIs Importantes
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propriedade SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Struct winrt::hresult-error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Tratamento de erro com C++/WinRT](error-handling.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md)
* [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
* [Processamento da cadeia de caracteres em C++/WinRT](strings.md)
