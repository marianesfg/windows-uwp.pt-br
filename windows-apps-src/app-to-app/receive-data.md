---
description: Este artigo explica como receber conteúdo em seu aplicativo UWP (Plataforma Universal do Windows) compartilhado de outro aplicativo usando o contrato de Compartilhamento. Este contrato de Compartilhamento permite que seu aplicativo seja apresentado como uma opção quando o usuário invoca o compartilhamento.
title: Receber dados
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e17b9ddd5833899a83e24d24c74f9c620a28f5c8
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7840107"
---
# <a name="receive-data"></a>Receber dados



Este artigo explica como receber conteúdo no aplicativo UWP (Plataforma Universal do Windows) compartilhado de outro app usando o contrato de Compartilhamento. Este contrato de Compartilhamento permite que seu aplicativo seja apresentado como uma opção quando o usuário invoca o compartilhamento.

## <a name="declare-your-app-as-a-share-target"></a>Declarar o aplicativo como um compartilhamento de destino

O sistema exibe uma lista de possíveis aplicativos de destino quando um usuário invoca o compartilhamento. Para aparecer na lista, seu aplicativo precisa declarar que ele oferece suporte ao contrato de Compartilhamento. Isso permite que o sistema saiba que seu aplicativo está disponível para receber conteúdo.

1.  Abra o arquivo de manifesto. Ele deve ser nomeado como algo semelhante a **package.appxmanifest**.
2.  Abra a guia **Declarações**.
3.  Escolha **Compartilhamento de Destino** na lista **Declarações disponíveis** e selecione **Adicionar**.

## <a name="choose-file-types-and-formats"></a>Escolher tipos de arquivo e formatos

Em seguida, decida quais tipos de arquivo e formatos de dados terão suporte. As APIs de Compartilhamento oferecem suporte a vários formatos padrão, como Texto, HTML e Bitmap. Também é possível especificar tipos de arquivo personalizado e formatos de dados. Se você especificar, lembre-se de que os aplicativos de origem precisam saber quais são esses tipos e formatos; caso contrário, eles não poderão usar os formatos para compartilhar dados.

Registre somente os formatos que seu aplicativo pode manipular. São exibidos somente os aplicativos de destino que oferecem suporte aos dados que estão sendo compartilhados quando o usuário invoca o compartilhamento.

Para definir os tipos de arquivo:

1.  Abra o arquivo de manifesto. Ele deve ser nomeado como algo semelhante a **package.appxmanifest**.
2.  Na seção **Tipos de Arquivos com Suporte** da página **Declarações**, selecione **Adicionar novo**.
3.  Digite a extensão de nome de arquivo à qual você deseja dar suporte, por exemplo, ".docx". É necessário incluir o ponto. Se quiser dar suporte a todos os tipos de arquivos, marque a caixa de seleção **SupportsAnyFileType**.

Para definir os formatos de dados:

1.  Abra o arquivo de manifesto.
2.  Abra a seção **Formatos de Dados** da página **Declarações** e selecione **Adicionar Novo**.
3.  Digite o nome do formato de dados compatível, por exemplo, "Texto".

## <a name="handle-share-activation"></a>Manipular a ativação de compartilhamento

Quando um usuário seleciona seu aplicativo (geralmente selecionando-o em uma lista de aplicativos de destino disponíveis na interface do usuário de compartilhamento), um evento [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Application.OnShareTargetActivated(Windows.ApplicationModel.Activation.ShareTargetActivatedEventArgs)) é gerado. Seu aplicativo precisa manipular esse evento para processar os dados que o usuário deseja compartilhar.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

Os dados que o usuário quer compartilhar estão contidos em um objeto [**ShareOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation). Você pode usar esse objeto para verificar o formato dos dados que ele contém.

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>Relatar status do compartilhamento

Em alguns casos, pode levar algum tempo para seu aplicativo processar os dados que deseja compartilhar. Exemplos incluem usuários que compartilham coleções de arquivos ou imagens. Esses itens são maiores do que uma cadeia de texto simples, por isso demoram mais tempo para processar.

```cs
shareOperation.ReportStarted(); 
```

Depois de chamar [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted), não espere mais interação do usuário com seu aplicativo. Como resultado, você não deverá chamá-lo, a menos que o aplicativo esteja em um ponto onde possa ser ignorado pelo usuário.

Com um compartilhamento estendido, é possível que o usuário ignore o aplicativo de origem antes que seu aplicativo tenha todos os dados do objeto DataPackage. Como resultado, recomendamos que o sistema saiba quando o aplicativo adquiriu os dados necessários. Dessa forma, o sistema pode suspender ou encerrar o aplicativo de origem conforme necessário.

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

Se algo der errado, chame [**ReportError**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportError(System.String)) para enviar uma mensagem de erro ao sistema. O usuário verá a mensagem quando verificar o status do compartilhamento. Nesse ponto, o aplicativo é desligado e o compartilhamento é encerrado. O usuário precisará iniciar novamente para compartilhar o conteúdo com seu aplicativo. Dependendo do cenário, você pode considerar que um determinado erro não é tão sério assim para finalizar a operação de compartilhamento. Nesse caso, opte por não chamar **ReportError** e continue com o compartilhamento.

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

Por fim, quando o aplicativo tiver processado o conteúdo compartilhado, chame [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted) para informar ao sistema.

```cs
shareOperation.ReportCompleted();
```

Ao usar esses métodos, você costuma chamá-los na ordem descrita e apenas uma vez. Porém, tem vezes que um aplicativo de destino pode chamar [**ReportDataRetrieved**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportDataRetrieved) antes de [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted). Por exemplo, o aplicativo pode recuperar os dados como parte de uma tarefa no manipulador de ativação, mas não pode chamar **ReportStarted** até que o usuário clique no botão **Compartilhar**.

## <a name="return-a-quicklink-if-sharing-was-successful"></a>Retornar um QuickLink se o compartilhamento tiver sido bem-sucedido

Quando um usuário seleciona seu aplicativo para receber conteúdo, recomendamos que você crie um [**QuickLink**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink). Um **QuickLink** é como um atalho que facilita o compartilhamento de informações com seu aplicativo. Por exemplo, você pode criar um **QuickLink** que abre uma nova mensagem de email pré-configurada com o endereço de email de um amigo.

Um **QuickLink** deve ter um título, um ícone e uma ID. O título (como "Email Mom") e o ícone aparecem quando o usuário toca no botão Compartilhar. A ID é o que o aplicativo usa para acessar qualquer informação personalizada, como um endereço de email ou credenciais de logon. Quando o seu aplicativo cria um **QuickLink**, ele retorna o **QuickLink** ao sistema chamando [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted).

Um **QuickLink** não armazena realmente nenhum dado. Em vez disso, ele contém um identificador que, quando selecionado, é enviado para o aplicativo. Seu aplicativo é responsável por armazenar a ID do **QuickLink** e os dados de usuário correspondentes. Quando o usuário toca no **QuickLink**, você pode obter sua ID por meio da propriedade [**QuickLinkId**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.QuickLinkId).

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>Consulte também 

* [Comunicação de aplicativo a aplicativo](index.md)
* [Compartilhar dados](share-data.md)
* [OnShareTargetActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onsharetargetactivated.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [ReportError](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror.aspx)
* [ReportCompleted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted.aspx)
* [ReportDataRetrieved](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [QuickLink](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.aspx)
* [QuickLInkId](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.id.aspx)
