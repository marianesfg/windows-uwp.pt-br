---
description: Este tutorial demonstra como adicionar interfaces de usuário do UWP XAML, criar pacotes do MSIX e incorporar outros componentes modernos ao seu aplicativo do WPF.
title: Migrar o aplicativo Contoso Expenses para o .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317102"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Parte 1: Migrar o aplicativo Contoso Expenses para o .NET Core 3

Esta é a primeira parte de um tutorial que demonstra como modernizar um aplicativo de área de trabalho WPF de exemplo chamado contoso despesas. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo [, consulte o tutorial: Modernizar um aplicativo](modernize-wpf-tutorial.md)do WPF.
  
Nesta parte do tutorial, você migrará todo o aplicativo de despesas da Contoso da .NET Framework 4.7.2 para o [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Antes de iniciar esta parte do tutorial, [abra e crie o exemplo ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) no Visual Studio 2019.

> [!NOTE]
> Para obter mais informações sobre como migrar um aplicativo WPF do .NET Framework para o .NET Core 3, consulte [esta série de Blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrar o projeto ContosoExpenses para o .NET Core 3

Nesta seção, você migrará o projeto ContosoExpenses no aplicativo de despesas da Contoso para o .NET Core 3. Você fará isso criando um novo arquivo de projeto que contém os mesmos arquivos que o projeto ContosoExpenses existente, mas que tem como alvo o .NET Core 3 em vez do .NET Framework 4.7.2. Isso permite que você mantenha uma única solução com as versões .NET Framework e .NET Core do aplicativo.

1. Verifique se o projeto ContosoExpenses atualmente tem como alvo o .NET Framework 4.7.2. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **ContosoExpenses** , escolha **Propriedades**e confirme se a propriedade **estrutura de destino** na guia **aplicativo** está definida como o .NET Framework 4.7.2.

    ![.NET Framework versão 4.7.2 para o projeto](images/wpf-modernize-tutorial/NETFramework472.png)

3. No Windows Explorer, navegue até a pasta **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** e crie um novo arquivo de texto chamado **ContosoExpenses. Core. csproj**.

4. Clique com o botão direito do mouse no arquivo, escolha **abrir com**e abra-o em um editor de texto de sua escolha, como o bloco de notas, Visual Studio Code ou Visual Studio.

5. Copie o texto a seguir para o arquivo e salve-o.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Feche o arquivo e retorne à solução **ContosoExpenses** no Visual Studio.

7. Clique com o botão direito do mouse na solução **ContosoExpenses** e escolha **Adicionar-> projeto existente**. Selecione o arquivo **ContosoExpenses. Core. csproj** que você acabou de criar `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` na pasta para adicioná-lo à solução.

O **ContosoExpenses. Core. csproj** inclui os seguintes elementos:

* O elemento **Project** especifica uma versão do SDK do **Microsoft. net. Sdk. WindowsDesktop**. Isso se refere a aplicativos .NET para Windows desktop e inclui componentes para WPF e Windows Forms aplicativos.
* O elemento **PropertyGroup** contém elementos filho que indicam que a saída do projeto é um executável (não uma dll), tem como alvo o .NET Core 3 e usa o WPF. Para um aplicativo Windows Forms, você usaria um elemento **UseWinForms** em vez do elemento **UseWPF** .

> [!NOTE]
> Ao trabalhar com o formato. csproj introduzido com o .NET Core 3,0, todos os arquivos na mesma pasta que o. csproj são considerados parte do projeto. Portanto, você não precisa especificar todos os arquivos incluídos no projeto. Você deve especificar somente os arquivos para os quais deseja definir uma ação de compilação personalizada ou que deseja excluir.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrar o projeto ContosoExpenses. Data para .NET Standard

A solução **ContosoExpenses** inclui uma biblioteca de classes **ContosoExpenses. Data** que contém modelos e interfaces para serviços e destinos .NET 4.7.2. Os aplicativos .NET Core 3,0 podem usar .NET Framework bibliotecas, desde que não usem APIs que não estejam disponíveis no .NET Core. No entanto, o melhor caminho de modernização é mover suas bibliotecas para .NET Standard. Isso garantirá que sua biblioteca seja totalmente suportada pelo seu aplicativo .NET Core 3,0. Além disso, você pode reutilizar a biblioteca também com outras plataformas, como Web (por meio de ASP.NET Core) e móvel (por meio do Xamarin).

Para migrar o projeto **ContosoExpenses. Data** para .net Standard:

1. No Visual Studio, clique com o botão direito do mouse no projeto **ContosoExpenses. Data** e escolha **descarregar projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses. Data. csproj**.

2. Exclua todo o conteúdo do arquivo de projeto.

3. Copie e cole o XML a seguir e salve o arquivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Clique com o botão direito do mouse no projeto **ContosoExpenses. Data** e escolha **recarregar projeto**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurar os pacotes e dependências do NuGet

Ao migrar os projetos **ContosoExpenses. Core** e **ContosoExpenses. Data** nas seções anteriores, você removeu as referências de pacote NuGet dos projetos. Nesta seção, você adicionará essas referências de volta.

Para configurar os pacotes NuGet para o projeto **ContosoExpenses. Data** :

1. No projeto **ContosoExpenses. Data** , expanda o nó **dependências** . Observe que a seção **NuGet** está ausente.

    ![Pacotes NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Se você abrir o **Packages. config** no **Gerenciador de soluções** , encontrará as referências ' antigas ' dos pacotes NuGet usados no projeto quando ele estava usando o .NET Framework completo.

    ![Dependências e pacotes](images/wpf-modernize-tutorial/Packages.png)

    Aqui está o conteúdo do arquivo **Packages. config** . Você observará que todos os pacotes NuGet visam o total de .NET Framework 4.7.2:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. No projeto **ContosoExpenses. Data** , exclua o arquivo **Packages. config** .

4. No projeto **ContosoExpenses. Data** , clique com o botão direito do mouse no nó **dependências** e escolha **gerenciar pacotes NuGet**.

  ![Gerenciar pacotes NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Na janela **Gerenciador de pacotes NuGet** , clique em **procurar**. Pesquise o `Bogus` pacote e instale a versão estável mais recente.

    ![Pacote NuGet falso](images/wpf-modernize-tutorial/Bogus.png)

6. Pesquise o `LiteDB` pacote e instale a versão estável mais recente.

    ![Pacote NuGet do LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Você pode estar imaginando onde essa lista de pacotes NuGet está armazenada, já que o projeto não tem mais um arquivo Packages. config. Os pacotes NuGet referenciados são armazenados diretamente no arquivo. csproj. Você pode verificar isso exibindo o conteúdo do arquivo de projeto **ContosoExpenses. Data. csproj** em um editor de texto. Você encontrará as seguintes linhas adicionadas ao final do arquivo:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Você também pode observar que está instalando os mesmos pacotes para este projeto do .NET Core 3 como aqueles usados por .NET Framework projetos do 4.7.2. Os pacotes NuGet dão suporte a vários destinos. Os autores de biblioteca podem incluir versões diferentes de uma biblioteca no mesmo pacote, compiladas para diferentes arquiteturas e plataformas. Esses pacotes dão suporte ao .NET Framework completo, bem como .NET Standard 2,0, que é compatível com projetos do .NET Core 3. Para obter mais informações sobre as diferenças .NET Framework, .NET Core e .NET Standard, consulte [.net Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Para configurar os pacotes NuGet para o projeto **ContosoExpenses. Core** :

1. No projeto **ContosoExpenses. Core** , abra o arquivo **Packages. config** . Observe que ele atualmente contém as seguintes referências que visam o .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    Nas etapas a seguir, você .net Standardá as versões `MvvmLightLibs` dos `Unity` pacotes e. Os outros dois são dependências automaticamente baixadas pelo NuGet quando você instala essas duas bibliotecas.

2. No projeto **ContosoExpenses. Core** , exclua o arquivo **Packages. config** .

3. Clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **gerenciar pacotes NuGet**.

4. Na janela **Gerenciador de pacotes NuGet** , clique em **procurar**. Pesquise o `Unity` pacote e instale a versão estável mais recente.

    ![Pacote do Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Pesquise o `MvvmLightLibsStd10` pacote e instale a versão estável mais recente. Esta é a versão .net standard do `MvvmLightLibs` pacote. Para este pacote, o autor optou por empacotar a versão .NET Standard da biblioteca em um pacote separado do que a versão .NET Framework.

    ![Pacote MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. No projeto **ContosoExpenses. Core** , clique com o botão direito do mouse no nó **dependências** e escolha **Adicionar referência**.

7. Na categoria **projetos > solução** , selecione **ContosoExpenses. Data** e clique em **OK**.

    ![Adicionar Referência](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Desabilitar atributos de assembly gerados automaticamente

Neste ponto do processo de migração, se você tentar criar o projeto **ContosoExpenses. Core** , verá alguns erros.

![Novos erros de compilação do .NET Core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Esse problema está ocorrendo porque o novo formato. csproj introduzido com o .NET Core 3,0 armazena as informações do assembly no arquivo de projeto em vez do arquivo **AssemblyInfo.cs** . Para corrigir esses erros, desabilite esse comportamento e deixe que o projeto continue a usar o arquivo **AssemblyInfo.cs** .

1. No Visual Studio, clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **descarregar projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses. Core. csproj**.

1. Adicione o seguinte elemento na seção **PropertyGroup** e salve o arquivo.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Depois de adicionar esse elemento, a seção **PropertyGroup** agora deve ser assim:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **recarregar projeto**.

4. Clique com o botão direito do mouse no projeto **ContosoExpenses. Data** e escolha **descarregar projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses. Data. csproj**.

5. Adicione a mesma entrada na seção **PropertyGroup** e salve o arquivo.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Depois de adicionar esse elemento, a seção **PropertyGroup** agora deve ser assim:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Clique com o botão direito do mouse no projeto **ContosoExpenses. Data** e escolha **recarregar projeto**.

## <a name="add-the-windows-compatibility-pack"></a>Adicionar o pacote de compatibilidade do Windows

Se agora você tentar compilar os projetos **ContosoExpenses. Core** e **ContosoExpenses. Data** , verá que os erros anteriores agora são corrigidos, mas ainda há alguns erros na biblioteca **ContosoExpenses. Data** semelhante a esses.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Esses erros são resultado da conversão do projeto **ContosoExpenses. Data** de uma biblioteca .NET Framework (que é específica para o Windows) em uma biblioteca .net Standard, que pode ser executada em várias plataformas, incluindo Linux, Android, Ios e muito mais. O projeto **ContosoExpenses. Data** contém uma classe chamada **RegistryService**, que interage com o registro, um conceito somente do Windows.

Para resolver esses erros, instale o pacote NuGet de [compatibilidade do Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) . Este pacote fornece suporte para várias APIs específicas do Windows a serem usadas em uma biblioteca de .NET Standard. A biblioteca não será mais plataforma cruzada depois de usar esse pacote, mas ainda será direcionada .NET Standard. 

1. Clique com o botão direito do mouse no projeto **ContosoExpenses. Data** .
2. Escolha **gerenciar pacotes NuGet**.
3. Na janela **Gerenciador de pacotes NuGet** , clique em **procurar**. Pesquise o `Microsoft.Windows.Compatibility` pacote e instale a versão estável mais recente.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Agora, tente novamente compilar o projeto clicando com o botão direito do mouse no projeto **ContosoExpenses. Data** e escolhendo **Compilar**.

Desta vez, o processo de compilação será concluído sem erros.

## <a name="test-and-debug-the-migration"></a>Testar e depurar a migração

Agora que os projetos estão sendo compiladas com êxito, você está pronto para executar e testar o aplicativo para ver se há erros de tempo de execução.

1. Clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **definir como projeto de inicialização**.

2. Pressione F5 para iniciar o projeto **ContosoExpenses. Core** no depurador. Você verá uma exceção semelhante à seguinte.

    ![Exceção exibida no Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Essa exceção está sendo gerada porque, quando você excluiu o conteúdo do arquivo. csproj no início da migração, você removeu as informações sobre a **ação de compilação** dos arquivos de imagem. As etapas a seguir corrigem esse problema.

3. Pare o depurador.

4. Clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **descarregar projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses. Core. csproj**.

5. Antes do elemento do **projeto** de fechamento, adicione a seguinte entrada:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **recarregar projeto**.

7. Para atribuir o contoso. ico ao aplicativo, clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **Propriedades**. Na página aberta, clique na lista suspensa em **ícone** e selecione `Images\contoso.ico`.

    ![Ícone da Contoso nas propriedades do projeto](images/wpf-modernize-tutorial/ContosoIco.png)

8. Clique em **Salvar**.

9. Pressione F5 para iniciar o projeto **ContosoExpenses. Core** no depurador. Confirme se o aplicativo agora é executado.

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você migrou com êxito o aplicativo contoso despesas para o .NET Core 3. Agora você está pronto para [a parte 2: Adicione um controle InkCanvas de UWP usando ilhas](modernize-wpf-tutorial-2.md)XAML.

> [!NOTE]
> Se você tiver uma tela de alta resolução, poderá notar que o aplicativo parece muito pequeno. Você resolverá esse problema na próxima etapa do tutorial.
