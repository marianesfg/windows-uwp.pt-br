---
description: Este artigo demonstra como hospedar um controle UWP personalizado em um aplicativo C++ Win32 usando a API de Hospedagem XAML.
title: Hospedar um controle UWP personalizado em um aplicativo C++ Win32 usando a API de Hospedagem XAML
ms.date: 04/07/2020
ms.topic: article
keywords: windows 10, uwp, C++, Win32, xaml islands, custom controls, user controls, host controls
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: eac2574d48864ba8b8dc907c8a7ec43ef266358b
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606326"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Hospedar um controle UWP personalizado em um aplicativo C++ Win32

Este artigo demonstra como usar a [API de Hospedagem UWP XAML](using-the-xaml-hosting-api.md) para hospedar um controle UWP XAML personalizado em um novo aplicativo C++ Win32. Se você já tiver um projeto de aplicativo C++ Win32, poderá adaptar estas etapas e estes exemplos de código para o seu projeto.

Para hospedar um controle UWP XAML personalizado, você criará os seguintes projetos e componentes como parte deste passo a passos:

* **Projeto de aplicativo da área de trabalho do Windows**. Este projeto implementa um aplicativo da área de trabalho C++ Win32 nativo. Você adicionará um código a este projeto que usa a API de Hospedagem UWP XAML para hospedar um controle UWP XAML personalizado.

* **Projeto de aplicativo UWP (C++/WinRT)** . Este projeto implementa um controle personalizado UWP XAML. Também implementa um provedor de metadados raiz para carregar os metadados dos tipos UWP XAML personalizados no projeto.

## <a name="requirements"></a>Requisitos

* Visual Studio 2019 versão 16.4.3 ou posterior.
* SDK do Windows 10, versão 1903 (versão 10.0.18362) ou posterior.
* [VSIX (Extensão do Visual Studio) C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) instalada com o Visual Studio. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do WinRT (Windows Runtime), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Para obter mais informações, confira [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Criar um projeto de aplicativo da área de trabalho

1. No Visual Studio, crie um projeto **Aplicativo da Área de Trabalho do Windows** chamado **MyDesktopWin32App**. Esse modelo de projeto está disponível nos filtros de projeto **C++** , **Windows** e **Área de Trabalho**.

2. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução, clique em **Redirecionar solução**, selecione **10.0.18362.0** ou uma versão posterior do SDK e, em seguida, clique em **OK**.

3. Instale o pacote NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para habilitar o suporte para o [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) no projeto:

    1. Clique com o botão direito do mouse no projeto **MyDesktopWin32App** no **Gerenciador de Soluções** e escolha **Gerenciar Pacotes NuGet**.
    2. Selecione a guia **Procurar**, procure o pacote [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale a última versão desse pacote.

4. Na janela **Gerenciar Pacotes NuGet**, instale os seguintes pacotes NuGet adicionais:

    * [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (versão v6.0.0 ou posterior). Esse pacote fornece vários ativos de build e runtime que permitem que as Ilhas XAML funcionem no seu aplicativo.
    * [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versão v6.0.0 ou posterior). Esse pacote define a classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication), que você usará posteriormente neste passo a passo.
    * [Microsoft.VCRTForwarders.140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Compile a solução e confirme se ela foi compilada com êxito.

## <a name="create-a-uwp-app-project"></a>Criar um projeto de aplicativo UWP

Em seguida, adicione um projeto de aplicativo **UWP (C++/WinRT)** à solução e faça algumas alterações de configuração nesse projeto. Posteriormente neste passo a passo, você adicionará o código a esse projeto para implementar um controle UWP XAML personalizado e definir uma instância da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication). 

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **Novo Projeto**.

2. Adicione o projeto **Aplicativo em Branco (C++/WinRT)** à solução. Nomeie o projeto **MyUWPApp** e verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.

3. Instale o pacote NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) no projeto **MyUWPApp**. Esse pacote define a classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication), que você usará posteriormente neste passo a passo.

    1. Clique com o botão direito do mouse no projeto **MyUWPApp** e escolha **Gerenciar Pacotes NuGet**.
    2. Selecione a guia **Procurar**, procure o pacote [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) e instale a v6.0.0 ou uma versão posterior desse pacote.

4. Clique com o botão direito do mouse no nó **MyUWPApp** e selecione **Propriedades**. Na página **Propriedades Comuns** -> **C++/WinRT**, defina as propriedades a seguir e, em seguida, clique em **Aplicar**.

    * Defina **Detalhamento** como **Normal**.
    * Defina **Otimizado** como **Não**.

    Quando você terminar, a página de propriedades deverá ter a aparência a seguir.

    ![Propriedades do projeto C++/WinRT](images/xaml-islands/xaml-island-cpp-1.png)

5. Na página **Propriedades de Configuração** -> **Geral** da janela Propriedades, defina o **Tipo de Configuração** como **Biblioteca Dinâmica (.dll)** e, em seguida, clique em **OK** para fechar a janela Propriedades.

    ![Propriedades gerais do projeto](images/xaml-islands/xaml-island-cpp-2.png)

6. Adicione um arquivo executável de espaço reservado ao projeto **MyUWPApp**. Esse arquivo executável de espaço reservado é necessário para que o Visual Studio gere os arquivos de projeto necessários e compilar o projeto corretamente.

    1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do projeto **MyUWPApp** e selecione **Adicionar** -> **Novo Item**.
    2. Na caixa de diálogo **Adicionar Novo Item**, selecione **Utilitário** na página esquerda e, em seguida, selecione **Arquivo de Texto (.txt)** . Insira o nome **placeholder.exe** e clique em **Adicionar**.
      ![Adicionar arquivo de texto](images/xaml-islands/xaml-island-cpp-3.png)
    3. No **Gerenciador de Soluções**, selecione o arquivo **placeholder.exe**. Na janela **Propriedades**, verifique se a propriedade **Conteúdo** está definida como **Verdadeiro**.
    4. No **Gerenciador de Soluções**, clique com o botão direito do mouse no arquivo **Package.appxmanifest** do projeto **MyUWPApp**, selecione **Abrir Com**, **Editor de XML (Texto)** e clique em **OK**.
    5. Encontre o elemento **&lt;Application&gt;** e altere o atributo **Executável** para o valor `placeholder.exe`. Quando você terminar, o elemento **&lt;Application&gt;** deverá ser semelhante a este.

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. Salve e feche o arquivo **Package.appxmanifest**.

7. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó **MyUWPApp** e selecione **Descarregar Projeto**.
8. Clique com o botão direito do mouse no nó **MyUWPApp** e selecione **Editar MyUWPApp.vcxproj**.
9. Encontre o elemento `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` e substitua-o pelo XML a seguir. Esse XML adiciona várias novas propriedades imediatamente antes do elemento.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. Salve e feche o arquivo de projeto.
11. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó **MyUWPApp** e selecione **Recarregar Projeto**.

## <a name="configure-the-solution"></a>Configurar a solução

Nesta seção, você atualizará a solução que contém os dois projetos para configurar as dependências do projeto e as propriedades de build necessárias para que os projetos sejam compilados corretamente.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e adicione um novo arquivo XML chamado **Solution.props**.
2. Adicione o XML a seguir ao arquivo **Solution.props**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. No menu **Exibir**, clique em **Gerenciador de Propriedades** (dependendo da configuração, ele poderá estar em **Exibir** -> **Outras Janelas**).
4. Na janela **Gerenciador de Propriedades**, clique com o botão direito do mouse em **MyDesktopWin32App** e selecione **Adicionar Folha de Propriedades Existente**. Navegue até o arquivo **Solution.props** recém-adicionado e clique em **Abrir**.
5. Repita a etapa anterior para adicionar o arquivo **Solution.props** ao projeto **MyUWPApp** na janela **Gerenciador de Propriedades**.
6. Feche a janela **Gerenciador de Propriedades**.
7. Confirme se as alterações da folha de propriedades foram salvas corretamente. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **MyDesktopWin32App** e escolha **Propriedades**. Clique em **Propriedades de Configuração** -> **Geral** e confirme se as propriedades **Diretório de Saída** e **Diretório Intermediário** têm os valores que você adicionou ao arquivo **Solution.props**. Confirme também o mesmo para o projeto **MyUWPApp**.
    ![Propriedades do projeto](images/xaml-islands/xaml-island-cpp-4.png)

8. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nó da solução e escolha **Dependências do Projeto**. Na lista suspensa **Projetos**, verifique se **MyDesktopWin32App** está selecionado e selecione **MyUWPApp** na lista **Depende de**.
    ![Dependências de projeto](images/xaml-islands/xaml-island-cpp-5.png)

9. Clique em **OK**.

## <a name="add-code-to-the-uwp-app-project"></a>Adicionar o código ao projeto de aplicativo UWP

Agora você está pronto para adicionar o código ao projeto **MyUWPApp** e executar estas tarefas:

* Implemente um controle UWP XAML personalizado. Mais adiante neste passo a passo, você adicionará o código que hospeda esse controle no projeto **MyDesktopWin32App**.
* Defina um tipo que deriva da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) no Windows Community Toolkit.

### <a name="define-a-custom-uwp-xaml-control"></a>Definir um controle UWP XAML personalizado

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **MyUWPApp** e selecione **Adicionar** -> **Novo Item**. Selecione o nó **Visual C++** no painel esquerdo, selecione **Controle de Usuário em Branco (C++/WinRT)** , dê a ele o nome **MyUserControl** e clique em **Adicionar**.
2. No editor XAML, substitua o conteúdo do arquivo **MyUserControl.xaml** pelo XAML a seguir e, em seguida, salve o arquivo.

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>Definir uma classe XamlApplication

Em seguida, revise a classe **App** padrão no projeto **MyUWPApp** para que ela derive da classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo Windows Community Toolkit. Essa classe dá suporte à interface [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), que permite que o aplicativo descubra e carregue metadados para controles XAML personalizados da UWP em assemblies no diretório atual do seu aplicativo em tempo de execução. Essa classe também inicializa a estrutura XAML da UWP para o thread atual. Posteriormente neste tutorial, você atualizará o projeto da área de trabalho para criar uma instância dessa classe.

  > [!NOTE]
  > Cada solução que usa as Ilhas XAML pode conter apenas um projeto que define um objeto `XamlApplication`. Todos os controles UWP XAML personalizados no aplicativo compartilham o objeto `XamlApplication`. 

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no arquivo **MainPage.xaml** no projeto **MyUWPApp**. Clique em **Remover** e, em seguida, em **Excluir** para excluir este arquivo permanentemente do projeto.
2. No projeto **MyUWPApp**, expanda o arquivo **App.xaml**.
3. Substitua o conteúdo dos arquivos **App.xaml**, **App.cpp**, **App.h** e **App.idl** pelo código a seguir.

    * **App.xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App.idl**:

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App.h**:

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App.cpp**:

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. Adicione um novo arquivo de cabeçalho ao projeto **MyUWPApp** chamado **app.base.h**.
5. Adicione o código a seguir ao arquivo **app.base.h**, salve o arquivo e feche-o.

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. Compile a solução e confirme se ela foi compilada com êxito.

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Configurar o projeto de área de trabalho para consumir tipos de controles personalizados

Para que o aplicativo **MyDesktopWin32App** hospede um controle UWP XAML personalizado em uma Ilha XAML, ele precisa ser configurado para consumir tipos de controles personalizados do projeto **MyUWPApp**. Há duas maneiras de fazer isso e você pode escolher uma das opções ao concluir este passo a passo.

### <a name="option-1-package-the-app-using-msix"></a>Opção 1: Empacotar o aplicativo usando o MSIX

Você pode empacotar o aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia moderna de empacotamento de aplicativo para o Windows e se baseia em uma combinação das tecnologias de instalação MSI, .appx, App-V e ClickOnce.

1. Adicione um novo [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à solução. Ao criar o projeto, dê a ele o nome **MyDesktopWin32Project** e selecione **Windows 10, versão 1903 (10.0; Build 18362)** para a **Versão de destino** e a **Versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **Aplicativos** e escolha **Adicionar referência**. Na lista de projetos, marque a caixa de seleção ao lado do projeto **MyDesktopWin32App** e clique em **OK**.
    ![Projeto de referência](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Se você optar por não empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação, os computadores que executam o aplicativo precisarão ter o [Runtime do Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) instalado.

### <a name="option-2-create-an-application-manifest"></a>Opção 2: Criar um manifesto do aplicativo

Você pode adicionar um [manifesto do aplicativo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu aplicativo.

1. Clique com o botão direito do mouse no projeto **MyDesktopWin32App** e selecione **Adicionar** -> **Novo Item**. 
2. Na caixa de diálogo **Adicionar Novo Item**, clique em **Web** no painel esquerdo e selecione **Arquivo XML (.xml)** . 
3. Dê ao novo arquivo o nome **app.manifest** e clique em **Adicionar**.
4. Substitua o conteúdo do novo arquivo pelo XML a seguir. Esse XML registra os tipos de controles personalizados no projeto **MyUWPApp**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>Configurar propriedades adicionais do projeto de área de trabalho

Em seguida, atualize o projeto **MyDesktopWin32App** para definir uma macro para diretórios de inclusão adicionais e configurar outras propriedades.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **MyDesktopWin32App** e selecione **Descarregar Projeto**.

2. Clique com o botão direito do mouse em **MyDesktopWin32App (Descarregado)** e selecione **Editar MyDesktopWin32App.vcxproj**.

3. Adicione o XML a seguir logo antes da marca `</Project>` de fechamento ao final do arquivo. Em seguida, salve e feche o arquivo.

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **MyDesktopWin32App (Descarregado)** e selecione **Recarregar Projeto**.

5. Clique com o botão direito do mouse em **MyDesktopWin32App**, selecione **Propriedades** e clique no nó **C/C++** no painel esquerdo. Confirme se a macro **Diretórios de Inclusão Adicionais** foi definida após a alteração de arquivo de projeto feita na etapa anterior.

    ![Configurações de projeto C/C++](images/xaml-islands/xaml-island-cpp-7.png)

6. Na caixa de diálogo **Páginas de Propriedades**, expanda **Ferramenta de Manifesto** -> **Entrada e Saída**. Defina a propriedade **Reconhecimento de DPI** como **Reconhecimento de Alto DPI por Monitor**. Se você não definir essa propriedade, poderá receber um erro de configuração do manifesto em alguns cenários de alto DPI.

    ![Configurações de projeto C/C++](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Hospedar o controle UWP XAML personalizado no projeto de área de trabalho

Por fim, você estará pronto para adicionar o código ao projeto **MyDesktopWin32App** e hospedar o controle UWP XAML personalizado definido anteriormente no projeto **MyUWPApp**.

1. No projeto **MyDesktopWin32App**, abra o arquivo **framework.h** e comente a linha de código a seguir. Salve o arquivo quando terminar.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Abra o arquivo **MyDesktopWin32App.h** e substitua o conteúdo desse arquivo pelo código a seguir para referenciar os arquivos de cabeçalho C++/WinRT necessários. Salve o arquivo quando terminar.

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. Abra o arquivo **MyDesktopWin32App.cpp** e adicione o código a seguir à seção `Global Variables:`.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. No mesmo arquivo, adicione o código a seguir à seção `Forward declarations of functions included in this code module:`.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. No mesmo arquivo, adicione o código a seguir imediatamente após o comentário `TODO: Place code here.` na função `wWinMain`.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. No mesmo arquivo, substitua a função `InitInstance` padrão pelo código a seguir.

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. No mesmo arquivo, adicione a nova função a seguir ao final do arquivo.

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. No mesmo arquivo, localize a função `WndProc`. Substitua o manipulador `WM_DESTROY` padrão na instrução switch pelo código a seguir.

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. Salve o arquivo.
10. Compile a solução e confirme se ela foi compilada com êxito.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Adicionar um controle da biblioteca WinUI ao controle personalizado

Tradicionalmente, os controles UWP foram lançados como parte do sistema operacional Windows 10 e disponibilizados para os desenvolvedores por meio do SDK do Windows. A [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) é uma abordagem alternativa, em que as versões atualizadas dos controles UWP do SDK do Windows são distribuídas em um pacote NuGet que não está vinculado às versões de SDK do Windows. Essa biblioteca também inclui novos controles que não fazem parte do SDK do Windows e da plataforma UWP padrão. Confira nosso [roteiro da biblioteca WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) para obter mais detalhes.

Esta seção demonstra como adicionar um controle UWP da biblioteca WinUI ao seu controle de usuário.

1. No projeto **MyUWPApp**, instale a versão de lançamento ou pré-lançamento mais recente do pacote NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

    > [!NOTE]
    > Se seu aplicativo da área de trabalho estiver empacotado em um [pacote MSIX](https://docs.microsoft.com/windows/msix), será possível usar uma versão de pré-lançamento ou de lançamento do pacote NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Se seu aplicativo da área de trabalho não estiver empacotado usando MSIX, será necessário instalar uma versão de pré-lançamento do pacote NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

2. No arquivo pch.h desse projeto, adicione as instruções `#include` a seguir e salve as alterações. Essas instruções trazem um conjunto necessário de cabeçalhos de projeção da biblioteca WinUI para seu projeto. Essa etapa é necessária para qualquer projeto C++/WinRT que usa a biblioteca WinUI. Para obter mais informações, consulte [este artigo](https://docs.microsoft.com/uwp/toolkits/winui/getting-started#additional-steps-for-a-cwinrt-project).

    ```cpp
    #include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
    #include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
    #include "winrt/Microsoft.UI.Xaml.Media.h"
    #include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
    ```

3. No arquivo App.xaml no mesmo projeto, adicione o elemento filho a seguir ao elemento `<xaml:XamlApplication>` e salve suas alterações.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Depois de adicionar esse elemento, o conteúdo do arquivo agora deve ser semelhante a este.

    ```xml
    <Toolkit:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
        </Application.Resources>
    </Toolkit:XamlApplication>
    ```

4. No mesmo projeto, abra o arquivo MyUserControl.xaml e adicione a declaração de namespace a seguir ao elemento `<UserControl>`.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. No mesmo arquivo, adicione um elemento `<winui:RatingControl />` como um filho do `<StackPanel>` e salve suas alterações. Esse elemento adiciona uma instância da classe [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol da biblioteca WinUI. Depois de adicionar esse elemento, o `<StackPanel>` agora deve ficar com a aparência a seguir.

    ```xml
    <StackPanel HorizontalAlignment="Center" Spacing="10" 
                Padding="20" VerticalAlignment="Center">
        <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                       Text="Hello from XAML Islands" FontSize="30" />
        <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                       Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
        <Button HorizontalAlignment="Center" 
                x:Name="Button" Click="ClickHandler">Click Me</Button>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Compile a solução e confirme se ela foi compilada com êxito.

## <a name="test-the-app"></a>Testar o aplicativo

Execute a solução e confirme se **MyDesktopWin32App** é aberto com a janela a seguir.

![Aplicativo MyDesktopWin32App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Próximas etapas

Muitos aplicativos da área de trabalho que hospedam as Ilhas XAML precisarão lidar com cenários adicionais para proporcionar uma experiência do usuário estável. Por exemplo, os aplicativos da área de trabalho podem precisar processar a entrada do teclado nas Ilhas XAML, focar a navegação entre as Ilhas XAML e outros elementos de interface do usuário, bem como alterações de layout.

Para obter mais informações sobre como lidar com esses cenários e ponteiros para exemplos de código relacionados, confira [Cenários avançados das Ilhas XAML em aplicativos C++ Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles UWP XAML em aplicativos da área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Como usar a API de Hospedagem UWP XAML em um aplicativo C++ Win32](using-the-xaml-hosting-api.md)
* [Hospedar um controle UWP padrão em um aplicativo C++ Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Cenários avançados das Ilhas XAML em aplicativos C++ Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemplos de código de Ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
