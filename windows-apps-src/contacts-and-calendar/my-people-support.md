---
title: Adicionando o suporte para Minhas Pessoas a um aplicativo
description: Explica como adicionar o suporte para Minhas Pessoas a um aplicativo e como fixar e desafixar contatos
author: muhsinking
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 33e567db01916367c8ea30d98e59f421581ac7aa
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5825073"
---
# <a name="adding-my-people-support-to-an-application"></a>Adicionando o suporte para Minhas Pessoas a um aplicativo

O recurso Minhas Pessoas permite que os usuários fixem contatos de um aplicativo diretamente à sua barra de tarefas, que cria um novo objeto de contato com o qual eles possam interagir de várias maneiras. Este artigo mostra como você pode adicionar suporte para esse recurso, permitindo que os usuários fixem contatos diretamente de seu app. Quando os contatos são fixados, novos tipos de interação do usuário se tornam disponíveis, como [compartilhamento](my-people-sharing.md) e [notificações do Minhas Pessoas](my-people-notifications.md).

![Chat do Minhas Pessoas](images/my-people-chat.png)

## <a name="requirements"></a>Requisitos

+ Windows 10 e Microsoft Visual Studio 2017. Para obter detalhes da instalação, consulte [Prepare-se para começar o Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conhecimento básico de C# ou uma linguagem de programação similar orientada a objetos. Para começar a usar C#, consulte [Criar um app "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Visão geral

Há três coisas que você precisa fazer para permitir que seu aplicativo use o recurso Minhas Pessoas:

1. [Declare o suporte para o contrato de ativação shareTarget no manifesto do aplicativo.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Anote os contatos com os quais os usuários podem compartilhar usando seu aplicativo.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3.  Dê suporte a várias instâncias do seu aplicativo em execução ao mesmo tempo. Os usuários devem poder interagir com a versão completa de seu aplicativo enquanto o usam em um painel de contato.  Eles podem até usá-lo em vários painéis de contato ao mesmo tempo.  Para dar suporte a isso, seu aplicativo precisa ser capaz de executar várias exibições simultaneamente. Para saber como fazer isso, consulte o artigo ["Mostrar vários modos de exibição para um aplicativo"](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views).

Feito isso, seu aplicativo será exibido no painel de contato para os contatos anotados.

## <a name="declaring-support-for-the-contract"></a>Declarando suporte para o contrato

Para declarar o suporte para o contrato do recurso Minhas Pessoas, abra seu aplicativo no Visual Studio. No **Gerenciador de Soluções**, clique com o botão direito do mouse no arquivo **Package.appxmanifest** e selecione **Abrir com**. No menu, selecione **Editor de XML (texto)** e clique em **OK**. Faça as seguintes alterações no manifesto:

**Antes**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**Depois**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

Com essa adição, agora seu aplicativo pode ser iniciado por meio do contrato **windows.ContactPanel**, que permite que você interaja com os painéis de contato.

## <a name="annotating-contacts"></a>Anotando contatos

Para permitir que os contatos do seu aplicativo apareçam na barra de tarefas por meio do painel Minhas Pessoas, você precisa anotá-los no repositório de contato do Windows.  Para saber como gravar contatos, consulte o [exemplo de cartão de contato](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

Seu aplicativo também deve escrever uma anotação para cada contato. Anotações são dados do seu aplicativo que são associados a um contato. A anotação deve conter a classe ativável correspondente ao seu modo de exibição desejado no membro **ProviderProperties** e declarar suporte para a operação **ContactProfile**.

Você pode anotar contatos a qualquer momento enquanto seu aplicativo estiver em execução, mas geralmente é necessário anotar os contatos assim que eles são adicionados ao repositório de contatos do Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

O "appId" é o Nome da Família de Pacotes, seguido por '!' e a ID da classe ativável. Para localizar o Nome da Família de Pacotes, abra **Package.appxmanifest** usando o editor de padrão e procure na guia "Empacotamento". Aqui, o "Aplicativo" é a classe ativável correspondente ao modo de exibição do aplicativo.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Permitir que os contatos convidem novos usuários em potencial

Por padrão, seu aplicativo só aparecerá no painel de contato para os contatos que você tiver anotado especificamente.  Isso é para evitar confusão com os contatos que não podem interagir por meio de seu app.  Se você quiser que o aplicativo apareça para os contatos que ele não conhece (para convidar os usuários a adicionarem esse contato à conta deles, por exemplo), você poderá adicionar o seguinte ao seu manifesto:

**Antes**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**Depois**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

Com essa alteração, seu aplicativo será exibido como uma opção disponível no painel de contato para todos os contatos que o usuário fixou.  Quando seu aplicativo for ativado usando o contrato do painel de contato, verifique se o contato é aquele que seu aplicativo conhece.  Caso contrário, você deverá mostrar a nova experiência do usuário do seu aplicativo.

![Painel de contato do recurso Minhas Pessoas](images/my-people.png)

## <a name="support-for-email-apps"></a>Suporte para aplicativos de email

Se você estiver escrevendo um aplicativo de email, não precisa anotar todos os contatos manualmente.  Se você declarar o suporte para o painel de contato e para o protocolo mailto:, seu aplicativo aparecerá automaticamente para os usuários com um endereço de email.

## <a name="running-in-the-contact-panel"></a>Execução no painel de contato

Agora que seu aplicativo aparece no painel de contato para alguns ou todos os usuários, você precisa processar a ativação com o contrato do painel de contato.

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

Quando seu aplicativo for ativado com esse contrato, ele receberá um [objeto ContactPanelActivatedEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Contém a ID do contato com o qual seu aplicativo está tentando interagir na inicialização e um objeto [ContactPanel](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactpanel). Você deve manter uma referência a esse objeto ContactPanel, que permitirá a interação com o painel.

O objeto ContactPanel tem dois eventos que seu aplicativo deve escutar:
+ O evento **LaunchFullAppRequested** é enviado quando o usuário chama o elemento de interface que solicita que seu aplicativo completo seja iniciado na própria janela.  Seu aplicativo é responsável pela própria inicializando, passando todo o contexto necessário.  Você é livre para fazer isso da maneira que quiser (por exemplo, por meio de inicialização de protocolo).
+ O **evento Closing** é enviado quando seu aplicativo está prestes a ser fechado, permitindo que você salve qualquer contexto.

O objeto ContactPanel também permite que você defina a cor de fundo do cabeçalho do painel de contato (se não for definida, o padrão será o tema do sistema) e feche o painel de contato programaticamente.

## <a name="supporting-notification-badging"></a>Suporte a selos de notificação

Se você deseja fixar contatos na barra de tarefas para marcação quando chegam novas notificações do aplicativo relacionados à pessoa, você deve incluir o parâmetro **hint-people** nas [notificações do sistema](https://docs.microsoft.com/en-us/windows/uwp/shell/tiles-and-notifications/adaptive-interactive-toasts) e [Notificações expressivas de Minhas Pessoas](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications).

![Selos de notificações de pessoas](images/my-people-badging.png)

Para marcar um contato, o nó de nível superior da notificação do sistema deve incluir o parâmetro hint-people para indicar o envio ou o contato relacionado. Esse parâmetro pode ter qualquer um dos seguintes valores:
+ **Endereço de email** 
    + Ex. mailto:johndoe@mydomain.com
+ **Número de telefone** 
    + Ex. tel:888-888-8888
+ **ID Remota** 
    + Ex. remoteid:1234

Aqui está um exemplo de como identificar uma notificação do sistema está relacionado a uma determinada pessoa:
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> Se seu app utiliza as [APIs de ContactStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) e utiliza a propriedade [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) para associar contatos armazenados no computador com contatos armazenados remotamente, é fundamental que o valor para a propriedade RemoteId seja estável e exclusivo. Isso significa que a ID remota deve identificar consistentemente uma única conta de usuário e deve manter uma única marca para garantir que ele não entre em conflito com as IDs remotas de outros contatos no computador, incluindo contatos que são de propriedade de outros aplicativos.
> Se as IDs remotas usadas por seu app não forem com certeza estáveis e exclusivas, você pode usar a classe RemoteIdHelper mostrada mais adiante neste tópico para adicionar uma marca exclusiva a todas as IDs remotas antes de você adicioná-las ao sistema. Ou você pode optar por não usar a propriedade RemoteId e criar uma propriedade estendida personalizada na qual armazenar as IDs remotas de seus contatos.

## <a name="the-pinnedcontactmanager-class"></a>A classe PinnedContactManager

O [PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) é usado para gerenciar quais contatos são fixados à barra de tarefas. Essa classe permite que você fixe e desafixe contatos, determine se um contato está fixado e se há suporte para fixação em uma superfície específica no sistema em que seu aplicativo está sendo executado.

Você pode recuperar o objeto PinnedContactManager usando o método **GetDefault**:

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Fixando e desafixando contatos
Agora você pode fixar e desafixar contatos usando o PinnedContactManager que acabou de criar. Os métodos **RequestPinContactAsync** e **RequestUnpinContactAsync** fornecem ao usuário caixas de diálogo de confirmação, para que sejam chamadas pelo thread do ASTA (Application Single-Threaded Apartment) ou da interface do usuário.

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

Você também pode fixar vários contatos ao mesmo tempo:

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> Atualmente, não há uma operação em lote para desafixar contatos.

**Observação:** 

## <a name="see-also"></a>Veja também
+ [Compartilhamento de Minhas Pessoas](my-people-sharing.md)
+ [Notificações de Minhas Pessoas](my-people-notifications.md)
+ [Vídeo do Channel 9 sobre como adicionar o suporte para Minhas Pessoas a um aplicativo](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [Exemplo de integração de Minhas Pessoas](http://aka.ms/mypeoplebuild2017)
+ [Exemplo de cartão de visita](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Documentação da classe PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [Conectar seu aplicativo a ações em um cartão de contato](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/integrating-with-contacts)
