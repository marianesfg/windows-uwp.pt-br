---
author: stevewhims
description: Uma introdução ao C++/WinRT, uma projeção de linguagem C++ padrão para APIs do Windows Runtime.
title: Introdução ao C++/WinRT
ms.author: stwhi
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção
ms.localizationpriority: medium
ms.openlocfilehash: 968afd6fdad1e7bf6b3c38d929ab79eefa71819a
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832320"
---
# <a name="introduction-to-cwinrt"></a>Introdução ao C++/WinRT
> [!NOTE]
> **Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Incorporado na versão 10.0.17134.0 (Windows 10, versão 1803), o SDK do Windows agora inclui C++/WinRT.

> [!IMPORTANT]
> As partes mais importantes do C++/WinRT a serem consideradas são descritas nas seções [Suporte do SDK para C++/WinRT](#sdk-support-for-cwinrt) e [Suporte do Visual Studio para C++/WinRT e o VSIX](#visual-studio-support-for-cwinrt-and-the-vsix).

C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada exclusivamente em arquivos de cabeçalho e projetada para fornecer acesso de primeira classe à API moderna do Windows. Com o C++/WinRT, você pode criar e consumir APIs do Windows Runtime usando qualquer compilador C++17 compatível com padrões.

## <a name="language-projections"></a>Projeções de linguagem
O Windows Runtime baseia-se nas COM (Component Object Model) APIs e ele foi projetado para ser acessado por meio de *projeções de linguagem*. Uma projeção oculta os detalhes do COM e oferece uma experiência de programação mais natural para uma linguagem específica.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>A projeção de linguagem C++/WinRT no conteúdo de referência de API da UWP no Windows
Quando você estiver navegando nas [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/), clique na caixa de combinação **Linguagem** no canto superior direito e selecione **C++/WinRT** para exibir os blocos de sintaxe de API conforme aparecem na projeção de linguagem C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Suporte a SDK para C++/WinRT
A partir da versão 10.0.17134.0 (Windows 10, versão 1803), o SDK do Windows contém as ferramentas e os cabeçalhos de namespace do Windows da projeção C++/WinRT. Uma ferramenta importante é a `cppwinrt.exe`, que, a partir de `.winmd`, gera arquivos de código-fonte que projetam os metadados no C++/WinRT. Os arquivos de metadados (`.winmd`) do Windows Runtime fornecem uma maneira canônica de descrever uma superfície de API do Windows Runtime. A partir dos metadados do Windows Runtime, `cppwinrt.exe` gera uma biblioteca C++ padrão que descreve, ou *projeta*, completamente a superfície da API, sejam elas APIs do Windows ou APIs do componente do Tempo de Execução do Windows. `Cppwinrt.exe` Desempenha um papel importante no fluxo de trabalho de desenvolvimento tanto para as APIs internas quanto para as APIs de terceiros, e também para criar suas próprias APIs em componentes.

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>Suporte do Visual Studio para C++/WinRT e o VSIX
Para modelos de projeto C++/WinRT no Visual Studio, bem como para propriedades e destinos C++/WinRT do MSBuild, baixe e instale a [Extensão do Visual Studio (VSIX) do C++/WinRT](https://aka.ms/cppwinrt/vsix) no [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

Você precisará do Visual Studio 2017 versão 15.6 ou posterior e do SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803). Em seguida, você poderá criar um novo projeto no Visual Studio ou converter um projeto existente adicionando a propriedade `<CppWinRTProject>true</CppWinRTProject>` ao arquivo `.vcxproj`, em Projeto > PropertyGroup. Após adicionar essa propriedade, você receberá o suporte do MSBuild para C++/WinRT no projeto, incluindo a invocação da ferramenta `cppwinrt.exe`.

Como o C++/WinRT usa recursos do padrão C++17, ele precisa da propriedade de projeto **C/C++** > **Linguagem** > **Padrão ISO C++17 (/std:c++17)**. Talvez também seja necessário definir **Modo de conformidade: Sim (/permissive-)**, o que restringirá ainda mais seu código para que seja compatível com padrões.

Outra propriedade de projeto a ser considerada é **C/C++** > **Geral** > **Tratar Avisos como Erros**. Defina-a como **Sim (/WX)** ou **Não (/WX-)** para fazer um teste. Às vezes, os arquivos de origem gerados pela ferramenta `cppwinrt.exe` geram avisos até você adicionar a implementação a eles.

Esses são os modelos de projeto do Visual Studio fornecidos pelo VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicativo de Console do Windows (C++/WinRT)
Um modelo de projeto para um aplicativo cliente C++/WinRT para Área de Trabalho do Windows, com uma interface de usuário de console.

### <a name="blank-app-cwinrt"></a>Aplicativo em Branco (C++/WinRT)
Um modelo de projeto para um aplicativo UWP (Plataforma Universal do Windows) que tem uma interface do usuário XAML.

O Visual Studio fornece suporte ao compilador XAML para gerar stubs de implementação e cabeçalho a partir do arquivo de linguagem IDL (`.idl`) que são a base de cada arquivo de marcação XAML. Em um arquivo IDL, defina qualquer classe de tempo de execução local que você deseja referenciar nas páginas XAML do seu app e crie o projeto uma vez para gerar modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de tempo de execução local. É recomendável declarar cada classe de tempo de execução em seu próprio arquivo IDL.

### <a name="core-app-cwinrt"></a>App Core (C++/WinRT)
Um modelo de projeto para um app da Plataforma Universal do Windows (UWP) que não usa XAML.

Em vez disso, ele usa o cabeçalho de namespace do Windows da projeção C++/WinRT para o namespace Windows.ApplicationModel.Core. Após a compilação e a execução, clique em um espaço vazio para adicionar um quadrado colorido; em seguida, clique em um quadrado colorido para arrastá-lo.

### <a name="windows-runtime-component-cwinrt"></a>Componente do Tempo de Execução do Windows (C++/WinRT)
Um modelo de projeto para um componente; normalmente para consumo de uma Plataforma Universal do Windows (UWP).

Esse modelo demonstra a cadeia de ferramentas `midl.exe` > `cppwinrt.exe`, na qual os metadados do Windows Runtime (`.winmd`) são gerados a partir da IDL. Depois, os stubs de implementação e de cabeçalho são gerados a partir desses metadados.

Em um arquivo IDL, defina as classes de tempo de execução no componente, na interface padrão e em qualquer outra interface implementada por ele. Compile o projeto uma vez para gerar `module.g.cpp`, `module.h.cpp`, modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de tempo de execução no componente. É recomendável declarar cada classe de tempo de execução em seu próprio arquivo IDL.

Agrupe o binário interno do componente do Tempo de Execução do Windows e seu `.winmd` com o aplicativo UWP que os utiliza.

## <a name="a-cwinrt-quick-start"></a>Início rápido do C++/WinRT
Crie um novo projeto **Aplicativo de Console do Windows (C++/WinRT)**. Edite `main.cpp` para que tenha a aparência a seguir.

```cppwinrt
// main.cpp

#include "pch.h"
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

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
        hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Os cabeçalhos `winrt/Windows.Foundation.h` e `winrt/Windows.Web.Syndication.h` incluídos estão no SDK, na pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. O Visual Studio inclui esse caminho na macro *IncludePath*. Os cabeçalhos contêm APIs do Windows projetadas para C++/WinRT. Sempre que você quiser usar tipos dos namespaces do Windows, inclua cabeçalhos de projeção C++/WinRT do Windows correspondentes como esses. As diretivas de `using namespace` são opcionais, mas convenientes.

Todos os tipos projetados estão no namespace raiz **winrt** do C++/WinRT. O [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e o SDK do Windows declaram tipos no namespace raiz **Windows**. Esses namespaces distintos permitem que você faça a migração do C++/CX para o C++/WinRT em seu próprio ritmo.

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) é um exemplo de uma função assíncrona do Windows Runtime. O exemplo de código recebe um objeto de operação assíncrona do **RetrieveFeedAsync** e chama **get** nesse objeto para bloquear o thread de chamada e aguardar os resultados. Para saber mais sobre simultaneidade e técnicas de não bloqueio, consulte [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) é um intervalo, definido pelos iteradores retornados pelas funções **begin** e **end** (ou por suas variantes constantes, reversas e constante-reversas). Por isso, você pode enumerar **itens** com uma instrução `for` baseada em intervalos ou a função de modelo **for_each**.

Em seguida, o código obtém o texto de título do feed como um objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (consulte [Tratamento de cadeia de caracteres em C++/WinRT](strings.md)). O **hstring** será gerado via **c_str**, que lhe parecerá familiar se você tiver usado cadeias de caracteres da Biblioteca Padrão C++.

Como você pode ver, o C++/WinRT incentiva o uso de expressões C++ modernas e semelhantes a classes, como `syndicationItem.Title().Text()`. Esse é um estilo de programação diferente e mais limpo em contraposição à programação COM tradicional. Você não precisa inicializar explicitamente o COM (o **winrt::init_apartment** faz isso para você), trabalhar com ponteiros COM ou tratar códigos de retorno HRESULT. O C++/WinRT converte os HRESULTs de erro em exceções para oferecer um estilo de programação moderno e natural.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados na projeção C++/WinRT
Você pode usar os recursos de linguagem C++ padrão, os [tipos de dados C++ padrão e o C++/WinRT](std-cpp-data-types.md), incluindo alguns tipos de dados da Biblioteca Padrão C++, na programação C++/WinRT. Mas, você também ficará a par de alguns tipos de dados personalizados na projeção e poderá optar por usá-los. Por exemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) no início rápido anteriormente.

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) é outro tipo que você provavelmente usará em algum momento. Mas a probabilidade de você usar diretamente um tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) é menor. Ou você pode optar por não usá-lo para que não tenha nenhum código a ser alterado se um tipo equivalente aparecer na Biblioteca Padrão C++.

Também existem tipos que você poderá ver se investigar detalhadamente os cabeçalhos de namespace da projeção C++/WinRT do Windows. Um exemplo deles é o **winrt::param::hstring**. Eles existem apenas por motivos de eficiência; você não deve usá-los no código.

## <a name="important-apis"></a>APIs importantes
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Tratamento de cadeia de caracteres em C++/WinRT](strings.md)
* [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
* [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/)
