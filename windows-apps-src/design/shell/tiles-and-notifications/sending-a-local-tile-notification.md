---
author: andrewleader
Description: This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates.
title: Enviar uma notificação de bloco local
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7e91d4bd481188f4d29af68af2c4572b26d446ae
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4131203"
---
# <a name="send-a-local-tile-notification"></a>Enviar uma notificação de bloco local
 

Os blocos dos aplicativos primários no Windows 10 são definidos no manifesto do aplicativo, e os blocos secundários são criados e definidos programaticamente pelo código do aplicativo. Este artigo descreve como enviar uma notificação de bloco local para um bloco primário e um bloco secundário usando modelos de bloco adaptável. (Notificação local é aquela que é enviada pelo código do app, e não uma enviada por push ou obtida em um servidor Web.)

![bloco padrão e bloco com notificação](images/sending-local-tile-01.png)

> [!NOTE] 
>Saiba mais sobre [como criar blocos adaptáveis](create-adaptive-tiles.md) e [esquema de conteúdo de bloco](../tiles-and-notifications/tile-schema.md).

 

## <a name="install-the-nuget-package"></a>Instalar o pacote NuGet


Recomendamos instalar o [Pacote NuGet Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), que simplifica as coisas pois gera cargas de bloco com objetos, em vez de XML bruto.

Os exemplos de código embutido neste artigo são para a linguagem C# usando a biblioteca de notificações. (Caso prefira criar o próprio XML, você pode encontrar exemplos de código sem o uso da biblioteca de notificações ao final do artigo.)

## <a name="add-namespace-declarations"></a>Adicionar declarações de namespace


Para acessar as APIs de bloco, inclua o namespace [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications). Também recomendamos incluir o namespace **Microsoft.Toolkit.Uwp.Notifications** de maneira que seja possível usufruir de nossas APIs auxiliares de bloco (você deve instalar o pacote NuGet [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para acessar essas APIs).

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>Criar o conteúdo da notificação


No Windows 10, as cargas de bloco são definidas usando-se modelos de bloco adaptável, que permitem criar layouts visuais personalizados para as notificações. (Para saber o que é possível com blocos adaptáveis, consulte [Criar blocos adaptáveis](create-adaptive-tiles.md).)

Este exemplo de código cria conteúdo de bloco adaptável para blocos médios e largos.

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },

        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

O conteúdo da notificação se parece com o seguinte quando exibido em um bloco médio:

![conteúdo de notificação em um bloco médio](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>Criar a notificação


Depois de ter o conteúdo da notificação, você precisará criar um novo [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification). O construtor **TileNotification** utiliza um objeto [**XmlDocument**](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument) do Windows Runtime, que você pode obter do método **TileContent.GetXml** caso você esteja usando a [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

Este exemplo de código cria uma notificação para um novo bloco.

```csharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>Definir um prazo de expiração para a notificação (opcional)


Por padrão, as notificações locais de bloco e selo não expiram, enquanto as notificações por push, periódicas e agendadas expiram após três dias. Como o conteúdo do bloco não deve persistir por mais tempo do que o necessário, é melhor prática definir um prazo de expiração que faça sentido para o aplicativo, especialmente em notificações de bloco e selo locais.

Este exemplo de código cria uma notificação que expira e será removida do bloco após dez minutos.

```csharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>Enviar a notificação


Embora o envio local de uma notificação de bloco seja simples, enviar a notificação para um bloco primário ou secundário é um pouco diferente.

**Bloco primário**

Para enviar uma notificação a um bloco primário, use o [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager) para criar um atualizador de bloco para o bloco primário e envie a notificação chamando "Update". Independentemente de estar visível, o bloco primário do aplicativo sempre existe, logo, é possível enviar notificações para ele, mesmo quando não está fixado. Se o usuário fixar o bloco primário posteriormente, as notificações que você enviou serão exibidas depois.

Este exemplo de código envia uma notificação para um bloco primário.


```csharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Bloco secundário**

Para enviar uma notificação para um bloco secundário, primeiro assegure-se de que o bloco secundário exista. Se você tentar criar um atualizador de bloco para um bloco secundário que não existe (por exemplo, se o usuário desafixou o bloco secundário), uma exceção será acionada. É possível usar [**SecondaryTile.Exists**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile#Windows_UI_StartScreen_SecondaryTile_Exists_System_String_) (tileId) para descobrir se o bloco secundário está fixado, criar um atualizador para o bloco secundário e enviar a notificação.

Este exemplo de código envia uma notificação para um bloco secundário.

```csharp
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![bloco padrão e bloco com notificação](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>Limpar notificações no bloco (opcional)


Na maioria dos casos, você deverá limpar uma notificação assim que o usuário tiver interagido com esse conteúdo. Por exemplo, quando o usuário inicia o aplicativo, convém limpar todas as notificações do bloco. Caso as notificações estejam associadas ao prazo, recomendamos definir um prazo de expiração na notificação, em vez de limpar explicitamente a notificação.

Este exemplo de código limpa a notificação de bloco para o bloco primário. Você pode fazer o mesmo para os blocos secundários. Basta criar um atualizador de bloco para o bloco secundário.

```csharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

Em um bloco com a fila de notificação habilitada e notificações na fila, chamar o método Clear esvazia a fila. No entanto, você não pode limpar uma notificação por meio do servidor de aplicativos; somente o código do aplicativo local pode limpar notificações.

Notificações periódicas ou enviadas por push só podem adicionar novas notificações ou substituir notificações existentes. Uma chamada local para o método Clear limpará o bloco independentemente das notificações propriamente ditas chegarem via push, serem periódicas ou locais. Notificações agendadas que ainda não foram exibidas não são apagadas por esse método.

![bloco com notificação e bloco após ser limpo](images/sending-local-tile-03.png)

## <a name="next-steps"></a>Próximas etapas


**Como usar a fila de notificações**

Agora que concluiu a primeira atualização do bloco, você pode expandir a funcionalidade do título habilitando uma [fila de notificação](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234).

**Outros métodos de entrega da notificação**

Este artigo mostra como enviar a atualização do bloco como uma notificação. Para explorar outros métodos de entrega da notificação, inclusive agendadas, periódicas e enviadas por push, consulte [Entrega de notificações](choosing-a-notification-delivery-method.md).

**Método de entrega XmlEncode**

Caso você não esteja usando a [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), esse método de entrega de notificação oferece outra alternativa.


```csharp
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <a name="code-examples-without-notifications-library"></a>Exemplos de código sem a Biblioteca de notificações


Caso você prefira trabalhar com o XML bruto, em vez do pacote NuGet [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), use esses exemplos de código alternativos para os três primeiros exemplos fornecidos neste artigo. O restante dos exemplos de código podem ser usados com a [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) ou com o XML bruto.

Adicionar declarações de namespace

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Criar o conteúdo da notificação

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template='TileMedium'>
            <text>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
        <binding template='TileWide'>
            <text hint-style='subtitle'>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

Criar a notificação

```csharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>Tópicos relacionados

* [Criar blocos adaptáveis](create-adaptive-tiles.md)
* [Esquema de conteúdo do bloco](../tiles-and-notifications/tile-schema.md)
* [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Namespace Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)
* [Como usar a fila de notificações (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Entrega de notificações](choosing-a-notification-delivery-method.md)
 

 




