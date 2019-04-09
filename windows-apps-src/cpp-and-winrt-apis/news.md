---
description: Notícias e as alterações a C++/WinRT.
title: O que há de novo no C++/WinRT
ms.date: 04/02/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, notícias, o que do, new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8ee10450a7a346c1ae032240aaecc65e7f87822d
ms.sourcegitcommit: 940645c705865ba9635ccae2da9d917420faf608
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58812605"
---
# <a name="whats-new-in-cwinrt"></a>O que há de novo no C++/WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>Notícias, as alterações e, em C++2.0 WinRT

Para obter mais informações sobre o [ C++WinRT Visual Studio VSIX (extensão)](https://aka.ms/cppwinrt/vsix), o [pacote do Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)e o de `cppwinrt.exe` ferramenta&mdash;incluindo como adquirir e instalá-los&mdash;ver [suporte do Visual Studio para C++/WinRT, XAML, a extensão do VSIX e o pacote NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Altera para o C++WinRT extensão VSIX (Visual Studio) para a versão 2.0

- O Visualizador de depuração agora dá suporte ao Visual Studio de 2019; bem como continuar dar suporte ao Visual Studio 2017.
- Diversas correções de bugs foram feitas.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Alterações para o pacote do Microsoft.Windows.CppWinRT NuGet para a versão 2.0

- O `cppwinrt.exe` ferramenta agora está incluída no pacote Microsoft.Windows.CppWinRT NuGet e a ferramenta gera cabeçalhos de projeção de plataforma para cada projeto sob demanda. Consequentemente, o `cppwinrt.exe` ferramenta não depende mais o SDK do Windows (embora, a ferramenta ainda seja fornecido com o SDK por razões de compatibilidade).
- `cppwinrt.exe` Agora gera os cabeçalhos de projeção na pasta de cada intermediário de específico da plataforma/configuração ($IntDir) para permitir compilações paralelas.
- O C++suporte de build /WinRT (Propriedades/destinos) agora está totalmente documentado, caso você deseje personalizar manualmente seus arquivos de projeto. Ver [pacote do NuGet Microsoft.Windows.CppWinRT](https://github.com/Microsoft/xlang/tree/user/sjones/cppwinrt_nuget/src/package/nuget).
- Diversas correções de bugs foram feitas.

### <a name="changes-to-cwinrt-for-version-20"></a>Altera para C++/WinRT para a versão 2.0

#### <a name="open-source"></a>Código-fonte aberto

O `cppwinrt.exe` ferramenta leva um tempo de execução do Windows de metadados (`.winmd`) de arquivos e gera de um padrão baseado em arquivo de cabeçalho C++ biblioteca de que *projetos* as APIs descritas nos metadados. Dessa forma, você pode consumir essas APIs de sua C++/WinRT código.

Essa ferramenta agora é um projeto de código-fonte totalmente aberto, disponível no GitHub. Visite [Microsoft\/xlang](https://github.com/Microsoft/xlang)e, em seguida, clique em para **src** > **ferramenta** > **cppwinrt**.

#### <a name="xlang-libraries"></a>bibliotecas de XLANG

Uma biblioteca somente cabeçalho totalmente portáteis (para analisar o formato de metadados do ECMA-335 usado pelo tempo de execução do Windows) constitui a base de todo o tempo de execução do Windows e xlang das ferramentas no futuro. Em especial, estamos também reescreveu o `cppwinrt.exe` ferramenta desde o usando as bibliotecas xlang. Isso fornece consultas de metadados muito mais precisas, resolver alguns problemas antigos com o C++/WinRT projeção de linguagem.

#### <a name="fewer-dependencies"></a>Menos dependências

Devido ao leitor de metadados xlang, o `cppwinrt.exe` ferramenta em si tem menos de dependências. Isso torna muito mais flexível, além de ser utilizável em cenários mais&mdash;especialmente em restrita criar ambientes. Em especial, ele não depende mais `RoMetadata.dll`.
 
Essas são as dependências para `cppwinrt.exe` 2.0.
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite. dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

Essas dependências, ao contrário que `cppwinrt.exe` 1.0 tem.

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite. dll
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

#### <a name="the-windows-runtime-noexcept-attribute"></a>O tempo de execução do Windows `noexcept` atributo

O tempo de execução do Windows tem um novo `[noexcept]` atributo, que podem ser usados para decorar seus métodos e propriedades no [3.0 MIDL](/uwp/midl-3/predefined-attributes). A presença do atributo indica para dar suporte a ferramentas que sua implementação não lança uma exceção (nem retornar uma falha HRESULT). Isso permite que as projeções de linguagem otimizar a geração de código, evitando a sobrecarga de manipulação de exceção que é necessário para dar suporte a chamadas ABI (interface binária) do aplicativo que potencialmente podem falhar.

C++/ WinRT se beneficia disso por meio da produção C++ `noexcept` implementações de tanto o consumo e a criação de código. Se você tiver métodos de API ou propriedades que estão livres de falha e você estiverem preocupadas sobre o tamanho do código, você pode investigar esse atributo.

#### <a name="optimized-code-generation"></a>Geração de código otimizada

C++/ WinRT agora gera ainda mais eficiente C++ código-fonte (em segundo plano) até que o C++ compilador pode produzir o código binário menor e mais eficiente possível. Muitas das melhorias são direcionadas a redução do custo de manipulação de exceção, evitando desnecessárias informações de desenrolamento. Binários que usam grandes quantidades de C++código do WinRT verá aproximadamente uma redução de 4% no tamanho do código. O código também é mais eficiente (ele é executado mais rápido) porque a contagem de instruções reduzido.

Essas melhorias contam com um novo recurso de interoperabilidade que está disponível para você, também. Todos os C++/tipos WinRT que são proprietários de recursos agora incluem um construtor para apropriar-se diretamente, evitando a abordagem em duas etapas anteriores.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Otimizado manipulação de exceção (EH)-geração de código

Essa alteração complementa o trabalho realizado pelo Microsoft C++ equipe de otimizador para reduzir o custo de manipulação de exceção. Se você usar intensamente aplicativo ABIs (interfaces binárias) (por exemplo, COM) em seu código, você observará um monte de código a seguir esse padrão.

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

C++/ WinRT em si gera esse padrão para todas as APIs que é implementada. Com milhares de funções de API, qualquer otimização aqui pode ser significativa. No passado, o otimizador não detecta os blocos catch são idênticos, portanto, ele foi duplicando um monte de código ao redor de cada ABI (que por sua vez contribuído à crença de que o uso de exceções no código do sistema produz binários grandes). No entanto, do Visual Studio de 2019, o C++ compilador dobra todas essas catch funclets e armazena apenas aqueles que são exclusivos. O resultado é uma redução de 18% adicional e geral no tamanho do código para binários que dependem desse padrão. Não apenas o código do EH agora é mais eficiente do que usar códigos de retorno, mas também a preocupação sobre binários maiores agora é coisa do passado.

#### <a name="incremental-build-improvements"></a>Aprimoramentos de build incremental

O `cppwinrt.exe` ferramenta agora compara a saída de um arquivo de cabeçalho gerado/origem contra o conteúdo de qualquer arquivo existente no disco, e ele apenas grava o arquivo se o arquivo foi alterado na verdade. Isso economiza um tempo considerável com e/s de disco e garante que os arquivos não são considerados "sujos", o C++ compilador. O resultado é que a recompilação é evitada ou reduzida, em muitos casos.

#### <a name="generic-interfaces-are-now-all-generated"></a>Interfaces genéricas agora são gerados

Devido ao leitor de metadados xlang, C++/WinRT agora gera todas as interfaces com parâmetros ou genéricas, de metadados. Interfaces como [Windows::Foundation::Collections::IVector\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_) são agora gerado a partir de metadados em vez de manuscritas `winrt/base.h`. O resultado é que o tamanho de `winrt/base.h` foi reduzido pela metade, e que as otimizações são geradas com o botão direito no código (que era complicado fazê-lo com a abordagem manualmente).

> [!IMPORTANT]
> Interfaces, como o exemplo fornecido agora aparecem em seus cabeçalhos do respectivo namespace, em vez de em `winrt/base.h`. Então, se você ainda não fez isso, você terá que incluir o cabeçalho de namespace apropriado para usar a interface.

#### <a name="component-optimizations"></a>Otimizações de componente

Essa atualização adiciona suporte para vários aceitação otimizações adicionais para C++/WinRT, descritos nas seções a seguir. Como essas otimizações são significativas alterações (o que você talvez precise fazer pequenas alterações para dar suporte a), você precisará ativá-los explicitamente usando o `cppwinrt.exe` da ferramenta `-opt` sinalizador.

Um novo projeto (de um modelo de projeto) usará `-opt` por padrão.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Construção uniforme e acesso direto de implementação

Essas duas otimizações permitem acesso direto de componente aos seus próprios tipos de implementação, mesmo quando ele está usando somente tipos projetados. Não é necessário usar [ **tornar**](/uwp/cpp-ref-for-winrt/make), [ **make_self**](/uwp/cpp-ref-for-winrt/make-self), nem [ **get_self** ](/uwp/cpp-ref-for-winrt/get-self) se você quiser simplesmente usar a superfície de API pública. Suas chamadas compilará chamadas diretas a implementação e aqueles ainda podem ser totalmente embutidas.

##### <a name="type-erased-factories"></a>Fábricas de tipo apagado

Essa otimização evita o #include dependências no `module.g.cpp` , de modo que ele não precisa ser recompilado toda vez que qualquer classe de implementação único ocorre alterar. O resultado é o desempenho de compilação aprimorados.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>Mais inteligentes e mais eficiente `module.g.cpp` para projetos grandes com várias bibliotecas

O `module.g.cpp` arquivo agora também contém dois auxiliares combináveis adicionais, chamados **winrt_can_unload_now**, e **winrt_get_activation_factory**. Eles foram criados para projetos maiores em que uma DLL é composta de um número de bibliotecas, cada um com suas próprias classes de tempo de execução. Nessa situação, você precisa fixá manualmente a DLL **DllGetActivationFactory** e **DllCanUnloadNow**. Esses auxiliares torná-lo muito mais fácil para fazer isso, evitando erros de empréstimos artificiais. O `cppwinrt.exe` da ferramenta `-lib` sinalizador também pode ser usado para fornecer seu próprio Preâmbulo de cada lib individual (em vez de `winrt_xxx`) para que funções da biblioteca para cada podem ser chamadas individualmente e, portanto, combinadas de maneira não ambígua.

#### <a name="new-winrtcoroutineh-header"></a>Novo `winrt/coroutine.h` cabeçalho

O `winrt/coroutine.h` cabeçalho é o novo lar para todos os C++suporte de corrotina do /WinRT. Anteriormente, esse suporte residia em alguns lugares, que acreditamos que era muito limitado. Uma vez que as interfaces de async de tempo de execução do Windows agora são geradas, em vez de manuscritas, que residem em `winrt/Windows.Foundation.h`. Além de ser mais fácil de manter e com suporte, isso significa que corrotina auxiliares, como [ **resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) não precisa ser incluídas ao final de um cabeçalho de namespace específico. Em vez disso, eles podem incluir mais naturalmente suas dependências. Isso ainda permite **resume_foreground** para dar suporte à retomada não apenas em um determinado [ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher), mas ele pode também dão suporte a retomada em um determinado [ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Anteriormente, somente um pode ter suporte; mas não ambos, desde que a definição apenas pode residir em um namespace.

Aqui está um exemplo de como o **DispatcherQueue** dão suporte.

```cppwinrt
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

Os auxiliares de corrotina agora também são decorados com `[[nodiscard]]`e, portanto, melhorando sua facilidade de uso. Se você esquecer de (ou não percebem que você precise) `co_await` -los para trabalhar e, devido a `[[nodiscard]]`, esses erros agora produzem um aviso do compilador.

#### <a name="help-with-diagnosing-stack-allocations"></a>Ajudar a diagnosticar as alocações de pilha

Como os nomes de classe projetados e implementação são os mesmos (por padrão) e diferem apenas pelo namespace, é possível confundir um do outro e criar acidentalmente uma implementação na pilha, em vez de usar o [ **tornar** ](/uwp/cpp-ref-for-winrt/make) família de auxiliares. Isso pode ser difícil de diagnosticar em alguns casos, porque o objeto pode ter sido destruído, enquanto as referências pendentes ainda estão em trânsito. Uma asserção agora pega isso, para compilações de depuração. Enquanto a asserção não detecta a alocação de pilha dentro de uma co-rotina, mesmo assim, é útil para capturar a maioria dos tais erros.

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Os auxiliares de captura aprimorada e delegados de variadic

Essa atualização corrige a limitação com os auxiliares de captura, oferecendo suporte a tipos projetados também. Isso aparece agora e, em seguida, com as APIs de interoperabilidade de tempo de execução do Windows, quando eles retornam um tipo projetado.

Essa atualização também adiciona suporte para [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) e [ **get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) durante a criação de um delegado variadic (não - Windows Runtime).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Suporte para QI de segurança durante a destruição e destruição adiada

Um aplicativo XAML pode entrar em si em dificuldade devido à necessidade de executar uma [ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) em um destruidor, para chamar uma implementação de limpeza para cima ou para baixo da hierarquia. No entanto, que a chamada envolve um QI após a contagem de referência do objeto já tem chegou a zero. Essa atualização adiciona suporte para a contagem de referência, garantindo que, após ele atingir zero nunca podem ser reativado; para a supressão de salto enquanto ainda permite que QI para qualquer temporário que é necessário durante a destruição. Esse procedimento é inevitável em certos controles/aplicativos em XAML, e C++/WinRT agora é resistente a ele.

Destruição pode ser adiada, fornecendo um estático **final_release** de função e movendo a propriedade da **unique_ptr** para algum outro contexto.

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

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

No exemplo a seguir, uma vez a **MainPage** for lançada (por hora final), **final_release** é chamado. Que função gasta aguardando (no pool de threads) de cinco segundos e, em seguida, ele será retomado usando a página **Dispatcher** (o que requer QI/AddRef/Release funcione). Então, limpa os **unique_ptr**, que faz com que o **MainPage** destruidor realmente seja chamado. Até aqui, **DataContext** é chamado, que requer um QI para **IFrameworkElement**. Obviamente, você não precisa implementar sua **final_release** como uma co-rotina. Mas que funciona e torna muito simples mover a destruição para um thread diferente.

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Suporte aprimorado para herança única interface de estilo COM

Bem como para programação de tempo de execução do Windows, C++/WinRT também é usado para criar e consumir APIs somente COM. Essa atualização torna possível implementar um servidor COM em que existe uma hierarquia de interface. Isso não é necessário para o tempo de execução do Windows; mas ele é necessário para algumas implementações de COM.

#### <a name="correct-handling-of-out-params"></a>Correto de tratamento de `out` param. autom.

Ele pode ser difícil de trabalhar com `out` params; particularmente matrizes de tempo de execução do Windows. Com essa atualização, C++/WinRT é consideravelmente mais robusto e flexível para erros quando se trata `out` params e matrizes; se esses parâmetros chegam por meio de uma projeção de linguagem, ou de um desenvolvedor COM quem está usando a ABI bruta e quem está fazendo o erro de não inicialização de variáveis consistentemente. Em ambos os casos, C++/WinRT agora faz a coisa certa quando se trata de entregando tipos projetados para a ABI (lembrando liberar quaisquer recursos), e quando se trata de anulação ou limpeza dos parâmetros que chegam através da ABI.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Eventos agora lidar com tokens inválidos confiável

O [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) implementação agora manipula normalmente o caso onde seu **remover** método é chamado com um valor inválido de token (um valor que não está presente no matriz).

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>Locais de corrotina agora são destruídos antes que a corrotina retorna

A maneira tradicional de implementação de um tipo de co-rotina pode permitir locais dentro da corrotina a ser destruído *após* ela retorna/ser concluída (em vez de antes da suspensão final). A retomada de qualquer garçom agora é adiada até o modo de suspensão final, para evitar esse problema e acumular outros benefícios.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Notícias e as alterações, no Windows SDK versão 10.0.17763.0 (Windows 10, versão 1809)

A tabela a seguir contém notícias e muda para [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) na versão disponível mais recente do SDK do Windows, que é 10.0.17763.0 (Windows 10, versão 1809). Essas alterações também podem estar presentes em versões posteriores do SDK Insider Preview.

| Recursos novos ou alterados | Mais informações |
| - | - |
| **Alteração significativa**. Para que ele seja compilado, C++/WinRT não depende de cabeçalhos do SDK do Windows. | Ver [isolamento dos arquivos de cabeçalho do SDK do Windows](#isolation-from-windows-sdk-header-files), abaixo. |
| O formato de sistema de projeto do Visual Studio foi alterado. | Ver [como redirecionar C + c++ /CLI WinRT projeto para uma versão posterior do SDK do Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), abaixo. |
| Há novas funções e classes base para ajudá-lo a passar um objeto de coleção para uma função de tempo de execução do Windows, ou para implementar seus próprios tipos de coleção e de propriedades de coleção. | Ver [coleções com C++/WinRT](collections.md). |
| Você pode usar o [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) extensão de marcação com C + c++ /CLI classes de tempo de execução do WinRT. | Para obter mais informações e exemplos de código, consulte [visão geral da vinculação de dados](/windows/uwp/data-binding/data-binding-quickstart). |
| Suporte ao cancelamento de uma corrotina permite que você registre um retorno de chamada de cancelamento. | Para obter mais informações e exemplos de código, consulte [Cancelando uma operação assíncrona e retornos de chamada de cancelamento](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Ao criar um delegado que aponta para uma função de membro, você pode estabelecer um forte ou uma referência fraca ao objeto atual (em vez de um brutos *isso* ponteiro) no ponto em que o manipulador é registrado. | Para obter mais informações e exemplos de código, consulte o **se você usar uma função de membro como representante** subseção na seção [com segurança ao acessar o *isso* ponteiro com um representante de manipulação de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bugs corrigidos que foram revelados por melhoria na conformidade do Visual Studio para o padrão C++. A cadeia de ferramentas do LLVM e Clang melhor também é utilizada para validar C++conformidade com os padrões do /WinRT. | Você não encontrará o problema descrito no [por que meu novo projeto não será compilado? Usando o Visual Studio 2017 (versão 15.8.0 ou superior) e o SDK versão 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Outras alterações.

- **Alteração significativa**. [**WinRT::get_abi(WinRT::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) agora retorna `void*` em vez de `HSTRING`. Você pode usar `static_cast<HSTRING>(get_abi(my_hstring));` para obter um HSTRING.
- **Alteração significativa**. [**WinRT::put_abi(WinRT::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) agora retorna `void**` em vez de `HSTRING*`. Você pode usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obter um HSTRING *.
- **Alteração significativa**. HRESULT agora é projetada como **winrt::hresult**. Se você precisa de um HRESULT (para a verificação de tipo, ou para dar suporte a características de tipo), você pode `static_cast` uma **winrt::hresult**. Caso contrário, **winrt::hresult** converte em HRESULT, contanto que você inclua `unknwn.h` antes de incluir qualquer C++/WinRT cabeçalhos.
- **Alteração significativa**. GUID agora é projetada como **winrt::guid**. Para as APIs que você implementa, você deve usar **winrt::guid** para parâmetros GUID. Caso contrário, **winrt::guid** converte em GUID, contanto que você inclua `unknwn.h` antes de incluir qualquer C++/WinRT cabeçalhos.
- **Alteração significativa**. O [ **winrt::handle_type construtor** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) protegeu fazendo explícita (ele agora é mais difícil escrever um código incorreto com ele). Se você precisar atribuir um valor de identificador bruto, chame o [ **função handle_type::attach** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) em vez disso.
- **Alteração significativa**. As assinaturas dos **WINRT_CanUnloadNow** e **WINRT_GetActivationFactory** foram alterados. Você não deve declarar essas funções em todos os. Em vez disso, inclua `winrt/base.h` (que é incluído automaticamente se você incluir qualquer C++arquivos de cabeçalho de namespace do WinRT Windows) para incluir as declarações de uma dessas funções.
- Para o [ **winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** foram preteridos em favor do **from_file_time/to_file_time**.
- As APIs que esperam **IBuffer** parâmetros são simplificados. Embora a maioria das APIs prefira coleções ou matrizes, suficiente APIs dependem **IBuffer** que precisava ser mais fácil de usar essas APIs do C++. Essa atualização fornece acesso direto aos dados por trás de um **IBuffer** implementação, usando a mesma convenção de nomenclatura de dados usada pelos contêineres da biblioteca padrão C++. Isso também evita o conflito com nomes de metadados que convencionalmente começam com uma letra maiuscula.
- Melhor geração de código: várias melhorias para reduzir o tamanho do código, melhorar o inlining e otimizar o cache de fábrica.
- Removida a recursão desnecessária. Quando a linha de comando refere-se para uma pasta, em vez de um determinado `.winmd`, o `cppwinrt.exe` ferramenta não pesquisa recursivamente para `.winmd` arquivos. O `cppwinrt.exe` ferramenta agora também lida com as duplicatas mais inteligente, tornando mais resiliente a erro de usuário e a mal formado `.winmd` arquivos.
- Proteção avançada de ponteiros inteligentes. Anteriormente, os revokers evento Falha ao revogar quando movem-recebe um novo valor. Isso ajudou a descobrir um problema em que classes de ponteiro inteligente não eram manipular confiavelmente autoatribuição; como raiz a [ **winrt::com_ptr struct modelo**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** tiver sido corrigido, e os revokers de evento fixados para lidar com a semântica de movimentação corretamente para que eles revogam após a atribuição.

> [!IMPORTANT]
> Alterações importantes foram feitas para o [ C++WinRT Visual Studio VSIX (extensão)](https://aka.ms/cppwinrt/vsix), ambas na versão 1.0.181002.2 e em seguida, na versão 1.0.190128.4. Para obter detalhes sobre essas alterações e como eles afetam seus projetos existentes, [suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) e [versões anteriores da extensão do VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Isolamento de arquivos de cabeçalho do SDK do Windows

É possível que haja uma alteração significativa para seu código.

Para que ele seja compilado, C++/WinRT não depende mais arquivos de cabeçalho do SDK do Windows. Arquivos de cabeçalho na biblioteca em tempo de execução C (CRT) e o C++ STL Standard Template Library () também não incluem todos os cabeçalhos do SDK do Windows. E o que melhora a conformidade com padrões, evita dependências acidentais e reduz significativamente o número de macros que você precisa se proteger contra.

Essa independência significa que c++ /CLI WinRT agora está mais portátil e compatíveis com padrões, e ele amplia a possibilidade de ele se torne uma biblioteca de plataforma cruzada e o compilador cruzado. Isso também significa que o C++/WinRT cabeçalhos não são afetadas negativamente macros.

Se você deixou anteriormente para C++/WinRT para incluir quaisquer cabeçalhos do Windows em seu projeto, em seguida, você precisará incluí-los agora. Ele é, de qualquer forma, sempre, práticas recomendadas para incluir explicitamente os cabeçalhos que dependem de você e não deixá-lo para outra biblioteca para incluí-los para você.

Atualmente, as únicas exceções para o isolamento de arquivo de cabeçalho do SDK do Windows são intrínsecos e numéricos. Não há nenhum problema conhecido com essas últimas dependências restantes.

Em seu projeto, você pode habilitar novamente a interoperabilidade com os cabeçalhos do SDK do Windows se você precisar. Por exemplo, convém implementar uma interface COM (como raiz [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Para este exemplo, incluir `unknwn.h` antes de incluir qualquer C++/WinRT cabeçalhos. Fazendo isso faz com que o C++biblioteca de base /WinRT para habilitar vários ganchos dar suporte a interfaces do COM clássico. Para obter um exemplo de código, consulte [componentes COM do autor com C++/WinRT](author-coclasses.md). Da mesma forma, inclua explicitamente outros cabeçalhos de SDK do Windows que declarar tipos de e/ou funções que você deseja chamar.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Como redirecionar seu C++projeto /WinRT para uma versão posterior do SDK do Windows

O método para seu projeto que provavelmente resultará na edição de compilador e vinculador menor número de redirecionamento também é o mais trabalhoso. Esse método envolve criar um novo projeto (direcionamento a versão do SDK do Windows de sua escolha) e, em seguida, copiando arquivos para o seu novo projeto do seu antigo. Haverá seções do seu antigo `.vcxproj` e `.vcxproj.filters` arquivos que você pode simplesmente copiar mais de salvar a você adicionar arquivos no Visual Studio.

No entanto, há duas outras maneiras de redirecionar seu projeto no Visual Studio.

- Vá para a propriedade do projeto **gerais** \> **versão do SDK do Windows**e selecione **todas as configurações** e **todas as plataformas**. Definir **versão do SDK do Windows** para a versão que você deseja direcionar.
- Na **Gerenciador de soluções**, clique com botão direito no nó do projeto, clique em **redirecionar projetos**, escolha a versão (ões) que você deseja direcionar e, em seguida, clique em **Okey**.

Se você encontrar qualquer compilador ou erros de vinculador depois de usar qualquer um desses dois métodos, você pode testar a solução de limpeza (**construir** > **limpar solução** e/ou exclua manualmente todos os arquivos e pastas temporárias) antes de tentar compilar novamente.

Se o compilador C++ produz "*erro C2039: 'IUnknown': não é um membro de '\`namespace global '*", em seguida, adicione `#include <unknwn.h>` na parte superior do seu `pch.h` arquivo (antes de incluir qualquer C++/WinRT cabeçalhos).

Você também precisará adicionar `#include <hstring.h>` depois disso.

Se o vinculador C++ gera "*erro LNK2019: símbolo externo não resolvido _WINRT_CanUnloadNow@0 referenciado na função _VSDesignerCanUnloadNow@0* ", em seguida, você pode resolver que adicionando `#define _VSDESIGNER_DONT_LOAD_AS_DLL` para seu `pch.h` arquivo.
