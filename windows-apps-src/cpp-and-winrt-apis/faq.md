---
description: Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT.
title: Perguntas frequentes sobre C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, frequente, pergunta, questões, faq
ms.localizationpriority: medium
ms.openlocfilehash: 914cf884b97d14af523cc61b0fcce719104783ba
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721692"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Perguntas frequentes sobre C++/WinRT
Respostas para perguntas que você provavelmente tem sobre a criação e consumo de APIs de tempo de execução do Windows com [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Caso sua pergunta seja sobre uma mensagem de erro já vista, consulte também o tópico de [Solução de problemas de C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Como redirecionar meu C++projeto /WinRT para uma versão posterior do SDK do Windows?
Ver [como redirecionar seu C++projeto /WinRT para uma versão posterior do SDK do Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>Por que não será meu novo projeto compilado, agora que migrei para C++WinRT 2.0?
Para o conjunto completo de alterações (incluindo alterações significativas), consulte [notícias, as alterações e, em C++2.0 WinRT](news.md#news-and-changes-in-cwinrt-20). Por exemplo, se você estiver usando um intervalo baseado `for` em uma coleção de tempo de execução do Windows, em seguida, você agora precisará `#include <winrt/Windows.Foundation.Collections.h>`.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Por que meu novo projeto não será compilado? Usando o Visual Studio 2017 (versão 15.8.0 ou superior) e o SDK versão 17134
Se você estiver usando o Visual Studio 2017 (versão 15.8.0 ou superior) e direcionados ao SDK do Windows versão 10.0.17134.0 (Windows 10, versão 1803), em seguida, um recém-criado C++/projeto WinRT pode falhar ao compilar com o erro "*erro c3861:: 'from_abi': Identificador não encontrado*"e com outros erros originados no *base.h*. A solução é o destino um posterior (mais compatível) versão do SDK do Windows, ou defina a propriedade de projeto **C/C++**  > **idioma** > **modo de conformidade: Não** (Além disso, se **/permissive--** é exibido na propriedade do projeto **C/C++**  > **linha de comando** sob **opções adicionais** , em seguida, excluí-lo).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Como faço para resolver o erro de build "C + c++ /CLI VSIX WinRT não fornece suporte de build do projeto.  Adicione uma referência de projeto para o pacote Microsoft.Windows.CppWinRT Nuget"?
Instalar o **Microsoft.Windows.CppWinRT** pacote do NuGet em seu projeto. Para obter detalhes, consulte [versões anteriores da extensão do VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>Quais são os requisitos para o C++WinRT extensão VSIX (Visual Studio)?
Para versão 1.0.190128.4 da extensão do VSIX e versões posteriores, consulte [suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Para outras versões, consulte [versões anteriores da extensão do VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>O que é uma *classe de tempo de execução*?
Uma classe de tempo de execução é um tipo que pode ser ativado e consumido por meio de interfaces COM modernas, normalmente entre limites executáveis. No entanto, uma classe de tempo de execução também pode ser usada dentro da unidade de compilação que a implementa. Você declara uma classe de tempo de execução em idioma de definição da Interface (IDL) e pode implementá-la em C++ padrão usando C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>O que *o tipo projetado* e *o tipo de implementação* significam?
Se você estiver apenas *consumindo* uma classe do Windows Runtime (classe do tempo de execução), então você estará lidando exclusivamente com *tipos projetados*. C++/WinRT é uma *projeção de linguagem*, portanto, tipos projetados fazem parte da superfície do Windows Runtime sendo *projetado* em C++ com C++/WinRT. Para obter mais detalhes, consulte [consumir APIs com C++/WinRT](consume-apis.md).

O *tipo de implementação* contém a implementação de uma classe de tempo de execução, portanto, ele só está disponível no projeto que implementa a classe de tempo de execução. Quando você estiver trabalhando em um projeto que implementa classes de tempo de execução (um projeto do componente do Tempo de Execução do Windows ou um projeto que usa interface do usuário XAML), é importante estar familiarizado com a distinção entre o tipo de implementação de uma classe de tempo de execução e o tipo projetado que representa a classe de tempo de execução projetada em C++/WinRT. Para obter mais detalhes, consulte [Criar APIs com C ++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>É necessário declarar um construtor em minha IDL da classe do tempo de execução?
Somente se a classe de tempo de execução for projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado a consumo geral pelos aplicativos de cliente do Windows Runtime). Para obter detalhes completos sobre a finalidade e consequências da declaração de construtores na IDL, consulte [Construtores de classe de tempo de execução](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Por que o vinculador me fornece um "LNK2019: Erro de símbolo externo não resolvido"?
Se o símbolo não resolvido for uma API dos cabeçalhos de namespace do Windows da projeção do C++/WinRT (no namespace **winrt**), a declaração da API é adiada em um cabeçalho que você incluiu, mas sua definição está em um cabeçalho ainda não incluído. Inclua o cabeçalho nomeado para o namespace da API e compile-o novamente. Para obter mais informações, consulte [Cabeçalhos de projeção do C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Se o símbolo não resolvido é uma função de tempo de execução do Windows gratuita, como [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize), em seguida, você precisará vincular explicitamente o [WindowsApp.lib](/uwp/win32-and-com/win32-apis) biblioteca abrangente em seu projeto. A projeção de C++/WinRT depende de algumas dessas funções (não membro) livres e pontos de entrada. Se você usar um dos modelos de projeto da [Extensão do Visual Studio (VSIX) do C++/WinRT](https://aka.ms/cppwinrt/vsix) para seu aplicativo, `WindowsApp.lib` é associado automaticamente para você. Caso contrário, você pode usar as configurações de associação do projeto para inclui-lo ou fazer isso no código-fonte.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

É importante que você resolva quaisquer erros de vinculador que é possível por meio da vinculação **WindowsApp.lib** em vez de uma biblioteca de vínculo estático alternativa, caso contrário, seu aplicativo não passa a [deKitdecertificaçãodeaplicativosdoWindows](../debug-test-perf/windows-app-certification-kit.md) testes usados pelo Visual Studio e por que a Microsoft Store para validar os envios (Isso significa que, consequentemente, não será possível para o aplicativo a ser processado com êxito para a Microsoft Store).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Devo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) e, em caso afirmativo, como?
Se você tiver uma classe de tempo de execução que libera recursos em seu destruidor, e essa classe de tempo de execução foi projetada para ser consumida fora de sua unidade de compilação de implementação (é um componente do Tempo de Execução do Windows direcionado a consumo geral pelos aplicativos de cliente do Windows Runtime), então, recomendamos que você também implemente **IClosable** para dar suporte ao consumo de sua classe de tempo de execução para idiomas que não têm finalização determinística. Certifique-se de que seus recursos sejam liberados se o destruidor [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close), ou ambos são chamados. **IClosable::Close** pode ser chamado um número arbitrário de vezes.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>É necessário chamar [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) nas classes de tempo de execução que eu consumo?
**IClosable** existe para dar suporte a idiomas que não têm finalização determinística. Portanto, você não deve chamar **IClosable::Close** a partir de C++/WinRT, exceto em casos bem raros, envolvendo corridas de desligamento ou amplexos semifatais. Se você estiver usando, por exemplo, tipos **Windows.UI.Composition**, em seguida, você pode encontrar casos em que você deseja descartar objetos em uma sequência de conjunto, como uma alternativa para permitir a destruição do wrapper de C++/WinRT seja feita para você.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>É possível usar LLVM/Clang para compilar com C++/WinRT?
Não oferecemos suporte à cadeia de ferramentas LLVM e Clang para C++/WinRT, mas podemos usá-la internamente para validar a conformidade de padrões do C ++/WinRT. Por exemplo, se você quisesse emular o que fazemos internamente, faça um teste, como descrito abaixo.

Acesse a [a página de Download de LLVM](https://releases.llvm.org/download.html), procure por **Baixar LLVM 6.0.0** > **Binários pré-criados** e baixe **Clang para Windows (64 bits)** . Durante a instalação, opte por adicionar LLVM à variável de sistema PATHA para que você possa invocá-la em um prompt de comando. Para os fins desse experimento, você pode ignorar qualquer "Falha ao localizar o diretório de conjuntos de ferramentas de MSBuild" e/ou "Falha na instalação de integração do MSVC" ao encontrá-las. Há diversas maneiras de invocar LLVM/Clang; o exemplo a seguir mostra apenas uma maneira.

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

Como o C++/WinRT usa recursos do C++ 17 padrão, é necessário usar qualquer sinalizador de compilador para obter esse suporte; esses sinalizadores diferem de um compilador para outro.

O Visual Studio é a ferramenta de desenvolvimento a qual oferecemos suporte e recomendamos para C++/WinRT. Ver [suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Por que a função de implementação gerado para uma propriedade somente leitura não tem o `const` qualificador?
Quando você declara uma propriedade somente leitura em [MIDL 3.0](/uwp/midl-3/), você pode esperar a `cppwinrt.exe` ferramenta para gerar uma função de implementação para você que está `const`-qualificado (const função trata o *esse* ponteiro como const).

Certamente, recomendamos o uso de const sempre que possível, mas o `cppwinrt.exe` ferramenta em si não tente ponderar sobre qual implementação as funções podem perfeitamente ser constantes, e que talvez não. Você pode optar por fazer qualquer uma das suas funções de implementação const, como neste exemplo.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Você poderá remover isso `const` qualificador **ToString** se você decidir que é preciso alterar algum estado de objeto em sua implementação. Mas cada um dos membro de sua funções constantes ou não const, não ambos. Em outras palavras, não sobrecarregar uma função de implementação de `const`.

Além de suas funções de implementação, outro outros colocar onde const entra na imagem é em projeções de função de tempo de execução do Windows. Considere este código.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Para a chamada para **ToString** acima, o **ir para declaração** comando do Visual Studio mostra que a projeção de tempo de execução do Windows **IStringable::ToString** em C++/WinRT tem esta aparência.

```cppwinrt
winrt::hstring ToString() const;
```

Funções na projeção são const, independentemente de como você optar por qualificar sua implementação delas. Nos bastidores, a projeção chama a interface binária de aplicativo (ABI), quais valores de uma chamada por meio de um ponteiro de interface COM. O único estado que o projetado **ToString** interage com é esse ponteiro de interface COM; e ele certamente tem há necessidade de modificar esse ponteiro, portanto, a função é const. Isso lhe dá a garantia de que ele não altera nada sobre o **IStringable** referência que você está chamando por meio e garante que você pode chamar **ToString** mesmo com uma referência const para um  **IStringable**.

Entender que esses exemplos de `const` são os detalhes de implementação de C++/WinRT projeções e implementações; eles constituem higiene de troca de código para o seu benefício. Há algo como `const` na ABI do tempo de execução do Windows (para funções de membro) nem COM.

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Você tem todas as recomendações para diminuir o tamanho do código para C++/WinRT binários?
Ao trabalhar com objetos de tempo de execução do Windows, você deve evitar o padrão de codificação mostrado abaixo, pois ele pode ter um impacto negativo em seu aplicativo, fazendo com que o código binário mais que o necessário a ser gerado.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

No mundo do tempo de execução do Windows, o compilador não puder armazenar em cache o valor de `c()` ou as interfaces para cada método que é chamado por meio de uma indireção ('. '). A menos que você intervenha, que resulta em mais chamadas virtuais e sobrecarga de contagem de referência. O padrão acima poderia gerar facilmente duas vezes mais código como estritamente necessário. Em vez disso, prefira o padrão mostrado abaixo onde quer que você pode. Ele gera muito menos código e ele também drasticamente pode melhorar o desempenho do tempo de execução.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

O padrão recomendado mostrado acima se aplica não somente ao C++/WinRT, mas para todas as projeções de linguagem do tempo de execução do Windows.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Como transformar uma cadeia de caracteres em um tipo&mdash;para navegação, por exemplo?
No final o [exemplo de código do modo de exibição de navegação](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (que é principalmente em C#), há um C++/WinRT trecho de código mostrando como fazer isso.

> [!NOTE]
> Se este tópico não responda sua pergunta, você pode encontrar ajuda visitando a [comunidade de desenvolvedores do Visual Studio C++](https://developercommunity.visualstudio.com/spaces/62/index.html), ou usando o [ `c++-winrt` marca no Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
