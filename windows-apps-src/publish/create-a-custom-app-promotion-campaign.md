---
author: JnHs
description: Além de criar uma campanha publicitária para seu app que será executada em aplicativos do Windows, você poderá promover seu app usando outros canais.
title: Criar uma campanha personalizada de promoção de app
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: wdg-dev-content
ms.date: 09/27/2017
ms.topic: article
keywords: windows 10, uwp, personalizado, app, promoção, campanha
ms.localizationpriority: medium
ms.openlocfilehash: 0349833c012789b55d33575702390f264335aa2f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5711373"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Criar uma campanha personalizada de promoção de app

Além de criar uma [campanha publicitária para seu aplicativo](create-an-ad-campaign-for-your-app.md) que será executada em aplicativos do Windows, você também pode promover seu aplicativo usando outros canais. Por exemplo, você pode promover o aplicativo usando um provedor de marketing de aplicativos terceirizado, ou pode postar links para seu aplicativo em redes sociais. Essas atividades são chamadas *campanhas personalizadas*.

Se você veicular campanhas personalizadas para seu aplicativo, é possível acompanhar o desempenho relativo de cada uma criando uma URL para cada campanha personalizada, na qual cada URL contém uma *ID de campanha* diferente. Quando um cliente que executam o Windows 10 clica em uma URL que contém uma ID de campanha, a Microsoft associa o clique campanha personalizada correspondente e disponibiliza esses dados para você.

> [!IMPORTANT]
> Esses dados acompanhados somente para clientes no Windows 10. Os clientes que usam outros sistemas operacionais ainda podem seguir o link até a listagem do seu aplicativo, mas dados sobre as atividades esses clientes não serão incluídos.

Existem dois tipos principais de dados associados a campanhas personalizadas: *visualizações de página* da listagem da Store do aplicativo e *conversões*. Uma conversão é uma aquisição do aplicativo resultante da visualização por um cliente na página de listagem da Loja da Windows Store a partir de uma URL que inclui uma ID de campanha personalizada. Para obter mais detalhes sobre conversões, confira [Noções básicas sobre como aquisições do app se qualificam como conversões](#understanding-how-acquisitions-qualify-as-conversions) neste tópico.

Você pode recuperar dados de desempenho da campanha personalizada para o seu app das seguintes maneiras:

* Você pode exibir dados sobre visualizações de página e conversões para seu aplicativo ou complemento nos gráficos **Visualizações de página e conversões do aplicativo por ID da campanha** e **Total de conversões de campanha** gráficos no [Relatório de aquisições](acquisitions-report.md) no painel do Centro de Desenvolvimento.
* Se o aplicativo for aplicativo UWP (Plataforma Universal do Windows), é possível usar APIs no SDK do Windows para recuperar de forma programática a ID da campanha personalizada que resultou em uma conversão.

## <a name="example-custom-campaign-scenario"></a>Exemplo de cenário de campanha personalizada

Considere uma desenvolvedora de jogos que acabou de criar um novo jogo e gostaria de promovê-lo para jogadores de seus jogos existentes. Ela posta o lançamento do novo jogo na sua página do Facebook, incluindo um link para a página do jogo na listagem da Store do jogo. Muitos jogadores também a seguem no Twitter, portanto ela também tweeta o anúncio com o link da listagem da Store do jogo.

Para acompanhar o sucesso de cada um desses canais de promoção, a desenvolvedora cria duas variantes da URL da listagem da Store do jogo:

* A URL que ela postará em sua página do Facebook inclui a ID da campanha personalizada `my-facebook-campaign`

* A URL que ela postará no Twitter inclui a ID da campanha personalizada `my-twitter-campaign`

Assim que os seguidores no Facebook e no Twitter clicam nas URLs, a Microsoft rastreia cada clique e o associa à campanha personalizada correspondente. As aquisições subsequentes qualificadas do jogo e quaisquer compras de complementos são associadas à campanha personalizada e reportadas como conversões.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Noções básicas sobre a qualificação de aquisições de aplicativo como conversões

Uma *conversão* de campanha personalizada é uma aquisição que resulta do clique de um cliente a partir de uma URL promovida por uma campanha personalizada. Existem diferentes cenários para a qualificação como uma conversão para os gráficos **Visualizações de página e conversões por ID da campanha** e **Total de conversões de campanha** no relatório [Relatório de aquisições](acquisitions-report.md) no painel Centro de Desenvolvimento e para a qualificação como uma conversão pela [recuperação da ID da campanha de forma programática](#programmatically).

### <a name="qualifying-conversions-in-the-dashboard-report"></a>Qualificação de conversões no relatório do painel

Os cenários a seguir se qualificam como uma conversão para os gráficos **Visualizações de página e conversões de aplicativo por ID da campanha** e **Total de conversões da campanha** no [Relatório de aquisições](acquisitions-report.md):

* Um cliente *com ou sem uma conta da Microsoft reconhecida* clica em uma URL do aplicativo que contém uma ID de campanha personalizada e é redirecionado para listagem da Loja para o aplicativo. Em seguida, o mesmo cliente adquire o aplicativo em 24 horas após clicar pela primeira vez na URL da Microsoft Store com a ID de campanha personalizada.

* Se o cliente adquirir o aplicativo em um computador ou dispositivo diferente daquele que usado para clicar na URL com a ID de campanha personalizada, a conversão será considerada somente se o cliente estiver conectado com a mesma conta da Microsoft usada ao clicar na URL.

> [!NOTE]
> Para aquisições de aplicativo contabilizadas como conversões para uma campanha personalizada, as compras de complementos nele também são contabilizadas como conversões para a mesma campanha personalizada.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Conversões de qualificação ao recuperar programaticamente a ID da campanha

Para se qualificar como uma conversão ao recuperar de forma programática a ID de campanha associada ao app, as seguintes condições devem ser atendidas:

* Em um dispositivo executando **Windows 10, versão 1607 ou posterior**: um cliente (conectado como uma conta da Microsoft reconhecida ou não) clica em uma URL com uma ID de campanha personalizada e é redirecionada para a página de listagem da Loja do aplicativo. O cliente adquire o aplicativo enquanto visualiza a listagem da Loja ao clicar na URL.

* Em um dispositivo executando o **Windows 10, versão 1511 ou anterior**: um cliente (que deve estar conectado com uma conta da Microsoft reconhecida) clica em uma URL de aplicativo que contém uma ID de campanha personalizada e é redirecionado para a página de listagem da Loja do aplicativo. O cliente adquire o aplicativo enquanto visualiza a listagem da Loja ao clicar na URL. Nessas versões do Windows 10, o usuário deve se conectar com uma conta da Microsoft reconhecida para que a aquisição seja qualificada como conversão ao recuperar de forma programática a ID da campanha.

> [!NOTE]
> Se o cliente sai da página de listagem da Loja, mas retorna à página em 24 horas (seja no mesmo dispositivo ou em um diferente quando conectado com a mesma conta da Microsoft) e adquire o aplicativo, isso **é** qualificado como uma conversão nos gráficos **Visualizações de página e conversões do aplicativo por ID da campanha** e **Total de conversões de campanha** no [Relatório de aquisições](acquisitions-report.md). Entretanto, isso **não se qualificará** como uma conversão se você recuperar de forma programática a ID de campanha.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Incorporar uma ID de campanha personalizada à URL da página do seu app da Microsoft Store

Para criar uma URL da página da Microsoft Store para o seu app com uma ID de campanha personalizada:

1.  Crie uma cadeia de caracteres de ID para a campanha personalizada. Essa cadeia de caracteres pode conter até 100 caracteres, embora recomendemos que você defina IDs de campanha curtas, que sejam facilmente identificáveis.

   > [!NOTE]
   > A cadeia de caracteres da ID de campanha pode estar visível para outros desenvolvedores ao exibirem o [Relatório de aquisições](acquisitions-report.md) para seus aplicativos. Isso pode ocorrer quando um cliente clica na ID de campanha personalizada para entrar na Loja, mas acaba comprando o aplicativo de outro desenvolvedor na mesma sessão, atribuindo a conversão à sua ID de campanha. Esse desenvolvedor visualizará quantas conversões de seu próprio aplicativo resultaram de um clique inicial na ID de campanha, incluindo o nome da ID da campanha, mas não visualizarão os dados sobre como muitos usuários compraram seus aplicativos (ou aplicativos de quaisquer outros desenvolvedores) depois de clicar na sua ID de campanha.

2.  Obtenha o link da listagem da Store do seu app no formato HTML ou de protocolo.

    * Use a URL HTML se você deseja que os clientes naveguem até a listagem da Loja com base na Web do aplicativo em um navegador de qualquer sistema operacional. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo. Essa URL tem o formato **`https://www.microsoft.com/store/apps/*your app ID*`**. Por exemplo, a URL HTML para o Skype é `https://www.microsoft.com/store/apps/9wzdncrfj364`. Você pode encontrar essa URL na página [Identidade do aplicativo](view-app-identity-details.md#link-to-your-apps-listing).

    * Use o formato de protocolo se for promover o app em outros aplicativos do Windows em execução em um computador ou um dispositivo com o aplicativo UWP instalado, ou quando você sabe que os clientes estão em um dispositivo com suporte à Microsoft Store. Esse link entrará diretamente na listagem da Loja do seu aplicativo sem abrir um navegador. Essa URL tem o formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Por exemplo, a URL do protocolo para o Skype é `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Acrescente a seguinte cadeia de caracteres ao final da URL de seu aplicativo:

    * Para uma URL em formato HTML, acrescente **`?cid=*my custom campaign ID*`**. Por exemplo, se o Skype apresentar uma ID de campanha com o valor **custom\_campaign**, a nova URL HTTP incluindo a ID da campanha ID será: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Para uma URL em formato de protocolo, acrescente **`&cid=*my custom campaign ID*`**. Por exemplo, se o Skype apresentar uma ID de campanha com o valor **custom\_campaign**, a nova URL de protocolo, incluindo a campanha ID, será: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Recuperar a ID da campanha personalizada para um app de forma programática

Se o aplicativo for um aplicativo UWP, é possível recuperar de modo programático a ID da campanha personalizada associada à aquisição do aplicativo usando APIs no SDK do Windows. Essas APIs possibilitam muitos cenários de análise e monetização. Por exemplo, você poderá descobrir se o usuário atual adquiriu seu aplicativo depois de descobri-lo por meio de sua campanha no Facebook e, em seguida, personalizar a experiência do aplicativo de modo correspondente. Como alternativa, se estiver usando um provedor de marketing de app terceirizado, você poderá enviar dados de volta ao provedor.

Essas APIs retornarão uma cadeia de caracteres da ID da campanha somente se o cliente tiver clicado na URL com a ID da campanha incorporada, exibiu a página da Microsoft Store do app e então adquirido o app sem sair da página de listagem da Store. Se o usuário sair da página e mais tarde retornar e adquirir o app, isso não se [qualificará como uma conversão](#conversions) ao usar essas APIs.

Existem APIs diferentes para você usar dependendo da versão do Windows 10 a que se destina o seu app:

* Windows 10, versão 1607 ou posterior: Use a classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace **Windows.Services.Store**. Ao usar essa API, é possível recuperar IDs de campanha personalizada para quaisquer [aquisições qualificadas](#conversions), independentemente do usuário se conectar com conta da Microsoft reconhecida.

* Windows 10, versão 1511 ou anterior: Use a classe [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) no namespace **ApplicationModel**. Ao usar essa API, é possível recuperar somente IDs de campanha personalizada para [aquisições qualificadas](#conversions), nas quais o usuário está conectado com uma conta da Microsoft reconhecida.

> [!NOTE]
> Embora o namespace **Windows.ApplicationModel.Store** esteja disponível em todas as versões do Windows 10, recomendamos que você use as APIs no namespace **Windows.Services.Store** se o aplicativo for direcionado para Windows 10, versão 1607 ou posterior. Para obter mais informações sobre as diferenças entre esses namespaces, consulte [compras no aplicativo e avaliações](../monetize/in-app-purchases-and-trials.md#choose-namespace). O exemplo de código a seguir mostra como estruturar seu código para usar as duas APIs no mesmo projeto.

### <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir mostra como recuperar a ID de campanha personalizada. Este exemplo usa dois conjuntos de APIs nos namespaces **Windows.Services.Store** e **Windows.ApplicationModel.Store** usando [código adaptável de versão](../debug-test-perf/version-adaptive-code.md). Ao seguir esse processo, seu código pode ser executado em qualquer versão do Windows 10. Para usar esse código, a versão do sistema operacional de destino do seu projeto deve ser **edição de aniversário do Windows 10 (10.0; Compilação 14394)** ou posterior, no entanto, a versão mínima do sistema operacional pode ser uma versão anterior.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Este código faz o seguinte:

1. Primeiro, ele verifica se a classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace **Windows.Services.Store** está disponível no dispositivo atual (isso significa que o dispositivo está executando o Windows 10, versão 1607 ou posterior). Em caso afirmativo, o código continua a usar essa classe.

2. Em seguida, ele tenta obter a ID de campanha personalizada para o caso de o usuário atual ter uma conta da Microsoft reconhecida. Para fazer isso, o código obtém um objeto [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) que representa o app atual SKU e acessa a propriedade [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId) para recuperar a ID da campanha, se houver uma disponível.
3. Em seguida, o código tenta recuperar a ID da campanha para o caso de o usuário atual não ter uma conta da Microsoft reconhecida. Nesse caso, a ID da campanha é inserida na licença do app. O código recupera a licença usando o método [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) e, em seguida, analisa o conteúdo JSON de licença para o valor de uma chave denominada *customPolicyField1*. Esse valor contém a ID da campanha.

4. Se a classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace **Windows.Services.Store** não estiver disponível, o código então recorrerá ao uso do método [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) no namespace **Windows.ApplicationModel.Store** para recuperar a ID de campanha personalizada (esse namespace está disponível em todas as versões do Windows 10, incluindo versão 1511 e anteriores). Observe que ao usar esse método, você só pode recuperar IDs de campanha personalizada para [aquisições qualificadas](#conversions), se o usuário tiver uma conta da Microsoft reconhecida.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Especifique a ID de campanha no arquivo de proxy para o namespace Windows.ApplicationModel.Store.

O namespace **ApplicationModel** inclui [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), uma classe especial que simula operações de Loja para testar seu código antes de enviar o aplicativo para a Loja. Essa classe recupera os dados de [um arquivo local chamado arquivo Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). O exemplo de código anterior mostra como incluir **CurrentApp** e **CurrentAppSimulator** em código de depuração e não depuração no seu projeto. Para testar esse código em um ambiente de depuração, adicione um elemento **AppPurchaseCampaignId** ao arquivo WindowsStoreProxy.xml no computador de desenvolvimento, conforme o exemplo a seguir. Quando você executar o app, o método [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) sempre retornará esse valor.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

O namespace **Windows.Services.Store** não fornece uma classe que seja possível usar para simular informações de licença durante o teste. Em vez disso, você deve publicar um aplicativo na Loja e baixá-lo para seu dispositivo de desenvolvimento para usar sua licença para teste. Para obter mais informações, consulte [Compras no aplicativo e avaliações](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Testar sua campanha personalizada

Antes de promover uma URL de campanha personalizada, recomendamos que você teste a campanha personalizada fazendo o seguinte:

1.  Entre em sua conta da Microsoft no dispositivo que estiver usando para testes.

2.  Clique em sua URL de campanha personalizada. Entre na página do app corretamente e feche o aplicativo UWP ou a página do navegador.

3.  Clique na URL muitas vezes, fechando o aplicativo UWP ou a página do navegador após cada visita à página do seu app. Em **uma** das visitas à página do aplicativo, adquira-o para gerar uma conversão. Conte o número total de vezes que você clicou na URL.

4. Confirme se as visualizações e conversões de página esperadas aparecem nos gráficos **Visualizações e conversões de página por ID da campanha** e **Total de conversões de campanha** no [relatório de Aquisições](acquisitions-report.md) no painel do Centro de Desenvolvimento e teste o código do aplicativo para confirmar se ele pode recuperar com êxito a ID da campanha usando as APIs descritas acima.
