---
description: Este artigo demonstra como hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML.
title: Hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML, controles personalizados, controles de usuário, controles de host
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9f8d9d56d8a05be7d22ea52c9b45908c246fc250
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643760"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML

Este artigo demonstra como usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows para hospedar um controle UWP personalizado em um aplicativo do WPF direcionado ao .NET Core 3. O controle personalizado contém vários controles UWP de terceiros e associa uma propriedade em um dos controles UWP a uma cadeia de caracteres no aplicativo do WPF.

Embora este artigo demonstre como fazer isso em um aplicativo do WPF, o processo é semelhante para um aplicativo Windows Forms. Para obter uma visão geral sobre como hospedar controles UWP no WPF e Windows Forms aplicativos, consulte [Este artigo](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="overview"></a>Visão geral

Para hospedar um controle UWP personalizado em um aplicativo do WPF, você precisará dos seguintes componentes. Este artigo fornece instruções para criar cada um desses componentes.

* **O código-fonte e o projeto para seu aplicativo WPF**. O uso do controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles UWP personalizados tem suporte apenas no WPF e Windows Forms aplicativos direcionados ao .NET Core 3. Não há suporte para esse cenário em aplicativos que se destinam ao .NET Framework.

* **O controle UWP personalizado**. Você precisará do código-fonte para o controle UWP personalizado que deseja hospedar para que possa compilá-lo com seu aplicativo. Normalmente, o controle personalizado é definido em um projeto de biblioteca de classes UWP que você faz referência na mesma solução que o projeto do WPF (ou Windows Forms).

* **Um projeto de aplicativo UWP que define um objeto XamlApplication**. Seu projeto do WPF (ou Windows Forms) deve ter acesso a uma instância da `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` classe fornecida pelo kit de ferramentas da Comunidade do Windows. Esse objeto atua como um provedor raiz de metadados para carregar metadados para tipos personalizados UWP XAML em assemblies no diretório atual do seu aplicativo. A maneira recomendada para fazer isso é adicionar um projeto de **aplicativo em branco (universal do Windows)** à mesma solução que o projeto do WPF (ou Windows Forms) e revisar a classe padrão `App` neste projeto.
  > [!NOTE]
  > Sua solução pode conter apenas um projeto que define um `XamlApplication` objeto. Todos os controles UWP personalizados em seu aplicativo compartilham o `XamlApplication` mesmo objeto. O projeto que define o `XamlApplication` objeto deve incluir referências a todas as outras bibliotecas UWP e projetos que são usados para hospedar controles UWP na ilha XAML.

## <a name="create-a-wpf-project"></a>Criar um projeto do WPF

Antes de começar, siga estas instruções para criar um projeto do WPF e configurá-lo para hospedar as ilhas XAML. Se você tiver um projeto existente do WPF, poderá adaptar essas etapas e exemplos de código para seu projeto.

> [!NOTE]
> Se você tiver um projeto existente que se destina ao .NET Framework, você precisará migrar seu projeto para o .NET Core 3. Para obter mais informações, consulte [esta série de Blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Se você ainda não tiver feito isso, instale a versão de visualização mais recente disponível do [SDK de visualização do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. No Visual Studio 2019, crie um novo projeto de **aplicativo WPF (.NET Core)** .

3. Verifique se as [referências do pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **ferramentas-> Gerenciador de pacotes NuGet-> configurações do Gerenciador de pacotes**.
    2. Verifique se **PackageReference** está selecionado para o **formato de gerenciamento de pacotes padrão**.

4. Clique com o botão direito do mouse no projeto do WPF no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.

5. Na janela **Gerenciador de pacotes NuGet** , certifique-se de que **incluir pré-lançamento** está selecionado.

6. Selecione a guia **procurar** , procure o pacote [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (versão v 6.0.0-preview7 ou posterior) e instale o pacote. Esse pacote fornece tudo o que você precisa para usar o controle **WindowsXamlHost** para hospedar um controle UWP, incluindo outros pacotes NuGet relacionados.
    > [!NOTE]
    > Windows Forms aplicativos devem usar o pacote [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (versão v 6.0.0-preview7 ou posterior).

7. Configure sua solução para direcionar uma plataforma específica, como x86 ou x64. Os controles UWP personalizados não têm suporte em projetos direcionados a **qualquer CPU**.

    1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades** -> de configuração**Configuration Manager**. 
    2. Em **plataforma de solução ativa**, selecione **novo**. 
    3. Na caixa de diálogo **nova plataforma de solução** , selecione **x64** ou **x86** e pressione **OK**. 
    4. Feche as caixas de diálogo abertas.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>Criar um objeto XamlApplication em um projeto de aplicativo UWP

Em seguida, adicione um projeto de aplicativo UWP à mesma solução que o seu projeto do WPF. Você irá revisar a `App` classe padrão neste projeto para derivar `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` da classe fornecida pelo kit de ferramentas da Comunidade do Windows. O objeto **WindowsXamlHost** em seu aplicativo WPF precisa desse `XamlApplication` objeto para hospedar controles UWP personalizados.

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
8. No projeto do WPF, clique com o botão direito do mouse no nó dependências e adicione uma referência ao seu projeto de aplicativo UWP.

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
7. No arquivo XAML para o controle de usuário, adicione o seguinte `StackPanel` como um filho do padrão. `Grid` Este exemplo adiciona um ``TextBlock`` controle e, em seguida, ``Text`` associa o atributo ``XamlIslandMessage`` do controle ao campo.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. No arquivo code-behind para o controle de usuário, adicione o `XamlIslandMessage` campo à classe de controle de usuário, conforme mostrado abaixo.

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
10. No projeto do WPF, clique com o botão direito do mouse no nó dependências e adicione uma referência ao projeto de biblioteca de classes UWP.
11. No projeto de aplicativo UWP que você configurou anteriormente, clique com o botão direito do mouse no nó **referências** e adicione uma referência ao projeto de biblioteca de classes UWP.
12. Reconstrua toda a solução e certifique-se de que todos os projetos sejam compilados com êxito.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hospedar o controle UWP personalizado em seu aplicativo WPF

1. Em **Gerenciador de soluções**, expanda o projeto do WPF e abra o arquivo MainWindow. XAML ou alguma outra janela na qual você deseja hospedar o controle personalizado.
2. No arquivo XAML, adicione a seguinte declaração de namespace ao `<Window>` elemento.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. No mesmo arquivo, adicione o seguinte controle ao `<Grid>` elemento. Altere o `InitialTypeName` atributo para o nome totalmente qualificado do controle de usuário em seu projeto de biblioteca de classes UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Abra o arquivo code-behind e adicione o código a seguir à `Window` classe. Esse código define um `ChildChanged` manipulador de eventos que atribui o valor ``XamlIslandMessage`` do campo do controle personalizado UWP `WPFMessage` ao valor do campo no aplicativo do WPF. Altere `UWPClassLibrary.MyUserControl` para o nome totalmente qualificado do controle de usuário em seu projeto de biblioteca de classes UWP.

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

## <a name="package-the-app"></a>Empacotar o aplicativo

Opcionalmente, você pode empacotar o aplicativo do WPF em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia de empacotamento de aplicativos moderna para Windows e é baseado em uma combinação de tecnologias de instalação do MSI, APPX, App-V e ClickOnce.

As instruções a seguir mostram como empacotar todos os componentes na solução em um pacote MSIX usando o projeto de empacotamento de [aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) no Visual Studio 2019. Essas etapas serão necessárias apenas se você quiser empacotar o aplicativo do WPF em um pacote MSIX. Observe que essas etapas atualmente incluem algumas soluções alternativas específicas para o cenário de Hospedagem de controles UWP personalizados.

1. Adicione um novo [projeto](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) de empacotamento de aplicativos do Windows à sua solução. Ao criar o projeto, selecione **Windows 10, versão 1903 (10,0; Build 18362)** para a **versão de destino** e a **versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **aplicativos** e escolha **Adicionar referência**. Na lista de projetos, selecione o projeto do WPF em sua solução e clique em **OK**.

3. Edite o arquivo de projeto de empacotamento. Essas alterações são necessárias no momento para empacotar aplicativos WPF direcionados ao .NET Core 3 e que hospedam as ilhas XAML.

    1. No Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto de empacotamento e selecione **Editar arquivo de projeto**.
    2. Localize o elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` no arquivo. Substitua este elemento pelo XML a seguir. Essas alterações são necessárias no momento para empacotar aplicativos WPF direcionados ao .NET Core 3 e que hospedam os controles UWP.

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
                <SourceProject>
                </SourceProject>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Salve o arquivo de projeto e feche-o.

4. Edite o manifesto do pacote para fazer referência à imagem de tela inicial padrão correta. Atualmente, essa solução alternativa é necessária para empacotar aplicativos WPF que hospedam controles UWP personalizados.

    1. No projeto de empacotamento, clique com o botão direito do mouse no arquivo **Package. appxmanifest** e clique em **Exibir código**.
    2. Localize o seguinte elemento no arquivo.

        ```<uap:SplashScreen Image="Images\SplashScreen.png" />```

    3. Altere este elemento para:

        ```<uap:SplashScreen Image="Images\SplashScreen.scale-200.png" />```

    4. Salve o arquivo **Package. appxmanifest** e feche-o.

5. Edite o arquivo de projeto do WPF. Essas alterações são necessárias no momento para empacotar aplicativos WPF que hospedam controles UWP personalizados.

    1. Em Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto WPF e selecione **descarregar projeto**.
    2. Clique com o botão direito do mouse no nó do projeto WPF e selecione **Editar**.
    3. Localize a última `</PropertyGroup>` marca de fechamento no arquivo e adicione o seguinte XML imediatamente após essa marca.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Salve o arquivo de projeto e feche-o.
    5. Clique com o botão direito do mouse no nó do projeto WPF e escolha **recarregar projeto**.

6. Compile e execute o projeto de empacotamento. Confirme se o WPF é executado e se o controle personalizado UWP é exibido conforme o esperado.

## <a name="related-topics"></a>Tópicos relacionados

* [Controles UWP em aplicativos de área de trabalho](xaml-islands.md)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
