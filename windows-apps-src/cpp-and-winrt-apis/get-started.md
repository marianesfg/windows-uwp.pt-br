---
description: Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples.
title: Introdução ao C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, introdução, ponto, de partida
ms.localizationpriority: medium
ms.openlocfilehash: 64104124a6342da3f6963c61bafc871838fd00f6
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721680"
---
# <a name="get-started-with-cwinrt"></a>Introdução ao C++/WinRT

Para você se familiarizar com o uso [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), este tópico apresenta um exemplo de código simples com base em uma nova **aplicativo de Console do Windows (C++/WinRT)** projeto. Este tópico também mostra como [adicionar C++/WinRT suporte a um projeto de aplicativo de área de trabalho do Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!NOTE]
> Embora seja recomendável que você desenvolva com as versões mais recentes do Visual Studio e o SDK do Windows, se você estiver usando o Visual Studio 2017 (versão 15.8.0 ou superior) e direcionados ao SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803), em seguida, um recém-criado C++/Projeto WinRT pode falhar ao compilar com o erro "*erro c3861:: 'from_abi': identificador não encontrado*" e com outros erros originados no *base.h*. A solução é o destino um posterior (mais compatível) versão do SDK do Windows, ou defina a propriedade de projeto **C/C++**  > **idioma** > **modo de conformidade: Não** (Além disso, se **/permissive--** é exibido na propriedade do projeto **C/C++**  > **idioma** > **linha de comando**  sob **opções adicionais**, em seguida, excluí-lo).

## <a name="a-cwinrt-quick-start"></a>Início rápido do C++/WinRT

> [!NOTE]
> Para obter informações sobre como instalar e usar o C++WinRT Visual Studio VSIX (extensão) e o pacote do NuGet (que juntos fornecem um modelo de projeto e suporte ao build), consulte [suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Crie um novo projeto **Aplicativo de Console do Windows (C++/WinRT)** .

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

Com as configurações de projeto padrão, os cabeçalhos incluídos vêm do SDK do Windows, dentro da pasta`%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. O Visual Studio inclui esse caminho na macro *IncludePath*. Mas não há nenhuma dependência estrita sobre o SDK do Windows, porque seu projeto (por meio de `cppwinrt.exe` ferramenta) gera os mesmos cabeçalhos no seu projeto *$(GeneratedFilesDir)* pasta. Eles serão carregados dessa pasta se eles não podem ser encontrados em outro lugar, ou se você alterar as configurações do projeto.

Os cabeçalhos contêm APIs do Windows projetadas para C++/WinRT. Em outras palavras, para cada tipo de Windows, o C++/WinRT define um equivalente C++ amigável (chamado de *tipo projetado*). Um tipo projetado tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace C++ **winrt**. Essas inclusões no cabeçalho pré-compilado reduz o tempo de compilação adicional.

> [!IMPORTANT]
> Sempre que você deseja usar um tipo de um namespaces do Windows, incluem correspondente C++Windows WinRT namespace arquivo de cabeçalho, conforme mostrado acima. O cabeçalho *correspondente* é aquele com o mesmo nome do tipo de namespace. Por exemplo, para usar a projeção de C++/WinRT para a classe de tempo de execução [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), `#include <winrt/Windows.Foundation.Collections.h>`. Se você incluir `winrt/Windows.Foundation.Collections.h`, em seguida, você não fizer isso *também* precisa incluir `winrt/Windows.Foundation.h`. Cada C++/cabeçalho de projeção do WinRT inclui automaticamente a seu arquivo de cabeçalho do namespace pai; Portanto, você não fizer isso *precisa* explicitamente incluí-lo. Embora, nesse caso, não exista nenhum erro.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

As diretivas de `using namespace` são opcionais, mas convenientes. O padrão mostrado acima para essas diretivas (permitindo que a pesquisa de nome não procure por nada no namespace **winrt**) é adequada para quando você estiver começando a um novo projeto e C++/WinRT for a única projeção de linguagem que estiver usando no projeto. Por outro lado, se estiver misturando o código C++/WinRT com [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e/ou o código ABI (interface binária de aplicativo) do SDK (você está fazendo a portabilidade de ou a interoperabilidade com um ou ambos os modelos), veja os tópicos [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md), [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md) e [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

A chamada para **winrt::init_apartment** inicializa o thread em tempo de execução do Windows; por padrão, em um apartamento de vários threads. A chamada também inicializa COM.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Aloque dois objetos em pilha: eles representam o uri do blog do Windows e um cliente de sindicalização. Construímos o uri com um literal de cadeia de caracteres simples (veja [Processamento da cadeia de caracteres em C++/WinRT](strings.md) para ver outras formas de trabalhar com cadeias de caracteres).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync** ](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) é um exemplo de uma função assíncrona do tempo de execução do Windows. O exemplo de código recebe um objeto de operação assíncrona do **RetrieveFeedAsync** e chama **get** nesse objeto para bloquear o thread de chamada e aguardar os resultados (no caso, um feed de sindicalização). Para saber mais sobre simultaneidade e técnicas de não bloqueio, consulte [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) é um intervalo definido pelos iteradores retornados do **begin** e **final** funções (ou suas variantes constante reversos e inverso constante). Por isso, você pode enumerar **itens** com uma instrução `for` baseada em intervalos ou a função de modelo **for_each**. Sempre que você itera em uma coleção de tempo de execução do Windows, assim, você precisará `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Veja o texto do título do feed como um objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (mais detalhes em [Processamento de cadeia de caracteres em C++/WinRT](strings.md)). Em seguida, **hstring** é enviado pela função **c_str**, que reflete o padrão usado com cadeias de caracteres da biblioteca padrão C++.

Como você pode ver, o C++/WinRT incentiva o uso de expressões C++ modernas e semelhantes a classes, como `syndicationItem.Title().Text()`. Esse é um estilo de programação diferente e mais limpo em contraposição à programação COM tradicional. Não é necessário inicializar diretamente o COM. Trabalhe com ponteiros COM.

Nem tampouco é necessário processar os códigos de retorno de HRESULT. O C++/WinRT converte os HRESULTs de erro em exceções, como [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) para oferecer um estilo de programação moderno e natural. Para saber mais sobre o processamento de erros e ver exemplos de código, consulte [Processamento de erros com C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modificar um projeto de aplicativo de área de trabalho do Windows para adicionar C++suporte /WinRT

Esta seção mostra como você pode adicionar C++/WinRT suporte para um projeto de aplicativo de área de trabalho do Windows que você possa ter. Se você não tiver um projeto de aplicativo de área de trabalho do Windows existente, em seguida, você pode prosseguir com essas etapas primeiro uma criação. Por exemplo, abra o Visual Studio e crie uma **Visual C++** \> **área de trabalho do Windows** \> **aplicativo de área de trabalho do Windows** projeto.

Opcionalmente, você pode instalar o [ C++WinRT Visual Studio VSIX (extensão)](https://aka.ms/cppwinrt/vsix) e o pacote do NuGet. Para obter detalhes, consulte [suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Definir propriedades de projeto

Vá para a propriedade do projeto **gerais** \> **versão do SDK do Windows**e selecione **todas as configurações** e **todas as plataformas**. Certifique-se de que **versão do SDK do Windows** é definido como 10.0.17134.0 (Windows 10, versão 1803) ou maior.

Confirme que você não será afetado por [por que meu novo projeto não será compilado?](/windows/uwp/cpp-and-winrt-apis/faq).

Porque C++/WinRT usa recursos do C + + 17 padrão, defina propriedade do projeto **C /C++**  > **idioma**  >   **C++ idioma Standard** à *ISO padrão c++17 (/ std:c++17 + + 17)* .

### <a name="the-precompiled-header"></a>O cabeçalho pré-compilado

O modelo de projeto padrão cria um cabeçalho pré-compilado para você, denominado `framework.h`, ou `stdafx.h`. Renomeá-lo para `pch.h`. Se você tiver um `stdafx.cpp` do arquivo e, em seguida, renomeá-lo para `pch.cpp`. Defina a propriedade do projeto **C /C++**  > **cabeçalhos pré-compilados** > **cabeçalho pré-compilado** para *criar (/Yc)* , e **arquivo de cabeçalho pré-compilado** à *pch*.

Localizar e substituir tudo `#include "framework.h"` (ou `#include "stdafx.h"`) com `#include "pch.h"`.

Na `pch.h`, incluem `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Vinculação

O C++/projeção de linguagem WinRT depende de determinadas funções do Windows Runtime livres (não-membro) e pontos de entrada, que exigem de vinculação para o [WindowsApp.lib](/uwp/win32-and-com/win32-apis) biblioteca abrangente. Esta seção descreve três maneiras de satisfazer o vinculador.

A primeira opção é adicionar ao seu Visual Studio projeto todos os C++propriedades do MSBuild do WinRT e destinos. Para fazer isso, instale o [pacote do Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) em seu projeto. Abra o projeto no Visual Studio, clique em **Project** \> **gerenciar pacotes NuGet...** \> **Navegue**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **instalar** para instalar o pacote para o projeto.

Você também pode usar as configurações de link de projeto para vincular explicitamente `WindowsApp.lib`. Ou, você pode fazê-lo no código-fonte (em `pch.h`, por exemplo) como esta.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Você agora pode compilar e vincular e adicionar C++/WinRT código ao seu projeto (por exemplo, o código semelhante ao mostrado na [um C++/WinRT de início rápido](#a-cwinrt-quick-start) seção acima).

## <a name="important-apis"></a>APIs Importantes
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propriedade SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [struct WinRT::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Tratamento de erro com C++/WinRT](error-handling.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md)
* [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
* [Cadeia de caracteres de manipulação em C++/WinRT](strings.md)
