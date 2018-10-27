---
author: msatranjr
title: Acionando eventos em componentes do Tempo de Execução do Windows
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Como disparar um evento de um tipo de representante definido pelo usuário em um thread em segundo plano para que o JavaScript é capaz de receber o evento.
ms.author: misatran
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2dddd170f5f056de18c4729b6b6b5b4b6cbcea7b
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5704658"
---
# <a name="raising-events-in-windows-runtime-components"></a>Acionando eventos em componentes do Tempo de Execução do Windows
> [!NOTE]
> Para saber como disparar eventos em um [C++ c++ WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) componente do tempo de execução do Windows, consulte [criar eventos em C++ com c++ WinRT](../cpp-and-winrt-apis/author-events.md).

Caso o componente do Tempo de Execução do Windows acione um evento de um tipo representante definido pelo usuário em um thread em segundo plano (thread de trabalho) e você deseje que o JavaScript seja capaz de receber o evento, é possível implementar e/ou acioná-lo destas formas:

-   (Opção 1) Acione o evento por meio do [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) para realizar marshaling do evento para o contexto do thread JavaScript. Embora normalmente essa seja a melhor opção, em alguns cenários ela talvez não ofereça o desempenho mais rápido.
-   (Opção 2) Usar [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;, mas perder informações sobre o tipo (mas perder as informações sobre o tipo de evento). Caso a Opção 1 não seja viável ou o desempenho não seja adequado, essa é uma boa segunda opção caso a perda de informações sobre o tipo seja aceitável.
-   (Opção 3) Crie os próprios proxy e stub do componente. Essa opção é mais difícil de implementar, mas preserva informações sobre o tipo e pode fornecer um desempenho melhor em comparação com a Opção 1 em cenários exigentes.

Se você simplesmente acionar um evento em um thread em segundo plano sem usar uma dessas opções, um cliente JavaScript não receberá o evento.

## <a name="background"></a>Segundo plano

Todos os componentes do Tempo de Execução do Windows e aplicativos são fundamentalmente objetos COM, independentemente da linguagem que você usa para criá-los. Na API do Windows, a maioria dos componentes é de objetos COM Agile que podem se comunicar igualmente bem com objetos no thread em segundo plano e no thread da interface do usuário. Caso um objeto COM não possa ser Agile, isso requer que objetos auxiliares conhecidos como proxies e stubs se comuniquem com outros objetos COM em todo o limite de thread em segundo plano do thread de interface do usuário. (Em termos de COM, isso é conhecido como comunicação entre apartments de thread.)

A maioria dos objetos na API do Windows é Agile ou tem proxies e stubs integrados. No entanto, proxies e stubs não podem ser criados para tipos genéricos, como Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) porque eles só serão tipos completos quando você fornecer o argumento de tipo. É apenas com clientes JavaScript que a falta de proxies ou stubs se torna um problema, mas caso queira que o componente seja utilizável em JavaScript, bem como em C++ ou em uma linguagem .NET, você deve usar uma das três opções a seguir.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(Opção 1) Acionar o evento por meio de CoreDispatcher

Você pode enviar eventos de qualquer tipo de representante definido pelo usuário usando o [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx), e o JavaScript poderá recebê-los. Caso você não tenha certeza de qual opção usar, tente esta primeiro. Caso a latência entre o acionamento do evento e a manipulação do evento se torne um problema, tente uma das outras opções.

O exemplo a seguir mostra como usar o CoreDispatcher para acionar um evento fortemente tipado. O argumento do tipo é Toast, e não Object.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(Opção 2) Usar EventHandler &lt;Object&gt;, mas perder informações sobre o tipo

Outra maneira de enviar um evento de um thread em segundo plano é usar [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; como o tipo do evento. O Windows oferece essa instanciação concreta do tipo genérico e fornece um proxy e um stub para ele. A desvantagem é que as informações de tipo dos argumentos de evento e remetente são perdidas. Os clientes C++ e .NET devem saber pela documentação para qual tipo reconverter quando o evento é recebido. Os clientes JavaScript não precisam das informações sobre o tipo original. Eles encontram as propriedades arg, com base nos nomes nos metadados.

Este exemplo mostra como usar Windows.Foundation.EventHandler&lt;Object&gt; em C#:

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Você consome esse evento no lado do JavaScript desta forma:

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(Opção 3) Criar os próprios proxy e stub

Para ganhos de desempenho potenciais em tipos de eventos definidos pelo usuário que tenham informações de tipo totalmente preservadas, você precisa criar os próprios objetos de proxy e stub e incorporá-los ao pacote do aplicativo. Normalmente, você só precisa usar essa opção em situações raras nas quais nenhuma das outras duas opções são adequadas. Além disso, não há garantia de que essa opção fornecerá desempenho melhor do que as outras duas opções. O desempenho real depende de muitos fatores. Use o criador de perfil do Visual Studio ou outras ferramentas de criação de perfil para avaliar o desempenho real no aplicativo e determinar se o evento é, na verdade, um afunilamento.

O restante deste artigo mostra como usar C# para criar um componente do Tempo de Execução do Windows básico e usar C++ para criar uma DLL para o proxy e o stub que permitirão que o JavaScript consuma um evento Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; acionado pelo componente em uma operação assíncrona. (Também é possível usar C++ ou Visual Basic para criar o componente. As etapas relacionadas à criação de proxies e stubs são as mesmas.) Este procedimento passo a passo se baseia na criação de uma amostra de componente no processo de Windows Runtime (C++/CX) e ajuda a explicar as finalidades.

Este procedimento passo a passo tem estas partes:

-   Aqui, você criará duas classes de Tempo de Execução do Windows básicas. Uma classe expõe um evento do tipo [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) e a outra classe é o tipo retornado para o JavaScript como o argumento de TValue. Essas classes não podem se comunicar com JavaScript até você concluir as etapas posteriores.
-   Este aplicativo ativa o objeto de classe principal, chama um método e manipula um evento acionado pelo componente do Tempo de Execução do Windows.
-   Elas são exigidas pelas ferramentas para gerar as classes de proxy e stub.
-   Em seguida, você usa o arquivo IDL para gerar o código-fonte C para o proxy e o stub.
-   Registre os objetos proxy-stub de maneira que o tempo de execução COM possa encontrá-los e faça referência à DLL proxy-stub no projeto do aplicativo.

## <a name="to-create-the-windows-runtime-component"></a>Para criar o componente do Tempo de Execução do Windows

No Visual Studio, na barra de menus, selecione **Arquivo &gt; Novo Projeto**. Na caixa de diálogo **Novo Projeto**, expanda **JavaScript &gt; Universal Windows** e selecione **Aplicativo em Branco**. Nomeie o projeto ToasterApplication e escolha o botão **OK**.

Adicione um componente do Tempo de Execução do Windows C# à solução: no Gerenciador de Soluções, abra o menu de atalho da solução e escolha **Adicionar&gt; Novo Projeto**. Expanda **Visual c# &gt; Microsoft Store** e, em seguida, selecione o **Componente de tempo de execução do Windows**. Nomeie o projeto ToasterComponent e escolha o botão **OK**. ToasterComponent será o namespace raiz dos componentes que você criará em etapas posteriores.

No Gerenciador de Soluções, abra o menu de atalho da solução e escolha **Propriedades**. Na caixa de diálogo **Páginas de Propriedades**, selecione **Propriedades de Configuração** no painel esquerdo e, na parte superior da caixa de diálogo, defina **Configuração** como **Depurar** e **Plataforma** como x86, x64 ou ARM. Escolha o botão **OK**.

**Importante**plataforma = nenhuma CPU funcionará porque ele não é válido para a DLL Win32 de código nativo que você adicionará à solução mais tarde.

No Gerenciador de Soluções, renomeie class1.cs para ToasterComponent.cs de maneira que ele corresponda ao nome do projeto. O Visual Studio renomeia automaticamente a classe no arquivo para coincidir com o novo nome de arquivo.

No arquivo .cs, adicione uma diretiva using para o namespace Windows.Foundation a fim de colocar TypedEventHandler em escopo.

Quando você precisa de proxies e stubs, o componente deve usar interfaces para expor os membros públicos. Em ToasterComponent.cs, defina uma interface para o notificador do sistema (toaster) e outra para a notificação (Toast) que o notificador produz.

**Observação**no c# pode ignorar esta etapa. Em vez disso, crie primeiro uma classe e, em seguida, abra o menu de atalho e escolha **Refatorar &gt; Extrair Interface**. No código gerado, dê manualmente às interfaces acessibilidade pública.

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

A interface IToast tem uma cadeia de caracteres que pode ser recuperada para descrever o tipo de notificação do sistema. A interface IToaster tem um método para criar a notificação do sistema e um evento para indicar que a notificação do sistema foi feita. Como retorna a parte específica (ou seja, tipo) da notificação do sistema, esse evento é conhecido como um evento tipado.

Em seguida, precisamos de classes que implementem essas interfaces e sejam públicas e seladas de maneira que permaneçam acessíveis no aplicativo JavaScript que você programará depois.

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

No código anterior, criamos a notificação do sistema e giramos um item de trabalho do pool de threads para acionar a notificação. Embora o IDE possa sugerir que você aplique a palavra-chave await à chamada assíncrona, isso não é necessário neste caso porque o método não faz nenhum trabalho que dependa dos resultados da operação.

**Observação**a chamada assíncrona no código anterior usa ThreadPool exclusivamente para demonstrar uma maneira simples de acionar o evento em um thread em segundo plano. Você pode escrever esse método em particular, conforme mostrado no exemplo a seguir, e ele funcionaria bem porque o agendador de tarefas .NET realiza marshaling automaticamente de chamadas async/await de volta para o thread da interface do usuário.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Se você compilar o projeto agora, ele deve criar corretamente.

## <a name="to-program-the-javascript-app"></a>Programa de aplicativo JavaScript

Agora podemos adicionar um botão para o aplicativo de JavaScript para fazer com que ele use a classe que são definidas apenas para tornar a notificação do sistema. Antes de fazer isso, podemos deve adicionar uma referência ao projeto ToasterComponent que acabamos de criar. No Gerenciador de soluções, abra o menu de atalho para o projeto ToasterApplication, escolha **Adicionar &gt; referências**e, em seguida, escolha o botão **Adicionar nova referência** . Na caixa de diálogo Adicionar referência, no painel esquerdo na solução, selecione o projeto de componente e, em seguida, no painel central, selecione ToasterComponent. Escolha o botão **OK**.

No Gerenciador de soluções, abra o menu de atalho para o projeto ToasterApplication e escolha **Definir como projeto de inicialização**.

No final do arquivo default. js, adicione um namespace para conter as funções para chamar o componente e a ser chamada por ele. O namespace terá duas funções, uma para fazer a notificação do sistema e outro para manipular o evento de conclusão de notificação do sistema. A implementação de makeToast cria um objeto notificador, registra o manipulador de eventos e faz a notificação do sistema. Até agora, o manipulador de eventos não faz muito, conforme mostrado aqui:

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

A função makeToast deve ser conectada a um botão. Atualize default para incluir um botão e algum espaço para gerar o resultado de tornar a notificação do sistema:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Se nós não estavam usando um TypedEventHandler, podemos agora será capazes de executar o aplicativo no computador local e clique no botão para fazer a notificação do sistema. Mas, em nosso aplicativo, nada acontece. Para descobrir o motivo, vamos depurar o código gerenciado que dispara o ToastCompletedEvent. Parar o projeto e, em seguida, na barra de menus, escolha **Depurar &gt; propriedades do Application Notificador**. Alterar o **tipo de depurador** para **só gerenciado**. Novamente na barra de menus, escolha **Depurar &gt; exceções**e, em seguida, selecione **Common Language Runtime Exceptions**.

Agora execute o aplicativo e clique no botão de notificação do sistema marca. O depurador detecta uma exceção de conversão inválida. Embora não seja óbvio a partir de sua mensagem, essa exceção está ocorrendo porque proxies estão ausentes para essa interface.

![proxy ausente](./images/debuggererrormissingproxy.png)

A primeira etapa na criação de um proxy e stub para um componente é adicionar uma ID ou o GUID exclusivo para as interfaces. Entretanto, o formato GUID para usar diferente dependendo se você estiver codificando em c#, Visual Basic ou outra linguagem .NET ou em C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Para gerar GUIDs para interfaces do componente (c# e outras linguagens .NET)

Na barra de menus, escolha ferramentas &gt; criar GUID. Na caixa de diálogo, selecione 5. \[GUID ("xxxxxxxx-xxxx... xxxx) \]. Escolha o botão novo GUID e, em seguida, escolha o botão de cópia.

![ferramenta de gerador de GUID](./images/guidgeneratortool.png)

Volte para a definição de interface e cole o novo GUID antes da interface IToaster, conforme mostrado no exemplo a seguir. (Não use o GUID no exemplo. Cada interface exclusivo deve ter seu próprio GUID).

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Adicionar um usando diretiva para o namespace InteropServices.

Repita essas etapas para a interface IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Para gerar GUIDs para interfaces do componente (C++)

Na barra de menus, escolha ferramentas &gt; criar GUID. Na caixa de diálogo, selecione 3. struct static const GUID = {...}. Escolha o botão novo GUID e, em seguida, escolha o botão de cópia.

Cole o GUID antes da definição de interface IToaster. Depois de colar, o GUID deve se parecer com o exemplo a seguir. (Não use o GUID no exemplo. Cada interface exclusivo deve ter seu próprio GUID).
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Adicionar um usando diretiva para Windows.Foundation.Metadata trazer GuidAttribute para o escopo.

Agora converta manualmente o GUID const em um GuidAttribute para que ele é formatado conforme mostrado no exemplo a seguir. Observe que as chaves são substituídas por colchetes e parênteses, e o ponto e vírgula à direita é removido.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Repita essas etapas para a interface IToast.

Agora que as interfaces tem IDs exclusivas, podemos criar um arquivo IDL ao alimentam o arquivo. winmd a ferramenta de linha de comando winmdidl e, em seguida, gerar o código-fonte C para o proxy e stub por alimentam desse arquivo IDL a ferramenta de linha de comando MIDL. Visual Studio fazer isso para nós se podemos criar eventos de pós-compilação conforme mostrado nas etapas a seguir.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Para gerar o proxy e stub de código-fonte

Para adicionar um evento de pós-compilação personalizado, no Gerenciador de soluções, abra o menu de atalho para o projeto ToasterComponent e, em seguida, escolha Propriedades. No painel esquerdo as páginas de propriedades, selecione compilar eventos e, em seguida, escolha o botão de pós-compilação editar. Adicione os seguintes comandos para a linha de comando de pós-compilação. (O arquivo em lotes deve ser chamado pela primeira vez para definir as variáveis de ambiente para encontrar a ferramenta winmdidl.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Importante**para uma ARM ou x64 configuração do projeto, altere o parâmetro de /env MIDL para x64 ou arm32.

Para garantir que o arquivo IDL é gerado novamente sempre que o arquivo. winmd é alterado, alterar a **executar o evento pós-compilação** para **quando a compilação atualiza a saída do projeto.**
A página de propriedades compilar eventos deve se parecer com isso: ![criar eventos](./images/buildevents.png)

Recrie a solução para gerar e compilar IDL.

Você pode verificar que MIDL compilado corretamente a solução olhando para o ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c e DLLDATA. c no diretório do projeto ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Para compilar o proxy e stub de código em uma DLL

Agora que você tem os arquivos necessários, você pode compilá-los para produzir uma DLL, que é um arquivo em C++. Para torná-lo tão fácil quanto possível, adicione um novo projeto para dar suporte à criação dos proxies. Abra o menu de atalho para a solução ToasterApplication e escolha **Adicionar > Novo projeto**. No painel esquerdo da caixa de diálogo **Novo projeto** , expanda **Visual C++ &gt; Windows &gt; Windows universal**e, em seguida, no painel do meio, selecione **DLL (aplicativos UWP)**. (Observe que isso não é um projeto de componente de tempo de execução do Windows C++). Nomeie o projeto Proxies e, em seguida, escolha o botão de **Okey** . Esses arquivos serão atualizados pelos eventos de pós-compilação quando algo for alterado na classe c#.

Por padrão, o projeto de Proxies gera arquivos de cabeçalho do. h e arquivos. cpp de C++. Como a DLL é compilada dos arquivos produzidos a partir MIDL, os arquivos. h e. cpp não são necessários. No Gerenciador de soluções, abra o menu de atalho para eles, escolha **Remover**e, em seguida, confirmar a exclusão.

Agora que o projeto estiver vazio, você pode adicionar novamente os arquivos gerados pelo MIDL. Abra o menu de atalho para o projeto de Proxies e, em seguida, escolha **Adicionar > Item existente.** Na caixa de diálogo, navegue até o diretório do projeto ToasterComponent e selecione esses arquivos: arquivos ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c e DLLDATA. c. Escolha o botão **Adicionar** .

No projeto Proxies, crie um arquivo. def para definir as exportações DLL descritas em DLLDATA. c. Abra o menu de atalho do projeto e escolha **Adicionar > Novo Item**. No painel esquerdo da caixa de diálogo, selecione o código e, em seguida, no painel do meio, selecione o arquivo de definição de módulo. Nomeie o arquivo proxies.def e, em seguida, selecione o botão **Adicionar** . Abrir esse arquivo. def e modificá-lo para incluir as exportações que são definidas em DLLDATA. c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Se você compilar o projeto agora, ele falhará. Para compilar corretamente este projeto, você precisa alterar como o projeto é compilado e vinculado. No Gerenciador de soluções, abra o menu de atalho para o projeto de Proxies e, em seguida, escolha **Propriedades**. Altere as páginas de propriedades da seguinte maneira.

No painel esquerdo, selecione **C/C++ > pré-processador**e, em seguida, no painel direito, selecione **As definições de pré-processador**, escolha o botão de seta para baixo e, em seguida, selecione **Editar**. Adicione essas definições na caixa:

```cpp
WIN32;_WINDOWS
```
Sob **C/C++ > cabeçalhos pré-compilados**, altere o **Cabeçalho pré-compilado** para **Não usando pré-compilado cabeçalhos**e, em seguida, escolha o botão de **Aplicar** .

Sob **vinculador > geral**, altere **Ignorar biblioteca de importação** para **Ye**s e, em seguida, escolha o botão de **Aplicar** .

Sob **vinculador > entrada**, selecione **Dependências adicionais**, escolha o botão de seta para baixo e, em seguida, selecione **Editar**. Adicione este texto na caixa:

```cpp
rpcrt4.lib;runtimeobject.lib
```

Essas bibliotecas não cole diretamente na linha da lista. Use a caixa **Editar** para garantir que o MSBuild no Visual Studio manterá as dependências adicionais corretas.

Quando você tiver feito essas alterações, escolha o botão **Okey** na caixa de diálogo **Páginas de propriedades** .

Em seguida, executar uma dependência no projeto ToasterComponent. Isso garante que o Notificador será compilado antes do projeto de proxy é compilado. Isso é necessário porque o projeto Notificador é responsável pela geração de arquivos para criar o proxy.

Abra o menu de atalho para o projeto de Proxies e, em seguida, escolha dependências do projeto. Marque as caixas de seleção para indicar que o projeto de Proxies depende do projeto ToasterComponent, para garantir que o Visual Studio cria-los na ordem correta.

Verifique se que a solução compilações corretamente, escolhendo **Build > solução recompile** na barra de menus do Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Registrar o proxy e stub

No projeto ToasterApplication, abra o menu de atalho de Package. appxmanifest e, em seguida, escolha **Abrir com**. Na caixa de diálogo Abrir com, selecione **Editor de texto XML** e, em seguida, escolha o botão de **Okey** . Vamos para colar em alguns XML que fornece que um registro de extensão windows.activatableClass.proxyStub e que se baseiam os GUIDs no proxy. Para localizar os GUIDs para usar no arquivo .appxmanifest, abra ToasterComponent_i.c. Localize entradas que reproduzem aqueles no exemplo a seguir. Observe também as definições para IToast, IToaster e um terceiro interface — um manipulador de eventos tipados que tem dois parâmetros: um Notificador e notificações do sistema. Isso corresponde ao evento que é definido na classe Notificador. Observe que os GUIDs para IToast e IToaster correspondem os GUIDs que são definidos nas interfaces no arquivo c#. Como a interface do manipulador de evento tipado é gerada automaticamente, o GUID para essa interface também é gerado automaticamente.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Agora vamos copiar os GUIDs, colá-los no Package. appxmanifest em um nó que adicionamos e extensões de nome e, em seguida, reformatá-los. A entrada de manifesto se parece com o exemplo a seguir — mas novamente, lembre-se de usar seus próprios GUIDs. Observe que o GUID ClassId no XML é o mesmo que ITypedEventHandler2. Isso ocorre porque esse GUID é o primeiro que esteja listada na ToasterComponent_i.c. Os GUIDs aqui diferenciam maiusculas de minúsculas. Em vez de reformatar manualmente os GUIDs para IToast e IToaster, você pode voltar para as definições de interface e obter o valor GuidAttribute, que tem o formato correto. No C++, há um GUID formatado corretamente no comentário. Em qualquer caso, você deve reformatar manualmente o GUID que é usado para a identificação de classe e o manipulador de eventos.

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

Cole o nó Extensões XML como um filho direto do nó do pacote e um par de, por exemplo, o nó de recursos.

Antes de passar, é importante garantir que:

-   ProxyStub ClassId é definido como o primeiro GUID no arquivo ToasterComponent\_i.c. Use o primeiro GUID é definido nesse arquivo para a identificação de classe. (Isso pode ser o mesmo que o GUID para ITypedEventHandler2.)
-   O caminho é o caminho relativo do pacote do proxy binário. (Neste passo a passo, proxies.dll está na mesma pasta que ToasterApplication.winmd.)
-   Os GUIDs estão no formato correto. (Isso é propensa a erros).
-   A interface IDs no manifesto do corresponder as IIDs no arquivo ToasterComponent\_i.c.
-   Os nomes da interface são exclusivos no manifesto. Porque eles não são usados pelo sistema, você pode escolher os valores. É uma boa prática para escolher nomes de interface que correspondem claramente interfaces que você definiu. Para interfaces gerados, os nomes devem ser indicativas das interfaces geradas. Você pode usar o arquivo ToasterComponent\_i.c para ajudar você a gerar nomes de interface.

Se você tentar executar a solução agora, você receberá um erro que proxies.dll não é parte da carga. Abra o menu de atalho para a pasta de **referências** do projeto ToasterApplication e escolha **Adicionar referência**. Marque a caixa de seleção ao lado do projeto de Proxies. Além disso, certifique-se de que a caixa de seleção ao lado de ToasterComponent também está selecionada. Escolha o botão **OK**.

O projeto agora deve ser compilado. Executar o projeto e verifique se que você pode fazer a notificação do sistema.

## <a name="related-topics"></a>Tópicos relacionados

* [Criando componentes do Tempo de Execução do Windows em C++](creating-windows-runtime-components-in-cpp.md)
