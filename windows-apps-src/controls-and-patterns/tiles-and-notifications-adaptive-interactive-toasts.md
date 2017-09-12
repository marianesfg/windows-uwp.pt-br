---
author: mijacobs
Description: "As notificações do sistema interativas e adaptáveis permitem criar notificações pop-up flexíveis com mais conteúdo, imagens embutidas opcionais e interação do usuário opcional."
title: "Notificações do sistema interativas e adaptáveis"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c8e77773b9118c3177dc958ddc7b51d32a452fa5
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2017
---
# <a name="adaptive-and-interactive-toast-notifications"></a>Notificações do sistema interativas e adaptáveis

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

As notificações do sistema interativas e adaptáveis permitem a criação de notificações flexíveis com texto, imagens e botões/entradas.

> **APIs importantes**: [pacote NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Para ver os modelos herdados do Windows8.1 e Windows Phone 8.1, consulte o [catálogo de modelos de notificação do sistema herdados](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Introdução

**Instale a biblioteca de notificações.** Se você quiser usar a linguagem C# em vez de XML para gerar notificações, instale o pacote NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (procure por "notificações uwp"). Os exemplos em linguagem C# fornecidos neste artigo usam o pacote NuGet, versão 1.0.0.

**Instale o Visualizador de Notificações.** Esse aplicativo UWP gratuito ajuda você a criar notificações interativas do sistema, fornecendo uma visualização instantânea da notificação enquanto você a edita, semelhante ao modo de exibição de editor/design XAML do Visual Studio. Você pode ler [esta postagem no blog](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx) para obter mais informações e baixar o Visualizador de Notificações [aqui](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envio da notificação do sistema

Para saber como enviar uma notificação, consulte [Enviar notificação do sistema local](tiles-and-notifications-send-local-toast.md). Esta documentação abrange apenas a criação do conteúdo de notificação do sistema.


## <a name="toast-notification-structure"></a>Estrutura de notificação do sistema

As notificações do sistema são uma combinação de algumas propriedades de dados como Marcação/Grupo (que permitem a você identificar a notificação) e o *conteúdo da notificação do sistema*.

Os componentes principais de conteúdo da notificação do sistema são...
* **iniciar**: define quais argumentos serão passados para o aplicativo quando o usuário clica na notificação, permitindo que você se aprofunde no conteúdo correto exibido pela notificação do sistema. Para saber mais, consulte [Enviar notificação do sistema local](tiles-and-notifications-send-local-toast.md).
* **visual**: a parte visual da notificação do sistema, incluindo a associação genérica que contém os logotipos de texto, imagens e aplicativo.
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

Cada notificação do sistema deve especificar um elemento visual, onde você deve fornecer uma associação de notificação do sistema genérica, que pode conter texto, imagens, logotipos e muito mais. Esses elementos serão renderizados em vários dispositivos Windows, incluindo a área de trabalho, telefones, tablets e Xbox.

Para todos os atributos com suporte na seção visual e seus elementos filho, [consulte a documentação do esquema](tiles-and-notifications-toast-schema.md#toastvisual).

A identidade do seu aplicativo na notificação do sistema é transmitida por meio do ícone do aplicativo. No entanto, se você usar a substituição de logotipo do aplicativo, exibiremos o nome do aplicativo abaixo das linhas de texto.

| Notificação normal do sistema                                                                              | Notificação do sistema com appLogoOverride                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![notificação sem appLogoOverride](images/adaptivetoasts-withoutapplogooverride.jpg) | ![notificação com appLogoOverride](images/adaptivetoasts-withapplogooverride.jpg) |


## <a name="text-elements"></a>Elementos de texto

Cada notificação do sistema deve ter pelo menos um elemento de texto e conter dois elementos de texto adicionais, todos do tipos [AdaptiveText](tiles-and-notifications-toast-schema.md#adaptivetext).

![Notificações do sistema com título e descrição](images/toast-title-and-description.jpg)

A partir da Atualização de aniversário, você pode controlar quantas linhas de texto são exibidas com a propriedade **HintMaxLines** no texto. Por padrão, o título exibe até 2 linhas de texto e as linhas de descrição exibem até quatro linhas de texto.

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

Por padrão, o sistema exibirá o logotipo do aplicativo. No entanto, você pode substituir esse logotipo com sua própria imagem [ToastGenericAppLogo](tiles-and-notifications-toast-schema.md#toastgenericapplogo). Por exemplo, se esta for uma notificação de uma pessoa, recomendamos substituir o logotipo do aplicativo por uma foto da pessoa.

![Notificação do sistema com substituição de logotipo do aplicativo](images/toast-applogooverride.jpg)

Você pode usar a propriedade **HintCrop** para alterar o recorte da imagem. Por exemplo, *círculo* resulta em uma imagem recortada de círculo. Do contrário, a imagem é quadrada. As dimensões da imagem são 64 x 64 pixels em escala de 100%.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://unsplash.it/64?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://unsplash.it/64?image=883"/>
</binding>
```


## <a name="hero-image"></a>Imagem de herói

**Novidade na atualização de aniversário**: as notificações do sistema podem exibir uma imagem de herói, que é uma [ToastGenericHeroImage](tiles-and-notifications-toast-schema.md#toastgenericheroimage) exibida com destaque na barra de notificação do sistema enquanto estiver na Central de Ações. As dimensões da imagem são 360 x 180 pixels em escala de 100%.

![Notificações do sistema com a imagem de herói](images/toast-heroimage.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastHeroImage()
    {
        Source = "https://unsplash.it/360/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://unsplash.it/360/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Imagem embutida

Você pode fornecer uma imagem embutida de largura inteira que aparece ao expandir a notificação do sistema.

![Notificação do sistema com imagem adicional](images/toast-additionalimage.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://unsplash.it/360/180?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://unsplash.it/360/180?image=1043" />
</binding>
```


## <a name="attribution-text"></a>Texto de atribuição

**Novidade na Atualização de aniversário**: se você precisar fazer referência à origem do conteúdo, é possível usar o texto de atribuição. Este texto sempre é exibido na parte inferior da sua notificação, juntamente com a identidade do aplicativo ou o carimbo de data e hora da notificação.

Em versões mais antigas do Windows sem suporte a texto de atribuição, o texto será exibido simplesmente como outro elemento de texto (supondo que você ainda não tem o máximo de três elementos de texto).

![Notificações do sistema com texto de atribuição](images/toast-attributiontext.jpg)

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

![Notificações do sistema com carimbo de data e hora personalizado](images/toast-customtimestamp.jpg)

Para saber mais sobre como usar um carimbo de data e hora personalizado, [consulte esta postagem do blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2017/01/09/custom-timestamp-on-toast-notifications-windows-10-creators-update/).

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


## <a name="adaptive-content"></a>Conteúdo adaptável

**Novidade na Atualização de aniversário**: além do conteúdo especificado acima, você também pode exibir conteúdo adaptável adicional que é visível quando a notificação é expandida.

Esse conteúdo adicional é especificado usando Adaptável e você pode saber mais sobre isso lendo a [documentação de Blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md).

Observe que qualquer conteúdo adaptável deve estar contido em um AdaptiveGroup. Caso contrário, ele não será renderizado usando o recurso adaptável.


### <a name="columns-and-text-elements"></a>Colunas e elementos de texto

Aqui está um exemplo em que as colunas e alguns elementos avançados de texto adaptável são usados. Como os elementos de texto estão em um AdaptiveGroup, eles oferecem suporte a todas as propriedades avançadas de estilo adaptável.

![Notificações do sistema com texto adicional](images/toast-additionaltext.jpg)

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


## <a name="inputs-and-buttons"></a>Entradas e botões

Os botões e as entradas são especificadas na região de Ações da região de notificação do sistema, ou seja, ficam visíveis somente quando a notificação do sistema é expandida.


### <a name="quick-reply-text-box"></a>Caixa de texto de resposta rápida

Para habilitar uma caixa de texto de resposta rápida, como para um cenário de mensagens, adicione uma entrada de texto e um botão, e faça referência à id de entrada de texto para que o botão seja exibido adjacentes à entrada.

![notificação com entrada e texto e ações](images/adaptivetoasts-xmlsample05.jpg)

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

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Send",
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Entradas com barra de botões

Você também pode ter uma (ou várias) entradas com botões normais exibidos abaixo das entradas.

![notificação com ações de entrada e texto](images/adaptivetoasts-xmlsample04.jpg)

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

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Reply",
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call",
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Entrada de seleção

Além de caixas de texto, você também pode usar um menu de seleção.

![notificação com entrada de seleção e ações](images/adaptivetoasts-xmlsample06.jpg)

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


## <a name="buttons"></a>Botões

Os botões proporcionam a interação com a notificação do sistema, permitindo ao usuário executar ações rápidas na notificação do sistema sem interromper o fluxo de trabalho atual. Por exemplo, os usuários podem responder a uma mensagem diretamente em uma notificação do sistema ou excluir um email sem precisar abrir o aplicativo de email.

Os botões podem realizar as seguintes ações diferentes...

-   Ativar o aplicativo em primeiro plano, com um argumento de ação específica que pode ser usado para navegar até uma página/contexto específico.
-   Ativar a tarefa em segundo plano do aplicativo para um cenário de resposta rápida ou semelhante.
-   Ativar outro aplicativo por meio da inicialização de protocolo.
-   Executar uma ação do sistema, como adiar ou ignorar a notificação.

Observe que você pode ter até cinco botões (incluindo itens de menu de contexto que abordaremos mais adiante).

![notificação com ações, exemplo 1](images/adaptivetoasts-xmlsample02.jpg)

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
            content="See more details",
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later",
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="snoozedismiss-buttons"></a>Botões de adiamento/ignorar

Com um menu de seleção e dois botões, podemos criar uma notificação de lembrete que utiliza as ações de adiamento de sistema e ignorar. Certifique-se de definir o cenário como Lembrete para que a notificação se comporte como um lembrete.

![notificação de lembrete](images/adaptivetoasts-xmlsample07.jpg)

Vinculamos o botão de Adiamento para a entrada do menu de seleção usando a propriedade *SepectionBoxId* no botão de notificação do sistema.

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

Para usar as ações de adiamento e ignorar, faça o seguinte:

-   Especifique um ToastButtonSnooze ou ToastButtonDismiss
-   Como alternativa, especifique uma cadeia de caracteres de conteúdo personalizada:
    -   Se você não fornecer uma cadeia de caracteres, usaremos automaticamente as cadeias de caracteres localizadas de "Adiar" e "Ignorar".
-   Opcionalmente, especifique a *SelectionBoxId*:
    -   Se você não quiser que o usuário selecione um intervalo de adiamento e, em vez disso, apenas que sua notificação seja adiada apenas uma vez por um intervalo de tempo definido pelo sistema (consistente com o sistema operacional), não crie nenhuma &lt;input&gt;.
    -   Se você quiser fornecer seleções de intervalo de adiamento:
        -   Especifique *SelectionBoxId* na ação de adiamento
        -   Combine a identificação da entrada com a *SelectionBoxId* da ação de adiamento
        -   Especifique o valor de *ToastSelectionBoxItem* como um nonNegativeInteger que represente o intervalo de adiamento em minutos.



## <a name="audio"></a>Áudio

O áudio personalizado sempre contou com suporte na versão Mobile e tem suporte na versão Desktop 1511 (compilação 10586) ou mais recente. O áudio personalizado pode ser referenciado por meio dos seguintes caminhos:

-   ms-appx:///
-   ms-appdata:///

Você também pode escolher uma opção da [lista de ms-winsoundevents](https://msdn.microsoft.com/library/windows/apps/br230842), que sempre tem suporte nas duas plataformas.

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

Consulte a [página de esquema de áudio](https://msdn.microsoft.com/library/windows/apps/br230842) para obter informações sobre áudio em notificações do sistema. Para saber como enviar uma notificação do sistema usando o áudio personalizado, [consulte esta postagem de blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmes, lembretes e chamadas de entrada

Para criar alarmes, lembretes e notificações de chamadas de entrada, você pode usar uma notificação normal do sistema com um valor de cenário atribuído a ela. O cenário ajusta alguns comportamentos para criar uma experiência do usuário unificada e consistente.

* **Lembrete**: a notificação do sistema permanecerá na tela até que o usuário a ignore ou execute uma ação. No Windows Mobile, as notificações do sistema também aparecem pré-expandidas. Um lembrete sonoro será reproduzido.
* **Alarme**: além de comportamentos de lembrete, os alarmes fazem o loop do áudio com um som de alarme padrão.
* **IncomingCall**: as notificações de chamadas de entrada são exibidas em tela inteira nos dispositivos Windows Mobile. Caso contrário, eles têm os mesmos comportamentos dos alarmes, exceto que utilizam áudio de toque.

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


## <a name="handling-activation"></a>Manipular a ativação
Para saber como manipular ativações de notificações do sistema (o usuário clica na notificação do sistema ou nos botões dela), consulte [Enviar notificação do sistema local](tiles-and-notifications-send-local-toast.md).


 
## <a name="related-topics"></a>Tópicos relacionados

* [Enviar uma notificação do sistema local e manipular a ativação](tiles-and-notifications-send-local-toast.md)
* [Biblioteca de notificações no GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)