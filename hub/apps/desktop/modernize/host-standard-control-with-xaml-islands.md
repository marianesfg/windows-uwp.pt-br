---
description: Este artigo demonstra como hospedar um controle UWP padrão em um aplicativo WPF usando ilhas XAML.
title: Hospedar um controle UWP padrão em um aplicativo WPF usando as Ilhas XAML
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, wrapped controls, standard controls, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ed6aa406cd1372819c25bd43b59cd416130b09e0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80482511"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar um controle UWP padrão em um aplicativo WPF usando as Ilhas XAML

Este artigo demonstra duas maneiras de hospedar um controle UWP padrão (ou seja, um controle UWP interno fornecido pelo SDK do Windows) em um aplicativo WPF usando [Ilhas XAML](xaml-islands.md):

* Ele mostra como hospedar controles [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) e [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) de UWP usando [controles encapsulados](xaml-islands.md#wrapped-controls) no Windows Community Toolkit. Esses controles encapsulam a interface e a funcionalidade de um pequeno conjunto de controles UWP úteis. Adicione-os diretamente à área de design do projeto do WPF ou do Windows Forms e use-os como qualquer outro controle WPF ou Windows Forms no designer.

* Ele também mostra como hospedar um controle UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) usando o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no Windows Community Toolkit. Como apenas um pequeno conjunto de controles UWP está disponível como controles encapsulados, você pode usar o [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar qualquer outro controle UWP padrão.

Embora este artigo demonstre como hospedar controles UWP em um aplicativo WPF, o processo é semelhante para um aplicativo do Windows Forms.

## <a name="required-components"></a>Componentes necessários

Para hospedar um controle UWP em um aplicativo WPF (ou do Windows Forms), você precisará dos componentes a seguir em sua solução. Este artigo fornece instruções para criar cada um desses componentes.

* **O projeto e o código-fonte para seu aplicativo**. O uso do controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles UWP internos padrão é compatível com aplicativos direcionados ao .NET Framework ou ao .NET Core 3.

* **Um projeto de aplicativo UWP que define uma Classe de aplicativo raiz que deriva de XamlApplication**. Seu projeto do WPF ou do Windows Forms deve ter acesso a uma instância da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo Windows Community Toolkit para que ele possa descobrir e carregar controles XAML personalizados da UWP. A maneira recomendada para fazer isso é definir esse objeto em um projeto de aplicativo UWP separado que faça parte da solução do seu aplicativo WPF ou Windows Forms. 

    > [!NOTE]
    > Embora o objeto `XamlApplication` não seja necessário para hospedar um controle UWP interno, seu aplicativo precisa desse objeto para dar suporte a todos os cenários possíveis da Ilha XAML, incluindo a hospedagem de controles UWP personalizados. Portanto, é recomendável que você sempre defina um objeto `XamlApplication` em qualquer solução na qual esteja usando as Ilhas XAML.

    > [!NOTE]
    > Sua solução pode conter apenas um projeto que define um objeto `XamlApplication`. Todos os controles UWP personalizados em seu aplicativo compartilham o mesmo objeto `XamlApplication`. O projeto que define o objeto `XamlApplication` deve incluir referências a todas as outras bibliotecas UWP e projetos que são usados para hospedar controles UWP na Ilha XAML.

## <a name="create-a-wpf-project"></a>Criar um projeto do WPF

Antes de começar, siga estas instruções para criar um projeto do WPF e configurá-lo para hospedar Ilhas XAML. Se você já tiver um projeto do WPF, poderá adaptar essas etapas e exemplos de código para seu projeto.

1. No Visual Studio 2019, crie um projeto de **Aplicativo WPF (.NET Framework)** ou **Aplicativo WPF (.NET Core)** . Se você quiser criar um projeto de **Aplicativo WPF (.NET Core)** , deverá primeiro instalar a versão mais recente do [SDK do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Verifique se as [referências de pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **Ferramentas -> Gerenciador de Pacotes NuGet -> Configurações do Gerenciador de Pacotes**.
    2. Verifique se **PackageReference** está selecionado para **Formato de gerenciamento de pacotes padrão**.

3. Clique com o botão direito do mouse no seu projeto do WPF no **Gerenciador de Soluções** e escolha **Gerenciar Pacotes NuGet**.

4. Na janela **Gerenciador de Pacotes NuGet**, verifique se a opção **Incluir pré-lançamento** está selecionada.

5. Selecione a guia **Procurar**, procure o pacote [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (versão v6.0.0 ou posterior) e instale o pacote. Este pacote fornece tudo o que você precisa para usar os controles UWP encapsulados para o WPF (incluindo [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) e o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost).
    > [!NOTE]
    > Os aplicativos do Windows Forms devem usar o pacote [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (versão v6.0.0 ou posterior).

6. Configure sua solução para destino a uma plataforma específica, como x86 ou x64. A maioria dos cenários de Ilhas XAML não são compatíveis com projetos direcionados a **Qualquer CPU**.

    1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades de Configuração** -> **Gerenciador de Configurações**. 
    2. Em **Plataforma da solução ativa**, selecione **Nova**. 
    3. Na caixa de diálogo **Nova Plataforma de Solução**, selecione **x64** ou **x86** e pressione **OK**. 
    4. Feche as caixas de diálogo abertas.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definir uma classe XamlApplication em um projeto de aplicativo UWP

Agora, adicione um projeto de aplicativo UWP à sua solução e revise a classe `App` padrão neste projeto para derivar da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo Windows Community Toolkit. Essa classe dá suporte à interface [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), que permite que o aplicativo descubra e carregue metadados para controles XAML personalizados da UWP em assemblies no diretório atual do seu aplicativo em tempo de execução. Essa classe também inicializa a estrutura XAML da UWP para o thread atual.

> [!NOTE]
> Embora essa etapa não seja necessária para hospedar um controle UWP interno, seu aplicativo precisa do objeto `XamlApplication` para dar suporte a todos os cenários possíveis da Ilha XAML, incluindo a hospedagem de controles UWP personalizados. Portanto, é recomendável que você sempre defina um objeto `XamlApplication` em qualquer solução na qual esteja usando as Ilhas XAML.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **Novo Projeto**.
2. Adicione um projeto **Aplicativo em Branco (Universal do Windows)** à sua solução. Verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.
3. No projeto de aplicativo UWP, instale o pacote NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versão v6.0.0 ou posterior).
4. Abra o arquivo **App.xaml** e substitua o conteúdo desse arquivo pelo XAML a seguir. Substitua `MyUWPApp` pelo namespace do seu projeto de aplicativo UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Abra o arquivo **App.xaml.cs** e substitua o conteúdo desse arquivo pelo código a seguir. Substitua `MyUWPApp` pelo namespace do seu projeto de aplicativo UWP.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Exclua o arquivo **MainPage.xaml** do projeto de aplicativo UWP.
7. Compile o projeto do aplicativo UWP.
8. No projeto do WPF, clique com o botão direito do mouse no nó **Dependências** e adicione uma referência ao seu projeto de aplicativo UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Criar uma instância do objeto XamlApplication no ponto de entrada do aplicativo WPF

Em seguida, adicione o código ao ponto de entrada do aplicativo WPF para criar uma instância da classe `App` que você acabou de definir no projeto UWP (essa é a classe que agora é derivada de `XamlApplication`).

1. No projeto do WPF, clique com o botão direito do mouse no nó do projeto, selecione **Adicionar** -> **Novo item** e, em seguida, selecione **Classe**. Nomeie a classe como **Programa** e clique em **Adicionar**.

2. Substitua a classe `Program` gerada pelo código a seguir e salve o arquivo. Substitua `MyUWPApp` pelo namespace do seu projeto de aplicativo UWP e substitua `MyWPFApp` pelo namespace do seu projeto de aplicativo WPF.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Clique com o botão direito do mouse no nó do projeto e escolha **Propriedades**.

4. Na guia **Aplicativo** das propriedades, clique na lista suspensa **Objeto de inicialização** e escolha o nome totalmente qualificado da classe `Program` que você adicionou na etapa anterior. 
    > [!NOTE]
    > Por padrão, os projetos do WPF definem uma função de ponto de entrada `Main` em um arquivo de código gerado que não deve ser modificado. Essa etapa altera o ponto de entrada do seu projeto para o método `Main` da nova classe `Program`, o que permite que você adicione o código que é executado o quanto antes no processo de inicialização do aplicativo. 

5. Salve as alterações que você fez nas propriedades do projeto.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Hospedar um InkCanvas e um InkToolbar usando controles encapsulados

Agora que configurou seu projeto para usar as Ilhas XAML da UWP, você está pronto para adicionar os controles UWP encapsulados [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) ao aplicativo.

1. No **Gerenciador de Soluções**, abra o arquivo **MainWindow.xaml**.

2. No elemento **Window** próximo à parte superior do arquivo XAML, adicione o atributo a seguir. Isso faz referência ao namespace XAML para os controles UWP encapsulados [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar).

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Depois de adicionar esse atributo, o elemento **Window** deve ficar como a seguir.

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

3. No arquivo **MainWindow.xaml**, substitua o elemento `<Grid>` existente pelo XAML a seguir. Esse XAML adiciona um controle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e um [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) (prefixados pela palavra-chave **Controls** que você definiu anteriormente como um namespace) para o `<Grid>`.

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
    > Você também pode adicionar esses e outros controles encapsulados à janela arrastando-os da seção **Windows Community Toolkit** da **Caixa de Ferramentas** para o designer.

4. Salve o arquivo **MainWindow.xaml**.

    Se você tiver um dispositivo compatível com uma caneta digital, como um Suface, e estiver executando esse laboratório em um computador físico, poderá criar e executar o aplicativo e desenhar à tinta digital na tela com a caneta. No entanto, se você não tiver um dispositivo compatível com caneta e tentar entrar com o mouse, nada acontecerá. Isso ocorre porque o controle **InkCanvas** é habilitado apenas para canetas digitais por padrão. Porém, é possível alterar esse comportamento.

5. Abra o arquivo **MainWindow.xaml.cs**.

6. Adicione a seguinte declaração de namespace à parte superior do arquivo:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Localize o construtor `MainWindow()`. Adicione a linha de código a seguir após o método `InitializeComponent()` e salve o arquivo de código.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Você pode usar o objeto **InkPresenter** para personalizar a experiência de tinta padrão. Esse código usa a propriedade **InputDeviceTypes** para habilitar o mouse, bem como a entrada à caneta.

8. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Se você estiver usando um computador com um mouse, veja se é possível desenhar algo no espaço da tela de tinta com o mouse.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Hospedar um CalendarView usando o controle de host

Agora que adicionou os controles UWP encapsulados [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) ao aplicativo, você está pronto para usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para adicionar um [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) ao aplicativo.

> [!NOTE]
> O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) é fornecido pelo pacote [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost). Esse pacote está incluído no pacote [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) que você instalou anteriormente.

1. No **Gerenciador de Soluções**, abra o arquivo **MainWindow.xaml**.

2. No elemento **Window** próximo à parte superior do arquivo XAML, adicione o atributo a seguir. Isso faz referência ao namespace XAML para o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost).

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Depois de adicionar esse atributo, o elemento **Window** deve ficar como a seguir.

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

4. No arquivo **MainWindow.xaml**, substitua o elemento `<Grid>` existente pelo XAML a seguir. Esse XAML adiciona uma linha à grade e adiciona um objeto [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) à última linha. Para hospedar um controle UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), esse XAML define a propriedade `InitialTypeName` com o nome totalmente qualificado do controle. Esse XAML também define um manipulador de eventos para o evento `ChildChanged`, que é gerado quando o controle hospedado é renderizado.

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

5. Salve o arquivo **MainWindow.xaml** e abra o arquivo **MainWindow.xaml.cs**.

7. Adicione a seguinte declaração de namespace à parte superior do arquivo:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Adicione o método a seguir do manipulador de eventos `ChildChanged` à classe `MainWindow` e salve o arquivo de código. Quando o controle de host for renderizado, esse manipulador de eventos será executado e criará um manipulador de eventos simples para o evento `SelectedDatesChanged` do controle de calendário.

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

11. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Confirme se o controle de calendário já é exibido na parte inferior da janela.

## <a name="package-the-app"></a>Empacotar o aplicativo

Opcionalmente, você pode empacotar o aplicativo WPF em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia moderna de empacotamento de aplicativo para o Windows e se baseia em uma combinação das tecnologias de instalação MSI, .appx, App-V e ClickOnce.

As instruções a seguir mostram como empacotar todos os componentes da solução em um pacote MSIX usando o [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) no Visual Studio 2019. Essas etapas serão necessárias apenas se você quiser empacotar o aplicativo WPF em um pacote MSIX.

> [!NOTE]
> Se você optar por não empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação, os computadores que executam o aplicativo precisarão ter o [Runtime do Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) instalado.

1. Adicione um novo [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à solução. Ao criar o projeto, selecione **Windows 10, versão 1903 (10.0; Build 18362)** para a **Versão de destino** e a **Versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **Aplicativos** e escolha **Adicionar referência**. Na lista de projetos, selecione o projeto do WPF em sua solução e clique em **OK**.

3. Configure sua solução para destino a uma plataforma específica, como x86 ou x64. Isso é necessário para compilar o aplicativo WPF em um pacote MSIX usando o Projeto de Empacotamento de Aplicativo do Windows.

    1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades de Configuração** -> **Gerenciador de Configurações**.
    2. Em **Plataforma da solução ativa**, selecione **x64** ou **x86**.
    3. Na linha do seu projeto do WPF, na coluna **Plataforma**, selecione **Nova**.
    4. Na caixa de diálogo **Nova Plataforma de Solução**, selecione **x64** ou **x86** (a mesma plataforma selecionada em **Plataforma de solução ativa**) e clique em **OK**.
    5. Feche as caixas de diálogo abertas.

5. Compile e execute o projeto de empacotamento. Confirme se o WPF é executado e se o controle personalizado UWP é exibido conforme o esperado.

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles UWP XAML em aplicativos da área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Exemplos de código de Ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
