---
author: TylerMSFT
title: "Iniciando automaticamente com a Reprodução Automática"
description: "Você pode usar a Reprodução Automática para fornecer seu aplicativo como uma opção quando um usuário conecta um dispositivo ao computador. Isso inclui dispositivos sem volume, como uma câmera ou um player de mídia, ou dispositivos com volume, como pen drives, cartões de memória ou DVDs."
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 02215be4cff7bbd42bdc1911777f62bacd22b6c7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddevlaunchresumeauto-launchingwithautoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>Iniciando automaticamente com a Reprodução Automática


\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Você pode usar a **Reprodução Automática** para fornecer seu aplicativo como uma opção quando um usuário conecta um dispositivo ao computador. Isso inclui dispositivos sem volume, como uma câmera ou um player de mídia, ou dispositivos com volume, como pen drives, cartões de memória ou DVDs. Também é possível usar a **Reprodução Automática** para oferecer seu aplicativo como uma opção quando os usuários compartilham arquivos entre dois computadores usando proximidade (encostar).

> **Observação**  Se você for um fabricante de dispositivos e desejar associar seu [aplicativo de dispositivo da Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=301381) como um manipulador de **Reprodução Automática** para o dispositivo, identifique o aplicativo nos metadados do dispositivo. Para obter mais informações, consulte [Reprodução Automática para aplicativos de dispositivo da Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=306684).

## <a name="register-for-autoplay-content"></a>Registrar para conteúdo de Reprodução Automática

Você pode registrar aplicativos como opções de eventos de conteúdo de **Reprodução Automática**. Os eventos de conteúdo da **Reprodução Automática** são gerados quando um dispositivo de volume, como um cartão de memória de câmera, um pen drive ou um DVD, for inserido no computador. Aqui, mostramos como identificar o aplicativo como uma opção de **Reprodução Automática** quando um dispositivo de volume de uma câmera for inserido.

Neste tutorial, você criou um aplicativo que exibe arquivos de imagem ou os copia para Imagens. Você registrou o aplicativo para o evento de conteúdo **ShowPicturesOnArrival** da Reprodução Automática.

A Reprodução Automática também gera eventos de conteúdo para conteúdo compartilhado entre computadores usando proximidade (encostar). Você pode usar as etapas e o código que constam nesta seção para manipular os arquivos compartilhados entre computadores que usam a proximidade. A tabela a seguir lista os eventos de conteúdo de Reprodução Automática que estão disponíveis para compartilhamento de conteúdo usando proximidade.

| Ação         | Evento de conteúdo do AutoPlay  |
|----------------|-------------------------|
| Compartilhamento de música  | PlayMusicFilesOnArrival |
| Compartilhamento de vídeos | PlayVideoFilesOnArrival |

 
Quando arquivos são compartilhados usando proximidade, a propriedade **Files** do objeto **FileActivatedEventArgs** contém uma referência a uma pasta raiz que contém todos os arquivos compartilhados.

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Etapa 1: criar um novo projeto e adicionar declarações de Reprodução Automática

1.  Abra o Microsoft Visual Studio e selecione **Novo Projeto** no menu **Arquivo**. Na seção **Visual C#**, em **Windows**, selecione **Aplicativo em Branco (Universal do Windows)**. Nomeie o aplicativo **AutoPlayDisplayOrCopyImages** e clique em **OK**.
2.  Abra o arquivo Package.appxmanifest e selecione a guia **Recursos**. Selecione os recursos **Armazenamento Removível** e **Biblioteca de Imagens**. Essa seleção lhe proporciona acesso de aplicativo aos dispositivos de armazenamento removível como memória de câmera e acesso às imagens locais.
3.  No arquivo de manifesto, selecione a guia **Declarações**. Na lista suspensa **Declarações Disponíveis**, selecione **Conteúdo de Reprodução Automática** e clique em **Adicionar**. Selecione o novo item **Conteúdo da Reprodução Automática** que foi adicionado à lista **Declarações Suportadas**.
4.  Uma declaração **Conteúdo de Reprodução Automática** identifica seu aplicativo como uma opção quando a Reprodução Automática gera um evento de conteúdo. O evento é baseado no conteúdo de um dispositivo de volume como um DVD ou um pen drive. A Reprodução Automática examina o conteúdo do dispositivo de volume e determina qual evento de conteúdo gerar. Se a raiz do volume contiver uma pasta DCIM, AVCHD ou PRIVATE\\ACHD, ou se um usuário tiver habilitado **Escolha o que fazer com cada tipo de mídia** no Painel de Controle de Reprodução Automática e forem encontradas imagens na raiz do volume, a Reprodução Automática gerará o evento **ShowPicturesOnArrival**. Na seção **Ações de Inicialização**, insira os valores da Tabela 1 abaixo para a primeira ação de inicialização.
5.  Na seção **Ações de Inicialização** do item **Conteúdo de Reprodução Automática**, clique em **Adicionar Novo** para adicionar a segunda ação de inicialização. Insira os valores na Tabela 2 abaixo para a segunda ação de inicialização.
6.  Na lista suspensa **Declarações Disponíveis**, selecione **Associações de Tipo de Arquivo** e clique em **Adicionar**. Nas Propriedades da nova declaração **Associações de Tipo de Arquivo**, defina o campo **Nome de Exibição** como **Copiar ou Mostrar Imagens de Reprodução Automática** e o campo **Nome** como **image\_association1**. Na seção **Tipos de Arquivo com Suporte**, clique em **Adicionar Novo**. Defina o campo **Tipo de Arquivo** como **.jpg**. Na seção **Tipos de Arquivo com Suporte**, defina o campo **Tipo de Arquivo** da nova associação de arquivo como **.png**. Para eventos de conteúdo, a Reprodução Automática não mostra os tipos de arquivo que não estejam explicitamente associados ao seu aplicativo.
7.  Salve e feche o arquivo de manifesto.


**Tabela 1**

| Configuração             | Value                 |
|---------------------|-----------------------|
| Verbo                | show                  |
| Nome de Exibição da Ação | Show Pictures         |
| Evento de Conteúdo       | ShowPicturesOnArrival |

A configuração **Nome de exibição da ação** identifica a cadeia de caracteres que a Reprodução Automática exibe para seu aplicativo. A configuração **Verbo** identifica um valor que é passado ao seu aplicativo para a opção selecionada. Você pode especificar várias ações de inicialização para um evento de Reprodução Automática e usar a configuração **Verbo** para determinar qual opção u usuário selecionou para seu aplicativo. Você pode descobrir a opção selecionada pelo usuário verificando a propriedade **verb** dos argumentos do evento de inicialização passados para seu aplicativo. Também pode usar qualquer valor para a configuração **Verbo** exceto **open**, que está reservado.

**Tabela 2**  

| Configuração             | Value                      |
|---------------------|----------------------------|
| Verbo                | copy                       |
| Nome de Exibição da Ação | Copy Pictures Into Library |
| Evento de Conteúdo       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>Etapa 2: Adicionar interface do usuário XAML

Abra o arquivo MainPage.xaml e adicione o seguinte XAML à seção &lt;Grade&gt; padrão.

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>Etapa 3: Adicionar código de inicialização

O código nesta etapa verifica o valor do verbo na propriedade **Verb**, a qual é um dos argumentos de inicialização passados para o aplicativo durante o evento **OnFileActivated**. Em seguida, o código chama um método relacionado à opção selecionada pelo usuário. Para o evento de memória da câmera, a Reprodução Automática passa a pasta raiz do armazenamento da câmera para o aplicativo. Você pode recuperar essa pasta no primeiro elemento da propriedade **Files**.

Abra o arquivo App.xaml.cs e adicione o código a seguir à classe **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **Observação**  Os métodos `DisplayImages` e `CopyImages` são adicionados nas etapas a seguir.

### <a name="step-4-add-code-to-display-images"></a>Etapa 4: Adicionar código para exibir imagens

No arquivo MainPage.xaml.cs, adicione o código a seguir à classe **MainPage**.

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### <a name="step-5-add-code-to-copy-images"></a>Etapa 5: Adicionar código para copiar imagens

No arquivo MainPage.xaml.cs, adicione o código a seguir à classe **MainPage**.

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### <a name="step-6-build-and-run-the-app"></a>Etapa 6: Compilar e executar o aplicativo

1.  Pressione F5 para compilar e implantar o aplicativo (no modo de depuração).
2.  Para executar o aplicativo, insira um cartão de memória de câmera ou outro dispositivo de armazenamento de uma câmera no computador. Em seguida, selecione uma das opções de evento de conteúdo que você especificou no arquivo package.appxmanifest na lista de opções de Reprodução Automática. Este código de exemplo apenas exibe ou copia imagens na pasta DCIM de um cartão de memória de câmera. Se o cartão de memória da câmera armazenar imagens em uma pasta AVCHD ou PRIVATE\\ACHD, será necessário atualizar o código adequadamente.
    **Observação**  Se você não tiver um cartão de memória da câmera, poderá usar uma unidade flash se ela tiver uma pasta chamada **DCIM** na raiz e se a pasta DCIM tiver uma subpasta que contenha imagens.

## <a name="register-for-an-autoplay-device"></a>Registrar para um dispositivo de Reprodução Automática


Você pode registrar aplicativos como opções de eventos de dispositivo de **Reprodução Automática**. Os eventos de dispositivo de **Reprodução Automática** são gerados quando um dispositivo é conectado a um computador.

Nesta seção, mostramos como identificar seu aplicativo como uma opção **Reprodução Automática** quando uma câmera é conectada a um computador. O aplicativo registra como um manipulador para o evento **WPD\\ImageSourceAutoPlay**. Esse é um evento comum que o sistema Dispositivo Portátil do Windows (WPD) gera quando câmeras e outros dispositivos de imagem o notifica que são uma ImageSource usando MTP. Para saber mais, consulte [Dispositivos portáteis do Windows](https://msdn.microsoft.com/library/windows/hardware/ff597729).

**Importante**  As APIS [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) fazem parte da [família de dispositivos da área de trabalho](https://msdn.microsoft.com/library/windows/apps/dn894631). Os aplicativos podem usar essas APIs somente em dispositivos com Windows 10 na família de dispositivos da área de trabalho, como computadores.

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Etapa 1: criar um novo projeto e adicionar declarações de Reprodução Automática

1.  Abra o Visual Studio e selecione **Novo Projeto** no menu **Arquivo**. Na seção **Visual C#**, em **Windows**, selecione **Aplicativo em Branco (Universal do Windows)**. Nomeie o aplicativo **AutoPlayDevice\_Camera** e clique em **OK**.
2.  Abra o arquivo Package.appxmanifest e selecione a guia **Recursos**. Selecione o recurso **Armazenamento Removível**. Isso dá ao aplicativo acesso aos dados na câmera como um dispositivo de volume de armazenamento removível.
3.  No arquivo de manifesto, selecione a guia **Declarações**. Na lista suspensa **Declarações Disponíveis**, selecione **Dispositivo de Reprodução Automática** e clique em **Adicionar**. Selecione o novo item **Dispositivo de Reprodução Automática** que foi adicionado à lista **Declarações com Suporte**.
4.  Uma declaração **Dispositivo de Reprodução Automática** identifica o aplicativo como uma opção quando a Reprodução Automática gera um evento de dispositivo para eventos conhecidos. Na seção **Ações de Inicialização**, insira os valores na tabela abaixo para a primeira ação de inicialização.
5.  Na lista suspensa **Declarações Disponíveis**, selecione **Associações de Tipo de Arquivo** e clique em **Adicionar**. Nas Propriedades da nova declaração **Associações de Tipo de Arquivo**, defina o campo **Nome de Exibição** como **Mostrar Imagens da Câmera** e o campo **Nome** como **camera\_association1**. Na seção **Tipos de Arquivo com Suporte**, clique em **Adicionar Novo**, se necessário. Defina o campo **Tipo de Arquivo** como **.jpg**. Na seção **Tipos de Arquivo com Suporte**, clique novamente em **Adicionar Novo**. Defina o campo **Tipo de Arquivo** da nova associação de arquivo como **.png**. Para eventos de conteúdo, a Reprodução Automática não mostra os tipos de arquivo que não estejam explicitamente associados ao seu aplicativo.
6.  Salve e feche o arquivo de manifesto.

| Configuração             | Value            |
|---------------------|------------------|
| Verbo                | show             |
| Nome de Exibição da Ação | Show Pictures    |
| Evento de Conteúdo       | WPD\\ImageSource |

A configuração **Nome de Exibição da Ação** identifica a cadeia de caracteres que a Reprodução Automática exibe para seu aplicativo. A configuração **Verbo** identifica um valor que é passado ao seu aplicativo para a opção selecionada. Você pode especificar várias ações de inicialização para um evento de Reprodução Automática e usar a configuração **Verbo** para determinar qual opção u usuário selecionou para seu aplicativo. Você pode descobrir a opção selecionada pelo usuário verificando a propriedade **verb** dos argumentos do evento de inicialização passados para seu aplicativo. Também pode usar qualquer valor para a configuração **Verbo** exceto **open**, que está reservado. Para ver um exemplo do uso de vários verbos em um único aplicativo, consulte [Registrar-se para conteúdo de reprodução automática](#register-for-autoplay-content).

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>Etapa 2: Adicionar referência de assembly para as extensões de área de trabalho

As APIs necessárias para acessar o armazenamento em um Dispositivo Portátil do Windows, [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654), fazem parte da [família de dispositivos da área de trabalho](https://msdn.microsoft.com/library/windows/apps/dn894631). Isso significa que um assembly especial é necessário para usar as APIs e essas chamadas funcionarão somente em um dispositivo na família de dispositivos da área de trabalho (como um computador).

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** em **Adicionar Referência...**.
2.  Expanda **Universal do Windows** e clique em **Extensões**.
3.  Em seguida, selecione **Extensões de Área de Trabalho do Windows para a UWP** e clique em **OK**.

### <a name="step-3-add-xaml-ui"></a>Etapa 3: Adicionar interface do usuário XAML

Abra o arquivo MainPage.xaml e adicione o seguinte XAML à seção &lt;Grade&gt; padrão.

```xml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Etapa 4: Adicionar código de ativação

O código nesta etapa referencia a câmera como um [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) passando a ID de informações de dispositivo da câmera para o método [**FromId**](https://msdn.microsoft.com/library/windows/apps/br225655). A ID de informações de dispositivo da câmera é obtida primeiro pela transmissão dos argumentos de evento, como [**DeviceActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224710), e em seguida obtendo o valor da propriedade [**DeviceInformationId**](https://msdn.microsoft.com/library/windows/apps/br224711).

Abra o arquivo App.xaml.cs e adicione o código a seguir à classe **App**.

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **Observação**  O método `ShowImages` é adicionado na etapa a seguir.

### <a name="step-5-add-code-to-display-device-information"></a>Etapa 5: Adicionar código para exibir informações de dispositivo

Você pode obter informações sobre a câmera nas propriedades da classe [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654). O código nesta etapa exibe o nome do dispositivo e outras informações para o usuário quando o aplicativo é executado. Em seguida, o código chama os métodos GetImageList e GetThumbnail, que você vai adicionar na próxima etapa, para exibir miniaturas das imagens armazenadas na câmera.

No arquivo MainPage.xaml.cs, adicione o código a seguir à classe **MainPage**.

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **Observação**  Os métodos `GetImageList` e `GetThumbnail` são adicionados na etapa a seguir.

 

### <a name="step-6-add-code-to-display-images"></a>Etapa 6: Adicionar código para exibir imagens

O código nesta etapa exibe miniaturas das imagens armazenadas na câmera. O código faz chamadas assíncronas para a câmera para obter a imagem em miniatura. Entretanto, a próxima chamada assíncrona só ocorre depois que a chamada assíncrona anterior é concluída. Isso garante que apenas uma solicitação seja feita à câmera de cada vez.

No arquivo MainPage.xaml.cs, adicione o código a seguir à classe **MainPage**.

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder)
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img)
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### <a name="step-7-build-and-run-the-app"></a>Etapa 7: Compilar e executar o aplicativo

1.  Pressione F5 para compilar e implantar o aplicativo (no modo de depuração).
2.  Para executar o aplicativo, conecte uma câmera ao seu computador. Em seguida, selecione o aplicativo na lista de opções da Reprodução Automática.
    **Observação**  Nem todas as câmeras anunciam o evento de dispositivo de Reprodução Automática **WPD\\ImageSource**.

     

## <a name="configure-removable-storage"></a>Configurar o armazenamento removível


Você pode identificar um dispositivo de volume, como um cartão de memória ou um pen drive, como um dispositivo de **Reprodução Automática** quando o dispositivo de volume é conectado a um computador. Isso é especialmente útil quando você quer associar um aplicativo específico para **Reprodução Automática** para apresentar ao usuário do seu dispositivo de volume.

Este exemplo mostra como identificar seu dispositivo de volume como um dispositivo de **Reprodução Automática**.

Para identificar seu dispositivo de volume como um dispositivo de **Reprodução Automática**, adicione um arquivo autorun.inf à unidade raiz do dispositivo. No arquivo autorun.inf, adicione uma chave **CustomEvent** à seção **AutoRun**. Quando o dispositivo de volume se conectar a um computador, a **Reprodução Automática** encontrará o arquivo autorun.inf e tratará o volume como um dispositivo. A **Reprodução Automática** criará um evento de **Reprodução Automática** usando o nome que você forneceu na chave **CustomEvent**. Em seguida, você pode criar um aplicativo e registrá-lo como manipulador desse evento de **Reprodução Automática**. Quando o dispositivo se conectar ao computador, a **Reprodução Automática** mostrar o aplicativo como manipulador do dispositivo de volume. Para obter mais informações sobre arquivos autorun.inf, consulte [entradas de autorun.inf](https://msdn.microsoft.com/library/windows/desktop/cc144200).

### <a name="step-1-create-an-autoruninf-file"></a>Etapa 1: Criar um arquivo autorun.inf

Na unidade raiz do dispositivo de volume, adicione um arquivo chamado autorun.inf. Abra o arquivo autorun.inf e adicione o texto a seguir.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>Etapa 2: Criar um novo projeto e criar declarações de Reprodução Automática

1.  Abra o Visual Studio e selecione **Novo Projeto** no menu **Arquivo**. Na seção **Visual C#**, em **Windows**, selecione **Aplicativo em Branco (Universal do Windows)**. Nomeie o aplicativo **AutoPlayCustomEvent** e clique em **OK**.
2.  Abra o arquivo Package.appxmanifest e selecione a guia **Recursos**. Selecione o recurso **Armazenamento Removível**. Isso dá ao aplicativo acesso aos arquivos e pastas em dispositivos de armazenamento removíveis.
3.  No arquivo de manifesto, selecione a guia **Declarações**. Na lista suspensa **Declarações Disponíveis**, selecione **Conteúdo de Reprodução Automática** e clique em **Adicionar**. Selecione o novo item **Conteúdo da Reprodução Automática** que foi adicionado à lista **Declarações Suportadas**.

    **Observação**  Como alternativa, você também pode optar por adicionar uma declaração de **Dispositivo de Reprodução Automática** para seu evento de Reprodução Automática personalizado.
    
4.  Na seção **Ações de Inicialização** para a declaração de evento **Conteúdo de Reprodução Automática**, insira os valores da tabela abaixo na primeira ação de inicialização.
5.  Na lista suspensa **Declarações Disponíveis**, selecione **Associações de Tipo de Arquivo** e clique em **Adicionar**. Nas Propriedades da nova declaração **Associações de Tipo de Arquivo**, defina o campo **Nome de Exibição** como **Mostrar Arquivos .ms** e o campo **Nome** como **ms\_association**. Na seção **Tipos de Arquivo com Suporte**, clique em **Adicionar Novo**. Defina o campo **Tipo de Arquivo** como **.ms**. Para eventos de conteúdo, a Reprodução Automática não mostra os tipos de arquivo que não estejam explicitamente associados ao seu aplicativo.
6.  Salve e feche o arquivo de manifesto.

| Configuração             | Value                         |
|---------------------|-------------------------------|
| Verbo                | show                          |
| Nome de Exibição da Ação | Show Files                    |
| Evento de Conteúdo       | AutoPlayCustomEventQuickstart |

O valor **Evento de Conteúdo** é o texto que você forneceu para a chave **CustomEvent** no arquivo autorun.inf. A configuração **Nome de Exibição da Ação** identifica a cadeia de caracteres que a Reprodução Automática exibe para seu aplicativo. A configuração **Verbo** identifica um valor que é passado ao seu aplicativo para a opção selecionada. Você pode especificar várias ações de inicialização para um evento de Reprodução Automática e usar a configuração **Verbo** para determinar qual opção u usuário selecionou para seu aplicativo. Você pode descobrir a opção selecionada pelo usuário verificando a propriedade **verb** dos argumentos do evento de inicialização passados para seu aplicativo. Também pode usar qualquer valor para a configuração **Verbo** exceto **open**, que está reservado.

### <a name="step-3-add-xaml-ui"></a>Etapa 3: Adicionar interface do usuário XAML

Abra o arquivo MainPage.xaml e adicione o seguinte XAML à seção &lt;Grade&gt; padrão.

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Etapa 4: Adicionar código de ativação

O código nesta etapa chama um método para exibir as pastas na unidade raiz do dispositivo de volume. Para eventos de conteúdo de Reprodução Automática, a Reprodução Automática passa a pasta raiz do dispositivo de armazenamento nos argumentos de inicialização passados para o aplicativo durante o evento **OnFileActivated**. Você pode recuperar essa pasta no primeiro elemento da propriedade **Files**.

Abra o arquivo App.xaml.cs e adicione o código a seguir à classe **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **Observação**  O método `DisplayFiles` é adicionado na etapa a seguir.

 

### <a name="step-5-add-code-to-display-folders"></a>Etapa 5: Adicionar código para exibir pastas

No arquivo MainPage.xaml.cs, adicione o código a seguir à classe **MainPage**.

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>>
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### <a name="step-6-build-and-run-the-qpp"></a>Etapa 6: Compilar e executar o aplicativo

1.  Pressione F5 para compilar e implantar o aplicativo (no modo de depuração).
2.  Para executar seu aplicativo, insira um cartão de memória ou outro dispositivo de armazenamento em seu computador. Em seguida, selecione seu aplicativo na lista de opções do manipulador de Reprodução Automática.

## <a name="autoplay-event-reference"></a>Referência de eventos de Reprodução Automática


O sistema de **Reprodução Automática** permite que os aplicativos se registrem para uma ampla variedade de eventos de chegada de dispositivo e volume (disco). Para registrar eventos de conteúdo da **Reprodução Automática**, é necessário habilitar o recurso **Armazenamento removível** no manifesto do seu pacote. Esta tabela mostra os eventos para os quais você pode se registrar e quando eles são gerados.

| Cenário                                                           | Evento   | Descrição   |
|--------------------------------------------------------------------|---------|---------------|
| Usando fotos em uma Câmera                                           | **WPD\ImageSource**                | Gerado para câmeras que são identificadas como dispositivos portáteis do Windows e que oferecem o recursos ImageSource.                                                                                                                                                                                                                                                                  |
| Usando música em um player de áudio                                     | **WPD\AudioSource**                | Gerado para media players que são identificados como dispositivos portáteis do Windows e que oferecem o recurso AudioSource.                                                                                                                                                                                                                                                            |
| Usando vídeos em uma câmera de vídeo                                     | **WPD\VideoSource**                | Gerado para câmeras de vídeo que são identificadas como dispositivos portáteis do Windows e que oferecem o recursos VideoSource.                                                                                                                                                                                                                                                            |
| Acessar uma unidade flash ou um disco rígido externo conectado              | **StorageOnArrival**               | Gerado quando uma unidade ou um volume é conectado ao computador.   Se a unidade ou o volume contiver uma pasta DCIM, AVCHD ou PRIVATE\ACHD na raiz do disco, será gerado o evento **ShowPicturesOnArrival**.                                                                                                                                                             |
| Usando fotos de armazenamento em massa (herdado)                            | **ShowPicturesOnArrival**          | Gerado quando uma unidade ou um volume contém uma pasta DCIM, AVCHD ou PRIVATE\ACHD na raiz do disco. Se um usuário tiver habilitado **Escolha o que fazer com cada tipo de mídia** no Painel de Controle de Reprodução Automática, a Reprodução Automática examinará um volume conectado ao computador para determinar o tipo de conteúdo no disco. Quando são encontradas imagens, é gerado **ShowPicturesOnArrival**. |
| Recebendo fotos com Compartilhamento de Proximidade (encostar e enviar)             | **ShowPicturesOnArrival**          | Quando usuários enviam conteúdo usando proximidade (encostar e enviar), a Reprodução Automática examina os arquivos compartilhados para determinar o tipo de conteúdo. Se forem encontradas imagens, será gerado **ShowPicturesOnArrival**.                                                                                                                                                                         |
| Usando música de armazenamento em massa (herdado)                             | **PlayMusicFilesOnArrival**        | Se um usuário tiver habilitado **Escolha o que fazer com cada tipo de mídia** no Painel de Controle de Reprodução Automática, a Reprodução Automática examinará um volume conectado ao computador para determinar o tipo de conteúdo no disco.  Quando são encontrados arquivos de música, é gerado **PlayMusicFilesOnArrival**.                                                                                                   |
| Recebendo música com Compartilhamento de Proximidade (encostar e enviar)              | **PlayMusicFilesOnArrival**        | Quando usuários enviam conteúdo usando proximidade (encostar e enviar), a Reprodução Automática examina os arquivos compartilhados para determinar o tipo de conteúdo. Se são encontrados arquivos de música, é gerado **PlayMusicFilesOnArrival**.                                                                                                                                                                    |
| Usando vídeos de armazenamento em massa (herdado)                            | **PlayVideoFilesOnArrival**        | Se um usuário tiver habilitado **Escolha o que fazer com cada tipo de mídia** no Painel de Controle de Reprodução Automática, a Reprodução Automática examinará um volume conectado ao computador para determinar o tipo de conteúdo no disco. Quando são encontrados arquivos de vídeo, é gerado **PlayVideoFilesOnArrival**.                                                                                                   |
| Recebendo vídeos com Compartilhamento de Proximidade (encostar e enviar)             | **PlayVideoFilesOnArrival**        | Quando usuários enviam conteúdo usando proximidade (encostar e enviar), a Reprodução Automática examina os arquivos compartilhados para determinar o tipo de conteúdo. Se são encontrados arquivos de vídeo, é gerado **PlayVideoFilesOnArrival**.                                                                                                                                                                    |
| Manipulando conjuntos mistos de arquivos de um dispositivo conectado               | **MixedContentOnArrival**          | Se um usuário tiver habilitado **Escolha o que fazer com cada tipo de mídia** no Painel de Controle de Reprodução Automática, a Reprodução Automática examinará um volume conectado ao computador para determinar o tipo de conteúdo no disco. Se nenhum tipo de conteúdo específico é encontrado (por exemplo, imagens), é gerado **MixedContentOnArrival**.                                                                    |
| Manipulando conjuntos mistos de arquivos com Compartilhamento de Proximidade (encostar e enviar) | **MixedContentOnArrival**          | Quando usuários enviam conteúdo usando proximidade (encostar e enviar), a Reprodução Automática examina os arquivos compartilhados para determinar o tipo de conteúdo. Se nenhum tipo de conteúdo específico é encontrado (por exemplo, imagens), é gerado **MixedContentOnArrival**.                                                                                                                                  |
| Manipular vídeo de mídia óptica                                    | **PlayDVDMovieOnArrival**          |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayBluRayOnArrival**            |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayVideoCDMovieOnArrival**      |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlaySuperVideoCDMovieOnArrival** |                                                                                                                                                                                                                                                                                                                                                                           |
| Manipular música de mídia óptica                                    | **PlayCDAudioOnArrival**           |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayDVDAudioOnArrival**          |                                                                                                                                                                                                                                                                                                                                                                           |
| Reproduzir discos avançados                                                | **PlayEnhancedCDOnArrival**        |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayEnhancedDVDOnArrival**       |                                                                                                                                                                                                                                                                                                                                                                           |
| Manipular discos ópticos graváveis                                     | **HandleCDBurningOnArrival**       |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **HandleDVDBurningOnArrival**      |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **HandleBDBurningOnArrival**       |                                                                                                                                                                                                                                                                                                                                                                           |
| Manipular qualquer outra conexão de dispositivo ou volume                       | **UnknownContentOnArrival**        | Gerado para todos os eventos, caso seja encontrado conteúdo que não corresponda nenhum dos eventos de conteúdo de Reprodução Automática. O uso desse evento não é recomendado. Você só deve registrar o seu aplicativo para os eventos de Reprodução Automática específicos que ele possa manipular.                                                                                                                               |

É possível especificar que a Reprodução Automática gere um evento de conteúdo de Reprodução Automática personalizado usando a entrada **CustomEvent** no arquivo autorun.inf de um volume. Para obter mais informações, consulte [entradas de Autorun.inf](https://msdn.microsoft.com/library/windows/desktop/cc144200).

Você pode registrar seu aplicativo como um conteúdo de Reprodução Automática ou um manipulador de evento de dispositivo de Reprodução Automática adicionando uma extensão ao arquivo package.appxmanifest para seu aplicativo. Se estiver usando o Visual Studio, você poderá adicionar uma declaração de **Conteúdo de Reprodução Automática** ou **Dispositivo de Reprodução Automática** na guia **Declarações**. Se você estiver editando diretamente o arquivo package.appxmanifest para seu aplicativo, adicione um elemento [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) ao manifesto do pacote que especifique **windows.autoPlayContent** ou **windows.autoPlayDevice** como **Category**. Por exemplo, a entrada a seguir no manifesto do pacote adiciona uma extensão de **Conteúdo de Reprodução Automática** para registrar o aplicativo como manipulador do evento **ShowPicturesOnArrival**.

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures"
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 
