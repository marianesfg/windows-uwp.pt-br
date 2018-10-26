---
author: TylerMSFT
title: Criar e consumir uma extensão de app
description: Extensões do aplicativo Gravar e hospedar Plataforma Universal do Windows (UWP) que permitam que você estenda seu aplicativo por meio de pacotes que os usuários podem instalar a partir da Microsoft Store.
keywords: extensão de aplicativo, serviço de aplicativo, segundo plano
ms.author: twhitney
ms.date: 10/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a1722c22c717ec1a349f6e7d48c1e151209eab2c
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5642139"
---
# <a name="create-and-host-an-app-extension"></a>Criar e armazenar uma extensão de app

Este artigo mostra como criar uma extensão de aplicativo UWP e hospedá-la em um aplicativo UWP.

Este artigo é acompanhado por um exemplo de código:
- Baixe e descompacte [Exemplo de código de extensão matemática](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip).
- No Visual Studio 2017, abra MathExtensionSample.sln. Defina o tipo de compilação para x86 (**Compilação** > **Gerente de configuração**, depois mude **Plataforma** para **x86** para ambos projetos).
- Implante a solução: **Compilar** > **Implantar solução**.

## <a name="introduction-to-app-extensions"></a>Introdução a extensões de aplicativo

Na Plataforma Universal do Windows (UWP), as extensões de aplicativo fornecem funcionalidades semelhante às funções de plug-ins, suplementos e complementos em outras plataformas. Por exemplo, extensões do Microsoft Edge são extensões de aplicativos UWP. Extensões de aplicativos UWP foram introduzidas na edição de aniversário do Windows 10 (versão 1607, compilação 10.0.14393).

Extensões de aplicativos UWP são aplicativos UWP que têm uma declaração de extensão que permite que eles compartilhem os eventos de conteúdo e implantação com um aplicativo de hospedagem. Um aplicativo de extensão pode fornecer várias extensões.

Como extensões de aplicativos são apenas aplicativos UWP, eles também podem ser aplicativos totalmente funcionais, e fornecerem extensões do host para outros aplicativos — tudo isso sem a criação de pacotes de aplicativo separados.

Quando você cria um host de extensão do aplicativo, você cria uma oportunidade de desenvolver um ecossistema em torno de seu aplicativo em que outros desenvolvedores podem melhorar seu aplicativo de maneiras de que você possa não esperado ou que não tinha os recursos para. Considere a possibilidade de extensões do Microsoft Office, extensões do Visual Studio, extensões do navegador, etc. Esses modelos criam experiências mais ricas para os aplicativos que vão além da funcionalidade que o acompanham. Extensões podem adicionar valor e longevidade ao seu aplicativo.

**Visão geral**

Em um nível alto, para configurar uma relação de extensão do aplicativo, nós precisamos:

1. Declarar um aplicativo para ser um host de extensão.
2. Declare um aplicativo para ser uma extensão.
3. Decida se deseja implementar a extensão como um serviço de aplicativo, tarefa em segundo plano ou alguma outra forma.
4. Defina como os hosts e suas extensões se comunicarão.
5. Use o API do [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions) no aplicativo host para acessar as extensões.

Vamos ver como isso é feito examinando o [Exemplo de código de extensão matemática](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) que implementa uma calculadora hipotética onde você pode adicionar novas funções para usar extensões. No Microsoft Visual Studio 2017, carregue **MathExtensionSample.sln** da amostra de código.

![Exemplo de código de extensão de matemática](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Declarar um aplicativo para ser um host de extensão

Um aplicativo se identifica como um host de extensão do aplicativo declarando o elemento `<AppExtensionHost>` em seu arquivo Package.appxmanifest. Consulte o arquivo **Package.appxmanifest** no projeto **MathExtensionHost** para ver como isso é feito.

_Package.appxmanifest no projeto MathExtensionHost_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>microsoft.com.MathExt</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Observe o `xmlns:uap3="http://..."` e a presença de `uap3` em `IgnorableNamespaces`. Elas são necessárias porque estamos usando o namespace uap3.

`<uap3:Extension Category="windows.appExtensionHost">` Identifica esse aplicativo como um host de extensão.

O elemento do **Nome** em `<uap3:AppExtensionHost>` é o nome do _contrato de extensão_. Quando uma extensão especifica o mesmo nome de contrato de extensão, o host poderá encontrá-la. Por convenção, é recomendável criar o nome do contrato de extensão usando seu aplicativo ou o nome do fornecedor para evitar possíveis conflitos com outros nomes de contrato de extensão.

Você pode definir vários hosts e várias extensões no mesmo aplicativo. Neste exemplo, nós declaramos um host. A extensão é definida em outro aplicativo.

## <a name="declare-an-app-to-be-an-extension"></a>Declare um aplicativo para ser uma extensão.

Um aplicativo se identifica como uma extensão do aplicativo declarando o elemento `<uap3:AppExtension>` em seu arquivo **Package.appxmanifest**. Abra o arquivo **Package.appxmanifest** no projeto **MathExtension** para ver como isso é feito.

_Package. appxmanifest no projeto MathExtension:_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="Microsoft.com.MathExt"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Novamente, observe a linha `xmlns:uap3="http://..."` e a presença de `uap3` em `IgnorableNamespaces`. Estes são necessários porque estamos usando o namespace `uap3`.

`<uap3:Extension Category="windows.appExtension">` identifica esse aplicativo como uma extensão.

O significado dos atributos `<uap3:AppExtension>` é o seguinte:

|Atributo|Descrição|Necessário|
|---------|-----------|:------:|
|**Nome**|Este é o nome do contrato de extensão. Quando ele corresponde ao **Nome** declarado em um host, esse host será capaz de encontrar essa extensão.| :heavy_check_mark: |
|**ID**| Identifica esse aplicativo como uma extensão. Como pode haver várias extensões que usam o mesmo nome de contrato de extensão (imagine um aplicativo de pintura que dá suporte a várias extensões), você pode usar a ID para diferenciá-los. Os hosts de extensão do aplicativo podem usar a ID para inferir algo sobre o tipo de extensão. Por exemplo, você pode ter uma extensão projetada para a área de trabalho e outra para dispositivos móveis, com a ID sendo o diferencial. Para isso, você também pode usar o elemento **Propriedades**, discutido abaixo.| :heavy_check_mark: |
|**DisplayName**| Pode ser usado em seu próprio aplicativo host para identificar a extensão para o usuário. Ele é consultável, e pode usar o [novo sistema de gerenciamento de recurso](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) para localização. O conteúdo localizado é carregado do pacote de extensão do aplicativo, não do aplicativo host. | |
|**Descrição** | Pode ser usado em seu próprio aplicativo host para descrever a extensão para o usuário. Ele é consultável, e pode usar o [novo sistema de gerenciamento de recurso](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) para localização. O conteúdo localizado é carregado do pacote de extensão do aplicativo, não do aplicativo host. | |
|**PublicFolder**|O nome de uma pasta, relativo à raiz do pacote, onde você pode compartilhar conteúdo com o host de extensão. Por convenção, o nome é "Public", mas você pode usar qualquer nome que corresponde a uma pasta na sua extensão.| :heavy_check_mark: |

`<uap3:Properties>` é um elemento opcional que contém metadados personalizadas onde os hosts podem ler no tempo de execução. No exemplo de código, a extensão é implementada como um serviço de aplicativo, assim o host precisa de uma maneira de obter o nome desse serviço de aplicativo para que ele possa chamá-lo. O nome do serviço de aplicativo é definido no elemento <Service>, que definimos (nós poderíamos ter chamado de qualquer nome que gostaríamos). O host no exemplo de código procura por essa propriedade em tempo de execução para saber o nome do serviço de aplicativo.

## <a name="decide-how-you-will-implement-the-extension"></a>Decida como implementar a extensão.

A [Sessão da compilação 2016 sobre extensões de aplicativo](https://channel9.msdn.com/Events/Build/2016/B808) demonstra como usar a pasta pública que é compartilhada entre o host e as extensões. Neste exemplo, a extensão é implementada por um arquivo Javascript que é armazenado na pasta pública, que o host invoca. Essa abordagem tem a vantagem de ser leve, não exige a compilação e pode dar suporte para tornar a página de aterrissagem padrão, que fornece instruções para a extensão e um link para página de Microsoft Store do aplicativo host. Consulte o [Exemplo de código de extensão de aplicativo de compilação 2016](https://github.com/Microsoft/App-Extensibility-Sample) para obter detalhes. Especificamente, consulte o projeto **InvertImageExtension** e `InvokeLoad()` em ExtensionManager.cs no projeto **ExtensibilitySample**.

Neste exemplo, usaremos um serviço de aplicativo para implementar a extensão. Serviços de aplicativo têm as seguintes vantagens:

- Se a extensão travar, não derrubará o aplicativo host porque o aplicativo host é executado em seu próprio processo.
- Você pode usar a linguagem de sua preferência para implementar o serviço. Ela não precisa corresponder a linguagem usada para implementar o aplicativo host.
- O serviço de aplicativo tem acesso ao seu próprio contêiner de aplicativo – que pode ter várias funcionalidades diferentes das que host tem.
- Não há isolamento entre dados no serviço e o aplicativo host.

### <a name="host-app-service-code"></a>Código de serviço de aplicativo host

Aqui está o código de host que invoca o serviço de aplicativo da extensão:

_ExtensionManager.cs no projeto MathExtensionHost_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

Este é o código típico para chamar um serviço de aplicativo. Para obter detalhes sobre como implementar e chamar um serviço de aplicativo, consulte [Como criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md).

Uma coisa a observar é como o nome do serviço de aplicativo para chamar é determinado. Como o host não tem informações sobre a implementação da extensão, a extensão precisa fornecer o nome do seu serviço de aplicativo. No exemplo de código, a extensão declara o nome do serviço de aplicativo em seu arquivo no elemento `<uap3:Properties>`:

_Package.appxmanifest no projeto MathExtension_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

Você pode definir seu próprio XML no elemento `<uap3:Properties>`. Nesse caso, definimos o nome do serviço de aplicativo para que o host possa usá-lo quando ele invoca a extensão.

Quando o host carrega uma extensão, um código como este extrai o nome do serviço das propriedades definidas no Package.appxmanifest da extensão:

_`Update()` na ExtensionManager.cs, no projeto MathExtensionHost_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

Com o nome do serviço de aplicativo armazenado em `_serviceName`, o host pode usá-lo para invocar o serviço de aplicativo.

Chamar um serviço de aplicativo também exige o nome da família do pacote que contém o serviço de aplicativo. Felizmente, a API de extensão do aplicativo fornece essas informações que são obtidas na linha: `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Defina como o host e a extensão se comunicarão

Os serviços de aplicativo usam [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) para trocar informações. Como o autor do host, você precisa inventar um protocolo para comunicação com extensões que são flexíveis. No exemplo de código, isso significa que contabilidade para extensões pode levar 1, 2 ou mais argumentos no futuro.

Para este exemplo, o protocolo para os argumentos é um **ValueSet** que contém os pares de valor chave denominados 'Arg' + o número de argumento, por exemplo, `Arg1` e `Arg2`. O host passa em todos os argumentos no **ValueSet**, e a extensão faz uso daqueles que ela precisa. Se a extensão é capaz de calcular um resultado, o host espera que o **ValueSet** retornado da extensão tenha uma chave denominada `Result`, que contém o valor do cálculo. Se essa chave não estiver presente, o host presume que a extensão não pôde concluir o cálculo.

### <a name="extension-app-service-code"></a>Código de serviço do aplicativo de extensão

No exemplo de código, o serviço de aplicativo da extensão não é implementado como uma tarefa em segundo plano. Em vez disso, ele usa o modelo de serviço de aplicativo único de processo no qual o serviço de aplicativo é executado no mesmo processo que o aplicativo de extensão o hospeda. Isso ainda é um processo diferente do aplicativo host, oferecendo os benefícios da separação de processo, enquanto aumenta alguns benefícios de desempenho evitando a comunicação entre processos entre o processo de extensão e o processo em segundo plano que implementa o aplicativo do serviço. Consulte [Converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host](convert-app-service-in-process.md) para ver um serviço de aplicativo que executa como uma tarefa de plano de fundo comparado ao mesmo processo.

O sistema chega ao `OnBackgroundActivate()` quando o serviço de aplicativo é ativado. Esse código define manipuladores de eventos para lidar com a chamada de serviço de aplicativo real, quando se trata de (`OnAppServiceRequestReceived()`), bem como lidar com eventos de manutenção do sistema, como obter um objeto de adiamento manipulando um cancelamento ou evento fechado.

_App.xaml.cs no projeto MathExtension._
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

O código que faz o trabalho da extensão está em `OnAppServiceRequestReceived()`. Essa função é chamada quando o serviço de aplicativo é invocado para executar um cálculo. Ela extrai os valores que precisa do **ValueSet**. Se ele puder fazer o cálculo, colocará o resultado, sob uma chave denominada **Resultado**, no **ValueSet** que é retornado para o host. Lembre-se de que, de acordo com o protocolo definido para esse host e suas extensões irão se comunicar, a presença de uma chave de **Resultado** indicará êxito; caso contrário, falha.

_App.xaml.cs no projeto MathExtension._
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>Gerenciar extensões

Agora que vimos como implementar a relação entre um host e suas extensões, vamos ver como um host encontra extensões instaladas no sistema e reage a adição e remoção de pacotes que contêm extensões.

A Microsoft Store entrega as extensões como pacotes. O [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) encontra pacotes instalados que contêm o extensões correspondentes ao nome de contrato da extensão do host, e fornece eventos que são acionados quando um pacote de extensão do aplicativo relevante para o host é instalado ou removido.

No exemplo de código, a classe `ExtensionManager` (definida em **ExtensionManager.cs** no projeto **MathExtensionHost**) ajusta a lógica para extensões de carregamento e responde ao pacote de extensão instalado e desinstalado.

O construtor `ExtensionManager` usa o `AppExtensionCatalog` para encontrar as extensões de aplicativo no sistema que têm o mesmo nome de contrato de extensão que o host:

_ExtensionManager.cs no projeto MathExtensionHost._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

Quando um pacote de extensão é instalado, o `ExtensionManager` reúne informações sobre as extensões no pacote que têm o mesmo nome de contrato de extensão que o host. Uma instalação pode representar uma atualização no caso em que as informações da extensão afetadas são atualizadas. Quando um pacote de extensão é desinstalado, o `ExtensionManager` remove as informações sobre as extensões afetadas para que o usuário saiba quais extensões não estão mais disponíveis.

A classe `Extension` (definida em **ExtensionManager.cs** no projeto **MathExtensionHost**) foi criada para o exemplo de código para acessar ID, descrição, logotipo e informações específicas de aplicativo de uma extensão, como se o usuário habilitou a extensão.

Para dizer que a extensão está carregada (veja `Load()` em **ExtensionManager.cs**) significa que o status do pacote é fino e que obtivemos sua ID, logo, descrição e pasta pública (que não usamos essa amostra apenas para mostrar como obtê-lo). O pacote de extensão em si não está sendo carregado.

O conceito de descarregamento é usado para controlar quais extensões não devem ser apresentadas ao usuário.

O `ExtensionManager` fornece uma coleção de instâncias `Extension` para que as extensões, seus nomes, descrições e logotipos possam ser vinculados à interface do usuário. A página **ExtensionsTab** se associa a essa coleção e fornece a interface do usuário para ativar/desativar extensões, bem como removê-las.

![Interface do usuário de exemplo da guia Extensões](images/mathextensionhost-extensiontab.png)

 Quando uma extensão é removida, o sistema solicita que o usuário confirme que deseja desinstalar o pacote que contém a extensão (e possivelmente contém outras extensões). Se o usuário concordar, a desinstalação do pacote e de `ExtensionManager` remove as extensões no pacote desinstalado na lista de extensões disponíveis para o aplicativo host.

 ![Desinstalar IU](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Extensões de aplicativos e hosts de depuração

Muitas vezes, o host de extensão e a extensão não fazem parte da mesma solução. Nesse caso, para depurar o host e a extensão:

1. Carregue seu projeto de host em uma instância do Visual Studio.
2. Carregue sua extensão em outra instância do Visual Studio.
3. Inicie seu aplicativo de host no depurador.
4. Inicie o aplicativo da extensão no depurador. (Se você deseja implantar a extensão, em vez de depurá-la para testar o evento de instalação do pacote do host, realize **Compilar &gt; Implantar solução**, em vez disso).

Agora você poderá atingir pontos de interrupção do host e a extensão.
Se você iniciar a depuração do próprio aplicativo de extensão, você verá uma janela em branco para o aplicativo. Se você não quiser ver a janela em branco, você pode alterar as configurações de depuração para o projeto de extensão para não iniciar o aplicativo, mas em vez disso, depurá-lo quando ele for iniciado (clique com botão direito do projeto de extensão, **Propriedades** > **Depurar**> selecione **Mão iniciar, mas sim depurar meu código quando ele é iniciado**), mas ainda será necessário iniciar depuração (**F5**) do projeto de extensão, mas ele esperará até que o host ative a extensão e, em seguida, os pontos de interrupção no extensão sejam atingidos.

**Depurar a amostra de código**

No exemplo de código, o host e a extensão estão na mesma solução. Faça o seguinte para depurar:

1. Certifique-se de que **MathExtensionHost** é o projeto de inicialização (clique com botão direito no projeto **MathExtensionHost**, clique em **Definir como projeto de inicialização**).
2. Coloque um ponto de interrupção no `Invoke` em ExtensionManager.cs, no projeto **MathExtensionHost**.
3. **F5** para executar o projeto **MathExtensionHost**.
4. Coloque um ponto de interrupção no `OnAppServiceRequestReceived` em App.xaml, no projeto **MathExtension**.
5. Inicie a depuração do projeto **MathExtension** (clique com botão direito no projeto **MathExtension**, **Depurar > Iniciar nova instância**) que implantará e disparará o evento de instalação do pacote no host.
6. No aplicativo **MathExtensionHost**, navegue até a página **Cálculo** e clique em **x^y** para ativar a extensão. O ponto de interrupção `Invoke()` é atingido pela primeira vez e você pode ver o serviço de aplicativo das extensões de chamada feitas. O método `OnAppServiceRequestReceived()` na extensão é atingido e você pode ver o serviço de aplicativo calcular o resultado e retorná-lo.

**Solucionando problemas de extensões implementadas como um serviço de aplicativo**

Se o host de extensão tiver problemas para se conectar ao serviço de aplicativo para sua extensão, certifique-se de que o atributo `<uap:AppService Name="...">` corresponde ao que você colocou em seu elemento `<Service>`. Se eles não corresponderem, o nome do serviço que sua extensão fornece o host não corresponde ao nome do serviço de aplicativo que você implementou e o host não poderá ativar sua extensão.

_Package. appxmanifest no projeto MathExtension:_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="Microsoft.com.MathExt" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Uma lista de verificação de cenários básicos para testar

Quando você compila um host de extensão e está pronto para testar o quanto ele dá suporte a extensões, aqui estão alguns cenários básicos para tentar:

- Execute o host e, em seguida, implante um aplicativo de extensão  
    - O host detecta novas extensões que surgir durante sua execução?  
- Implantar um aplicativo de extensão e depois implantar e executar o host.
    - O host atende às extensões existentes anteriormente?  
- Execute o host e, em seguida, remova o aplicativo de extensão.
    - O host detecta a remoção corretamente?
- Execute o host e, em seguida, atualize o aplicativo de extensão para uma versão mais recente.
    - O host recomeça a mudança e descarrega as versões antigas da extensão corretamente?  

**Cenários avançados para testar:**

- Execute o host, mova o aplicativo de extensão para mídia removível, remova a mídia
    - O host detecta a alteração no status do pacote e desabilita a extensão?
- Executa o host e, em seguida, corrompe o aplicativo de extensão (torna o inválido, atribuído de maneira diferente, etc.)
    - O host detecta a extensão violada e a manipula corretamente?
- Executa o host e, em seguida, implanta um aplicativo de extensão que tem conteúdo ou propriedades inválido
    - O host detecta o conteúdo inválido e o manipula corretamente?

## <a name="design-considerations"></a>Considerações de design

- Forneça a interface do usuário que mostra ao usuário quais extensões estão disponíveis e permite que eles o habilitem/desabilitem. Você também pode considerar a adição de glifos de extensões que não ficam disponíveis como um pacote que fica offline, etc.
- Encaminha o usuário para onde ele pode baixar extensões. Talvez a sua página de extensão possa fornecer uma consulta de pesquisa do Microsoft Store que exibe uma lista de extensões que podem ser usadas com seu aplicativo.
- Considere como notificar o usuário sobre a adição e remoção de extensões. Você pode criar uma notificação quando uma nova extensão está instalada e convidar o usuário para habilitá-la. Extensões devem ser desabilitadas por padrão para que os usuários estejam no controle.

## <a name="how-app-extensions-differ-from-optional-packages"></a>Como extensões de aplicativos diferem dos pacotes opcionais

O diferencial entre [pacotes opcionais](https://docs.microsoft.com/windows/uwp/packaging/optional-packages) e extensões de aplicativos é o ecossistema aberto versus ecossistema fechado e pacote dependente versus pacote independente.

Extensões de aplicativo participam de um ecossistema aberto. Se seu aplicativo puder hospedar extensões de aplicativo, qualquer pessoa pode gravar uma extensão para seu host, desde que elas cumpram seu método de passar/receber informações da extensão. Isso difere pacotes opcionais, que participam de um ecossistema fechado, onde o fornecedor decide quem tem permissão para fazer um pacote opcional que pode ser usado com o aplicativo.

Extensões de aplicativos são pacotes independentes e podem ser aplicativos autônomos. Elas não têm uma dependência de implantação em outro aplicativo.Pacotes opcionais exigem o pacote principal e não podem ser executados sem ele.

Um pacote de expansão para um jogo seria um bom candidato para um pacote opcional, pois ele está estreitamente ligado ao jogo, ele não será executado independentemente do jogo e talvez não queira que os pacotes de expansão sejam criados por qualquer desenvolvedor no ecossistema.

Se o mesmo jogo tiver complementos personalizáveis de interface do usuário ou temas, uma extensão de aplicativo pode ser uma boa opção porque o aplicativo fornece a extensão que pode ser executada em seu próprio, e qualquer terceiro pode criá-los.

## <a name="remarks"></a>Comentários

Este tópico fornece uma introdução às extensões de aplicativo. Os principais aspectos a serem observados são a criação do host e marcá-lo como tal em seu arquivo Package. appxmanifest, criando a extensão e marcando-a como tal em seu arquivo Package. appxmanifest, determinando como implementar a extensão (como um serviço de aplicativo em segundo plano de tarefa ou outros meios), definir como o host se comunicará com extensões e usando o [AppExtensions API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) para acessar e gerenciar extensões.

## <a name="related-topics"></a>Tópicos relacionados

* [Introdução às extensões de aplicativo](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [Sessão da compilação 2016 sobre extensões de aplicativo](https://channel9.msdn.com/Events/Build/2016/B808)
* [Exemplo de código de extensão de aplicativo de compilação 2016](https://github.com/Microsoft/App-Extensibility-Sample)
* [Torne seu aplicativo compatível com tarefas em segundo plano](support-your-app-with-background-tasks.md)
* [Como criar e consumir um serviço de aplicativo](how-to-create-and-consume-an-app-service.md).
* [Namespace de AppExtensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [Estender seu aplicativo com serviços de aplicativo, extensões e pacotes](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)
