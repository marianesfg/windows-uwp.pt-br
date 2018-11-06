---
author: TylerMSFT
title: Crie um aplicativo universal do Windows de várias instâncias
description: Este tópico descreve como escrever aplicativos UWP que dão suporte a várias instâncias.
keywords: uwp de várias instâncias
ms.author: twhitney
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c70d696c1211cfa4f929178f0cf0d9da76ae74c2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6041064"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Crie um aplicativo universal do Windows de várias instâncias

Este tópico descreve como criar aplicativos da Plataforma Universal do Windows (UWP) de várias instâncias.

Do Windows 10, versão 1803 (10.0; Build 17134) em diante, seu aplicativo UWP pode aceitar para dar suporte a várias instâncias. Se uma instância de um aplicativo UWP de várias instâncias estiver em execução, e uma solicitação de ativação subsequente chegar, a plataforma não ativará a instância existente. Em vez disso, ela criará uma nova instância, executada em um processo separado.

> [!IMPORTANT]
> Várias instâncias é compatível para aplicativos de JavaScript, mas não é de redirecionamento de várias instâncias. Como não há suporte para o redirecionamento de várias instâncias para aplicativos de JavaScript, a classe [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) não é útil para esses aplicativos.

## <a name="opt-in-to-multi-instance-behavior"></a>Aceitar o comportamento de várias instâncias

Se você estiver criando um novo aplicativo de várias instâncias, você pode instalar o **Templates.VSIX de projeto de aplicativo de várias instâncias**, disponível no [Visual Studio Marketplace ](https://aka.ms/E2nzbv). Depois de instalar os modelos, eles estarão disponíveis na caixa de diálogo **Novo Projeto** em **Visual C# > Universal do Windows** (ou **Outras linguagens > Visual C++ > Universal do Windows**).

Dois modelos instalados: **aplicativo UWP de várias instâncias**, que fornece o modelo para a criação de um aplicativo de várias instâncias, e **aplicativo UWP de redirecionamento de várias instâncias**, que fornece a lógica adicional na qual você pode se basear para iniciar uma nova instância ou seletivamente ativar uma instância que já foi iniciada. Por exemplo, talvez você deseja apenas depois que a instância de cada vez editando o mesmo documento, para que você colocar a instância que tiver esse arquivo abrir em primeiro plano em vez de iniciar uma nova instância.

Ambos os modelos adicionam `SupportsMultipleInstances` para o `package.appxmanifest` arquivo. Observe o prefixo do namespace `desktop4` e `iot2`: apenas os projetos direcionados a área de trabalho ou projetos de Internet das coisas (IoT), suportam várias instâncias.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>Redirecionamento de ativação de várias instâncias

 Várias instâncias de suporte para aplicativos UWP vai além de simplesmente possibilitando a iniciar várias instâncias do aplicativo. Ele permite a personalização em casos em que você deseja selecionar se uma nova instância do seu aplicativo for iniciada ou uma instância que já está em execução é ativada. Por exemplo, se o aplicativo é iniciado para editar um arquivo que já está sendo editado em outra instância, convém redirecionar a ativação para essa instância em vez de abrir outra instância que já está editando o arquivo.

Para vê-lo em ação, assista a este vídeo sobre como criar aplicativos UWP de várias instâncias.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

O modelo de **aplicativo UWP de redirecionamento de várias instâncias** adiciona `SupportsMultipleInstances` ao arquivo package.appxmanifest, como mostrado acima, e também adiciona um **Program.cs** (ou **Program.cpp**, se você estiver usando a versão C++ do modelo) ao seu projeto que contém uma função `Main()`. Vai para a lógica para redirecionar a ativação da função `Main` . O modelo para **Program.cs** é mostrado abaixo.

A propriedade [**AppInstance.RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) representa a instância fornecida pelo shell preferencial para essa solicitação de ativação, se houver uma (ou `null` se houver um). Se o shell fornece uma preferência, em seguida, você pode redirecionar a ativação para essa instância ou você pode ignorá-lo se você escolher.

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` é a primeira coisa que é executado. Ele é executado antes de [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) e [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Isso permite que você determinar se deve ativar isso ou outra instância, antes de qualquer outro código de inicialização em seu aplicativo é executado.

O código acima determina se um existente ou nova instância do seu aplicativo for ativada. Uma chave é usada para determinar se há uma instância existente que você deseja ativar. Por exemplo, se seu aplicativo pode ser iniciado em [manipular a ativação de arquivo](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/handle-file-activation), você pode usar o nome do arquivo como uma chave. Em seguida, você pode verificar se uma instância do seu aplicativo já está registrada com essa chave e ativá-lo em vez de abrir uma nova instância. Esta é a ideia por detrás do código: `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Se uma instância registrada com a chave for encontrada, essa instância é ativada. Se a chave não for encontrado, em seguida, a instância atual (a instância que está sendo executado `Main`) cria seu objeto de aplicativo e começa a ser executado.

## <a name="background-tasks-and-multi-instancing"></a>Tarefas em segundo plano agora podem ter várias instâncias

- Tarefas em segundo plano fora do processo suportam a várias instâncias. Normalmente, cada novo disparador resulta em uma nova instância da tarefa em segundo plano (embora tecnicamente falando várias tarefas em segundo plano pode ser executado no mesmo processo de host). No entanto, é criada uma instância da tarefa em segundo plano de fundo diferente.
- Tarefas em segundo plano fora do processo não suportam várias instâncias.
- Tarefas de áudio em segundo plano fora do processo não suportam várias instâncias.
- Quando um aplicativo registra uma tarefa em segundo plano, ele geralmente primeiro verifica se a tarefa já está registrada e, em seguida, exclui e registra-la novamente ou não faz nada para manter o registro existente. Isso ainda é o comportamento típico com aplicativos de várias instâncias. No entanto, um aplicativo de várias instâncias pode optar por registrar um nome de tarefa de plano de fundo diferente em uma base por instância. Isso resultará em vários registros para o gatilho mesmo e várias instâncias de tarefa em segundo plano serão ativadas quando o gatilho é acionado.
- Serviços de aplicativo inicie uma instância separada da tarefa em segundo plano de serviço de aplicativo para cada conexão. Isso permanece inalterado para aplicativos de várias instâncias, que é a que cada instância de um aplicativo de várias instâncias obterão sua própria instância da tarefa em segundo plano de serviço de aplicativo. 

## <a name="additional-considerations"></a>Considerações adicionais

- Várias instâncias é compatível com aplicativos UWP destinados a área de trabalho e projetos da Internet das Coisas (IoT).
- Para evitar condições de corrida e problemas de contenção, os aplicativos de várias instâncias precisam tomar medidas para particionar/sincronizar o acesso às configurações, armazenamento local do aplicativo e qualquer outro recurso (como arquivos do usuário, um repositório de dados e assim por diante) que pode ser compartilhado entre várias instâncias. Mecanismos de sincronização padrão, como exclusões mútuas, semáforos, eventos e assim por diante, estão disponíveis.
- Se o aplicativo tiver `SupportsMultipleInstances`em seu arquivo Package. appxmanifest, em seguida, suas extensões não precisa declarar `SupportsMultipleInstances`. 
- Se você adicionar `SupportsMultipleInstances`para qualquer outra extensão, além de plano de fundo tarefas ou serviços de aplicativo e o aplicativo que hospeda a extensão não também declara `SupportsMultipleInstances`em seu arquivo Package. appxmanifest, um erro de esquema é gerado.
- Aplicativos podem usar a declaração [**ResourceGroup**](https://docs.microsoft.com/windows/uwp/launch-resume/declare-background-tasks-in-the-application-manifest) em seu manifesto para agrupar várias tarefas em segundo plano para o mesmo host. Entra em conflito com várias instâncias, onde cada ativação entra em um host separado. Portanto, um aplicativo não pode declarar as duas `SupportsMultipleInstances`e `ResourceGroup` em seu manifesto.

## <a name="sample"></a>Exemplo

Consulte o [exemplo de várias instâncias](https://aka.ms/Kcrqst) para obter um exemplo de redirecionamento de ativação de várias instâncias.

## <a name="see-also"></a>Veja também

[AppInstance.FindOrRegisterInstanceForKey](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_)
[AppInstance.GetActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs)
[AppInstance.RedirectActivationTo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo)
[manipular ativação de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/activate-an-app)