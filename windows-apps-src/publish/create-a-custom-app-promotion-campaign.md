---
author: jnHs
Description: "Além de criar uma campanha publicitária para seu aplicativo que será executada em aplicativos do Windows, você também pode promover seu aplicativo usando outros canais."
title: "Criar uma campanha de promoção de aplicativos personalizada"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: a6e97968df4e9ab986d364b2573a31b4ba9d1958

---

# Criar uma campanha de promoção de aplicativos personalizada



Além de criar uma [campanha publicitária para seu aplicativo](create-an-ad-campaign-for-your-app.md) que será executada em aplicativos do Windows, você também pode promover seu aplicativo usando outros canais. Por exemplo, você pode promover o aplicativo usando um provedor de marketing de aplicativos terceirizado, ou pode postar links para seu aplicativo em redes sociais. Essas atividades são chamadas *campanhas personalizadas*.

Se você executar campanhas personalizadas para seu aplicativo, poderá acompanhar o desempenho relativo de cada uma criando uma URL do aplicativo da Windows Store diferente para cada campanha personalizada, onde cada URL contém uma ID de campanha diferente. Quando um cliente executando o Windows 10 clica em uma URL que contém uma ID de campanha, a Microsoft associa o clique à campanha personalizada correspondente e disponibiliza esses dados para você.

Há dois tipos principais de dados associados a campanhas personalizadas: visualizações da página do aplicativo e *conversões*. Uma conversão é uma instalação do aplicativo que resulta do clique de um cliente na página da Windows Store do seu aplicativo a partir de uma URL promovida por uma campanha personalizada. Para obter mais detalhes sobre conversões, confira [Noções básicas sobre como instalações do aplicativo se qualificam como conversões](#understanding-how-app-installs-qualify-as-conversions) neste tópico.

Você pode recuperar dados de desempenho da campanha personalizada para seu aplicativo das seguintes maneiras:

-   Se o seu aplicativo for um aplicativo UWP (Plataforma Universal do Windows), ele poderá usar o método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) para recuperar a ID da campanha personalizada que resultou em uma conversão de forma programática.
-   Você pode exibir dados sobre exibições e conversões de página do aplicativo ou complemento no [Relatório Canais e conversões](channels-and-conversions-report.md) no painel do Centro de Desenvolvimento.

> **Importante**   Esses dados só serão acompanhados para clientes que executam o Windows 10. Os clientes que usam outros sistemas operacionais ainda podem seguir o link até a listagem do seu aplicativo, mas dados sobre as atividades esses clientes não serão incluídos.

 

## Exemplo de cenário de campanha personalizada


Considere uma desenvolvedora de jogos que acabou de criar um novo jogo e gostaria de promovê-lo para jogadores de seus jogos existentes. Ela posta o lançamento do novo jogo na sua página do Facebook, incluindo um link para a página do jogo na Windows Store. Muitos jogadores também a seguem no Twitter, portanto ela também Tweeta o anúncio com o link da página do jogo na Windows Store.

Para acompanhar o sucesso de cada um desses canais de promoção, a desenvolvedora cria duas variantes da URL da página do jogo na Windows Store:

-   A URL que ela postará em sua página do Facebook inclui a ID da campanha personalizada `my-facebook-campaign`.
-   A URL que ela postará no Twitter inclui a ID da campanha personalizada `my-twitter-campaign`.

Assim que os seguidores no Facebook e no Twitter clicam nas URLs, a Microsoft rastreia cada clique e o associa à campanha personalizada correspondente. Aquisições subsequentes qualificadas do jogo e quaisquer compras de complementos são associadas à campanha personalizada e reportadas como conversões.

## Noções básicas sobre como instalações do aplicativo se qualificam como conversões


Uma *conversão* é uma instalação do aplicativo que resulta do clique de um cliente na página da Windows Store do seu aplicativo a partir de uma URL promovida por uma campanha personalizada. Há diferentes condições para a qualificação como uma conversão para o relatório [Canais e conversões](channels-and-conversions-report.md) no painel do Centro de Desenvolvimento e para a qualificação como uma conversão pela [recuperação da ID da campanha de forma programática](#programmatically).

Para se qualificar como uma conversão para o relatório [Canais e conversões](channels-and-conversions-report.md), as seguintes condições devem ser atendidas:

-   Um cliente com uma conta da Microsoft reconhecida clica em uma URL de aplicativo que contém uma ID de campanha personalizada e é redirecionado para a página da Windows Store do aplicativo.
-   O mesmo cliente (conforme identificado pela mesma conta da Microsoft) instala o aplicativo dentro de 24 horas após ter clicado pela primeira vez na URL da Windows Store com a ID de campanha personalizada. Isso se qualifica como uma conversão mesmo se o cliente instalar o aplicativo em um computador ou dispositivo diferente daquele no qual clicou na URL da Windows Store com a ID da campanha personalizada.
    > **Observação**  Para instalações de aplicativos contabilizadas como conversões para uma campanha personalizada, as compras de complementos nesse aplicativo também são contabilizadas como conversões para a mesma campanha personalizada.

     

Para se qualificar como uma conversão pela recuperação da ID da campanha associada ao aplicativo de forma programática, as seguintes condições devem ser atendidas:

-   Um cliente com uma conta da Microsoft reconhecida clica em uma URL de aplicativo que contém uma ID de campanha personalizada e é redirecionado para a página da Windows Store do aplicativo.
-   O cliente instala o aplicativo durante a mesma visualização da página da Windows Store depois de clicar na URL. Se o usuário sai da página e, em seguida, retorna à página (no mesmo computador ou dispositivo ou em outro computador ou dispositivo) dentro de 24 horas e instala o aplicativo, isso se qualifica como uma conversão no relatório [Canais e conversão](channels-and-conversions-report.md), mas não se qualificará como uma conversão se você recuperar a ID da campanha de forma programática.

## Incorporar uma ID de campanha personalizada à URL da página do seu aplicativo da Windows Store


Para criar uma URL da página da Windows Store para o seu aplicativo com uma ID de campanha personalizada:

1.  Crie uma cadeia de caracteres de ID para a campanha personalizada. Essa cadeia de caracteres pode conter até 100 caracteres, embora recomendemos que você defina IDs de campanha curtas, que sejam facilmente identificáveis.
2.  Obtenha a URL da página da Windows Store para seu aplicativo no formato HTML ou de protocolo. A URL em formato HTML está disponível na página [**Identidade do Aplicativo** no painel do Centro de Desenvolvimento](link-to-your-app.md).
    -   Use o formato HTTP se você quiser que os clientes naveguem até a página de seu aplicativo na Windows Store em um navegador (essa URL também iniciará o aplicativo da Windows Store, se estiver instalado, na listagem de seu aplicativo). Essa URL tem o formato **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Por exemplo, a URL HTTP para o Skype é `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`.
        > **Observação**  As URLs em formato HTTP podem ser usadas para navegar até a Windows Store em um navegador em computadores e tablets que executam o Windows7 e posterior e em telefones que executam o Windows Phone 8 e versões posteriores.
-   Use o formato de protocolo se for promover seu aplicativo por meio de outros aplicativos do Windows que estão em execução em um computador ou dispositivo com o aplicativo da Windows Store instalado, e quiser que os clientes abram a página de seu aplicativo no aplicativo da Windows Store. Essa URL tem o formato **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Por exemplo, a URL do protocolo para o Skype é `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.
3.  Acrescente a seguinte cadeia de caracteres ao final da URL de seu aplicativo:
    -   Para uma URL em formato HTTP, acrescente **`?cid=*my custom campaign ID*`**. Por exemplo, se o Skype apresentar uma ID de campanha com o valor **custom\_campaign**, a nova URL de HTTP, incluindo a campanha ID, será: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.
    -   Para uma URL em formato de protocolo, acrescente **`&cid=*my custom campaign ID*`**. Por exemplo, se o Skype apresentar uma ID de campanha com o valor **custom\_campaign**, a nova URL de protocolo, incluindo a campanha ID, será: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

## Recuperar a ID da campanha personalizada para um aplicativo de forma programática


Se o seu aplicativo for um aplicativo UWP, você poderá recuperar de forma programática a ID da campanha personalizada associada ao seu aplicativo usando o método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445). Esse método possibilita muitos cenários de análise e monetização. Por exemplo, você poderá descobrir se o usuário atual adquiriu seu aplicativo depois de descobri-lo por meio de sua campanha no Facebook e, em seguida, personalizar a experiência do aplicativo de modo correspondente. Como alternativa, se estiver usando um provedor de marketing de aplicativo terceirizado, você poderá enviar dados de volta ao provedor.

> **Observação**  O método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) retornará uma cadeia de caracteres da ID da campanha somente se o cliente clicar em sua URL com a ID da campanha incorporada, for redirecionado para a página da Windows Store do seu aplicativo e depois instalar seu aplicativo sem sair da página. Se o usuário sair da página e mais tarde retornar e instalar o aplicativo, isso não se qualificará como uma conversão ao usar **GetAppPurchaseCampaignIdAsync**. Para saber mais, confira [Noções básicas sobre conversões](#conversions) neste tópico.

 

O exemplo a seguir demonstra como usar o método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) para recuperar a ID da campanha personalizada para o aplicativo. Se o aplicativo não foi instalado como parte de uma conversão bem-sucedida, esse método retorna uma cadeia de caracteres vazia.

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

O método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) acessa dados da Windows Store. Siga estas diretrizes ao usar esse método:

-   Faça o encapsulamento dessa chamada de método em uma operação assíncrona para permitir a conclusão da chamada.
-   Se seu aplicativo ainda não foi publicado na Windows Store e você estiver testando sua campanha personalizada, use o método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) da classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), em vez da classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Siga estas diretrizes:
    -   Adicione um elemento **AppPurchaseCampaignId** ao arquivo WindowsStoreProxy.xml, conforme mostrado no exemplo a seguir. Atribua o valor do elemento à ID da campanha personalizada. Quando você executar o aplicativo, o método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) sempre retornará esse valor. Para obter mais informações sobre o arquivo WindowsStoreProxy.xml, consulte a documentação do [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```
    
    -   Para alternar facilmente entre usar [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) e [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), recomendamos adicionar as instruções a seguir ao seu código para definir um alias da **Store** e qualificar suas chamadas [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) com o alias da **Store**.

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## Testar sua campanha personalizada


Antes de promover uma URL de campanha personalizada, recomendamos que você teste a campanha personalizada fazendo o seguinte:

1.  Entre em sua conta da Microsoft no computador ou dispositivo que estiver usando para testes.
2.  Clique em sua URL de campanha personalizada. Verifique se a Windows Store carrega a página do aplicativo corretamente e feche o aplicativo da Windows Store ou a página do navegador.
3.  Clique na URL muitas vezes mais, fechando o aplicativo da Windows Store ou a página do navegador após cada visita à página do seu aplicativo. Em uma das visitas à página do seu aplicativo, instale-o para gerar uma conversão. Conte o número total de vezes que você clicou na URL.
4.  Se você estiver recuperando de forma programática a ID da campanha personalizada em seu aplicativo, teste essa operação usando o método [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) da classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), em vez da classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765).

 

 







<!--HONumber=Aug16_HO3-->


