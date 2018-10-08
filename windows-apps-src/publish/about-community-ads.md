---
author: jnHs
Description: You can cross-promote your app with apps published by other developers. We call this feature community ads.
title: Sobre anúncios de comunidade
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ada2e0610e81e986deba3ddab5e87547e05fe108
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4418707"
---
# <a name="about-community-ads"></a>Sobre anúncios de comunidade

Se seu aplicativo [exibe anúncios em faixa ou anúncios intersticiais em faixa](../monetize/display-ads-in-your-app.md), você pode promover seu aplicativo com outros desenvolvedores com aplicativos na Microsoft Store para gratuitos. Chamamos este recurso de *anúncios de comunidade*.  

Veja como este programa funciona:

* Depois que você opt-in para anúncios de comunidade conforme descrito a seguir, você pode [criar uma campanha publicitária de comunidade gratuita](create-an-ad-campaign-for-your-app.md). Em seguida, o seu aplicativo compartilhará espaço publicitário promocional com outros desenvolvedores que também aceitarão de anúncios de comunidade. Seu aplicativo mostrará anúncios para os aplicativos publicados por outros desenvolvedores que participam em anúncios de comunidade, e seus aplicativos exibirão anúncios para seu aplicativo.
* Ganhe créditos para o espaço de anúncio promocional em outros aplicativos, mostrando anúncios de comunidade em seu aplicativo. Os créditos são calculados de acordo com o seguinte processo:
  * Para cada país ou região em que um aplicativo que serve anúncios da comunidade está disponível, o valor de eCPM (custo efetivo por mil impressões) da taxa do mercado atual para o país ou a região é multiplicado pelo número de solicitações de anúncios da comunidade feitas pelo seu aplicativo nesse país ou região. Esse valor corresponde aos créditos que você ganhou para o seu aplicativo nesse país ou nessa região.
  * Seus créditos totais acumulados por um determinado tempo são iguais à soma de todos os créditos obtidos em cada país ou região para cada um dos seus aplicativos que está servindo anúncios da comunidade.
* Seus créditos são divididos igualmente entre todas as campanhas publicitárias ativas da comunidade e são convertidos em impressões de anúncios para o seu aplicativo com base nos valores de eCPM da taxa do mercado atual dos países de destino das suas campanhas publicitárias da comunidade.
* Para controlar o desempenho dos anúncios da comunidade no aplicativo, consulte o [relatório de desempenho de publicidade](advertising-performance-report.md).

### <a name="opt-in-to-community-ads"></a>Para aceitar anúncios da comunidade

Antes de criar uma campanha publicitária de comunidade para um dos seus aplicativos, você deve aceitar na **monetizar** &gt; página de **anúncios no app** no painel do Centro de desenvolvimento do Windows.

Para aceitar anúncios da comunidade para um aplicativo UWP:

1. Na seção **configurações de controle** na página de **anúncios no app** , selecione uma unidade de anúncio que você está usando no aplicativo.
2. Se a opção **Permitir que a Microsoft Escolha as melhores configurações de mediação para o seu aplicativo** estiver selecionada, anúncios de comunidade são habilitados para sua unidade publicitária automaticamente. Caso contrário, selecione a configuração de linha de base ou uma configuração específicas de mercado na lista suspensa **destino** e, em seguida, marque a caixa de **anúncios da Microsoft Community** na lista de **outras redes de anúncios** .

    > [!NOTE]
    > Você pode usar os campos de **Peso** para especificar a proporção de anúncios que você deseja mostrar da redes pagas e de outras redes de anúncios, incluindo anúncios de comunidade.

Para aceitar anúncios da comunidade para que o Windows 8. x ou Windows Phone 8. x aplicativo,

1. Na página de **anúncios no app** , marque a caixa **Mostrar anúncios da comunidade em meu aplicativo** .

Você não precisa republicar o aplicativo depois de fazer suas seleções. Depois de aceitar, você poderá selecionar **Anúncio de comunidade (gratuito)** como o tipo de campanha ao [criar uma campanha publicitária ](create-an-ad-campaign-for-your-app.md).

### <a name="related-topics"></a>Tópicos relacionados

* [Anúncios no aplicativo](in-app-ads.md)
* [Criar uma campanha publicitária para seu aplicativo](create-an-ad-campaign-for-your-app.md)
