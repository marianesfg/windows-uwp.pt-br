---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "Saiba como usar a classe AdControl para exibir anúncios em faixa em um aplicativo JavaScript/HTML para Windows 10 (UWP), Windows 8.1 ou Windows Phone 8.1."
title: AdControl em HTML 5 e JavaScript
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 501edf178ecccf8a6b62d4602837dbbdf820d744

---

# AdControl em HTML 5 e JavaScript




Este guia passo a passo mostra como usar a classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) para exibir anúncios em faixa em um aplicativo JavaScript/HTML para Windows 10 (UWP), Windows 8.1 ou Windows Phone 8.1. Este guia passo a passo não usa o **AdMediatorControl** nem a mediação de anúncios.

Para um projeto de exemplo completo que demonstra como adicionar anúncios em faixa a um aplicativo JavaScript/HTML, consulte os [Exemplos de publicidade no GitHub](http://aka.ms/githubads).

## Pré-requisitos


* Para aplicativos UWP: instale o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) com o Visual Studio 2015.
* Em aplicativos do Windows 8.1 ou Windows Phone 8.1: instale o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.

> **Observação** Se você instalou a Compilação 14295 do Windows 10 Anniversary SDK Preview ou superior com o Visual Studio 2015, também deverá instalar a biblioteca WinJS. Essa biblioteca costumava ser incluída em versões anteriores do SDK do Windows para Windows 10, mas a partir da Compilação do 14295 Windows 10 Anniversary SDK Preview, ela deve ser instalada separadamente. Para instalar o WinJS, consulte [Baixar o WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## Desenvolvimento de código

1. No Visual Studio, abra o projeto ou crie um novo projeto.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

4.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

    -   Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).

    -   Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows 8.1 nativo (JS)**.

    -   Para um projeto do Windows 8.1: expanda **Windows Phone 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows Phone 8.1 nativo (JS)**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **Observação**  Esta imagem é do Visual Studio 2015 compilando um projeto UWP para Windows 10. Se você estiver compilando um aplicativo do Windows 8.1 ou Windows Phone 8.1 ou usando o Visual Studio 2013, sua tela terá uma aparência diferente.

5.  No **Gerenciador de Referências**, clique em OK.

6.  Abra o arquivo default.html (ou outro arquivo html apropriado para o seu projeto).

7.  Na seção **&lt;head&gt;**, após as referências JavaScript de default.css e default.js do projeto, adicione a referência ao ad.js.

    Em um projeto UWP, adicione o código a seguir.

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    Em um projeto do Windows 8.1 ou Windows Phone 8.1, adicione o código a seguir.

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > **Observação**&nbsp;&nbsp;Esta linha deve ser colocada na seção **&lt;head&gt;** após a inclusão do default.js; caso contrário, você encontrará um erro ao compilar seu projeto.

8.  Modifique a seção **&lt;body&gt;** no arquivo default.html (ou outro arquivo html apropriado para o seu projeto) para incluir o elemento div do **AdControl**. Atribua as propriedades **applicationId** e **adUnitId** no **AdControl** para testar os valores fornecidos em [Valores de modo de teste](test-mode-values.md), e ajuste a altura e a largura do controle para que ele fique com um dos [tamanhos de anúncio compatíveis com anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > **Observação**&nbsp;&nbsp;Você substituirá os valores de teste **applicationId** e **adUnitId** por valores dinâmicos antes de enviar seu aplicativo.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
          data-win-control="MicrosoftNSJS.Advertising.AdControl"
          data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

9.  Compile e execute o aplicativo para vê-lo com um anúncio.

## Lançar seu aplicativo com anúncios dinâmicos usando o Centro de Desenvolvimento do Windows


1.  No painel do Centro de Desenvolvimento, vá para a página **Monetização** &gt; **Monetizar com anúncios** para seu aplicativo e [crie uma unidade autônoma do Microsoft Advertising](../publish/monetize-with-ads.md). Para obter o tipo de unidade de anúncio, especifique **Banner**. Anote o ID da unidade de anúncio e o ID do aplicativo.

2.  Em seu código, substitua os valores da unidade de anúncio de teste (**applicationId** e **adUnitId**) pelos valores dinâmicos gerados no Centro de Desenvolvimento.

3.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento.

4.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

## Conclua o arquivo default.html de um projeto UWP de exemplo.


``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>My_Windows_10_Ad_Funded_JavaScript_App</title>

    <!-- WinJS references -->
    <link href="//Microsoft.WinJS.2.0.Preview/css/ui-dark.css" rel="stylesheet" />
    <script src="//Microsoft.WinJS.2.0.Preview/js/base.js"></script>
    <script src="//Microsoft.WinJS.2.0.Preview/js/ui.js"></script>

    <!-- My_Windows_10_Ad_Funded_JavaScript_App references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>

    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
 

 



<!--HONumber=Sep16_HO2-->


