---
author: jnHs
Description: "Se seu app usa um AdMediatorControl ou AdControl para exibir anúncios na barra de notificação, você pode aumentar sua taxa de preenchimento de anúncio e receita mostrando anúncios de afiliada da Microsoft em seu app."
title: "Sobre os anúncios de afiliadas"
ms.assetid: 7B5478FB-7E68-4956-82EF-B43C2873E3EF
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c114b7fa5c0e6a613a2c5485903d856592f0863e
ms.lasthandoff: 02/07/2017

---

# <a name="about-affiliate-ads"></a>Sobre os anúncios de afiliadas

Se o app usar um **AdMediatorControl** ou **AdControl** para exibir anúncios, você poderá aumentar sua receita e a taxa de preenchimento de anúncios exibindo anúncios da afiliada Microsoft em seu app para produtos da Loja. Quando os usuários clicam nos anúncios da afiliada e compram produtos em uma determinada janela de atribuição, você obtém receita sobre as compras aprovadas.

Veja como este programa funciona:

* Depois que você [aceita o programa de anúncios de afiliadas da Microsoft](#opt-in) no Centro de Desenvolvimento, a Microsoft seleciona anúncios dos principais produtos na Loja compatíveis com seu app. Esses produtos podem incluir apps, jogos, músicas, filmes, hardware e software.

 > **Observação** A Microsoft veicula anúncios de afiliada apenas nos tamanhos de unidade de anúncio: 300 x 50, 480 x 80, 160 x 600, 300 x 250 ou 728 x 90.

* A Microsoft veiculará anúncios de afiliadas em seu app apenas quando nenhum anúncio de outras redes de anúncios estiverem disponíveis. Isso se destina a ajudar a monetizar seus impressões não atendidas e maximizar a taxa de preenchimento de anúncios.
* Quando um usuário clicar em um anúncio da afiliada e comprar qualquer produto na Loja em uma determinada janela de atribuição, você receberá uma parte da receita ou uma comissão fixa pela compra (até 10% de comissão).

  Para ser qualificado para uma comissão, o anúncio de afiliada deve resultar em uma venda qualificada na Loja em dispositivos Windows 10 ou na Loja baseada na Web; vendas na Loja em dispositivos Windows 8. x não estão qualificadas para uma comissão. Os anúncios de afiliada para produtos digitais da Loja (incluindo apps, jogos, músicas e filmes) são veiculados apenas em dispositivos Windows 10. Os anúncios de afiliada para produtos na Loja baseada na Web (incluindo dispositivos e software) são veiculados nos dispositivos Windows 10 e Windows 8.x.

    > **Observação**  Você pode receber o pagamento para *qualquer* produto que o usuário comprar na janela de atribuição, não apenas o produto oferecido em seu app. Para apps gratuitos oferecidos em seu app, você poderá ganhar uma participação de receita para compras no app feitas pelo usuário na janela de atribuição.

* Quaisquer ganhos acumulados com relação ao programa de anúncios da afiliada Microsoft serão distribuídos para você na [conta de pagamento configurada no Centro de Desenvolvimento](setting-up-your-payout-account-and-tax-forms.md), juntamente com seus lucros de publicidade da Microsoft.
* Para controlar o desempenho dos anúncios da afiliada em seu app, veja o [relatório de desempenho de afiliadas](affiliates-performance-report.md). Você pode controlar as compras diárias feitas por meio dos anúncios da afiliada em seu app e a participação na receita recebida.  

  > **Observação** Depois que um usuário compra um produto na Loja, há um período de espera de 45 dias antes que a compra seja aprovada para o programa de anúncios de afiliada. Por causa desse período de espera, os dados de **Receita estimada**, **Compras (aprovadas)** e **Compras (aprovação pendente)** sobre o [relatório de desempenho de afiliadas](affiliates-performance-report.md) para um determinado dia podem mudar depois que as compras forem aprovadas ou rejeitadas.

<span id="opt-in" />
## <a name="how-to-opt-in-to-the-affiliate-ads-program"></a>Como aceitar o programa de anúncios da afiliada

Para aceitar o programa de anúncios da afiliada Microsoft:

1. Vá para a página **Monetização** &gt; **Monetizar com anúncios** no painel do Centro de Desenvolvimento do Windows.
2. Na seção **Anúncios da afiliada Microsoft**, marque a caixa **Mostrar anúncios da afiliada Microsoft em meu app**.

Depois de marcar ou desmarcar essa caixa, você não precisará republicar seu app para que as alterações entrem em vigor.


## <a name="related-topics"></a>Tópicos relacionados


* [Monetizar com anúncios](monetize-with-ads.md)
* [Relatório de desempenho de afiliadas](affiliates-performance-report.md)

