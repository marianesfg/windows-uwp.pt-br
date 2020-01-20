---
description: O Windows Runtime é um sistema de contagem de referência; e, em um sistema desse tipo, é importante que você conheça o significado e a distinção entre referências fortes e fracas.
title: Referências fortes e fracas em C++/WinRT
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, forte, fraca, referência
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 781b63f9f32a0fdf7edee6479b60fd82822cc745
ms.sourcegitcommit: e1bd1d9b2598e40b5d788338a07e12f4e4c58495
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/01/2020
ms.locfileid: "75585389"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Referências fortes e fracas em C++/WinRT

O Windows Runtime é um sistema de contagem de referência; e, em um sistema desse tipo, é importante que você conheça o significado e a distinção entre referências fortes e fracas (e as referências que não se enquadram em nenhum dos dois casos, como o ponteiro *this* implícito). Como você verá neste tópico, saber como gerenciar essas referências corretamente pode significar a diferença entre um sistema confiável que é executado sem problemas e outro que falha de forma imprevisível. Ao fornecer funções auxiliares com amplo suporte na projeção de linguagem, o [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) e você alcançam um meio-termo para seu trabalho de criar sistemas mais complexos de maneira simples e correta.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acessar com segurança o ponteiro *this* em uma corrotina de membro de classe

Para saber mais sobre corrotinas e obter exemplos de código, confira [Simultaneidade e operações assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

A listagem de códigos abaixo mostra um exemplo típico de uma corrotina que é uma função de membro de uma classe. Você pode copiar e colar esse exemplo para os arquivos especificados em um novo projeto de **aplicativo de console do Windows (C++/WinRT)** .

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** passa algum tempo trabalhando e, eventualmente, retorna uma cópia do membro de dados `MyClass::m_value`. Chamar **RetrieveValueAsync** faz com que um objeto assíncrono seja criado, contendo um ponteiro *this* implícito (por meio do qual, por fim, `m_value` é acessado).

Lembre-se de que, em uma corrotina, a execução é síncrona até o primeiro ponto de suspensão, no qual o controle é retornado para o autor da chamada. Em **RetrieveValueAsync**, o primeiro `co_await` é o primeiro ponto de suspensão. Quando a corrotina for retomada (cerca de cinco segundos mais tarde, neste caso), qualquer coisa pode ter acontecido com o ponteiro implícito *this*, por meio do qual acessamos `m_value`.

Aqui está a sequência completa de eventos.

1. Em **main**, uma instância de **MyClass** é criada (`myclass_instance`).
2. O objeto `async` é criado, que aponta para (por meio de seu *this*) para `myclass_instance`.
3. A função **winrt::Windows::Foundation::IAsyncAction::get** atinge seu primeiro ponto de suspensão, bloqueia por alguns segundos e, em seguida, retorna o resultado de **RetrieveValueAsync**.
4. **RetrieveValueAsync** retorna o valor de `this->m_value`.

A etapa 4 é segura desde que *this* permaneça válido.

Mas e se a instância da classe for destruída antes que a operação assíncrona seja concluída? Há todos os tipos de maneiras em que a instância da classe poderia sair do escopo antes do método assíncrono ser concluído. No entanto, é possível simulá-la definindo a instância da classe como `nullptr`.

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

Após o ponto em que podemos destruir a instância da classe, parece que não fazemos mais referência a ela diretamente. É claro que o objeto assíncrono tem um ponteiro *this* para ele e tenta usá-lo para copiar o valor armazenado dentro da instância de classe. A corrotina é uma função de membro e ela espera ser capaz de usar o respectivo ponteiro *this* livremente.

Com essa alteração no código, nos deparamos um problema na etapa 4, porque a instância da classe foi destruída, e *this* não é mais válido. Assim que o objeto assíncrono tenta acessar a variável dentro da instância de classe, ele falha (ou faz algo totalmente indefinido).

A solução é fornecer à operação assíncrona &mdash; corrotina &mdash; sua própria referência forte para a instância da classe. Como está escrito atualmente, a corrotina contém efetivamente um ponteiro *this* bruto para a instância de classe, mas que não é suficiente para manter a instância da classe ativa.

Para manter a instância da classe de funcionamento, altere a implementação de **RetrieveValueAsync** para aquela mostrada abaixo.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Uma classe C++/WinRT deriva direta ou indiretamente do modelo [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Por causa disso, o objeto C++/WinRT pode chamar sua função de membro protegido [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) para recuperar uma referência forte a seu ponteiro *this*. Observe que não há necessidade real de usar a variável `strong_this` no código de exemplo acima; apenas chamar **get_strong** incrementa a contagem de referências do objeto C++/WinRT e mantém seu ponteiro *this* implícito válido.

> [!IMPORTANT]
> Devido a **get_strong** ser uma função de membro do modelo de struct **winrt::implements**, você pode chamá-lo apenas de uma classe que deriva direta ou indiretamente de **winrt::implements**, tal como uma classe C++/WinRT. Para obter mais informações sobre derivação de **winrt::implements** e exemplos, consulte [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Isso resolve o problema que tivemos anteriormente quando passamos para a etapa 4. Mesmo que todas as outras referências para a instância da classe desapareçam, a corrotina tomará o cuidado de garantir que suas dependências estejam estáveis.

Se uma referência forte não for apropriada, você poderá, em vez disso, chamar [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) para recuperar uma referência fraca para *this*. Apenas confirme que você pode recuperar uma referência forte antes de acessar *this*. Novamente, **get_weak** é uma função de membro do modelo de struct **winrt::implements**.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

No exemplo acima, a referência fraca não impede que a instância da classe seja destruída quando não resta nenhuma referência forte. Mas ela oferece uma maneira de verificar se uma referência forte pode ser adquirida antes de acessar a variável de membro.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acessar com segurança o ponteiro *this* com um delegado de manipulação de eventos

### <a name="the-scenario"></a>O cenário

Para obter informações gerais sobre manipulação de eventos, consulte [Manipular eventos usando delegados em C++/WinRT](handle-events.md).

A seção anterior realçou possíveis problemas de tempo de vida nas áreas de corrotinas e simultaneidade. No entanto, se você manipular um evento com uma função de membro de um objeto de dentro de uma função lambda dentro da função de membro de um objeto, precisará pensar sobre os tempos de vida relativos do destinatário do evento (o objeto manipulando o evento) e a origem do evento (o objeto acionando o evento). Vamos examinar alguns exemplos de código.

A listagem de códigos abaixo primeiro define uma classe **EventSource** simples, que dispara um evento genérico que é manipulado por quaisquer delegados que foram adicionados a ele. Esse evento de exemplo ocorre ao usar o tipo de delegado [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler), mas os problemas e soluções aqui se aplicam a todos e quaisquer tipos de delegado.

Em seguida, a classe **EventRecipient** fornece um manipulador para os eventos **EventSource::Event** na forma de uma função lambda.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

O padrão é que o destinatário do evento tenha um manipulador de eventos de lambda com dependências seu ponteiro *this*. Sempre que o destinatário do evento durar mais que a origem do evento, ela durará mais do que essas dependências. E nesses casos, que são comuns, o padrão funciona bem. Alguns desses casos são óbvios, como quando uma página de interface do usuário manipula um evento gerado por um controle que está na página. A página dura mais que o botão &mdash; dessa forma, o manipulador também dura mais que o botão. Isso permanece sendo verdadeiro sempre que o destinatário é proprietário da origem (como um membro de dados, por exemplo), ou sempre que o destinatário e a origem são irmãos e diretamente pertencentes a algum outro objeto.

Quando tiver certeza de que tem um caso no qual o manipulador não durará mais que o *this* do qual ele depende, você poderá capturar *this* normalmente, sem se preocupar com tempo de vida forte ou fraco.

Mas ainda há casos em que *this* não dura até cumprir sua função em um manipulador (incluindo os manipuladores de eventos de progresso e de conclusão gerados por ações e operações assíncronas) e é importante saber como lidar com eles.

- Quando uma origem do evento gerar seus eventos *de forma síncrona*, você poderá revogar o manipulador e ter certeza de que não receberá nenhum outro evento. Mas para eventos assíncronos, mesmo após a revogação (e especialmente durante a revogação no destruidor), um evento em andamento poderá acessar o objeto depois que ele começar a ser destruído. Encontrar um local para cancelar a assinatura antes da destruição pode atenuar o problema, mas continue lendo para obter uma solução robusta.
- Se você estiver criando uma corrotina para implementar um método assíncrono, então isso será possível.
- Em casos raros com objetos de estrutura de interface de usuário XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por exemplo), então será possível, se o destinatário for finalizado sem cancelar o registro da fonte de evento.

### <a name="the-issue"></a>O problema

Esta próxima versão da função **main** simula o que acontece quando o destinatário do evento é destruído (ou talvez sai do escopo) enquanto a origem do evento ainda está gerando eventos.

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

O destinatário do evento é destruído, mas o manipulador de eventos lambda dentro dele ainda está inscrito no evento **Evento**. Quando esse evento é gerado, o lambda tenta desreferenciar o ponteiro *this*, que nesse momento é inválido. Assim, uma violação de acesso resulta de código no manipulador (ou em uma tentativa de continuação de corrotina) tentando usá-lo.

> [!IMPORTANT]
> Se você encontrar uma dessas situações, precisará pensar sobre o tempo de vida do objeto *this* e considerar se o objeto *this* capturado sobrevive ou não à captura. Se não sobreviver, então capture-o com uma referência forte ou fraca, conforme demonstraremos abaixo.
>
> Ou &mdash; se fizer sentido para seu cenário e se as considerações de conversa o possibilitarem &mdash; então, outra opção é revogar o manipulador depois que o destinatário termina de usar o evento ou no destruidor do destinatário. Veja [Revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate).

É assim que estamos registrando o manipulador.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

O lambda captura automaticamente todas as variáveis locais por referência. Portanto, neste exemplo, poderíamos equivalentemente ter escrito isso.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Em ambos os casos, estamos apenas capturando o ponteiro *this* bruto. E isso não tem nenhum efeito na contagem de referências, então nada está impedindo que o objeto atual seja destruído.

### <a name="the-solution"></a>A solução

A solução é capturar uma referência forte (ou, como veremos adiante, uma referência fraca, caso isso seja mais apropriado). Uma referência forte *incrementa* a contagem de referências e *mantém* o objeto atual ativo. Basta declarar uma variável de captura (chamada `strong_this` neste exemplo) e inicializá-la com uma chamada para [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function), que recupera uma referência forte para nosso ponteiro *this*.

> [!IMPORTANT]
> Devido a **get_strong** ser uma função de membro do modelo de struct **winrt::implements**, você pode chamá-lo apenas de uma classe que deriva direta ou indiretamente de **winrt::implements**, tal como uma classe C++/WinRT. Para obter mais informações sobre derivação de **winrt::implements** e exemplos, consulte [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Você ainda pode omitir a captura automática do objeto atual e acessar o membro de dados por meio da variável de captura em vez de por meio do *this* implícito.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Se uma referência forte não for apropriada, você poderá, em vez disso, chamar [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) para recuperar uma referência fraca para *this*. Uma referência fraca *não* mantém o objeto atual ativo. Portanto, apenas confirme que você ainda pode recuperar uma referência forte da referência fraca antes de acessar os membros.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

Se você capturar um ponteiro bruto, precisará garantir que manterá o objeto apontado ativo.

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Se você usar uma função de membro como um delegado

Assim como as funções lambda, esses princípios também se aplicam ao uso de uma função de membro como seu delegado. A sintaxe é diferente, portanto, vamos examinar alguns códigos. Primeiro, aqui está o manipulador de eventos de função de membro potencialmente não seguros, usando um ponteiro *this* bruto.

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

Essa é a maneira convencional/padrão para se referir a um objeto e sua função de membro. Para que isso seja seguro, você pode &mdash; da versão 10.0.17763.0 (Windows 10, versão 1809) do SDK do Windows em diante &mdash; estabelecer uma referência forte ou fraca no ponto em que o manipulador é registrado. Nesse ponto, sabe-se que o objeto de destinatário do evento ainda está ativo.

Para obter uma referência forte, basta chamar [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) no lugar do ponteiro *this* bruto. O C++/WinRT garante que o delegado resultante contenha uma referência forte para o objeto atual.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Capturar uma referência forte significa que o objeto se tornará qualificado para destruição somente depois que o registro do manipulador for cancelado e todos os retornos de chamada pendentes forem retornados. No entanto, essa garantia é válida somente no momento em que o evento é gerado. Se o manipulador de eventos for assíncrono, você precisará fornecer à corrotina uma referência forte para a instância de classe antes do primeiro ponto de suspensão (para obter detalhes e código, confira a seção [Como acessar com segurança o ponteiro *this* em uma corrotina de membro de classe](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) anteriormente neste tópico). Mas isso cria uma referência circular entre a origem do evento e o objeto e, portanto, você precisa desfazer isso explicitamente revogando o evento.

Para obter uma referência fraca, chame [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function). O C++/ WinRT garante que o delegado resultante contenha uma referência fraca. No último momento e em segundo plano, o delegado tentará resolver a referência fraca para uma forte e chamará a função de membro apenas se for bem-sucedido.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

Se o representante *chamar* a função de membro, o C++/WinRT manterá o objeto ativo até que o manipulador seja retornado. No entanto, se o manipulador for assíncrono, ele será retornado nos pontos de suspensão e, portanto, você precisará fornecer à corrotina uma referência forte à instância de classe antes do primeiro ponto de suspensão. Novamente, para obter mais informações, confira a seção [Como acessar com segurança o ponteiro *this* em uma corrotina de membro de classe](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) anteriormente neste tópico.

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Um exemplo de referência fraca usando **SwapChainPanel::CompositionScaleChanged**

Neste exemplo de código, usamos o evento [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) por meio de outra ilustração de referências fracas. O código registra um manipulador de eventos usando um lambda que captura uma referência fraca para o destinatário.

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

Na cláusula de captura lambda, é criada uma variável temporária, que representa uma referência fraca a *this*. No corpo do lambda, se uma referência forte a *this* puder ser obtida, então a função **OnCompositionScaleChanged** será chamada. Dessa forma, dentro de **OnCompositionScaleChanged**, *this* pode ser usado com segurança.

## <a name="weak-references-in-cwinrt"></a>Referências fracas em C++/WinRT

Acima, nós vimos referências fracas sendo usadas. Em geral, eles são bons para quebrar referências cíclicas. Por exemplo, quando se trata da implementação nativa da estrutura da IU baseada em XAML &mdash; devido ao design histórico da estrutura &mdash; o mecanismo de referência fraca em C++/WinRT é necessário para tratar as referências cíclicas. Fora do XAML, no entanto, você provavelmente não precisará usar referências fracas (não há nada nelas inerentemente específico a XAML). Ao contrário, você deve ser capaz na maioria das vezes de criar suas próprias APIs C++/WinRT, de modo a evitar a necessidade de referências cíclicas e referências fracas. 

Em qualquer tipo declarado, não fica imediatamente óbvio para o C++/WinRT se ou quando são necessárias referências fracas. Portanto, o C++/WinRT dá suporte a referência fraca automaticamente no modelo de struct [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), do qual são derivados direta ou indiretamente seus próprios tipos C++/WinRT. Ele é pago pelo uso, o que significa que você só pagará se o objeto for consultado em busca de [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). E você pode optar explicitamente por [recusar esse suporte](#opting-out-of-weak-reference-support).

### <a name="code-examples"></a>Exemplos de código
O modelo de struct [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) é uma opção para obter uma referência fraca para uma instância de classe.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

Ou então, você pode usar a função auxiliar [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

A criação de uma referência fraca não afeta a contagem de referências no próprio objeto; ela apenas faz com que um bloco de controle seja alocado. Esse bloco de controle se encarrega de implementar a semântica da referência fraca. Em seguida, tente promover a referência fraca como uma referência forte e, se você conseguir fazer isso com êxito, use-a.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Desde que ainda exista alguma outra referência forte, a chamada de [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) incrementará a contagem de referências e retornará a referência forte ao autor da chamada.

### <a name="opting-out-of-weak-reference-support"></a>Recusando o suporte a referência fraca
O suporte a referência fraca é automático. Mas você pode optar explicitamente por recusar esse suporte passando o struct de marcador [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) como um argumento de modelo para a classe base.

Se você derivar diretamente de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Se você estiver criando uma classe de runtime.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Não importa onde o struct de marcador apareça no pacote de parâmetros variadic. Se você solicitar uma referência fraca para um tipo recusado, o compilador ajudará você com "*This is only for weak ref support*".

## <a name="important-apis"></a>APIs importantes
* [implements::get_weak function](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak function template](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref marker struct](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref struct template](/uwp/cpp-ref-for-winrt/weak-ref)
