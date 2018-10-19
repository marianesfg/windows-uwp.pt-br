---
author: Xansky
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Saiba como usar o namespace Windows.Services.Store para obter informações de licença para o aplicativo atual e seus complementos.
title: Obter informações de licença para seus aplicativos e complementos
ms.author: mhopkins
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, licenças, aplicativos, complementos, compras no aplicativo, IAPs, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 83889dce2959a3d373081808864a6b7913fb142b
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4945583"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obter informações de licença para apps e complementos

Este artigo demonstra como usar métodos da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) a fim de obter informações de licença do aplicativo atual ou um de seus complementos. Por exemplo, você pode usar essas informações para determinar se as licenças para o aplicativo ou seus complementos estão ativas ou se são licenças de avaliação.

> [!NOTE]
> O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [este artigo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Pré-requisitos

Este exemplo tem os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você [criou um envio de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) no painel do Centro de Desenvolvimento do Windows e esse app foi publicado e disponibilizado na Loja. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Loja enquanto você o testa. Para obter mais informações, consulte as [diretrizes para teste](in-app-purchases-and-trials.md#testing).
* Se você quiser obter informações de licença para um complemento para o aplicativo, também deve [criar o complemento no painel do Centro de Desenvolvimento](../publish/add-on-submissions.md).

O código neste exemplo pressupõe que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado neste exemplo para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemplo de código

Para obter informações de licença para o aplicativo atual, use o método [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync). Trata-se de um método assíncrono que retorna um objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) que fornece informações de licença do aplicativo, inclusive propriedades que indicam se atualmente o usuário tem uma licença válida para usar o aplicativo ([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)) e se a licença se destina a uma versão de avaliação ([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)).

Para acessar as licenças para complementos duráveis do app atual que o usuário tem o direito de usar, use a propriedade [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses) do objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Essa propriedade retorna uma coleção de objetos [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) que representam as licenças do complemento.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Para obter um aplicativo de exemplo completo, consulte o [exemplo da Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
