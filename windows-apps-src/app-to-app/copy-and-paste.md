---
description: "Este artigo explica como dar suporte a copiar e colar em aplicativos da Plataforma Universal do Windows (UWP) usando a área de transferência."
title: Copiar e colar
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: awkoren
ms.sourcegitcommit: bf081c07f8235790b99b3c1037751f24a86bbc1f
ms.openlocfilehash: ed1dc1ca0f34f0efafd14aa1cfd1e4b75351882c

---
#Copiar e colar

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo explica como dar suporte a copiar e colar em aplicativos da Plataforma Universal do Windows (UWP) usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre aplicativos ou dentro de um aplicativo, e quase todos os aplicativos podem suportar operações da área de transferência em algum grau.

## Verifique Suporte interno da área de transferência


Em muitos casos, você não precisa gravar código para suportar operações da área da transferência. Muitos dos controles XAML padrão que você pode usar para criar aplicativos já permitem operações na área da transferência. 

## Prepare-se para começar

Primeiro, inclua o namespace [**Windows.ApplicationModel.DataTransfer**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer) no seu aplicativo. Em seguida, adicione uma instância do objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage). Esse objeto contém os dados que o usuário quer copiar e quaisquer propriedades (como uma descrição) que você quer incluir.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

AuthenticateAsync

## Copiar e cortar

Copiar e cortar (também chamado de mover) funcionam quase exatamente da mesma maneira. Escolha qual operação deseja usando a propriedade [**RequestedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage.RequestedOperation).

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```
Arrastar e soltar Em seguida, você pode adicionar os dados que um usuário selecionou para o objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage). Se esses dados tiverem suporte pela classe **DataPackage**, você poderá usar um dos métodos correspondentes no objeto **DataPackage**. Veja a seguir como adicionar texto:

```cs
dataPackage.SetText("Hello World!");
```

A última etapa é adicionar o [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) à área de transferência chamando o método estático [**SetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.SetContent(Windows.ApplicationModel.DataTransfer.DataPackage)).

```cs
Clipboard.SetContent(dataPackage);
```
## Colar

Para obter o conteúdo da área de transferência, chame o método estático [**GetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.GetContent). Este método retorna um [**DataPackageView**][DataPackageView] que contém o conteúdo. Este objeto é quase idêntico a um objeto [**DataPackage**][DataPackage], exceto que o seu conteúdo é somente leitura. Com esse objeto, você pode usar o método [**AvailableFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.AvailableFormats) ou [**Contains**][Contains] para identificar quais formatos estão disponíveis. Em seguida, você pode chamar o método correspondente **DataPackageView** para obter os dados.

```cs
DataPackageView dataPackageView = Clipboard.GetContent();
if (dataPackageView.Contains(StandardDataFormats.Text))
{
    string text = await dataPackageView.GetTextAsync();
    // To output the text from this example, you need a TextBlock control
    TextOutput.Text = "Clipboard now contains: " + text;
}
```

## Acompanhar mudanças na área de transferência

Além dos comandos de copiar e colar, você também pode controlar alterações de área de transferência. Faça isso manipulando o evento [**ContentChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.ContentChanged) da área da transferência.

```cs
Clipboard.ContentChanged += (s, e) => 
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

## Consulte também

* [DataTransfer](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.aspx)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackageView](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
* [RequestedOperation](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx) 
* [ControlsList](https://msdn.microsoft.com/library/windows/apps/xaml/mt185406.aspx)
* [SetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx)
* [GetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx)
* [AvailableFormats](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx)
* [Contém](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx)
* [ContentChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx)




<!--HONumber=Jun16_HO3-->


