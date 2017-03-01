---
author: DelfCo
description: "Recupere ou crie o conteúdo da Web mais atual e popular usando feeds sindicalizados gerados de acordo com os padrões RSS e Atom, usando os recursos no namespace Windows.Web.Syndication."
title: Feeds RSS/Atom
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: da28caa7703a04b1d11254d9c894cf02b044ab64
ms.lasthandoff: 02/07/2017

---

# <a name="rssatom-feeds"></a>Feeds RSS/Atom

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)
-   [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)
-   [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)

Recupere ou crie o conteúdo da Web mais atual e popular usando feeds sindicalizados gerados de acordo com os padrões RSS e Atom, usando os recursos no namespace [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632).

## <a name="what-is-a-feed"></a>O que é um feed?

Um web feed é um documento que contém qualquer número de entradas individuais compostas de texto, links e imagens. As atualizações feitas em um feed são na forma de novas entradas usadas para promover o conteúdo mais atualizado na Web. Os consumidores de conteúdo podem usar um aplicativo de leitura de feed para agregar e monitorar feeds de qualquer número de autores de conteúdo individual. Com isso, eles ganham ao conteúdo mais atualizado de forma rápida e conveniente.

## <a name="which-feed-format-standards-are-supported"></a>Que padrões de formato de feed são compatíveis?

A UWP (Plataforma Universal do Windows) dá suporte à recuperação de feed nos padrões de formato RSS, de 0.91 a RSS 2.0, e nos padrões Atom, de 0.3 a 1.0. As classes no namespace [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) podem definir feeds e itens de feed capazes de representar elementos de RSS e Atom.

Adicionalmente, os formatos Atom 1.0 e RSS 2.0 permitem que seus documentos de feed contenham elementos ou atributos não definidos nas especificações oficiais. Ao longo do tempo, esses elementos e atributos personalizados se tornaram uma forma de definir informações específicas de domínios consumidas por outros formatos de dados de serviços Web, como GData e OData. Para ter suporte desse recurso adicional, a classe [**SyndicationNode**](https://msdn.microsoft.com/library/windows/apps/br243585) representa elementos XML genéricos. O uso de **SyndicationNode** com classes no namespace [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819) permite que os aplicativos acessem atributos, extensões e qualquer conteúdo contido neles.

Observe que, para a publicação de conteúdo sindicalizado, a implementação da UWP do Protocolo Atom Publication ([**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)) só é compatível com operações de conteúdo de feed de acordo com os padrões Atom e Atom Publication.

## <a name="using-syndicated-content-with-network-isolation"></a>Usando conteúdo sindicalizado com isolamento de rede

O recurso de isolamento de rede na UWP permite que o desenvolvedor controle e limite o acesso à rede por meio de um aplicativo UWP. Nem todos os aplicativos exigem acesso à rede. Porém, para os aplicativos que fazem isso, a UWP oferece diferentes níveis de acesso à rede que podem ser ativados selecionando recursos adequados.

O isolamento de rede permite que um desenvolvedor defina o escopo necessário de acesso à rede para cada aplicativo. Um aplicativo sem o escopo apropriado definido não consegue acessar o tipo especificado de rede e o tipo específico de solicitação de rede (solicitações de saída iniciadas pelo cliente ou solicitações de entrada não solicitadas e de saída iniciadas pelo cliente). A capacidade de definir e impor o isolamento de rede garante que, se um aplicativo não ficar comprometido, ele só poderá acessar as redes às quais o aplicativo recebeu acesso explicitamente. Isto reduz significativamente o âmbito do impacto sobre outros aplicativos e sobre o Windows.

O isolamento de rede afeta todos os elementos de classe nos namespaces [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) e [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609) que tentem acessar a rede. O Windows impõe ativamente o isolamento de rede. Uma chamada a um elemento de classe no namespace **Windows.Web.Syndication** ou **Windows.Web.AtomPub** que resulta em acesso à rede pode falhar por causa do isolamento de rede se o recurso de rede adequado não for ativado.

Os recursos de rede para um aplicativo são configurados no manifesto do aplicativo quando o aplicativo é compilado. Geralmente, os recursos de rede são adicionados usando o Microsoft Visual Studio 2015 ao desenvolver o aplicativo. Os recursos de rede também podem ser definidos manualmente no arquivo manifesto do aplicativo usando um editor de texto.

Para saber mais sobre o isolamento de rede e as funcionalidades de rede, consulte a seção "Funcionalidades" no tópico [Noções básicas de rede](networking-basics.md).

## <a name="how-to-access-a-web-feed"></a>Como acessar um web feed

Esta seção mostra como recuperar e exibir um feed da Web usando classes no namespace [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) em seu aplicativo UWP escrito em C# ou Javascript.

**Pré-requisitos**

Para garantir que o aplicativo UWP esteja pronto para a rede, defina alguns recursos de rede necessários no arquivo **Package.appxmanifest** do projeto. Se o aplicativo precisa ser capaz de se conectar como um cliente aos serviços remotos na Internet, a funcionalidade **internetClient** é necessária. Para obter mais informações, consulte a seção "Funcionalidades" no tópico [Noções básicas de rede](networking-basics.md).

**Recuperando conteúdo sindicalizado de um web feed**

Agora iremos revisar alguns códigos que demonstram como recuperar um feed e, depois, exibir cada item individual que o feed contém. Antes de configurar e enviar a solicitação, iremos definir algumas variáveis que usaremos durante a operação, e inicializar uma instância de [**SyndicationClient**](https://msdn.microsoft.com/library/windows/apps/br243456), que define os métodos e propriedades que usaremos para recuperar e exibir o feed.

O construtor [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) lança uma exceção se o *uriString* passado para o construtor não for um URI válido. Portanto, validamos o *uriString* usando um bloco try/catch.

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;
// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;
// Use your own uriString for the feed you are connecting to.
string uriString = "";
try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
var client = new Windows.Web.Syndication.SyndicationClient();
// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

Em seguida, configuraremos a solicitação definindo as credenciais do Servidor (a propriedade [**ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461)), as credenciais do proxy (a propriedade [**ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459)) e os cabeçalhos HTTP (o método [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br243462)) necessários. Com os parâmetros de solicitação básicos configurados, um objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) válido, criado usando uma cadeia de caracteres URI do feed fornecida pelo aplicativo. O objeto **Uri** é então passado para a função [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) para solicitar o feed.

Pressupondo que o conteúdo de feed desejado foi retornado, o exemplo de código itera cada item do feed, chamando **displayCurrentItem** (que definiremos em seguida), para exibir itens e seu conteúdo como uma lista na IU.

É necessário escrever um código para tratar exceções quando você chama a maioria dos métodos de rede assíncronos. Seu manipulador de exceção recupera informações mais detalhadas sobre a causa da exceção para entender melhor a falha e tomar as medidas adequadas.

O método [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) lança uma exceção se uma conexão não puder ser estabelecida com o servidor HTTP ou o objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) não apontar para um AtomPub ou RSS feed válido. O código de exemplo Javascript usa uma função **onError** para capturar quaisquer exceções e imprime mais informações detalhadas sobre a exceção, se ocorrer um erro.

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    feed = await client.RetrieveFeedAsync(uri);
    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;
    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");
    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}
// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;
        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");
        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;
        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }
        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

Na etapa anterior, [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) retornou o conteúdo de feed solicitado e o código de exemplo teve que trabalhar iterando através dos itens de feed disponíveis. Cada um desses itens é representado através de um objeto [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) que contém todas as propriedades e conteúdo do item suportado pelo padrão de sindicalização relevante (RSS ou Atom). No exemplo a seguir, observamos a função **displayCurrentItem** trabalhando em cada item e exibindo seu conteúdo através de vários elementos de IU indicados.

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.
```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];
    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;
    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;
    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }
    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;
    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

Como sugerido anteriormente, o tipo de conteúdo representado por um objeto [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) diferirá dependendo do padrão de feed (RSS ou Atom) empregado para publicar o feed. Por exemplo, um feed Atom é capaz de fornecer uma lista de [**Contributors**](https://msdn.microsoft.com/library/windows/apps/br243540), mas um feed RSS não é. No entanto, elementos de extensão incluídos em um feed que não são aceitos por nenhum padrão (por exemplo, elementos de extensão Dublin Core) podem ser acessados com a propriedade [**SyndicationItem.ElementExtensions**](https://msdn.microsoft.com/library/windows/apps/br243543) e, depois, exibidos como demonstrado no seguinte exemplo de código.

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";
    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;
        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }
    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }
    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource
    });
}
```

