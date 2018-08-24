---
author: jnHs
Description: If your app displays ads using the Microsoft Advertising SDK, use the In-app ads page of the Dev Center dashboard to manage your use of ads.
title: Anúncios no aplicativo
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 83c4645a09a38a76dfd230436e858e222d817eab
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2835605"
---
# <a name="in-app-ads"></a>Anúncios no aplicativo

Use a página **Monetizar** &gt; **Anúncios no aplicativo** no painel do Centro de Desenvolvimento para criar e gerenciar unidades de anúncio para:

* Aplicativos da Plataforma Universal do Windows (UWP) que usam o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp).
* Aplicativos do Windows 8.x e Windows Phone 8.x que usam o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk).

Para obter mais informações sobre como integrar esses SDKs com seus aplicativos para exibir anúncios, consulte [Exibir anúncios em seu aplicativo com o SDK do Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Criar unidades publicitárias

Para criar uma unidade de anúncio para um [Anúncio em faixa](../monetize/banner-ads.md), [Anúncio intersticial](../monetize/interstitial-ads.md) ou [Anúncio nativo](../monetize/native-ads.md) em seu aplicativo:

1.  Vá para a página **Monetizar** &gt; **Anúncios em aplicativo** no painel e clique em **Criar unidade publicitária**.
2.  Na lista suspensa **Nome do aplicativo**, selecione o aplicativo no qual a unidade publicitária será usada.
3.  No campo **Nome da unidade de anúncio**, insira um nome para a unidade publicitária. Pode ser qualquer cadeia de caracteres descritiva a ser usada para identificar a unidade publicitária para fins de relatório.
4.  Na lista suspensa **Tipo de unidade de anúncio**, selecione o tipo de anúncio.

    * Se você está mostrando uma faixa de anúncio no seu aplicativo, selecione **Banner**.
    * Se você está mostrando um anúncio intersticial de vídeo ou intersticial banner em seu aplicativo, selecione **vídeo intersticial** ou **Banner intersticial** (certifique-se de selecionar a opção apropriada para o tipo de intersticial ad que você deseja mostrar).
    * Se você está mostrando um anúncio nativo no seu aplicativo, selecione **Native**.

5. Na lista suspensa **Família de dispositivos**, selecione a família de dispositivos direcionada pelo aplicativo no qual a unidade publicitária será usada. As opções disponíveis são: **UWP (Windows 10)**, **Computador/Tablet (Windows 8.1)** ou **Celular (Windows Phone 8.x)**.

6. Defina as seguintes configurações adicionais conforme desejado:

    * Se você selecionar a família de dispositivos **UWP (Windows 10)** para a unidade publicitária, você pode configurar opcionalmente [Configurações de mediação](#mediation) para a unidade publicitária.
    * Se você selecionar a família de dispositivos **Computador/Tablet (Windows 8.1)** ou **Celular (Windows Phone 8. x)** para uma unidade de anúncio em faixa, você pode selecionar opcionalmente **Mostrar anúncios da comunidade em seu aplicativo** para optar por [Anúncios de comunidade](about-community-ads.md).

7.  Se você ainda não tiver definido a conformidade com COPPA para o aplicativo selecionado, escolha uma opção na seção [Conformidade com COPPA](#coppa).
8.  Clique em **Criar unidade publicitária**.

Depois de criar a nova unidade publicitária, ela aparecerá na tabela de unidades publicitárias disponíveis na página **Monetizar** &gt; **Anúncios em aplicativo**.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Revisar e editar unidades de anúncio

Depois de criar unidades publicitárias para um ou mais aplicativos em sua conta, essas unidades aparecem em uma tabela na parte inferior da página **Monetizar** &gt; **Anúncios em aplicativos**. Esta tabela mostra a **ID do aplicativo** e **ID da unidade publicitária** para cada unidade publicitária, juntamente com outras informações. Para mostrar anúncios em seu aplicativo, você precisará usar estes valores em seu código. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](../monetize/set-up-ad-units-in-your-app.md).

* Se o aplicativo mostrar [anúncios em banner](../monetize/banner-ads.md), atribua esses valores às propriedades [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) e [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) de seu objeto [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol).
* Se seu aplicativo mostrar [anúncios intersticiais](../monetize/interstitial-ads.md), passe esses valores para o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) de seu objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad).
* Se seu aplicativo mostrar [ads nativos](../monetize/native-ads.md), passa esses valores para o construtor **NativeAdsManagerV2** .
  > [!IMPORTANT]
  > Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

  > [!NOTE]
  > Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

Para editar as [Configurações de mediação](#mediation) para uma unidade publicitária UWP ou a [Conformidade com COPPA](#coppa) para o aplicativo no qual a unidade publicitária é usada, clique no nome da unidade publicitária.

> [!NOTE]
> Se uma unidade de ad não tem nenhuma atividade para os últimos seis meses, podemos será rotule-o como **inativo**e, por fim, remova-o do seu painel. Você pode usar filtros para mostrar apenas unidades publicitárias **Ativas** ou **Inativas**. Caso veja uma unidade publicitária que você acredita que esteja marcada incorretamente como **Inativa**, [contate o suporte](http://aka.ms/storesupport).

<span id="mediation" />

## <a name="mediation-settings"></a>Configurações de mediação

Quando você [criar uma nova unidade da ad UWP](#create-ad-unit) ou [Editar uma unidade de ad UWP existente](#available-ad-units), use as opções nesta seção para configurar a [mediação da ad](../monetize/ad-mediation-service.md) para a unidade do ad. O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas e os anúncios não relacionados à geração de receitas para campanhas promocionais de aplicativos da Microsoft. Assumimos o controle das solicitações de anúncios em faixa nas redes de publicidade escolhidas. Se você já tiver uma unidade publicitária UWP associada ao anúncio em faixa, intersticial ou nativo no aplicativo, a habilitação do controle de anúncios não requer nenhuma alteração de código no aplicativo.

> [!NOTE]
> Ao habilitar um controle de anúncio para uma unidade publicitária UWP, não é necessário obter uma unidade publicitária em redes publicitárias de terceiros. Nosso serviço de controle de anúncios criará automaticamente quaisquer unidades publicitárias de terceiros necessárias.

Para definir as configurações de controle de anúncio de uma unidade de anúncio UWP no UWP:

1. [Criar uma unidade de anúncio](#create-ad-unit) ou [Selecionar uma unidade de anúncio existente](#available-ad-units).
2. Na página **no aplicativo ads** , vá para a seção **configurações de mediação** e a configuração suas configurações.

    * Por padrão, a caixa de seleção **Permitir que a Microsoft escolha as melhores configurações de controle para seu app** é marcada. É recomendável usar essa opção. Essa opção usa algoritmos de aprendizado de máquina para escolher automaticamente as configurações de controle de anúncios para que o aplicativo ajude você a maximizar a receita de anúncios em todos os mercados aos quais o aplicativo ofereça suporte. Quando você usa essa opção, você também pode escolher as redes do ad que você deseja usar na configuração. Desmarque as redes do ad que você não quer ser parte da configuração e nosso algoritmo garantirá que o seu aplicativo só recebe ads das redes ad selecionado.
    * Se desejar escolher seu próprios ad configurações de mediação, escolha **modificar configurações do padrão**.

    > [!NOTE]
    > As etapas restantes nesta seção só são aplicáveis se você escolher **modificar configurações do padrão**.

4. Na lista suspensa **Destino**, escolha **Linha de Base** para definir as configurações padrão do controle de anúncios. Essa configuração padrão será aplicada a todos os mercados, exceto para mercados em que você pode definir configurações específicas de mercado.
6. Em seguida, especifique a proporção de anúncios que você deseja mostrar no controle de redes pagas (que pagarão a receita das impressões) e outras redes de anúncios (que não pagarão a receita das impressões). Para fazer isso, insira um valor entre 0 e 100 nos campos **Peso** de **Redes de publicidade pagas** e **Outras redes de publicidade**.  
7. Na seção **Redes de publicidade pagas**, marque a caixa de seleção na coluna **Ativa** de cada [rede paga](#paid-networks) a ser usada e, em seguida, use as setas na coluna **Classificação** para ordenar as redes por classificação (isso especifica a frequência em que cada rede deve ser usada pelo controle).
8. Se você tiver selecionado uma unidade publicitária **Em faixa** ou **Intersticial de faixa**, também é possível exibir uma seção chamada **Outras redes de publicidade**. As redes nesta seção não geram receita por impressões de anúncios. Em vez disso, essas redes mostram anúncios de fontes como campanhas de promoção de aplicativos.

    Na seção **Outras redes de publicidade**, marque a caixa de seleção na coluna **Ativa** de cada [rede](#other-networks) a ser usada e, em seguida, use as setas na coluna **Classificação** para ordenar as redes por classificação (isso especifica a frequência em que cada rede deve ser usada pelo controle). Há suporte também para as seguintes redes:

9. Para cada mercado em que você deseja substituir a configuração de controle padrão, selecione o mercado na lista suspensa **Destino** e atualize as seleções de rede de publicidade e classificação.
10. Clique em **Criar unidade publicitária** (se você estiver criando uma nova unidade publicitária) ou **Salvar** (se você estiver editando uma unidade publicitária existente).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Redes de anúncios pagas com suporte

A tabela a seguir lista as redes pagas atualmente com suporte para cada tipo de anúncio. Observe que algumas dessas redes [não estão disponíveis em todos os mercados](#network-markets).

|  Rede de publicidade  |  Descrição  |  Tipos de anúncio com suporte  |
|--------------|---------------|---------------------|
| Juramento e AppNexus |  Esta é uma rede de ad Microsoft gerenciados que serve ads através de nosso parceiro redes, juramento e AppNexus.<p/>**Observação**: juramento e AppNexus é sempre classificados primeiro na lista **pago ad redes** para unidades de ad banner, e ele não pode ser alterado para uma classificação inferior para esses tipos de anúncios. | Faixa, Vídeo intersticial |
| AppNexus (direto) | Selecione essa opção para servir anúncios de [AppNexus](https://www.appnexus.com). | Vídeo intersticial, Nativo  |
| Anúncios de instalação de Aplicativos Microsoft | Selecione esta opção para veicular anúncios de instalação de aplicativo ou anúncios de novo envolvimento de aplicativos criados por outros desenvolvedores no ecossistema do Windows, que [cria campanhas publicitárias promocionais para seus aplicativos](create-an-ad-campaign-for-your-app.md).  |  Faixa, Vídeo intersticial, Nativo  |
| Recomendações de conteúdo do MSN |  Selecione essa opção para servir anúncios de recomendações de conteúdo do MSN. |  Faixa, faixa intersticial  |
| Outbrain |  Selecione esta opção para veicular anúncios da [Outbrain](https://www.outbrain.com/). |  Faixa, faixa intersticial  |
| Revcontent |  Selecione esta opção para veicular anúncios da [Revcontent](http://www.revcontent.com/). |  Faixa, nativo  |
| Smaato |  Selecione esta opção para veicular anúncios da [Smaato](https://www.smaato.com/). |  Faixa  |
| smartclip |  Selecione esta opção para veicular anúncios da [smartclip](http://www.smartclip.com/). |  Intersticial em vídeo  |
| SpotX |  Selecione esta opção para veicular anúncios da [SpotX](https://www.spotx.tv/). |  Intersticial em vídeo  |
| Taboola |  Selecione esta opção para veicular anúncios da [Taboola](https://www.taboola.com/). |  Faixa  |
| Undertone | Selecione essa opção para servir anúncios de [Undertone](https://www.undertone.com/). | Banner intersticial |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Outras redes de publicidade

A tabela a seguir lista as outras redes atualmente com suporte para cada tipo de anúncio.

|  Rede de publicidade  |  Descrição  |  Tipos de anúncio com suporte  |
|--------------|---------------|---------------------|
| Anúncios da comunidade da Microsoft |  Se você [criar uma campanha publicitária promocional para um dos seus aplicativos](create-an-ad-campaign-for-your-app.md) e configurá-la como uma [campanha de anúncio da comunidade](about-community-ads.md), selecione estas opções para mostrar anúncios dessa campanha. | Faixa, faixa intersticial |
| Anúncios Internos da Microsoft | Se você [criar uma campanha publicitária promocional para um dos seus aplicativos](create-an-ad-campaign-for-your-app.md) e configurá-la como uma [campanha de anúncio interno](about-house-ads.md), selecione esta opção para mostrar anúncios dessa campanha. | Faixa, faixa intersticial  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Mercados com suporte para redes de publicidade

As redes de publicidade disponíveis veiculam anúncios em todos os [mercados com suporte](define-pricing-and-market-selection.md#microsoft-store-consumer-markets), com as seguintes exceções.

|  Rede de publicidade  |  Mercados com suporte  |
|--------------|---------------------|
| Revcontent | Brasil, Canadá, França, Alemanha, Itália, Japão, Espanha, Reino Unido, Estados Unidos  |
| Smaato | Brasil, Canadá, França, Alemanha, Itália, Japão, Espanha, Reino Unido, Estados Unidos |
| smartclip | Áustria, Bélgica, Dinamarca, Finlândia, Alemanha, Itália, Países Baixos, Noruega, Suécia, Suíça  |
| Undertone | Estados Unidos |

<span id="coppa" />

## <a name="coppa-compliance"></a>Conformidade com o COPPA

Quando você [cria uma unidade publicitária](#create-ad-unit) ou [seleciona uma unidade publicitária existente](#available-ad-units), a seção **Conformidade com o COPPA** aparece na parte inferior da página do painel se o aplicativo selecionado para a unidade publicitária tiver pelo menos uma assinatura que tenha chegado à etapa [na Store](../publish/the-app-certification-process.md#in-the-store) no processo de certificação do aplicativo.

Para fins do Children's Online Privacy Protection Act (“COPPA”), você deve selecionar **Este aplicativo é direcionado para crianças com menos de 13 anos** nessa seção se seu aplicativo é direcionado para crianças com menos de 13 anos. Se você selecionar essa opção, a Microsoft seguirá as etapas para desabilitar seus serviços de publicidade comportamental ao oferecer um anúncio em seu aplicativo.

A configuração **Conformidade com o COPPA** escolhida por você é automaticamente aplicada a todas as unidades publicitárias do aplicativo selecionado.

> [!IMPORTANT]
> Caso seu aplicativo seja direcionado para crianças com menos de 13 anos, você tem determinadas obrigações segundo COPPA. Para obter mais informações sobre as obrigações, consulte [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).