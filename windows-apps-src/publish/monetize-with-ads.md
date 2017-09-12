---
author: jnHs
Description: "Se o aplicativo usa o controle de anúncios ou exibe anúncios em faixa ou intersticiais usando o Microsoft Store Services SDK, use a página Monetizar com anúncios para gerenciar o uso de anúncios."
title: "Monetizar com anúncios"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 6ecd37e54de266764570606ceaa575614dfb050c
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="monetize-with-ads"></a>Monetizar com anúncios

Cada aplicativo no painel inclui uma página **Monetização** &gt; **Monetizar com anúncios**. Use esta página para gerenciar o uso dos anúncios nos seguintes cenários de desenvolvimento no aplicativo:

* O aplicativo UWP usa um [AdControl](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) ou [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) do [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp).
* O aplicativo Windows 8.x ou Windows Phone 8.x usa um **AdControl** ou **InterstitialAd** no [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk).
* O aplicativo Windows 8.x ou Windows Phone 8.x usa um **AdMediatorControl** do [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk).

Para saber mais sobre esses cenários de desenvolvimento, consulte [Exibir anúncios em seu aplicativo com o SDK do Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>Criar unidades publicitárias

Use esta seção para criar uma unidade de anúncio nos seguintes cenários:

* Seu aplicativo mostra anúncios em faixa usando um **AdControl**. Para obter mais informações, consulte [AdControl em XAML e .NET](../monetize/adcontrol-in-xaml-and--net.md) e [AdControl em HTML5 e JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
* O aplicativo mostra anúncios intersticiais em vídeo ou faixa usando um **InterstitialAd**. Para obter mais informações, consulte [Anúncios intersticiais](../monetize/interstitial-ads.md).
* O aplicativo mostra anúncios nativos usando um **NativeAd**. Para obter mais informações, consulte [Anúncios nativos](../monetize/native-ads.md).

Para criar uma unidade publicitária:

1.  No campo **Nome da unidade de anúncio**, insira um nome para a unidade publicitária. Pode ser qualquer cadeia de caracteres descritiva a ser usada para identificar a unidade publicitária para fins de relatório.
2.  Na lista suspensa **Tipo de unidade de publicitária**, selecione o tipo de unidade publicitária correspondente aos anúncios que você está mostrando no controle. As opções disponíveis são: **Faixa**, **Faixa instersticial**, **Vídeo intersticial** e **Nativo**.
    > [!NOTE]
    > A capacidade de criar unidades publicitárias **Nativas** só está disponível para selecionar os desenvolvedores que estejam participando de um programa piloto, mas nós pretendemos disponibilizar esse recurso para todos os desenvolvedores em breve. Se você estiver interessado em participar do nosso programa piloto, entre em contato conosco pelo email aiacare@microsoft.com.

3.  Na lista suspensa **Família de dispositivos**, selecione a família de dispositivos direcionada pelo aplicativo no qual a unidade publicitária será usada. As opções disponíveis são: **UWP (Windows 10)**, **Computador/Tablet (Windows 8.1)** ou **Celular (Windows Phone 8.x)**.
4.  Clique em **Criar unidade publicitária**.

A nova unidade publicitária aparecerá na parte superior da lista na seção **Unidades publicitárias disponíveis** desta página. Para obter mais informações sobre como trabalhar com unidades publicitárias no aplicativo, consulte [Configurar unidades de anúncio no aplicativo](../monetize/set-up-ad-units-in-your-app.md).

> [!NOTE]
> Se o aplicativo Windows 8.x ou Windows Phone 8.x usar um **AdMediatorControl** para mostrar anúncios em faixa, você não precisará solicitar unidades publicitárias aqui. Nesse cenário, as unidades publicitárias serão geradas automaticamente para você.

<span id="available-ad-units" />
## <a name="available-ad-units"></a>Unidades publicitárias disponíveis

As unidades publicitárias aparecem em uma tabela na parte inferior desta seção. Para cada unidade de anúncio, você verá uma **ID do Aplicativo** e uma **ID da unidade de anúncio**. Para mostrar anúncios em seu aplicativo, você precisará usar estes valores em seu código:

-   Se o aplicativo mostrar anúncios em banner, atribua esses valores às propriedades de [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) e de seu objeto [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Para saber mais, consulte [AdControl em XAML e .NET](../monetize/adcontrol-in-xaml-and--net.md) e [AdControl em HTML5 e JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
-   Se seu aplicativo mostrar anúncios intersticiais, passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de seu objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Para obter mais informações, consulte [Anúncios intersticiais](../monetize/interstitial-ads.md).
-   Se o app mostrar anúncios nativos, passe esses valores para os parâmetros *applicationId* e *adUnitId* do construtor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Para obter mais informações, consulte [Anúncios nativos](../monetize/native-ads.md).

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

> [!NOTE]
> Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribuía uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/monetize-with-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

<span id="mediation" />
## <a name="ad-mediation"></a>Controle de anúncios

Se este for um aplicativo UWP para Windows 10, você poderá usar as opções desta seção para habilitar o controle de anúncios de uma unidade publicitária UWP associada ao anúncio em faixa, intersticial ou nativo do aplicativo. O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas e os anúncios não relacionados à geração de receitas para campanhas promocionais de aplicativos da Microsoft. Assumimos o controle das solicitações de anúncios em faixa nas redes de publicidade escolhidas.

Se você já tiver uma unidade publicitária UWP associada ao anúncio em faixa, intersticial ou nativo no aplicativo, a habilitação do controle de anúncios não requer nenhuma alteração de código no aplicativo.

> [!NOTE]
> Esta seção descreve as opções **Controle de anúncios** dos pacotes de aplicativo UWP. Se o pacote do aplicativo direcionar o Windows 8.x ou o Windows Phone 8.x e usar o **AdMediatorControl** no [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk), a seção **Controle de anúncios** do painel exibirá um conjunto diferente de opções. Para obter mais informações sobre como definir as configurações de controle de um pacote de aplicativo Windows 8.x ou Windows Phone 8.x que use **AdMediatorControl**, consulte [este artigo](https://msdn.microsoft.com/library/windows/apps/mt219689).

Para definir as configurações de controle de anúncio de uma unidade de anúncio UWP no UWP:

1. Na lista suspensa **Configurar controle para**, selecione o pacote de aplicativo UWP que contém a unidade publicitária em faixa, intersticial ou nativa que você deseja configurar.

2. Na lista suspensa **Tipo de unidade de anúncio**, selecione o tipo de unidade de anúncio que você deseja configurar (**Em faixa**, **Faixa intersticial**, **Vídeo intersticial** ou **Nativo**).

3. Na lista suspensa **Unidade publicitária**, selecione o nome da unidade publicitária UWP que você deseja configurar.
    > [!NOTE]
    > Ao habilitar um controle de anúncio para uma unidade publicitária UWP, não é necessário obter uma unidade publicitária em redes publicitárias de terceiros. Nosso serviço de controle de anúncios criará automaticamente quaisquer unidades publicitárias de terceiros necessárias.

4. Por padrão, a caixa de seleção **Permitir que a Microsoft escolha as melhores configurações de controle para seu app** é marcada. Essa opção usa algoritmos de aprendizado de máquina para escolher automaticamente as configurações de controle de anúncios para que o aplicativo ajude você a maximizar a receita de anúncios em todos os mercados aos quais o aplicativo ofereça suporte. É recomendável usar essa opção. Porém, se você quiser escolher suas próprias configurações de controle de anúncios, desmarque essa caixa de seleção.
    > [!NOTE]
    > As demais etapas nesta seção são aplicáveis somente se você desmarcar essa caixa de seleção e escolher suas configurações de controle de anúncios.

5. Na lista suspensa **Destino**, escolha **Linha de Base** para definir as configurações padrão do controle de anúncios. Essa configuração padrão será aplicada a todos os mercados, exceto para mercados em que você pode definir configurações específicas de mercado.

6. Em seguida, especifique a proporção de anúncios que você deseja mostrar no controle de redes pagas (que pagarão a receita das impressões) e outras redes de anúncios (que não pagarão a receita das impressões). Para fazer isso, insira um valor entre 0 e 100 nos campos **Peso** de **Redes de publicidade pagas** e **Outras redes de publicidade**.  

7. Na seção **Redes de publicidade pagas**, marque a caixa de seleção na coluna **Ativa** de cada rede paga a ser usada e, em seguida, use as setas na coluna **Classificação** para ordenar as redes por classificação (isso especifica a frequência em que cada rede deve ser usada pelo controle).

    Há suporte para as seguintes redes pagas: Observe que algumas dessas redes [não estão disponíveis em todos os mercados](#network-markets).

    -   **AOL e AppNexus**. Esta é uma rede de anúncios gerenciada pelo Microsoft que veicula anúncios por meio das redes parceiras AOL e AppNexus. Essa rede é compatível com unidades publicitárias de anúncio **Em faixa** e **Vídeo intersticial**.
        > [!NOTE]
        > **AOL e AppNexus** sempre aparecem em primeiro lugar na lista **Redes de publicidade pagas** das unidades publicitárias **Em faixa** e não podem ser rebaixadas de posição nesses tipos de anúncios.

    -   **AppNexus (direto)**: selecione esta opção para veicular anúncios intersticiais em vídeo da [AppNexus](https://www.appnexus.com). Essa rede é compatível com unidades publicitárias de **Vídeo intersticial** e **Nativo**.

    -   **Anúncios de instalação de Aplicativos Microsoft** Selecione esta opção para veicular anúncios de instalação de aplicativo ou anúncios de novo envolvimento de aplicativos criados por outros desenvolvedores no ecossistema do Windows, que [cria campanhas publicitárias promocionais para seus aplicativos](create-an-ad-campaign-for-your-app.md). Essa rede é compatível com unidades publicitárias de anúncio **Em faixa**, **Faixa intersticial** e **Nativo**.

    -   **Smaato**: selecione esta opção para veicular anúncios da [Smaato](https://www.smaato.com/). Essa rede é compatível com unidades publicitárias de anúncio **Em faixa**.

    -   **smartclip**: selecione esta opção para veicular anúncios da [smartclip](http://www.smartclip.com/). Essa rede é compatível com unidades publicitárias de **Vídeo intersticial**.

    -   **SpotX**: selecione esta opção para veicular anúncios da [SpotX](https://www.spotx.tv/). Essa rede é compatível com unidades publicitárias de **Vídeo intersticial**.

    -   **Taboola**: selecione esta opção para veicular anúncios da [Taboola](https://www.taboola.com/). Essa rede é compatível com unidades publicitárias de anúncio **Em faixa**.

8. Se você tiver selecionado uma unidade publicitária **Em faixa** ou **Intersticial de faixa**, também é possível exibir uma seção chamada **Outras redes de publicidade**. As redes nesta seção não geram receita por impressões de anúncios. Em vez disso, essas redes mostram anúncios de fontes como campanhas de promoção de aplicativos. 

    Na seção **Outras redes de publicidade**, marque a caixa de seleção na coluna **Ativa** de cada rede a ser usada e, em seguida, use as setas na coluna **Classificação** para ordenar as redes por classificação (isso especifica a frequência em que cada rede deve ser usada pelo controle). Há suporte também para as seguintes redes:

    -   **Anúncios da Microsoft Community**. Se você [criar uma campanha publicitária promocional para um dos seus aplicativos](create-an-ad-campaign-for-your-app.md) e configurá-la como uma [campanha de anúncio da comunidade](about-community-ads.md), selecione estas opções para mostrar anúncios dessa campanha. 

    -   **Anúncios Internos da Microsoft**. Se você [criar uma campanha publicitária promocional para um dos seus aplicativos](create-an-ad-campaign-for-your-app.md) e configurá-la como uma [campanha de anúncio interno](about-house-ads.md), selecione esta opção para mostrar anúncios dessa campanha.

9. Para cada mercado em que você deseja substituir a configuração de controle padrão, selecione o mercado na lista suspensa **Destino** e atualize as seleções de rede de publicidade e classificação.

10. Clique em **Salvar**.

<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>Mercados com suporte para redes de publicidade

As redes de publicidade disponíveis veiculam anúncios em todos os [mercados com suporte](define-pricing-and-market-selection.md#windows-store-consumer-markets) para aplicativos UWP, com as seguintes exceções.

|  Rede de publicidade  |  Mercados com suporte  |
|--------------|---------------------|
| Smaato | Brasil, Canadá, França, Alemanha, Itália, Japão, Espanha, Reino Unido, Estados Unidos |
| smartclip | Áustria, Bélgica, Dinamarca, Finlândia, Alemanha, Itália, Países Baixos, Noruega, Suécia, Suíça  |


## <a name="microsoft-affiliate-ads"></a>Anúncios de afiliada da Microsoft

Marque a caixa nesta seção se você quer mostrar anúncios de afiliadas da Microsoft em seu aplicativo. Se você marcar essa caixa, anúncios de produtos na Loja, incluindo música, jogos, filmes, aplicativos, hardware e software, serão disponibilizados para seu aplicativo quando nenhum anúncio de outras redes de anúncios estiverem disponíveis. Quando os clientes clicam nos anúncios e adquirem produtos na Loja dentro de uma janela de atribuição determinado, receberão uma comissão sobre compras aprovadas.

Se você alterar essa seleção, não precisa publicar novamente seu app para que as alterações entrem em vigor. Para obter mais informações sobre anúncios de afiliada da Microsoft, consulte [Sobre anúncios de afiliada](about-affiliate-ads.md).

## <a name="coppa-compliance"></a>Conformidade com o COPPA

Para fins do Children's Online Privacy Protection Act (“COPPA”), você deve notificar a Microsoft caso seu aplicativo seja direcionado para crianças com menos de 13 anos. Se você usar o Centro de Desenvolvimento para indicar à Microsoft que seu aplicativo é direcionado a crianças com menos de 13 anos, a Microsoft seguirá as etapas para desabilitar seus serviços de publicidade comportamental ao oferecer um anúncio em seu aplicativo. Caso seu aplicativo seja direcionado para crianças com menos de 13 anos, você tem determinadas obrigações segundo COPPA.

Para obter mais informações sobre as obrigações estabelecidas pelo COPPA, consulte [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).
