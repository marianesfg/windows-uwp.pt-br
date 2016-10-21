---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Saiba mais sobre como instalar as bibliotecas do Microsoft Advertising.
title: Instalar as bibliotecas do Microsoft Advertising
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: a18822b167b7b4dab2dee02439c82c1a0dafcf31


---

# Instalar as bibliotecas do Microsoft Advertising




Para aplicativos da Plataforma Universal do Windows (UWP) para Windows 10, as bibliotecas do Microsoft Advertising estão incluídas no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Esse SDK é uma extensão do Visual Studio 2015. Para saber mais sobre esse SDK, consulte [este artigo](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk).

> **Observação**&nbsp;&nbsp;Se você instalou o SDK do Windows 10 (14393) ou posterior, também precisará instalar a biblioteca WinJS se quiser adicionar anúncios a um aplicativo UWP JavaScript/HTML. Essa biblioteca costumava ser incluída em versões anteriores do SDK do Windows 10, mas a partir do SDK do Windows 10 (14393), ela deve ser instalada separadamente. Para instalar o WinJS, consulte [Baixar o WinJS](http://try.buildwinjs.com/download/GetWinJS/).

Para aplicativos XAML e JavaScript/HTML para Windows 8.1 e Windows Phone 8.x, as bibliotecas do Microsoft Advertising estão incluídas no [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk). Esse SDK é uma extensão do Visual Studio 2015 e do Visual Studio 2013.

Para aplicativos do Windows Phone Silverlight 8.x, as bibliotecas do Microsoft Advertising estão disponíveis em um pacote NuGet que você pode baixar e instalar no seu projeto. Para saber mais, consulte [AdControl no Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## Nomes de bibliotecas para publicidade


Há várias bibliotecas de publicidade diferentes disponíveis no Microsoft Store Services SDK e no SDK do Microsoft Advertising para Windows e Windows Phone 8.x:

* O Microsoft Store Services SDK inclui as bibliotecas do Microsoft Advertising (que fornecem as classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para aplicativos XAML e JavaScript/HTML).

* O SDK do Microsoft Advertising para Windows e Windows Phone 8.x inclui dois conjuntos de bibliotecas de publicidade: as bibliotecas do Microsoft Advertising (que fornecem as classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para aplicativos XAML e JavaScript/HTML) e as bibliotecas para mediação de anúncios (que fornecem a classe **AdMediatorControl**).

Esta documentação descreve como usar as classes **AdControl** e **InterstitialAd** nas bibliotecas do Microsoft Advertising para exibir anúncios em faixa ou anúncios intersticiais de vídeo. Para obter informações sobre como usar a mediação de anúncios para os aplicativos Windows 8.1 e Windows Phone 8.x, consulte [Usar mediação de anúncios para maximizar a receita](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Observação**&nbsp;&nbsp;A mediação de anúncios usando a classe **AdMediatorControl** atualmente não é permitida para aplicativos UWP usando o Windows 10. O controle do servidor estará disponível em breve para aplicativos UWP usando as mesmas APIs para anúncios em faixa (**AdControl**) e anúncios intersticiais em vídeo (**InterstitialAd**).

Para poder usar qualquer um dos controles de publicidade no código do aplicativo, você deve fazer referência à biblioteca apropriada em seu projeto. As tabelas a seguir listam os nomes de cada biblioteca conforme eles aparecem na caixa de diálogo **Gerenciador de Referências** no Visual Studio.


<table>
    <thead>
        <tr><th>Nome do controle</th><th>Tipo de projeto</th><th>Nome da biblioteca no Gerenciador de Referências</th><th>Número de versão</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** e **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>SDK do Microsoft Advertising para XAML</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>SDK do Ad Mediator para Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>SDK do Ad Mediator para Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** e **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>SDK do Microsoft Advertising para JavaScript</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>SDK do Microsoft Advertising para Windows 8.1 Nativo (JS)</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>SDK do Microsoft Advertising para Windows Phone 8.1 Nativo (JS)</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl** (somente XAML)</td>
            <td>UWP</td>
            <td>SDK do Microsoft Advertising Universal</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>SDK do Ad Mediator para Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>SDK do Ad Mediator para Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Sep16_HO2-->


