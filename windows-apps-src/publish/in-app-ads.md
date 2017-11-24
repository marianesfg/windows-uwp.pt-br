---
author: jnHs
Description: "Se seu aplicativo exibe anúncios usando o SDK do Microsoft Advertising, use a página de anúncios no aplicativo do painel do Centro de Desenvolvimento para gerenciar o uso de anúncios."
title: "Anúncios no aplicativo"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
localizationpriority: high
ms.openlocfilehash: eb835269cd9df7385d8d03e032d8b41cb7f92a19
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2017
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

    * Se você estiver usando um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em seu aplicativo para mostrar anúncios em faixa, selecione **Faixa** como o tipo de unidade publicitária.

    * Se você estiver usando um [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) em seu aplicativo para mostrar anúncios de vídeo intersticiais ou anúncios de banner intersticiais, selecione **Vídeo intersticial** ou **Banner intersticial** (certifique-se de selecionar a opção adequada para o tipo de anúncio intersticial você quer mostrar).

    * Se você estiver usando um [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) em seu aplicativo para mostrar anúncios em faixa, selecione **Nativo** como o tipo de unidade publicitária.
      > [!NOTE]
      > A capacidade de criar unidades publicitárias **Nativas** só está disponível para selecionar os desenvolvedores que estejam participando de um programa piloto, mas nós pretendemos disponibilizar esse recurso para todos os desenvolvedores em breve. Se você estiver interessado em participar do nosso programa piloto, entre em contato conosco pelo email aiacare@microsoft.com.

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

* Se o aplicativo mostrar [anúncios em banner](../monetize/banner-ads.md), atribua esses valores às propriedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) e [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) de seu objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).

* Se seu aplicativo mostrar [anúncios intersticiais](../monetize/interstitial-ads.md), passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de seu objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

* Se o app mostrar [anúncios nativos](../monetize/native-ads.md), passe esses valores para os parâmetros *applicationId* e *adUnitId* do construtor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx).
  > [!IMPORTANT]
  > Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

  > [!NOTE]
  > Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

Para editar as [Configurações de mediação](#mediation) para uma unidade publicitária UWP ou a [Conformidade com COPPA](#coppa) para o aplicativo no qual a unidade publicitária é usada, clique no nome da unidade publicitária.

<span id="mediation" />
## <a name="mediation-settings"></a>Ajustes de controle

Quando você [Criar uma nova unidade publicitária UWP](#create-ad-unit) ou [Editar uma unidade publicitária UWP existente](#available-ad-units), use as opções desta seção para configurar a mediação de anúncios para a unidade publicitária. O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas e os anúncios não relacionados à geração de receitas para campanhas promocionais de aplicativos da Microsoft. Assumimos o controle das solicitações de anúncios em faixa nas redes de publicidade escolhidas. Se você já tiver uma unidade publicitária UWP associada ao anúncio em faixa, intersticial ou nativo no aplicativo, a habilitação do controle de anúncios não requer nenhuma alteração de código no aplicativo.

> [!NOTE]
> Ao habilitar um controle de anúncio para uma unidade publicitária UWP, não é necessário obter uma unidade publicitária em redes publicitárias de terceiros. Nosso serviço de controle de anúncios criará automaticamente quaisquer unidades publicitárias de terceiros necessárias.

Para definir as configurações de controle de anúncio de uma unidade de anúncio UWP no UWP:

1. [Criar uma unidade de anúncio](#create-ad-unit) ou [Selecionar uma unidade de anúncio existente](#available-ad-units).

2. Na página **Anúncios no aplicativo**, vá para a seção **Configurações de mediação**.

3. Por padrão, a caixa de seleção **Permitir que a Microsoft escolha as melhores configurações de controle para seu app** é marcada. Essa opção usa algoritmos de aprendizado de máquina para escolher automaticamente as configurações de controle de anúncios para que o aplicativo ajude você a maximizar a receita de anúncios em todos os mercados aos quais o aplicativo ofereça suporte. É recomendável usar essa opção. Porém, se você quiser escolher suas próprias configurações de controle de anúncios, desmarque essa caixa de seleção.
    > [!NOTE]
    > As demais etapas nesta seção são aplicáveis somente se você desmarcar essa caixa de seleção e escolher suas configurações de controle de anúncios.

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
| AOL e AppNexus |  Esta é uma rede de anúncios gerenciada pelo Microsoft que veicula anúncios por meio das redes parceiras AOL e AppNexus.<p/>**Observação**: AOL e AppNexus sempre aparecem em primeiro lugar na lista **Redes de publicidade pagas** das unidades publicitárias Em faixa e não podem ser rebaixadas de posição nesses tipos de anúncios. | Faixa, Vídeo intersticial |
| AppNexus (direto) | Selecione esta opção para veicular anúncios intersticiais em vídeo da [AppNexus](https://www.appnexus.com). | Vídeo intersticial, Nativo  |
| Anúncios de instalação de Aplicativos Microsoft | Selecione esta opção para veicular anúncios de instalação de aplicativo ou anúncios de novo envolvimento de aplicativos criados por outros desenvolvedores no ecossistema do Windows, que [cria campanhas publicitárias promocionais para seus aplicativos](create-an-ad-campaign-for-your-app.md).  |  Faixa, Vídeo intersticial, Nativo  |
| Smaato |  Selecione esta opção para veicular anúncios da [Smaato](https://www.smaato.com/). |  Faixa  |
| smartclip |  Selecione esta opção para veicular anúncios da [smartclip](http://www.smartclip.com/). |  Intersticial em vídeo  |
| SpotX |  Selecione esta opção para veicular anúncios da [SpotX](https://www.spotx.tv/). |  Intersticial em vídeo  |
| Taboola |  Selecione esta opção para veicular anúncios da [Taboola](https://www.taboola.com/). |  Faixa  |

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
| Smaato | Brasil, Canadá, França, Alemanha, Itália, Japão, Espanha, Reino Unido, Estados Unidos |
| smartclip | Áustria, Bélgica, Dinamarca, Finlândia, Alemanha, Itália, Países Baixos, Noruega, Suécia, Suíça  |

<span id="coppa" />
## <a name="coppa-compliance"></a>Conformidade com o COPPA

Para fins do Children's Online Privacy Protection Act (“COPPA”), você deve notificar a Microsoft caso seu aplicativo seja direcionado para crianças com menos de 13 anos. Se você usar o Centro de Desenvolvimento para indicar à Microsoft que seu aplicativo é direcionado a crianças com menos de 13 anos, a Microsoft seguirá as etapas para desabilitar seus serviços de publicidade comportamental ao oferecer um anúncio em seu aplicativo. Caso seu aplicativo seja direcionado para crianças com menos de 13 anos, você tem determinadas obrigações segundo COPPA.

Para obter mais informações sobre as obrigações estabelecidas pelo COPPA, consulte [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).
