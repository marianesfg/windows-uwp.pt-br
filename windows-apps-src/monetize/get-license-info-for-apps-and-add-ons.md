---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Saiba como usar o namespace Windows.Services.Store para obter informações de licença para o aplicativo atual e seus complementos.
title: Obter informações de licença para seus aplicativos e complementos
ms.date: 12/04/2017
ms.topic: article
keywords: windows 10, uwp, licenças, aplicativos, complementos, compras no aplicativo, IAPs, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: c8bef40cb34412fbb92585d2ccd25682c72ffe5c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371633"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obter informações de licença para apps e complementos

Este artigo demonstra como usar métodos da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) a fim de obter informações de licença do aplicativo atual ou um de seus complementos. Por exemplo, você pode usar essas informações para determinar se as licenças para o aplicativo ou seus complementos estão ativas ou se são licenças de avaliação.

> [!NOTE]
> O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [este artigo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Pré-requisitos

Este exemplo tem os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você tem [criou um envio de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions) no Partner Center e esse aplicativo é publicado na Store. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Store enquanto você o testa. Para obter mais informações, consulte as [diretrizes para teste](in-app-purchases-and-trials.md#testing).
* Se você quiser obter informações de licença para um complemento para o aplicativo, você deve também [criar o complemento no Partner Center](../publish/add-on-submissions.md).

O código neste exemplo pressupõe que:
* O código seja executado no contexto de uma [Página](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) que contenha um [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` e um [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado neste exemplo para configurar o objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemplo de código

Para obter informações de licença para o aplicativo atual, use o método [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync). Trata-se de um método assíncrono que retorna um objeto [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense) que fornece informações de licença do aplicativo, inclusive propriedades que indicam se atualmente o usuário tem uma licença válida para usar o aplicativo ([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)) e se a licença se destina a uma versão de avaliação ([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)).

Para acessar as licenças para complementos duráveis do app atual que o usuário tem o direito de usar, use a propriedade [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses) do objeto [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense). Essa propriedade retorna uma coleção de objetos [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) que representam as licenças do complemento.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Para obter um app de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações sobre produtos para os aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
