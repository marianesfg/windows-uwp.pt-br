---
author: TylerMSFT
title: Continue a atividade do usuário, mesmo entre dispositivos
description: Este tópico descreve como ajudar os usuários a retomar o que eles estavam fazendo no seu aplicativo, mesmo em vários dispositivos.
keywords: atividade do usuário, atividades do usuário, linha do tempo, cortana retorma de onde você parou, cortana retoma de onde eu parei, project rome
ms.author: twhitney
ms.date: 04/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 53aac2375d60df3cd9493f315b20431961378fe8
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3390362"
---
# <a name="continue-user-activity-even-across-devices"></a>Continue a atividade do usuário, mesmo entre dispositivos

Este tópico descreve como ajudar os usuários a retomar o que eles estavam fazendo no seu aplicativo no PC e em vários dispositivos.

## <a name="user-activities-and-timeline"></a>Atividades do usuário e linha do tempo

Nosso tempo todo dia é distribuído em vários dispositivos. Nós podemos usar nosso telefone enquanto estiver no ônibus, um computador durante o dia, em seguida, um telefone ou tablet durante a noite. A partir do Windows 10 compilação 1803 ou superior, criar uma [Atividade do usuário](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) faz com que essa atividade apareça na linha do tempo do Windows e no recurso Continue de onde você parou da Cortana. Linha do tempo é um modo de exibição de tarefa avançada que tira proveito das atividades do usuário para mostrar uma exibição cronológica nas quais você tem trabalhado. Ele também pode incluir o que você estava trabalhando em todos os dispositivos.

![Imagem de linha do tempo do Windows](images/timeline.png)

Da mesma forma, vincular seu telefone ao seu computador Windows permite que você continue o que estava fazendo anteriormente no dispositivo Android ou iOS.

Pense em uma **UserActivity** como algo específico o usuário estava trabalhando dentro de seu aplicativo. Por exemplo, se você estiver usando um leitor RSS, um **UserActivity** poderia ser o feed que você está lendo. Se você estiver jogando um jogo, o **UserActivity** poderia ser o nível que você está executando. Se você está ouvindo um aplicativo de música, o **UserActivity** poderia ser a playlist que você está ouvindo. Se você estiver trabalhando em um documento, o **UserActivity** poderia ser onde estava trabalhando nisso e assim por diante.  Em poucas palavras, uma **UserActivity** representa um destino dentro de seu aplicativo que permite que o usuário retome o que estava fazendo.

Quando você se envolve com uma **UserActivity** chamando [UserActivity.CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), o sistema cria um registro de histórico que indica a hora de início e fim de **UserActivity**. Conforme você se envolver novamente com que **UserActivity** ao longo do tempo, vários registros de histórico são registrados para ele.

## <a name="add-user-activities-to-your-app"></a>Adicionar atividades do usuário ao seu app

Uma [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) é a unidade de envolvimento do usuário no Windows. Ele tem três partes: um URI usado para ativar o aplicativo pertence a atividade, os elementos visuais e metadados que descrevem a atividade.

1. O [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) é usado para retomar o aplicativo com um contexto específico. Normalmente, esse link assume a forma de manipulador de protocolo para um esquema (por exemplo, "my-app://page2?action=edit") ou de um AppUriHandler (por exemplo, http://constoso.com/page2?action=edit).
2. [VisualElements](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) expõe uma classe que permite ao usuário identificar visualmente uma atividade com um título, descrição ou elementos de cartão adaptável.
3. Por fim, [conteúdo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) é onde você pode armazenar metadados para a atividade que pode ser usado para agrupar e recuperar as atividades em um contexto específico. Muitas vezes, isso assume a forma de dados [http://schema.org](http://schema.org).

Para adicionar uma **UserActivity** ao seu app:

1. Gerar objetos **UserActivity** quando o contexto do usuário muda dentro do aplicativo (como navegação de página, novo nível de jogo etc.)
2. Popular objetos **UserActivity** com o conjunto mínimo de campos obrigatórios: [ActivityId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri) e [UserActivity.VisualElements.DisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. Adicione um manipulador de esquema personalizado ao seu aplicativo para que ele possa ser ativado novamente por uma **UserActivity**.

Uma **UserActivity** pode ser integrada em um aplicativo com apenas algumas linhas de código. Por exemplo, imagine este código em MainPage.xaml.cs, dentro da classe MainPage (observação: pressupõe `using Windows.ApplicationModel.UserActivities;`):

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

A primeira linha no método `GenerateActivityAsync()` acima obtém um [UserActivityChannel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel) do usuário. Este é o feed de atividades do aplicativo serão publicadas. A próxima linha consulta o canal de uma atividade chamado `MainPage`.

* Seu aplicativo deve nomear as atividades de forma que o mesmo ID é gerado cada vez que o usuário está em um local específico no aplicativo. Por exemplo, se seu aplicativo for baseado em página, use um identificador para a página; se com base em seu documento, use o nome do documento (ou um hash do nome).
* Se houver uma atividade existente no feed com a mesma ID, essa atividade será retornada do canal com `UserActivity.State` definido como [Publicado](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitystate)). Se não houver nenhuma atividade com esse nome e nova atividade for retornada com `UserActivity.State` definido como **Novo**.
* Atividades passam para seu aplicativo. Você não precisa se preocupar em seu ID de atividade colidindo com IDs em outros aplicativos.

Depois de obter ou criar a **UserActivity**, especifique os outros dois campos obrigatórios: `UserActivity.VisualElements.DisplayText`e `UserActivity.ActivationUri`.

Em seguida, salve os metadados de **UserActivity** chamando [SaveAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync) e, finalmente, [CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), que retorna uma [UserActivitySession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitysession). A **UserActivitySession** é o objeto que podemos usar para gerenciar quando o usuário realmente está envolvido com a **UserActivity**. Por exemplo, devemos chamar `Dispose()` na **UserActivitySession** quando o usuário sai da página. No exemplo acima, também chamamos `Dispose()` em `_currentActivity` antes de chamar `CreateSession()`. Isso ocorre porque fizemos `_currentActivity` um campo de membro de nossa página, e queremos parar qualquer atividade existente antes de começar uma nova (observação: o `?` é o [operador null-conditional](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operators) que testa para null antes de executar o acesso de membro).

Como, nesse caso, o `ActivationUri` é um esquema personalizado, também precisamos registrar o protocolo no manifesto do aplicativo. Isso é feito no arquivo XML Package.appmanifest ou usando o designer.

Para fazer a alteração com o designer, clique duas vezes no arquivo Package.appmanifest no seu projeto para iniciar o designer, selecione a guia **Declarações** e adicione uma definição de **Protocolo**. A única propriedade que precisa ser preenchida, por enquanto, é **Nome**. Ele deve coincidir com o URI especificado acima,`my-app`.

Agora, precisamos criar um código para informar ao aplicativo o que fazer quando ele for foi ativado por um protocolo. Vamos substituir o método `OnActivated` em App.xaml.cs para passar o URI para a página principal, da seguinte forma:

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

Esse código detecta se o aplicativo foi ativado por meio de um protocolo. Se foi, ele procura para ver o que o aplicativo deve fazer para retomar a tarefa que ele está sendo ativado. Sendo um aplicativo simples, a atividade somente que esse aplicativo é retomado está ficando na página secundária quando o aplicativo é exibida.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Usar Cartões Adaptáveis para melhorar a experiência de linha do tempo

Atividades do usuário aparecem na Cortana e linha do tempo. Quando as atividades aparecem na linha do tempo, podemos exibi-las usando a estrutura [Cartão adaptável](http://adaptivecards.io/). Se você não fornecer um cartão adaptável para cada atividade, a linha do tempo criará automaticamente um cartão de atividade simples com base no nome do seu aplicativo e ícone, o campo de título e descrição opcional do campo. Abaixo há uma carga de cartão adaptável de exemplo e o cartão que ele produz.

![Um cartão adaptável](images/adaptivecard.png)]

Cadeia de caracteres JSON de conteúdo de cartão adaptável de exemplo:

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

Adicione a carga de cartões adaptáveis como uma cadeia de caracteres JSON para o **UserActivity** da seguinte maneira:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Integração de plataforma cruzada e serviço para serviço

Se seu aplicativo é executado em várias plataformas (por exemplo, no Android e iOS), ou mantém o estado do usuário na nuvem, você pode publicar UserActivities via [Microsoft Graph](https://developer.microsoft.com/graph/).
Depois que seu aplicativo ou serviço está autenticado com uma conta da Microsoft, é preciso apenas duas chamadas REST simples para gerar objetos de [Atividade](https://developer.microsoft.com/graph/docs/api-reference/beta/api/projectrome_put_activity) e [Histórico](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/projectrome_historyitem), usando os mesmos dados como descrito acima.

## <a name="summary"></a>Resumo

Você pode usar a API [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) para fazer com que seu aplicativo apareça na linha do tempo e Cortana.
* Saiba mais sobre a API **UserActivity** no [Centro de Desenvolvimento do Windows](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)
* Confira o [exemplo de código](https://github.com/Microsoft/project-rome).
* Consulte [Cartões Adaptáveis mais sofisticados](http://adaptivecards.io/).
* Publique uma **UserActivity** do iOS, Android ou seu serviço Web via [Microsoft Graph](https://developer.microsoft.com/graph/).
* Saiba mais sobre o [Project Rome no GitHub](https://github.com/Microsoft/project-rome).

## <a name="key-apis"></a>APIs-chave

* [Namespace UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Tópicos relacionados

* [Atividades do usuário (documentos do projeto Roma)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de cartões adaptáveis, amostras](http://adaptivecards.io/)
* [Manipular a ativação do URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Envolvimento com seus clientes em qualquer plataforma usando o Microsoft Graph, Feed de atividades e cartões adaptáveis](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)