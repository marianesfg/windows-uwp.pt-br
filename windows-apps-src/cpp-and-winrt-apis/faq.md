---
author: stevewhims
description: Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT.
title: Perguntas frequentes sobre C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, frequente, pergunta, questões, faq
ms.localizationpriority: medium
ms.openlocfilehash: 617f9ee49130a55cf0378f2a70b72296224dcefc
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1881017"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Perguntas frequentes sobre [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT.

> [!NOTE]
> Caso sua pergunta seja sobre uma mensagem de erro já vista, consulte também o tópico de [Solução de problemas de C++/WinRT](troubleshooting.md).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>Quais são os requisitos para o [Extensão do Visual Studio (VSIX) para C++/WinRT](https://aka.ms/cppwinrt/vsix)?
O [VSIX](https://aka.ms/cppwinrt/vsix) impõe uma versão de destino mínima do SDK do Windows de 10.0.17134.0 (Windows 10, versão 1803). Você também precisará do Visual Studio 2017 (pelo menos a versão 15.6; recomendamos pelo menos a 15.7). Você pode identificar um projeto que usa o VSIX pela presença de `<CppWinRTEnabled>true</CppWinRTEnabled>` em `<PropertyGroup Label="Globals">` no arquivo `.vcxproj`. Para obter mais informações, consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>O que é uma *classe de tempo de execução*?
Uma classe de tempo de execução é um tipo que pode ser ativado e consumido por meio de interfaces COM modernas, normalmente entre limites executáveis. No entanto, uma classe de tempo de execução também pode ser usada dentro da unidade de compilação que a implementa. Você declara uma classe de tempo de execução em idioma de definição da Interface (IDL) e pode implementá-la em C++ padrão usando C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>O que *o tipo projetado* e *o tipo de implementação* significam?
Se você estiver apenas *consumindo* uma classe do Windows Runtime (classe do tempo de execução), então você estará lidando exclusivamente com *tipos projetados*. C++/WinRT é uma *projeção de linguagem*, portanto, tipos projetados fazem parte da superfície do Windows Runtime sendo *projetado* em C++ com C++/WinRT. Para obter mais detalhes, consulte [Consumir APIs com C ++/WinRT](consume-apis.md).

O *tipo de implementação* contém a implementação de uma classe de tempo de execução, portanto, ele só está disponível no projeto que implementa a classe de tempo de execução. Quando você estiver trabalhando em um projeto que implementa classes de tempo de execução (um projeto do componente do Tempo de Execução do Windows ou um projeto que usa interface do usuário XAML), é importante estar familiarizado com a distinção entre o tipo de implementação de uma classe de tempo de execução e o tipo projetado que representa a classe de tempo de execução projetada em C++/WinRT. Para obter mais detalhes, consulte [Criar APIs com C ++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>É necessário declarar um construtor em minha IDL da classe do tempo de execução?
Somente se a classe de tempo de execução for projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado a consumo geral pelos aplicativos de cliente do Windows Runtime). Para obter detalhes completos sobre a finalidade e consequências da declaração de construtores na IDL, consulte [Construtores de classe de tempo de execução](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Por que é o vinculador apresenta o erro "LNK2019: símbolo externo não resolvido"?
Se o símbolo não resolvido for uma API dos cabeçalhos de namespace do Windows da projeção do C++/WinRT (no namespace **winrt**), a declaração da API é adiada em um cabeçalho que você incluiu, mas sua definição está em um cabeçalho ainda não incluído. Inclua o cabeçalho nomeado para o namespace da API e compile-o novamente. Para obter mais informações, consulte [Cabeçalhos de projeção do C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Se o símbolo não resolvido for uma função livre do Windows Runtime, como [RoInitialize](https://msdn.microsoft.com/library/br224650), você precisará incluir explicitamente a biblioteca [WindowsApp.lib](/uwp/win32-and-com/win32-apis) em seu projeto. A projeção de C++/WinRT depende de algumas dessas funções (não membro) livres e pontos de entrada. Se você usar um dos modelos de projeto da [Extensão do Visual Studio (VSIX) do C++/WinRT](https://aka.ms/cppwinrt/vsix) para seu aplicativo, `WindowsApp.lib` é associado automaticamente para você. Caso contrário, você pode usar as configurações de associação do projeto para inclui-lo ou fazer isso no código-fonte.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Devo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) e, em caso afirmativo, como?
Se você tiver uma classe de tempo de execução que libera recursos em seu destruidor, e essa classe de tempo de execução foi projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado a consumo geral pelos aplicativos de cliente do Windows Runtime), então, recomendamos que você também implemente **IClosable** para dar suporte ao consumo de sua classe de tempo de execução para idiomas que não têm finalização determinística. Certifique-se de que seus recursos sejam liberados se o destruidor [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close), ou ambos são chamados. **IClosable::Close** pode ser chamado um número arbitrário de vezes.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>É necessário chamar [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) nas classes de tempo de execução que eu consumo?
**IClosable** existe para dar suporte a idiomas que não têm finalização determinística. Portanto, você não deve chamar **IClosable::Close** a partir de C++/WinRT, exceto em casos bem raros, envolvendo corridas de desligamento ou amplexos semifatais. Se você estiver usando, por exemplo, tipos **Windows.UI.Composition**, em seguida, você pode encontrar casos em que você deseja descartar objetos em uma sequência de conjunto, como uma alternativa para permitir a destruição do wrapper de C++/WinRT seja feita para você.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>É possível usar LLVM/Clang para compilar com C++/WinRT?
Não oferecemos suporte à cadeia de ferramentas LLVM e Clang para C++/WinRT, mas podemos usá-la internamente para validar a conformidade de padrões do C ++/WinRT. Por exemplo, se você quisesse emular o que fazemos internamente, faça um teste, como descrito abaixo.

Acesse a [a página de Download de LLVM](https://releases.llvm.org/download.html), procure por **Baixar LLVM 6.0.0** > **Binários pré-criados** e baixe **Clang para Windows (64 bits)**. Durante a instalação, opte por adicionar LLVM à variável de sistema PATHA para que você possa invocá-la em um prompt de comando. Para os fins desse experimento, você pode ignorar qualquer "Falha ao localizar o diretório de conjuntos de ferramentas de MSBuild" e/ou "Falha na instalação de integração do MSVC" ao encontrá-las. Há diversas maneiras de invocar LLVM/Clang; o exemplo a seguir mostra apenas uma maneira.

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include "winrt/Windows.Foundation.h"
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

Como o C++/WinRT usa recursos do C++ 17 padrão, é necessário usar qualquer sinalizador de compilador para obter esse suporte; esses sinalizadores diferem de um compilador para outro.

O Visual Studio é a ferramenta de desenvolvimento a qual oferecemos suporte e recomendamos para C++/WinRT. Veja o [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!NOTE]
> Se este tópico não responder à sua pergunta, você pode encontrar ajuda usando a [`c++-winrt`marca no Stack Overflow ](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).