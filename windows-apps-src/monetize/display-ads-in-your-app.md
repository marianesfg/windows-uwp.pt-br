---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: O SDK do Microsoft Advertising oferece várias maneiras de monetizar seu aplicativo com anúncios.
title: Exibir anúncios em seu aplicativo com o SDK do Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, faixa, controle de anúncio, intersticial
ms.localizationpriority: medium
ms.openlocfilehash: 5f318a68a97e98d3da24da16778988ee40c4e1dd
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463778"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Exibir anúncios em seu aplicativo com o SDK do Microsoft Advertising

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://aka.ms/ad-monetization-shutdown)

Aumente suas oportunidades de receita ao colocar anúncios no seu aplicativo UWP (Plataforma Universal do Windows) para Windows 10 usando o SDK do Microsoft Advertising. Nossa plataforma ad monetização oferece uma variedade de formatos de anúncios que podem ser integrados diretamente aos seus aplicativos e dá suporte à mediação com muitas redes populares do AD. Nossa plataforma é compatível com os padrões OpenRTB, VAST 2.x, MRAID 2 e VPAID 3, além de aceitar MOAT e IAS. 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>Introdução</b><br/><br/>
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Instalar o SDK do Microsoft Advertising</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Guias de desenvolvedor</b><br/><br/>
     de 
    <a href="banner-ads.md">anúncios de faixa</a><br/>
    <a href="interstitial-ads.md">Intersticial ads</a>
    <br/>
    <a href="native-ads.md">Anúncios nativos</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Outros recursos</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Configurar unidades do AD no seu aplicativo</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Práticas recomendadas</a>
    <br/>
    <a href="https://docs.microsoft.com/uwp/api/overview/advertising">Referência de API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Etapa 1: Instalar o SDK do Microsoft Advertising

Para começar, instale o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) no computador de desenvolvimento usado para criar seu aplicativo. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Etapa 2: implementar anúncios no aplicativo

O SDK do Microsoft Advertising fornece vários tipos diferentes de controles de anúncios que você pode usar no aplicativo. Escolha quais tipos de anúncios são recomendados para seu cenário e então adicione código ao seu aplicativo para exibir os anúncios. Durante esta etapa, você usará uma unidade de anúncio de teste para ver como o aplicativo renderiza anúncios durante o teste.

### <a name="banner-ads"></a>Anúncios em faixa

Os anúncios em faixa são anúncios de exibição estática que utilizam uma parte de uma página no aplicativo para exibir conteúdo promocional. Esses anúncios podem ser atualizados automaticamente em intervalos regulares. Esse é um bom ponto de partida se você ainda não anuncia em seu aplicativo.

Para obter instruções e exemplos de código, consulte [este artigo](adcontrol-in-xaml-and--net.md).

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Anúncios de faixa intersticiais e vídeo intersticial

São os anúncios de tela inteira que geralmente exigem que o usuário assista a um vídeo ou clique neles para continuar no app ou no jogo. Oferecemos suporte a dois tipos de anúncios intersticiais: vídeo e faixa.

Para obter instruções e exemplos de código, consulte [este artigo](interstitial-ads.md).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Anúncios nativos

São anúncios baseados em componente. Cada parte da parte criativa do anúncio (por exemplo, o título, a imagem, a descrição e o texto do plano de ação) é disponibilizado para o aplicativo como um elemento individual que você pode integrar a ele usando suas próprias fontes, cores e outros componentes da interface do usuário.

Para obter instruções e exemplos de código, consulte [este artigo](native-ads.md).

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Etapa 3: criar uma unidade publicitária e configurar a mediação

Depois de concluir o teste do aplicativo e você estiver pronto para enviá-lo para a loja, crie uma unidade do AD na página [anúncios no aplicativo](../publish/in-app-ads.md) no Partner Center. Em seguida, atualize o código do aplicativo para usar essa unidade publicitária para o aplicativo receba anúncios ativos. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

Por padrão, o aplicativo exibirá anúncios da rede da Microsoft para anúncios pagos. Para maximizar a receita do anúncio, você pode habilitar o [controle de anúncios](ad-mediation-service.md) da unidade publicitária para exibir anúncios de outras redes de publicidade pagas, como o Taboola e o Smaato. Você também pode aumentar os recursos de promoção de apps ao veicular anúncios de campanhas promocionais de aplicativos da Microsoft.

Para começar a usar o controle de anúncios em seu aplicativo UWP, [definir configurações de controle de anúncios](../publish/in-app-ads.md#mediation-settings) para sua unidade publicitária. Por padrão, configuramos automaticamente as configurações de controle usando algoritmos de aprendizado de máquina para ajudá-lo a maximizar sua receita de publicidade em todos os mercados que seu aplicativo suporte. No entanto, você também tem a opção de escolher manualmente as redes que deseja usar. De qualquer forma, as configurações de mediação são totalmente configuradas nos nossos servidores; você não precisa fazer alterações de código em seu app.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Etapa 4: Enviar seu app e analisar o desempenho

Depois de concluir o desenvolvimento de seu aplicativo com o ADS, você pode [enviar seu aplicativo atualizado](https://docs.microsoft.com/windows/uwp/publish/app-submissions) no Partner Center para disponibilizá-lo na loja. Os apps que exibem anúncios devem atender aos requisitos adicionais especificados na [seção 10.10 das Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) e no [Anexo E do Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement).

Depois que o aplicativo for publicado e disponibilizado na loja, você poderá revisar seus [relatórios de desempenho de anúncios](../publish/advertising-performance-report.md) no Partner Center e continuar a fazer alterações nas configurações de mediação para otimizar o desempenho de seus anúncios. Sua receita de publicidade está incluída no seu [resumo de pagamento](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Ajuda adicional

Para obter ajuda adicional usando o SDK do Microsoft Advertising, use os recursos a seguir.

|  {1&gt;Tarefa&lt;1}    | Recurso |               
|----------|-------|
| Relatar um bug ou obter suporte assistido para publicidade     | Visite a [página de suporte](https://developer.microsoft.com/windows/support) e escolha **Anúncios em Apps**.        |
| Obter suporte da comunidade     | Visite o [fórum](https://go.microsoft.com/fwlink/?LinkID=401264).       |
| Baixe os projetos de exemplo que demonstram como adicionar faixa e anúncios intersticiais a aplicativos.     | Consulte os [Exemplos de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Saiba mais sobre as oportunidades de monetização mais recentes para aplicativos do Windows     | Visite [Monetize seus aplicativos](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Aplicativos para Windows 8.1 e Windows Phone 8.x

Para os aplicativos do Windows 8.1 e Windows Phone 8. x, fornecedos [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x). Para obter mais informações sobre como usar este SDK para mostrar anúncio em aplicativos para Windows 8.1 ou Windows Phone 8.x, consulte [este artigo](https://docs.microsoft.com/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Tópicos relacionados

* [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Relatório de desempenho da publicidade](../publish/advertising-performance-report.md)
* [Programa editores do Windows Premium ADS](windows-premium-ads-publishers-program.md)
