---
description: Este artigo demonstra como hospedar um controle UWP personalizado em um C++ aplicativo Win32 usando a API de hospedagem XAML.
title: Hospedar um controle UWP personalizado em um C++ aplicativo Win32 usando a API de hospedagem XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, C++, Win32, Ilhas XAML, controles personalizados, controles de usuário, controles de host
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226310"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Hospedar um controle UWP personalizado em um C++ aplicativo Win32

Este artigo demonstra como usar a [API de hospedagem XAML do UWP](using-the-xaml-hosting-api.md) para hospedar um controle XAML personalizado do UWP em C++ um novo aplicativo Win32. Se você tiver um projeto C++ de aplicativo Win32 existente, poderá adaptar estas etapas e exemplos de código para seu projeto.

Para hospedar um controle XAML do UWP personalizado, você criará os seguintes projetos e componentes como parte deste passo a passos:

* **Projeto de aplicativo da área de trabalho do Windows**. Este projeto implementa um aplicativo C++ de área de trabalho Win32 nativo. Você adicionará código a este projeto que usa a API de hospedagem XAML do UWP para hospedar um controle XAML do UWP personalizado.

* **Projeto de aplicativo UWPC++(/WinRT)** . Este projeto implementa um controle personalizado UWP XAML. Ele também implementa um provedor de metadados raiz para carregar metadados para tipos personalizados UWP XAML no projeto.

## <a name="requirements"></a>{1&gt;{2&gt;Requisitos&lt;2}&lt;1}

* Visual Studio 2019 versão 16.4.3 ou posterior.
* Windows 10, versão 1903 SDK (versão 10.0.18362) ou posterior.
* [/WinRT a extensão do Visual Studio (VSIX) instalada com o Visual Studio. C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Para obter mais informações, consulte [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Criar um projeto de aplicativo de área de trabalho

1. No Visual Studio, crie um novo projeto de **aplicativo da área de trabalho do Windows** chamado **MyDesktopWin32App**. Este modelo de projeto está disponível nos **C++** filtros de projeto do, **Windows**e **Desktop** .

2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução, clique em **redirecionar solução**, selecione o **10.0.18362.0** ou uma versão do SDK posterior e clique em **OK**.

3. Instale o pacote NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para habilitar o suporte para [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) em seu projeto:

    1. Clique com o botão direito do mouse no projeto **MyDesktopWin32App** em **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.
    2. Selecione a guia **procurar** , procure o pacote [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale a versão mais recente deste pacote.

4. Na janela **gerenciar pacotes NuGet** , instale os seguintes pacotes NuGet adicionais:

    * [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (versão v 6.0.0 ou posterior). Esse pacote fornece vários ativos de compilação e tempo de execução que permitem que as ilhas XAML funcionem em seu aplicativo.
    * [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versão v 6.0.0 ou posterior).
    * [Microsoft. VCRTForwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Compile a solução e confirme se ela foi compilada com êxito.

## <a name="create-a-uwp-app-project"></a>Criar um projeto de aplicativo UWP

Em seguida, adicione um projeto de aplicativo **UWP (C++/WinRT)** à sua solução e faça algumas alterações de configuração neste projeto. Mais adiante neste tutorial, você adicionará código a esse projeto para implementar um controle personalizado UWP XAML e definir uma instância da classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo kit de ferramentas da Comunidade do Windows. Seu aplicativo usará essa classe como um provedor de metadados raiz para carregar metadados para tipos personalizados UWP XAML.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e selecione **Adicionar** -> **novo projeto**.

2. Adicione um projeto de **aplicativoC++em branco (/WinRT)** à sua solução. Nomeie o projeto **MyUWPApp** e verifique se a versão de destino e a versão mínima estão definidas como **Windows 10, versão 1903** ou posterior.

3. Instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) no projeto **MyUWPApp** :

    1. Clique com o botão direito do mouse no projeto **MyUWPApp** e escolha **gerenciar pacotes NuGet**.
    2. Selecione a guia **procurar** , procure o pacote [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) e instale o v 6.0.0 ou uma versão posterior deste pacote.

4. Clique com o botão direito do mouse no nó **MyUWPApp** e selecione **Propriedades**. Na página **Propriedades comuns** ->  **C++/WinRT** , defina as propriedades a seguir e clique em **aplicar**.

    * Defina **detalhamento** como **normal**.
    * Defina **otimizado** como **não**.

    Quando terminar, a página Propriedades deverá ter a seguinte aparência.

    ![C++Propriedades do projeto/WinRT](images/xaml-islands/xaml-island-cpp-1.png)

5. Na página **Propriedades de configuração** -> **geral** da janela Propriedades, defina **tipo de configuração** como **biblioteca dinâmica (. dll)** e clique em **OK** para fechar a janela Propriedades.

    ![Propriedades gerais do projeto](images/xaml-islands/xaml-island-cpp-2.png)

6. Adicione um arquivo executável de espaço reservado ao projeto **MyUWPApp** . Esse arquivo executável de espaço reservado é necessário para que o Visual Studio gere os arquivos de projeto necessários e compile o projeto corretamente.

    1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó do projeto **MyUWPApp** e selecione **Adicionar** -> **novo item**.
    2. Na caixa de diálogo **Adicionar novo item** , selecione **utilitário** na página esquerda e, em seguida, selecione **arquivo de texto (. txt)** . Digite o nome **PlaceHolder. exe** e clique em **Adicionar**.
      ![Adicionar arquivo de texto](images/xaml-islands/xaml-island-cpp-3.png)
    3. Em **Gerenciador de soluções**, selecione o arquivo **PlaceHolder. exe** . Na janela **Propriedades** , verifique se a propriedade **conteúdo** está definida como **true**.
    4. Em **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo **Package. Appxmanifest** no projeto **MyUWPApp** , selecione **abrir com**e selecione **editor XML (texto)** e clique em **OK**.
    5. Localize o elemento **&lt;Application&gt;** e altere o atributo **executável** para o valor `placeholder.exe`. Quando terminar, o elemento de **&gt;de aplicativo&lt;** deverá ser semelhante a este.

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

    6. Salve e feche o arquivo **Package. appxmanifest** .

7. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó **MyUWPApp** e selecione **descarregar projeto**.
8. Clique com o botão direito do mouse no nó **MyUWPApp** e selecione **Editar MyUWPApp. vcxproj**.
9. Localize o elemento `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` e substitua-o pelo XML a seguir. Esse XML adiciona várias novas propriedades imediatamente antes do elemento.

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
11. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó **MyUWPApp** e selecione **recarregar projeto**.

## <a name="configure-the-solution"></a>Configurar a solução

Nesta seção, você atualizará a solução que contém os dois projetos para configurar dependências do projeto e as propriedades de compilação necessárias para que os projetos sejam compilados corretamente.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e adicione um novo arquivo XML chamado **Solution. props**.
2. Adicione o seguinte XML ao arquivo de **solução. props** .

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

3. No menu **Exibir** , clique em **Gerenciador de propriedades** (dependendo de sua configuração, isso pode estar em **exibição** -> **outras janelas**).
4. Na janela **Gerenciador de propriedades** , clique com o botão direito do mouse em **MyDesktopWin32App** e selecione **Adicionar folha de propriedades existente**. Navegue até o arquivo **Solution. props** que você acabou de adicionar e clique em **abrir**.
5. Repita a etapa anterior para adicionar o arquivo de **solução. props** ao projeto **MyUWPApp** na janela **Gerenciador de propriedades** .
6. Feche a janela de **Gerenciador de propriedades** .
7. Confirme se as alterações da folha de propriedades foram salvas corretamente. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MyDesktopWin32App** e escolha **Propriedades**. Clique **em Propriedades de configuração** -> **Genneral**e confirme se as propriedades **diretório de saída** e **diretório intermediário** têm os valores que você adicionou ao arquivo **Solution. props** . Você também pode confirmar o mesmo para o projeto **MyUWPApp** .
    ![Propriedades do projeto](images/xaml-islands/xaml-island-cpp-4.png)

8. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução e escolha **dependências do projeto**. No menu suspenso **projetos** , verifique se **MyDesktopWin32App** está selecionado e selecione **MyUWPApp** na lista **depende de** .
    ![dependências do projeto](images/xaml-islands/xaml-island-cpp-5.png)

9. Clique em **OK**.

## <a name="add-code-to-the-uwp-app-project"></a>Adicionar código ao projeto de aplicativo UWP

Agora você está pronto para adicionar código ao projeto **MyUWPApp** para executar estas tarefas:

* Implemente um controle personalizado UWP XAML. Mais adiante neste tutorial, você adicionará o código que hospeda esse controle no projeto **MyDesktopWin32App** .
* Defina um tipo que deriva da classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) no kit de ferramentas da Comunidade do Windows. Essa classe atua como um provedor de metadados raiz para carregar metadados para tipos personalizados UWP XAML.

### <a name="define-a-custom-uwp-xaml-control"></a>Definir um controle personalizado UWP XAML

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **MyUWPApp** e selecione **Adicionar** -> **novo item**. Selecione o **nó C++ Visual** no painel esquerdo, selecione **/WinRTC++(controle de usuário em branco)** , nomeie-o **MyUserControl**e clique em **Adicionar**.
2. No editor XAML, substitua o conteúdo do arquivo **MyUserControl. XAML** pelo XAML a seguir e salve o arquivo.

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

Em seguida, revise a classe de **aplicativo** padrão no projeto **MyUWPApp** para derivar da classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fornecida pelo kit de ferramentas da Comunidade do Windows. Mais adiante neste tutorial, você atualizará o projeto da área de trabalho para criar uma instância dessa classe como um provedor de metadados raiz para carregar metadados para tipos personalizados UWP XAML.

  > [!NOTE]
  > Cada solução que usa ilhas XAML pode conter apenas um projeto que define um objeto `XamlApplication`. Todos os controles personalizados UWP XAML em seu aplicativo compartilham o mesmo objeto `XamlApplication`. 

1. No **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo **MainPage. XAML** no projeto **MyUWPApp** . Clique em **remover** e em **excluir** para excluir este arquivo permamently do projeto.
2. No projeto **MyUWPApp** , expanda arquivo **app. XAML** .
3. Substitua o conteúdo dos arquivos **app. XAML**, **app. cpp**, **app. h**e **app. idl** pelo código a seguir.

    * **App. XAML**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App. idl**:

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

    * **App. h**:

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

    * **App. cpp**:

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

4. Adicione um novo arquivo de cabeçalho ao projeto **MyUWPApp** chamado **app. base. h**.
5. Adicione o código a seguir ao arquivo **app. base. h** , salve o arquivo e feche-o.

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

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Configurar o projeto de desktop para consumir tipos de controle personalizados

Antes que o aplicativo **MyDesktopWin32App** possa hospedar um controle XAML do UWP personalizado em uma ilha XAML, ele deve ser configurado para consumir tipos de controle personalizados do projeto **MyUWPApp** . Há duas maneiras de fazer isso, e você pode escolher uma das opções ao concluir este passo a passos.

### <a name="option-1-package-the-app-using-msix"></a>Opção 1: empacotar o aplicativo usando MSIX

Você pode empacotar o aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia de empacotamento de aplicativo moderna para Windows e é baseado em uma combinação das tecnologias de instalação MSI,. Appx, App-V e ClickOnce.

1. Adicione um novo [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à sua solução. Ao criar o projeto, nomeie-o **MyDesktopWin32Project** e selecione **Windows 10, versão 1903 (10,0; Build 18362)** para a **versão de destino** e a **versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **aplicativos** e escolha **Adicionar referência**. Na lista de projetos, marque a caixa de seleção ao lado do projeto **MyDesktopWin32App** e clique em **OK**.
    ![projeto de referência](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Se você optar por não empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação, os computadores que executam seu aplicativo deverão ter o [tempo de execução C++ Visual](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) instalado.

### <a name="option-2-create-an-application-manifest"></a>Opção 2: criar um manifesto de aplicativo

Você pode adicionar um [manifesto de aplicativo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu aplicativo.

1. Clique com o botão direito do mouse no projeto **MyDesktopWin32App** e selecione **Adicionar** -> **novo item**. 
2. Na caixa de diálogo **Adicionar novo item** , clique em **Web** no painel esquerdo e selecione **arquivo XML (. xml)** . 
3. Nomeie o novo arquivo **app. manifest** e clique em **Adicionar**.
4. Substitua o conteúdo do novo arquivo pelo XML a seguir. Esse XML registra os tipos de controle personalizados no projeto **MyUWPApp** .

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

## <a name="configure-additional-desktop-project-properties"></a>Configurar propriedades de projeto de área de trabalho adicionais

Em seguida, atualize o projeto **MyDesktopWin32App** para definir uma macro para diretórios de inclusão adicionais e configurar propriedades adicionais.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MyDesktopWin32App** e selecione **descarregar projeto**.

2. Clique com o botão direito do mouse em **MyDesktopWin32App (descarregado)** e selecione **Editar MyDesktopWin32App. vcxproj**.

3. Adicione o seguinte XML logo antes da marca de `</Project>` de fechamento no final do arquivo. Em seguida, salve e feche o arquivo.

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

4. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **MyDesktopWin32App (descarregado)** e selecione **recarregar projeto**.

5. Clique com o botão direito do mouse em **MyDesktopWin32App**, selecione **Propriedades**e clique no **C/C++**  nó no painel esquerdo. Confirme se a macro de **diretórios de inclusão adicional** foi definida da alteração de arquivo de projeto que você criou na etapa anterior.
    ![C/C++ configurações do projeto](images/xaml-islands/xaml-island-cpp-7.png)

6. Na caixa de diálogo **páginas de propriedades** , expanda **ferramenta de manifesto** -> **entrada e saída**. Defina a propriedade **reconhecimento de DPI** como **por monitor com reconhecimento de DPI alto**. Se você não definir essa propriedade, poderá encontrar um erro de configuração de manifesto em determinados cenários de DPI alto.
    ![C/C++ configurações do projeto](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Hospedar o controle XAML do UWP personalizado no projeto de desktop

Por fim, você está pronto para adicionar código ao projeto **MyDesktopWin32App** para hospedar o controle XAML do UWP personalizado que você definiu anteriormente no projeto **MyUWPApp** .

1. No projeto **MyDesktopWin32App** , abra o arquivo **Framework. h** e comente a linha de código a seguir. Salve o arquivo quando terminar.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Abra o arquivo **MyDesktopWin32App. h** e substitua o conteúdo desse arquivo pelo código a seguir para fazer referência aos C++arquivos de cabeçalho/WinRT necessários. Salve o arquivo quando terminar.

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

3. Abra o arquivo **MyDesktopWin32App. cpp** e adicione o código a seguir à seção `Global Variables:`.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. No mesmo arquivo, adicione o código a seguir à seção `Forward declarations of functions included in this code module:`.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. No mesmo arquivo, adicione o código a seguir imediatamente após o comentário de `TODO: Place code here.` na função `wWinMain`.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. No mesmo arquivo, substitua a função de `InitInstance` padrão pelo código a seguir.

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

8. No mesmo arquivo, localize a função `WndProc`. Substitua o manipulador de `WM_DESTROY` padrão na instrução switch pelo código a seguir.

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

## <a name="test-the-app"></a>Testar o aplicativo

Execute a solução e confirme se o **MyDesktopWin32App** é aberto com a janela a seguir.

![Aplicativo MyDesktopWin32App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Muitos aplicativos de área de trabalho que hospedam ilhas XAML precisarão lidar com cenários adicionais para proporcionar uma experiência de usuário tranqüila. Por exemplo, os aplicativos da área de trabalho podem precisar manipular a entrada do teclado em ilhas XAML, concentrar a navegação entre as ilhas XAML e outros elementos da interface do usuário e as alterações de layout.

Para obter mais informações sobre como lidar com esses cenários e ponteiros para exemplos de código relacionados, consulte [cenários C++ avançados para ilhas XAML em aplicativos Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Usando a API de hospedagem XAML do UWP C++ em um aplicativo Win32](using-the-xaml-hosting-api.md)
* [Hospedar um controle UWP padrão em um C++ aplicativo Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Cenários avançados para ilhas XAML em C++ aplicativos Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemplos de código de ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
