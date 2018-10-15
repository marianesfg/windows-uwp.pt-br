---
author: stevewhims
Description: If you want your app to support different display languages, and you have string literals in your code or XAML markup or app package manifest, then move those strings into a Resources File (.resw). You can then make a translated copy of that Resources File for each language that your app supports.
title: Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote de aplicativos
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.author: stwhi
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: c9db9f3ce4397bec6fb0b6b339875c206d17c3fd
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4611701"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote de aplicativos
Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md).

Se você deseja que o app ofereça suporte a diferentes idiomas de exibição e houver literais de cadeia de caracteres no código, na marcação XAML ou no manifesto do pacote de aplicativos, mova essas cadeias de caracteres para um arquivo de recursos (.resw). Em seguida, você poderá fazer uma cópia traduzida desse arquivo de recursos para cada idioma ao qual o app ofereça suporte.

Os literais de cadeia de caracteres codificados podem ser exibidos no código imperativo ou na marcação XAML, por exemplo, como a propriedade **Text** de um **TextBlock**. Eles também podem ser exibidos no arquivo de origem do manifesto do pacote de aplicativos (o arquivo `Package.appxmanifest`), por exemplo, como o valor de Nome para exibição na guia Aplicativo do Designer de Manifesto do Visual Studio. Mova essas cadeias de caracteres para um arquivo de recursos (.resw) e substitua os literais de cadeia de caracteres inseridos em código do app e do manifesto por referências aos identificadores de recurso.

Diferente dos recursos de imagem, em que apenas um recurso de imagem está contido em um arquivo de recurso de imagem, *vários* recursos de cadeia de caracteres estão contidos em um arquivo de recursos de cadeia de caracteres. Um arquivo de recurso de cadeia de caracteres é um arquivo de recursos (.resw), e você normalmente criar esse tipo de arquivo de recurso em uma pasta \Strings do projeto. Para obter informações sobre como usar os qualificadores nos nomes dos arquivos de recursos (.resw), consulte [Personalizar os recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md).

## <a name="create-a-resources-file-resw-and-put-your-strings-in-it"></a>Criar um arquivo de recursos (.resw) e inserir cadeias de caracteres nele
1. Defina o idioma padrão do app.
    1. Com a solução aberta no Visual Studio, abra `Package.appxmanifest`.
    2. Na guia Aplicativo, confirme se o idioma padrão está definido corretamente (por exemplo, "en" ou "en-US"). As etapas restantes assumirão que você definiu o idioma padrão como "en-US".
    <br>**Observação** No mínimo, você precisa fornecer recursos de cadeias de caracteres localizados para esse idioma padrão. Esses são os recursos que serão carregados se nenhuma correspondência melhor for encontrada para o idioma preferencial do usuário ou as configurações de idioma de exibição.
2. Crie um arquivo de recursos (.resw) para o idioma padrão.
    1. No nó do projeto, crie uma nova pasta e nomeie-a como "Strings".
    2. Em `Strings`, crie uma nova subpasta e nomeie-a como "en-US".
    3. Em `en-US`, crie um novo arquivo de recursos (.resw) e verifique se ele foi nomeado como "Resources. resw".
    <br>**Observação** Se você tiver arquivos de recursos .NET (.resx) cuja portabilidade deseja fazer, consulte [Fazendo a portabilidade do XAML e da interface do usuário](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3.  Abra `Resources.resw` e adicione esses recursos de cadeia de caracteres.

    `Strings/en-US/Resources.resw`

    ![adicionar recurso, inglês](images/addresource-en-us.png)

    Neste exemplo, "Greeting" é um identificador de recurso de cadeia de caracteres ao qual você pode referência na sua marcação, como mostraremos. No identificador "Greeting", uma cadeia de caracteres é fornecida para uma propriedade Text e uma cadeia de caracteres é fornecida para uma propriedade Width. "Greeting.Text" é um exemplo de identificador de propriedade porque corresponde a uma propriedade de um elemento de interface do usuário. Você também pode, por exemplo, adicionar "Greeting.Foreground" à coluna Nome e definir seu valor para "Red". O identificador "Farewell" é um identificador de recurso de cadeia de caracteres simples; ele não tem subpropriedades e pode ser carregado a partir do código imperativo, como mostraremos. A coluna Comment é um excelente lugar para fornecer qualquer instrução especial aos tradutores.

    Neste exemplo, como temos uma entrada de identificador de recurso de cadeia de caracteres simples denominada "Farewell", não podemos ter *também* identificadores de propriedade com base no mesmo identificador. Portanto, adicionar "Farewell.Text" causaria um erro de entrada duplicada durante a compilação de `Resources.resw`.

    Os identificadores de recurso não diferenciam maiúsculas de minúsculas e devem ser exclusivos por arquivo de recurso. Use identificadores de recurso significativos para fornecer contexto adicional para tradutores. Não altere os identificadores de recurso depois que os recursos de cadeia de caracteres forem enviados para tradução. As equipes de localização usam o identificador de recursos para rastrear adições, exclusões e atualizações nos recursos. As alterações em identificadores de recursos, o que também é conhecido como "deslocamento de identificadores de recursos", exigem que cadeias de caracteres sejam traduzidas novamente, pois parecerá que elas foram excluídas e que outras foram adicionadas.

## <a name="refer-to-a-string-resource-identifier-from-xaml-markup"></a>Fazer referência a um identificador de recurso de cadeia de caracteres na marcação XAML
Use uma [diretiva x:Uid](../xaml-platform/x-uid-directive.md) para associar um controle ou outro elemento na marcação a um identificador de recurso de cadeia de caracteres.

```xaml
<TextBlock x:Uid="Greeting"/>
```

Em tempo de execução, `\Strings\en-US\Resources.resw` é carregado (já que agora ele é o único arquivo de recursos do projeto). A diretiva **x:Uid** no **TextBlock** provoca uma pesquisa, que localiza os identificadores de propriedade de `Resources.resw` que contêm o identificador de recurso de cadeia de caracteres "Greeting". Os identificadores de propriedade "Greeting.Text" e "Greeting.Width" são localizados e seus valores são aplicados ao **TextBlock**, substituindo qualquer valor definido localmente na marcação. O valor de "Greeting.Foreground" seria aplicado também se você o tivesse adicionado. Mas, somente os identificadores de propriedade são usados para definir propriedades nos elementos de marcação XAML; portanto, a configuração **x:Uid** para "Farewell" neste TextBlock não surtiria nenhum efeito. `Resources.resw` *does* contém o identificador de recurso de cadeia de caracteres "Farewell", mas não contém nenhum identificador de propriedade para ele.

Ao atribuir um identificador de recurso de cadeia de caracteres a um elemento XAML, tenha certeza de que *todos* os identificadores de propriedade desse identificador são apropriados para o elemento XAML. Por exemplo, se você definir `x:Uid="Greeting"` em um **TextBlock**, "Greeting" será retornado porque o tipo **TextBlock** tem uma propriedade Text. Mas, se você definir `x:Uid="Greeting"` em um **Button**, "Greeting" causará um erro de tempo de execução porque o tipo **Button** não tem uma propriedade Text. Uma solução para esse caso é criar um identificador de propriedade chamado "ButtonGreeting.Content" e definir `x:Uid="ButtonGreeting"` em **Button**.

Em vez de definir **Width** em um arquivo de recursos, você provavelmente permitirá que os controles sejam dimensionados dinamicamente de acordo com o conteúdo.

**Observação** Para [propriedades anexadas](../xaml-platform/attached-properties-overview.md), você precisará de uma sintaxe especial na coluna Name de um arquivo. resw. Por exemplo, para definir um valor para a propriedade anexada [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) do identificador "Greeting" identifier, este é o conteúdo que você inserirá na coluna Name.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Consulte um identificador de recurso de cadeia de caracteres no código
Você pode carregar explicitamente um recurso de cadeia de caracteres com base em um identificador de recurso de cadeia de caracteres simples.

> [!NOTE]
> Se você tiver uma chamada para qualquer método **GetForCurrentView** que *possa* ser executado em um thread de trabalho/segundo plano, proteja essa chamada com um teste `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`. Chamar **GetForCurrentView** em um thread de trabalho/segundo plano impossibilitará a criação da exceção "*&lt;typename&gt; em threads que não têm uma CoreWindow*".

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

Você pode usar esse mesmo código em uma biblioteca de classes (Universal do Windows) ou em um projeto da [Biblioteca do Windows Runtime (Universal do Windows)](../winrt-components/index.md). Em tempo de execução, os recursos do app que está hospedando a biblioteca são carregados. Recomendamos que uma biblioteca carregue os recursos do app que a hospeda, pois o app provavelmente tem um maior grau de localização. Se uma biblioteca não precisar fornecer recursos, ela deverá permitir que seus apps de hospedagem substituam esses recursos como uma entrada.

Se um nome de recurso é segmentado (ele contém "." caracteres), em seguida, substitua pontos com barra ("/") caracteres no nome do recurso. Identificadores de propriedade, por exemplo, contenham pontos; Portanto, você precisa fazer esse substition para carregar um do código.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

Se estiver em dúvida, você pode usar [MakePri.exe](makepri-exe-command-options.md) para despejar o arquivo PRI do seu aplicativo. Cada recurso `uri` é mostrada no arquivo despejado.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Consulte um identificador de recurso de cadeia de caracteres no manifesto do pacote de aplicativos
1. Abra o arquivo de origem do manifesto do pacote de aplicativos (arquivo `Package.appxmanifest`), no qual, por padrão, o Nome para exibição do app é expresso como um literal de cadeia de caracteres.

   ![adicionar recurso, inglês](images/display-name-before.png)

2. Para criar uma versão localizável dessa cadeia de caracteres, abra `Resources.resw` e adicione um novo recurso de cadeia de caracteres com o nome "AppDisplayName" e o valor "Adventure Works Cycles".

3. Substitua o literal de cadeia de caracteres Nome para exibição por uma referência ao identificador de recurso de cadeia de caracteres que você acabou de criar ("AppDisplayName"). Use o esquema URI `ms-resource` (Uniform Resource Identifier) para fazer isso.

   ![adicionar recurso, inglês](images/display-name-after.png)

4. Repita esse processo para cada cadeia de caracteres do manifesto que você deseja localizar. Por exemplo, o nome curto do app (que você pode configurar para aparecer no bloco do app na tela inicial). Para obter uma lista de todos os itens no manifesto do pacote de aplicativos que você pode localizar, consulte [Itens de manifesto localizáveis](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Localizar os recursos de cadeia de caracteres
1. Faça uma cópia do arquivo de recursos (.resw) em outro idioma.
    1. Em "Strings", crie uma nova subpasta e nomeie-a como "de-DE" para alemão (Alemanha).
   <br>**Observação** Como nome de pasta, você pode usar qualquer [marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Consulte [Personalizar os recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md) para obter informações detalhadas sobre o qualificador de idioma e uma lista de marcas de idioma comuns.
   2. Faça uma cópia de `Strings/en-US/Resources.resw` na pasta `Strings/de-DE`.
2. Traduza as cadeias de caracteres.
    1. Abra `Strings/de-DE/Resources.resw` e traduza os valores na coluna Value. Você não precisa traduzir os comentários.

    `Strings/de-DE/Resources.resw`

    ![adicionar recurso, alemão](images/addresource-de-de.png)

Se quiser, repita as etapas 1 e 2 para um idioma adicional.

`Strings/fr-FR/Resources.resw`

![adicionar recurso, francês](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Testar o app
Teste o app no idioma de exibição padrão. Em seguida, altere o idioma de exibição em **Configurações** > **Hora e Idioma** > **Região e idioma** > **Idiomas** e teste o app novamente. Analise as cadeias de caracteres na interface do usuário e no shell (por exemplo, a barra de títulos, que é o Nome para exibição, e o Nome curto em seus blocos).

**Observação** Se for encontrado um nome de pasta que corresponda à configuração de idioma de exibição, o arquivo de recursos contido nessa pasta será carregado. Caso contrário, ocorrerá um fallback, que encerrará com os recursos do idioma padrão do app.

## <a name="factoring-strings-into-multiple-resources-files"></a>Decompondo cadeias de caracteres em vários arquivos de recursos
Você pode manter todas as cadeias de caracteres em um único arquivo de recursos (resw) ou decompô-las em vários arquivos de recursos. Por exemplo, você talvez queira manter as mensagens de erro em um arquivo de recursos, as cadeias de caracteres do manifesto do pacote de aplicativos em outro, e as cadeias de caracteres da interface do usuário em um terceiro arquivo de recursos. Esta será a aparência da sua estrutura de pastas nesse caso.

![adicionar recurso, inglês](images/manifest-resources.png)

Para definir o escopo de uma referência de identificador de recurso de cadeia de caracteres para um determinado arquivo, basta adicionar `/<resources-file-name>/` antes do identificador. O exemplo de marcação abaixo assume que `ErrorMessages.resw` contém um recurso cujo nome é "PasswordTooWeak.Text" e cujo valor descreve o erro.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Você só precisa adicionar `/<resources-file-name>/` antes do identificador de recurso de cadeia de caracteres para arquivos de recursos *diferentes* de `Resources.resw`. Isso ocorre porque "Resources.resw" é o nome de arquivo padrão; portanto, essa será a expectativa se você omitir um nome de arquivo (como fizemos nos exemplos anteriores deste tópico).

O exemplo de código abaixo assume que `ErrorMessages.resw` contém um recurso cujo nome é "MismatchedPasswords" e cujo valor descreve o erro.

> [!NOTE]
> Se você tiver uma chamada para qualquer método **GetForCurrentView** que *possa* ser executado em um thread de trabalho/segundo plano, proteja essa chamada com um teste `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`. Chamar **GetForCurrentView** em um thread de trabalho/segundo plano impossibilitará a criação da exceção "*&lt;typename&gt; em threads que não têm uma CoreWindow*".

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

Se você pretendia mover o recurso "AppDisplayName" de `Resources.resw` para `ManifestResources.resw`, no manifesto do pacote de aplicativos, altere `ms-resource:AppDisplayName` para `ms-resource:/ManifestResources/AppDisplayName`.

Se um nome de arquivo de recurso é segmentado (ele contém "." caracteres), em seguida, deixe os pontos no nome quando você faz referência a ele. **Não** substitua os pontos por caracteres de barra ("/"), como você faria para um nome de recurso.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

Se estiver em dúvida, você pode usar [MakePri.exe](makepri-exe-command-options.md) para despejar o arquivo PRI do seu aplicativo. Cada recurso `uri` é mostrada no arquivo despejado.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Carregar uma cadeia de caracteres para um idioma específico ou outro contexto
O [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) padrão (obtido em [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contém um valor de qualificador para cada nome de qualificador, representando o contexto de tempo de execução padrão (em outras palavras, as configurações do usuário atual e da máquina). Os arquivos de recursos (.resw) são comparados, com base nos qualificadores contidos em seus nomes, com os valores de qualificador nesse contexto de tempo de execução.

Mas, pode haver momentos em que seu app deverá substituir as configurações do sistema e ser explícito sobre o idioma, a escala ou outro valor de qualificador a ser usado durante a procura de um arquivo de recursos correspondente a ser carregado. Por exemplo, talvez seja necessário que seus usuários sejam capazes de selecionar um idioma alternativo para dicas de ferramentas ou mensagens de erro.

Você pode fazer isso criando um novo **ResourceContext**, em vez de usar o padrão, substituindo seus valores e, em seguida, usando esse objeto de contexto em pesquisas de cadeia de caracteres.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

Usar **QualifierValues** como no exemplo de código acima funciona para qualquer qualificador. No caso especial de idioma, você também pode fazer isso.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Para obter o mesmo efeito em um nível global, você *pode* substituir os valores de qualificador no **ResourceContext** padrão. Mas, em vez disso, recomendamos que você chame [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Você define valores de uma só vez com uma chamada para **SetGlobalQualifierValue**, e esses valores estarão em vigor no **ResourceContext** padrão cada vez que você usá-lo nas pesquisas.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Alguns qualificadores têm um provedor de dados do sistema. Portanto, em vez de chamar **SetGlobalQualifierValue**, você poderá ajustar o provedor por meio de sua própria API. Por exemplo, este código mostra como definir [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride).

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Atualizando cadeias de caracteres em resposta aos eventos de alteração do valor de qualificador
O app em execução pode responder às alterações feitas nas configurações do sistema que afetam os valores de qualificador no **ResourceContext** padrão. Qualquer uma dessas configurações de sistema invoca o evento [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) em [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

Em resposta a esse evento, você pode recarregar as cadeias de caracteres no **ResourceContext** padrão.

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
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="loading-strings-from-a-class-library-or-a-windows-runtime-library"></a>Carregando cadeias de caracteres de uma biblioteca de classes ou uma biblioteca do Windows Runtime
Os recursos de cadeia de caracteres de uma biblioteca de classes referenciada (Universal do Windows) ou uma [Biblioteca do Windows Runtime (Universal do Windows)](../winrt-components/index.md) são normalmente adicionados a uma subpasta do pacote em que eles estão incluídos durante o processo de compilação. O identificador de recurso de uma cadeia de caracteres normalmente assume o formato *LibraryName/ResourcesFileName/ResourceIdentifier*.

Uma biblioteca pode obter um ResourceLoader para seus próprios recursos. Por exemplo, o código a seguir ilustra como uma biblioteca ou um app que faz referência a ela pode obter um ResourceLoader para recursos de cadeia de caracteres da biblioteca.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Para uma biblioteca de tempo de execução do Windows (Windows Universal), se o namespace padrão é segmentado (ele contém "." caracteres), em seguida, use pontos no nome do mapa de recursos.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

Você não precisa fazer isso para uma biblioteca de classes (Universal Windows). Se estiver em dúvida, você pode usar [MakePri.exe](makepri-exe-command-options.md) para despejar o componente ou do arquivo PRI da biblioteca. Cada recurso `uri` é mostrada no arquivo despejado.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Carregando cadeias de caracteres de outros pacotes
Os recursos para um pacote de aplicativo são gerenciados e acessados por meio do pacote possui nível superior [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) que pode ser acessado no [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)atual. Em cada pacote, vários componentes podem ter suas próprias subárvores de ResourceMap, que podem ser acessadas via [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live).

Um pacote de estrutura podem acessar seus próprios recursos com um URI de identificador de recurso absoluto. Consulte também [Esquemas URI](uri-schemes.md).

## <a name="important-apis"></a>APIs importantes
* [ApplicationModel.Resources.ResourceLoader](https://msdn.microsoft.com/library/windows/apps/br206014)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Tópicos relacionados
* [Fazendo a portabilidade do XAML e da interface do usuário](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [Diretiva x:Uid](../xaml-platform/x-uid-directive.md)
* [propriedades anexadas](../xaml-platform/attached-properties-overview.md)
* [Itens de manifesto localizáveis](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Personalizar os recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md)
* [Como carregar recursos de cadeias de caracteres](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)