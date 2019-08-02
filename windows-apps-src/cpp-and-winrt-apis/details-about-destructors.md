---
description: O C++O/WinRT 2.0 permite adiar a destruição de seus tipos de implementação e consultar com segurança durante a destruição. Este tópico descreve esses recursos e explica quando usá-los.
title: Detalhes sobre destruidores
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projeção, destruição adiada, consultas seguras
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387793"
---
# <a name="details-about-destructors"></a>Detalhes sobre destruidores

O C++O/WinRT 2.0 permite adiar a destruição de seus tipos de implementação e consultar com segurança durante a destruição. Este tópico descreve esses recursos e explica quando usá-los.

## <a name="deferred-destruction"></a>Destruição adiada

No tópico [Diagnosticar alocações diretas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc), mencionamos que seu tipo de implementação não pode ter um destruidor privado.

O benefício de ter um destruidor público é que ele permite a destruição adiada, que é a capacidade de detectar a chamada final [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) em seu objeto e, em seguida, apropriar-se desse objeto para adiar sua destruição indefinidamente.

Lembre-se de que objetos COM clássicos têm referência intrinsecamente contada; a contagem de referência é gerenciada por meio das funções [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) e **IUnknown::Release**. Em uma implementação tradicional de **Release**, um destruidor C++ do objeto COM clássico é invocado após a contagem de referências atingir 0.

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

O `delete this;` chama o destruidor do objeto antes de liberar a memória ocupada por ele. Isso funciona bem o suficiente, contanto que você não precise fazer nada interessante em seu destruidor.

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

O que queremos dizer com *interessante*? Para começar, um destruidor é inerentemente síncrono. Não é possível alternar threads&mdash;talvez para destruir alguns recursos específicos do thread em um contexto diferente. Não é possível consultar o objeto de forma confiável para alguma outra interface que você talvez precise para liberar determinados recursos. A lista continua. Para os casos em que sua destruição não é trivial, é necessária uma solução mais flexível. Que é onde a função **final_release** do C++/WinRT entra.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

Atualizamos a implementação do C++/WinRT de **Release** para chamar **final_release** logo quando a contagem de referências do seu objeto faz a transição para 0. Nesse estado, o objeto pode ter certeza de que não há mais referências pendentes e agora tem a propriedade exclusiva de si mesmo. Por esse motivo, ele pode transferir a propriedade de si mesmo para a função **final_release**.

Em outras palavras, o objeto transformou-se em um que dá suporte à propriedade compartilhada em um que tem propriedade exclusiva. O **std::unique_ptr** tem a propriedade exclusiva do objeto e naturalmente destruirá o objeto como parte de sua semântica&mdash;, daí a necessidade de um destruidor público&mdash;quando o **std::unique_ptr** sai do escopo (desde que não seja movido para outro lugar antes disso). E esse é o segredo. É possível usar o objeto indefinidamente, desde que **std::unique_ptr** mantenha o objeto ativo. Veja uma ilustração de como você pode mover o objeto para outro lugar.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

Considere isso como um coletor de lixo determinístico. Talvez de maneira mais prática e avançada, você pode transformar a função **final_release** em uma corrotina e manipular sua eventual destruição em um lugar, além de suspender e alternar threads, conforme necessário.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

Uma suspensão apontará as causas de o thread de chamada&mdash;que originalmente iniciava a chamada para a função **IUnknown::Release**&mdash;retornar e, portanto, indicará ao chamador que o objeto que ele já ocupou não está mais disponível por meio desse ponteiro de interface. As estruturas de interface do usuário geralmente precisam verificar se os objetos são destruídos no thread de interface de usuário específico que originalmente criou o objeto. Esse recurso facilita o atendimento de um requisito, porque a destruição é separada da liberação do objeto.

## <a name="safe-queries-during-destruction"></a>Consultas seguras durante a destruição

A criação da noção de destruição adiada é a capacidade de consultar interfaces com segurança durante a destruição.

O COM clássico baseia-se em dois conceitos centrais. O primeiro é a contagem de referências e o segundo é a consulta de interfaces. Além de **AddRef** e **Release**, a interface **IUnknown** fornece [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void). Esse método é muito usado por determinadas estruturas de interface do usuário&mdash;como XAML, para atravessar a hierarquia XAML enquanto simula seu sistema de tipos combináveis. Considere um exemplo simples.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

Isso pode *parecer* inofensivo. Esta página XAML deseja limpar seu contexto de dados em seu destruidor. Mas [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) é uma propriedade da classe base **FrameworkElement** e reside na interface **IFrameworkElement** distinta. Como resultado, o C++/WinRT deve injetar uma chamada em **QueryInterface** para pesquisar o vtable correto antes de poder chamar a propriedade **DataContext**. Mas o motivo pelo qual estamos até no destruidor é que a contagem de referência fez a transição para 0. Chamar **QueryInterface** aqui impacta temporariamente a contagem de referência; e quando ela é retornada novamente para 0, o objeto é destruído novamente.

O C++/WinRT 2.0 foi protegido para dar suporte a isso. Veja a implementação do C++/WinRT 2.0 de Release, em uma forma simplificada.

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

Como você deve ter previsto, ela primeiro decrementa a contagem de referência e, em seguida, agirá somente se não houver referências pendentes. No entanto, antes de chamar a função **final_release** estática que descrevemos anteriormente neste tópico, ela estabiliza a contagem de referência definindo-a como 1. Denominamos isso *ressalto* (utilizando um termo emprestado da engenharia elétrica). Isso é fundamental para impedir a liberação da referência final. Quando isso acontece, a contagem de referência é instável e não pode dar suporte com confiança a uma chamada a **QueryInterface**.

Chamar **QueryInterface** será perigoso depois que a referência final tiver sido liberada, pois a contagem de referência poderá crescer indefinidamente. É sua responsabilidade chamar apenas caminhos de código conhecidos que não prolongam a vida útil do objeto. O C++/WinRT vai ao seu encontro verificando se essas chamadas **QueryInterface** podem ser feitas de forma confiável.

Ele faz isso estabilizando a contagem de referência. Quando a referência final tiver sido liberada, a contagem de referência real será 0 ou algum valor muito imprevisível. O último caso poderá ocorrer se referências fracas estiverem envolvidas. De qualquer forma, isso será insustentável se uma chamada subsequente a **QueryInterface** ocorrer; porque isso fará necessariamente com que a contagem de referência seja incrementada temporariamente&mdash;e, consequentemente, o salto da referência seja desfeito. Defini-la como 1 garante que uma chamada final a **Release** nunca mais ocorrerá neste objeto. Isso é justamente o que queremos, já que **std::unique_ptr** agora tem a propriedade do objeto, mas as chamadas associadas a pares **QueryInterface**/**Release** estarão seguras.

Considere um exemplo mais interessante.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

Primeiro, a função **final_release** é chamada, notificando a implementação de que é hora de limpar. Aqui, **final_release** é uma corrotina. Para simular um primeiro ponto de suspensão, ele começa aguardando o pool de threads por alguns segundos. Em seguida, ele é retomado no thread de dispatcher da página. A última etapa envolve uma consulta, já que [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) é uma propriedade da classe base **DependencyObject**. Por fim, a página é realmente excluída em virtude da atribuição de `nullptr` a **std::unique_ptr**. Isso, por sua vez, chama o destruidor da página.

Dentro do destruidor, limpamos o contexto de dados, que, como sabemos, requer uma consulta para a classe base **FrameworkElement**.

Tudo isso é possível devido ao ressalto da contagem de referência (ou estabilização da contagem de referência) fornecido pelo C++/WinRT 2.0.