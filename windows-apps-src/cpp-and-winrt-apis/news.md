---
description: Notícias e as alterações a C++/WinRT.
title: O que há de novo no C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, novidades, o que há, de novo
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 24abdb26cf884367d9a9521d30b09b443d2e4e00
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72998620"
---
# <a name="whats-new-in-cwinrt"></a>O que há de novo no C++/WinRT

Conforme as versões subsequentes do C++/WinRT são lançadas, este tópico descreve o que há de novo e o que mudou.

## <a name="news-and-changes-in-cwinrt-20"></a>Novidades e alterações em C++/WinRT 2.0

Para obter mais informações sobre a [Extensão de C++WinRT Visual Studio VSIX](https://aka.ms/cppwinrt/vsix), o [pacote do NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e a ferramenta `cppwinrt.exe`,&mdash;incluindo como adquirir e instalá-los&mdash;, veja o[suporte do Visual Studio para C++/WinRT, XAML, a extensão do VSIX e o pacote do NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Alterações à VSIX (Extensão C++WinRT Visual Studio) para a versão 2.0

- O visualizador de depuração agora dá suporte ao Visual Studio 2019, além de continuar dando suporte ao Visual Studio 2017.
- Diversas correções de bugs foram feitas.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Alterações ao pacote do NuGet Microsoft.Windows.CppWinRT para a versão 2.0

- A ferramenta `cppwinrt.exe` agora está incluída no pacote do NuGet Microsoft.Windows.CppWinRT e a ferramenta gera cabeçalhos de projeção de plataforma para cada projeto sob demanda. Assim, a ferramenta `cppwinrt.exe` não depende mais do SDK do Windows (embora a ferramenta ainda seja fornecida com o SDK por razões de compatibilidade).
- O `cppwinrt.exe` agora gera os cabeçalhos de projeção sob cada pasta intermediária específica da plataforma/configuração ($IntDir) para habilitar builds paralelos.
- O suporte de build C++/WinRT (propriedades/destinos) agora está totalmente documentado, caso você queira personalizar manualmente seus arquivos de projeto. Confira o [arquivo Leiame](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) do pacote NuGet Microsoft.Windows.CppWinRT.
- Diversas correções de bugs foram feitas.

### <a name="changes-to-cwinrt-for-version-20"></a>Alterações ao C++/WinRT para a versão 2.0

#### <a name="open-source"></a>Software livre

A ferramenta `cppwinrt.exe` pega um arquivo de metadados do Windows Runtime (`.winmd`) e gera uma biblioteca C++ padrão baseada em arquivo de cabeçalho que *projeta* as APIs descritas nos metadados. Dessa forma, você pode consumir essas APIs de seu código C++/WinRT.

Essa ferramenta agora é um projeto totalmente de software livre, disponível no GitHub. Visite [Microsoft\/xlang](https://github.com/Microsoft/xlang) e clique em **src** > **ferramenta** > **cppwinrt**.

#### <a name="xlang-libraries"></a>Bibliotecas xlang

Uma biblioteca somente cabeçalho totalmente portátil (para analisar o formato de metadados ECMA-335 usado pelo Windows Runtime) constitui a base de todo o Windows Runtime e ferramentas xlang no futuro. Em especial, também reescrevemos a ferramenta `cppwinrt.exe` desde o princípio usando as bibliotecas xlang. Isso fornece consultas de metadados muito mais precisas, resolvendo alguns problemas antigos com a projeção de linguagem C++/WinRT.

#### <a name="fewer-dependencies"></a>Menos dependências

Devido ao leitor de metadados xlang, a ferramenta `cppwinrt.exe` em si tem menos de dependências. Isso o torna muito mais flexível, além de ser utilizável em mais cenários&mdash;especialmente em ambientes de build restritos. Em especial, não depende mais de `RoMetadata.dll`.
 
Essas são as dependências para `cppwinrt.exe` 2.0.
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

Todas essas DLLs estão disponíveis não só no Windows 10, mas em todos os anteriores até o Windows 7, até mesmo no Windows Vista. Se desejar, o servidor de build antigo que executa o Windows 7 agora poderá executar `cppwinrt.exe` para gerar cabeçalhos em C++ para seu projeto. Com um pouco de trabalho, será possível até mesmo [executar o C++/WinRT no Windows 7](https://github.com/kennykerr/win7), se isso lhe interessar.

Compare a lista acima com essas dependências, que o `cppwinrt.exe` 1.0 tem.

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>O atributo `noexcept` do Windows Runtime

O Windows Runtime tem um novo atributo `[noexcept]`, que pode ser usado para decorar seus métodos e propriedades no [3.0 MIDL](/uwp/midl-3/predefined-attributes). A presença do atributo indica a ferramentas de suporte que sua implementação não gera uma exceção (nem retorna uma falha HRESULT). Isso permite que as projeções de linguagem otimizem a geração de código, evitando a sobrecarga de manipulação de exceção necessária para dar suporte a chamadas de ABI (interface binária) do aplicativo que potencialmente podem falhar.

C++/ WinRT beneficia-se disso produzindo implementações `noexcept` C++ dos códigos de consumo e de criação. Se você tiver métodos de API ou propriedades livres de falhas e estiver preocupado com o tamanho do código, poderá investigar esse atributo.

#### <a name="optimized-code-generation"></a>Geração de código otimizada

C++/ WinRT agora gera código-fonte C++ ainda mais eficiente (em segundo plano) de modo que o compilador C++ possa produzir o menor e mais eficiente código binário possível. Muitas das melhorias são direcionadas a reduzir o custo de manipulação de exceção, evitando informações de desenrolamento desnecessárias. Binários que usam grandes quantidades de código C++ do WinRT terão uma redução de aproximadamente 4% no tamanho do código. O código também é mais eficiente (é executado em menos tempo) devido à contagem de instruções reduzida.

Essas melhorias contam com um novo recurso de interoperabilidade que está disponível para você também. Todos os tipos C++/WinRT proprietários de recursos agora incluem um construtor para apropriação direta, evitando a abordagem em duas etapas anterior.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Geração de código de EH (manipulação de exceção) otimizada

Essa alteração complementa o trabalho realizado pela equipe de otimizador de C++ da Microsoft para reduzir o custo de manipulação de exceção. Se você usar intensamente ABIs (interfaces binárias de aplicativos) (por exemplo, COM) em seu código, observará muito código seguindo esse padrão.

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/ WinRT em si gera esse padrão para todas as APIs implementadas. Com milhares de funções de API, qualquer otimização aqui pode ser significativa. No passado, o otimizador não detectava que esses blocos catch são todos idênticos, portanto, ele estava duplicando muito código relacionado a cada ABI (contribuído, por sua vez, para a crença de que usar exceções no código do sistema produz binários grandes). No entanto, do Visual Studio de 2019 em diante, o compilador C++ dobra todos esses catch funclets e armazena apenas os que são exclusivos. O resultado é uma redução adicional geral de 18% no tamanho do código para binários que dependem desse padrão. Não apenas o código do EH agora é mais eficiente do que usar códigos de retorno, mas também a preocupação quanto a binários maiores é coisa do passado.

#### <a name="incremental-build-improvements"></a>Aprimoramentos de build incrementais

A ferramenta `cppwinrt.exe` agora compara a saída de um cabeçalho gerado/arquivo de origem com o conteúdo de qualquer arquivo existente no disco e apenas gravará o arquivo se ele tiver sido de fato alterado. Isso economiza um tempo considerável com E/S de disco e garante que os arquivos não sejam considerados "sujos" pelo compilador C++. O resultado é que a recompilação é evitada ou reduzida em muitos casos.

#### <a name="generic-interfaces-are-now-all-generated"></a>Interfaces genéricas agora são todas geradas

Devido ao leitor de metadados xlang, C++/WinRT agora gera todas as interfaces com parâmetros, ou genéricas, com base em metadados. Interfaces como [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) agora são geradas com base em metadados, em vez de manuscritas em `winrt/base.h`. O resultado é que o tamanho de `winrt/base.h` foi reduzido pela metade e as otimizações são geradas diretamente no código (o que era complicado de fazer com a abordagem manual).

> [!IMPORTANT]
> Interfaces como o exemplo apresentado agora aparecem em seus respectivos cabeçalhos de namespace, em vez de em `winrt/base.h`. Então, se você ainda não fez isso, precisará incluir o cabeçalho de namespace apropriado para usar a interface.

#### <a name="component-optimizations"></a>Otimizações de componente

Esta atualização adiciona suporte para várias otimizações de aceitação adicionais para C++/WinRT, descritas nas seções a seguir. Como essas otimizações são alterações significativas (e você talvez precise fazer pequenas alterações para dar suporte a elas), será preciso ativá-las explicitamente. No Visual Studio, defina a propriedade do projeto **Common Properties** > **C++/WinRT** > **Optimized** como *Yes*. Isso tem o efeito de adicionar `<CppWinRTOptimized>true</CppWinRTOptimized>` ao arquivo de projeto. E tem o mesmo efeito que adicionar o comutador `-opt[imize]` ao invocar `cppwinrt.exe` da linha de comando.

Um novo projeto (de um modelo de projeto) usará `-opt` por padrão.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Construção uniforme e acesso direto de implementação

Essas duas otimizações permitem acesso direto do componente a seus próprios tipos de implementação, mesmo quando ele está usando somente tipos projetados. Não será necessário usar [**make**](/uwp/cpp-ref-for-winrt/make), [**make_self**](/uwp/cpp-ref-for-winrt/make-self) nem [**get_self**](/uwp/cpp-ref-for-winrt/get-self) se você desejar simplesmente usar a superfície de API pública. Suas chamadas serão compiladas para chamadas diretas na implementação e elas poderão, inclusive, ser totalmente embutidas.

Para saber mais e obter exemplos de código, confira [Aceitar a construção uniforme e o acesso direto de implementação](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

##### <a name="type-erased-factories"></a>Fábricas de tipo apagado

Essa otimização evita as dependências #include no `module.g.cpp`, de modo que ele não precisa ser recompilado sempre que qualquer classe de implementação única muda. O resultado é um desempenho de build aprimorado.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>`module.g.cpp` mais inteligente e eficiente para projetos grandes com várias bibliotecas

O arquivo `module.g.cpp` agora também contém dois auxiliares combináveis adicionais chamados **winrt_can_unload_now** e **winrt_get_activation_factory**. Eles foram criados para projetos maiores em que uma DLL é composta por várias bibliotecas, cada uma com as próprias classes de runtime. Nessa situação, você precisa unir manualmente as DLLs **DllGetActivationFactory** e **DllCanUnloadNow**. Esses auxiliares tornam muito mais fácil fazer isso, evitando erros de empréstimos artificiais. O sinalizador `-lib` da ferramenta `cppwinrt.exe` também pode ser usado para fornecer seu próprio preâmbulo a cada lib individual (em vez de `winrt_xxx`) para que as funções de cada biblioteca possam ser nomeadas individualmente e, portanto, combinadas de maneira não ambígua.

#### <a name="coroutine-support"></a>Suporte a rotina combinada

O suporte a rotina combinada está incluído automaticamente. Anteriormente, o suporte residia em vários locais, o que acreditamos que era muito limitante. Então, temporariamente para v2.0, um arquivo `winrt/coroutine.h` de cabeçalho era necessário, mas agora não é mais. Uma vez que as interfaces assíncronas do Windows Runtime agora são geradas, em vez de manuscritas, elas residem em `winrt/Windows.Foundation.h`. Além de ter uma manutenção e um suporte mais fáceis, isso significa que os auxiliares de rotina combinada, como [**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground), não precisam ser incluídos no final de um cabeçalho de namespace específico. Em vez disso, podem incluir mais naturalmente suas dependências. Isso ainda permite que **resume_foreground** dê suporte não apenas a retomar em um determinado [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher), como também a retomar em um determinado [**Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Anteriormente, podia haver suporte para apenas um, não ambos, já que a definição apenas podia residir em um namespace.

Aqui está um exemplo do suporte à **DispatcherQueue**.

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

Os auxiliares de rotina combinada agora também são decorados com `[[nodiscard]]`, melhorando sua facilidade de uso. Se você esquecer-se, ou não perceber que precisa, de realizar `co_await` com relação a eles para que funcionem devido a `[[nodiscard]]`, esses erros agora produzirão um aviso do compilador.

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>Ajuda com o diagnóstico de alocações diretas (pilha)

Uma vez que os nomes de classe de implementação e projetados são os mesmos (por padrão) e diferem apenas pelo namespace, é possível confundir um do outro e acidentalmente criar uma implementação na pilha, em vez de usar a família de auxiliares [**make**](/uwp/cpp-ref-for-winrt/make). Isso pode ser difícil de diagnosticar em alguns casos, pois o objeto pode ter sido destruído enquanto as referências pendentes ainda estão disponibilizadas em versão piloto. Uma asserção agora pega isso para depurar builds. Embora a asserção não detecte a alocação de pilha dentro de uma rotina combinada, ainda é útil para capturar a maioria desses erros.

Para saber mais, confira [Diagnosticar alocações diretas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Auxiliares de captura aprimorados e delegados de variadic

Essa atualização corrige a limitação com os auxiliares de captura, oferecendo suporte a tipos projetados também. Isso aparece agora e então com as APIs de interoperabilidade do Windows Runtime, quando retornam um tipo projetado.

Essa atualização também adiciona suporte para [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) e [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) durante a criação de um delegado variadic (não do Windows Runtime).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Suporte para destruição adiada e QI segura durante a destruição

Não é incomum no destruidor de um objeto de classe de runtime chamar um método que eleva temporariamente a contagem de referência. Quando a contagem de referência volta a ser zero, o objeto é destruído uma segunda vez. Em um aplicativo XAML, você pode precisar executar uma QI ([**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))) em um destruidor para chamar uma implementação de limpeza acima ou abaixo na hierarquia. Mas a contagem de referência do objeto já atingiu zero, de modo que essa QI também constitui um salto na contagem.

Essa atualização adiciona suporte para eliminar o salto da contagem de referência, garantindo que, após atingir zero, ele nunca mais possa ser reativado, enquanto ainda permite QI para qualquer temporário necessário durante a destruição. Esse procedimento é inevitável em certos controles/aplicativos em XAML, e C++/WinRT agora é resiliente a ele.

Você pode adiar a destruição fornecendo uma função **final_release** estática em seu tipo de implementação. Esse último ponteiro restante para o objeto, na forma de um **std::unique_ptr**, é passado para seu **final_release**. Você pode, em seguida, optar por mover a propriedade desse ponteiro para algum outro contexto. É seguro executar QI com o ponteiro sem acionar uma destruição dupla. No entanto, a alteração líquida da contagem de referência deverá ser zero no ponto em que você destruir o objeto.

O valor retornado por **final_release** pode ser `void`, um objeto de operação assíncrona, como [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) ou **winrt::fire_and_forget**.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

No exemplo a seguir, uma vez que **MainPage** é liberada (para a hora final), **final_release** é chamada. Essa função passa cinco aguardando (no pool de threads) e, em seguida, é retomada usando a página **Dispatcher** (o que exige que QI/AddRef/Release funcione). Em seguida, ela limpa um recurso nesse thread da interface do usuário. E, por fim, limpa **unique_ptr**, fazendo com que o destruidor de **MainPage** realmente seja chamado. Mesmo nesse destruidor, **DataContext** é chamado, o que requer uma QI para **IFrameworkElement**.

Você não precisa implementar sua **final_release** como uma corrotina. No entanto, isso funciona e torna muito simples mover a destruição para um thread diferente, que é o que está acontecendo neste exemplo.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

Para obter mais informações, consulte [Destruição adiada](/windows/uwp/cpp-and-winrt-apis/details-about-destructors#deferred-destruction).

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Suporte aprimorado para herança de interface única no estilo COM

Assim como para programação do Windows Runtime, C++/WinRT também é usado para criar e consumir APIs somente COM. Essa atualização torna possível implementar um servidor COM em que existe uma hierarquia de interface. Isso não é necessário para o Windows Runtime, mas é necessário para algumas implementações de COM.

#### <a name="correct-handling-of-out-params"></a>Tratamento correto de parâmetros `out`

Pode ser difícil trabalhar com parâmetros `out`, particularmente matrizes do Windows Runtime. Com essa atualização, C++/WinRT é consideravelmente mais robusto e resiliente a erros quando se tratam de parâmetros e matrizes `out`, não importa se esses parâmetros chegam por meio de uma projeção de linguagem ou de um desenvolvedor COM que está usando a ABI bruta e que está cometendo o erro de não inicializar variáveis de modo consistente. Em ambos os casos, C++/WinRT agora faz a coisa certa no que se refere a entregar tipos projetados para a ABI (ao lembrar-se de lançar quaisquer recursos) e no que se refere a anular ou limpar os parâmetros que chegam pela ABI.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Os eventos agora lidam com tokens inválidos de modo confiável

A implementação [**winrt::event**](/uwp/cpp-ref-for-winrt/event) agora manipula normalmente o caso em que o método **remove** é chamado com um valor inválido de token (um valor que não está presente na matriz).

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>As variáveis locais de corrotina agora são destruídas antes que a corrotina retorne

A maneira tradicional de implementar um tipo de corrotina pode permitir que variáveis locais dentro da corrotina sejam destruídas *após* ela retornar/ser concluída (em vez de antes da suspensão final). A retomada de qualquer waiter agora é adiada até o modo de suspensão final para evitar esse problema e acumular outros benefícios.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Novidades e alterações no Windows SDK versão 10.0.17763.0 (Windows 10, versão 1809)

A tabela a seguir contém as novidades e as alterações ao C++/WinRT no SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809).

| Recursos novos ou alterados | Mais informações |
| - | - |
| **Alteração da falha**. Para que ele seja compilado, C++/WinRT não depende de cabeçalhos do SDK do Windows. | Veja [Isolamento de arquivos de cabeçalho do SDK do Windows](#isolation-from-windows-sdk-header-files) abaixo. |
| O formato de sistema de projeto do Visual Studio foi alterado. | Confira [Como redirecionar seu projeto do C++/WinRT para uma versão posterior do SDK do Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk) abaixo. |
| Há novas funções e classes base para ajudá-lo a passar um objeto de coleção para uma função do Windows Runtime ou para implementar seus próprios tipos de coleção e propriedades de coleção. | Veja [Coleções com C++/WinRT](collections.md). |
| Você pode usar a extensão de marcação [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) com suas classes de runtime C++/WinRT. | Para mais informações e exemplos de código, veja [Visão geral de associação de dados](/windows/uwp/data-binding/data-binding-quickstart). |
| O suporte ao cancelamento de uma rotina combinada permite que você registre um retorno de chamada de cancelamento. | Para obter mais informações e exemplos de código, confira [Retornos de chamada de cancelamento e cancelar uma operação assíncrona](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks). |
| Ao criar um delegado que aponta para uma função de membro, você pode estabelecer uma referência forte ou fraca ao objeto atual (em vez de um ponteiro *this* bruto) no ponto em que o manipulador é registrado. | Para obter mais informações e exemplos de código, veja a subseção **Se você usar uma função de membro como delegado** na seção [Acessar com segurança o ponteiro *this* com um delegado de manipulação de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Foram corrigidos os bugs revelados pela conformidade aprimorada do Visual Studio com o padrão C++. A cadeia de ferramentas do LLVM e Clang também é mais bem utilizada para validar a conformidade com os padrões C++/WinRT. | Você não encontrará o problema descrito em [Por que meu novo projeto não é compilado? Estou usando o Visual Studio 2017 (versão 15.8.0 ou superior) e a versão 17134 do SDK](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Outras alterações.

- **Alteração da falha**. [**WinRT::get_abi(WinRT::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) agora retorna `void*`, em vez de `HSTRING`. Você pode usar `static_cast<HSTRING>(get_abi(my_hstring));` para obter um HSTRING. Confira [Interoperação com a HSTRING de GUID da ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Alteração da falha**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) agora retorna `void**`, em vez de `HSTRING*`. Você pode usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obter um HSTRING*. Confira [Interoperação com a HSTRING de GUID da ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Alteração da falha**. HRESULT agora é projetada como **winrt::hresult**. Se você precisar de um HRESULT (para a verificação de tipo ou para dar suporte a características de tipo), poderá `static_cast` um **winrt::hresult**. Caso contrário, **winrt::hresult** será convertido em HRESULT, contanto que você inclua `unknwn.h` antes de incluir qualquer cabeçalho C++/WinRT.
- **Alteração da falha**. O GUID agora é projetado como **winrt::guid**. Para as APIs que você implementa, você deve usar **winrt::guid** para parâmetros de GUID. Caso contrário, **winrt::guid** será convertido em GUID, contanto que você inclua `unknwn.h` antes de incluir qualquer cabeçalho C++/WinRT. Confira [Interoperação com o struct de GUID da ABI](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct).
- **Alteração da falha**. O [**winrt::handle_type construtor**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) foi reforçado tornando-o explícito (agora é mais difícil escrever um código incorreto com ele). Se você precisar atribuir um valor de identificador bruto, chame a [**função handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) em vez disso.
- **Alteração da falha**. As assinaturas de **WINRT_CanUnloadNow** e **WINRT_GetActivationFactory** foram alteradas. Você não deve declarar essas funções de modo algum. Em vez disso, inclua `winrt/base.h` (que será incluído automaticamente se você incluir qualquer arquivos de cabeçalho de namespace do Windows C++/WinRT) para incluir as declarações de uma dessas funções.
- Para [**winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** foram preteridos em prol do **from_file_time/to_file_time**.
- APIs simplificadas que esperam parâmetros **IBuffer**. A maioria das APIs prefere coleções ou matrizes. Mas sentimos que devemos tornar mais fácil a chamada de APIs que dependem de **IBuffer**. Essa atualização fornece acesso direto aos dados por trás de uma implementação de **IBuffer**. Ela usa a mesma convenção de nomenclatura de dados que a usada pelos contêineres da Biblioteca Padrão do C++. Essa convenção também evita conflitos com nomes de metadados que convencionalmente começam com uma letra maiúscula.
- Geração de código aprimorada: várias melhorias para reduzir o tamanho do código, melhorar o inlining e otimizar o cache de alocador.
- Removida a recursão desnecessária. Quando a linha de comando refere-se a uma pasta, em vez de a um determinado `.winmd`, a ferramenta `cppwinrt.exe` não pesquisa recursivamente arquivos `.winmd`. A ferramenta `cppwinrt.exe` agora também lida com duplicatas de modo mais inteligente, tornando-a mais resiliente a erros do usuário e a arquivos `.winmd` mal-formados.
- Ponteiros inteligentes reforçados. Anteriormente, os revogadores de evento falhavam em revogar quando movidos-recebiam um novo valor. Isso ajudou a descobrir um problema em que classes de ponteiro inteligente não estavam manipulando de modo confiável a autoatribuição; como raiz no [**modelo de struct winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** foi corrigido e os revogadores de evento foram corrigidos para tratar a semântica de movimentação corretamente para revogarem após a atribuição.

> [!IMPORTANT]
> Alterações importantes foram feitas à [VSIX (Extensão do Visual Studio em C++/WinRT)](https://aka.ms/cppwinrt/vsix), ambas na versão 1.0.181002.2 e depois na versão 1.0.190128.4. Para obter detalhes sobre essas alterações e como elas afetam seus projetos existentes, veja [Suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) e [Versões anteriores da extensão VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Isolamento de arquivos de cabeçalho do SDK do Windows

Essa é possivelmente uma alteração da falha para seu código.

Para que ela seja compilada, C++/WinRT não depende mais de arquivos de cabeçalho do SDK do Windows. Arquivos de cabeçalho na biblioteca CRT (tempo de execução do C) e a STL (Biblioteca de Modelo Padrão) em C++ não incluem nenhum cabeçalho do SDK do Windows. E isso melhora a conformidade com os padrões, evita dependências acidentais e reduz significativamente o número de macros contra os quais você precisa se proteger.

Essa independência significa que C++/WinRT agora é mais portátil e em conformidade com padrões, e amplia a possibilidade de ele se tornar uma biblioteca multiplataforma e multicompilador. Isso também significa que os cabeçalhos C++/WinRT não estão afetando macros negativamente.

Se você antes deixava para C++/WinRT incluir quaisquer cabeçalhos do Windows em seu projeto, agora você mesmo precisará incluí-los. Sempre é, de qualquer forma, uma melhor prática incluir explicitamente os cabeçalhos dos quais você depende e não deixá-los para outra biblioteca incluí-los para você.

Atualmente, as únicas exceções ao isolamento de arquivo de cabeçalho do SDK do Windows são intrínsecos e numéricos. Não há nenhum problema conhecido nessas últimas dependências restantes.

Em seu projeto, você poderá habilitar novamente a interoperabilidade com os cabeçalhos do SDK do Windows, se precisar. Por exemplo, convém implementar uma interface COM (enraizada em [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)). Para este exemplo, inclua `unknwn.h` antes de incluir qualquer cabeçalho C++/WinRT. Fazer isso faz com que a biblioteca de base C++/WinRT habilite vários ganchos para dar suporte a interfaces COM clássicas. Para obter um exemplo de código, veja [Criar componentes COM usando C++/WinRT](author-coclasses.md). Da mesma forma, inclua explicitamente outros cabeçalhos do SDK do Windows que declarem tipos e/ou funções que você deseja chamar.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Como redirecionar seu projeto do C++/WinRT para uma versão posterior do SDK do Windows

O método para seu projeto que provavelmente resultará no menor número de problemas de compilador e vinculador também é o mais trabalhoso. Esse método envolve criar um novo projeto (direcionado à versão do SDK do Windows de sua escolha) e, em seguida, copiar arquivos para o seu novo projeto do seu antigo. Haverá seções de seus arquivos `.vcxproj` e `.vcxproj.filters` antigos que você pode simplesmente copiar para não precisar adicionar arquivos no Visual Studio.

No entanto, há duas outras maneiras de redirecionar seu projeto no Visual Studio.

- Vá para a propriedade do projeto **Geral** \> **Versão do SDK do Windows** e selecione **Todas as Configurações** e **Todas as Plataformas**. Defina a **Versão do SDK do Windows** para a versão que você deseja direcionar.
- Em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do projeto, clique em **Redirecionar Projetos**, escolha uma ou mais versões para as quais você deseja direcionar e clique em **OK**.

Se você encontrar qualquer erro de compilador ou vinculador depois de usar qualquer um desses dois métodos, poderá testar a solução de limpeza (**Build** > **Limpar Solução** e/ou excluir manualmente todos os arquivos e pastas temporários) antes de tentar compilar novamente.

Se o compilador C++ produzir "*error C2039: 'IUnknown': não é um membro de '\`namespace global''* ", então adicione `#include <unknwn.h>` na parte superior do seu arquivo `pch.h` (antes de incluir qualquer cabeçalho C++/WinRT).

Você também precisará adicionar `#include <hstring.h>` depois disso.

Se o vinculador C++ produzir "*erro LNK2019: símbolo externo não resolvido _WINRT_CanUnloadNow@0 referenciado na função _VSDesignerCanUnloadNow@0* ", você poderá resolver isso adicionando `#define _VSDESIGNER_DONT_LOAD_AS_DLL` a seu arquivo `pch.h`.
