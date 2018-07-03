---
author: stevewhims
description: Este tópico demonstra como criar um componente do Tempo de Execução do Windows contendo uma classe de tempo de execução que gera eventos. Ele também demonstra um aplicativo que consome o componente e maneja os eventos.
title: Criar eventos com C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, criar, evento
ms.localizationpriority: medium
ms.openlocfilehash: 192000f937989d7218931ce1465bd96d5d9b71c6
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880877"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Criar eventos com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tópico demonstra como criar um componente do Tempo de Execução do Windows que contém uma classe de tempo de execução representando uma conta bancária, que gera um evento quando seu saldo entra em débito. Ele também demonstra um aplicativo principal que consome a classe de tempo de execução de conta bancária, chama uma função para ajustar o saldo e manipula todos os eventos resultantes.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt; e TypedEventHandler&lt;T&gt;
Se você deseja disparar um evento de uma classe de tempo de execução implementada em um componente do Tempo de Execução do Windows, então você deve usar [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) ou [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler) para seu tipo de representante de evento. Os parâmetros de tipo devem ser tipos do Windows Runtime, portanto, tipos primitivos são permitidos, bem como classes de tempo de execução de terceiros.

O compilador ajudará você com um erro "*deve ser o tipo WinRT*" se você esquecer essa restrição.

## <a name="winrtdelegatelt-tgt"></a>winrt::delegate&lt;...T&gt;
Se você deseja disparar um evento de um tipo C++ (criado e consumido dentro do mesmo projeto), então você pode usar [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) de C++/WinRT para o tipo delegado do evento. Nesse caso, os parâmetros de tipo do delegado não precisam ser tipos do Windows Runtime. Se estiver fazendo a portabilidade de uma base de código C++/CX, onde os eventos e delegados são usados internamente (e não em binários), **winrt::delegate** ajuda você a replicar esse padrão em C++/WinRT.

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Criar um componente do Tempo de Execução do Windows (BankAccountWRC)
Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Componente do Tempo de Execução do Windows com Visual C++ (C++/WinRT)** e nomeie *BankAccountWRC* (para "conta bancária do componente do Tempo de Execução do Windows").

O projeto recém-criado contém um arquivo chamado `Class.idl`. Renomeie o arquivo `BankAccountWRC.idl` para que, quando você o compilar, o arquivo de metadados do componente do Tempo de Execução do Windows seja chamado para o componente em si. Em `BankAccountWRC.idl`, defina sua interface como na lista abaixo.

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

Salve o arquivo e compile o projeto. A compilação não acontecerá ainda. Mas, durante o processo de compilação, a ferramenta `midl.exe` é executada para criar o arquivo de metadados do componente do Tempo de Execução do Windows (que é `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Em seguida, a ferramenta `cppwinrt.exe` é executada (com a opção `-component`) para gerar arquivos de código fonte para dar suporte a você na criação do componente. Esses arquivos incluem stubs para ajudar você a começar a implementar a classe de tempo de execução `BankAccount` que foi declarada em sua IDL. Esses stubs são `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` e `BankAccount.cpp`.

Copie os arquivos stub `BankAccount.h` e `BankAccount.cpp` de `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` na pasta do projetor, que é `\BankAccountWRC\BankAccountWRC\`. No **Explorador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**. Além disso, clique com botão direito em `Class.h` e em `Class.cpp`, e clique em **Excluir do projeto**.

Agora, vamos abrir `BankAccount.h` e `BankAccount.cpp` e implementar nossa classe de tempo de execução. Em `BankAccount.h`, adicione dois membros privados à implementação (*não* a implementação de fábrica) de BankAccount.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

Em `BankAccount.cpp`, implemente as funções, conforme mostrado no exemplo de código abaixo. Em C++/WinRT, um evento declarado por IDL é implementado como um conjunto de funções sobrecarregadas (semelhante à forma como um propriedade é implementada como um par de funções get e set sobrecarregadas). Uma sobrecarga resulta no registro de um delegado e retorna um token. O outro recebe um token e revoga o registro do delegado associado.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(event_token const& token)
    {
        accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        balance += value;
        if (balance < 0.f) accountIsInDebitEvent(*this, balance);
    }
}
```

Se não precisar implementar a sobrecarga do revogador do evento (para obter mais detalhes, consulte [Revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate))&mdash;isso é resolvido para você pela projeção de C++/WinRT. As outras sobrecargas não são incorporadas na projeção a fim de oferecer a flexibilidade para implementá-las da forma ideal no seu cenário. Chamar [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) e [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) dessa forma é um padrão eficiente e simultâneo/seguro para o thread. Mas se houver um grande número de eventos, você talvez não queira um campo de evento para cada uma, mas em vez disso, opte por algum tipo de implementação esparsa.

Você também pode ver acima que essa implementação da função **AdjustBalance** gera o evento **AccountIsInDebit** se o saldo ficar negativo.

Se quaisquer avisos o impedirem de criar, então defina a propriedade de projeto como **C/C++** > **Geral** > **Tratar avisos como erros** para **Não (/WX-)** e crie o projeto novamente.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Criar um aplicativo principal (BankAccountCoreApp) para testar o componente do Tempo de Execução do Windows
Agora crie um novo projeto (em sua solução `BankAccountWRC` ou em uma nova). Crie um projeto do **Aplicativo principal do Visual C++ (C++/WinRT)** e nomeie-o *BankAccountCoreApp*.

Adicione uma referência e navegue até `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou adicione uma referência de projeto, se os dois projetos estiverem na mesma solução). Clique em **Adicionar** e, depois, em **OK**. Agora crie BankAccountCoreApp. Se você vir um erro onde o arquivo de carga `readme.txt` não existe, então exclua esse arquivo do projeto do componente do Tempo de Execução do Windows, reconstrua-o e recompile BankAccountCoreApp.

Durante o processo de compilação, a ferramenta `cppwinrt.exe` é executada para processar o arquivo `.winmd` referenciado nos arquivos de código-fonte que contêm tipos projetados para ajudar você a consumir o componente. O cabeçalho dos tipos projetados de suas classes de tempo de execução do componente&mdash;nomeado `BankAccountWRC.h`&mdash;é gerado na pasta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Inclua esse cabeçalho em `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Também em `App.cpp`, adicione o seguinte código para instanciar uma BankAccount (usando o construtor padrão do tipo projetado), registre um manipulador de eventos e, em seguida, faça com que a conta entre em débito.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    event_token eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        eventToken = bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            assert(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        bankAccount.AccountIsInDebit(eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Cada vez que você clicar na janela, subtraia 1 do saldo da conta bancária. Para demonstrar que o evento está sendo acionado conforme o esperado, insira um ponto de interrupção dentro da expressão lambda, execute o aplicativo e clique dentro da janela.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C++/WinRT](author-apis.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Manejar eventos usando delegados em C++/WinRT](handle-events.md)
