---
title: Notificações de Minhas Pessoas
description: Explica como criar e usar notificações de Minhas Pessoas, um novo tipo de notificação do sistema.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d4150e7296badd3d31a9aacc7becd3d849f6affd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360417"
---
# <a name="my-people-notifications"></a>Notificações de Minhas Pessoas

As notificações de Minhas Pessoas oferecem uma nova maneira para os usuários se conectarem com as pessoas importantes para eles por meio de gestos expressivos rápidos. Este artigo mostra como projetar e implementar as notificações de Minhas Pessoas no aplicativo. Para implementações completas, consulte o [Exemplo de notificações de Minhas Pessoas.](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![notificação de emoji de coração](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Requisitos

+ Windows 10 e Microsoft Visual Studio 2017. Para obter detalhes da instalação, consulte [Prepare-se para começar o Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conhecimento básico de C# ou uma linguagem de programação similar orientada a objetos. Para começar a usar C#, consulte [Criar um app "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="how-it-works"></a>Como funciona

Como uma alternativa para as notificações genéricas do sistema, agora você pode enviar notificações por meio do recurso Minhas Pessoas, para oferecer uma experiência mais pessoal aos usuários. Esse é um novo tipo de notificação do sistema, enviado de um contato fixado na barra de tarefas do usuário com o recurso Minhas Pessoas. Quando a notificação for recebida, a imagem do contato remetente será animada na barra de tarefas e um som será reproduzido, indicando que a notificação está iniciando. A animação ou imagem especificada no conteúdo será exibida por 5 segundos (ou se o conteúdo for uma animação que dura menos de 5 segundos, ela será exibida em loop até se passarem 5 segundos).

## <a name="supported-image-types"></a>Tipos de imagem com suporte

+ GIF
+ Imagem estática (JPEG, PNG)
+ Spritesheet (somente vertical)

> [!NOTE]
> Spritesheet é uma animação derivada de uma imagem estática (JPEG ou PNG). Quadros individuais são organizados verticalmente, de forma que o primeiro quadro fique na parte superior (no entanto, você pode especificar um quadro inicial diferente no conteúdo da notificação do sistema). Todos os quadros devem ter a mesma altura, que o programa transforma em loop para criar uma sequência animada (como um flipbook com suas páginas dispostas na vertical). Um exemplo de spritesheet é mostrado abaixo.

![spritesheet de arco-íris](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Parâmetros de notificação
As notificações de Minhas Pessoas usam a estrutura de [notificação do sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md), mas exigem um nó de associação adicional no conteúdo da notificação do sistema. Essa segunda associação deve incluir o seguinte parâmetro:

```xml
experienceType=”shoulderTap”
```

Isso indica que a notificação do sistema deve ser tratada como uma notificação de Minhas Pessoas.

O nó da imagem na associação deve incluir os seguintes parâmetros:

+ **src**
    + O URI do ativo. Isso pode ser um URI HTTP/HTTPS da Web, um URI msappx ou um caminho para um arquivo local.
+ **spritesheet-src**
    + O URI do ativo. Isso pode ser um URI HTTP/HTTPS da Web, um URI msappx ou um caminho para um arquivo local. Só é necessário para animações em spritesheet.
+ **spritesheet-height**
    + A altura do quadro (em pixels). Só é necessário para animações em spritesheet.
+ **spritesheet-fps**
    + Quadros por segundo (FPS). Só é necessário para animações em spritesheet. Somente valores de 1 a 120 têm suporte.
+ **spritesheet-startingFrame**
    + O número do quadro para iniciar a animação. Somente usado para animações em spritesheet, e o padrão será 0 se não especificado.
+ **alt**
    + Cadeia de texto usada para narração do leitor de tela.

> [!NOTE]
> Ao fazer uma notificação animada, você ainda deve especificar uma imagem estática no parâmetro "src". Ela será usada como um fallback se a animação não for exibida.

Além disso, o nó de nível superior da notificação do sistema deve incluir o parâmetro **hint-people** para especificar o contato remetente. Esse parâmetro pode ter qualquer um dos seguintes valores:

+ **Endereço de email** 
    + Ex. ` mailto:johndoe@mydomain.com `
+ **Número de telefone** 
    + Ex. tel:888-888-8888
+ **ID remota** 
    + Ex. remoteid:1234

> [!NOTE]
> Se seu app utiliza as [APIs de ContactStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) e utiliza a propriedade [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) para associar contatos armazenados no computador com contatos armazenados remotamente, é fundamental que o valor para a propriedade RemoteId seja estável e exclusivo. Isso significa que a ID remota deve identificar de modo consistente uma única conta de usuário e deve manter uma única marca para garantir que ela não entrará em conflito com as IDs remotas de outros contatos no computador, incluindo contatos que são de propriedade de outros apps.
> Se as IDs remotas usadas por seu app não forem com certeza estáveis e exclusivas, você poderá usar a [classe RemoteIdHelper](https://msdn.microsoft.com/en-us/library/windows/apps/jj207024(v=vs.105).aspx#BKMK_UsingtheRemoteIdHelperclass) para adicionar uma marca exclusiva a todas as IDs remotas antes de adicioná-las ao sistema. Como alternativa, você pode optar por não usar a propriedade RemoteId e criar uma propriedade estendida personalizada na qual armazenará as IDs remotas de seus contatos.

Além da segunda associação e conteúdo, você deve incluir outra conteúdo na primeira associação para a notificação do sistema de fallback. A notificação usará isso se precisar voltar a ser uma notificação do sistema regular (esse assunto será abordado em detalhes no [final deste artigo](/windows/uwp/contacts-and-calendar/my-people-notifications#falling-back-to-toast)).

## <a name="creating-the-notification"></a>Criando a notificação
Você pode criar um modelo de notificação de Minhas Pessoas exatamente como faria com uma [notificação do sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).

Este é um exemplo de como criar uma notificação de Minhas Pessoas com um conteúdo de imagem estática:

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Quando você iniciar a notificação, ela terá a seguinte aparência:

![notificação de imagem estática](images/static-image-notification-small.gif)

Este é um exemplo de como criar uma notificação com um conteúdo de spritesheet animada. Esta spritesheet tem uma altura de quadro de 80 pixels, que animaremos em 25 quadros por segundo. Definimos o quadro inicial para 15 e fornecemos a ele uma imagem estática de fallback no parâmetro "src". A imagem de fallback será usada se a animação em spritesheet não for exibida.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Quando você iniciar a notificação, ela terá a seguinte aparência:

![notificação em spritesheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Iniciando a notificação
Para iniciar uma notificação de Minhas Pessoas, precisamos converter o modelo de notificação do sistema em um objeto [XmlDocument](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument). Quando você tiver definido a notificação do sistema em um arquivo XML (denominado "content.xml"), poderá usar este código para iniciá-la:

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

Depois, você pode usar esse código para criar e enviar a notificação do sistema:

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>Fazendo o fallback para notificação do sistema
Haverá casos em que uma notificação de Minhas Pessoas será exibida como uma notificação comum do sistema. Uma notificação de Minhas Pessoas fará fallback para a notificação do sistema nas seguintes condições:

+ Falha na exibição da notificação
+ Notificações de Minhas Pessoas não habilitadas pelo destinatário
+ O contato do remetente não está fixado na barra de tarefas do destinatário

Se uma notificação de Minhas Pessoas fizer fallback para a notificação do sistema, a segunda associação específica de Minhas Pessoas será ignorada, e somente a primeira associação será usada para exibir a notificação do sistema. É por isso que é fundamental fornecer um conteúdo de fallback na primeira associação de notificação do sistema.

## <a name="see-also"></a>Consulte também
+ [Meu exemplo de notificações de pessoas](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [Adicionar pessoas Meu suporte](my-people-support.md)
+ [Notificações do sistema adaptável](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [Classe ToastNotification](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)
