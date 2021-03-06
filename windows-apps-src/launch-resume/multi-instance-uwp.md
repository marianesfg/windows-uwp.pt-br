---
title: Crie um aplicativo universal do Windows de várias instâncias
description: Este tópico descreve como escrever aplicativos UWP que dão suporte a várias instâncias.
keywords: uwp de várias instâncias
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cdb8d87a63eba14ecb2dc25e3cb5451dce6cae60
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684644"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Crie um aplicativo universal do Windows de várias instâncias

Este tópico descreve como criar aplicativos da Plataforma Universal do Windows (UWP) de várias instâncias.

Do Windows 10, versão 1803 (10,0; Build 17134) em diante, seu aplicativo UWP pode optar por oferecer suporte a várias instâncias. Se uma instância de um aplicativo UWP de várias instâncias estiver em execução e uma solicitação de ativação subsequente chegar, a plataforma não ativará a instância existente. Em vez disso, ela criará uma instância, executada em um processo separado.

> [!IMPORTANT]
> Há suporte para várias instâncias em aplicativos JavaScript, mas o redirecionamento de várias instâncias não é. Como o redirecionamento de várias instâncias não tem suporte para aplicativos JavaScript, a classe [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) não é útil para tais aplicativos.

## <a name="opt-in-to-multi-instance-behavior"></a>Aceitar o comportamento de várias instâncias

Se você estiver criando um novo aplicativo de várias instâncias, você pode instalar o [Templates.VSIX de projeto de aplicativo de várias instâncias](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.MultiInstanceApps), disponível no [Visual Studio Marketplace ](https://marketplace.visualstudio.com/). Depois de instalar os modelos, eles estarão disponíveis na caixa de diálogo **Novo Projeto** em **Visual C# > Universal do Windows** (ou **Outras linguagens > Visual C++ > Universal do Windows**).

Dois modelos instalados: **aplicativo UWP de várias instâncias**, que fornece o modelo para a criação de um aplicativo de várias instâncias, e **aplicativo UWP de redirecionamento de várias instâncias**, que fornece a lógica adicional na qual você pode se basear para iniciar uma nova instância ou seletivamente ativar uma instância que já foi iniciada. Por exemplo, talvez você queira apenas uma instância por vez editando o mesmo documento, de modo que você traga a instância que tem esse arquivo aberto para o primeiro plano em vez de iniciar uma nova instância.

Ambos os modelos adicionam `SupportsMultipleInstances` ao arquivo de `package.appxmanifest`. Observe o prefixo do namespace `desktop4` e `iot2`: somente projetos direcionados aos projetos da área de trabalho ou Internet das Coisas (IoT) oferecem suporte a várias instâncias.

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

Para vê-lo em ação, Assista a este vídeo sobre como criar aplicativos UWP de várias instâncias.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

O modelo de **aplicativo UWP de redirecionamento de várias instâncias** adiciona `SupportsMultipleInstances` ao arquivo package.appxmanifest, como mostrado acima, e também adiciona um **Program.cs** (ou **Program.cpp**, se você estiver usando a versão C++ do modelo) ao seu projeto que contém uma função `Main()`. Vai para a lógica para redirecionar a ativação da função `Main` . O modelo para **Program.cs** é mostrado abaixo.

A propriedade [**AppInstance. RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) representa a instância preferencial fornecida pelo shell para essa solicitação de ativação, se houver uma (ou `null` se não houver uma). Se o Shell fornecer uma preferência, você poderá redirecionar a ativação para essa instância ou poderá ignorá-la se escolher.

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

`Main()` é a primeira coisa que é executada. Ele é executado antes de [**onlaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_)  e [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Isso permite que você determinar se deve ativar isso ou outra instância, antes de qualquer outro código de inicialização em seu aplicativo é executado.

O código acima determina se um existente ou nova instância do seu aplicativo for ativada. Uma chave é usada para determinar se há uma instância existente que você deseja ativar. Por exemplo, se seu aplicativo pode ser iniciado em [manipular a ativação de arquivo](https://docs.microsoft.com/windows/uwp/launch-resume/handle-file-activation), você pode usar o nome do arquivo como uma chave. Em seguida, você pode verificar se uma instância do seu aplicativo já está registrada com essa chave e ativá-lo em vez de abrir uma nova instância. Essa é a ideia por trás do código: `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Se uma instância registrada com a chave for encontrada, essa instância é ativada. Se a chave não for encontrado, em seguida, a instância atual (a instância que está sendo executado `Main`) cria seu objeto de aplicativo e começa a ser executado.

## <a name="background-tasks-and-multi-instancing"></a>Tarefas em segundo plano agora podem ter várias instâncias

- Tarefas em segundo plano fora do processo suportam a várias instâncias. Normalmente, cada novo disparador resulta em uma nova instância da tarefa em segundo plano (embora tecnicamente falando várias tarefas em segundo plano pode ser executado no mesmo processo de host). No entanto, é criada uma instância da tarefa em segundo plano de fundo diferente.
- Tarefas em segundo plano fora do processo não suportam várias instâncias.
- Tarefas de áudio em segundo plano fora do processo não suportam várias instâncias.
- Quando um aplicativo registra uma tarefa em segundo plano, ele geralmente primeiro verifica se a tarefa já está registrada e, em seguida, exclui e registra-la novamente ou não faz nada para manter o registro existente. Isso ainda é o comportamento típico com aplicativos de várias instâncias. No entanto, um aplicativo de várias instâncias pode optar por registrar um nome de tarefa de plano de fundo diferente em uma base por instância. Isso resultará em vários registros para o gatilho mesmo e várias instâncias de tarefa em segundo plano serão ativadas quando o gatilho é acionado.
- Serviços de aplicativo inicie uma instância separada da tarefa em segundo plano de serviço de aplicativo para cada conexão. Isso permanece inalterado para aplicativos de várias instâncias, que é a que cada instância de um aplicativo de várias instâncias obterão sua própria instância da tarefa em segundo plano de serviço de aplicativo. 

## <a name="additional-considerations"></a>Considerações adicionais

- Várias instâncias é compatível com aplicativos UWP destinados a área de trabalho e projetos da Internet das Coisas (IoT).
- Para evitar condições de corrida e problemas de contenção, os aplicativos de várias instâncias precisam tomar medidas para particionar/sincronizar o acesso às configurações, armazenamento local do aplicativo e qualquer outro recurso (como arquivos do usuário, um repositório de dados e assim por diante) que pode ser compartilhado entre várias instâncias. Mecanismos de sincronização padrão, como mutexes, semáforos, eventos e assim por diante, estão disponíveis.
- Se o aplicativo tiver `SupportsMultipleInstances`em seu arquivo Package. appxmanifest, em seguida, suas extensões não precisa declarar `SupportsMultipleInstances`. 
- Se você adicionar `SupportsMultipleInstances`para qualquer outra extensão, além de plano de fundo tarefas ou serviços de aplicativo e o aplicativo que hospeda a extensão não também declara `SupportsMultipleInstances`em seu arquivo Package. appxmanifest, um erro de esquema é gerado.
- Os aplicativos podem usar a Declaração [**resourcegroup**](https://docs.microsoft.com/windows/uwp/launch-resume/declare-background-tasks-in-the-application-manifest) em seu manifesto para agrupar várias tarefas em segundo plano no mesmo host. Entra em conflito com várias instâncias, onde cada ativação entra em um host separado. Portanto, um aplicativo não pode declarar as duas `SupportsMultipleInstances`e `ResourceGroup` em seu manifesto.

## <a name="sample"></a>Exemplo

Consulte [exemplo de várias instâncias](https://github.com/Microsoft/AppModelSamples/tree/master/Samples/BananaEdit) para obter um exemplo de redirecionamento de ativação de várias instâncias.

## <a name="see-also"></a>Veja também

[AppInstance.FindOrRegisterInstanceForKey](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_)
[AppInstance.GetActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs)
[AppInstance.RedirectActivationTo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo)
[manipular ativação de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/activate-an-app)
