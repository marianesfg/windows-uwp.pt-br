---
description: Este artigo explica como dar suporte a copiar e colar em aplicativos da Plataforma Universal do Windows (UWP) usando a área de transferência.
title: Copiar e colar
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dfa5c15c2bd4d82588e0b197dc265c4b529e64c9
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302680"
---
# <a name="copy-and-paste"></a>Copiar e colar

Este artigo explica como dar suporte a copiar e colar em aplicativos da Plataforma Universal do Windows (UWP) usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre aplicativos ou dentro de um aplicativo, e quase todos os aplicativos podem suportar operações da área de transferência em algum grau. Para obter exemplos de código completos que demonstram vários cenários diferentes de copiar e colar, consulte a [amostra da área de transferência](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard).

## <a name="check-for-built-in-clipboard-support"></a>Verifique Suporte interno da área de transferência

Em muitos casos, você não precisa gravar código para suportar operações da área da transferência. Muitos dos controles XAML padrão que você pode usar para criar aplicativos já permitem operações na área da transferência. 

## <a name="get-set-up"></a>Prepare-se

Primeiro, inclua o namespace [**Windows.ApplicationModel.DataTransfer**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer) no seu aplicativo. Em seguida, adicione uma instância do objeto [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Esse objeto contém os dados que o usuário quer copiar e quaisquer propriedades (como uma descrição) que você quer incluir.

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Copiar e cortar

Copiar e cortar (também chamado de *mover*) funcionam quase exatamente da mesma maneira. Escolha qual operação deseja usando a propriedade [**RequestedOperation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation).

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>Definir o conteúdo copiado

Em seguida, você pode adicionar os dados que um usuário selecionou para o objeto [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Se esses dados forem compatíveis com a classe **DataPackage** , você poderá usar um dos [métodos](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods) correspondentes do objeto **DataPackage** . Veja como adicionar texto usando o método [**SetText**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) :

```cs
dataPackage.SetText("Hello World!");
```

A última etapa é adicionar o [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) à área de transferência chamando o método estático [**SetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent).

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>Colar

Para obter o conteúdo da área de transferência, chame o método estático [**GetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent). Esse método retorna um [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) que inclui o conteúdo. Esse objeto é quase idêntico a um objeto [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage), com a diferença de que seu conteúdo é somente leitura. Com esse objeto, você pode usar o método [**AvailableFormats**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) ou [**Contains**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) para identificar quais formatos estão disponíveis. Em seguida, você pode chamar o método correspondente [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) para obter os dados.

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>Acompanhar mudanças na área de transferência

Além dos comandos de copiar e colar, você também pode controlar alterações de área de transferência. Faça isso manipulando o evento [**ContentChanged**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) da área da transferência.

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>Consulte também

* [Exemplo da área de transferência](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [Comunicação de aplicativo a aplicativo](index.md)
* [DataTransfer](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [Solicitação de DataPedido](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [Controle da](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)
* [SetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [AvailableFormats](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Terá](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [Contentchanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)

