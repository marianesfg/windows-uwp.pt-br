---
title: Iniciar o app padrão para um arquivo
description: Aprenda como iniciar o app padrão para um arquivo.
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8e59ae5fb20ce8e1a900f7c1415a699715215e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594521"
---
# <a name="launch-the-default-app-for-a-file"></a>Iniciar o app padrão para um arquivo

**APIs importantes**

-   [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)

Aprenda como iniciar o app padrão para um arquivo. Muitos aplicativos precisam funcionar com arquivos que eles não podem manipular sozinhos. Por exemplo, os aplicativos de email recebem uma variedade de tipos de arquivo e precisam de alguma maneira iniciá-los em seus manipuladores padrão. Essas etapas mostram como usar a API [**Windows.System.Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) para iniciar o manipulador padrão para um arquivo que seu aplicativo não pode manipular sozinho.

## <a name="get-the-file-object"></a>Obter o objeto de arquivo

Primeiro, obtenha um objeto [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) para o arquivo.

Se o arquivo estiver incluído no pacote do aplicativo, você poderá usar a propriedade [**Package.InstalledLocation**](https://msdn.microsoft.com/library/windows/apps/br224681) para obter um objeto [**Windows.Storage.StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) e o método [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) para obter o objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

Se o arquivo estiver em uma pasta conhecida, é possível usar as propriedades da classe [**Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) para obter uma [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) e o método [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) para obter o objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

## <a name="launch-the-file"></a>Iniciar o arquivo

O Windows fornece várias opções diferentes para iniciar o manipulador padrão para um arquivo. Essas opções estão descritas neste gráfico e nas seções que seguem.

| Opção | Método | Descrição |
|--------|--------|-------------|
| Início padrão | [**LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) | Inicie o arquivo especificado com o manipulador padrão. |
| Início com Abrir com | [**LaunchFileAsync (IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | Inicie o arquivo especificado deixando que o usuário escolha o manipulador na caixa de diálogo Abrir com. |
| Iniciar com um fallback do aplicativo recomendado | [**LaunchFileAsync (IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | Inicie o arquivo especificado com o manipulador padrão. Se nenhum manipulador estiver instalado no sistema, recomende ao usuário um aplicativo na Loja. |
| Iniciar com um modo de exibição restante desejado | [**LaunchFileAsync (IStorageFile, LauncherOptions)** ](https://msdn.microsoft.com/library/windows/apps/hh701465) (somente Windows) | Inicie o arquivo especificado com o manipulador padrão. Especifique uma preferência para permanecer na tela após a inicialização e solicite um tamanho específico de janela. [**LauncherOptions.DesiredRemainingView** ](https://msdn.microsoft.com/library/windows/apps/dn298314) não é compatível com a família de dispositivos móveis. |

### <a name="default-launch"></a>Início padrão

Chame o método [**Windows.System.Launcher.LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) para inicializar o aplicativo padrão. Este exemplo usa o método [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) para iniciar um arquivo de imagem, test.png, que está incluído no pacote do aplicativo.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>Início com Abrir com

Chame o método [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) com [**LauncherOptions.DisplayApplicationPicker**](https://msdn.microsoft.com/library/windows/apps/hh701438) configurado para **true** para inicializar o aplicativo que o usuário selecionar na caixa de diálogo **Abrir com**.

Recomendamos que use a caixa de diálogo **Abrir com** quando o usuário quiser selecionar um aplicativo diferente do padrão para um determinado tipo de arquivo. Por exemplo, se o seu aplicativo permitir que o usuário inicie um arquivo de imagem, provavelmente o manipulador padrão será um aplicativo de visualização. Em alguns casos, o usuário pode querer editar a imagem em vez de visualizá-la. Use a opção **Abrir com** junto com um comando alternativo no controle **AppBar** ou em um menu de contexto para permitir que o usuário abra a caixa de diálogo **Abrir com** e selecione o aplicativo de edição nesses tipos de cenário.

![a caixa de diálogo Abrir com para a inicialização de um arquivo .png. a caixa de diálogo contém uma caixa de seleção que especifica se a opção do usuário deve ser usada para todos os arquivos .png ou apenas esse arquivo .png. a caixa de diálogo contém quatro opções de aplicativo para iniciar o arquivo e um link "mais opções".](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**Inicie com um aplicativo recomendado fallback**

Em alguns casos, pode ser que o usuário não tenha um aplicativo instalado para manipular o arquivo que você está iniciando. Por padrão, nesses casos, o Windows oferece ao usuário um link para pesquisar o aplicativo apropriado na Loja. Se você quiser, poderá fazer uma recomendação específica sobre qual aplicativo deve ser adquirido nesse cenário, transmitindo tal recomendação junto com o arquivo que você está iniciando. Para fazer isso, chame o método [**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) com [**LauncherOptions.PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) definido para o nome da família do pacote do aplicativo na Loja que você deseja recomendar. Em seguida, defina [**LauncherOptions.PreferredApplicationDisplayName**](https://msdn.microsoft.com/library/windows/apps/hh965481) como o nome desse aplicativo. O Windows usará essas informações para substituir a opção geral de pesquisar um aplicativo na Loja por uma opção específica para adquirir o aplicativo recomendado na Loja.

> [!NOTE]
> Você deve definir essas duas opções para recomendar um aplicativo. Se você definir uma opção sem a outra, haverá falha.

![a caixa de diálogo Abrir com para a inicialização de um arquivo .contoso. como .contoso não tem um manipulador instalado no computador, a caixa de diálogo contém uma opção com o ícone da Loja e o texto que aponta o usuário para o manipulador correto na Loja. a caixa de diálogo também contém um link "mais opções".](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>Iniciar com um Modo de Exibição Restante Desejado (somente Windows)

Aplicativos de origem que chamam [**LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461) podem solicitar que permaneçam na tela após a inicialização de um arquivo. Por padrão, o Windows tenta dividir todo o espaço disponível igualmente entre o aplicativo de origem e o aplicativo de destino que manipula o arquivo. Aplicativos de origem podem usar a propriedade [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) para indicar ao sistema operacional que eles preferem que sua janela de aplicativo ocupe mais ou menos espaço disponível. **DesiredRemainingView** também pode ser usado para indicar que o aplicativo de origem não precisa permanecer na tela depois da inicialização do arquivo e pode ser completamente substituído pelo aplicativo de destino. Esta propriedade especifica somente o tamanho da janela preferido do aplicativo de chamada. Ele não especifica o comportamento de outros aplicativos que podem acontecer de também estar na tela ao mesmo tempo.

> [!NOTE]
> Windows leva em consideração vários fatores diferentes quando ele determina o tamanho de janela final do código-fonte do aplicativo, por exemplo, a preferência do aplicativo de origem, o número de aplicativos em tela, a orientação da tela e assim por diante. Definindo [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314), você não garante um comportamento de janelas específico para o aplicativo de origem.

**Família de dispositivos móveis:  **[**LauncherOptions.DesiredRemainingView** ](https://msdn.microsoft.com/library/windows/apps/dn298314) não é compatível com a família de dispositivos móveis.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>Comentários

Seu aplicativo não pode selecionar o aplicativo que foi iniciado. O usuário determina o aplicativo que é iniciado. O usuário pode selecionar um aplicativo da Plataforma Universal do Windows (UWP) ou um aplicativo da área de trabalho do Windows.

Ao iniciar um arquivo, seu aplicativo tem que estar em primeiro plano, ou seja, visível para o usuário. Essa exigência ajuda a garantir que o usuário permaneça no controle. Para que essa exigência seja atendida, você deve vincular todas as inicializações de arquivo diretamente à interface do usuário do aplicativo. O mais provável é que o usuário execute alguma ação para iniciar um arquivo.

Você não pode iniciar tipos de arquivos com código ou script que são executados automaticamente pelo sistema operacional, como os arquivos .exe, .msi e .js. Essa restrição protege os usuários contra arquivos possivelmente maliciosos que podem modificar o sistema operacional. Você pode usar esse método para iniciar arquivos que contenham script se eles forem executados por um aplicativo que isole o script, como os arquivos .docx. Aplicativos como o Microsoft Word impedem que o script nos arquivos .docx modifique o sistema operacional.

Quando você tenta iniciar um tipo de arquivo restrito, há falha na inicialização e o retorno de chamada de erro é invocado. Quando o seu aplicativo manipula vários tipos de arquivos diferentes e você espera obter esse erro, nós recomendamos que você ofereça uma experiência de fallback ao usuário. Por exemplo, você pode dar ao usuário a opção de salvar o arquivo na área de trabalho para abri-lo de lá.

## <a name="related-topics"></a>Tópicos relacionados

### <a name="tasks"></a>Tarefas

* [Iniciar o aplicativo padrão para um URI](launch-default-app.md)
* [Manipular a ativação do arquivo](handle-file-activation.md)

### <a name="guidelines"></a>Diretrizes

* [Diretrizes para tipos de arquivo e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Referência

* [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
* [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)
