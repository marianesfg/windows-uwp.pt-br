---
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: Imprimir de seu app
description: Aprenda a imprimir documentos a partir de um aplicativo Universal do Windows. Este tópico também mostra como imprimir páginas específicas.
ms.date: 01/29/2018
ms.topic: article
keywords: Windows 10, UWP, impressão
ms.localizationpriority: medium
ms.openlocfilehash: d14a037a84fe64fd9fd3ccca171e3ecfdaaae472
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258640"
---
# <a name="print-from-your-app"></a>Imprimir de seu app



**APIs importantes**

-   [**Windows. Graphics. Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)
-   [**Windows. UI. XAML. impressão**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument)

Aprenda a imprimir documentos a partir de um aplicativo Universal do Windows. Este tópico também mostra como imprimir páginas específicas. Para alterações mais avançadas à interface do usuário para visualização de impressão, consulte [Personalizar a interface do usuário para visualização de impressão](customize-the-print-preview-ui.md).

> [!TIP]
> a maioria dos exemplos neste tópico se baseia na amostra de [impressão de plataforma universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing), que faz parte do repositório de [exemplos de aplicativos da UWP (plataforma universal do Windows)](https://github.com/Microsoft/Windows-universal-samples) no github.

## <a name="register-for-printing"></a>Registrar-se para impressão

A primeira etapa para adicionar impressão ao seu aplicativo é registrar-se para o contrato do botão Imprimir. Seu aplicativo deve fazer isso em cada tela na qual você quer que o seu usuário seja capaz de imprimir. Apenas a tela exibida para o usuário pode ser registrada para impressão. Se uma tela do seu aplicativo registrou-se para impressão, ela deve cancelar o registro para impressão quando ele existir. Se ela for substituída por outra tela, esta deverá se registrar para um novo contrato do botão Imprimir quando for aberta.

> [!TIP]
> se você precisar dar suporte à impressão de mais de uma página em seu aplicativo, poderá colocar esse código de impressão em uma classe auxiliar comum e fazer com que suas páginas de aplicativo a reutilizem. Para um exemplo de como fazer isso, veja a classe `PrintHelper` na [amostra de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing).

Primeiro, declare o [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) e [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument). O tipo **PrintManager** está no namespace [**Windows.Graphics.Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing) juntamente com tipos que suportam outras funcionalidades de impressão do Windows. O tipo **PrintDocument** está no namespace [**Windows.UI.Xaml.Printing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing) juntamente com outros tipos que suportam a preparação de conteúdos XAML para impressão. Você pode facilitar a gravação do seu código de impressão adicionando as seguintes declarações **using** ou **Imports** à sua página.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

A classe [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument) é usada para manipular grande parte da interação entre o aplicativo e o [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager), mas ela expõe vários de seus retornos de chamada. Durante o registro, crie instâncias de **PrintManager** e **PrintDocument**, e registre manipuladores para os eventos de impressão deles.

Na [amostra de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing), o registro é executado pelo método `RegisterForPrinting`.

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

Quando o usuário vai para uma página que oferece suporte à impressão, ele inicia o registro no método `OnNavigatedTo`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initialize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

No exemplo, os manipuladores de eventos têm o registro cancelado no método `UnregisterForPrinting`.

```csharp
public virtual void UnregisterForPrinting()
{
    if (printDocument == null)
    {
        return;
    }

    printDocument.Paginate -= CreatePrintPreviewPages;
    printDocument.GetPreviewPage -= GetPrintPreviewPage;
    printDocument.AddPages -= AddPrintPages;

    PrintManager printMan = PrintManager.GetForCurrentView();
    printMan.PrintTaskRequested -= PrintTaskRequested;
}
```

Quando o usuário deixa uma página que dá suporte à impressão, os manipuladores de eventos têm o registro cancelado dentro do método `OnNavigatedFrom`. 

> [!NOTE]
> Se você tiver um aplicativo de várias páginas e não desconectar a impressão, uma exceção será lançada quando o usuário sair da página e, em seguida, retornar a ela.

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```

## <a name="create-a-print-button"></a>Criar um botão Imprimir

Adicione um botão Imprimir no local da tela do aplicativo onde você quer que ele apareça. Verifique se ele não está interferindo no conteúdo que você quer imprimir.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

Em seguida, adicione um manipulador de eventos ao código do seu aplicativo para manipular o evento de clique. Use o método [**ShowPrintUIAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printmanager.showprintuiasync) para começar a imprimir a partir do seu aplicativo. **ShowPrintUIAsync** é um método assíncrono que exibe a janela de impressão adequada. É recomendável chamar o método [**IsSupported**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printmanager.issupported) primeiro para verificar se o aplicativo está sendo executado em um dispositivo compatível com a impressão (e tratar o caso em que ele não está). Se a impressão não puder ser realizada neste momento por qualquer outro motivo, **ShowPrintUIAsync** gerará uma exceção. Recomendamos capturar essas exceções e permitir que o usuário saiba quando a impressão não pode continuar.

Neste exemplo, uma janela de impressão é exibida no manipulador de eventos para um clique de botão. Se o método abrir uma exceção (porque a impressão não pôde ser executada naquele momento), um controle [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) informa o usuário sobre a situação.

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    if (Windows.Graphics.Printing.PrintManager.IsSupported())
    {
        try
        {
            // Show print UI
            await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

        }
        catch
        {
            // Printing cannot proceed at this time
            ContentDialog noPrintingDialog = new ContentDialog()
            {
                Title = "Printing error",
                Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
            };
            await noPrintingDialog.ShowAsync();
        }
    }
    else
    {
        // Printing is not supported on this device
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing not supported",
            Content = "\nSorry, printing is not supported on this device.",PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

## <a name="format-your-apps-content"></a>Formatar o conteúdo do seu aplicativo

Quando **ShowPrintUIAsync** é chamado, o evento [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) é acionado. O manipulador de evento **PrintTaskRequested** mostrado nesta etapa cria um [**PrintTask**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintTask) chamando o método [**PrintTaskRequest.CreatePrintTask**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtaskrequest.createprinttask) e passa o título para a página de impressão e o nome de uma delegação [**PrintTaskSourceRequestedHandler**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtask.source). Observe que, neste exemplo, o **PrintTaskSourceRequestedHandler** é definido de forma embutida. O **PrintTaskSourceRequestedHandler** fornece o conteúdo formatado para impressão e é descrito depois.

Neste exemplo, um manipulador de conclusão também é definido para capturar erros. É uma boa ideia lidar com eventos de conclusão, pois o seu aplicativo pode informar o usuário se um erro tiver ocorrido e fornecer as possíveis soluções. Da mesma forma, o seu aplicativo poderia usar um evento de conclusão que indica etapas subsequentes para o usuário seguir após o trabalho de impressão ser bem-sucedido.

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
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

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

Depois que a tarefa de impressão é criada, o [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) exige uma coleção de páginas de impressão para mostras na IU de visualização de impressão acionando o evento [**Paginate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.paginate). Isso corresponde ao método **Paginate** da interface **IPrintPreviewPageCollection**. O manipulador de eventos criado durante o registro será chamado dessa vez.

> [!IMPORTANT]
> se o usuário alterar as configurações de impressão, o manipulador de eventos paginar será chamado novamente para permitir que você reflua o conteúdo. Para a melhor experiência de usuário, é recomendável verificar as configurações antes de você redirecionar o fluxo do conteúdo e evitar a reinicialização do conteúdo paginado quando não é necessário.

No manipulador de evento [**Paginate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.paginate) (o método `CreatePrintPreviewPages` na [amostra de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)), crie as páginas para mostrar na IU de visualização de impressão e mandar para a impressora. O código usado para preparar o conteúdo do seu aplicativo para impressão é específico para o aplicativo e o conteúdo que você imprimir. Consulte o código fonte de [amostra de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) para ver como ele formata o conteúdo para impressão.

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

Quando uma página em particular vai ser mostrada na janela de visualização de impressão, o [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) aciona o evento [**GetPreviewPage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.getpreviewpage). Isso corresponde ao método **MakePage** da interface **IPrintPreviewPageCollection**. O manipulador de eventos criado durante o registro será chamado dessa vez.

No manipulador de evento [**GetPreviewPage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.getpreviewpage) (o método `GetPrintPreviewPage` na [amostra de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)), defina as páginas apropriadas no documento de impressão.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

Por fim, quando o usuário clica no botão de impressão, o [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) exige a coleção final de páginas a ser enviada para a impressora, chamando o método **MakeDocument** da interface **IDocumentPageSource**. Em XAML, isso aciona o evento [**AddPages**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.addpages). O manipulador de eventos criado durante o registro será chamado dessa vez.

No manipulador de evento [**AddPages**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.addpages) (o método `AddPrintPages` na [amostra de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)), adicione páginas da coleção de páginas ao objeto [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument) a ser enviado à impressora. Se um usuário especificar páginas em particular ou um intervalo de página a serem impressas, use essas informações aqui para adicionar apenas as páginas que de fato serão enviadas para a impressora.

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## <a name="prepare-print-options"></a>Preparar opções de impressão

Em seguida, prepare as opções de impressão. Para ilustrar, esta seção descreve como definir a opção de intervalo de páginas para permitir a impressão de páginas específicas. Para obter mais opções avançadas, consulte [Personalizar a interface do usuário para visualização de impressão](customize-the-print-preview-ui.md).

Esta etapa cria uma nova opção de impressão, define uma lista de valores aceitos por essa opção e então a adiciona à interface do usuário para visualização de impressão. A opção de intervalo de páginas tem estas configurações:

| Nome da opção          | Ação |
|----------------------|--------|
| **Imprimir tudo**        | Imprimir todas as páginas do documento.|
| **Imprimir seleção**  | Imprimir apenas o conteúdo selecionado pelo usuário.|
| **Intervalo de impressão**      | Exiba um controle de edição onde o usuário possa inserir as páginas a serem impressas.|

Primeiro, modifique o manipulador de evento [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) para adicionar o código para obter um objeto [**PrintTaskOptionDetails**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails).

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

Limpe a lista de opções apresentadas na IU de visualização de impressão e adicione as opções que você quer exibir quando o usuário quiser imprimir a partir do aplicativo.

> [!NOTE]
> as opções aparecem na interface do usuário da visualização de impressão na mesma ordem em que são acrescentadas, com a primeira opção mostrada na parte superior da janela.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

Crie a nova opção de impressão e inicialize a lista de valores da opção.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

Adicione sua opção de impressão personalizada e atribua o manipulador de eventos. A opção personalizada é acrescentada por último e, portanto, aparece no final da lista de opções. Mas você pode colocá-la em qualquer lugar da lista. Opções de impressão personalizadas não precisam ser adicionadas por último.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

O método [**CreateTextOption**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption) cria a caixa de texto **Intervalo**. É onde o usuário insere as páginas específicas que quer imprimir quando selecionam a opção **Intervalo de impressão**.

## <a name="handle-print-option-changes"></a>Manipular alterações de opção de impressão

O manipulador de evento **OptionChanged** faz duas coisas. Primeiro, ele exibe e oculta o campo de edição de texto para o intervalo de página, dependendo da opção de intervalo de página que o usuário selecionar. Depois, ele testa o texto inserido na caixa de texto de intervalo de página para garantir que ele representar um intervalo de página válido para o documento.

Este exemplo mostra como os eventos de alteração de opção de impressão são tratados no [exemplo de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing).

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

> [!TIP]
> consulte o método `GetPagesInRange` no [exemplo de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) para obter detalhes sobre como analisar o intervalo de páginas que o usuário insere na caixa de texto intervalo.

## <a name="preview-selected-pages"></a>Visualizar páginas selecionadas

A maneira como você formata o conteúdo do seu aplicativo para impressão depende da natureza desse aplicativo e do seu conteúdo. Uma classe auxiliar de impressão em usada no [exemplo de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) para formatar o conteúdo para impressão.

Ao imprimir um subconjunto de páginas, há várias maneiras de mostrar o conteúdo na visualização de impressão. Independentemente do método escolhido para mostrar o intervalo de página na visualização de impressão, a saída impressa deve conter somente as páginas selecionadas.

-   Mostre todas as páginas na visualização de impressão se um intervalo de páginas estiver especificado ou não, deixando o usuário saber quais páginas serão realmente impressas.
-   Mostre somente as páginas selecionadas pelo intervalo de página do usuário na visualização de impressão, atualizando a exibição sempre que o usuário alterar o intervalo de páginas.
-   Mostre todas as páginas na visualização de impressão, mas coloque em cinza as páginas que não estão no intervalo de página selecionado pelo usuário.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes de design para impressão](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning)
* [Vídeo de Build 2015: desenvolvendo aplicativos que são impressos no Windows 10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Exemplo de impressão UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)
