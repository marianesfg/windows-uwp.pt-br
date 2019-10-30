---
title: Acionando eventos em componentes do Tempo de Execução do Windows
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Como acionar um evento de um tipo de representante definido pelo usuário em um thread em segundo plano para que o JavaScript seja capaz de receber o evento.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78bc43c26a73a6184e5788ad7d003813e567a8d0
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052015"
---
# <a name="raising-events-in-windows-runtime-components"></a>Acionando eventos em componentes do Tempo de Execução do Windows
> [!NOTE]
> Para saber como gerar eventos em um [ C++](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) componente de Windows Runtime do/WinRT, consulte [criar eventos C++em/WinRT](../cpp-and-winrt-apis/author-events.md).

Caso o componente do Tempo de Execução do Windows acione um evento de um tipo representante definido pelo usuário em um thread em segundo plano (thread de trabalho) e você deseje que o JavaScript seja capaz de receber o evento, é possível implementar e/ou acioná-lo destas formas:

-   (Opção 1) Acione o evento por meio do [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher) para realizar marshaling do evento para o contexto do thread JavaScript. Embora normalmente essa seja a melhor opção, em alguns cenários ela talvez não ofereça o desempenho mais rápido.
-   (Opção 2) Usar [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt;, mas perder informações sobre o tipo (mas perder as informações sobre o tipo de evento). Caso a Opção 1 não seja viável ou o desempenho não seja adequado, essa é uma boa segunda opção caso a perda de informações sobre o tipo seja aceitável.
-   (Opção 3) Crie os próprios proxy e stub do componente. Essa opção é mais difícil de implementar, mas preserva informações sobre o tipo e pode fornecer um desempenho melhor em comparação com a Opção 1 em cenários exigentes.

Se você simplesmente acionar um evento em um thread em segundo plano sem usar uma dessas opções, um cliente JavaScript não receberá o evento.

## <a name="background"></a>Histórico

Todos os componentes do Tempo de Execução do Windows e aplicativos são fundamentalmente objetos COM, independentemente da linguagem que você usa para criá-los. Na API do Windows, a maioria dos componentes é de objetos COM Agile que podem se comunicar igualmente bem com objetos no thread em segundo plano e no thread da interface do usuário. Caso um objeto COM não possa ser Agile, isso requer que objetos auxiliares conhecidos como proxies e stubs se comuniquem com outros objetos COM em todo o limite de thread em segundo plano do thread de interface do usuário. (Em termos de COM, isso é conhecido como comunicação entre apartments de thread.)

A maioria dos objetos na API do Windows é Agile ou tem proxies e stubs integrados. No entanto, proxies e stubs não podem ser criados para tipos genéricos, como Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) porque eles só serão tipos completos quando você fornecer o argumento de tipo. É apenas com clientes JavaScript que a falta de proxies ou stubs se torna um problema, mas caso queira que o componente seja utilizável em JavaScript, bem como em C++ ou em uma linguagem .NET, você deve usar uma das três opções a seguir.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(Opção 1) Acionar o evento por meio de CoreDispatcher

Você pode enviar eventos de qualquer tipo de representante definido pelo usuário usando o [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher), e o JavaScript poderá recebê-los. Caso você não tenha certeza de qual opção usar, tente esta primeiro. Caso a latência entre o acionamento do evento e a manipulação do evento se torne um problema, tente uma das outras opções.

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

Outra maneira de enviar um evento de um thread em segundo plano é usar [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt; como o tipo do evento. O Windows oferece essa instanciação concreta do tipo genérico e fornece um proxy e um stub para ele. A desvantagem é que as informações de tipo dos argumentos de evento e remetente são perdidas. Os clientes C++ e .NET devem saber pela documentação para qual tipo reconverter quando o evento é recebido. Os clientes JavaScript não precisam das informações sobre o tipo original. Eles encontram as propriedades arg, com base nos nomes nos metadados.

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

-   Aqui, você criará duas classes de Tempo de Execução do Windows básicas. Uma classe expõe um evento do tipo [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) e a outra classe é o tipo retornado para o JavaScript como o argumento de TValue. Essas classes não podem se comunicar com JavaScript até você concluir as etapas posteriores.
-   Este aplicativo ativa o objeto de classe principal, chama um método e manipula um evento acionado pelo componente do Tempo de Execução do Windows.
-   Elas são exigidas pelas ferramentas para gerar as classes de proxy e stub.
-   Em seguida, você usa o arquivo IDL para gerar o código-fonte C para o proxy e o stub.
-   Registre os objetos proxy-stub de maneira que o tempo de execução COM possa encontrá-los e faça referência à DLL proxy-stub no projeto do aplicativo.

## <a name="to-create-the-windows-runtime-component"></a>Para criar o componente do Tempo de Execução do Windows

No Visual Studio, na barra de menus, selecione **Arquivo &gt; Novo Projeto**. Na caixa de diálogo **Novo Projeto**, expanda **JavaScript &gt; Universal Windows** e selecione **Aplicativo em Branco**. Nomeie o projeto ToasterApplication e escolha o botão **OK**.

Adicione um componente do Tempo de Execução do Windows C# à solução: no Gerenciador de Soluções, abra o menu de atalho da solução e escolha **Adicionar&gt; Novo Projeto**. Expanda o **Visual C#&gt;Microsoft Store** e, em seguida, selecione **Windows Runtime componente**. Nomeie o projeto ToasterComponent e escolha o botão **OK**. ToasterComponent será o namespace raiz dos componentes que você criará em etapas posteriores.

No Gerenciador de Soluções, abra o menu de atalho da solução e escolha **Propriedades**. Na caixa de diálogo **Páginas de Propriedades**, selecione **Propriedades de Configuração** no painel esquerdo e, na parte superior da caixa de diálogo, defina **Configuração** como **Depurar** e **Plataforma** como x86, x64 ou ARM. Escolha o botão **OK**.

**Importante** plataforma = qualquer CPU não funcionará porque não é válido para a dll Win32 de código nativo que você adicionará à solução mais tarde.

No Gerenciador de Soluções, renomeie class1.cs para ToasterComponent.cs de maneira que ele corresponda ao nome do projeto. O Visual Studio renomeia automaticamente a classe no arquivo para coincidir com o novo nome de arquivo.

No arquivo .cs, adicione uma diretiva using para o namespace Windows.Foundation a fim de colocar TypedEventHandler em escopo.

Quando você precisa de proxies e stubs, o componente deve usar interfaces para expor os membros públicos. Em ToasterComponent.cs, defina uma interface para o notificador do sistema (toaster) e outra para a notificação (Toast) que o notificador produz.

**Observe** em C# você pode ignorar esta etapa. Em vez disso, crie primeiro uma classe e, em seguida, abra o menu de atalho e escolha **Refatorar &gt; Extrair Interface**. No código gerado, dê manualmente às interfaces acessibilidade pública.

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

**Observe** a chamada assíncrona no código anterior usa ThreadPool. RunAsync unicamente para demonstrar uma maneira simples de acionar o evento em um thread em segundo plano. Você pode escrever esse método em particular, conforme mostrado no exemplo a seguir, e ele funcionaria bem porque o agendador de tarefas .NET realiza marshaling automaticamente de chamadas async/await de volta para o thread da interface do usuário.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Caso você compile o projeto agora, ele deve ser compilado tranquilamente.

## <a name="to-program-the-javascript-app"></a>Para programar o aplicativo em JavaScript

Agora podemos adicionar um botão ao aplicativo JavaScript para fazê-lo usar a classe que acabamos de definir para criar a notificação do sistema. Antes de fazer isso, devemos adicionar uma referência ao projeto ToasterComponent que acabamos de criar. No Gerenciador de Soluções, abra o menu de atalho para o projeto ToasterApplication, selecione **Adicionar &gt; Referências** e, em seguida, clique no botão **Adicionar Nova Referência**. Na caixa de diálogo Adicionar referência, no painel esquerdo em Solução, selecione o projeto do componente e, em seguida, no painel do meio, selecione ToasterComponent. Escolha o botão **OK**.

No Gerenciador de Soluções, abra o menu de atalho do projeto ToasterApplication e escolha **Definir como Projeto de Inicialização**.

Ao final do arquivo default.js, adicione um namespace para conter as funções para chamar o componente e ser chamado por ele. O namespace terá duas funções, uma para criar a notificação do sistema e outra para manipular o evento toast-complete. A implementação de makeToast cria um objeto Toaster, registra o manipulador de eventos e cria a notificação do sistema. Até agora, o manipulador de eventos não faz muito, conforme mostrado aqui:

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

A função makeToast deve ser conectada a um botão. Atualize default.html para incluir um botão e um espaço para gerar o resultado da notificação do sistema:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Se não estivéssemos usando um TypedEventHandler, poderíamos agora executar o aplicativo na máquina local e clicar no botão para criar a notificação do sistema. Mas, em nosso aplicativo, nada acontece. Para descobrir o motivo, vamos depurar o código gerenciado que dispara o ToastCompletedEvent. Pare o projeto e, em seguida, na barra de menu, selecione **Depurar &gt; propriedades do Toaster Application**. Altere o **Tipo de depurador** para **Somente gerenciado**. Novamente na barra de menu, selecione **Depurar &gt; Exceções** e, em seguida, selecione **Exceções de tempo de execução de linguagem comum**.

Agora execute o aplicativo e clique no botão para criar notificações do sistema. O depurador detecta uma exceção de conversão inválida. Embora não seja óbvio por sua mensagem, esta exceção está ocorrendo porque os proxies estão faltando para essa interface.

![proxy ausente](./images/debuggererrormissingproxy.png)

O primeiro passo na criação de um proxy e um talão para um componente é adicionar um ID ou GUID exclusivo às interfaces. No entanto, o formato GUID a ser usado muda dependendo se você estiver codificando em C#, Visual Basic ou outra linguagem .NET ou em C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Para gerar GUIDs para as interfaces do componente (C# e outras linguagens .NET)

Na barra de menus, selecione Ferramentas &gt; Criar GUID. Na caixa de diálogo, selecione 5. GUID de \[("xxxxxxxx-xxxx... xxxx ")\]. Escolha o botão Novo GUID e o botão Copiar.

![ferramenta geradora de GUIDs](./images/guidgeneratortool.png)

Volte para a definição da interface e cole o novo GUID antes da interface IToaster, conforme mostrado no exemplo a seguir. (Não use o GUID no exemplo. Cada interface exclusiva deve ter o próprio GUID.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Adicione uma diretiva using para o namespace System.Runtime.InteropServices.

Repita essas etapas para a interface IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Para gerar GUIDs para as interfaces do componente (C ++)

Na barra de menus, selecione Ferramentas &gt; Criar GUID. Na caixa de diálogo, selecione 3. estrutura estática const GUID = {...}. Escolha o botão Novo GUID e o botão Copiar.

Cole o GUID pouco antes da definição da interface IToaster. Após a colagem, o GUID deve ser parecido com o exemplo a seguir. (Não use o GUID no exemplo. Cada interface exclusiva deve ter o próprio GUID.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Adicione uma diretiva using para Windows.Foundation.Metadata a fim de colocar GuidAttribute em escopo.

Agora converta manualmente o GUID const em um GuidAttribute, de maneira que ele seja formatado conforme mostrado no exemplo a seguir. As chaves são substituídas por colchetes e parênteses, e o ponto-e-vírgula à direita é removido.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Repita essas etapas para a interface IToast.

Agora que as interfaces têm IDs exclusivas, podemos criar um arquivo IDL alimentando o arquivo .winmd na ferramenta de linha de comando winmdidl e gerar o código-fonte C do proxy e do stub alimentando esse arquivo IDL na ferramenta de linha de comando MIDL. O Visual Studio faz isso para caso criemos eventos de pós-compilação conforme mostrado nas etapas a seguir.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Para gerar o código-fonte de proxy e stub

Para adicionar um evento de pós-compilação personalizado, no Gerenciador de Soluções, abra o menu de atalho do projeto ToasterComponent e escolha Propriedades. No painel esquerdo das páginas de propriedades, selecione Compilar Eventos e escolha o botão Editar Pós-compilação. Adicione os seguintes comandos à linha de comando de pós-compilação. (O arquivo em lote deve ser chamado primeiro para definir as variáveis de ambiente para encontrar a ferramenta winmdidl.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Importante**  para uma configuração de projeto ARM ou x64, altere o parâmetro MIDL/Env para x64 ou arm32.

Para garantir que o arquivo IDL seja regenerado sempre que o arquivo .winmd for alterado, mude **Execute o evento pós-compilação** para **Quando a compilação atualiza a saída do projeto.**
A página de propriedades compilar eventos deve ser semelhante a esta: ![eventos de compilação](./images/buildevents.png)

Recompile a solução para gerar e compilar IDL.

É possível verificar que MIDL compilou corretamente a solução procurando ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c, e dlldata.c no diretório do projeto ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Para compilar o código de proxy e stub em uma DLL

Agora que tem os arquivos necessários, você pode compilá-los para produzir uma DLL, que é um arquivo em C++. Para que isso seja o mais fácil possível, adicione um novo projeto para dar suporte à compilação dos proxies. Abra o menu de atalho da solução ToasterApplication e escolha **Adicionar > Novo Projeto**. No painel esquerdo da caixa de diálogo **novo projeto** , expanda **Visual C++&gt;Windows&gt;Universal Windows**e, no painel central, selecione **dll (aplicativos UWP)** . (Observe que este NÃO é um projeto do componente do Tempo de Execução do Windows do C ++.) Nomeie o projeto Proxies e clique no botão **OK**. Esses arquivos serão atualizados pelos eventos de pós-compilação quando algo mudar na classe C#.

Por padrão, o projeto Proxies gera arquivos .h de cabeçalho e arquivos .cpp em C++. Como a DLL é compilada com base nos arquivos produzidos no MIDL, os arquivos .h e .cpp não são necessários. No Gerenciador de Soluções, abra o menu de atalho deles, escolha **Remover** e confirme a exclusão.

Agora que o projeto está vazio, é possível readicionar os arquivos gerados por MIDL. Abra o menu de atalho para o projeto Proxies e clique em **Adicionar > Item Existente.** Na caixa de diálogo, navegue até o diretório do projeto ToasterComponent e selecione estes arquivos: ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c e dlldata.c. Escolha o botão **Adicionar**.

No projeto Proxies, crie um arquivo .def para definir as exportações DLL descritas em dlldata.c. Abra o menu de atalho do projeto e escolha **Adicionar > Novo Item**. No painel à esquerda da caixa de diálogo, selecione Código e, no painel intermediário, selecione Arquivo de Definição de Módulo. Nomeie o arquivo proxies.def e escolha o botão **Adicionar**. Abra esse arquivo .def e o modifique para incluir as EXPORTS definidas em dlldata.c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Se você compilar o projeto agora, ele falhará. Para compilar corretamente o projeto, você precisa alterar a maneira como o projeto é compilado e vinculado. No Gerenciador de soluções, abra o menu de atalho para o projeto Proxies e selecione **Propriedades**. Mude as páginas de propriedades da seguinte forma.

No painel esquerdo, selecione **C/C++ > Pré-processador** e, em seguida, no painel direito, selecione **Definições de pré-processador**, escolha o botão de seta para baixo e selecione **Editar**. Adicione estas definições na caixa:

```cpp
WIN32;_WINDOWS
```
Em **C/C++ > Cabeçalhos Pré-compilados**, altere **Cabeçalho Pré-compilado** para **Não Usar Cabeçalhos Pré-Compilados** e escolha o botão **Aplicar**.

Em **Linker > Geral**, altere **Ignorar Biblioteca de Importação** para **Si**m e, em seguida, clique no botão **Aplicar**.

Em **Vinculador > Entrada**, selecione **Dependências Adicionais**, escolha o botão de seta para baixo e selecione **Editar**. Adicione este texto na caixa:

```cpp
rpcrt4.lib;runtimeobject.lib
```

Não cole essas bibliotecas diretamente na linha da lista. Use a caixa **Editar** para garantir que MSBuild no Visual Studio mantenha as dependências adicionais corretas.

Ao fazer essas alterações, escolha o botão **OK** na caixa de diálogo **Páginas de Propriedades**.

Em seguida, utilize uma dependência no projeto ToasterComponent. Isso garante que o Toaster será compilado antes da compilação do projeto de proxy. Isso é necessário porque o projeto Toaster é responsável por gerar os arquivos para compilar o proxy.

Abra o menu de atalho do projeto Proxies e escolha Dependências do Projeto. Marque as caixas de seleção para indicar que o projeto Proxies depende do projeto ToasterComponent, para garantir que o Visual Studio os compile na ordem correta.

Verifique se que a solução é compilada corretamente escolhendo **Compilar > Recompilar Solução** na barra de menus do Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Para registrar o proxy e o stub

No projeto ToasterApplication, abra o menu de atalho de package.appxmanifest e escolha **Abrir Com**. Na caixa de diálogo Abrir Com, selecione **Editor de texto XML** e, em seguida, clique no botão **OK**. Colemos em um XML que forneça um registro de extensão windows.activatableClass.proxyStub e que se baseie nos GUIDs no proxy. Para encontrar os GUIDs a serem usados no arquivo .appxmanifest, abra ToasterComponent_i.c. Encontre entradas semelhantes às do exemplo a seguir. Observe também que as definições de IToast, IToaster e uma terceiro interface – um manipulador de eventos tipado com dois parâmetros: Toaster e Toast. Isso corresponde ao evento definido na classe Toaster. Observe os GUIDs de IToast e IToaster correspondem aos GUIDs definidos nas interfaces no arquivo C#. Como a interface do manipulador de eventos tipados é gerada automaticamente, o GUID dessa interface também é gerado automaticamente.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Agora copiamos os GUIDs, os colamos em appxmanifest em um nó que adicionamos, damos o nome de Extensões e os reformatamos. A entrada de manifesto se parece com o exemplo a seguir – mas, novamente, lembre-se de usar os próprios GUIDs. O GUID ClassId no XML é o mesmo de ITypedEventHandler2. Isso porque esse GUID é o primeiro listado em ToasterComponent_i.c. Os GUIDs aqui diferenciam maiúsculas de minúsculas. Em vez de reformatar manualmente os GUIDs de IToast e IToaster, você pode voltar para as definições de interface e obter o valor GuidAttribute, que tem o formato correto. Em C++, existe um GUID formatado corretamente no comentário. Em qualquer caso, você deve reformatar manualmente o GUID usado para a ClassId e o manipulador de eventos.

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

Cole o nó XML de extensões como um filho direto do nó Pacote e um par de, por exemplo, o nó Recursos.

Antes de continuar, é importante garantir que:

-   O ProxyStub ClassID é definido como o primeiro GUID no arquivo ToasterComponent\_i. c. Use o primeiro GUID que está definido neste arquivo para o classId. (Ele pode ser o mesmo que o GUID de ITypedEventHandler2.)
-   Path é o caminho relativo do pacote do proxy binário. (Neste procedimento passo a passo, proxies.dll está na mesma pasta de ToasterApplication.winmd.)
-   Os GUIDs estão no formato correto. (É fácil errar.)
-   As IDs de interface no manifesto correspondem ao IIDs no arquivo ToasterComponent\_i. c.
-   Os nomes das interfaces são únicos no manifesto. Porque elas não são usadas pelo sistema, você pode escolher os valores. É uma boa prática escolher nomes de interface claramente correspondentes a interfaces que você definiu. Para interfaces geradas, os nomes devem ser indicativos das interfaces geradas. Você pode usar o arquivo ToasterComponent\_i. c para ajudá-lo a gerar nomes de interface.

Se você tentar executar a solução agora, você receberá um erro que proxies.dll não faz parte da carga útil. Abra o menu de atalho da pasta **Referências** no projeto ToasterApplication e escolha **Adicionar Referência**. Marque a caixa de seleção próxima do projeto Proxies. Além disso, assegure-se de que a caixa de seleção próxima de ToasterComponent também esteja selecionada. Escolha o botão **OK**.

O projeto agora deve ser compilado. Execute o projeto e verifique se você pode fazer torradas.

## <a name="related-topics"></a>Tópicos relacionados

* [Componentes do Windows Runtime com C++/CX](creating-windows-runtime-components-in-cpp.md)
