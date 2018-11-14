---
author: andrewleader
Description: Adaptive and interactive toast notifications let you create flexible pop-up notifications with more content, optional inline images, and optional user interaction.
title: Conteúdo da notificação do sistema
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.author: mijacobs
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, uwp, notificações do sistema, notificações do sistema interativas, notificações do sistema adaptáveis, conteúdo de notificação do sistema, conteúdo da notificação do sistema
ms.localizationpriority: medium
ms.openlocfilehash: 791b1dcede799de4ecf8480d994a5c0f1ddb58af
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6192177"
---
# <a name="toast-content"></a>Conteúdo da notificação do sistema

As notificações do sistema interativas e adaptáveis permitem a criação de notificações flexíveis com texto, imagens e botões/entradas.

> **APIs importantes**: [pacote NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Para ver os modelos herdados do Windows 8.1 e Windows Phone 8.1, consulte o [Catálogo de modelos de notificação do sistema herdados](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Introdução

**Instale a biblioteca de notificações.** Se você quiser usar a linguagem C# em vez de XML para gerar notificações, instale o pacote NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (procure por "notificações uwp"). Os exemplos em linguagem C# fornecidos neste artigo usam o pacote NuGet, versão 1.0.0.

**Instale o Visualizador de Notificações.** Esse aplicativo UWP gratuito ajuda você a criar notificações interativas do sistema, fornecendo uma visualização instantânea da notificação enquanto você a edita, semelhante ao modo de exibição de editor/design XAML do Visual Studio. Consulte [Visualizador de notificações](notifications-visualizer.md) para obter mais informações ou [Baixar o Visualizador de notificações da Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envio da notificação do sistema

Para saber como enviar uma notificação, consulte [Enviar notificação do sistema local](send-local-toast.md). Esta documentação abrange apenas a criação do conteúdo de notificação do sistema.


## <a name="toast-notification-structure"></a>Estrutura de notificação do sistema

As notificações do sistema são uma combinação de algumas propriedades de dados como Marcação/Grupo (que permitem a você identificar a notificação) e o *conteúdo da notificação do sistema*.

Os componentes principais de conteúdo da notificação do sistema são...
* **iniciar**: define quais argumentos serão passados para o aplicativo quando o usuário clica na notificação, permitindo que você se aprofunde no conteúdo correto exibido pela notificação do sistema. Para saber mais, consulte [Enviar notificação do sistema local](send-local-toast.md).
* **visual**: a parte visual da notificação do sistema, incluindo a associação genérica que contém os logotipos texto e imagens.
* **ações**: a parte da notificação do sistema, incluindo entradas e ações.
* **áudio**: controla o áudio reproduzido quando a notificação é exibida para o usuário.

O conteúdo da notificação do sistema é definido em XML bruto, mas você pode usar nossa [biblioteca NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para obter um modelo de objeto C# (ou C++) para construir o conteúdo de notificação do sistema. Este artigo documenta todo o conteúdo da notificação do sistema.

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

Esta é uma representação visual do conteúdo da notificação do sistema:

![estrutura de notificação do sistema](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Visual

Cada notificação do sistema deve especificar um elemento visual, onde você deve fornecer uma associação de notificação do sistema genérica que pode conter texto, imagens e muito mais. Esses elementos serão renderizados em vários dispositivos Windows, incluindo a área de trabalho, telefones, tablets e Xbox.

Para todos os atributos com suporte na seção visual e seus elementos filho, [consulte a documentação do esquema](toast-schema.md#toastvisual).

A identidade do seu aplicativo na notificação do sistema é transmitida por meio do ícone do aplicativo. No entanto, se você usar a substituição de logotipo do aplicativo, exibiremos o nome do aplicativo abaixo das linhas de texto.

| Identidade do aplicativo para notificação do sistema normal | Identidade do aplicativo com appLogoOverride |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>Elementos de texto

Cada notificação do sistema deve ter pelo menos um elemento de texto e conter dois elementos de texto adicionais, todos do tipo [**AdaptiveText**](toast-schema.md#adaptivetext).

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

A partir da Atualização de Aniversário do Windows 10, você pode controlar quantas linhas de texto são exibidas com a propriedade **HintMaxLines** no texto. O padrão (e máximo) é de até 2 linhas de texto para o título e até 4 linhas (combinadas) para os dois elementos de descrição adicionais (o segundo e o terceiro **AdaptiveText**).

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>Substituição de logotipo do aplicativo

Por padrão, o sistema exibirá o logotipo do aplicativo. No entanto, você pode substituir esse logotipo com sua própria imagem [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo). Por exemplo, se esta for uma notificação de uma pessoa, recomendamos substituir o logotipo do aplicativo por uma foto da pessoa.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

Você pode usar a propriedade **HintCrop** para alterar o recorte da imagem. Por exemplo, **Círculo** resulta em uma imagem recortada de círculo. Do contrário, a imagem é quadrada. As dimensões da imagem são 48 x 48 pixels em escala de 100%.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://picsum.photos/48?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```


## <a name="hero-image"></a>Imagem de herói

**Novidade na atualização de aniversário:** as notificações do sistema podem exibir uma imagem de herói, que é uma [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage) exibida com destaque na barra de notificação do sistema enquanto estiver na Central de Ações. As dimensões da imagem são 364 x 180 pixels em escala de 100%.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastGenericHeroImage()
    {
        Source = "https://picsum.photos/364/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Imagem embutida

Você pode fornecer uma imagem embutida de largura inteira que aparece ao expandir a notificação do sistema.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://picsum.photos/360/202?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```


## <a name="image-size-restrictions"></a>Restrições de tamanho de imagem

As imagens usadas em sua notificação do sistema podem ser originadas de...

 - http://
 - ms-appx:///
 - ms-appdata:///

Para imagens http e http remotas da Web, existem limites de tamanho do arquivo de cada imagem individual. Na Fall Creators Update (16299), aumentamos o limite para 3 MB em conexões normais e 1 MB em conexões limitadas. Antes disso, as imagens sempre eram limitadas a 200 KB.

| Conexão normal | Conexão limitada | Antes do Fall Creators Update |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

Se uma imagem excede o tamanho do arquivo ou não foi possível baixá-la, ou atingir o tempo limite, ela será descartada e o restante da notificação será exibido.


## <a name="attribution-text"></a>Texto de atribuição

**Novidade na Atualização de aniversário**: se você precisar fazer referência à origem do conteúdo, é possível usar o texto de atribuição. Este texto sempre é exibido na parte inferior da sua notificação, juntamente com a identidade do aplicativo ou o carimbo de data e hora da notificação.

Em versões mais antigas do Windows sem suporte a texto de atribuição, o texto será exibido simplesmente como outro elemento de texto (supondo que você ainda não tem o máximo de três elementos de texto).

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>Carimbo de data e hora personalizado

**Novidade na Atualização de Criadores**: agora você pode substituir o carimbo de data e hora do sistema por um personalizado que aparece quando a mensagem/informações/conteúdo é gerado. Este carimbo de data e hora fica visível na Central de Ações.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Para saber mais sobre como usar um carimbo de data e hora personalizado [carimbo de data e hora personalizado em notificações do sistema](custom-timestamps-on-toasts.md).

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="progress-bar"></a>Barra de progresso

**Novidade na Atualização para Criadores**: você pode fornecer uma barra de progresso em sua notificação do sistema para manter o usuário informado sobre o progresso das operações, como downloads e muito mais.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

Para saber mais sobre como usar uma barra de progresso, consulte [barra de progresso da notificação do sistema](toast-progress-bar.md).


## <a name="headers"></a>Cabeçalhos

**Novidade na Atualização para Criadores**: você pode agrupar as notificações em cabeçalhos na Central de ações. Por exemplo, você pode agrupar as mensagens de um chat do grupo em um cabeçalho ou notificações do grupo de um tema comum em um cabeçalho ou mais.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Para saber mais sobre como usar cabeçalhos, consulte [Cabeçalhos da notificação do sistema](toast-headers.md).


## <a name="adaptive-content"></a>Conteúdo adaptável

**Novidade na Atualização de aniversário**: além do conteúdo especificado acima, você também pode exibir conteúdo adaptável adicional que é visível quando a notificação é expandida.

Esse conteúdo adicional é especificado usando Adaptável e você pode saber mais sobre isso lendo a [documentação de Blocos adaptáveis](create-adaptive-tiles.md).

Observe que qualquer conteúdo adaptável deve estar contido em um [**AdaptiveGroup**](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema#adaptivegroup). Caso contrário, ele não será renderizado usando o recurso adaptável.


### <a name="columns-and-text-elements"></a>Colunas e elementos de texto

Aqui está um exemplo em que as colunas e alguns elementos avançados de texto adaptável são usados. Como os elementos de texto estão em um **AdaptiveGroup**, eles oferecem suporte a todas as propriedades avançadas de estilo adaptável.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="buttons"></a>Botões

Os botões proporcionam a interação com a notificação do sistema, permitindo ao usuário executar ações rápidas na notificação do sistema sem interromper o fluxo de trabalho atual. Por exemplo, os usuários podem responder a uma mensagem diretamente em uma notificação do sistema ou excluir um email sem precisar abrir o aplicativo de email. Os botões aparecem na parte expandida da notificação.

Para saber mais sobre a implementação de botões de ponta a ponta, consulte [Enviar notificações do sistema local](send-local-toast.md).

Os botões podem realizar as seguintes ações diferentes...

-   Ativar o aplicativo em primeiro plano, com um argumento de ação específica que pode ser usado para navegar até uma página/contexto específico.
-   Ativar a tarefa em segundo plano do aplicativo para um cenário de resposta rápida ou semelhante.
-   Ativar outro aplicativo por meio da inicialização de protocolo.
-   Executar uma ação do sistema, como adiar ou ignorar a notificação.

> [!NOTE]
> Você pode ter até cinco botões (incluindo itens de menu de contexto que abordaremos mais adiante).

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="buttons-with-icons"></a>Botões com ícones

Você pode adicionar ícones ao seus botões. Esses ícones são imagens transparentes brancas de 16 x 16 pixels na escala de 100% e não devem ter preenchimento na imagem em si. Se você optar por fornecer ícones em uma notificação do sistema, é necessário fornecer ícones para todos os seus botões na notificação, pois isso transforma o estilo dos botões em botões de ícones.

> [!NOTE]
> Para fins de acessibilidade, certifique-se de incluir uma versão do ícone em contraste com branco (um ícone preto para telas de fundo brancas), para que o ícone fique visível quando o usuário ativar o modo de Branco em alto contraste. Saiba mais sobre a página de [acessibilidade de notificação do sistema](tile-toast-language-scale-contrast.md).

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

```csharp
new ToastButton("Dismiss", "dismiss")
{
    ActivationType = ToastActivationType.Background,
    ImageUri = "Assets/ToastButtonIcons/Dismiss.png"
}
```


```xml
<action
    content="Dismiss"
    imageUri="Assets/ToastButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```


### <a name="buttons-with-pending-update-activation"></a>Botões com ativação de atualização pendente

**Novidade na Fall Creators Update**: você pode usar uma ativação após o comportamento de **PendingUpdate** nos botões de ativação em segundo plano para criar interações de várias etapas nas notificações do sistema. Quando o usuário clicar no botão, a tarefa em segundo plano é ativada e a notificação do sistema é colocada em um estado de "atualização pendente", onde permanece na tela até que a tarefa em segundo plano substitua a notificação do sistema por uma nova notificação do sistema.

Para saber como implementar isso, consulte [atualização pendente de notificação do sistema](toast-pending-update.md).

![Notificação do sistema com atualizações pendentes](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>Ações do menu de contexto

**Novidade na Atualização de Aniversário**: você pode adicionar mais ações de menu de contexto ao menu de contexto existente que aparece quando o usuário clica com o botão direito do mouse na sua notificação na Central de ações. Observe que esse menu aparece somente quando o botão direito do mouse clicar na Central de ações. Ele não aparece quando o botão direito do mouse clica em uma faixa de pop-up de notificação do sistema.

> [!NOTE]
> Em dispositivos mais antigos, essas ações de menu de contexto adicionais aparecem como botões normais na notificação do sistema.

As ações do menu de contexto adicionadas (por exemplo, "Alterar local") aparecem acima das duas entradas de sistema padrão.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

> [!NOTE]
> Itens de menu de contexto adicionais contribuem para o limite total de cinco botões em uma notificação do sistema.

A ativação de itens de menu de contexto adicionais é processada de forma idêntica a dos botões de notificação.


## <a name="inputs"></a>Entradas

As entradas são especificadas na região de Ações da região de notificação do sistema, ou seja, ficam visíveis somente quando a notificação do sistema é expandida.


### <a name="quick-reply-text-box"></a>Caixa de texto de resposta rápida

Para habilitar uma caixa de texto de resposta rápida, como para um cenário de mensagens, adicione uma entrada de texto e um botão, e faça referência à id de entrada de texto para que o botão seja exibido adjacentes à entrada.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Entradas com barra de botões

Você também pode ter uma (ou várias) entradas com botões normais exibidos abaixo das entradas.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Entrada de seleção

Além de caixas de texto, você também pode usar um menu de seleção.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


### <a name="snoozedismiss"></a>Adiamento/ignorar

Com um menu de seleção e dois botões, podemos criar uma notificação de lembrete que utiliza as ações de adiamento de sistema e ignorar. Certifique-se de definir o cenário como Lembrete para que a notificação se comporte como um lembrete.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

Vinculamos o botão de Adiamento para a entrada do menu de seleção usando a propriedade **SelectionBoxId** no botão de notificação do sistema.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

Para usar as ações de adiamento e ignorar:

-   Especifique um **ToastButtonSnooze** ou **ToastButtonDismiss**
-   Como alternativa, especifique uma cadeia de caracteres de conteúdo personalizada:
    -   Se você não fornecer uma cadeia de caracteres, usaremos automaticamente as cadeias de caracteres localizadas de "Adiar" e "Ignorar".
-   Opcionalmente, especifique a **SelectionBoxId**:
    -   Se você não quiser que o usuário selecione um intervalo de adiamento e, em vez disso, apenas que sua notificação seja adiada apenas uma vez por um intervalo de tempo definido pelo sistema (consistente com o sistema operacional), não crie nenhuma &lt;input&gt;.
    -   Se você quiser fornecer seleções de intervalo de adiamento:
        -   Especifique **SelectionBoxId** na ação de adiamento
        -   Combine a identificação da entrada com a **SelectionBoxId** da ação de adiamento
        -   Especifique o valor de **ToastSelectionBoxItem** como um nonNegativeInteger que represente o intervalo de adiamento em minutos.



## <a name="audio"></a>Áudio

O áudio personalizado sempre contou com suporte na versão Mobile e tem suporte na versão Desktop 1511 (compilação 10586) ou mais recente. O áudio personalizado pode ser referenciado por meio dos seguintes caminhos:

-   ms-appx:///
-   ms-appdata:///

Você também pode escolher uma opção da [lista de ms-winsoundevents](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements), que sempre tem suporte nas duas plataformas.

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

Consulte a [página de esquema de áudio](/uwp/schemas/tiles/toastschema/element-audio) para obter informações sobre áudio em notificações do sistema. Para saber como enviar uma notificação do sistema usando o áudio personalizado, consulte [áudio personalizado em notificações do sistema](custom-audio-on-toasts.md).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmes, lembretes e chamadas de entrada

Para criar alarmes, lembretes e notificações de chamadas de entrada, você pode usar uma notificação normal do sistema com um valor de cenário atribuído a ela. O cenário ajusta alguns comportamentos para criar uma experiência do usuário unificada e consistente.

> [!IMPORTANT]
> Ao usar o Lembrete ou o Alarme, você deve fornecer pelo menos um botão em sua notificação do sistema. Caso contrário, a notificação do sistema será tratada como uma notificação do sistema normal.

* **Lembrete**: a notificação do sistema permanecerá na tela até que o usuário a ignore ou execute uma ação. No Windows Mobile, as notificações do sistema também aparecem pré-expandidas. Um lembrete sonoro será reproduzido.
* **Alarme**: além de comportamentos de lembrete, os alarmes fazem o loop do áudio com um som de alarme padrão.
* **IncomingCall**: as notificações de chamadas de entrada são exibidas em tela inteira nos dispositivos Windows Mobile. Caso contrário, elas têm os mesmos comportamentos dos alarmes, exceto que utilizam áudio de toque e os botões têm estilos diferentes.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="localization-and-accessibility"></a>Localização e acessibilidade

Seu blocos e notificações do sistema podem carregar cadeias de caracteres e imagens adaptadas para exibição de idioma, de fator de escala, alto contraste e outros contextos de tempo de execução. Para obter mais informações, consulte [Suporte à notificação de bloco e notificações do sistema para o idioma, escala e alto contraste](tile-toast-language-scale-contrast.md).


## <a name="handling-activation"></a>Manipular a ativação
Para saber como manipular ativações de notificações do sistema (o usuário clica na notificação do sistema ou nos botões dela), consulte [Enviar notificação do sistema local](send-local-toast.md).
 
## <a name="related-topics"></a>Tópicos relacionados

* [Enviar uma notificação do sistema local e manipular a ativação](send-local-toast.md)
* [Biblioteca de notificações no GitHub (parte do Kit de ferramentas da comunidade UWP)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Suporte à notificação de bloco e notificações do sistema para o idioma, escala e alto contraste](tile-toast-language-scale-contrast.md)
