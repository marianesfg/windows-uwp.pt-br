---
Description: Se seu aplicativo exibir anúncios usando o SDK do Microsoft Advertising, use a página anúncios no aplicativo do Partner Center para gerenciar o uso de anúncios.
title: Anúncios no aplicativo
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 96994566d19e03f1d85b751242331f04fef098ad
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78290340"
---
# <a name="in-app-ads"></a>Anúncios no aplicativo

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Use o **monetizar** &gt; página **de anúncios no aplicativo** no [Partner Center](https://partner.microsoft.com/dashboard) para criar e gerenciar unidades do AD para:

* Aplicativos da Plataforma Universal do Windows (UWP) que usam o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK).
* Aplicativos do Windows 8. x e Windows Phone 8. x publicados anteriormente que usam o [SDK do Microsoft Advertising para Windows e o Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, os produtos recém-criados não podem incluir pacotes destinados ao Windows 8. x/Windows Phone 8. x ou anterior. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para obter mais informações sobre como integrar esses SDKs com seus aplicativos para exibir anúncios, consulte [Exibir anúncios em seu aplicativo com o SDK do Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Criar unidades publicitárias

Para criar uma unidade de anúncio para um [Anúncio em faixa](../monetize/banner-ads.md), [Anúncio intersticial](../monetize/interstitial-ads.md) ou [Anúncio nativo](../monetize/native-ads.md) em seu aplicativo:

1.  Acesse o **monetizar** &gt; página **de anúncios no aplicativo** no Partner Center e clique em **criar unidade de anúncio**.
2.  Na lista suspensa **Nome do aplicativo**, selecione o aplicativo no qual a unidade publicitária será usada.
3.  No campo **Nome da unidade de anúncio**, insira um nome para a unidade publicitária. Pode ser qualquer cadeia de caracteres descritiva a ser usada para identificar a unidade publicitária para fins de relatório.
4.  Na lista suspensa **Tipo de unidade de anúncio**, selecione o tipo de anúncio.

    * Se você estiver mostrando um anúncio de faixa em seu aplicativo, selecione **faixa**.
    * Se você estiver mostrando um anúncio do intersticial video ad ou intersticial banner em seu aplicativo, selecione **vídeo intersticial** ou **faixa intersticial** (certifique-se de selecionar a opção apropriada para o tipo de intersticial AD que você deseja mostrar).
    * Se você estiver mostrando um anúncio nativo em seu aplicativo, selecione **nativo**.

5. Na lista suspensa **Família de dispositivos**, selecione a família de dispositivos direcionada pelo aplicativo no qual a unidade publicitária será usada. As opções disponíveis são: **UWP (Windows 10)** , **Computador/Tablet (Windows 8.1)** ou **Celular (Windows Phone 8.x)** .

6. Defina as seguintes configurações adicionais conforme desejado:

    * Se você selecionar a família de dispositivos **UWP (Windows 10)** para a unidade publicitária, você pode configurar opcionalmente [Configurações de mediação](#mediation) para a unidade publicitária.
    * Se você selecionar a família de dispositivos **Computador/Tablet (Windows 8.1)** ou **Celular (Windows Phone 8. x)** para uma unidade de anúncio em faixa, você pode selecionar opcionalmente **Mostrar anúncios da comunidade em seu aplicativo** para optar por [Anúncios de comunidade](about-community-ads.md).

7.  Se você ainda não tiver definido a conformidade com COPPA para o aplicativo selecionado, escolha uma opção na seção [Conformidade com COPPA](#coppa).
8.  Clique em **Criar unidade de anúncio**.

Depois de criar a nova unidade do AD, ela aparecerá na tabela de unidades do AD disponíveis no **monetizar** &gt; página **de anúncios no aplicativo** .

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Revisar e editar unidades de anúncio

Depois de criar unidades do AD para um ou mais aplicativos em sua conta, essas unidades do AD aparecem em uma tabela na parte inferior da página de anúncios do **monetizar** &gt; **no aplicativo** . Esta tabela mostra a **ID do aplicativo** e **ID da unidade publicitária** para cada unidade publicitária, juntamente com outras informações. Para mostrar anúncios em seu aplicativo, você precisará usar estes valores em seu código. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](../monetize/set-up-ad-units-in-your-app.md).

* Se o aplicativo mostrar [anúncios em banner](../monetize/banner-ads.md), atribua esses valores às propriedades [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) e [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) de seu objeto [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol).
* Se seu aplicativo mostrar [anúncios intersticiais](../monetize/interstitial-ads.md), passe esses valores para o método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) de seu objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad).
* Se seu aplicativo mostrar [anúncios nativos](../monetize/native-ads.md), passe esses valores para o construtor **NativeAdsManagerV2** .
  > [!IMPORTANT]
  > Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

  > [!NOTE]
  > Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

Para editar as [Configurações de mediação](#mediation) para uma unidade publicitária UWP ou a [Conformidade com COPPA](#coppa) para o aplicativo no qual a unidade publicitária é usada, clique no nome da unidade publicitária.

> [!NOTE]
> Se uma unidade de anúncio não tiver nenhuma atividade nos últimos seis meses, iremos rotulá-la como **inativa**e, eventualmente, removê-la do Partner Center. Você pode usar filtros para mostrar apenas unidades publicitárias **Ativas** ou **Inativas**. Caso veja uma unidade publicitária que você acredita que esteja marcada incorretamente como **Inativa**, [contate o suporte](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>Configurações de controle

Quando você [cria uma nova unidade de AD UWP](#create-ad-unit) ou [edita uma unidade de AD UWP existente](#available-ad-units), use as opções nesta seção para configurar a [mediação do AD](../monetize/ad-mediation-service.md) para a unidade do AD. O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas e os anúncios não relacionados à geração de receitas para campanhas promocionais de aplicativos da Microsoft. Assumimos o controle das solicitações de anúncios em faixa nas redes de publicidade escolhidas. Se você já tiver uma unidade publicitária UWP associada ao anúncio em faixa, intersticial ou nativo no aplicativo, a habilitação do controle de anúncios não requer nenhuma alteração de código no aplicativo.

> [!NOTE]
> Ao habilitar um controle de anúncio para uma unidade publicitária UWP, não é necessário obter uma unidade publicitária em redes publicitárias de terceiros. Nosso serviço de controle de anúncios criará automaticamente quaisquer unidades publicitárias de terceiros necessárias.

Para definir as configurações de controle de anúncio de uma unidade de anúncio UWP no UWP:

1. [Criar uma unidade de anúncio](#create-ad-unit) ou [Selecionar uma unidade de anúncio existente](#available-ad-units).
2. Na página **anúncios no aplicativo** , vá para a seção **configurações de mediação** e configuração de suas configurações.

    * Por padrão, a caixa de seleção **permitir que a Microsoft Otimize minhas configurações** esteja marcada. É recomendável usar essa opção. Essa opção usa algoritmos de aprendizado de máquina para escolher automaticamente as configurações de controle de anúncios para que o aplicativo ajude você a maximizar a receita de anúncios em todos os mercados aos quais o aplicativo ofereça suporte. Ao usar essa opção, você também pode escolher as redes do AD que deseja usar na configuração. Desmarque as redes do AD que você não deseja que façam parte da configuração e nosso algoritmo garantirá que seu aplicativo receba somente anúncios das redes do AD selecionadas.
    * Se você quiser escolher suas próprias configurações de mediação do AD, escolha **Modificar configurações padrão**.

    > [!NOTE]
    > As etapas restantes nesta seção só serão aplicáveis se você escolher **Modificar configurações padrão**.

3. Na lista suspensa **Destino**, escolha **Linha de Base** para definir as configurações padrão do controle de anúncios. Essa configuração padrão será aplicada a todos os mercados, exceto para mercados em que você pode definir configurações específicas de mercado.
4. Em seguida, especifique a proporção de anúncios que você deseja mostrar no controle de redes pagas (que pagarão a receita das impressões) e outras redes de anúncios (que não pagarão a receita das impressões). Para fazer isso, insira um valor entre 0 e 100 nos campos **Peso** de **Redes de publicidade pagas** e **Outras redes de publicidade**.  
5. Na seção **Redes de publicidade pagas**, marque a caixa de seleção na coluna **Ativa** de cada [rede paga](#paid-networks) a ser usada e, em seguida, use as setas na coluna **Classificação** para ordenar as redes por classificação (isso especifica a frequência em que cada rede deve ser usada pelo controle).
6. Se você tiver selecionado uma unidade publicitária **Em faixa** ou **Intersticial de faixa**, também é possível exibir uma seção chamada **Outras redes de publicidade**. As redes nesta seção não geram receita por impressões de anúncios. Em vez disso, essas redes mostram anúncios de fontes como campanhas de promoção de aplicativos.

    Na seção **Outras redes de publicidade**, marque a caixa de seleção na coluna **Ativa** de cada [rede](#other-networks) a ser usada e, em seguida, use as setas na coluna **Classificação** para ordenar as redes por classificação (isso especifica a frequência em que cada rede deve ser usada pelo controle). Há suporte também para as seguintes redes:

7. Para cada mercado em que você deseja substituir a configuração de controle padrão, selecione o mercado na lista suspensa **Destino** e atualize as seleções de rede de publicidade e classificação.
8. Clique em **Criar unidade publicitária** (se você estiver criando uma nova unidade publicitária) ou **Salvar** (se você estiver editando uma unidade publicitária existente).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Redes de anúncios pagas com suporte

A tabela a seguir lista as redes pagas atualmente com suporte para cada tipo de anúncio. Observe que algumas dessas redes [não estão disponíveis em todos os mercados](#network-markets).

|  Rede de publicidade  |  Descrição  |  Tipos de anúncio com suporte  |
|--------------|---------------|---------------------|
| Oath e AppNexus |  Esta é uma rede do AD gerenciada pela Microsoft que atende aos anúncios por meio de nossas redes de parceiros, Oath e AppNexus.<p/>**Observação**: o Oath e o AppNexus sempre são classificados primeiro na lista de **redes pagas** do AD para unidades de faixa publicitária e não podem ser alterados para uma classificação mais baixa para esses tipos de anúncios. | Faixa, Vídeo intersticial |
| AppNexus (direto) | Selecione esta opção para servir anúncios de [AppNexus](https://www.appnexus.com). | Vídeo intersticial, Nativo  |
| Anúncios de instalação de Aplicativos Microsoft | Selecione esta opção para veicular anúncios de instalação de aplicativo ou anúncios de novo envolvimento de aplicativos criados por outros desenvolvedores no ecossistema do Windows, que [cria campanhas publicitárias promocionais para seus aplicativos](create-an-ad-campaign-for-your-app.md).  |  Faixa, Vídeo intersticial, Nativo  |
| Recomendações de conteúdo do MSN |  Selecione esta opção para atender anúncios de recomendações de conteúdo do MSN. |  Faixa, faixa intersticial  |
| Outbrain |  Selecione esta opção para veicular anúncios da [Outbrain](https://www.outbrain.com/). |  Faixa, faixa intersticial  |
| Revcontent |  Selecione esta opção para veicular anúncios da [Revcontent](https://www.revcontent.com/). |  Faixa, nativo  |
| Smaato |  Selecione esta opção para veicular anúncios da [Smaato](https://www.smaato.com/). |  Faixa  |
| smartclip |  Selecione esta opção para veicular anúncios da [smartclip](http://www.smartclip.com/). |  Intersticial em vídeo  |
| SpotX |  Selecione esta opção para veicular anúncios da [SpotX](https://www.spotx.tv/). |  Intersticial em vídeo  |
| Taboola |  Selecione esta opção para veicular anúncios da [Taboola](https://www.taboola.com/). |  Faixa  |
| Vungle | Selecione esta opção para servir anúncios de [Vungle](https://vungle.com/) | Intersticial em vídeo |
| Undertone | Selecione esta opção para servir anúncios de [undertone](https://www.undertone.com/). | Intersticial de faixa |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Outras redes de publicidade

A tabela a seguir lista as outras redes atualmente com suporte para cada tipo de anúncio.

|  Rede de publicidade  |  Descrição  |  Tipos de anúncio com suporte  |
|--------------|---------------|---------------------|
| Anúncios da comunidade da Microsoft |  Se você [criar uma campanha publicitária promocional para um dos seus aplicativos](create-an-ad-campaign-for-your-app.md) e configurá-la como uma [campanha de anúncio da comunidade](about-community-ads.md), selecione estas opções para mostrar anúncios dessa campanha. | Faixa, faixa intersticial |
| Anúncios Internos da Microsoft | Se você [criar uma campanha publicitária promocional para um dos seus aplicativos](create-an-ad-campaign-for-your-app.md) e configurá-la como uma [campanha de anúncio interno](about-house-ads.md), selecione esta opção para mostrar anúncios dessa campanha. | Faixa, faixa intersticial  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Mercados com suporte para redes de publicidade

As redes de publicidade disponíveis veiculam anúncios em todos os [mercados com suporte](define-market-selection.md#microsoft-store-consumer-markets), com as seguintes exceções.

|  Rede de publicidade  |  Mercados com suporte  |
|--------------|---------------------|
| Revcontent | Brasil, Canadá, França, Alemanha, Itália, Japão, Espanha, Reino Unido, Estados Unidos  |
| Smaato | Brasil, Canadá, França, Alemanha, Itália, Japão, Espanha, Reino Unido, Estados Unidos |
| smartclip | Áustria, Bélgica, Dinamarca, Finlândia, Alemanha, Itália, Países Baixos, Noruega, Suécia, Suíça  |
| Undertone | Estados Unidos |

<span id="coppa" />

## <a name="coppa-compliance"></a>Conformidade com o COPPA

Quando você [cria uma unidade de anúncio](#create-ad-unit) ou [seleciona uma unidade existente do AD](#available-ad-units), a seção de conformidade do **Coppa** é exibida na parte inferior da página se o aplicativo selecionado para a unidade do AD tiver pelo menos um envio que tenha atingido o na etapa de [armazenamento](../publish/the-app-certification-process.md#in-the-store) no processo de certificação do aplicativo.

Para fins do Children's Online Privacy Protection Act (“COPPA”), você deve selecionar **Este aplicativo é direcionado para crianças com menos de 13 anos** nessa seção se seu aplicativo é direcionado para crianças com menos de 13 anos. Se você selecionar essa opção, a Microsoft seguirá as etapas para desabilitar seus serviços de publicidade comportamental ao oferecer um anúncio em seu aplicativo.

A configuração **Conformidade com o COPPA** escolhida por você é automaticamente aplicada a todas as unidades publicitárias do aplicativo selecionado.

> [!IMPORTANT]
> Caso seu aplicativo seja direcionado para crianças com menos de 13 anos, você tem determinadas obrigações segundo COPPA. Para obter mais informações sobre as obrigações, consulte [esta página](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule).
