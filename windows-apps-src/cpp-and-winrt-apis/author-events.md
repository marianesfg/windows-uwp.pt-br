---
author: stevewhims
description: Este tópico demonstra como criar um componente do Tempo de Execução do Windows contendo uma classe de tempo de execução que gera eventos. Ele também demonstra um aplicativo que consome o componente e maneja os eventos.
title: Criar eventos com C++/WinRT
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, criar, evento
ms.localizationpriority: medium
ms.openlocfilehash: 2c4d36fa22953bc4745b631303aae62985a5aa05
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6671610"
---
# <a name="author-events-in-cwinrt"></a>Criar eventos com C++/WinRT

Este tópico demonstra como criar um componente do Tempo de Execução do Windows que contém uma classe de tempo de execução representando uma conta bancária, que gera um evento quando seu saldo entra em débito. Ele também demonstra um aplicativo principal que consome a classe de tempo de execução de conta bancária, chama uma função para ajustar o saldo e manipula todos os eventos resultantes.

> [!NOTE]
> Para obter informações sobre como instalar e usar o [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio Extension (VSIX) (que oferece suporte ao modelo de projeto, bem como C++ c++ WinRT MSBuild propriedades e destinos) consulte [suporte do Visual Studio para C++ c++ /WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Criar um componente do Tempo de Execução do Windows (BankAccountWRC)

Comece criando um novo projeto no Microsoft Visual Studio. Criar um **Visual C++** > **Universal do Windows** > **componente do Windows Runtime (C++ c++ WinRT)** projeto e nomeie- *BankAccountWRC* (para "conta bancária componente de tempo de execução do Windows").

O projeto recém-criado contém um arquivo chamado `Class.idl`. Renomeie o arquivo `BankAccount.idl` (renomear o `.idl` arquivo renomeia automaticamente o dependente `.h` e `.cpp` arquivos, também). Substitua o conteúdo do `BankAccount.idl` com a lista abaixo.

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

Salve o arquivo. O projeto não será compilado para a conclusão no momento, mas criando agora é algo útil porque ele gera os arquivos de código de origem no qual você implementará a classe de tempo de execução de **BankAccount** . Portanto, vá em frente e crie agora (os erros de compilação que você pode esperar ver neste estágio tem a ver com `Class.h` e `Class.g.h` não foi encontrado). Durante o processo de compilação, o `midl.exe` ferramenta é executada para criar o arquivo de metadados do componente do tempo de execução do Windows (que é `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Em seguida, a ferramenta `cppwinrt.exe` é executada (com a opção `-component`) para gerar arquivos de código fonte para dar suporte a você na criação do componente. Esses arquivos incluem stubs para ajudar você a começar a implementar a classe de tempo de execução de **BankAccount** que foi declarada em sua IDL. Esses stubs são `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` e `BankAccount.cpp`.

No Explorador de arquivos, copie os arquivos stub `BankAccount.h` e `BankAccount.cpp` da pasta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` para a pasta que contém os arquivos de projeto, que é `\BankAccountWRC\BankAccountWRC\`e substituir os arquivos no destino. Agora, vamos abrir `BankAccount.h` e `BankAccount.cpp` e implementar nossa classe de tempo de execução. Em `BankAccount.h`, adicione dois membros privados à implementação (*não* a implementação de fábrica) de BankAccount.

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

Como você pode ver acima, o evento é implementado em termos de modelo de struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event) parametrizado por um determinado tipo de representante.

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

Se quaisquer avisos impedirem construção, resolvê-los ou definir a propriedade de projeto **C/C++** > **Geral** > **Tratar avisos como erros** **não (/ /WX-)** e compile o projeto novamente.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Criar um aplicativo principal (BankAccountCoreApp) para testar o componente do Tempo de Execução do Windows

Agora crie um novo projeto (em sua solução `BankAccountWRC` ou em uma nova). Criar um **Visual C++** > **Universal do Windows** > **aplicativo principal (C++ c++ WinRT)** projeto e nomeie-a *BankAccountCoreApp*.

Adicione uma referência e navegue até `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou adicione uma referência de projeto a projeto, se os dois projetos estiverem na mesma solução). Clique em **Adicionar** e, depois, em **OK**. Agora crie BankAccountCoreApp. No evento improvável que você vir um erro que o arquivo de carga `readme.txt` não existe, exclua esse arquivo do projeto do componente de tempo de execução do Windows, reconstrua-o e recompile BankAccountCoreApp.

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

Cada vez que você clicar na janela, subtraia 1 do saldo da conta bancária. Para demonstrar que o evento está sendo acionado conforme o esperado, insira um ponto de interrupção dentro da expressão lambda que está manipulando o evento **AccountIsInDebit** , execute o aplicativo e clique dentro da janela.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Delegados parametrizados e sinais simples, em uma ABI

Se o evento deve ser acessível em uma interface binária do aplicativo (ABI)&mdash;como entre um componente e seu aplicativo consumo&mdash;, em seguida, o evento deve usar um tipo de representante de tempo de execução do Windows. O exemplo acima usa a [**Windows::Foundation::EventHandler\<T\ >**](/uwp/api/windows.foundation.eventhandler) tipo de representante de tempo de execução do Windows. [**TypedEventHandler\<TSender, TResult\ >**](/uwp/api/windows.foundation.eventhandler) é outro exemplo de um tipo de representante de tempo de execução do Windows.

Os parâmetros de tipo para esses tipos de dois delegado têm cruzar a ABI, portanto, os parâmetros de tipo devem ser tipos de tempo de execução do Windows, também. Isso inclui classes de tempo de execução de primeiro e de terceiros, bem como tipos primitivos, como números e cadeias de caracteres. O compilador ajuda você com um erro "*deve ser tipo WinRT*" Se você esquecer essa restrição.

Se você não precisará passar os parâmetros ou argumentos com seu evento, você pode definir seu próprio tipo de representante de tempo de execução do Windows simples. O exemplo a seguir mostra uma versão mais simples da classe de tempo de execução **BankAccount** . Ele declara um tipo de representante chamado **SignalDelegate** e, em seguida, que ele usa para disparar um evento de tipo de sinal em vez de um evento com um parâmetro.

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

Se o evento é usado apenas internamente dentro do C++ c++ WinRT projeto (não em binários), em seguida, você usa o modelo de struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , mas você parametriza com C++ c++ WinRT:: delegate [**de Windows Runtime do WinRT&lt;... T&gt; **](/uwp/cpp-ref-for-winrt/delegate) modelo de struct, que é um representante eficiente, contagem de referência. Ele dá suporte a qualquer número de parâmetros, e eles não estão limitados a tipos de tempo de execução do Windows.

Primeiro, o exemplo a seguir mostra um delegado assinatura que não usa os parâmetros (essencialmente um sinal simple) e, em seguida, que aceita uma cadeia de caracteres.

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

Observe como você pode adicionar ao evento quantos representantes assinantes conforme desejar. No entanto, há algumas sobrecarga associada a um evento. Se você só precisará de um retorno de chamada simple com apenas um único delegado da assinatura, você pode usar WinRT:: delegate [**&lt;... T&gt; **](/uwp/cpp-ref-for-winrt/delegate) por conta própria.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Se você estiver fazendo a portabilidade do C++ c++ /CX codebase onde eventos e delegados são usados internamente dentro de um projeto e, em seguida, **WinRT:: delegate** ajudará você a replicar esse padrão em C++ c++ WinRT.

## <a name="design-guidelines"></a>Diretrizes de design

É recomendável que você passe eventos e delegados não, como parâmetros de função. A função **Adicionar** [**winrt::event**](/uwp/cpp-ref-for-winrt/event) é a única exceção, porque você deve passar um delegado nesse caso. O motivo para essa diretriz é como representantes podem levar formas diferentes em diferentes idiomas de tempo de execução do Windows (em termos se eles oferecem suporte de registro de um cliente, ou vários). Eventos, com o modelo de vários assinante, constituem uma opção muito mais consistente e previsível.

A assinatura para um representante do manipulador de eventos deve consistem dois parâmetros: o *remetente* (**IInspectable**) e *argumentos* (alguns eventos tipo de argumento, por exemplo [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Observe que essas diretrizes não necessariamente se aplica se você estiver criando uma API interna. Embora, APIs internas geralmente se tornar públicas ao longo do tempo.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C++/WinRT](author-apis.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Manejar eventos usando delegados em C++/WinRT](handle-events.md)
