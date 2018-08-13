---
author: stevewhims
description: Uma introdução ao C++/WinRT, uma projeção de linguagem C++ padrão para APIs do Windows Runtime.
title: Introdução ao C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, introdução
ms.localizationpriority: medium
ms.openlocfilehash: b22f331c1b39d85baa8a38975aef925576226eaa
ms.sourcegitcommit: 618741673a26bd718962d4b8f859e632879f9d61
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "1992082"
---
# <a name="introduction-to-cwinrt"></a>Introdução ao C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Com o C++/WinRT, você pode criar e consumir APIs do Windows Runtime usando qualquer compilador C++17 compatível com padrões. O SDK do Windows inclui o C++/WinRT; ele foi introduzido na versão 10.0.17134.0 (Windows 10, versão 1803).

> [!IMPORTANT]
> Duas das partes mais importantes do C++/WinRT a serem consideradas são descritas nas seções [Suporte do SDK para C++/WinRT](#sdk-support-for-cwinrt) e [Suporte do Visual Studio para C++/WinRT e o VSIX](#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="language-projections"></a>Projeções de linguagem
O Windows Runtime baseia-se nas COM (Component Object Model) APIs e ele foi projetado para ser acessado por meio de *projeções de linguagem*. Uma projeção oculta os detalhes do COM e oferece uma experiência de programação mais natural para uma linguagem específica.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>A projeção de linguagem C++/WinRT no conteúdo de referência de API da UWP no Windows
Quando você estiver navegando nas [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/), clique na caixa de combinação **Linguagem** no canto superior direito e selecione **C++/WinRT** para exibir os blocos de sintaxe de API conforme aparecem na projeção de linguagem C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Suporte a SDK para C++/WinRT
A partir da versão 10.0.17134.0 (Windows 10, versão 1803), o SDK do Windows contém uma biblioteca C++ padrão com base em cabeçalho e arquivo para consumir APIs primárias do Windows (APIs do Windows Runtime em namespaces do Windows). O C++/WinRT também vem com a ferramenta `cppwinrt.exe`, que você pode apontar para um arquivo de metadados do Windows Runtime (`.winmd`) a fim de gerar uma biblioteca C++ padrão com base em cabeçalho e arquivo que *projeta* as APIs descritas nos metadados para consumo pelo código do C++/WinRT. Os arquivos de metadados (`.winmd`) do Windows Runtime fornecem uma maneira canônica de descrever uma superfície de API do Windows Runtime. Ao apontar `cppwinrt.exe` nos metadados, você pode gerar uma biblioteca para uso com qualquer classe de tempo de execução implementada em um componente secundário ou de terceiros do Windows Runtime, ou implementar em seu próprio aplicativo. Para obter mais informações, consulte [Consumir APIs com C++/WinRT](consume-apis.md).

Com o C++/WinRT, você também pode implementar suas próprias classes de tempo de execução usando o C++ padrão, sem precisar recorrer à programação de estilo COM. Para uma classe de tempo de execução, você descreve apenas os tipos em um arquivo IDL e `midl.exe` e `cppwinrt.exe` geram os arquivos de código-fonte clichê de implementação para você. Você também pode implementar apenas interfaces derivando uma classe base do C++/WinRT. Para obter mais informações, consulte [Criar APIs com C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>Suporte do Visual Studio para C++/WinRT e o VSIX
Para modelos de projeto C++/WinRT no Visual Studio, bem como para propriedades e destinos C++/WinRT do MSBuild, baixe e instale a [Extensão do Visual Studio (VSIX) do C++/WinRT](https://aka.ms/cppwinrt/vsix) no [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

Você precisará do Visual Studio 2017 (pelo menos a versão15.6; recomendamos no mínimo a 15.7), e o SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803). Em seguida, você poderá criar um novo projeto no Visual Studio ou converter um projeto existente adicionando a propriedade `<CppWinRTEnabled>true</CppWinRTEnabled>` ao arquivo `.vcxproj`, em Projeto > PropertyGroup. Após adicionar essa propriedade, você receberá o suporte do MSBuild para C++/WinRT no projeto, incluindo a invocação da ferramenta `cppwinrt.exe`.

Como o C++/WinRT usa recursos do padrão C++17, ele precisa da propriedade de projeto **C/C++** > **Linguagem** > **Padrão ISO C++17 (/std:c++17)**. Talvez também seja necessário definir **Modo de conformidade: Sim (/permissive-)**, o que restringirá ainda mais seu código para que seja compatível com padrões.

Outra propriedade de projeto a ser considerada é **C/C++** > **Geral** > **Tratar Avisos como Erros**. Defina-a como **Sim (/WX)** ou **Não (/WX-)** para fazer um teste. Às vezes, os arquivos de origem gerados pela ferramenta `cppwinrt.exe` geram avisos até você adicionar a implementação a eles.

O VSIX também oferece visualização de depuração nativa do Visual Studio (natvis) dos tipos projetados do C++/WinRT; fornecendo uma experiência semelhante à depuração de C#. Natvis é automático para compilações de depuração. Você pode escolher compilações de liberação definindo o símbolo WINRT_NATVIS.

Veja os modelos de projeto do Visual Studio fornecidos pelo VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicativo de Console do Windows (C++/WinRT)
Um modelo de projeto para um aplicativo cliente C++/WinRT para Área de Trabalho do Windows, com uma interface de usuário de console.

### <a name="blank-app-cwinrt"></a>Aplicativo em Branco (C++/WinRT)
Um modelo de projeto para um aplicativo UWP (Plataforma Universal do Windows) que tem uma interface do usuário XAML.

O Visual Studio fornece suporte ao compilador XAML para gerar stubs de implementação e cabeçalho a partir do arquivo de linguagem IDL (`.idl`) que são a base de cada arquivo de marcação XAML. Em um arquivo IDL, defina qualquer classe de tempo de execução local que você deseja referenciar nas páginas XAML do seu app e crie o projeto uma vez para gerar modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de tempo de execução local. É recomendável declarar cada classe de tempo de execução em seu próprio arquivo IDL.

### <a name="core-app-cwinrt"></a>App Core (C++/WinRT)
Um modelo de projeto para um app da Plataforma Universal do Windows (UWP) que não usa XAML.

Em vez disso, ele usa o cabeçalho de namespace do Windows do C++/WinRT para o namespace Windows.ApplicationModel.Core. Após a compilação e a execução, clique em um espaço vazio para adicionar um quadrado colorido; em seguida, clique em um quadrado colorido para arrastá-lo.

### <a name="windows-runtime-component-cwinrt"></a>Componente do Tempo de Execução do Windows (C++/WinRT)
Um modelo de projeto para um componente; normalmente para consumo de uma Plataforma Universal do Windows (UWP).

Esse modelo demonstra a cadeia de ferramentas `midl.exe` > `cppwinrt.exe`, na qual os metadados do Windows Runtime (`.winmd`) são gerados a partir da IDL. Depois, os stubs de implementação e de cabeçalho são gerados a partir desses metadados.

Em um arquivo IDL, defina as classes de tempo de execução no componente, na interface padrão e em qualquer outra interface implementada por ele. Compile o projeto uma vez para gerar `module.g.cpp`, `module.h.cpp`, modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de tempo de execução no componente. É recomendável declarar cada classe de tempo de execução em seu próprio arquivo IDL.

Agrupe o binário interno do componente do Tempo de Execução do Windows e seu `.winmd` com o aplicativo UWP que os utiliza.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados na projeção C++/WinRT
Na programação do C++/WinRT, você pode usar os recursos de linguagem C++ padrão e [tipos de dados do C++ padrão e do C++/WinRT](std-cpp-data-types.md)&mdash;incluindo alguns tipos de dados da Biblioteca padrão do C++&mdash. Mas, você também ficará a par de alguns tipos de dados personalizados na projeção e poderá optar por usá-los. Por exemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) no exemplo do código de início rápido em [Introdução com C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) é outro tipo que você provavelmente usará em algum momento. Mas a probabilidade de você usar diretamente um tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) é menor. Ou você pode optar por não usá-lo para que não tenha nenhum código a ser alterado se um tipo equivalente aparecer na Biblioteca Padrão C++.

> [!WARNING]
> Também existem tipos que você poderá ver se investigar detalhadamente os cabeçalhos de namespace do C++/WinRT do Windows. Um exemplo é **winrt::param::hstring**, mas há exemplos de coleção também. Elas existem exclusivamente para otimizar a associação de parâmetros de entrada e geram melhorias de desempenho grande, além de transformar a maioria dos padrões de chamadas em "trabalho" para os tipos e contêineres de C++ padrão relacionados. Esses tipos são usados apenas pela projeção em casos onde agregarem o maior valor. São altamente otimizados e não são para uso geral; não tente usá-los. Você também não deve usar qualquer item do namespace `winrt::impl`, pois são tipos de implementação e, portanto, estão sujeitos a mudanças. Você deve continuar a usar tipos padrão ou tipos do [winrt namespace](/uwp/cpp-ref-for-winrt/winrt).

## <a name="important-apis"></a>APIs Importantes
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Extensão do Visual Studio Extension (VSIX) para C++/WinRT](https://aka.ms/cppwinrt/vsix)
* [Introdução ao C++/WinRT](get-started.md)
* [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md)
* [Processamento de cadeia de caracteres em C++/WinRT](strings.md)
* [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/)