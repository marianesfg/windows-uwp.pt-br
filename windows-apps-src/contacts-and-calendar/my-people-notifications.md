---
title: "Notificações de Minhas Pessoas"
description: "Explica como criar e usar notificações de Minhas Pessoas, um novo tipo de notificação do sistema."
author: mukin
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b1fbba8b8cea3edd51dc9b60cae1ea3853f39dd1
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2017
---
# <a name="my-people-notifications"></a>Notificações de Minhas Pessoas

> [!IMPORTANT]
> **PRÉ-LANÇAMENTO | Requer a Fall Creators Update**: você deve ter como alvo o [SDK do Insider 16225](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) e ter a [compilação 16226 do Insider](https://blogs.windows.com/windowsexperience/2017/06/21/announcing-windows-10-insider-preview-build-16226-pc/) ou superior instalada para usar as APIs de Minhas Pessoas.

As notificações de Minhas Pessoas são um novo tipo de gesto que colocam as pessoas em primeiro lugar. Elas fornecem aos usuários uma nova maneira de se conectar com as pessoas importantes para eles por meio de gestos expressivos rápidos e leves.

![notificação de emoji de coração](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Requisitos

+ Windows 10 e Microsoft Visual Studio 2017. Para obter detalhes da instalação, consulte [Prepare-se para começar o Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conhecimento básico de C# ou uma linguagem de programação similar orientada a objetos. Para começar a usar C#, consulte [Criar um app "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="how-it-works"></a>Como funciona

Como uma alternativa para as notificações genéricas do sistema, os desenvolvedores de aplicativos agora podem enviar notificações por meio do recurso Minhas Pessoas, para oferecer uma experiência mais pessoal aos usuários. Esse é um novo tipo de notificação do sistema, enviada de um contato fixado na barra de tarefas do usuário. Quando a notificação é recebida, a imagem do contato remetente será animada na barra de tarefas e um som será reproduzido, indicando que uma notificação de Minhas Pessoas está iniciando. Em seguida, a animação ou imagem especificada no conteúdo será exibida por 5 segundos (se o conteúdo for uma animação que dura menos de 5 segundos, ela será exibida em loop até se passarem 5 segundos).

## <a name="supported-image-types"></a>Tipos de imagem com suporte

+ GIF
+ Imagem estática (JPEG, PNG)
+ Spritesheet (somente vertical)

> [!NOTE]
> Spritesheet é uma animação derivada de uma imagem estática (JPEG ou PNG). Quadros individuais são organizados verticalmente, de forma que o primeiro quadro fique na parte superior (você pode especificar um quadro inicial diferente no conteúdo da notificação do sistema). Cada quadro deve ter a mesma altura, que o programa transforma em loop para criar uma sequência animada (como um flipbook com todas as páginas dispostas na vertical). Um exemplo de spritesheet é mostrado abaixo.

![spritesheet de arco-íris](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Parâmetros de notificação
As notificações de Minhas Pessoas usam a estrutura de [notificação do sistema](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md) no Windows 10 e exigem um nó de associação adicional no conteúdo da notificação do sistema. Isso significa que as notificações por meio de Minhas Pessoas devem ter duas associações em vez de uma. Essa segunda associação deve incluir o seguinte parâmetro:

```xml
experienceType=”shoulderTap”
```

Isso indica que a notificação do sistema deve ser tratada como uma notificação de Minhas Pessoas.

O nó da imagem dentro de associação deve incluir os seguintes parâmetros:

+ **src**
    + O URI do ativo. Isso pode ser um URI HTTP/HTTPS da Web, um URI msappx ou um caminho para um arquivo local.
+ **spritesheet-src**
    + O URI do ativo. Isso pode ser um URI HTTP/HTTPS da Web, um URI msappx ou um caminho para um arquivo local. Só é necessário para animações em spritesheet.
+ **spritesheet-height**
    + A altura do quadro (em pixels). Só é necessário para animações em spritesheet.
+ **spritesheet-fps**
    + Quadros por segundo. Só é necessário para animações em spritesheet.
+ **spritesheet-startingFrame**
    + O número do quadro para iniciar a animação. Somente usado para animações em spritesheet, e o padrão será 0 se não especificado.
+ **alt**
    + Cadeia de texto usada para narração do leitor de tela.

> [!NOTE]
> Mesmo se você estiver usando uma spritesheet, ainda deve especificar uma imagem estática no parâmetro "src" como fallback em caso de falha na exibição da animação.

Além disso, o nó de nível superior da notificação do sistema deve incluir o parâmetro **hint-people** para indicar o contato remetente. Esse parâmetro pode ter qualquer um dos seguintes valores:

+ **Endereço de email** 
    + Ex. mailto:johndoe@mydomain.com
+ **Número de telefone** 
    + Ex. tel:888-888-8888
+ **ID Remota** 
    + Ex. remoteid:1234

> [!NOTE]
> Se seu app utiliza as [APIs de ContactStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) e utiliza a propriedade [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact#Windows_Phone_PersonalInformation_StoredContact_RemoteId) para associar contatos armazenados no computador com contatos armazenados remotamente, é fundamental que o valor para a propriedade RemoteId seja estável e exclusivo. Isso significa que a ID remota deve identificar consistentemente uma única conta de usuário e deve manter uma única marca para garantir que ele não entre em conflito com as IDs remotas de outros contatos no computador, incluindo contatos que são de propriedade de outros aplicativos.
> Se as IDs remotas usadas por seu app não forem com certeza estáveis e exclusivas, você pode usar a classe RemoteIdHelper mostrada mais adiante neste tópico para adicionar uma marca exclusiva a todas as IDs remotas antes de você adicioná-las ao sistema. Ou você pode optar por não usar a propriedade RemoteId e criar uma propriedade estendida personalizada na qual armazenar as IDs remotas de seus contatos.

Além da segunda associação e do conteúdo, você DEVE incluir outro conteúdo na primeira associação para a notificação do sistema de fallback (que a notificação usará caso seja forçado a reverter para uma notificação do sistema comum). Isso é explicado com mais detalhes na última seção.

## <a name="creating-the-notification"></a>Criando a notificação
Você pode criar um modelo de notificação de Minhas Pessoas exatamente como faria com uma [notificação do sistema](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md).

Aqui está um exemplo de como criar uma notificação de Minhas Pessoas usando uma imagem estática:

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

Se você iniciar a notificação, ela deverá ser semelhante a esta:

![notificação em imagem estática](images/static-image-notification-small.gif)

Agora vamos mostrar como criar uma notificação usando uma spritesheet animada. Esta spritesheet tem uma altura de quadro de 80 pixels, que animaremos em 25 quadros por segundo. Definimos o quadro inicial para 15 e fornecemos a ele uma imagem estática de fallback no parâmetro "src". A imagem de fallback é usada para o caso de a animação em spritesheet falhar.

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

Se você iniciar a notificação, ela deverá ser semelhante a esta:

![notificação em spritesheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Iniciando a notificação
Para iniciar uma notificação de Minhas Pessoas, precisamos converter o modelo de notificação do sistema em um objeto [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx). Presumindo que você definiu a notificação do sistema em um arquivo XML (denominado "content.xml" aqui), você pode usar este código C# para iniciá-la:

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
Haverá casos em que uma notificação codificada como notificação de Minhas Pessoas será exibida como uma notificação comum do sistema. Uma notificação de Minhas Pessoas fará fallback para a notificação do sistema nas seguintes condições:

+ Falha na exibição da notificação
+ Notificações de Minhas Pessoas não habilitadas pelo destinatário
+ O contato do remetente não está fixado na barra de tarefas do destinatário

Se uma notificação de Minhas Pessoas fizer fallback para a notificação do sistema, a segunda associação específica de Minhas Pessoas será ignorada, e somente a primeira associação será usada para exibir a notificação do sistema. Isso significa que, conforme mencionado anteriormente, um conteúdo de fallback deve ser fornecido na primeira associação de notificação do sistema.

## <a name="see-also"></a>Veja também
+ [Adicionando suporte para Minhas Pessoas](my-people-support.md)
+ [Notificações do sistema adaptativas](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md)
+ [Classe ToastNotification](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)