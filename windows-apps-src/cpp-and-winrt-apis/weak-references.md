---
author: stevewhims
description: O tempo de execução do Windows é um sistema de contagem de referência. em um sistema desse tipo, é importante saber sobre o significado do e distinção entre, referências fracas e fortes.
title: Referências fracas em C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, padrão, c++, cpp, winrt, projeção, fraca, referência forte
ms.localizationpriority: medium
ms.openlocfilehash: c37319ce7f7d29acfc2c1822e76fadc29b5ec863
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6451534"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Referências fracas e fortes em C++ c++ WinRT

O tempo de execução do Windows é um sistema de contagem de referência. e esse sistema é importante saber sobre o significado do e distinção entre, forte e fracas referências (e referências que são nenhum, como o implícito *esse* ponteiro). Como você verá neste tópico, saber como gerenciar essas referências corretamente pode significar a diferença entre um sistema confiável que é executado sem problemas e aquele que falhas imprevisível. Fornecendo funções auxiliares que têm suporte profunda na projeção de linguagem [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) atende você metade em seu trabalho de criação de sistemas mais complexos simplesmente e corretamente.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acessar com segurança o *esse* ponteiro em uma corrotina de membro de classe

Listagem de código abaixo mostra um exemplo típico de uma corrotina que é uma função de membro de uma classe.

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

**MyClass::RetrieveValueAsync** funciona por um tempo e, em seguida, eventualmente retorna uma cópia do `MyClass::m_value` membro de dados. Chamar **RetrieveValueAsync** faz com que um objeto assíncrono a serem criados, e esse objeto tem um implícito *esse* ponteiro (por meio da qual, por fim, `m_value` é acessada).

Aqui está a sequência de eventos completa.

1. No **principal**, é criada uma instância de **MyClass** (`myclass_instance`).
2. O `async` objeto é criado, apontando (por meio de seu *esse*) para `myclass_instance`.
3. A função **winrt::Windows::Foundation::IAsyncAction::get** bloqueia por alguns segundos e, em seguida, retorna o resultado da **RetrieveValueAsync**.
4. **RetrieveValueAsync** retorna o valor de `this->m_value`.

Etapa 4 é segura contanto que *Este* seja válido.

Mas, e se a instância de classe é destruída antes que a operação assíncrona for concluída? Há todos os tipos de maneiras que a instância de classe poderia saiam do escopo antes do método assíncrono é concluída. Porém, pode simulá-lo definindo a instância de classe `nullptr`.

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

Depois do ponto onde podemos destruir a instância de classe, parece que não diretamente nos referimos a ele novamente. Mas é claro do objeto assíncrono tem um *esse* ponteiro para ele e tenta a usá-lo para copiar o valor armazenado a instância de classe. A rotina concomitante é uma função de membro, e que possam usar seu *esse* ponteiro com impunity.

Com essa alteração no código, executamos um problema na etapa 4, porque a instância de classe é destruída, e *Este* não é mais válida. Assim que o objeto assíncrono tenta acessar a variável dentro de instância da classe, ele será falhar (ou fazer algo inteiramente indefinidas).

A solução é dar a operação assíncrona&mdash;a rotina concomitante&mdash;sua própria referência forte para a instância de classe. Como escrita atualmente, a rotina concomitante efetivamente contém um bruto *esse* ponteiro para a instância de classe; mas isso não é suficiente para manter a instância de classe ativa.

Para manter a instância de classe viva, altere a implementação de **RetrieveValueAsync** ao mostrado abaixo.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Como C++ c++ objeto WinRT direta ou indiretamente derivada do modelo [**WinRT:: Implements**](/uwp/cpp-ref-for-winrt/implements) , C++ c++ objeto WinRT pode chamar sua função de membro [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) protegido para recuperar uma referência forte a sua *esse* ponteiro. Observe que não é necessário para usar o `strong_this` variável; Basta chamar **get_strong** incrementará a contagem de referências e mantém o implícito *esse* ponteiro válido.

Isso elimina o problema que usamos anteriormente quando que obtivemos para a etapa 4. Mesmo se todas as outras referências para a instância de classe desaparecerem, a rotina concomitante realizou a precaução de garantir que suas dependências são estáveis.

Se uma referência forte não for apropriada, em seguida, você pode chamar em vez disso [**Implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) para recuperar uma referência fraca para *Esta*. Assim, confirme que você pode recuperar uma referência forte antes de acessar *essa*.

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

No exemplo acima, a referência fraca não manter o instância da classe sejam destruídos quando nenhuma referência forte permanecem. Mas ele oferece uma maneira de verificar se uma referência forte pode ser adquirida antes de acessar a variável de membro.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acessar com segurança o *esse* ponteiro com um representante do manipulador de eventos

### <a name="the-scenario"></a>O cenário

Para obter informações gerais sobre a manipulação de eventos, consulte [processar eventos usando delegados em C++ c++ WinRT](handle-events.md).

A seção anterior realçado potenciais problemas de tempo de vida nas áreas de rotinas concomitantes e simultaneidade. Mas, se você manipular um evento com função de membro de um objeto ou de uma função lambda dentro da função de membro de um objeto, então você precisa pensar sobre os tempos de vida relativos do destinatário do evento (o objeto manipulando o evento) e a origem do evento (o objeto acionamento do evento). Vamos examinar alguns exemplos de código.

A listagem de código abaixo primeiro define uma classe **EventSource** simple, que dispara um evento genérico que é manipulado por qualquer delegados que foram adicionados a ela. Este evento de exemplo ocorre a usar o tipo de representante [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) , mas os problemas e soluções aqui se aplicam a tipos de delegado todo e qualquer.

Em seguida, a classe **EventRecipient** fornece um manipulador para o evento **EventSource::Event** na forma de uma função lambda.

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

O padrão é que o destinatário do evento tem um manipulador de eventos de lambda com dependências em seu *esse* ponteiro. Sempre que o destinatário do evento sobrevive a origem do evento, ele sobrevive essas dependências. E, nesses casos, que são comuns, o padrão funciona bem. Alguns desses casos são óbvios, como quando uma página de interface do usuário manipula um evento gerado por um controle que está na página. A página sobrevive no botão&mdash;assim, o manipulador também sobrevive no botão. Isso mantém verdadeiro qualquer momento que o destinatário possui a origem (como um membro de dados, por exemplo), ou sempre que o destinatário e a origem são irmãos e diretamente pertencentes a algum outro objeto. Se você tiver certeza que tem um caso onde o manipulador não sobrepõem a *isso* no qual ele depende, então você pode capturar *isso* normalmente, sem consideração para tempo de vida forte ou fraco.

Mas ainda há casos em que *Este* não sobrepõem seu uso em um manipulador (incluindo os manipuladores de eventos de progresso gerados pelas ações assíncronas e operações e a conclusão), e é importante saber como lidar com eles.

- Se você estiver criando um rotina concomitante para implementar um método assíncrono, então é possível.
- Em casos raros com objetos de estrutura de interface de usuário do XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por exemplo), então é possível, se o destinatário for finalizado sem cancelar o registro da fonte de evento.

### <a name="the-issue"></a>O problema

Esta versão próxima da função **principal** simula o que acontece quando o destinatário do evento é destruído (talvez ele sai do escopo) enquanto a origem do evento ainda é acionamento de eventos.

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

O destinatário do evento é destruído, mas o manipulador de eventos de lambda dentro dele ainda está inscrito no evento de **evento** . Quando esse evento é gerado, o lambda tenta referência o *esse* ponteiro, nesse ponto é inválido. Portanto, resultados de uma violação de acesso do código no manipulador (ou na continuação de rotina concomitante) tentar usá-lo.

> [!IMPORTANT]
> Se você encontrar uma situação assim, em seguida, você precisará pensar sobre o tempo de vida *desse* objeto; e se ou não o capturada *esse* objeto sobrevive a captura. Se isso não acontecer, então capture-o com uma referência forte ou fraca, como demonstraremos abaixo.
>
> Ou&mdash;se fizer sentido para seu cenário e se as considerações possibilitarem&mdash;, em seguida, outra opção é revogar o manipulador depois que o destinatário é concluído com o evento ou no destruidor do destinatário. Consulte [revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate).

Isso é como podemos está registrando o manipulador.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

O lambda captura automaticamente todas as variáveis locais por referência. Portanto, neste exemplo, estamos poderia maneira equivalente ter escrito.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Em ambos os casos, podemos apenas estiver capturando o bruto *esse* ponteiro. E que não tem efeito sobre contagem de referência, para que nada está impedindo que o objeto atual que está sendo destruído.

### <a name="the-solution"></a>A solução

A solução é capturar uma referência forte. Uma referência forte *faz* incrementar a contagem de referência e ela *faz* keep alive objeto atual. Declarar uma variável de captura apenas (chamado `strong_this` neste exemplo) e inicializá-lo com uma chamada para [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function), que recupera uma referência forte a nossa *esse* ponteiro.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Você pode até mesmo omitir a captura automática do objeto atual e acessar o membro de dados através da variável de captura, em vez de via o implícito *Este*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Se uma referência forte não for apropriada, em seguida, você pode chamar em vez disso [**Implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) para recuperar uma referência fraca para *Esta*. Assim, confirme que você ainda pode recuperar uma referência forte dele antes de acessar membros.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Se você usar uma função de membro como um delegado

Assim como funções lambda, esses princípios também se aplicam ao usar uma função de membro como seu delegado. A sintaxe é diferente, portanto, vamos examinar alguns códigos. Primeiro, aqui está o manipulador de eventos de função de membro possivelmente inseguro, usando um ponteiro bruto *Este* .

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

Essa é a maneira padrão, convencional para fazer referência a um objeto e sua função de membro. Para torná-lo seguro, você pode&mdash;a partir da versão 10.0.17763.0 (Windows 10, versão 1809) do SDK do Windows&mdash;estabelecer uma referência forte ou fraca no ponto onde o manipulador é registrado. Nesse ponto, o objeto destinatário do evento é conhecido por ser ainda ativo.

Para obter uma referência forte, chamam [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) no lugar o bruto *esse* ponteiro. C++ c++ WinRT garante que o delegado resultante mantém uma referência forte para o objeto atual.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Para obter uma referência fraca, chame [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function). C++ c++ WinRT garante que o delegado resultante mantém uma referência fraca. No último minuto, nos bastidores, o representante tenta resolver a referência fraca para um forte e só chama a função de membro se for bem-sucedida.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Um exemplo de referência fraca usando **SwapChainPanel::CompositionScaleChanged**

Este exemplo de código, usamos o evento [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) por meio de outro ilustração de referências fracas. O código registra um manipulador de eventos usando um lambda que captura uma referência fraca para o destinatário.

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

Na cláusula de captura lambda, é criada uma variável temporária, que representa uma referência fraca a *isso*. No corpo do lambda, se uma referência forte a *isso* puder ser obtida, então a função **OnCompositionScaleChanged** é chamada. Dessa forma, dentro de **OnCompositionScaleChanged**, *isso* pode ser usado com segurança.

## <a name="weak-references-in-cwinrt"></a>Referências fracas em C++/WinRT

Acima, vimos referências fracas que está sendo usadas. Em geral, elas são ideais para quebrar referências cíclicas. Por exemplo, para a implementação nativa da estrutura de interface do usuário baseada em XAML&mdash;devido ao design da estrutura&mdash;mecanismo em C++ de referência fraca c++ WinRT é necessário para tratar referências cíclicas. Fora do XAML, no entanto, você provavelmente não precisará usar referências fracas (não há nada inerentemente XAML específicas sobre eles). Em vez disso, muito frequentemente, será capaz de criar seu próprio C + c++ APIs do WinRT de forma evitar a necessidade de referências cíclicas e referências fracas. 

Em qualquer tipo declarado, não fica imediatamente óbvio para o C++/WinRT se ou quando são necessárias referências fracas. Portanto, o C++/WinRT oferece suporte a referência fraca automaticamente no modelo de struct [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), do qual são derivados direta ou indiretamente seus próprios tipos C++/WinRT. Ele é pago pelo uso, o que significa que você só pagará se o objeto for consultado em busca de [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). Você pode optar explicitamente por [recusar esse suporte](#opting-out-of-weak-reference-support).

### <a name="code-examples"></a>Exemplos de código
O modelo de struct [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) é uma opção para obter uma referência fraca em uma instância de classe.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

Ou você pode usar a função auxiliar [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

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

Desde que ainda existe alguma outra referência forte, a chamada de [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) incrementará a contagem de referências e retornará a referência forte ao chamador.

### <a name="opting-out-of-weak-reference-support"></a>Recusando o suporte a referência fraca
Os suporte a referência fraca é automático. Mas você pode optar explicitamente por recusar esse suporte passando o struct de marcador [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) como um argumento de modelo para a classe base.

Se você derivar diretamente de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Se você estiver criando uma classe de tempo de execução.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Não importa onde o struct de marcador apareça no pacote de parâmetros variadic. Se você solicitar uma referência fraca para um tipo recusado, o compilador ajudará você com "*This is only for weak ref support*".

## <a name="important-apis"></a>APIs importantes
* [Função implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Modelo de função winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Struct de marcador winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Modelo de struct winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
