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
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2883606"
---
# <a name="about-community-ads"></a>Sobre anúncios de comunidade

Se seu aplicativo [exibe banner ou anúncios intersticiais faixa](../monetize/display-ads-in-your-app.md), poderá cross-promover seu aplicativo com outros desenvolvedores com aplicativos no Microsoft Store para gratuito. Chamamos este recurso de *anúncios de comunidade*.  

Veja como este programa funciona:

* Depois que você opt-in para anúncios de comunidade, conforme descrito abaixo, você pode [criar uma campanha de ad gratuito para a comunidade](create-an-ad-campaign-for-your-app.md). Seu aplicativo, em seguida, compartilharão espaço da ad promocional com outros desenvolvedores que optarem também anúncios de comunidade. Seu aplicativo mostrará anúncios para os aplicativos publicados por outros desenvolvedores que participam em anúncios de comunidade, e seus aplicativos exibirão anúncios para seu aplicativo.
* Ganhe créditos para o espaço de anúncio promocional em outros aplicativos, mostrando anúncios de comunidade em seu aplicativo. Os créditos são calculados de acordo com o seguinte processo:
  * Para cada país ou região em que um aplicativo que serve anúncios da comunidade está disponível, o valor de eCPM (custo efetivo por mil impressões) da taxa do mercado atual para o país ou a região é multiplicado pelo número de solicitações de anúncios da comunidade feitas pelo seu aplicativo nesse país ou região. Esse valor corresponde aos créditos que você ganhou para o seu aplicativo nesse país ou nessa região.
  * Seus créditos totais acumulados por um determinado tempo são iguais à soma de todos os créditos obtidos em cada país ou região para cada um dos seus aplicativos que está servindo anúncios da comunidade.
* Seus créditos são divididos igualmente entre todas as campanhas publicitárias ativas da comunidade e são convertidos em impressões de anúncios para o seu aplicativo com base nos valores de eCPM da taxa do mercado atual dos países de destino das suas campanhas publicitárias da comunidade.
* Para controlar o desempenho dos anúncios da comunidade no aplicativo, consulte o [relatório de desempenho de publicidade](advertising-performance-report.md).

### <a name="opt-in-to-community-ads"></a>Para aceitar anúncios da comunidade

Antes de criar uma campanha publicitária de comunidade de um dos seus aplicativos, você deve aceitar no **Monetize** &gt; página **no aplicativo ads** no painel do Centro de desenvolvimento do Windows.

Optarem ads da comunidade para um aplicativo UWP:

1. Na seção **configurações de mediação** na página **ads - app** , selecione uma unidade de anúncio que você está usando o aplicativo.
2. Se a opção **Permitir que o Microsoft escolher as configurações recomendadas de mediação para seu aplicativo** estiver selecionada, os anúncios de comunidade estão habilitados para sua unidade de ad automaticamente. Caso contrário, selecione a configuração de linha de base ou uma configuração de mercado específico na lista suspensa **destino** e verifique se a caixa **ads na comunidade Microsoft** na lista de **outras redes ad** .

    > [!NOTE]
    > Você pode usar os campos de **Peso** para especificar a proporção de anúncios que você deseja mostrar de redes pagas e outras redes ad incluindo anúncios de comunidade.

Optarem ads da comunidade para o Windows 8. x ou do aplicativo do Windows Phone 8. x,

1. Na página **ads - app** , marque a caixa de **Mostrar anúncios de comunidade em meu aplicativo** .

Você não precisa republicar o aplicativo depois de fazer suas seleções. Depois de aceitar, você poderá selecionar **Anúncio de comunidade (gratuito)** como o tipo de campanha ao [criar uma campanha publicitária ](create-an-ad-campaign-for-your-app.md).

### <a name="related-topics"></a>Tópicos relacionados

* [Anúncios no aplicativo](in-app-ads.md)
* [Criar uma campanha publicitária para seu aplicativo](create-an-ad-campaign-for-your-app.md)
