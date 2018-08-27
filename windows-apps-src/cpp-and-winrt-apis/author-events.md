---
author: stevewhims
description: Este tópico demonstra como criar um componente do Tempo de Execução do Windows contendo uma classe de tempo de execução que gera eventos. Ele também demonstra um aplicativo que consome o componente e maneja os eventos.
title: Criar eventos com C++/WinRT
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, criar, evento
ms.localizationpriority: medium
ms.openlocfilehash: 3b52bf8e33bbf111dd02c695d8c3baf77e1338ac
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862602"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Criar eventos com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Este tópico demonstra como criar um componente do Tempo de Execução do Windows que contém uma classe de tempo de execução representando uma conta bancária, que gera um evento quando seu saldo entra em débito. Ele também demonstra um aplicativo principal que consome a classe de tempo de execução de conta bancária, chama uma função para ajustar o saldo e manipula todos os eventos resultantes.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Criar um componente do Tempo de Execução do Windows (BankAccountWRC)

Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Componente do Tempo de Execução do Windows com Visual C++ (C++/WinRT)** e nomeie *BankAccountWRC* (para "conta bancária do componente do Tempo de Execução do Windows").

O projeto recém-criado contém um arquivo chamado `Class.idl`. Renomeie esse arquivo `BankAccount.idl` (renomear o `.idl` arquivo automaticamente renomeia o dependente `.h` e `.cpp` arquivos muito). Substituir o conteúdo de `BankAccount.idl` com a lista abaixo.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

Salve o arquivo. O projeto não criar com êxito no momento, mas criando agora é uma coisa útil fazer, porque ele gera arquivos código-fonte na qual você implementará classe **BankAccount** runtime. Portanto, vá em frente e crie agora (os erros de compilação que você pode esperar para ver nesse estágio tem a ver com `Class.h` e `Class.g.h` não foi encontrado). Durante o processo de compilação, o `midl.exe` ferramenta for executada para criar o arquivo de metadados de tempo de execução do Windows do seu componente (que é `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Em seguida, a ferramenta `cppwinrt.exe` é executada (com a opção `-component`) para gerar arquivos de código fonte para dar suporte a você na criação do componente. Esses arquivos incluem stubs para você começar a implementação da classe **BankAccount** runtime declarado na sua IDL. Esses stubs são `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` e `BankAccount.cpp`.

No Gerenciador de arquivos, copie os arquivos de stub `BankAccount.h` e `BankAccount.cpp` da pasta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` para a pasta que contém seus arquivos de projeto, que é `\BankAccountWRC\BankAccountWRC\`e substituir os arquivos no destino. Agora, vamos abrir `BankAccount.h` e `BankAccount.cpp` e implementar nossa classe de tempo de execução. Em `BankAccount.h`, adicione dois membros privados à implementação (*não* a implementação de fábrica) de BankAccount.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

Como você pode ver acima, o evento é implementado em termos de modelo [**winrt::event**](/uwp/cpp-ref-for-winrt/event) struct, parametrizado por um tipo específico de delegado.

Em `BankAccount.cpp`, implemente as funções, conforme mostrado no exemplo de código abaixo. Em C++/WinRT, um evento declarado por IDL é implementado como um conjunto de funções sobrecarregadas (semelhante à forma como um propriedade é implementada como um par de funções get e set sobrecarregadas). Uma sobrecarga resulta no registro de um delegado e retorna um token. O outro recebe um token e revoga o registro do delegado associado.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

Se não precisar implementar a sobrecarga do revogador do evento (para obter mais detalhes, consulte [Revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate))&mdash;isso é resolvido para você pela projeção de C++/WinRT. As outras sobrecargas não são incorporadas na projeção a fim de oferecer a flexibilidade para implementá-las da forma ideal no seu cenário. Chamar [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) e [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) dessa forma é um padrão eficiente e simultâneo/seguro para o thread. Mas se houver um grande número de eventos, você talvez não queira um campo de evento para cada uma, mas em vez disso, opte por algum tipo de implementação esparsa.

Você também pode ver acima que essa implementação da função **AdjustBalance** gera o evento **AccountIsInDebit** se o saldo ficar negativo.

Se todos os avisos impedirem-lo de construção, resolvê-los ou definir a propriedade project **C/C++** > **Geral** > **Tratar avisos como erros** **não (/ WX-)** e compile o projeto novamente.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Criar um aplicativo principal (BankAccountCoreApp) para testar o componente do Tempo de Execução do Windows

Agora crie um novo projeto (em sua solução `BankAccountWRC` ou em uma nova). Crie um projeto do **Aplicativo principal do Visual C++ (C++/WinRT)** e nomeie-o *BankAccountCoreApp*.

Adicionar uma referência e navegue até `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou adicionar uma referência de projeto no project, se os dois projetos na mesma solução). Clique em **Adicionar** e, depois, em **OK**. Agora crie BankAccountCoreApp. Na hipótese que você veja um erro que o arquivo de carga `readme.txt` não existir, excluir esse arquivo de projeto do componente de tempo de execução do Windows, recompilá-lo e reconstruir BankAccountCoreApp.

Durante o processo de compilação, a ferramenta `cppwinrt.exe` é executada para processar o arquivo `.winmd` referenciado nos arquivos de código-fonte que contêm tipos projetados para ajudar você a consumir o componente. O cabeçalho dos tipos projetados de suas classes de tempo de execução do componente&mdash;nomeado `BankAccountWRC.h`&mdash;é gerado na pasta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Inclua esse cabeçalho em `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Também em `App.cpp`, adicione o seguinte código para instanciar uma BankAccount (usando o construtor padrão do tipo projetado), registre um manipulador de eventos e, em seguida, faça com que a conta entre em débito.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Cada vez que você clicar na janela, subtraia 1 do saldo da conta bancária. Para demonstrar que o evento está sendo gerado conforme o esperado, coloque um ponto de interrupção dentro a expressão lambda que está manipulando o evento **AccountIsInDebit** , execute o aplicativo e clique dentro da janela.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Os representantes parametrizados e sinais simples, entre uma ABI

Se seu evento deve estar acessível através de uma interface de binários de aplicativo (ABI)&mdash;como entre um componente e seu aplicativo consumo&mdash;e em seguida, o evento deve usar um tipo de delegado em tempo de execução do Windows. O exemplo acima usa o tipo de delegado [**Windows::Foundation::EventHandler\ < T\ >**](/uwp/api/windows.foundation.eventhandler) tempo de execução do Windows. [**TypedEventHandler\ < TSender, TResult\ >**](/uwp/api/windows.foundation.eventhandler) é outro exemplo de um tipo de representante de tempo de execução do Windows.

Os parâmetros de tipo para esses tipos de dois representante tem cruze ABI, portanto, os parâmetros de tipo devem ser muito tipos de tempo de execução do Windows. Que inclui classes de tempo de execução da primeira e terceira parte, bem como tipos primitivos, como números e cadeias de caracteres. O compilador ajuda com um erro "*deve ser do tipo WinRT*" Se você esquecer essa restrição.

Se você não precisa passar quaisquer parâmetros ou argumentos com seu evento, você pode definir seu próprio tipo de representante de tempo de execução do Windows simples. O exemplo a seguir mostra uma versão mais simples da classe **BankAccount** runtime. Ele declara um tipo de representante chamado **SignalDelegate** e, em seguida, ele usa que para gerar um evento de tipo de sinal em vez de um evento com um parâmetro.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Representantes parametrizados, sinais simples e retornos de chamada dentro de um projeto

Se o seu evento é usado apenas internamente dentro C + + / WinRT project (não em binários), então você usar o modelo de struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , mas você parametriza com C + + / Windows Runtime [**winrt::delegate do WinRT&lt;… T&gt; **](/uwp/cpp-ref-for-winrt/delegate) modelo struct, que é um representante eficiente, contado de referência. Ele oferece suporte a qualquer número de parâmetros, e eles não estão limitados aos tipos de tempo de execução do Windows.

Primeiro, o exemplo a seguir mostra um representante assinatura que não terá nenhum parâmetro (essencialmente um sinal simple) e, em seguida, que tem uma cadeia de caracteres.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

Observe como você pode adicionar ao evento quantos representantes inscritos conforme desejado. No entanto, há alguma sobrecarga associada a um evento. Se tudo o que você precisa é um retorno de chamada simple com apenas um único delegado da assinatura, então você pode usar [**winrt::delegate&lt;… T&gt; **](/uwp/cpp-ref-for-winrt/delegate) por conta própria.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Se você estiver portabilidade de C + + / CX codebase onde eventos e delegados são usados internamente dentro de um projeto, então **winrt::delegate** ajudará você a replicar padrão na C + + / WinRT.

## <a name="design-guidelines"></a>Diretrizes de design

É recomendável que você passe eventos e não delegados, como parâmetros de função. A função **Adicionar** [**winrt::event**](/uwp/cpp-ref-for-winrt/event) é uma exceção, pois você deve passar um representante nesse caso. O motivo para esta diretriz é porque os representantes podem ter formas diferentes em idiomas diferentes do tempo de execução do Windows (em termos de se eles suportam o registro de um cliente ou múltiplo). Eventos, com seu modelo de assinante múltiplo, constituem uma opção muito mais previsível e consistente.

A assinatura de um manipulador de eventos delegado deve consistir em dois parâmetros: o *remetente* (**IInspectable**) e *args* (algum argumento tipo de evento, por exemplo [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Observe que essas diretrizes não necessariamente se aplicam se você estiver criando uma API interna. Embora, APIs internos acabam sendo públicas ao longo do tempo.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C++/WinRT](author-apis.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Manejar eventos usando delegados em C++/WinRT](handle-events.md)
