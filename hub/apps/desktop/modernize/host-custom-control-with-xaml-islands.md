---
description: Este artigo demonstra como hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML.
title: Hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML, controles personalizados, controles de usuário, controles de host
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b6bd46bcdec639cee2bc867c2c4e71cccbb13cfb
ms.sourcegitcommit: a2a3c887f6da47a6638ce5286199ea31ee7780e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77606696"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML

Este artigo demonstra como usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows para hospedar um controle UWP personalizado em um aplicativo do WPF direcionado ao .NET Core 3. O controle personalizado contém vários controles UWP de terceiros da SDK do Windows e associa uma propriedade em um dos controles UWP a uma cadeia de caracteres no aplicativo do WPF. Este artigo também demonstra como hospedar também um controle UWP de primeira parte da [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/).

Embora este artigo demonstre como fazer isso em um aplicativo do WPF, o processo é semelhante para um aplicativo Windows Forms. Para obter uma visão geral sobre como hospedar controles UWP no WPF e Windows Forms aplicativos, consulte [Este artigo](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="required-components"></a>Componentes necessários

Para hospedar um controle UWP personalizado em um aplicativo WPF (ou Windows Forms), você precisará dos seguintes componentes em sua solução. Este artigo fornece instruções para criar cada um desses componentes.

* **O código-fonte e o projeto para seu aplicativo**. O uso do controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles UWP personalizados tem suporte apenas em aplicativos direcionados ao .NET Core 3. Não há suporte para esse cenário em aplicativos que se destinam ao .NET Framework.

* **O controle UWP personalizado**. Você precisará do código-fonte para o controle UWP personalizado que deseja hospedar para que possa compilá-lo com seu aplicativo. Normalmente, o controle personalizado é definido em um projeto de biblioteca de classes UWP que você faz referência na mesma solução que o seu projeto do WPF ou Windows Forms.

* **Um projeto de aplicativo UWP que define uma classe de aplicativo raiz que deriva de XamlApplication**. Seu projeto do WPF ou Windows Forms deve ter acesso a uma instância da classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo kit de ferramentas da Comunidade do Windows. Esse objeto atua como um provedor raiz de metadados para carregar metadados para tipos personalizados UWP XAML em assemblies no diretório atual do seu aplicativo.

    A maneira recomendada para fazer isso é adicionar um projeto de **aplicativo em branco (universal do Windows)** à mesma solução que o seu projeto do WPF ou Windows Forms, revisar a classe de `App` padrão neste projeto para derivar de `XamlApplication`e, em seguida, criar uma instância desse objeto no código do ponto de entrada para seu aplicativo.

    > [!NOTE]
    > Sua solução pode conter apenas um projeto que define um objeto `XamlApplication`. Todos os controles UWP personalizados em seu aplicativo compartilham o mesmo objeto `XamlApplication`. O projeto que define o objeto `XamlApplication` deve incluir referências a todas as outras bibliotecas UWP e projetos que são usados host para controles UWP na ilha XAML.

## <a name="create-a-wpf-project"></a>Criar um projeto do WPF

Antes de começar, siga estas instruções para criar um projeto do WPF e configurá-lo para hospedar as ilhas XAML. Se você tiver um projeto existente do WPF, poderá adaptar essas etapas e exemplos de código para seu projeto.

> [!NOTE]
> Se você tiver um projeto existente que se destina ao .NET Framework, você precisará migrar seu projeto para o .NET Core 3. Para obter mais informações, consulte [esta série de Blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Caso ainda não tenha feito isso, instale a versão mais recente do [SDK do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. No Visual Studio 2019, crie um novo projeto de **aplicativo WPF (.NET Core)** .

3. Verifique se as [referências do pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **ferramentas-> Gerenciador de pacotes NuGet-> configurações do Gerenciador de pacotes**.
    2. Verifique se **PackageReference** está selecionado para o **formato de gerenciamento de pacotes padrão**.

4. Clique com o botão direito do mouse no projeto do WPF no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.

5. Na janela **Gerenciador de pacotes NuGet** , certifique-se de que **incluir pré-lançamento** está selecionado.

6. Selecione a guia **procurar** , procure o pacote [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (versão v 6.0.0 ou posterior) e instale o pacote. Esse pacote fornece tudo o que você precisa para usar o controle **WindowsXamlHost** para hospedar um controle UWP, incluindo outros pacotes NuGet relacionados.
    > [!NOTE]
    > Windows Forms aplicativos devem usar o pacote [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (versão v 6.0.0 ou posterior).

7. Configure sua solução para direcionar uma plataforma específica, como x86 ou x64. Os controles UWP personalizados não têm suporte em projetos direcionados a **qualquer CPU**.

    1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades de configuração** -> **Configuration Manager**.
    2. Em **plataforma de solução ativa**, selecione **novo**. 
    3. Na caixa de diálogo **nova plataforma de solução** , selecione **x64** ou **x86** e pressione **OK**. 
    4. Feche as caixas de diálogo abertas.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definir uma classe XamlApplication em um projeto de aplicativo UWP

Em seguida, adicione um projeto de aplicativo UWP à mesma solução que o seu projeto do WPF. Você irá revisar a classe de `App` padrão neste projeto para derivar da classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo kit de ferramentas da Comunidade do Windows. Para obter mais informações sobre a finalidade dessa classe, consulte [esta seção](#required-components).

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **novo projeto**.
2. Adicione um projeto **App em Branco (Universal do Windows)** à sua solução. Verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.
3. No projeto de aplicativo UWP, instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versão v 6.0.0 ou posterior).
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
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Exclua o arquivo **MainPage. XAML** do projeto de aplicativo UWP.
7. Limpe o projeto de aplicativo UWP e compile-o.
8. No projeto do WPF, clique com o botão direito do mouse no nó **dependências** e adicione uma referência ao seu projeto de aplicativo UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Criar uma instância do objeto XamlApplication no ponto de entrada do aplicativo do WPF

Em seguida, adicione o código ao ponto de entrada do aplicativo WPF para criar uma instância da classe `App` que você acabou de definir no projeto UWP (essa é a classe que agora é derivada de `XamlApplication`). Para obter mais informações sobre a finalidade deste objeto, consulte [esta seção](#required-components).

1. No projeto do WPF, clique com o botão direito do mouse no nó do projeto, selecione **adicionar** -> **novo item**e, em seguida, selecione **classe**. Nomeie o **programa** de classe e clique em **Adicionar**.

2. Substitua a classe `Program` gerada pelo código a seguir e salve o arquivo. Substitua `MyUWPApp` pelo namespace do seu projeto de aplicativo UWP e substitua `MyWPFApp` pelo namespace do seu projeto de aplicativo do WPF.

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

4. Na guia **aplicativo** das propriedades, clique na lista suspensa **objeto de inicialização** e escolha o nome totalmente qualificado da classe `Program` que você adicionou na etapa anterior. 
    > [!NOTE]
    > Por padrão, os projetos do WPF definem uma `Main` função de ponto de entrada em um arquivo de código gerado que não deve ser modificado. Essa etapa altera o ponto de entrada para o seu projeto para o método `Main` da nova classe `Program`, que permite que você adicione o código que é executado como antes no processo de inicialização do aplicativo possível. 

5. Salve as alterações nas propriedades do projeto.

## <a name="create-a-custom-uwp-control"></a>Criar um controle UWP personalizado

Para hospedar um controle UWP personalizado em seu aplicativo do WPF, você deve ter o código-fonte do controle para que possa compilá-lo com seu aplicativo. Normalmente, os controles personalizados são definidos em um projeto de biblioteca de classes UWP para portabilidade fácil.

Nesta seção, você definirá um controle de UWP personalizado simples em um novo projeto de biblioteca de classes. Como alternativa, você pode definir o controle UWP personalizado no projeto de aplicativo UWP criado na seção anterior. No entanto, essas etapas fazem isso em um projeto separado de biblioteca de classes para fins ilustrativos, pois normalmente é assim que os controles personalizados são implementados para portabilidade.

Se você já tiver um controle personalizado, poderá usá-lo em vez do controle mostrado aqui. No entanto, você ainda precisará configurar o projeto que contém o controle, conforme mostrado nestas etapas.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **novo projeto**.
2. Adicione um projeto de **biblioteca de classes (universal do Windows)** à sua solução. Verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.
3. Clique com o botão direito do mouse no arquivo de projeto e selecione **descarregar projeto**. Clique com o botão direito do mouse no arquivo de projeto novamente e selecione **Editar**.
4. Antes do elemento `</Project>` de fechamento, adicione o seguinte XML para desabilitar várias propriedades e, em seguida, salve o arquivo de projeto. Essas propriedades devem ser habilitadas para hospedar o controle UWP personalizado em um aplicativo WPF (ou Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Clique com o botão direito do mouse no arquivo de projeto e selecione **recarregar projeto**.
6. Exclua o arquivo **Class1.cs** padrão e adicione um novo item de **controle de usuário** ao projeto.
7. No arquivo XAML para o controle de usuário, adicione o seguinte `StackPanel` como um filho do `Grid`padrão. Este exemplo adiciona um controle ``TextBlock`` e associa o atributo ``Text`` do controle ao campo ``XamlIslandMessage``.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. No arquivo code-behind para o controle de usuário, adicione o campo `XamlIslandMessage` à classe de controle de usuário, conforme mostrado abaixo.

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. Crie o projeto de biblioteca de classes UWP.
10. No projeto do WPF, clique com o botão direito do mouse no nó **dependências** e adicione uma referência ao projeto de biblioteca de classes UWP.
11. No projeto de aplicativo UWP que você configurou anteriormente, clique com o botão direito do mouse no nó **referências** e adicione uma referência ao projeto de biblioteca de classes UWP.
12. Reconstrua toda a solução e certifique-se de que todos os projetos sejam compilados com êxito.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hospedar o controle UWP personalizado em seu aplicativo WPF

1. Em **Gerenciador de soluções**, expanda o projeto do WPF e abra o arquivo MainWindow. XAML ou alguma outra janela na qual você deseja hospedar o controle personalizado.
2. No arquivo XAML, adicione a seguinte declaração de namespace ao elemento `<Window>`.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. No mesmo arquivo, adicione o seguinte controle ao elemento `<Grid>`. Altere o atributo `InitialTypeName` para o nome totalmente qualificado do controle de usuário em seu projeto de biblioteca de classes UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Abra o arquivo code-behind e adicione o código a seguir à classe `Window`. Esse código define um manipulador de eventos `ChildChanged` que atribui o valor do campo ``XamlIslandMessage`` do controle personalizado UWP ao valor do campo `WPFMessage` no aplicativo WPF. Altere `UWPClassLibrary.MyUserControl` para o nome totalmente qualificado do controle de usuário em seu projeto de biblioteca de classes UWP.

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. Compile e execute seu aplicativo e confirme se o controle de usuário UWP é exibido conforme o esperado.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Adicionar um controle da biblioteca WinUI ao controle personalizado

Tradicionalmente, os controles UWP foram lançados como parte do sistema operacional Windows 10 e disponibilizados para os desenvolvedores por meio do SDK do Windows. A [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) é uma abordagem alternativa, em que versões atualizadas dos controles UWP de primeira parte da SDK do Windows são distribuídas em um pacote NuGet que não está vinculado a versões SDK do Windows. Essa biblioteca também inclui novos controles que não fazem parte do SDK do Windows e da plataforma UWP padrão. Consulte nosso [roteiro da biblioteca WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) para obter mais detalhes.

Esta seção demonstra como adicionar um controle UWP da biblioteca WinUI ao seu controle de usuário para que você possa hospedar esse controle em seu aplicativo WPF.

1. No projeto de aplicativo UWP, instale a versão mais recente do pacote NuGet [Microsoft. UI. XAML](https://www.nuget.org/packages/Microsoft.UI.Xaml) .

2. No arquivo app. XAML neste projeto, adicione o elemento filho a seguir ao elemento `<xaml:XamlApplication>`.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Depois de adicionar esse elemento, o conteúdo desse arquivo agora deve ser semelhante a este.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. No projeto de biblioteca de classes UWP, instale a versão mais recente do pacote NuGet [Microsoft. UI. XAML](https://www.nuget.org/packages/Microsoft.UI.Xaml) (a mesma versão que você instalou no projeto de aplicativo UWP).

4. No mesmo projeto, abra o arquivo XAML para o controle de usuário e adicione a seguinte declaração de namespace ao elemento `<UserControl>`.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. No mesmo arquivo, adicione um elemento `<winui:RatingControl />` como um filho do `<StackPanel>`. Esse elemento adiciona uma instância da classe [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2) da biblioteca WinUI. Depois de adicionar esse elemento, o `<StackPanel>` agora deve ser semelhante a este.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Compile e execute seu aplicativo e confirme se o novo controle de classificação é exibido conforme o esperado.

## <a name="package-the-app"></a>Empacotar o aplicativo

Opcionalmente, você pode empacotar o aplicativo do WPF em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia de empacotamento de aplicativo moderna para Windows e é baseado em uma combinação das tecnologias de instalação MSI,. Appx, App-V e ClickOnce.

As instruções a seguir mostram como empacotar todos os componentes na solução em um pacote MSIX usando o [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) no Visual Studio 2019. Essas etapas serão necessárias apenas se você quiser empacotar o aplicativo do WPF em um pacote MSIX. Observe que essas etapas atualmente incluem algumas soluções alternativas específicas para o cenário de Hospedagem de controles UWP personalizados.

1. Adicione um novo [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à sua solução. Ao criar o projeto, selecione **Windows 10, versão 1903 (10,0; Build 18362)** para a **versão de destino** e a **versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **aplicativos** e escolha **Adicionar referência**. Na lista de projetos, selecione o projeto do WPF em sua solução e clique em **OK**.

3. Edite o arquivo de projeto do WPF. Essas alterações são necessárias no momento para empacotar aplicativos WPF que hospedam controles UWP personalizados.

    1. Em Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto WPF e selecione **descarregar projeto**.
    2. Clique com o botão direito do mouse no nó do projeto WPF e selecione **Editar**.
    3. Localize a última `</PropertyGroup>` marca de fechamento no arquivo e adicione o seguinte XML imediatamente após essa marca.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Salve o arquivo do projeto e feche-o.
    5. Clique com o botão direito do mouse no nó do projeto WPF e escolha **recarregar projeto**.

4. Compile e execute o projeto de empacotamento. Confirme se o WPF é executado e se o controle personalizado UWP é exibido conforme o esperado.

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Exemplos de código de ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
