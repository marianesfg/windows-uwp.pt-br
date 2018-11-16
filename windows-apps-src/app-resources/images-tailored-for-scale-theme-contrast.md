---
author: stevewhims
Description: Your app can load image resource files containing images tailored for display scale factor, theme, high contrast, and other runtime contexts.
title: Carregar imagens e ativos personalizados para escala, tema, alto contraste e outros
template: detail.hbs
ms.author: stwhi
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 4db96cea273348b4e1bc7059446f7528ba30a645
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981066"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Carregar imagens e ativos personalizados para escala, tema, alto contraste e outros
O app pode carregar arquivos de recurso de imagem (ou outros arquivos de ativo) personalizados para [fator de escala de exibição](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), tema, alto contraste e outros contextos de tempo de execução. Essas imagens podem ser referenciadas no código imperativo ou na marcação XAML, por exemplo, como a propriedade **Source** de uma **Imagem**. Elas também podem aparecer no arquivo de origem do manifesto do pacote de aplicativos (o arquivo `Package.appxmanifest`), por exemplo, como o valor do ícone de aplicativo na guia Ativos Visuais do Designer de Manifesto do Visual Studio, ou em seus blocos e notificações do sistema. Usando qualificadores nos nomes de arquivo das imagens e, opcionalmente, carregando-os dinamicamente com a ajuda de um [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live), você pode fazer com que o arquivo de imagem que melhor corresponde às configurações de tempo de execução do usuário para escala de exibição, tema, alto contraste, idiomas e outros contextos seja carregado.

Um recurso de imagem está contido em um arquivo de recurso de imagem. Você também pode considerar a imagem como um ativo e o arquivo que o contém como um arquivo de ativo; esses tipos de arquivos de recurso podem ser encontrados na pasta \Assets do projeto. Para informações sobre como usar os qualificadores nos nomes de seus arquivos de recurso de imagem, consulte [Ajustar seus recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md).

Alguns qualificadores comuns para imagens são [scale](tailor-resources-lang-scale-contrast.md#scale), [theme](tailor-resources-lang-scale-contrast.md#theme), [contrast](tailor-resources-lang-scale-contrast.md#contrast) e [targetsize](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Qualificar um recurso de imagem para escala, tema e contraste
O valor padrão do qualificador `scale` é `scale-100`. Portanto, essas duas variantes são equivalentes (elas fornecem uma imagem em escala 100 ou fator de escala 1).

```
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
```

Você pode usar qualificadores nos nomes de pasta, e não nos nomes de arquivo. Essa será uma estratégia melhor se você tiver vários arquivos de ativo por qualificador. Para fins de ilustração, essas duas variantes são equivalentes às duas acima.

```
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
```

Veja a seguir um exemplo de como você pode fornecer variantes de um recurso de imagem, &mdash;denominado `/Assets/Images/logo.png`&mdash;, para as diferentes configurações de escala de exibição, tema e alto contraste. Este exemplo usa a pasta de nomenclatura.

```
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
```

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Fazer referência a uma imagem ou outro ativo no código e na marcação XAML
O nome &mdash;ou identificador&mdash; de um recurso de imagem é seu caminho e nome de arquivo com todos os qualificadores removidos. Se você nomear pastas e/ou arquivos como qualquer um dos exemplos na seção anterior, terá um recurso de imagem único e seu nome (como um caminho absoluto) será `/Assets/Images/logo.png`. Veja como usar esse nome na marcação XAML.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Observe que você usa o esquema de URI `ms-appx` porque está fazendo referência a um arquivo que vem do pacote do app. Consulte [Esquemas de URI](uri-schemes.md). Veja também como fazer referência ao mesmo recurso de imagem no código imperativo.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Você pode usar `ms-appx` para carregar qualquer arquivo arbitrário do pacote do app.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

O esquema `ms-appx-web` acessa os mesmos arquivos que `ms-appx`, mas no compartimento da Web.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Para qualquer um dos cenários mostrados nestes exemplos, use a sobrecarga do [construtor Uri](https://docs.microsoft.com/en-us/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) que infere o [UriKind](https://docs.microsoft.com/en-us/dotnet/api/system.urikind). Especifique um URI absoluto válido, incluindo o esquema e a autoridade, ou deixe a autoridade assumir o conjunto de aplicativo como valor padrão no exemplo acima.

Observe como, nestes URIs de exemplo, o esquema ("`ms-appx`" ou "`ms-appx-web`") é seguido por "`://`", que é acompanhado por um caminho absoluto. Em um caminho absoluto, o símbolo "`/`" à esquerda faz com que o caminho seja interpretado da raiz do pacote.

**Observação** Os esquemas URI `ms-resource` (para [recursos de cadeia de caracteres](localize-strings-ui-manifest.md)) e `ms-appx(-web)` (para imagens e outros ativos) fazem a correspondência automática de qualificador para localizar o recurso mais apropriado para o contexto atual. O esquema URI `ms-appdata` (que é usado para carregar dados de app) não executa nenhuma correspondência automática, mas você pode responder ao conteúdo de [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) e carregar explicitamente os ativos apropriados a partir dos dados de app usando seu nome de arquivo físico completo no URI. Para obter mais informações sobre dados de aplicativo, consulte [Armazenar e recuperar configurações e outros dados de app](../design/app-settings/store-and-retrieve-app-data.md). Esquemas URI da Web (por exemplo, `http`, `https` e `ftp`) não executam quaisquer correspondências automáticas. Para obter informações sobre o que fazer nesse caso, consulte [Hospedando e carregando imagens na nuvem](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Os caminhos absolutos serão uma boa opção se os arquivos de imagem permanecerem onde estão na estrutura do projeto. Se você deseja mover um arquivo de imagem, mas deseja mantê-lo no mesmo local em relação ao seu arquivo de marcação XAML de referência, use um caminho relativo ao arquivo de marcação que o contêm, em vez de usar um caminho absoluto. Se você fizer isso, não precisará usar um esquema de URI. Você ainda tirará proveito da correspondência automática de qualificador nesse caso, mas apenas porque está usando o caminho relativo na marcação XAML.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Consulte também [Suporte a bloco e notificações do sistema para idioma, escala e alto contraste](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Qualifica um recurso de imagem para targetsize
Você pode usar os qualificadores `scale` e `targetsize` em variantes diferentes do mesmo recurso de imagem, mas não poderá usar os dois em uma única variante de um recurso. Além disso, você precisa definir pelo menos uma variante sem um qualificador `TargetSize`. Essa variante deve definir um valor para `scale` ou deixá-la assumir `scale-100` como valor padrão. Portanto, estas duas variantes do recurso `/Assets/Square44x44Logo.png` são válidas.

```
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
```

E essas duas variantes são válidas. 

```
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
```

Mas essa variante não é válida.

```
\Assets\Square44x44Logo.scale-200_targetsize-24.png
```

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Consulte um arquivo de imagem do manifesto do conjunto de aplicativo
Se você nomear pastas e/ou arquivos como em qualquer um dos dois exemplos válidos da seção anterior, terá um único recurso de imagem de ícone de app e seu nome (como caminho relativo) será `Assets\Square44x44Logo.png`. No manifesto do conjunto de aplicativo, basta fazer referência ao recurso pelo nome. Não é necessário para usar qualquer esquema de URI.

![adicionar recurso, inglês](images/app-icon.png)

Isso é tudo o que você precisa fazer; o sistema operacional fará a correspondência automática de qualificador para localizar o recurso mais apropriado para o contexto atual. Para obter uma lista de todos os itens no manifesto do conjunto de aplicativo que você pode localizar ou se qualificar dessa maneira, consulte [Itens de manifesto localizáveis](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Qualifica um recurso de imagem para layoutdirection
Consulte [Espelhando imagens](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Carregar uma imagem para um idioma ou outro contexto
Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md).

O [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) padrão (obtido em [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contém um valor de qualificador para cada nome de qualificador, representando o contexto de tempo de execução padrão (em outras palavras, as configurações do usuário atual e da máquina). Os arquivos de imagem são comparados&mdash;, com base nos qualificadores contidos em seus nomes&mdash;, com os valores de qualificador nesse contexto de tempo de execução.

Mas, pode haver momentos em que seu app deverá substituir as configurações do sistema e ser explícito sobre o idioma, a escala ou outro valor de qualificador a ser usado durante a procura de uma imagem correspondente a ser carregada. Por exemplo, convém controlar exatamente quando as imagens de alto contraste serão carregadas e quais serão carregadas.

Você pode fazer isso criando um novo **ResourceContext**, em vez de usar o padrão, substituindo seus valores e, em seguida, usando esse objeto de contexto em pesquisas de imagem.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

Para obter o mesmo efeito em um nível global, você *pode* substituir os valores de qualificador no **ResourceContext** padrão. Mas, em vez disso, recomendamos que você chame [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Você define valores de uma só vez com uma chamada para **SetGlobalQualifierValue**, e esses valores estarão em vigor no **ResourceContext** padrão cada vez que você usá-lo nas pesquisas. Por padrão, a classe [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) usa o **ResourceContext** padrão.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Atualizando imagens em resposta aos eventos de alteração de valor de qualificador
O app em execução pode responder às alterações feitas nas configurações do sistema que afetam os valores de qualificador no contexto de recurso padrão. Qualquer uma dessas configurações de sistema invoca o evento [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) em [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

Em resposta a esse evento, você pode recarregar suas imagens com a ajuda do **ResourceContext** padrão, que o [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) usa por padrão.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>APIs importantes
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Tópicos relacionados
* [Personalizar os recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md)
* [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md)
* [Armazenar e recuperar configurações e outros dados de aplicativo](../design/app-settings/store-and-retrieve-app-data.md)
* [Suporte ao bloco e notificações do sistema para o idioma, escala e alto contraste](tile-toast-language-scale-contrast.md)
* [Itens de manifesto localizáveis](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Espelhando imagens](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Globalização e localização](../design/globalizing/globalizing-portal.md)
