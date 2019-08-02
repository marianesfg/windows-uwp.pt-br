---
description: Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT.
title: Perguntas frequentes sobre C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, frequente, pergunta, questões, perguntas frequentes
ms.localizationpriority: medium
ms.openlocfilehash: 6bac3fec34467f29d9cf2cc3f1ce4e3754187745
ms.sourcegitcommit: 7ece8a9a9fa75e2e92aac4ac31602237e8b7fde5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485155"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Perguntas frequentes sobre C++/WinRT
As respostas às perguntas que você pode ter sobre a criação e o consumo de APIs do Windows Runtime com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Caso sua pergunta seja sobre uma mensagem de erro já vista, consulte também o tópico [Solução de problemas de C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Como redireciono meu projeto do C++/WinRT para uma versão posterior do SDK do Windows?
Confira [Como redirecionar seu projeto do C++/WinRT para uma versão posterior do SDK do Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>Por que meu novo projeto não será compilado, agora que migrei para o C++/WinRT 2.0?
Para o conjunto completo de alterações (incluindo alterações significativas), confira [Notícias e alterações do C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20). Por exemplo, se estiver usando `for` baseado em intervalo em uma coleção do Windows Runtime, você precisará `#include <winrt/Windows.Foundation.Collections.h>`.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Por que meu novo projeto não será compilado? Estou usando o Visual Studio 2017 (versão 15.8.0 ou superior) e a versão 17134 do SDK
Se você estiver usando o Visual Studio 2017 (versão 15.8.0 ou superior) e visando o SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803), haverá falha de compilação de um projeto do C++/WinRT recentemente criado com o erro "*erro C3861: 'from_abi': Identificador não encontrado*" e com outros erros que são originados em *base.h*. A solução é visar uma versão posterior (mais compatível) do SDK do Windows ou definir a propriedade do projeto **C/C++**  > **Language** > **Conformance mode: No** (além disso, se **/permissive-** for exibido na propriedade do projeto **C/C++**  > **Linha de comando** em **Opções Adicionais**, exclua-o).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Como resolvo o erro de compilação "O C++/WinRT VSIX não fornece mais suporte à compilação do projeto.  Adicione uma referência de projeto ao pacote do Nuget Microsoft.Windows.CppWinRT"?
Instale o pacote do NuGet **Microsoft.Windows.CppWinRT** em seu projeto. Para obter detalhes, confira [Versões anteriores da extensão do VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>Quais são os requisitos para as Extensões do Visual Studio (VSIX) no C++/WinRT?
Para a versão 1.0.190128.4 e posteriores da extensão VSIX, confira [Suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Para outras versões, confira [Versões anteriores da extensão VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>O que é uma *classe de tempo de execução*?
Uma classe de tempo de execução é um tipo que pode ser ativado e consumido por meio de interfaces COM modernas, normalmente entre limites executáveis. No entanto, uma classe de tempo de execução também pode ser usada dentro da unidade de compilação que a implementa. Você declara uma classe de tempo de execução na linguagem IDL e pode implementá-la em C++ padrão usando C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Qual o significado de *tipo projetado* e *tipo de implementação*?
Se estiver *consumindo* apenas uma classe do Windows Runtime (classe do tempo de execução), você estará lidando exclusivamente com *tipos projetados*. C++/WinRT é uma *projeção de linguagem*, de modo que os tipos projetados fazem parte da superfície do Windows Runtime que é *projetada* em C++ com C++/WinRT. Para obter mais detalhes, confira [Consumir APIs com C++/WinRT](consume-apis.md).

O *tipo de implementação* contém a implementação de uma classe de tempo de execução, portanto, ele só está disponível no projeto que implementa a classe de tempo de execução. Quando você estiver trabalhando em um projeto que implementa classes de tempo de execução (um projeto do componente do Tempo de Execução do Windows ou um projeto que usa interface do usuário XAML), é importante estar familiarizado com a distinção entre o tipo de implementação de uma classe de tempo de execução e o tipo projetado que representa a classe de tempo de execução projetada em C++/WinRT. Para obter mais detalhes, confira [Criar APIs com C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>É necessário declarar um construtor em minha IDL da classe do tempo de execução?
Somente se a classe de tempo de execução for projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado ao consumo geral pelos aplicativos cliente do Windows Runtime). Para obter detalhes completos sobre a finalidade e as consequências da declaração de construtores na IDL, confira [Construtores de classe de tempo de execução](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Por que o vinculador fornece um erro "LNK2019: Símbolo externo não resolvido"?
Se o símbolo não resolvido for uma API dos cabeçalhos de namespace do Windows para a projeção do C++/WinRT (no namespace **winrt**), a API será declarada por encaminhamento em um cabeçalho que você incluiu, mas sua definição estará em um cabeçalho ainda não incluído. Inclua o cabeçalho nomeado para o namespace da API e compile-o novamente. Para saber mais, confira [Cabeçalhos de projeção do C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Se o símbolo não resolvido for uma função livre do Windows Runtime, como [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize), você precisará vincular explicitamente a biblioteca [WindowsApp.lib](/uwp/win32-and-com/win32-apis) em seu projeto. A projeção de C++/WinRT depende de algumas dessas funções livres (não membro) e de pontos de entrada. Se você usar um dos modelos de projeto da [Extensão do Visual Studio (VSIX) para C++/WinRT](https://aka.ms/cppwinrt/vsix) em seu aplicativo, `WindowsApp.lib` será vinculado automaticamente. Caso contrário, você poderá usar as configurações de vínculo do projeto para inclui-lo ou fazer isso no código-fonte.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

É importante que você resolva todos os erros de vinculador possíveis vinculando **WindowsApp.lib** em vez de uma biblioteca de vínculo estático alternativa, caso contrário, seu aplicativo não passará nos testes do [Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) usados pelo Visual Studio e pela Microsoft Store para validar os envios (isso significa que, consequentemente, não será possível para o aplicativo ser ingerido com êxito pela Microsoft Store).

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>Por que estou recebendo uma exceção de "classe não registrada"?

Nesse caso, o sintoma é que&mdash;ao construir uma classe de tempo de execução ou acessar um membro estático&mdash;você vê uma exceção acionada em tempo de execução com REGDB_E_CLASSNOTREGISTERED com um valor de REGDB.

Uma das causas disso é que o Componente do Tempo de Execução do Windows não pode ser carregado. Verifique se o arquivo de metadados do componente de Tempo de Execução do Windows (`.winmd`) tem o mesmo nome do binário de componente (o `.dll`), que também é o nome do projeto e o nome do namespace raiz. Além disso, verifique se os metadados do Tempo de Execução do Windows e o binário foram copiados corretamente pelo processo de compilação para a pasta `Appx` do aplicativo de consumo. Confirme se o `AppxManifest.xml` do aplicativo de consumo (também na pasta `Appx`) contém um elemento **&lt;InProcessServer&gt;** que está declarando corretamente a classe ativável e o nome binário.

### <a name="uniform-construction"></a>Construção uniforme

Esse erro também poderá ocorrer se você tentar criar uma instância de uma classe de tempo de execução implementada localmente por meio de qualquer um dos construtores do tipo projetado (diferente de seu construtor **std::nullptr_t**). Para fazer isso, você precisará do recurso de C++/WinRT 2.0 que costuma ser chamado de construção uniforme. Se desejar aceitar esse recurso, então, para saber mais e obter exemplos de código, confira [Aceitar a construção uniforme e o acesso direto de implementação](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

Para saber como criar uma instância de suas classes de tempo de execução implementadas localmente que *não* exigem construção uniforme, consulte [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Devo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) e, em caso afirmativo, como?
Se você tiver uma classe de tempo de execução que libera recursos em seu destruidor, e essa classe de tempo de execução foi projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado ao consumo geral pelos aplicativos cliente do Windows Runtime), é recomendável implementar também **IClosable** para dar suporte ao consumo de sua classe de tempo de execução por linguagens que carecem de finalização determinística. Certifique-se de que seus recursos sejam liberados se o destruidor, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close), ou ambos forem chamados. **IClosable::Close** pode ser chamado um número arbitrário de vezes.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>É necessário chamar [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) nas classes de tempo de execução que eu consumo?
**IClosable** existe para dar suporte a linguagens que não têm finalização determinística. Portanto, você não deve chamar **IClosable::Close** de C++/WinRT, exceto em casos bem raros, envolvendo corridas de desligamento ou adoções semifatais. Se estiver usando tipos **Windows.UI.Composition**, por exemplo, você poderá se deparar com casos em que queira descartar objetos em uma sequência definida, como uma alternativa para permitir que a destruição do wrapper de C++/WinRT faça o trabalho para você.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>É possível usar LLVM/Clang para compilar com C++/WinRT?
Não oferecemos suporte à cadeia de ferramentas LLVM e Clang para C++/WinRT, mas podemos usá-la internamente para validar a conformidade de padrões do C ++/WinRT. Por exemplo, se quiser emular o que fazemos internamente, faça um teste, como descrito abaixo.

Acesse a [Página de Download de LLVM](https://releases.llvm.org/download.html), procure por **Baixar LLVM 6.0.0** > **Binários Predefinidos** e baixe **Clang para Windows (64 bits)** . Durante a instalação, opte por adicionar LLVM à variável de sistema PATH para que você possa invocá-la em um prompt de comando. Para os fins desse experimento, você pode ignorar qualquer erro de "Falha ao localizar o diretório de conjuntos de ferramentas de MSBuild" e/ou "Falha na instalação de integração do MSVC", caso os veja. Há diversas maneiras de invocar LLVM/Clang; o exemplo a seguir mostra apenas uma delas.

```cmd
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
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

Como o C++/WinRT usa recursos do padrão de C++ 17, é necessário usar qualquer sinalizador de compilador para obter esse suporte; esses sinalizadores diferem de um compilador para outro.

O Visual Studio é a ferramenta de desenvolvimento à qual oferecemos suporte e recomendamos para C++/WinRT. Confira [Suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Por que a função de implementação gerada para uma propriedade somente leitura não tem o qualificador `const`?
Ao declarar uma propriedade somente leitura em [MIDL 3.0](/uwp/midl-3/), você pode esperar que a ferramenta `cppwinrt.exe` gere um função de implementação que seja qualificada para `const` (um função const trata o ponteiro *this* como const).

Sem sombra de dúvida, é recomendável o uso de const sempre que possível, mas a ferramenta `cppwinrt.exe` em si não tenta ponderar sobre quais funções de implementação teoricamente podem ser const e quais não podem. Você pode optar por transformar em const qualquer uma das suas funções de implementação, como neste exemplo.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Você poderá remover esse qualificador `const` em **ToString** se decidir que precisa alterar o estado de algum objeto na respectiva implementação. Porém, transforme cada uma das suas funções de membro em const ou não const, não em ambos. Em outras palavras, não sobrecarregue uma função de implementação em `const`.

Além de suas funções de implementação, outro local em que const entra em cena são nas projeções de função do Windows Runtime. Considere este código.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Para a chamada a **ToString** acima, o comando **Ir para Declaração** no Visual Studio mostra que a projeção do Windows Runtime **IStringable::ToString** em C++/WinRT tem esta aparência.

```cppwinrt
winrt::hstring ToString() const;
```

As funções na projeção são const, independentemente de como você optar por qualificar a implementação delas. Nos bastidores, a projeção chama a ABI (Interface Binária de Aplicativo), que constitui uma chamada por meio de um ponteiro de interface COM. O único estado com o qual **ToString** projetado interage é esse ponteiro de interface COM; e, certamente, não há necessidade de modificar esse ponteiro, de modo que a função é const. Isso dá a você a garantia de que nada será alterado em relação à referência **IStringable** pela qual você está fazendo a chamada, além de garantir que seja possível chamar **ToString** mesmo com uma referência de const para um **IStringable**.

Entenda que esses exemplos de `const` são detalhes de implementação das projeções e implementações de C++/WinRT; eles constituem em limpeza de código para o seu benefício. Não existe nada como `const` no COM nem na ABI do Windows Runtime (para funções de membro).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Existe alguma recomendação para diminuir o tamanho do código para binários de C++/WinRT?
Ao trabalhar com objetos do Windows Runtime, você deve evitar o padrão de codificação mostrado abaixo, pois ele pode ter um impacto negativo no seu aplicativo, gerando mais código binário que o necessário.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

No mundo do Windows Runtime, o compilador é incapaz de armazenar em cache o valor de `c()` ou as interfaces para cada método que é chamado por meio de uma indireção ('.'). A menos que você interfira, isso resulta em mais chamadas virtuais e na sobrecarga de contagem de referência. O padrão acima poderia gerar facilmente duas vezes mais código do que o estritamente necessário. Em vez disso, prefira o padrão mostrado abaixo sempre que possível. Ele gera muito menos código, além de melhorar consideravelmente o desempenho do tempo de execução.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

O padrão recomendado mostrado acima se aplica não somente ao C++/WinRT, mas a todas as projeções de linguagem do Windows Runtime.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Como transformar uma cadeia de caracteres em um tipo&mdash;para navegação, por exemplo?
No final do [exemplo de código do modo de exibição de navegação](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (que é principalmente em C#), há um snippet de código do C++/WinRT mostrando como fazer isso.

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>Como resolver ambiguidades com GetCurrentTime e/ou TRY?

O arquivo de cabeçalho `winrt/Windows.UI.Xaml.Media.Animation.h` declara um método denominado **GetCurrentTime**, enquanto `windows.h` (via `winbase.h`) define uma macro denominada **GetCurrentTime**. Quando os dois entram em conflito, o compilador C++ produz "*Erro C4002: Muitos argumentos para a invocação de macro do tipo função GetCurrentTime*".

Da mesma forma, `winrt/Windows.Globalization.h` declara um método denominado **TRY**, enquanto `afx.h` define uma macro denominada **GetCurrentTime**. Quando eles entram em conflito, o compilador C++ produz o "*Erro C2334: tokens inesperados antes de '{'; ignorando o corpo aparente da função*".

Para corrigir um ou ambos os problemas, você pode fazer isso.

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> Se este tópico não solucionar sua dúvida, você pode encontrar ajuda visitando a [comunidade de desenvolvedores do Visual Studio C++](https://developercommunity.visualstudio.com/spaces/62/index.html) ou usando a marca [`c++-winrt` no Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
