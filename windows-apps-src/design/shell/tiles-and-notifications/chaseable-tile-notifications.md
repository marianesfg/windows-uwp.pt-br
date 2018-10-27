---
author: andrewleader
Description: Use chaseable tile notifications to find out what your app displayed on its Live Tile when the user clicked it.
title: Notificações de blocos rastreáveis
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, uwp, blocos rastreáveis, blocos dinâmicos, notificações de bloco rastreáveis
ms.localizationpriority: medium
ms.openlocfilehash: 8126755dfb6f5f0e117d10daef85a83e8a171f1f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703346"
---
# <a name="chaseable-tile-notifications"></a>Notificações de blocos rastreáveis

As notificações de bloco rastreáveis permitem que você determine quais notificações de bloco o Bloco Dinâmico do app estava exibindo quando o usuário clicou no bloco.  
Por exemplo, um app de notícias pode usar esse recurso para determinar qual notícia o Bloco Dinâmico estava exibindo quando o usuário o inicializou; isso pode garantir que a notícia seja exibida com destaque para que o usuário consiga encontrá-la. 

> [!IMPORTANT]
> **Requer a Atualização de Aniversário**: para usar notificações de bloco rastreáveis com C#, C++ ou aplicativos UWP com base em VB, você deve usar o SDK 14393 e executar o build 14393 ou posterior. Para aplicativos UWP com base em JavaScript, você deve usar o SDK 17134 e executar novamente o build 17134 ou posterior. 


> **APIs importantes**: [propriedade LaunchActivatedEventArgs.TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [classe TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Como funciona

Para habilitar notificações de bloco rastreáveis, use a propriedade **Arguments** na carga de notificação de bloco, assim como acontece na propriedade launch na carga de notificação do sistema, para inserir informações sobre o conteúdo na notificação de bloco.

Quando o app for iniciado por meio do Bloco Dinâmico, o sistema retornará uma lista de argumentos nas notificações de bloco atuais/exibidas recentemente.


## <a name="when-to-use-chaseable-tile-notifications"></a>Quando usar notificações de bloco rastreáveis

As notificações de bloco rastreáveis são normalmente usadas quando você estiver usando a fila de notificação no Bloco Dinâmico (o que significa que você está percorrendo até cinco notificações diferentes). Elas também serão úteis quando o conteúdo do Bloco Dinâmico estiver potencialmente fora de sincronia com o conteúdo mais recente do app. Por exemplo, o app Notícias atualiza o Bloco Dinâmico a cada 30 minutos, mas, quando o app é iniciado, ele carrega as últimas notícias (que pode não incluir algo que estava no bloco a partir do último intervalo de sondagem). Quando isso acontece, o usuário pode ficar frustrado por não conseguir encontrar a notícia que leu no Bloco Dinâmico. É aqui que as notificações de bloco rastreáveis podem ajudar, permitindo que você se certifique de que o que o usuário leu no Bloco pode ser facilmente descoberto.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>O que fazer com uma notificação de bloco rastreável

O mais importante a ser observado é que, na maioria dos cenários, **você NÃO deve navegar diretamente para a notificação específica** que estava no Bloco quando o usuário clicou nele. O Bloco Dinâmico é usado como um ponto de entrada para seu aplicativo. Dois cenários são possíveis quando um usuário clica no Bloco Dinâmico: (1) ele queria iniciar o app normalmente ou (2) queria ver mais informações sobre uma notificação específica que estava no Bloco Dinâmico. Como não há nenhuma maneira de o usuário dizer explicitamente qual comportamento ele deseja, a experiência ideal é **iniciar o app normalmente, certificando-se de que a notificação vista pelo usuário pode ser descoberta facilmente**.

Por exemplo, clicar no Bloco Dinâmico do app MSN Notícias iniciará o app normalmente: ele exibirá a home page ou qualquer artigo que o usuário tenha lido anteriormente. No entanto, na home page, o app garante que a notícia lida no Bloco Dinâmico poderá ser facilmente descoberta. Dessa forma, os dois cenários têm suporte: o cenário em que você simplesmente deseja a inicialização/retomada do app e o cenário em que você deseja exibir a notícia específica.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Como incluir a propriedade Arguments na carga de notificação do bloco

Em uma carga de notificação, a propriedade arguments permite que o app forneça dados que você pode usar para identificar a notificação posteriormente. Por exemplo, seu argumentos podem incluir a ID da notícia, para que, quando iniciada, você possa recuperá-la e exibi-la. A propriedade aceita uma cadeia de caracteres, que pode ser serializada da forma desejada (cadeia de caracteres de consulta, JSON etc.), mas normalmente recomendamos a cadeia de caracteres de consulta, pois ela é leve e é perfeitamente codificada em XML.

A propriedade pode ser definida nos dois elementos, **TileVisual** e **TileBinding**, e será cascateada. Se você quiser os mesmos argumentos em cada tamanho de bloco, basta definir os argumentos no elemento **TileVisual**. Se você precisar de argumentos específicos para tamanhos de bloco específicos, defina os argumentos em elementos individuais **TileBinding**.

Este exemplo cria uma carga de notificação que usa a propriedade arguments para que a notificação pode ser identificada mais tarde. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>Como procurar a propriedade arguments quando o app for iniciado

A maioria dos apps tem um arquivo App.xaml.cs, que contém uma substituição para o método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_). Como o nome sugere, o app chama esse método quando ele é iniciado. Ele precisa de um único argumento, um objeto [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs).

O objeto LaunchActivatedEventArgs tem uma propriedade que habilita notificações rastreáveis: a [propriedade TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), que fornece acesso a um [objeto TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo). Quando o usuário inicia o app de seu bloco (e não a lista de apps, a pesquisa ou qualquer outro ponto de entrada), o app inicializa essa propriedade.

O [objeto TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) contém uma propriedade chamada [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), que contém uma lista de notificações exibida no bloco nos últimos 15 minutos. O primeiro item na lista representa a notificação atual do bloco, enquanto os itens subsequentes representam as notificações que o usuário viu antes da notificação atual. Se o bloco foi limpo, essa lista estará vazia.

Cada um Argumentsproperty ShownTileNotificationhas. O Argumentsproperty será inicializado com o argumentsstring de carga de notificação do bloco ou nulo se a carga não incluiu o argumentsstring.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


## <a name="raw-xml-example"></a>Exemplo de XML bruto

Se você estiver usando XML bruto, em vez da biblioteca de notificações, este será o XML.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>Artigos relacionados

- [Propriedade LaunchActivatedEventArgs.TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [Classe TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)