---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: Personalizar a interface do usuário para visualização de impressão
description: Esta seção descreve como personalizar as opções de impressão e as configurações na interface do usuário de visualização da impressão.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, impressão
ms.localizationpriority: medium
ms.openlocfilehash: 000985d5f9dac5363a1ea2fb002c2be40e2777dd
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258659"
---
# <a name="customize-the-print-preview-ui"></a>Personalizar a interface do usuário para visualização de impressão



**APIs importantes**

-   [**Windows. Graphics. Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)
-   [**Windows. UI. XAML. impressão**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)
-   [**Gerenciador de digerente**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager)

Esta seção descreve como personalizar as opções de impressão e as configurações na interface do usuário de visualização da impressão. Para obter mais informações sobre a impressão, consulte [Imprimir no aplicativo](print-from-your-app.md).

**Dica**  a maioria dos exemplos neste tópico baseia-se na amostra de impressão. Para ver o código completo, baixe a [amostra de impressão da UWP (Plataforma Universal do Windows)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) do [repositório de amostras universais do Windows](https://github.com/Microsoft/Windows-universal-samples) no GitHub.

 

## <a name="customize-print-options"></a>Personalizar opções de impressão

Por padrão, a Interface do Usuário para visualização de impressão mostra as opções de impressão [**ColorMode**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.colormode), [**Copies**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.copies) e [**Orientation**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.orientation): Além delas, há várias outras opções de impressora comuns que você pode adicionar à interface do usuário para visualização de impressão:

-   [**Vinculação**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.binding)
-   [**Agrupamento**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.collation)
-   [**Duplex**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.duplex)
-   [**HolePunch**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.holepunch)
-   [**InputBin**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.inputbin)
-   [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize)
-   [**MediaType**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediatype)
-   [**Mpeza**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.nup)
-   [**Qualidade**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.printquality)
-   [**Conjunto**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.staple)

Essas opções são definidas na classe [**StandardPrintTaskOptions**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.StandardPrintTaskOptions). Você pode adicionar ou remover opções na lista de opções exibida na interface do usuário para visualização de impressão. Também pode mudar a ordem de exibição dessas opções e definir as configurações padrão que são apresentadas para o usuário.

No entanto, as modificações feitas com o uso desse método só afetam a interface do usuário para visualização de impressão. O usuário sempre pode acessar todas as opções com suporte na impressora tocando em **Mais configurações** na interface do usuário para visualização de impressão.

**Observe**  embora seu aplicativo possa especificar qualquer opção de impressão a ser exibida, somente os que têm suporte na impressora selecionada são mostrados na interface do usuário da visualização de impressão. A interface do usuário para impressão não mostrará nenhuma opção para a qual não haja suporte na impressora selecionada.

 

### <a name="define-the-options-to-display"></a>Definir as opções para exibição

Quando a tela do aplicativo estiver carregada, ela se registrará para o contrato do botão Imprimir. Parte desse registro inclui definir o manipulador de eventos [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress). O código para personalizar as opções exibidas na interface de usuário para visualização da impressão é adicionado ao manipulador de eventos **PrintTaskRequested**.

Modifique o manipulador de eventos [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) para incluir as instruções [**printTask.options**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtask.options) que configuram os parâmetros de impressão que você deseja exibir na interface do usuário para visualização de impressão. Para a tela em seu aplicativo para o qual você quer mostrar uma lista personalizada de opções de impressão, substitua o manipulador de eventos **PrintTaskRequested** na classe auxiliar para incluir o código que especifica as opções a serem exibidas quando a tela for impressa.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**Importante**  chamar [**distocadooptions. Clear**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtaskoptions.displayedoptions)() remove todas as opções de impressão da interface do usuário da visualização de impressão, incluindo o link **mais configurações** . Não deixe de acrescentar as opções que você quer mostrar na interface de usuário para visualização de impressão.

### <a name="specify-default-options"></a>Especificar opções padrão

Você também pode definir os valores padrão das opções na interface de usuário para visualização de impressão. A linha de código a seguir, obtida do último exemplo, define o valor padrão da opção [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize).

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>Adicionar novas opções de impressão

Esta seção mostra como criar uma nova opção de impressão, definir uma lista de valores aceitos por essa opção e adicionar a opção à visualização de impressão. Como na seção anterior, adicione a nova opção de impressão no manipulador de eventos [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress).

Primeiro, obtenha um objeto [**PrintTaskOptionDetails**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails). Isso é usado para adicionar a nova opção à interface do usuário para visualização de impressão. Depois, limpe a lista de opções que são apresentadas na interface do usuário para visualização de impressão e adicione as opções que você quer exibir quando o usuário quiser imprimir a partir do aplicativo. Após isso, crie a nova opção de impressão e inicialize a lista de valores da opção. Por fim, adicione a nova opção e atribua um manipulador para o evento **OptionChanged**.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

As opções aparecem na interface do usuário para visualização de impressão na mesma ordem em que foram acrescentadas, com a primeira exibida no topo da janela. Neste exemplo, a opção personalizada é acrescentada por último e, portanto, aparece no final da lista de opções. Porém, você pode colocá-la em qualquer lugar na lista. Opções de impressão personalizadas não precisam ser adicionadas por último.

Quando o usuário mudar a opção selecionada na sua opção personalizada, atualize a imagem de visualização de impressão. Chame o método [**InvalidatePreview**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.invalidatepreview) para redesenhar a imagem na interface do usuário para visualização de impressão, como mostrado abaixo.

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes de design para impressão](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning)
* [Vídeo de Build 2015: desenvolvendo aplicativos que são impressos no Windows 10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Exemplo de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)
