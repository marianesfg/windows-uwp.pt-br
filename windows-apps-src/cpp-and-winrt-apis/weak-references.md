---
description: O Windows Runtime é um sistema de contagem de referência; e, em um sistema desse tipo, é importante que você conheça o significado e a distinção entre referências fortes e fracas.
title: Referências fracas em C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, padrão e c++, cpp, winrt, projeção, forte e fraca, de referência
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 507b3cee71819df1d0163380a494e6a15936109f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630811"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Referências fortes e fracas no C + + c++ /CLI WinRT

O tempo de execução do Windows é um sistema de contagem de referência; e, em um sistema desse tipo é importante que você conheça o significado de e a distinção entre, referências fortes e fracas (e as referências que são nenhum, como o implícita *isso* ponteiro). Como você verá neste tópico, a saber como gerenciar essas referências corretamente pode significar a diferença entre um sistema confiável que é executado sem problemas e outro que falha de forma imprevisível. Ao fornecer funções auxiliares que têm suporte de profundidade na projeção de linguagem, [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) atende você na metade do seu trabalho de criação de sistemas mais complexos simples e corretamente.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acessar com segurança os *isso* ponteiro em uma corrotina de membro de classe

Listagem de códigos abaixo mostra um exemplo típico de uma co-rotina que é uma função de membro de uma classe.

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

**MyClass::RetrieveValueAsync** trabalhar por algum tempo, e, em seguida, eventualmente, ele retorna uma cópia do `MyClass::m_value` membro de dados. Chamando **RetrieveValueAsync** faz com que um objeto assíncrono deve ser criada, e esse objeto tem implícito *isso* ponteiro (por meio do qual, por fim, `m_value` é acessado).

Aqui está a sequência completa de eventos.

1. Na **principal**, uma instância do **MyClass** é criado (`myclass_instance`).
2. O `async` objeto é criado, que aponta para a (por meio de seu *isso*) para `myclass_instance`.
3. O **winrt::Windows::Foundation::IAsyncAction::get** função bloqueia por alguns segundos e, em seguida, retorna o resultado da **RetrieveValueAsync**.
4. **RetrieveValueAsync** retorna o valor de `this->m_value`.

Etapa 4 é segura, desde *isso* é válido.

Mas e se a instância da classe é destruída antes que a operação assíncrona seja concluída? Há todos os tipos de maneiras que a instância da classe poderia saem do escopo antes do método assíncrono é concluído. Mas, é possível simulá-lo definindo a instância da classe como `nullptr`.

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

Após o ponto onde podemos destruir a instância da classe, parece que estamos não diretamente consultá-lo novamente. Mas certamente objeto assíncrono tem um *isso* ponteiro para ele e tenta usá-la para copiar o valor armazenado dentro da instância de classe. A co-rotina é uma função de membro, e ele espera que seja capaz de usar suas *isso* ponteiro com impunity.

Com essa alteração no código, nos deparamos um problema na etapa 4, porque a instância da classe foi destruída, e *isso* não é mais válido. Assim que o objeto assíncrono tenta acessar a variável dentro da instância de classe, ela será falhar (ou fazer algo totalmente indefinido).

A solução é fornecer a operação assíncrona&mdash;co-rotina&mdash;sua própria referência forte para a instância da classe. Como está escrito atualmente, a co-rotina contém efetivamente um brutos *isso* ponteiro para a instância de classe, mas que não é suficiente para manter a instância da classe ativa.

Para manter a instância da classe de funcionamento, alterar a implementação de **RetrieveValueAsync** à mostrada abaixo.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Porque C + c++ /CLI WinRT objeto direta ou indiretamente deriva a [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) modelo, o C + + c++ /CLI objeto WinRT pode chamar seu [ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) protegidos a função de membro para recuperar uma referência forte para seus *isso* ponteiro. Observe que não há necessidade de usar, na verdade, o `strong_this` variável; apenas chamando **get_strong** incrementa a contagem de referência e mantém seu implícita *isso* ponteiro válido.

Isso resolve o problema que usamos anteriormente quando temos para a etapa 4. Mesmo se todas as outras referências para a instância da classe desaparecerem, a co-rotina apresentou cuidado para garantir que suas dependências sejam estáveis.

Se uma referência forte não é apropriada e, em seguida, em vez disso, você pode chamar [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) para recuperar uma referência fraca para *isso*. Confirmar que você pode recuperar uma referência forte antes de acessar *isso*.

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

No exemplo acima, a referência fraca não manter a instância da classe sejam destruídos quando nenhuma referência forte permanecem. Mas ele oferece uma maneira de verificar se uma referência forte pode ser adquirida antes de acessar a variável de membro.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acessar com segurança os *isso* ponteiro com um representante de manipulação de eventos

### <a name="the-scenario"></a>O cenário

Para obter informações gerais sobre a manipulação de eventos, consulte [manipular eventos usando delegados em C + + c++ /CLI WinRT](handle-events.md).

A seção anterior realçou possíveis problemas de tempo de vida nas áreas de co-rotinas e simultaneidade. Mas, se você manipular um evento com a função de membro de um objeto ou de uma função lambda dentro da função de membro de um objeto, em seguida, você precisa pensar sobre os tempos de vida relativos do destinatário de evento (o objeto manipulando o evento) e a origem do evento (o objeto Gerando o evento). Vamos examinar alguns exemplos de código.

Listagem de códigos abaixo primeiro define uma simples **EventSource** classe, que dispara um evento genérico que é tratado por quaisquer delegados que foram adicionados a ele. Esse evento de exemplo ocorre ao usar o [ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler) tipo delegado, mas os problemas e soluções aqui se aplicam a tipos de delegado toda e qualquer.

Em seguida, o **EventRecipient** classe fornece um manipulador para o **EventSource::Event** eventos na forma de uma função lambda.

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

O padrão é que o destinatário de evento tem um manipulador de eventos de lambda com dependências seu *isso* ponteiro. Sempre que o destinatário de evento é maior que a origem do evento, ela dura mais do que essas dependências. E, nesses casos, o que são comuns, o padrão funciona bem. Alguns desses casos são óbvios, como quando uma página de interface do usuário manipula um evento gerado por um controle que está na página. A página é maior que o botão&mdash;dessa forma, o manipulador também é maior que o botão. Isso mantém verdadeiro qualquer momento que o destinatário possui a origem (como um membro de dados, por exemplo), ou sempre que o destinatário e a origem são irmãos e diretamente pertencentes a algum outro objeto. Se você tiver certeza que tem um caso onde o manipulador não sobrepõem a *isso* no qual ele depende, então você pode capturar *isso* normalmente, sem consideração para tempo de vida forte ou fraco.

Mas ainda há casos em que *isso* não sobreviver além de seu uso em um manipulador (incluindo manipuladores de eventos de conclusão e o progresso gerados pelas ações e operações assíncronas), e é importante saber como lidar com eles.

- Se você estiver criando um rotina concomitante para implementar um método assíncrono, então é possível.
- Em casos raros com objetos de estrutura de interface de usuário do XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por exemplo), então é possível, se o destinatário for finalizado sem cancelar o registro da fonte de evento.

### <a name="the-issue"></a>O problema

Nesta próxima versão dos **principal** função simula o que acontece quando o destinatário do evento é destruído (talvez ele sai do escopo) enquanto a origem do evento ainda está gerando eventos.

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

O destinatário do evento é destruído, mas o manipulador de eventos de lambda dentro dele ainda está inscrito para o **evento** evento. Quando esse evento é gerado, o lambda tenta cancelar a referência a *isso* ponteiro, que nesse momento é inválido. Dessa forma, os resultados de uma violação de acesso do código no manipulador (ou em continuação de uma co-rotina) tentar usá-lo.

> [!IMPORTANT]
> Se você encontrar uma situação como essa, você precisará pensar sobre a vida útil do *isso* do objeto; e ou não capturado *isso* objeto é maior que a captura. Caso contrário, em seguida, capturá-la com uma forte ou uma referência fraca, como demonstraremos abaixo.
>
> Ou&mdash;se faz sentido para seu cenário e threading considerações possibilitam até mesmo&mdash;e em seguida, outra opção é revogar o manipulador depois que o destinatário é feito com o evento, ou no destruidor do destinatário. Ver [revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate).

Isso é como podemos estiver registrando o manipulador.

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

Em ambos os casos, podemos apenas está capturando o raw *isso* ponteiro. E que não tem nenhum efeito na contagem de referência, para que nada está impedindo que o objeto atual que está sendo destruído.

### <a name="the-solution"></a>A solução

A solução é capturar uma referência forte. Uma referência forte *faz* incrementar a contagem de referência e ele *faz* manter ativa o objeto atual. Basta declarar uma variável de captura (chamado `strong_this` neste exemplo) e inicializá-lo com uma chamada para [ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function), que recupera uma referência forte para nosso  *Isso* ponteiro.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Você ainda pode omitir a captura automática do objeto atual e acessar o membro de dados por meio da variável de captura em vez de por meio de implícito *isso*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Se uma referência forte não é apropriada e, em seguida, em vez disso, você pode chamar [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) para recuperar uma referência fraca para *isso*. Confirme que você ainda pode recuperar uma referência forte dele antes de acessar membros.

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

Como as funções lambda, esses princípios também se aplicam ao uso de uma função de membro como seu delegado. A sintaxe é diferente, portanto, vamos examinar alguns códigos. Primeiro, aqui está o manipulador de eventos de função de membro potencialmente não seguros, usando um brutos *isso* ponteiro.

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

Essa é a maneira convencional, padrão para se referir a um objeto e sua função de membro. Para que isso seja seguro, você pode&mdash;a partir da versão 10.0.17763.0 (Windows 10, versão 1809) do SDK do Windows&mdash;estabelecer um forte ou uma referência fraca no ponto em que o manipulador é registrado. Nesse ponto, o objeto de destinatário do evento é conhecido por ser ainda ativa.

Para obter uma referência forte, basta chamar [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) no lugar bruto *isso* ponteiro. C + + c++ /CLI WinRT garante que o delegado resultante contém uma referência forte para o objeto atual.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Para obter uma referência fraca, chame [ **get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function). C + + c++ /CLI WinRT garante que o delegado resultante contém uma referência fraca. No último minuto e segundo plano, o delegado tenta resolver a referência fraca para um forte e chama a função de membro apenas se for bem-sucedido.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Um exemplo de referência fraca usando **SwapChainPanel::CompositionScaleChanged**

Este exemplo de código, usamos o [ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) eventos por meio de outra ilustração de referências fracas. O código registra um manipulador de eventos usando um lambda que captura uma referência fraca para o destinatário.

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

Acima, nós vimos referências fracas que está sendo usadas. Em geral, eles são bons para quebrar as referências cíclicas. Por exemplo, para a implementação nativa da estrutura de interface do usuário baseada em XAML&mdash;por causa do design histórico do framework&mdash;a fraca mecanismo no C + + de referência c++ /CLI WinRT é necessária lidar com referências cíclicas. Fora do XAML, no entanto, você provavelmente não precisará usar referências fracas (não há nada inerentemente XAML-específicas sobre eles). Em vez disso, você deve, mais frequência, ser capaz de criar seu próprio C + + c++ /CLI APIs do WinRT na forma como para evitar a necessidade de referências cíclicas e referências fracas. 

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

## <a name="important-apis"></a>APIs Importantes
* [função Implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [modelo de função WinRT::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [WinRT::no_weak_ref marcador struct](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [WinRT::weak_ref struct modelo](/uwp/cpp-ref-for-winrt/weak-ref)
