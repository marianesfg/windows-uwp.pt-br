---
description: Este artigo demonstra como hospedar um controle UWP personalizado em um aplicativo WPF usando Ilhas XAML.
title: Hospedar um controle UWP personalizado em um aplicativo WPF usando as Ilhas XAML
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, custom controls, user controls, host controls
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: fa8dd744120d5751dcf8c10a090ccc31094000d2
ms.sourcegitcommit: df0cd9c82d1c0c17ccde424e3c4a6ff680c31a35
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80482496"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar um controle UWP personalizado em um aplicativo WPF usando as Ilhas XAML

Este artigo demonstra como usar o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no Windows Community Toolkit para hospedar um controle UWP personalizado em um aplicativo WPF direcionado ao .NET Core 3. O controle personalizado contém vários controles UWP internos da SDK do Windows e associa uma propriedade em um dos controles UWP a uma cadeia de caracteres no aplicativo WPF. Este artigo também demonstra como hospedar um controle UWP da [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/).

Embora este artigo demonstre como fazer isso em um aplicativo WPF, o processo é semelhante para um aplicativo do Windows Forms. Para uma visão geral sobre a hospedagem de controles UWP em aplicativos WPF e do Windows Forms, confira [este artigo](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="required-components"></a>Componentes necessários

Para hospedar um controle UWP personalizado em um aplicativo WPF (ou do Windows Forms), você precisará dos componentes a seguir em sua solução. Este artigo fornece instruções para criar cada um desses componentes.

* **O projeto e o código-fonte para seu aplicativo**. O uso do controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles UWP personalizados só é compatível com aplicativos direcionados ao .NET Core 3. Esse cenário não é compatível com aplicativos direcionados ao .NET Framework.

* **O controle UWP personalizado**. Você precisará do código-fonte do controle UWP personalizado que deseja hospedar para compilá-lo com seu aplicativo. Normalmente, o controle personalizado é definido em um projeto de biblioteca de classes UWP que você faz referência na mesma solução que o seu projeto do WPF ou Windows Forms.

* **Um projeto de aplicativo UWP que define uma Classe de aplicativo raiz que deriva de XamlApplication**. Seu projeto do WPF ou do Windows Forms deve ter acesso a uma instância da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo Windows Community Toolkit para que ele possa descobrir e carregar controles XAML personalizados da UWP. A maneira recomendada para fazer isso é definir esse objeto em um projeto de aplicativo UWP separado que faça parte da solução do seu aplicativo WPF ou Windows Forms. 

    > [!NOTE]
    > Sua solução pode conter apenas um projeto que define um objeto `XamlApplication`. Todos os controles UWP personalizados em seu aplicativo compartilham o mesmo objeto `XamlApplication`. O projeto que define o objeto `XamlApplication` deve incluir referências a todas as outras bibliotecas UWP e projetos que são usados para hospedar controles UWP na Ilha XAML.

## <a name="create-a-wpf-project"></a>Criar um projeto do WPF

Antes de começar, siga estas instruções para criar um projeto do WPF e configurá-lo para hospedar Ilhas XAML. Se você já tiver um projeto do WPF, poderá adaptar essas etapas e exemplos de código para seu projeto.

> [!NOTE]
> Se você já tiver um projeto que se destina ao .NET Framework, precisará migrá-lo para o .NET Core 3. Para obter mais informações, confira [esta série do blog](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Caso ainda não tenha feito, instale a versão mais recente do [SDK do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. No Visual Studio 2019, crie um projeto **Aplicativo WPF (.NET Core)** .

3. Verifique se as [referências de pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **Ferramentas -> Gerenciador de Pacotes NuGet -> Configurações do Gerenciador de Pacotes**.
    2. Verifique se **PackageReference** está selecionado para **Formato de gerenciamento de pacotes padrão**.

4. Clique com o botão direito do mouse no seu projeto do WPF no **Gerenciador de Soluções** e escolha **Gerenciar Pacotes NuGet**.

5. Na janela **Gerenciador de Pacotes NuGet**, verifique se a opção **Incluir pré-lançamento** está selecionada.

6. Selecione a guia **Procurar**, procure o pacote [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (versão v6.0.0 ou posterior) e instale o pacote. Esse pacote fornece tudo o que você precisa para usar o controle **WindowsXamlHost** para hospedar um controle UWP, incluindo outros pacotes NuGet relacionados.
    > [!NOTE]
    > Os aplicativos do Windows Forms devem usar o pacote [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (versão v6.0.0 ou posterior).

7. Configure sua solução para destino a uma plataforma específica, como x86 ou x64. Os controles UWP personalizados não são compatíveis com projetos direcionados a **Qualquer CPU**.

    1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e selecione **Propriedades** -> **Propriedades de Configuração** -> **Gerenciador de Configurações**.
    2. Em **Plataforma da solução ativa**, selecione **Nova**. 
    3. Na caixa de diálogo **Nova Plataforma de Solução**, selecione **x64** ou **x86** e pressione **OK**. 
    4. Feche as caixas de diálogo abertas.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definir uma classe XamlApplication em um projeto de aplicativo UWP

Agora, adicione um projeto de aplicativo UWP à sua solução e revise a classe `App` padrão neste projeto para derivar da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo Windows Community Toolkit. Essa classe dá suporte à interface [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), que permite que o aplicativo descubra e carregue metadados para controles XAML personalizados da UWP em assemblies no diretório atual do seu aplicativo em tempo de execução. Essa classe também inicializa a estrutura XAML da UWP para o thread atual. 

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
7. Limpe o projeto de aplicativo UWP e compile-o.
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

## <a name="create-a-custom-uwp-control"></a>Criar um controle UWP personalizado

Para hospedar um controle UWP personalizado em seu aplicativo WPF, você deve ter o código-fonte do controle para que possa compilá-lo com o aplicativo. Normalmente, os controles personalizados são definidos em um projeto de biblioteca de classes UWP para uma portabilidade mais fácil.

Nesta seção, você definirá um controle de UWP personalizado simples em um novo projeto de biblioteca de classes. Como alternativa, você pode definir o controle UWP personalizado no projeto de aplicativo UWP criado na seção anterior. No entanto, essas etapas fazem isso em outro projeto de biblioteca de classes para fins ilustrativos, pois normalmente é assim que os controles personalizados são implementados para portabilidade.

Se você já tem um controle personalizado, pode usá-lo em vez do controle mostrado aqui. No entanto, ainda precisará configurar o projeto que contém o controle, conforme mostrado nestas etapas.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **Novo Projeto**.
2. Adicione um projeto **Biblioteca de Classes (Universal do Windows)** à sua solução. Verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.
3. Clique com o botão direito do mouse no arquivo de projeto e selecione **Descarregar Projeto**. Clique com o botão direito do mouse no arquivo de projeto novamente e selecione **Editar**.
4. Antes do elemento de fechamento `</Project>`, adicione o XML a seguir para desabilitar algumas propriedades e, em seguida, salve o arquivo de projeto. Essas propriedades devem ser habilitadas para hospedar o controle UWP personalizado em um aplicativo WPF (ou do Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Clique com o botão direito do mouse no arquivo de projeto e selecione **Recarregar Projeto**.
6. Exclua o arquivo padrão **Class1.cs** e adicione um novo item **Controle de Usuário** ao projeto.
7. No arquivo XAML do controle de usuário, adicione o `StackPanel` a seguir como um filho do `Grid` padrão. Este exemplo adiciona um controle ``TextBlock`` e associa o atributo ``Text`` do controle ao campo ``XamlIslandMessage``.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. No arquivo code-behind do controle de usuário, adicione o campo `XamlIslandMessage` à classe de controle de usuário, conforme mostrado abaixo.

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

9. Compile o projeto de biblioteca de classes UWP.
10. No projeto do WPF, clique com o botão direito do mouse no nó **Dependências** e adicione uma referência ao projeto de biblioteca de classes UWP.
11. No projeto de aplicativo UWP que você configurou anteriormente, clique com o botão direito do mouse no nó **Referências** e adicione uma referência ao projeto de biblioteca de classes UWP.
12. Recompile toda a solução e verifique se todos os projetos serão compilados com êxito.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hospedar o controle UWP personalizado em seu aplicativo WPF

1. No **Gerenciador de Soluções**, expanda o projeto do WPF e abra o arquivo MainWindow.xaml ou alguma outra janela na qual você deseja hospedar o controle personalizado.
2. No arquivo XAML, adicione a declaração a seguir de namespace ao elemento `<Window>`.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. No mesmo arquivo, adicione o controle a seguir ao elemento `<Grid>`. Altere o atributo `InitialTypeName` para o nome totalmente qualificado do controle de usuário em seu projeto de biblioteca de classes UWP.

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

Tradicionalmente, os controles UWP foram lançados como parte do sistema operacional Windows 10 e disponibilizados para os desenvolvedores por meio do SDK do Windows. A [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) é uma abordagem alternativa, em que as versões atualizadas dos controles UWP do SDK do Windows são distribuídas em um pacote NuGet que não está vinculado às versões de SDK do Windows. Essa biblioteca também inclui novos controles que não fazem parte do SDK do Windows e da plataforma UWP padrão. Confira nosso [roteiro da biblioteca WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) para obter mais detalhes.

Esta seção demonstra como adicionar um controle UWP da biblioteca WinUI ao seu controle de usuário, para que você possa hospedar esse controle em no aplicativo WPF.

1. No projeto do aplicativo UWP, instale a versão mais recente do pacote NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

2. No arquivo App.xaml deste projeto, adicione o elemento filho a seguir ao elemento `<xaml:XamlApplication>`.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Depois de adicionar esse elemento, o conteúdo do arquivo agora deve ser semelhante a este.

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

3. No projeto de biblioteca de classes UWP, instale a versão mais recente do pacote NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) (a mesma versão que você instalou no projeto do aplicativo UWP).

4. No mesmo projeto, abra o arquivo XAML para o controle de usuário e adicione a declaração de namespace a seguir ao elemento `<UserControl>`.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. No mesmo arquivo, adicione um elemento `<winui:RatingControl />` como um filho do `<StackPanel>`. Esse elemento adiciona uma instância da classe [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2) da biblioteca WinUI. Depois de adicionar esse elemento, o `<StackPanel>` agora deve ficar com a aparência a seguir.

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

Opcionalmente, você pode empacotar o aplicativo WPF em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia moderna de empacotamento de aplicativo para o Windows e se baseia em uma combinação das tecnologias de instalação MSI, .appx, App-V e ClickOnce.

As instruções a seguir mostram como empacotar todos os componentes da solução em um pacote MSIX usando o [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) no Visual Studio 2019. Essas etapas serão necessárias apenas se você quiser empacotar o aplicativo WPF em um pacote MSIX. Observe que essas etapas atualmente incluem algumas soluções alternativas específicas para o cenário de Hospedagem de controles UWP personalizados.

> [!NOTE]
> Se você optar por não empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação, os computadores que executam o aplicativo precisarão ter o [Runtime do Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) instalado.

1. Adicione um novo [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à solução. Ao criar o projeto, selecione **Windows 10, versão 1903 (10.0; Build 18362)** para a **Versão de destino** e a **Versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **Aplicativos** e escolha **Adicionar referência**. Na lista de projetos, selecione o projeto do WPF em sua solução e clique em **OK**.

3. Edite o arquivo de projeto do WPF. Essas alterações são necessárias no momento para empacotar aplicativos do WPF que hospedam controles UWP personalizados.

    1. No Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto do WPF e selecione **Descarregar Projeto**.
    2. Clique com o botão direito do mouse no nó do projeto do WPF e selecione **Editar**.
    3. Localize a última marca de fechamento `</PropertyGroup>` no arquivo e adicione o XML a seguir, imediatamente após essa marca.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Salve o arquivo do projeto e feche-o.
    5. Clique com o botão direito do mouse no nó do projeto do WPF e escolha **Recarregar Projeto**.

4. Compile e execute o projeto de empacotamento. Confirme se o WPF é executado e se o controle personalizado UWP é exibido conforme o esperado.

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles UWP XAML em aplicativos da área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Exemplos de código de Ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
