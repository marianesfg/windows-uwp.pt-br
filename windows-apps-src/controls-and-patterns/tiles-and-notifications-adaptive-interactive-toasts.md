---
author: mijacobs
Description: "As notificações do sistema interativas e adaptáveis permitem criar notificações pop-up flexíveis com mais conteúdo, imagens embutidas opcionais e interação do usuário opcional."
title: "Notificações do sistema interativas e adaptáveis"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b1962e58d3513ddff908a0d556731d83cce20af4
ms.lasthandoff: 02/07/2017

---
# <a name="adaptive-and-interactive-toast-notifications"></a>Notificações do sistema interativas e adaptáveis

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

As notificações do sistema interativas e adaptáveis permitem criar notificações pop-up flexíveis com mais conteúdo, imagens embutidas opcionais e interação do usuário opcional.

O modelo de notificações do sistema interativas e adaptáveis tem estas atualizações sobre o catálogo de modelos de notificação do sistema herdados:

-   A opção de incluir botões e entradas nas notificações.
-   Três tipos diferentes de ativação para a notificação do sistema principal e para cada ação.
-   A opção para criar uma notificação para determinados cenários, inclusive alarmes, lembretes e chamadas de entrada.

**Observação**   Para ver os modelos herdados do Windows 8.1 e Windows Phone 8.1, consulte o [catálogo de modelos de notificação do sistema herdados](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Introdução

**Instale a biblioteca de notificações.** Se você quiser usar a linguagem C# em vez de XML para gerar notificações, instale o pacote NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (procure por "notificações uwp"). Os exemplos em linguagem C# fornecidos neste artigo usam o pacote NuGet, versão 1.0.0.

**Instale o Visualizador de Notificações.** Esse aplicativo UWP gratuito ajuda você a criar notificações interativas do sistema, fornecendo uma visualização instantânea da notificação enquanto você a edita, semelhante ao modo de exibição de editor/design XAML do Visual Studio. Você pode ler [esta postagem no blog](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx) para obter mais informações e pode baixar o Visualizador de Notificações [aqui](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="toast-notification-structure"></a>Estrutura de notificação do sistema


Notificações do sistema são construídas usando XML, que normalmente contêm estes elementos-chave:

-   &lt;visual&gt; abrange o conteúdo disponível para que os usuários vejam visualmente, incluindo texto e imagens
-   &lt;actions&gt; contém botões/entradas que o desenvolvedor quer adicionar dentro da notificação
-   &lt;audio&gt; especifica o som reproduzido quando a notificação aparece

Este é um exemplo de código:

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Sample</text>
      <text>This is a simple toast notification example</text>
      <image placement="AppLogoOverride" src="oneAlarm.png" />
    </binding>
  </visual>
  <actions>
    <action content="check" arguments="check" imageUri="check.png" />
    <action content="cancel" arguments="cancel" />
  </actions>
  <audio src="ms-winsoundevent:Notification.Reminder"/>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Sample"
                },
 
                new AdaptiveText()
                {
                    Text = "This is a simple toast notification example"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "oneAlarm.png"
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("check", "check")
            {
                ImageUri = "check.png"
            },
 
            new ToastButton("cancel", "cancel")
            {
                ImageUri = "cancel.png"
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = new Uri("ms-winsoundevent:Notification.Reminder")
    }
};
```

E uma representação visual da estrutura:

![estrutura de notificação do sistema](images/adaptivetoasts-structure.jpg)

### <a name="visual"></a>Visual

Dentro do elemento visual, você deve ter exatamente um elemento de associação que contém o conteúdo visual da notificação do sistema.

As notificações de bloco em aplicativos da Plataforma Universal do Windows (UWP) oferecem suporte a vários modelos que são baseados em diferentes tamanhos de bloco. As notificações do sistema, no entanto, têm apenas um nome de template: **ToastGeneric**. Ter apenas o nome de um modelo significa:

-   Você pode alterar o conteúdo da notificação do sistema, como adicionar outra linha de texto, adicionar uma imagem embutida ou alterar a imagem em miniatura de exibição do ícone do aplicativo para outra coisa, e fazer qualquer uma dessas coisas sem se preocupar com a alteração do modelo inteiro ou a criação de uma carga inválida devido a uma discrepância entre o nome do modelo e o conteúdo.
-   Você pode usar o mesmo código para construir a mesma carga para a **notificação do sistema** voltada para a entrega a diferentes tipos de dispositivos Microsoft Windows, incluindo telefones, tablets, computadores e Xbox One. Cada um desses dispositivos aceitará a notificação e a exibirá para o usuário em suas políticas de interface do usuário com as funcionalidades visuais apropriadas e o modelo de interação.

Para todos os atributos com suporte na seção visual e seus elementos filho, consulte a seção Esquema abaixo. Para obter mais exemplos, consulte a seção Exemplos de XML abaixo.

A identidade do seu aplicativo é representada pelo ícone dele. No entanto, se você usar appLogoOverride, exibiremos o nome de seu aplicativo embaixo das linhas de texto.

| Notificação normal do sistema                                                                              | Notificação do sistema com appLogoOverride                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![notificação sem appLogoOverride](images/adaptivetoasts-withoutapplogooverride.jpg) | ![notificação com appLogoOverride](images/adaptivetoasts-withapplogooverride.jpg) |

### <a name="actions"></a>Ações

Em aplicativos UWP, você pode adicionar botões e outras entradas às suas notificações do sistema, o que permite aos usuários fazer mais fora do aplicativo. Essas ações são especificadas no elemento &lt;actions&gt;, do qual há dois tipos que você pode especificar:

-   &lt;action&gt; Isso aparece como um botão em dispositivos móveis e desktop. É possível especificar até cinco ações do sistema ou personalizadas dentro de uma notificação do sistema.
-   &lt;input&gt; Isso permite que os usuários forneçam entrada, como responder rapidamente a uma mensagem ou selecionar uma opção em um menu suspenso.

&lt;action&gt; e &lt;input&gt; são adaptáveis na família de dispositivos Windows. Por exemplo, em dispositivos móveis ou desktop, uma &lt;action&gt; para um usuário é um botão no qual tocar/clicar. text &lt;input&gt; é uma caixa em que os usuários podem inserir texto usando um teclado físico ou virtual. Esses elementos também se adaptam a cenários de interação futuros, como uma ação anunciada por voz ou uma entrada de texto gerada por ditado.

Quando uma ação é executada pelo usuário, você pode executar um destes procedimentos especificando o atributo [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) dentro do elemento &lt;action&gt;.

-   Ativar o aplicativo em primeiro plano, com um argumento de ação específica que pode ser usado para navegar até uma página/um contexto específica(o).
-   Ativar a tarefa em segundo plano do aplicativo sem afetar o usuário.
-   Ativar outro aplicativo por meio da inicialização de protocolo.
-   Especificar uma ação do sistema para executar. As atuais ações do sistema disponíveis são adiar e ignorar o alarme/lembrete agendado, o que será explicado com mais detalhes em uma seção abaixo.

Para todos os atributos com suporte na seção visual e seus elementos filho, consulte a seção Esquema abaixo. Para obter mais exemplos, consulte a seção Exemplos de XML abaixo.

### <a name="audio"></a>Áudio

O áudio personalizado sempre contou com suporte na versão Mobile e tem suporte na versão Desktop 1511 (compilação 10586) ou mais recente. O áudio personalizado pode ser referenciado por meio dos seguintes caminhos:

-   ms-appx:///
-   ms-appdata:///

Você também pode escolher uma opção da [lista de ms-winsoundevents](https://msdn.microsoft.com/library/windows/apps/br230842), que sempre tem suporte nas duas plataformas.

Consulte a [página de esquema de áudio](https://msdn.microsoft.com/library/windows/apps/br230842) para obter informações sobre áudio em notificações do sistema. Para saber como enviar uma notificação do sistema usando o áudio personalizado, [consulte esta postagem de blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/).

## <a name="alarms-reminders-and-incoming-calls"></a>Alarmes, lembretes e chamadas de entrada


Você pode usar notificações do sistema para alarmes, lembretes e chamadas de entrada. Essas notificações especiais têm uma aparência que é consistente com as notificações do sistema padrão, apesar de as notificações do sistema especiais apresentarem alguns padrões e IU personalizados com base em cenário:

-   Uma notificação do sistema de lembrete permanecerá na tela até que o usuário a ignore ou execute uma ação. No Windows Mobile, as notificações do sistema de lembrete também aparecerão pré-expandidas.
-   Além de compartilhar os comportamentos acima com notificações de lembrete, as notificações de alarme também reproduzem automaticamente o áudio em loop.
-   As notificações de chamadas de entrada são exibidas em tela inteira em dispositivos Windows Mobile. Isso é feito especificando o atributo de cenário dentro do elemento raiz de uma notificação do sistema – &lt;toast&gt;: &lt;toast scenario=" { default | alarm | reminder | incomingCall }" &gt;

## <a name="xml-examples"></a>Exemplos de XML


**Observação**  As capturas de tela de notificação do sistema para estes exemplos foram extraídas de um aplicativo em desktop. Em dispositivos móveis, uma notificação do sistema pode estar recolhida quando aparece, com um elemento na parte inferior da notificação do sistema para expandi-la.

 

**Notificação com conteúdo visual avançado**

Este exemplo mostra como você pode ter várias linhas de texto, uma pequena imagem opcional para substituir o logotipo do aplicativo e uma miniatura da imagem embutida opcional.

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image src="https://unsplash.it/360/180?image=1043" />
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Photo Share"
                },
 
                new AdaptiveText()
                {
                    Text = "Andrew sent you a picture"
                },
 
                new AdaptiveText()
                {
                    Text = "See it in full size!"
                },
 
                new AdaptiveImage()
                {
                    Source = "https://unsplash.it/360/180?image=1043"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    }
};
```

![notificação com conteúdo visual avançado](images/adaptivetoasts-xmlsample01.jpg)

 

**Notificação com ações, exemplo 1**

Este exemplo mostra...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="See more details" arguments="details"/>
    <action activationType="background" content="Remind me later" arguments="later"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Microsoft Company Store"
                },
 
                new AdaptiveText()
                {
                    Text = "New Halo game is back in stock!"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "details"),
 
            new ToastButton("Remind me later", "later")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![notificação com ações, exemplo 1](images/adaptivetoasts-xmlsample02.jpg)

 

**Notificação com ações, exemplo 2**

Este exemplo mostra...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Restaurant suggestion...</text>
      <text>We noticed that you are near Wasaki. Thomas left a 5 star rating after his last visit, do you want to try it?</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="Reviews" arguments="reviews" />
    <action activationType="protocol" content="Show map" arguments="bingmaps:?q=sushi" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Restaurant suggestion..."
                },
 
                new AdaptiveText()
                {
                    Text = "We noticed that you are near Wasaki. Thomas left a 5 star rating after his last visit, do you want to try it?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("Reviews", "reviews"),
 
            new ToastButton("Show map", "bingmaps:?q=sushi")
            {
                ActivationType = ToastActivationType.Protocol
            }
        }
    }
};
```

![notificação com ações, exemplo 2](images/adaptivetoasts-xmlsample03.jpg)

 

**Notificação com entrada de texto e ações, exemplo 1**

Este exemplo mostra...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" />
    <action activationType="foreground" content="Video call" arguments="video" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Video call", "video")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![notificação com ações de entrada e texto](images/adaptivetoasts-xmlsample04.jpg)

 

**Notificação com entrada de texto e ações, exemplo 2**

Este exemplo mostra...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" hint-inputId="message" imageUri="Assets/Icons/send.png"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                TextBoxId = "message",
                ImageUri = "Assets/Icons/send.png",
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![notificação com entrada e texto e ações](images/adaptivetoasts-xmlsample05.jpg)

 

**Notificação com entrada de seleção e ações**

Este exemplo mostra...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
      <selection id="1" content="Breakfast" />
      <selection id="2" content="Lunch" />
      <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="foreground" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Spicy Heaven"
                },
 
                new AdaptiveText()
                {
                    Text = "When do you plan to come in tomorrow?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "2",
                Items =
                {
                    new ToastSelectionBoxItem("1", "Breakfast"),
                    new ToastSelectionBoxItem("2", "Lunch"),
                    new ToastSelectionBoxItem("3", "Dinner")
                }
            }
        },
 
        Buttons =
        {
            new ToastButton("Reserve", "reserve")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Call Restaurant", "call")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![notificação com entrada de seleção e ações](images/adaptivetoasts-xmlsample06.jpg)

 

**Notificação de lembrete**

Este exemplo mostra...

```XML
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  <visual>
    <binding template="ToastGeneric">
      <text>Adaptive Tiles Meeting</text>
      <text>Conf Room 2001 / Building 135</text>
      <text>10:00 AM - 10:30 AM</text>
    </binding>
  </visual>
 
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

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "action=viewEvent&eventId=1983",
    Scenario = ToastScenario.Reminder,
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Adaptive Tiles Meeting"
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
    },
 
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

![notificação de lembrete](images/adaptivetoasts-xmlsample07.jpg)

 

## <a name="handling-activation-foreground-and-background"></a>Manipulando a ativação (em primeiro plano e em segundo plano)

Para saber como manipular as ativações de notificação do sistema (o usuário clicar na notificação do sistema ou nos botões da notificação do sistema), consulte [Guia de início rápido: enviar uma notificação do sistema local e manipular a ativação](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/).


## <a name="schemas-ltvisualgt-and-ltaudiogt"></a>Esquemas: &lt;visual&gt; e &lt;audio&gt;


Nos esquemas XML a seguir, o sufixo "?" significa que o atributo é opcional.

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Launch = ?,
    Duration = ?,
    ActivationType = ?,
    Scenario = ?,
 
    Visual = new ToastVisual()
    {
        Language = ?,
        BaseUri = ?,
        AddImageQuery = ?,
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = ?,
                    Language = ?,
                    HintMaxLines = ?
                },
 
                new AdaptiveGroup()
                {
                    Children =
                    {
                        new AdaptiveSubgroup()
                        {
                            HintWeight = ?,
                            HintTextStacking = ?,
                            Children =
                            {
                                new AdaptiveText(),
                                new AdaptiveImage()
                            }
                        }
                    }
                },
 
                new AdaptiveImage()
                {
                    Source = ?,
                    AddImageQuery = ?,
                    AlternateText = ?,
                    HintCrop = ?
                }
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = ?,
        Loop = ?,
        Silent = ?
    }
};
```

**Atributos em &lt;toast&gt;**

launch?

-   launch? = string
-   Esse é um atributo opcional.
-   Uma cadeia de caracteres que é passada para o aplicativo quando ele é ativado pela notificação do sistema.
-   Dependendo do valor de activationType, esse valor pode ser recebido pelo aplicativo em primeiro plano, dentro de tarefa em segundo plano ou por outro aplicativo que é iniciado por protocolo a partir do aplicativo original.
-   O formato e o conteúdo dessa cadeia de caracteres são definidos pelo aplicativo para seu uso próprio.
-   Quando o usuário toca ou clica na notificação do sistema para iniciar o aplicativo associado, a cadeia de caracteres de inicialização fornece o contexto ao aplicativo que o permite mostrar ao usuário uma exibição relevante para o conteúdo da notificação do sistema, em vez de inicializar em sua maneira padrão.
-   Se a ativação ocorreu porque o usuário clicou em uma ação, em vez do corpo da notificação do sistema, o desenvolvedor recupera os "argumentos" predefinidos dessa marca &lt;action&gt;, em vez do "launch" predefinido na marca &lt;toast&gt;.

duration?

-   duration? = "short|long"
-   Esse é um atributo opcional. O valor padrão é "short".
-   Isso fica aqui apenas para cenários específicos e appCompat. Você não precisa mais disso para o cenário de alarme.
-   Não recomendamos o uso dessa propriedade.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Esse é um atributo opcional.
-   O valor padrão é "foreground".

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Esse é um atributo opcional, o valor padrão é "default".
-   Você não precisa disso, a menos que seu cenário seja exibir um alarme, lembrete ou chamada de entrada.
-   Não use isso apenas para manter sua notificação persistente na tela.

**Atributos em &lt;visual&gt;**

lang?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

baseUri?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

addImageQuery?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

**Atributos em &lt;binding&gt;**

template?

-   \[Important\] template? = "ToastGeneric"
-   Se você estiver usando qualquer um dos novos recursos de notificação interativa e adaptável, comece a usar o modelo "ToastGeneric" em vez do modelo herdado.
-   Usar os modelos herdados com as novas ações pode funcionar agora, mas esse não é o caso de uso esperado, e não podemos garantir que isso continuará a funcionar.

lang?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

baseUri?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

addImageQuery?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

**Atributos em &lt;text&gt;**

lang?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

**Atributos em &lt;image&gt;**

src

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230844) para obter detalhes sobre esse atributo obrigatório.

placement?

-   placement? = "inline" | "appLogoOverride"
-   Esse atributo é opcional.
-   Isso especifica onde essa imagem será exibida.
-   "inline" significa dentro do corpo da notificação do sistema, abaixo do texto; "appLogoOverride" significa substituir o ícone do aplicativo (que aparece no canto superior esquerdo da notificação do sistema).
-   Você pode ter até uma imagem para cada valor de posicionamento.

alt?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230844) para obter detalhes sobre esse atributo opcional.

addImageQuery?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230844) para obter detalhes sobre esse atributo opcional.

hint-crop?

-   hint-crop? = "none" | "circle"
-   Esse atributo é opcional.
-   "none" é o valor padrão que significa nenhum corte.
-   "circle" corta a imagem em formato circular. Use isso para imagens de perfil de um contato, imagens de uma pessoa e assim por diante.

**Atributos em &lt;audio&gt;**

src?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230842) para obter detalhes sobre esse atributo opcional.

loop?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230842) para obter detalhes sobre esse atributo opcional.

silent?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230842) para obter detalhes sobre esse atributo opcional.

## <a name="schemas-ltactiongt"></a>Esquemas: &lt;action&gt;


Nos esquemas XML a seguir, o sufixo "?" significa que o atributo é opcional.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("id")
            {
                Title = ?
                DefaultSelectionBoxItemId = ?,
                Items =
                {
                    new ToastSelectionBoxItem("id", "content")
                }
            },
 
            new ToastTextBox("id")
            {
                Title = ?,
                PlaceholderContent = ?,
                DefaultInput = ?
            }
        },
 
        Buttons =
        {
            new ToastButton("content", "args")
            {
                ActivationType = ?,
                ImageUri = ?,
                TextBoxId = ?
            },
 
            new ToastButtonSnooze("content")
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss("content")
        }
    }
};
```

**Atributos em &lt;input&gt;**

id

-   id = string
-   Esse atributo é obrigatório.
-   O atributo id é obrigatório e é usado pelos desenvolvedores para recuperar entradas do usuário depois que o aplicativo é ativado (em primeiro plano ou em segundo plano).

type

-   type = "text | selection"
-   Esse atributo é obrigatório.
-   Ele é usado para especificar uma entrada de texto ou entrada de uma lista de seleções predefinidas.
-   No celular e desktop, isso serve para especificar se você deseja uma entrada de caixa de texto ou uma entrada de caixa de listagem.

title?

-   title? = string
-   O atributo title é opcional e é para os desenvolvedores especificarem um título para a entrada para a renderização de shells quando há funcionalidade.
-   Para celular e desktop, esse título será exibido acima da entrada.

placeHolderContent?

-   placeHolderContent? = string
-   O atributo placeHolderContent é opcional e é o texto de dica cinza para o tipo de entrada de texto. Esse atributo é ignorado quando o tipo de entrada não é "text".

defaultInput?

-   defaultInput? = string
-   O atributo defaultInput é opcional e é usado para fornecer um valor de entrada padrão.
-   Se o tipo de entrada for "text", isso será tratado como uma entrada de cadeia de caracteres.
-   Se o tipo de entrada for "selection", isso deve ser a identificação de uma das seleções disponíveis dentro dos elementos dessa entrada.

**Atributos em &lt;selection&gt;**

id

-   Esse atributo é obrigatório. Ele é usado para identificar as seleções do usuário. A identificação é retornada para seu aplicativo.

content

-   Esse atributo é obrigatório. Ele fornece a cadeia de caracteres a ser exibida para esse elemento de seleção.

**Atributos em &lt;action&gt;**

content

-   content = string
-   O atributo content é obrigatório. Ele fornece a cadeia de caracteres de texto exibida no botão.

arguments

-   arguments = string
-   O atributo arguments é obrigatório. Ele descreve os dados definidos pelo aplicativo que, mais tarde, o aplicativo pode recuperar quando ele for ativado quando o usuário executar essa ação.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   O atributo activationType é opcional e seu valor padrão é "foreground".
-   Descreve o tipo de ativação que essa ação causará: em primeiro plano, em segundo plano, iniciar outro aplicativo por meio de inicialização por protocolo ou invocar uma ação do sistema.

imageUri?

-   imageUri? = string
-   imageUri é opcional e é usado para fornecer um ícone de imagem para essa ação exibir dentro do botão somente com o conteúdo de texto.

hint-inputId

-   hint-inputId = string
-   O atributo hint-inpudId é obrigatório. Ele é usado especificamente para o cenário de resposta rápida.
-   O valor deve ser que a identificação do elemento input desejado a ser associado.
-   No celular e desktop, isso colocará o botão diretamente ao lado da caixa de entrada.

## <a name="attributes-for-system-handled-actions"></a>Atributos para ações manipuladas pelo sistema


O sistema pode manipular ações para adiar e ignorar notificações se você não quiser que seu aplicativo manipular o adiamento/reagendamento de notificações como tarefa em segundo plano. As ações manipuladas pelo sistema podem ser combinadas (ou especificadas individualmente), mas não é recomendável implementar uma ação de adiamento sem uma ação de descarte.

Combinação de comandos do sistema: SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsSnoozeAndDismiss()
};
```

Ações individuais manipuladas pelo sistema

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
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
                    new ToastSelectionBoxItem("10", "10 minutes"),
                    new ToastSelectionBoxItem("20", "20 minutes"),
                    new ToastSelectionBoxItem("30", "30 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour")
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

Para construir ações individuais de adiamento e descarte, faça o seguinte:

-   Especifique activationType = "system"
-   Especifique arguments = "snooze" | "dismiss"
-   Especifique o conteúdo:
    -   Se você quiser que cadeias de caracteres localizadas de "snooze" e "dismiss" sejam exibidas nas ações, especifique o conteúdo para ser uma cadeia de caracteres vazia: &lt;action content = ""/&gt;
    -   Se você quiser uma cadeia de caracteres personalizada, basta fornecer seu valor: &lt;action content="Lembrar-me mais tarde" /&gt;
-   Especifica a entrada:
    -   Se você não quiser que o usuário selecione um intervalo de adiamento e, em vez disso, apenas que sua notificação seja adiada apenas uma vez por um intervalo de tempo definido pelo sistema (consistente com o sistema operacional), não crie nenhuma &lt;input&gt;.
    -   Se você quiser fornecer seleções de intervalo de adiamento:
        -   Especifique hint-inputId na ação snooze
        -   Combine a identificação da entrada com a hint-inputId da ação snooze: &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;<action hint-inputId="snoozeTime"/&gt;
        -   Especifique a identificação da seleção para ser um nonNegativeInteger que represente o intervalo de adiamento em minutos: &lt;selection id="240" /&gt; significa adiar por 4 horas
        -   Certifique-se de que o valor de defaultInput em &lt;input&gt; corresponda a uma das identificações dos elementos &lt;selection&gt; filho
        -   Forneça até (mas não mais que) 5 valores de &lt;selection&gt;

 

 
## <a name="related-topics"></a>Tópicos relacionados

* [Guia de início rápido: enviar uma notificação do sistema local e manipular a ativação](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [Biblioteca de notificações no GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)
