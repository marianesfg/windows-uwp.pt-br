---
Description: Learn how to group notifications in Action Center using collections.
title: Coleções de notificações do sistema
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: windows 10, uwp, notificação, coleções, coleção, notificações de grupo, notificações de agrupamentos, grupo, organizar, Central de ações, notificação do sistema
ms.localizationpriority: medium
ms.openlocfilehash: 9b6818f876c094298a0a6636faa00efa9a192545
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7698365"
---
# <a name="grouping-toast-notifications-with-collections"></a>Agrupar notificações do sistema com coleções
Use coleções para organizar as notificações do sistema do aplicativo na Central de ações. As coleções ajudam os usuários a localizar informações na Central de ações mais facilmente e permitem aos desenvolvedores gerenciar melhor suas notificações.  As APIs a seguir permitem remover, criar e atualizar coleções de notificação.

> [!IMPORTANT]
> **Requer a Atualização para Criadores**: você precisa usar o SDK 15063 e executar a compilação 15063 ou mais recente para usar as coleções de notificações do sistema. As APIs relacionadas incluem [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection) e [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager)

Você pode ver o exemplo a seguir com um aplicativo de mensagens que separa as notificações com base no grupo de bate-papo; cada título (bate-papo do projeto comp Sci 160A, mensagens diretas, bate-papo do time de Lacrosse) é um conjunto separado.  Observe como as notificações são distintamente agrupadas como se estivessem em um aplicativo separado, mesmo quando são todas notificações do mesmo aplicativo.  Se estiver procurando uma maneira mais sutil de organizar suas notificações, consulte [cabeçalhos de notificação do sistema](toast-headers.md).  
![Exemplo de coleção com dois grupos diferentes de notificações](images/toast-collection-example.png)

## <a name="creating-collections"></a>Criar coleções
Ao criar cada coleção, você é solicitado a fornecer um nome de exibição e um ícone, que são exibidos na Central de ações como parte do título da coleção, como mostrado na imagem acima. As coleções também exigem um argumento de inicialização para ajudar o aplicativo a navegar até o local correto no aplicativo quando o título da coleção é clicado pelo usuário.  

### <a name="create-a-collection"></a>Criar uma coleção

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>Enviar notificações para uma coleção
Abordaremos notificações de envio de três pipelines de notificação do sistema diferentes: local, agendada e por push.  Para cada um desses exemplos, criaremos uma notificação do sistema de amostra para enviar o código a seguir imediatamente. Em seguida, destacaremos como adicionar a notificação do sistema para uma coleção por meio de cada pipeline.

Construir o conteúdo da notificação:

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>Enviar uma notificação do sistema para uma coleção

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>Adicionar uma notificação do sistema agendada para uma coleção

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>Enviar uma notificação do sistema por push para uma coleção
Para notificações do sistema por push, você precisa adicionar o cabeçalho X-WNS-CollectionId à mensagem POST.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Gerenciar coleções
#### <a name="create-the-toast-collection-manager"></a>Criar o gerenciador da coleção da notificação do sistema
Para o restante dos trechos de código da seção "Gerenciar coleções" usaremos o collectionManager abaixo.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Obter todas as coleções

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Obter o número de coleções criadas

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Remover uma coleção

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Atualizar uma coleção
Você pode atualizar coleções criando uma nova coleção com a mesma ID e salvando a nova instância da coleção.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>Gerenciando notificações em uma coleção
#### <a name="group-and-tag-properties"></a>Propriedades de grupo e marca
As propriedades de grupo e marca juntas identificam exclusivamente uma notificação em uma coleção.  Grupo (e marca) atuam como uma chave primária composta (mais de um identificador) para identificar sua notificação. Por exemplo, se quiser remover ou substituir uma notificação, você deve especificar *qual notificação* deseja remover/substituir; para fazer isso, especifique a Marca e o Grupo. Um exemplo é um aplicativo de mensagens.  O desenvolvedor pode usar a ID da conversa como o Grupo e a ID de mensagem como a Marca.

#### <a name="remove-a-toast-from-a-collection"></a>Remover um notificação do sistema de um coleção
Você pode remover notificações individuais usando a marca e as IDs de grupo ou apagar todas as notificações do sistema em uma coleção.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Apagar todas as notificações do sistema em uma coleção
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Coleções no Visualizador de Notificações
Você pode usar a ferramenta [Visualizador de notificações](notifications-visualizer.md) para ajudar a projetar suas coleções. Siga os passos abaixo:

* Clique no ícone de engrenagem no canto inferior direito. 
* Selecione "Coleções de notificações do sistema".
* Acima da visualização da notificação, há um menu suspenso de "Coleção de notificações do sistema". Selecione gerenciar as coleções.
* Clique em "Adicionar coleção", preencha os detalhes da coleção e salve.
* Você pode adicionar mais coleções ou clicar fora da caixa de gerenciamento de coleções para retornar à tela principal.
* Selecione a coleção à qual você deseja adicionar a notificação no menu suspenso "Coleção de notificações do sistema".
* Ao disparar a notificação do sistema, ela será adicionada à coleção apropriada na Central de ações.


## <a name="other-details"></a>Outros detalhes
As coleções de notificações do sistema que você cria também são refletidas em configurações de notificação do usuário.  Os usuários podem ativar as configurações para cada coleção de individual a fim de ativar ou desativar esses subgrupos.  Se as notificações estiverem desativadas no nível superior do aplicativo, todas as notificações de coleções serão desativadas também.  Além disso, cada coleção mostrará por padrão 3 notificações na Central de ações, e o usuário pode expandi-la para mostrar até 20 notificações.

## <a name="related-topics"></a>Tópicos relacionados

* [Conteúdo da notificação do sistema](adaptive-interactive-toasts.md)
* [Cabeçalhos de notificação do sistema](toast-headers.md)
* [Biblioteca de notificações no GitHub (parte do Kit de ferramentas da comunidade do Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)