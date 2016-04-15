---
description: Este artigo explica como dar suporte a copiar e colar em aplicativos da Plataforma Universal do Windows (UWP) usando a área de transferência.
title: Copiar e colar
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: awkoren
---
#Copiar e colar

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo explica como dar suporte a copiar e colar em aplicativos da Plataforma Universal do Windows (UWP) usando a área de transferência. Copiar e colar é a maneira clássica de trocar dados entre aplicativos ou dentro de um aplicativo, e quase todos os aplicativos podem suportar operações da área de transferência em algum grau.

## Verifique Suporte interno da área de transferência


Em muitos casos, você não precisa gravar código para suportar operações da área da transferência. Muitos dos controles XAML padrão que você pode usar para criar aplicativos já permitem operações na área da transferência. Para saber mais sobre os controles disponíveis, consulte a [lista de controles][ControlsList].

## Preparar-se

Primeiro, inclua o namespace [**Windows.ApplicationModel.DataTransfer**][DataTransfer] no seu aplicativo. Em seguida, adicione uma instância do objeto [**DataPackage**][DataPackage]. Esse objeto contém os dados que o usuário quer copiar e quaisquer propriedades (como uma descrição) que você quer incluir.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

## Copiar e cortar

Copiar e cortar (também chamado de mover) funcionam quase exatamente da mesma maneira. Escolha qual operação deseja usando a propriedade [**DataPackage.RequestedOperation**][RequestedOperation].

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

Em seguida, você pode adicionar os dados que um usuário selecionou para o objeto [**DataPackage**][DataPackage]. Se esses dados tiverem suporte pela classe **DataPackage**, você poderá usar um dos métodos correspondentes no objeto **DataPackage**. Veja a seguir como adicionar texto:

```cs
dataPackage.SetText("Hello World!");
```

A última etapa é adicionar o [**DataPackage**][DataPackage] à área de transferência chamando o método estático [**Clipboard.SetContent**][SetContent].

```cs
Clipboard.SetContent(dataPackage);
```
## Colar

Para obter o conteúdo da área de transferência, chame o método estático [**Clipboard.GetContent**[GetContent]. Esse método retorna um [**DataPackageView**][DataPackageView] que inclui o conteúdo. Esse objeto é quase idêntico a um objeto [**DataPackage**][DataPackage], com a diferença de que seu conteúdo é somente leitura. Com esse objeto, você pode usar o método [**AvailableFormats**][AvailableFormats] ou [**Contains**][Contains] para identificar quais formatos estão disponíveis. Em seguida, você pode chamar o método correspondente **DataPackageView** para obter os dados.

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

Além dos comandos de copiar e colar, você também pode controlar alterações de área de transferência. Faça isso manipulando o evento [**Clipboard.ContentChanged**][ContentChanged] da área da transferência.

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

<!-- LINKS --> 
[DataTransfer]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.aspx 
[DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
[DataPackageView]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx
[DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
[DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
[DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
[FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
[ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
[RequestedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx 
[ControlsList]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/mt185406.aspx 
[SetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx 
[GetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx
[AvailableFormats]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx 
[Contains]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx
[ContentChanged]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx 



<!--HONumber=Mar16_HO5-->


