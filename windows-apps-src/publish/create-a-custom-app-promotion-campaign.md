---
author: shawjohn
Description: "Além de criar uma campanha publicitária para seu aplicativo que será executada em aplicativos do Windows, você também pode promover seu aplicativo usando outros canais."
title: "Criar uma campanha personalizada de promoção de app"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, personalizado, app, promoção, campanha"
ms.openlocfilehash: 1e56b1df4b5501ded5db799c3ad67f97c0e1cfcd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-a-custom-app-promotion-campaign"></a>Criar uma campanha personalizada de promoção de app

Além de criar uma [campanha publicitária para seu aplicativo](create-an-ad-campaign-for-your-app.md) que será executada em aplicativos do Windows, você também pode promover seu aplicativo usando outros canais. Por exemplo, você pode promover o aplicativo usando um provedor de marketing de aplicativos terceirizado, ou pode postar links para seu aplicativo em redes sociais. Essas atividades são chamadas *campanhas personalizadas*.

Se você executar campanhas personalizadas para seu aplicativo, poderá acompanhar o desempenho relativo de cada uma criando uma URL do aplicativo da Windows Store diferente para cada campanha personalizada, onde cada URL contém uma ID de campanha diferente. Quando um cliente executando o Windows 10 clica em uma URL que contém uma ID de campanha, a Microsoft associa o clique à campanha personalizada correspondente e disponibiliza esses dados para você.

Há dois tipos principais de dados associados a campanhas personalizadas: visualizações de página do app e *conversões*. Uma conversão é uma aquisição do app que resulta do clique de um cliente na página da Windows Store do seu app a partir de uma URL promovida por uma campanha personalizada. Para obter mais detalhes sobre conversões, confira [Noções básicas sobre como aquisições do app se qualificam como conversões](#understanding-how-app-acquisitions-qualify-as-conversions) neste tópico.

Você pode recuperar dados de desempenho da campanha personalizada para o seu app das seguintes maneiras:

* Você pode exibir dados sobre exibições e conversões de página do aplicativo ou complemento no [Relatório Canais e conversões](channels-and-conversions-report.md) no painel do Centro de Desenvolvimento.
* Se o seu app for um app UWP (Plataforma Universal do Windows), ele poderá usar APIs no SDK do Windows para recuperar de forma programática a ID da campanha personalizada que resultou em uma conversão.

> **Importante**&nbsp;&nbsp;Esses dados só serão acompanhados para clientes que executam o Windows 10. Os clientes que usam outros sistemas operacionais ainda podem seguir o link até a listagem do seu aplicativo, mas dados sobre as atividades esses clientes não serão incluídos.

## <a name="example-custom-campaign-scenario"></a>Exemplo de cenário de campanha personalizada

Considere uma desenvolvedora de jogos que acabou de criar um novo jogo e gostaria de promovê-lo para jogadores de seus jogos existentes. Ela posta o lançamento do novo jogo na sua página do Facebook, incluindo um link para a página do jogo na Windows Store. Muitos jogadores também a seguem no Twitter, portanto ela também Tweeta o anúncio com o link da página do jogo na Windows Store.

Para acompanhar o sucesso de cada um desses canais de promoção, a desenvolvedora cria duas variantes da URL da página do jogo na Windows Store:

* A URL que ela postará em sua página do Facebook inclui a ID da campanha personalizada `my-facebook-campaign`.
* A URL que ela postará no Twitter inclui a ID da campanha personalizada `my-twitter-campaign`.

Assim que os seguidores no Facebook e no Twitter clicam nas URLs, a Microsoft rastreia cada clique e o associa à campanha personalizada correspondente. As aquisições subsequentes qualificadas do jogo e quaisquer compras de complementos são associadas à campanha personalizada e reportadas como conversões.

<span id="conversions" />
## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>Noções básicas sobre como aquisições de app se qualificam como conversões

Uma *conversão* é uma aquisição do app que resulta do clique de um cliente na página da Windows Store do seu app a partir de uma URL promovida por uma campanha personalizada. Há diferentes cenários para a qualificação como uma conversão para o relatório [Canais e conversões](channels-and-conversions-report.md) no painel do Centro de Desenvolvimento e para a qualificação como uma conversão pela [recuperação da ID da campanha de forma programática](#programmatically).

### <a name="qualifying-conversions-for-the-dashboard-report"></a>Conversões de relatório do painel de qualificação

Os cenários a seguir se qualificam como uma conversão para o relatório [Canais e conversões](channels-and-conversions-report.md):

* Um cliente *com ou sem uma conta da Microsoft reconhecida* clica em uma URL de app que contém uma ID de campanha personalizada e é redirecionado para a página da Windows Store do app. O mesmo cliente adquire o app dentro de 24 horas após clicar pela primeira vez na URL da Windows Store com a ID de campanha personalizada.

* Se o cliente adquirir o app em um computador ou dispositivo diferente daquele que foi usado para clicar na URL da Windows Store com a ID de campanha personalizada, a conversão será contada somente se o cliente tiver uma conta da Microsoft.

> **Observação**&nbsp;&nbsp;Para aquisições de app contabilizadas como conversões para uma campanha personalizada, as compras de complementos nesse app também são contabilizadas como conversões para a mesma campanha personalizada.

### <a name="qualifying-conversions-for-programmatically-retrieving-the-campaign-id"></a>Conversões de qualificação para a recuperação da ID da campanha de forma programática

Para se qualificar como uma conversão ao recuperar de forma programática a ID de campanha associada ao app, as seguintes condições devem ser atendidas:

* Em um dispositivo executando **Windows 10, versão 1607 ou posterior**: um cliente *com ou sem uma conta da Microsoft reconhecida* clica em uma URL de app que contém uma ID de campanha personalizada e é redirecionado para a página da Windows Store para o aplicativo. O cliente adquire o app durante a mesma visualização da página da Windows Store depois de clicar na URL.

* Em um dispositivo executando **Windows 10, versão 1511 ou posterior**: um cliente *com uma conta da Microsoft reconhecida* clica em uma URL de app que contém uma ID de campanha personalizada e é redirecionado para a página da Windows Store para o aplicativo. O cliente adquire o app durante a mesma visualização da página da Windows Store depois de clicar na URL. Nessas versões do Windows 10, o usuário deve ter uma conta da Microsoft reconhecida para que a aquisição seja qualificada como conversão ao recuperar de forma programática a ID da campanha.

>**Observação**&nbsp;&nbsp;Se o cliente sair da página e, em seguida, retornar à página (no mesmo computador ou dispositivo, ou em outro computador ou dispositivo se tiver uma conta da Microsoft) dentro de 24 horas e adquirir o app, isso se qualifica como uma conversão no [Relatório canais e conversões](channels-and-conversions-report.md). No entanto, isso não se qualificará como uma conversão se você recuperar de forma programática a ID de campanha.

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>Incorporar uma ID de campanha personalizada à URL da página do seu aplicativo da Windows Store

Para criar uma URL da página da Windows Store para o seu aplicativo com uma ID de campanha personalizada:

1.  Crie uma cadeia de caracteres de ID para a campanha personalizada. Essa cadeia de caracteres pode conter até 100 caracteres, embora recomendemos que você defina IDs de campanha curtas, que sejam facilmente identificáveis.

  >**Observação**&nbsp;&nbsp;A cadeia de caracteres da ID de campanha pode estar visível para outros desenvolvedores dentro de um relatório de canais e conversões. Isso pode ocorrer quando um cliente clica em sua ID de campanha para entrar na Loja, mas acaba comprando o app de outro desenvolvedor dentro da mesma sessão. Mesmo que seu nome de ID de campanha possa estar visível para outro desenvolvedor nesse caso, esse desenvolvedor só verá quantas conversões de seu próprio app resultaram de um clique inicial em sua ID de campanha. Eles não verão quaisquer dados sobre como muitos usuários adquiriram seu app ao clicar em seu ID de campanha.

2.  Obtenha a URL da página da Windows Store para seu app no formato HTML ou de protocolo.

    * Use o formato HTTP se você quiser que os clientes naveguem até a página de seu aplicativo na Windows Store em um navegador (essa URL também iniciará o aplicativo da Windows Store, se estiver instalado, na listagem de seu aplicativo). Essa URL tem o formato **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Por exemplo, a URL HTTP para o Skype é `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`. A URL em formato HTML está disponível na página [**Identidade do Aplicativo** no painel do Centro de Desenvolvimento](link-to-your-app.md). Observe que as URLs em formato HTTP podem ser usadas para navegar até a Windows Store em um navegador em computadores e tablets que executam o Windows7 e posterior e em telefones que executam o Windows Phone 8 e versões posteriores.

    * Use o formato de protocolo se for promover seu app por meio de outros aplicativos do Windows que estão em execução em um computador ou dispositivo com o aplicativo da Windows Store instalado, e quiser que os clientes abram a página de seu app no aplicativo da Windows Store. Essa URL tem o formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Por exemplo, a URL do protocolo para o Skype é `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Acrescente a seguinte cadeia de caracteres ao final da URL de seu aplicativo:

    * Para uma URL em formato HTTP, acrescente **`?cid=*my custom campaign ID*`**. Por exemplo, se o Skype apresentar uma ID de campanha com o valor **custom\_campaign**, a nova URL de HTTP, incluindo a campanha ID, será: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Para uma URL em formato de protocolo, acrescente **`&cid=*my custom campaign ID*`**. Por exemplo, se o Skype apresentar uma ID de campanha com o valor **custom\_campaign**, a nova URL de protocolo, incluindo a campanha ID, será: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />
## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Recuperar a ID da campanha personalizada para um app de forma programática

Se o seu app for um app UWP, você poderá recuperar de forma programática a ID da campanha personalizada associada ao seu app usando APIs no SDK do Windows. Essas APIs possibilitam muitos cenários de análise e monetização. Por exemplo, você poderá descobrir se o usuário atual adquiriu seu aplicativo depois de descobri-lo por meio de sua campanha no Facebook e, em seguida, personalizar a experiência do aplicativo de modo correspondente. Como alternativa, se estiver usando um provedor de marketing de app terceirizado, você poderá enviar dados de volta ao provedor.

Essas APIs retornarão uma cadeia de caracteres da ID da campanha somente se o cliente clicar em sua URL com a ID da campanha incorporada, for redirecionado para a página da Windows Store do seu app e depois adquirir seu app sem sair da página. Se o usuário sair da página e mais tarde retornar e adquirir o app, isso não se [qualificará como uma conversão](#conversions) ao usar essas APIs.

Existem APIs diferentes para você usar dependendo da versão do Windows 10 a que se destina o seu app:

* Windows 10, versão 1607 ou posterior: Use a classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace **Windows.Services.Store**. Ao usar essa API, você pode recuperar IDs de campanha personalizada para quaisquer [aquisições qualificadas](#conversions), se o usuário tiver uma conta da Microsoft reconhecida ou não.
* Windows 10, versão 1511 ou anterior: Use a classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) no namespace **ApplicationModel**. Ao usar essa API, você só pode recuperar IDs de campanha personalizada para [aquisições qualificadas](#conversions), se o usuário tiver uma conta da Microsoft reconhecida.

>**Observação**&nbsp;&nbsp;Embora o namespace **Windows.ApplicationModel.Store** esteja disponível em todas as versões do Windows 10, recomendamos que você use as APIs no namespace **Windows.Services.Store** se o seu aplicativo é voltado para Windows 10, versão 1607 ou posterior. Para obter mais informações sobre as diferenças entre esses namespaces, consulte [compras no aplicativo e avaliações](../monetize/in-app-purchases-and-trials.md#choose-namespace). O exemplo de código a seguir mostra como estruturar seu código para usar as duas APIs no mesmo projeto.

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

2. Em seguida, ele tenta obter a ID de campanha personalizada para o caso de o usuário atual ter uma conta da Microsoft reconhecida. Para fazer isso, o código obtém um objeto [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) que representa o app atual SKU e acessa a propriedade [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_CampaignId) para recuperar a ID da campanha, se houver uma disponível.

2. Em seguida, o código tenta recuperar a ID da campanha para o caso de o usuário atual não ter uma conta da Microsoft reconhecida. Nesse caso, a ID da campanha é inserida na licença do app. O código recupera a licença usando o método [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppLicenseAsync) e, em seguida, analisa o conteúdo JSON de licença para o valor de uma chave denominada *customPolicyField1*. Esse valor contém a ID da campanha.

3. Se a classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace **Windows.Services.Store** não estiver disponível, o código, em seguida, recorrerá ao uso do método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getapppurchasecampaignidasync.aspx) no namepsace **ApplicationModel** para recuperar a ID de campanha personalizada (esse namespace está disponível em todas as versões do Windows 10, incluindo versão 1511 e anteriores). Observe que ao usar esse método, você só pode recuperar IDs de campanha personalizada para [aquisições qualificadas](#conversions), se o usuário tiver uma conta da Microsoft reconhecida.

  >**Observação**&nbsp;&nbsp;o namespace **ApplicationModel** inclui [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), uma classe especial que simula operações de loja para testar seu código antes de enviar seu app para a loja. Essa classe recupera os dados de [um arquivo local chamado arquivo Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). O exemplo de código anterior mostra como incluir **CurrentApp** e **CurrentAppSimulator** em código de depuração e não depuração no seu projeto. Para testar esse código em um ambiente de depuração, adicione um elemento **AppPurchaseCampaignId** ao arquivo WindowsStoreProxy.xml no computador de desenvolvimento, conforme o exemplo a seguir. Quando você executar o app, o método [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator#Windows_ApplicationModel_Store_CurrentAppSimulator_GetAppPurchaseCampaignIdAsync) sempre retornará esse valor.

  ``` xml
  <CurrentApp>
      ...
      <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
  </CurrentApp>
  ```

  >**Observação**&nbsp;&nbsp;O namespace **Windows.Services.Store** não fornece uma classe que possibilite a você usar para simular informações de licença durante o teste. Em vez disso, você deve publicar um aplicativo na Loja e baixá-lo para seu dispositivo de desenvolvimento para usar sua licença para teste. Para obter mais informações, consulte [Compras no aplicativo e avaliações](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Testar sua campanha personalizada

Antes de promover uma URL de campanha personalizada, recomendamos que você teste a campanha personalizada fazendo o seguinte:

1.  Entre em sua conta da Microsoft no computador ou dispositivo que estiver usando para testes.
2.  Clique em sua URL de campanha personalizada. Verifique se a Windows Store carrega a página do aplicativo corretamente e feche o aplicativo da Windows Store ou a página do navegador.
3.  Clique na URL muitas vezes, fechando o aplicativo da Windows Store ou a página do navegador após cada visita à página do seu app. Em uma das visitas à página do seu app, adquira-o para gerar uma conversão. Conte o número total de vezes que você clicou na URL.
4. Confirme se as exibições e conversões de página esperadas aparecem no [Relatório de canais e conversões](channels-and-conversions-report.md)e teste o código do app para confirmar se ele pode recuperar a ID da campanha com êxito.
