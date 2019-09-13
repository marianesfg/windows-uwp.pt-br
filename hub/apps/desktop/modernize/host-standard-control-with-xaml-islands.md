---
description: Este artigo demonstra como hospedar um controle UWP padrão em um aplicativo WPF usando as ilhas XAML.
title: Hospedar um controle UWP padrão em um aplicativo WPF usando as ilhas XAML
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML, controles encapsulados, controles padrão, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2deae93f8a9706b2d5d6bebfa23b852c8d6d554f
ms.sourcegitcommit: 8cbc9ec62a318294d5acfea3dab24e5258e28c52
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70911561"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar um controle UWP padrão em um aplicativo WPF usando as ilhas XAML

Este artigo demonstra duas maneiras de hospedar um controle UWP padrão (ou seja, um controle UWP de primeira parte fornecido pela biblioteca SDK do Windows ou WinUI) em um aplicativo WPF usando as [ilhas XAML](xaml-islands.md):

* Ele mostra como hospedar os controles [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) e [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) de UWP usando [controles encapsulados](xaml-islands.md#wrapped-controls) no kit de ferramentas da Comunidade do Windows. Esses controles encapsulam a interface e a funcionalidade de um pequeno conjunto de controles UWP úteis. Você pode adicioná-los diretamente à superfície de design do seu projeto do WPF ou Windows Forms e, em seguida, usá-los como qualquer outro controle WPF ou Windows Forms no designer.

* Ele também mostra como hospedar um controle [CALENDARVIEW](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) UWP usando o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows. Como apenas um pequeno conjunto de controles UWP está disponível como controles encapsulados, você pode usar [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar qualquer outro controle UWP padrão.

Para hospedar um controle UWP em um aplicativo do WPF, você precisará dos componentes a seguir. Este artigo fornece instruções para criar cada um desses componentes.

* O código-fonte e o projeto para seu aplicativo WPF.
* Um projeto de aplicativo UWP que define uma instância da `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` classe do kit de ferramentas da Comunidade do Windows.
  > [!NOTE]
  > Para garantir que seu aplicativo funcione bem em todos os cenários da ilha XAML, seu projeto do WPF (ou Windows Forms) deve ter `XamlApplication` acesso a um objeto. Esse objeto atua como um provedor de metadados raiz para carregar metadados para tipos de XAML do UWP em assemblies no diretório atual do seu aplicativo. A maneira recomendada para fazer isso é adicionar um projeto de **aplicativo em branco (universal do Windows)** à mesma solução que o projeto do WPF (ou Windows Forms) e revisar a classe padrão `App` neste projeto para derivar de. `XamlApplication`
  >
  > Embora essa etapa não seja necessária para cenários da ilha trivial XAML, como a hospedagem de um controle UWP de primeira empresa, seu `XamlApplication` aplicativo do WPF precisa desse objeto para dar suporte a toda a gama de cenários da ilha XAML, incluindo a hospedagem de controles UWP personalizados. É recomendável que você sempre adicione um projeto UWP e defina `XamlApplication` um objeto em qualquer solução na qual esteja usando as ilhas XAML. Sua solução pode conter apenas um projeto que define um `XamlApplication` objeto. Todos os controles UWP personalizados em seu aplicativo compartilham o `XamlApplication` mesmo objeto.

Embora este artigo demonstre como hospedar controles UWP em um aplicativo do WPF, o processo é semelhante para um aplicativo Windows Forms.

## <a name="create-a-wpf-project"></a>Criar um projeto do WPF

Antes de começar, siga estas instruções para criar um projeto do WPF e configurá-lo para hospedar as ilhas XAML. Se você tiver um projeto existente do WPF, poderá adaptar essas etapas e exemplos de código para seu projeto.

1. No Visual Studio 2019, crie um novo projeto de aplicativo **WPF (.NET Framework)** ou **aplicativo WPF (.NET Core)** . Se você quiser criar um projeto de **aplicativo WPF (.NET Core)** , deverá primeiro instalar a versão de visualização mais recente disponível do [SDK de visualização do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Verifique se as [referências do pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **ferramentas-> Gerenciador de pacotes NuGet-> configurações do Gerenciador de pacotes**.
    2. Verifique se **PackageReference** está selecionado para o **formato de gerenciamento de pacotes padrão**.

3. Clique com o botão direito do mouse no projeto do WPF no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.

4. Na janela **Gerenciador de pacotes NuGet** , certifique-se de que **incluir pré-lançamento** está selecionado.

5. Selecione a guia **procurar** , procure o pacote [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (versão v 6.0.0-preview7 ou posterior) e instale o pacote. Esse pacote fornece tudo o que você precisa para usar os controles de UWP encapsulados para WPF (incluindo [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) e o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .
    > [!NOTE]
    > Windows Forms aplicativos devem usar o pacote [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (versão v 6.0.0-preview7 ou posterior).

6. Configure sua solução para direcionar uma plataforma específica, como x86 ou x64. A maioria dos cenários de ilhas XAML não tem suporte em projetos direcionados a **qualquer CPU**.

    1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades** -> de configuração**Configuration Manager**. 
    2. Em **plataforma de solução ativa**, selecione **novo**. 
    3. Na caixa de diálogo **nova plataforma de solução** , selecione **x64** ou **x86** e pressione **OK**. 
    4. Feche as caixas de diálogo abertas.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>Criar um objeto XamlApplication em um projeto de aplicativo UWP

Em seguida, adicione um projeto de aplicativo UWP à mesma solução que o seu projeto do WPF. Você irá revisar a `App` classe padrão neste projeto para derivar `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` da classe fornecida pelo kit de ferramentas da Comunidade do Windows. Embora essa etapa não seja necessária para cenários de ilha XAML trivial, como hospedar um único controle UWP de terceiros, seu aplicativo do WPF `XamlApplication` precisa desse objeto para dar suporte a toda a gama de cenários da ilha XAML. É recomendável que você sempre adicione esse projeto a qualquer solução na qual esteja usando as ilhas XAML.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **novo projeto**.
2. Adicione um projeto **App em Branco (Universal do Windows)** à sua solução. Verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.
3. No projeto de aplicativo UWP, instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versão v 6.0.0-preview7 ou posterior).
4. Abra o arquivo **app. XAML** e substitua o conteúdo desse arquivo pelo XAML a seguir. Substitua `MyUWPApp` pelo namespace do seu projeto de aplicativo UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Abra o arquivo **app.XAML.cs** e substitua o conteúdo deste arquivo pelo código a seguir. Substitua `MyUWPApp` pelo namespace do seu projeto de aplicativo UWP.

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Exclua o arquivo **MainPage. XAML** do projeto de aplicativo UWP.
7. Crie o projeto de aplicativo UWP.
8. No projeto do WPF, clique com o botão direito do mouse no nó **dependências** e adicione uma referência ao seu projeto de aplicativo UWP.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Hospedar um InkCanvas e InkToolbar usando controles encapsulados

Agora que você configurou seu projeto para usar as ilhas do UWP XAML, agora você está pronto para adicionar os controles do [UWP e do](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) encapsulado ao aplicativo.

1. No **Gerenciador de soluções**, abra o arquivo **MainWindow. XAML** .

2. No elemento **Window** próximo à parte superior do arquivo XAML, adicione o atributo a seguir. Isso faz referência ao namespace XAML para o controle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) encapsulado UWP.

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Depois de adicionar esse atributo, o elemento **Window** agora deve ser semelhante a este.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. No arquivo **MainWindow. XAML** , substitua o elemento existente `<Grid>` pelo XAML a seguir. Esse XAML adiciona um controle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) (prefixado pela palavra-chave **Controls** que você definiu anteriormente como `<Grid>`um namespace) ao.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > Você também pode adicionar esses e outros controles encapsulados à janela arrastando-os da seção **Windows Community Toolkit** da **caixa de ferramentas** para o designer.

4. Salve o arquivo **MainWindow. XAML** .

    Se você tiver um dispositivo que dá suporte a uma caneta digital, como uma superfície e estiver executando esse laboratório em um computador físico, você poderá criar e executar o aplicativo e desenhar tinta digital na tela com a caneta. No entanto, se você não tiver um dispositivo com capacidade de caneta e tentar entrar com o mouse, nada acontecerá. Isso ocorre porque o controle **InkCanvas** está habilitado apenas para canetas digitais por padrão. No entanto, você pode alterar esse comportamento.

5. Abra o arquivo **MainWindow.XAML.cs** .

6. Adicione a seguinte declaração de namespace na parte superior do arquivo:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Localize o `MainWindow()` Construtor. Adicione a seguinte linha de código após o `InitializeComponent()` método e salve o arquivo de código.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Você pode usar o objeto InkPresenter para personalizar a experiência de tinta padrão. Esse código usa a propriedade **InputDeviceTypes** para habilitar o mouse, bem como a entrada de caneta.

8. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Se você estiver usando um computador com um mouse, confirme que pode desenhar algo no espaço da tela de tinta com o mouse.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Hospedar um CalendarView usando o controle de host

Agora que você adicionou o [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e os controles UWP encapsulados [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) ao aplicativo, agora você está pronto para usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para adicionar um [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) ao aplicativo.

> [!NOTE]
> O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) é fornecido pelo pacote [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) . Esse pacote está incluído no pacote [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) que você instalou anteriormente.

1. No **Gerenciador de soluções**, abra o arquivo **MainWindow. XAML** .

2. No elemento **Window** próximo à parte superior do arquivo XAML, adicione o atributo a seguir. Isso faz referência ao namespace XAML para o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Depois de adicionar esse atributo, o elemento **Window** agora deve ser semelhante a este.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. No arquivo **MainWindow. XAML** , substitua o elemento existente `<Grid>` pelo XAML a seguir. Esse XAML adiciona uma linha à grade e adiciona um objeto [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) à última linha. Para hospedar um controle [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) UWP, esse XAML define a `InitialTypeName` Propriedade como o nome totalmente qualificado do controle. Esse XAML também define um manipulador de eventos para `ChildChanged` o evento, que é gerado quando o controle hospedado é renderizado.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. Salve o arquivo **MainWindow. XAML** e abra o arquivo **MainWindow.XAML.cs** .

7. Adicione a seguinte declaração de namespace na parte superior do arquivo:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Adicione o seguinte `ChildChanged` método de manipulador de eventos `MainWindow` à classe e salve o arquivo de código. Quando o controle de host é renderizado, esse manipulador de eventos é executado e cria um manipulador de `SelectedDatesChanged` eventos simples para o evento do controle Calendar.

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Confirme se o controle Calendar agora é exibido na parte inferior da janela.

## <a name="package-the-app"></a>Empacotar o aplicativo

Opcionalmente, você pode empacotar o aplicativo do WPF em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia de empacotamento de aplicativos moderna para Windows e é baseado em uma combinação de tecnologias de instalação do MSI, APPX, App-V e ClickOnce.

As instruções a seguir mostram como empacotar todos os componentes na solução em um pacote MSIX usando o [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) no Visual Studio 2019. Essas etapas serão necessárias apenas se você quiser empacotar o aplicativo do WPF em um pacote MSIX.

1. Adicione um novo [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à sua solução. Ao criar o projeto, selecione **Windows 10, versão 1903 (10,0; Build 18362)** para a **versão de destino** e a **versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **aplicativos** e escolha **Adicionar referência**. Na lista de projetos, selecione o projeto do WPF em sua solução e clique em **OK**.

3. Se o projeto do WPF for destinado ao .NET Core 3, você deverá editar o arquivo de projeto de empacotamento. Essas alterações são necessárias no momento para empacotar aplicativos WPF direcionados ao .NET Core 3 e que hospedam os controles UWP.

    1. No Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto de empacotamento e selecione **Editar arquivo de projeto**.
    2. Localize o elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` no arquivo. Substitua este elemento pelo XML a seguir.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Salve o arquivo de projeto e feche-o.

4. Configure sua solução para direcionar uma plataforma específica, como x86 ou x64. Isso é necessário para criar o aplicativo do WPF em um pacote MSIX usando o projeto de empacotamento de aplicativos do Windows.

    1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades** -> de configuração**Configuration Manager**.
    2. Em **plataforma de solução ativa**, selecione **x64** ou **x86**.
    3. Na linha do seu projeto do WPF, na coluna **plataforma** , selecione **novo**.
    4. Na caixa de diálogo **nova plataforma de solução** , selecione **x64** ou **x86** (a mesma plataforma selecionada para **plataforma de solução ativa**) e clique em **OK**.
    5. Feche as caixas de diálogo abertas.

5. Compile e execute o projeto de empacotamento. Confirme se o WPF é executado e se o controle personalizado UWP é exibido conforme o esperado.

## <a name="related-topics"></a>Tópicos relacionados

* [Controles UWP em aplicativos de área de trabalho](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
