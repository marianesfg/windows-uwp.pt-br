---
author: stevewhims
description: Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT.
title: Perguntas frequentes sobre C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, frequente, pergunta, questões, faq
ms.localizationpriority: medium
ms.openlocfilehash: aad5c5ed2123af39ebb6aff0c9098586ce958196
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832030"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Perguntas frequentes sobre [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>Quais são os requisitos para o C++/WinRT [Extensões do Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix)?
O [VSIX](https://aka.ms/cppwinrt/vsix) impõe uma versão de destino mínima do SDK do Windows de 10.0.17134.0 (Windows 10, versão 1803). Você também precisa ter o Visual Studio 2017 versão 15.6, ou mais recente. Você pode identificar um projeto que usa o VSIX pela presença de `<CppWinRTEnabled>true</CppWinRTEnabled>` em `<PropertyGroup Label="Globals">` no arquivo `.vcxproj`. Para obter mais informações, consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>O que é uma *classe de tempo de execução*?
Uma classe de tempo de execução é um tipo que pode ser ativado e consumido por meio de interfaces COM modernas, normalmente entre limites executáveis. No entanto, uma classe de tempo de execução também pode ser usada dentro da unidade de compilação que a implementa. Você declara uma classe de tempo de execução em idioma de definição da Interface (IDL) e pode implementá-la em C++ padrão usando C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>O que *o tipo projetado* e *o tipo de implementação* significam?
Se você estiver apenas *consumindo* uma classe do Windows Runtime (classe do tempo de execução), então você estará lidando exclusivamente com *tipos projetados*. C++/WinRT é uma *projeção de linguagem*, portanto, tipos projetados fazem parte da superfície do Windows Runtime sendo *projetado* em C++ com C++/WinRT. Para obter mais detalhes, consulte [Consumir APIs com C ++/WinRT](consume-apis.md).

O *tipo de implementação* contém a implementação de uma classe de tempo de execução, portanto, ele só está disponível no projeto que implementa a classe de tempo de execução. Quando você estiver trabalhando em um projeto que implementa classes de tempo de execução (um projeto do componente do Tempo de Execução do Windows ou um projeto que usa interface do usuário XAML), é importante estar familiarizado com a distinção entre o tipo de implementação de uma classe de tempo de execução e o tipo projetado que representa a classe de tempo de execução projetada em C++/WinRT. Para obter mais detalhes, consulte [Criar APIs com C ++/WinRT](author-apis.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Devo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) e, em caso afirmativo, como?
Se você tiver uma classe de tempo de execução que libera recursos em seu destruidor, e essa classe de tempo de execução foi projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado a consumo geral pelos aplicativos de cliente do Windows Runtime), então, recomendamos que você também implemente **IClosable** para dar suporte ao consumo de sua classe de tempo de execução para idiomas que não têm finalização determinística. Certifique-se de que seus recursos sejam liberados se o destruidor [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close), ou ambos são chamados. **IClosable::Close** pode ser chamado um número arbitrário de vezes.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>É necessário chamar [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) nas classes de tempo de execução que eu consumo?
**IClosable** existe para dar suporte a idiomas que não têm finalização determinística. Portanto, você não deve chamar **IClosable::Close** a partir de C++/WinRT, exceto em casos bem raros, envolvendo corridas de desligamento ou amplexos semifatais. Se você estiver usando, por exemplo, tipos **Windows.UI.Composition**, em seguida, você pode encontrar casos em que você deseja descartar objetos em uma sequência de conjunto, como uma alternativa para permitir a destruição do wrapper de C++/WinRT seja feita para você.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>É necessário declarar um construtor em minha IDL da classe do tempo de execução?
Somente se a classe de tempo de execução for projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado a consumo geral pelos aplicativos de cliente do Windows Runtime). Para obter detalhes completos sobre a finalidade e consequências da declaração de construtores na IDL, consulte [Construtores de classe de tempo de execução](author-apis.md#runtime-class-constructors).