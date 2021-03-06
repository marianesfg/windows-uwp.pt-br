---
description: Este artigo explica como dar suporte ao contrato de Compartilhamento em um aplicativo da Plataforma Universal do Windows (UWP).
title: Compartilhar dados
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2be088edd732a22acb11be5fc209ff25c84bae17
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218346"
---
# <a name="share-data"></a>Compartilhar dados


Este artigo explica como dar suporte ao contrato de Compartilhamento em um aplicativo da Plataforma Universal do Windows (UWP). O contrato de Compartilhamento é uma maneira fácil de compartilhar dados como texto, links, fotos e vídeos entre aplicativos rapidamente. Por exemplo, um usuário pode querer compartilhar uma página da Web com seus amigos usando um aplicativo de rede social ou salvar um link em um aplicativo de anotações para consultar mais tarde.

> [!NOTE]
> Os exemplos de código neste artigo são escritos para aplicativos UWP. Os aplicativos de área de C++trabalho do WPF, Windows Forms e/Win32 devem usar a interface [IDataTransferManagerInterop](https://docs.microsoft.com/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) para obter o objeto [datatransfermanager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) para uma janela específica. Para obter mais informações, consulte o exemplo de [ShareName](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) .

## <a name="set-up-an-event-handler"></a>Configurar um manipulador de eventos

Adicione um manipulador de eventos [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) para ser chamado sempre que um usuário invocar o compartilhamento. Isso pode ocorrer quando o usuário toca em um controle no aplicativo (por exemplo, um botão ou um comando da barra de aplicativos) ou automaticamente em um cenário específico (se o usuário terminar um nível e obtiver uma pontuação alta, por exemplo).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

Quando ocorre um evento [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested), seu aplicativo recebe um objeto [**DataRequest**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest). Este contém um [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) que você pode usar para fornecer o conteúdo que o usuário quer compartilhar. Você deve fornecer um título e os dados a serem compartilhados. Uma descrição é opcional, mas recomendada.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>Escolher dados

Você pode compartilhar vários tipos de dados, incluindo:

-   Texto simples
-   Uniform Resource Identifiers (URIs)
-   HTML
-   Texto formatado
-   Bitmaps
-   Files
-   Dados personalizados definidos pelo desenvolvedor

O objeto [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) pode conter um ou mais desses formatos, em qualquer combinação. O exemplo a seguir demonstra o compartilhamento de texto.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>Definir propriedades

Ao empacotar dados para compartilhamento, você pode fornecer uma variedade de propriedades que dão informações adicionais sobre o conteúdo compartilhado. Essas propriedades podem ajudar os aplicativos de destino a melhorar a experiência do usuário. Por exemplo, uma descrição ajuda quando o usuário está compartilhando conteúdo com mais de um aplicativo. A adição de uma miniatura ao compartilhar uma imagem ou um link para uma página da Web fornece uma referência visual ao usuário. Para obter mais informações, consulte [**DataPackagePropertySet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Todas as propriedades, exceto o título, são opcionais. A propriedade de título é obrigatória e deve ser definida.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>Iniciar a interface do usuário de compartilhamento

Uma interface do usuário para compartilhamento é fornecida pelo sistema. Para iniciá-la, chame o método [**ShowShareUI**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>Manipular erros

Na maioria dos casos, o compartilhamento de conteúdo é um processo simples e direto. Contudo, há sempre a chance de algo inesperado acontecer. Por exemplo, o aplicativo pode exigir que o usuário selecione conteúdo para compartilhamento, mas ele não faz essa seleção. Para lidar com essas situações, use o método [**FailWithDisplayText**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_), que exibirá uma mensagem ao usuário se algo der errado.

## <a name="delay-share-with-delegates"></a>Atrasar o compartilhamento com delegados

Às vezes, pode não fazer sentido preparar imediatamente os dados que o usuário deseja compartilhar. Por exemplo, se o seu aplicativo oferece suporte ao envio de um arquivo de imagem grande em diversos formatos possíveis, é ineficiente criar todas essas imagens antes de o usuário fazer sua seleção.

Para resolver o problema, um [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) pode conter um delegado - uma função que é chamada quando o aplicativo de recebimento solicita dados. Recomendamos o uso de um delegado sempre que os dados que um usuário deseja compartilhar usarem muitos recursos.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>Consulte também 

* [Comunicação de aplicativo a aplicativo](index.md)
* [Receber dados](receive-data.md)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [Solicitação de DataPedido](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 

