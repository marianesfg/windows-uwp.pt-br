---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Saiba mais sobre como instalar as bibliotecas do Microsoft Advertising.
title: Instalar as bibliotecas do Microsoft Advertising

---

# Instalar as bibliotecas do Microsoft Advertising


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

As bibliotecas do Microsoft Advertising para aplicativos do Windows estão incluídas no [SDK do Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Esse SDK é uma extensão do Visual Studio 2015 e do Visual Studio 2013.

Para obter instruções sobre instalação, consulte [Monetizar seu aplicativo e envolver os clientes com o SDK do Microsoft Store Engagement and Monetization](https://msdn.microsoft.com/windows/uwp/monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk).

> **Observação** Se você instalou a Compilação 14295 do Windows 10 Anniversary SDK Preview ou posterior com o Visual Studio 2015, também precisará instalar a biblioteca WinJS se quiser adicionar anúncios a um aplicativo JavaScript/HTML. Essa biblioteca costumava ser incluída em versões anteriores do SDK do Windows para Windows 10, mas a partir da Compilação 14295 do Windows 10 Anniversary SDK Preview, ela deve ser instalada separadamente. Para instalar o WinJS, consulte [Baixar o WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## Nomes de bibliotecas para publicidade e mediação de anúncios


O SDK do Microsoft Store Engagement and Monetization inclui dois conjuntos de bibliotecas de publicidade: as bibliotecas do Microsoft Advertising (que fornecem as classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para aplicativos XAML e JavaScript/HTML) e as bibliotecas para bibliotecas de mediação de anúncios (que fornecem a classe **AdMediatorControl**).

Esta documentação descreve como usar as classes **AdControl** e **InterstitialAd** nas bibliotecas do Microsoft Advertising para exibir anúncios em faixa ou anúncios intersticiais de vídeo da Microsoft. Para obter mais informações sobre como usar mediação de anúncios, consulte [Usar a mediação de anúncios para maximizar a receita](https://msdn.microsoft.com/windows/uwp/monetize/use-ad-mediation-to-maximize-revenue).


Para poder usar qualquer um dos controles de publicidade no código do aplicativo, você deve fazer referência à biblioteca apropriada em seu projeto. As tabelas a seguir listam os nomes de cada biblioteca no SDK do Microsoft Store Engagement and Monetization conforme eles aparecem na caixa de diálogo **Gerenciador de Referências** no Visual Studio.


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

 

 

 


<!--HONumber=May16_HO2-->


