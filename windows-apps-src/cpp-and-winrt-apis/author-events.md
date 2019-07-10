---
description: Este tópico demonstra como criar um componente do Tempo de Execução do Windows contendo uma classe de tempo de execução que gera eventos. Ele também demonstra um aplicativo que consome o componente e maneja os eventos.
title: Criar eventos em C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, criar, evento
ms.localizationpriority: medium
ms.openlocfilehash: 5a3c834a1696b65099549aa001338a8a02f60e50
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64745235"
---
# <a name="author-events-in-cwinrt"></a>Criar eventos em C++/WinRT

Este tópico demonstra como criar um Componente do Tempo de Execução do Windows contendo uma classe de tempo de execução representando uma conta bancária, que gera um evento quando seu saldo entra em débito. Também demonstra um App Core que utiliza a classe de tempo de execução de conta bancária, chama uma função para ajustar o saldo e manipula todos os eventos resultantes.

> [!NOTE]
> Para saber mais sobre como instalar e usar a VSIX (Extensão do Visual Studio) para [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) e o pacote NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Para ver conceitos e termos essenciais que ajudam a entender como utilizar e criar classes de tempo de execução com C++/WinRT, confira [Utilizar APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Criar um componente do Tempo de Execução do Windows (BankAccountWRC)

Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Componente do Tempo de Execução do Windows (C++/WinRT)** e nomeie-o como *BankAccountWRC* (para "Componente do Tempo de Execução do Windows para conta bancária"). Não compile o projeto ainda.

O projeto recém-criado contém um arquivo chamado `Class.idl`. Renomeie esse arquivo `BankAccount.idl` (renomear o arquivo `.idl` também renomeia automaticamente os arquivos dependentes `.h` e `.cpp`). Substitua o conteúdo de `BankAccount.idl` pela listagem a seguir.

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

Salve o arquivo. O projeto não será compilado até a conclusão, mas é útil compilá-lo agora porque ele gera os arquivos de código fonte nos quais você implementará a classe de tempo de execução **BankAccount**. Portanto, vá em frente e compile agora (os erros de compilação que você espera ver neste estágio têm a ver com `Class.h` e `Class.g.h` que não foram encontrados).

Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar o arquivo de metadados do componente do Tempo de Execução do Windows (que é `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Em seguida, a ferramenta `cppwinrt.exe` é executada (com a opção `-component`) para gerar arquivos de código fonte que dão suporte na criação do componente. Esses arquivos incluem stubs para ajudá-lo a começar a implementar a classe de tempo de execução **BankAccount**, que foi declarada em seu IDL. Esses stubs são `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` e `BankAccount.cpp`.

Clique com o botão direito do mouse no nó do projeto e clique em **Abrir Pasta no Explorador de Arquivos**. A pasta do projeto abre no Explorador de Arquivos. Copie os arquivos de stub `BankAccount.h` e `BankAccount.cpp` da pasta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` para a pasta que contém os arquivos de projeto, que é `\BankAccountWRC\BankAccountWRC\`, e substitua os arquivos no destino. Agora vamos abrir `BankAccount.h` e `BankAccount.cpp`, e implementar nossa classe de tempo de execução. Em `BankAccount.h`, adicione dois membros privados à implementação de BankAccount, *não* à implementação de fábrica.

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

Como você pode ver acima, o evento é implementado em termos do modelo de struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event), parametrizado por um determinado tipo delegado.

Em `BankAccount.cpp`, implemente as funções, como mostrado no exemplo de código abaixo. No C++/WinRT, um evento declarado de IDL é implementado como um conjunto de funções sobrecarregadas (de modo semelhante à maneira como uma propriedade é implementada como um par de funções get e set sobrecarregadas). Uma sobrecarga resulta no registro de um delegado e retorna um token. A outra recebe um token e revoga o registro do delegado associado.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
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

Você não precisa implementar a sobrecarga do revogador do evento (para saber mais, confira [Revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate)); isso já foi resolvido na projeção de C++/WinRT. As outras sobrecargas não são incorporadas na projeção, para que você tenha flexibilidade de implementá-las da forma ideal para o seu cenário. Chamar [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) e [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) dessa forma é um padrão eficiente, simultâneo e thread-safe. Mas se houver um grande número de eventos, talvez não seja ideal um campo de evento para cada um, em vez disso, opte por algum tipo de implementação esparsa.

Você também pode ver acima que a implementação da função **AdjustBalance** gera o evento **AccountIsInDebit** se o saldo ficar negativo.

Se um aviso impedir a compilação, resolva-o ou defina a propriedade de projeto **C/C++**  > **Geral** > **Tratar avisos como erros** como **Não (/WX-)** , e compile o projeto novamente.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Criar um App Core (BankAccountCoreApp) para testar o Componente do Tempo de Execução do Windows

Agora crie um novo projeto (na sua solução `BankAccountWRC` ou em uma nova). Crie um projeto do **App Core (C++/WinRT)** e nomeie-o como *BankAccountCoreApp*.

Adicione uma referência e navegue até `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou adicione uma referência de projeto a projeto, se os dois projetos estiverem na mesma solução). Clique em **Adicionar** e em **OK**. Agora compile o BankAccountCoreApp. Se houver um erro, o que é improvável, que o arquivo de carga `readme.txt` não existe, exclua esse arquivo do projeto do Componente do Tempo de Execução do Windows, compile-o novamente e recompile BankAccountCoreApp.

Durante o processo de compilação, a ferramenta `cppwinrt.exe` é executada para processar o arquivo `.winmd` mencionado nos arquivos de código-fonte que contêm os tipos projetados para ajudá-lo a utilizar o componente. O cabeçalho dos tipos projetados para suas classes de tempo de execução do componente, denominado `BankAccountWRC.h`, é gerado na pasta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Inclua esse cabeçalho em `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Também em `App.cpp`, adicione o seguinte código para instanciar uma BankAccount (usando o construtor padrão do tipo projetado), registre um manipulador de eventos e faça com que a conta entre em débito.

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
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
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

Cada vez que você clicar na janela, você subtrai 1 do saldo da conta bancária. Para demonstrar que o evento está sendo gerado conforme o esperado, coloque um ponto de interrupção dentro da expressão lambda que está manipulando o evento **AccountIsInDebit**, execute o aplicativo e clique dentro da janela.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Delegados parametrizados e sinais simples em uma ABI

Se o evento deve estar acessível por meio de uma ABI (interface binária de aplicativo), como entre um componente e seu aplicativo de consumo, o evento deve usar um tipo delegado do Windows Runtime. O exemplo acima usa o tipo delegado [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) do Windows Runtime. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) é outro exemplo de um tipo delegado do Windows Runtime.

Os parâmetros de tipo desses dois tipos delegados têm que cruzar a ABI, portanto, os parâmetros de tipo também devem ser do tipo do Windows Runtime. Isso inclui classes de tempo de execução primárias e de terceiros, bem como tipos primitivos, como números e cadeias de caracteres. O compilador o ajudará com um erro "*deve ser do tipo WinRT*", caso você esqueça essa restrição.

Se você não precisa passar nenhum parâmetro ou argumento com seu evento, defina seu próprio tipo delegado simples do Windows Runtime. O exemplo a seguir mostra uma versão mais simples da classe de tempo de execução **BankAccount**. Ele declara um tipo delegado chamado **SignalDelegate** e, em seguida, usa isso para gerar um evento de tipo de sinal ao invés de um evento com um parâmetro.

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Delegados parametrizados, sinais simples e retornos de chamada dentro de um projeto

Se o evento é usado somente internamente em seu projeto de C++/WinRT (não entre binários), ainda é possível usar o modelo de struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event), mas é preciso parametrizá-lo com o modelo de struct do C++/WinRT [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate), que não é do Windows Runtime e é um delegado eficiente e de contagem de referências. E é compatível com qualquer número de parâmetros e não está limitado aos tipos do Windows Runtime.

O exemplo a seguir mostra primeiro uma assinatura do delegado que não usa parâmetros (basicamente, um sinal simples) e, em seguida, outra que usa uma cadeia de caracteres.

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

Observe como é possível adicionar tantos delegados de assinaturas quanto desejar ao evento. No entanto, há alguma sobrecarga associada a um evento. Se tudo o que você precisa é um retorno de chamada simples com apenas um único delegado da assinatura, então, use somente [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate).

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Se estiver fazendo a portabilidade de uma base de código do C++/CX, em que os eventos e delegados são usados internamente em um projeto, **winrt::delegate** ajuda a replicar esse padrão em C++/WinRT.

## <a name="design-guidelines"></a>Diretrizes de design

É recomendável passar eventos, e não delegados, como parâmetros de função. A função **add** de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) é a única exceção, porque nesse caso é preciso passar um delegado. A razão para essa diretriz é porque os delegados podem assumir diferentes formas em diversas linguagens do Windows Runtime (considerando se dão suporte a um ou vários registros de cliente). Eventos, com seus vários modelos de assinantes, constituem uma opção muito mais previsível e consistente.

A assinatura para um delegado de manipulador de eventos deve consistir de dois parâmetros: *sender* (**IInspectable**) e *args* (algum tipo de argumento de evento, por exemplo [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Observe que essas diretrizes não se aplicam necessariamente se você estiver criando uma API interna. No entanto, as APIs internas geralmente se tornam públicas ao longo do tempo.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C++/WinRT](author-apis.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Manipular eventos usando delegados em C++/WinRT](handle-events.md)
