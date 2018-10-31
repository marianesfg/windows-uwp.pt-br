---
author: andrewleader
Description: Learn how to pin a secondary tile from your UWP app.
title: Fixar blocos secundários
label: Pin secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, uwp, blocos secundários, fixar, fixando, guia de início rápido, exemplo de código, exemplo, secondarytile
ms.localizationpriority: medium
ms.openlocfilehash: 7fcea65a43ec3ca3d7e29056bec129e0f03d4229
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5840821"
---
# <a name="pin-secondary-tiles"></a>Fixar blocos secundários


Este tópico o orienta pelas etapas de criação de um bloco secundário para um aplicativo UWP e de sua fixação no menu Iniciar.

![Captura de tela de blocos secundários](images/secondarytiles.png)

Para saber mais sobre blocos secundários, consulte a [Visão geral de blocos secundários](secondary-tiles.md).


## <a name="add-namespace"></a>Adicionar namespace

O namespace Windows.UI.StartScreen inclui a classe SecondaryTile.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Inicializar o bloco secundário

Blocos secundários são compostos de alguns componentes principais...

* **TileId**: um identificador exclusivo que permite identificar o bloco entre outros blocos secundários.
* **DisplayName**: o nome que você deseja mostrar no bloco.
* **Arguments**: os argumentos que você deseja que sejam passados novamente ao aplicativo quando o usuário clicar no bloco.
* **Square150x150Logo**: o logotipo obrigatório, exibido no bloco médio (e redimensionado para o bloco pequeno se nenhum logotipo pequeno for fornecido).

Você **DEVE** fornecer valores inicializados para todas as propriedades acima, caso contrário, obterá uma exceção.

Há uma variedade de construtores que podem ser usados, mas usar aquele que inclui tileId, displayName, arguments, square150x150Logo e desiredSize ajuda a garantir que você definirá todas as propriedades necessárias.

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>Opcional: adicionar suporte a blocos de tamanhos maiores

Se você for exibir notificações de bloco avançadas no bloco secundário, permita que o usuário redimensione a largura e o tamanho do bloco para que ele possa ver ainda mais o conteúdo.

Para habilitar bloco largos e grandes, é necessário fornecer *Wide310x150Logo* e *Square310x310Logo*. Além disso, se possível, você deve fornecer *Square71x71Logo* para o bloco pequeno (caso contrário, vamos diminuir o tamanho do Square150x150Logo necessário para o bloco pequeno).

Você também pode fornecer um *Square44x44Logo* exclusivo, que, opcionalmente, é exibido no canto inferior direito quando uma notificação está presente. Se você não fornecer um, o *Square44x44Logo* do bloco primário será usado.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Opcional: habilitar a apresentação do nome de exibição

Por padrão, o nome de exibição NÃO será mostrado. Para mostrar o nome de exibição em um bloco médio/largo/grande, adicione o código a seguir.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Opcional: blocos secundários em 3D
Você pode aprimorar o bloco secundário do Windows Mixed Reality adicionando recursos 3D. Os usuários podem colocar os blocos 3D diretamente a página inicial do Windows Mixed Reality em vez do menu Iniciar ao usar seu aplicativo em um ambiente de realidade misturada. Por exemplo, você pode criar fotoesferas 360° associadas diretamente a um aplicativo do visualizador de fotos 360° ou permitir que os usuários coloquem um modelo 3D de uma cadeira de um catálogo de móveis que abre uma página de detalhes sobre as opções de preço e cores do objeto quando selecionado. Para começar, consulte a [documentação do desenvolvedor de Realidade Misturada](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Fixar o bloco secundário

Por fim, solicite para fixar o bloco. Observe que deve ser chamado de um thread de interface do usuário. Na área de trabalho, uma caixa de diálogo será exibida solicitando que o usuário confirme se gostaria de fixar o bloco.

> [!IMPORTANT]
> No caso de um aplicativo da área de trabalho do Windows usando a Ponte de Desktop, primeiro você deve executar uma etapa extra conforme descrito em [Fixar do aplicativo da área de trabalho](secondary-tiles-desktop-pinning.md)

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Verificar se existe um bloco secundário

Se o usuário visitar uma página no aplicativo que já tenha sido fixada em Iniciar, exiba um botão "Desafixar".

Portanto, ao escolher qual botão será exibido, é necessário verificar primeiro se o bloco secundário está fixado no momento.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Desafixando um bloco secundário

Se o bloco estiver fixado e o usuário clicar no botão Desafixar, você vai querer desafixar (excluir) o bloco.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Atualizando um bloco secundário

Se for necessário atualizar os logotipos, o nome de exibição ou outro item no bloco secundário, use *RequestUpdateAsync*.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Enumerando todos os blocos secundários fixados

Se for necessário descobrir todos os blocos que o usuário fixou, em vez de usar *SecondaryTile.Exists*, use *SecondaryTile.FindAllAsync()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Enviar uma notificação de bloco

Para saber como exibir conteúdo sofisticado no bloco por meio de notificações de bloco, consulte [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md).


## <a name="related"></a>Relacionado

* [Visão geral de blocos secundários](secondary-tiles.md)
* [Diretriz de blocos secundários](secondary-tiles-guidance.md)
* [Ativos de bloco](app-assets.md)
* [Documentação sobre conteúdo de blocos](create-adaptive-tiles.md)
* [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md)
