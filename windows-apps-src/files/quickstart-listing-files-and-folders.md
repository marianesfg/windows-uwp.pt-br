---
author: TylerMSFT
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: Enumerar e consultar arquivos e pastas
description: "Acesse arquivos e pastas que estão em uma pasta, biblioteca, dispositivo ou local de rede. Você também pode consultar arquivos e pastas em um local por meio de consultas de arquivo e pasta."
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6ecad1bbd3c08dcd7aa1d3b82739931f20fc4ee2

---
# Enumerar e consultar arquivos e pastas


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Acesse arquivos e pastas que estão em uma pasta, biblioteca, dispositivo ou local de rede. Você também pode consultar arquivos e pastas em um local por meio de consultas de arquivo e pasta.


            **Observação**  Consulte também a [Amostra de enumeração de pastas](http://go.microsoft.com/fwlink/p/?linkid=619993).

 
## Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Acessar permissões ao local**

    Por exemplo, o código nesses exemplos exige a funcionalidade **picturesLibrary**, mas o local talvez exija outra funcionalidade ou até mesmo nenhuma funcionalidade. Para saber mais, consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## Enumerar arquivos e pastas em um local

> 
            **Observação**  Lembre-se de declarar a funcionalidade **picturesLibrary**.

Neste exemplo, usamos primeiro o método [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) para obter todos os arquivos na pasta raiz da [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (não em subpastas) e listar o nome de cada arquivo. Em seguida, usamos o método [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) para obter todas as subpastas na **PicturesLibrary** e listar o nome de cada subpasta.

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"] 
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections;
> using namespace concurrency;
> using namespace std;
> 
> // Be sure to specify the Pictures Folder capability in the appxmanifext file.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> 
> // Use a shared_ptr so that the string stays in memory
> // until the last task is complete.
> auto outputString = make_shared<wstring>();
> *outputString += L"Files:\n";
> 
> // Get a read-only vector of the file objects
> // and pass it to the continuation. 
> create_task(picturesFolder->GetFilesAsync())        
>    // outputString is captured by value, which creates a copy 
>    // of the shared_ptr and increments its reference count.
>    .then ([outputString] (IVectorView\<StorageFile^>^ files)
>    {        
>        for ( unsigned int i = 0 ; i < files->Size; i++)
>        {
>            *outputString += files->GetAt(i)->Name->Data();
>            *outputString += L"\n";
>       }
>    })
>    // We need to explicitly state the return type 
>    // here: -> IAsyncOperation<...>
>    .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^ 
>    {
>        return picturesFolder->GetFoldersAsync();
>    })
>    // Capture "this" to access m_OutputTextBlock from within the lambda.
>    .then([this, outputString](IVectorView\<StorageFolder^>^ folders)
>    {        
>        *outputString += L"Folders:\n";
> 
>        for ( unsigned int i = 0; i < folders->Size; i++)
>        {
>           *outputString += folders->GetAt(i)->Name->Data();
>           *outputString += L"\n";
>        }
> 
>        // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
>        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<StorageFile> fileList = 
>     await picturesFolder.GetFilesAsync();
> 
> outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> {
>     outputText.Append(file.Name + "\n");
> }
> 
> IReadOnlyList<StorageFolder> folderList = 
>     await picturesFolder.GetFoldersAsync();
>            
> outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> {
>     outputText.Append(folder.DisplayName + "\n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
> 
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
> 
>     outputText.Append(file.Name & vbLf)
> 
> Next file
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
> 
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
> 
>     outputText.Append(folder.DisplayName & vbLf)
> 
> Next folder
> ```


> [!div class="tabbedCodeSnippets"]
 


            **Observação**  Em C# ou Visual Basic, lembre-se de colocar a palavra-chave **async** na declaração de qualquer método no qual você utilize o operador **await**. Como alternativa, você pode usar o método [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) para obter todos os itens (arquivos e subpastas) em um local específico. O seguinte exemplo usa o método **GetItemsAsync** para obter todos os arquivos e subpastas na pasta raiz da [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (e não em subpastas). Em seguida, o exemplo lista o nome de cada arquivo e subpasta.

> [!div class="tabbedCodeSnippets"] 
> ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>();
> 
> create_task(picturesFolder->GetItemsAsync())        
>     .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
> {        
>     for ( unsigned int i = 0 ; i < items->Size; i++)
>     {
>         *outputString += items->GetAt(i)->Name->Data();
>         if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
>         {
>             *outputString += L"  folder\n";
>         }
>         else
>         {
>             *outputString += L"\n";
>         }
>         m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     }
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<IStorageItem> itemsList = 
>     await picturesFolder.GetItemsAsync();
> 
> foreach (var item in itemsList)
> {
>     if (item is StorageFolder)
>     {
>         outputText.Append(item.Name + " folder\n");
> 
>     }
>     else
>     {
>         outputText.Append(item.Name + "\n");
> 
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim itemsList As IReadOnlyList(Of IStorageItem) =
>     Await picturesFolder.GetItemsAsync()
> 
> For Each item In itemsList
> 
>     If TypeOf item Is StorageFolder Then
> 
>         outputText.Append(item.Name & " folder" & vbLf)
> 
>     Else
> 
>         outputText.Append(item.Name & vbLf)
> 
>     End If
> 
> Next item
> ```

## Se o item é uma subpasta, o exemplo anexa `"folder"` ao nome.

[!div class="tabbedCodeSnippets"] Consultar arquivos em um local e enumerar os arquivos correspondentes Neste exemplo, consultamos todos os arquivos na [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) agrupados por mês e, desta vez, o exemplo retorna em subpastas.

Primeiro, chamamos [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) e passamos o valor [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) para o método. Isso nos dá um objeto [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066).

> [!div class="tabbedCodeSnippets"] 
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search;
> using namespace concurrency;
> using namespace Platform::Collections;
> using namespace Windows::Foundation::Collections;
> using namespace std;
> 
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> 
> StorageFolderQueryResult^ queryResult = 
>     picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);
> 
> // Use shared_ptr so that outputString remains in memory
> // until the task completes, which is after the function goes out of scope.
> auto outputString = std::make_shared<wstring>();
> 
> create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view) 
> {        
>     for ( unsigned int i = 0; i < view->Size; i++)
>     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
>         {
>             *outputString += view->GetAt(i)->Name->Data();
>             *outputString += L"(";
>             *outputString += to_wstring(files->Size);
>             *outputString += L")\r\n";
>             for (unsigned int j = 0; j < files->Size; j++)
>             {
>                 *outputString += L"     ";
>                 *outputString += files->GetAt(j)->Name->Data();
>                 *outputString += L"\r\n";
>             }
>         }).then([this, outputString]()
>         {
>             m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>         });
>     }    
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> 
> StorageFolderQueryResult queryResult = 
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList = 
>     await queryResult.GetFoldersAsync();
> 
> StringBuilder outputText = new StringBuilder();
> 
> foreach (StorageFolder folder in folderList)
> {
>     IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();
> 
>     // Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");
> 
>     foreach (StorageFile file in fileList)
>     {
>         // Print the name of the file.
>         outputText.AppendLine("   " + file.Name);
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim queryResult As StorageFolderQueryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await queryResult.GetFoldersAsync()
> 
> For Each folder As StorageFolder In folderList
> 
>     Dim fileList As IReadOnlyList(Of StorageFile) =
>         Await folder.GetFilesAsync()
> 
>     ' Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")
> 
>     For Each file As StorageFile In fileList
> 
>         ' Print the name of the file.
>         outputText.AppendLine("   " & file.Name)
> 
>     Next file
> 
> Next folder
> ```

Em seguida, chamamos [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074), que retorna objetos [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que representam pastas virtuais.

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```




<!--HONumber=Jun16_HO4-->


