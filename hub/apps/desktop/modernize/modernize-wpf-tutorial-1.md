---
description: Esse tutorial demonstra como adicionar interfaces de usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo WPF.
title: Migrar o aplicativo Contoso Expenses para o .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317102"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Parte 1: Migrar o aplicativo Contoso Expenses para o .NET Core 3

Esta é a primeira parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho WPF de exemplo chamado Despesas da Contoso (ContosoExpenses). Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo, confira [Tutorial: modernizar um aplicativo WPF](modernize-wpf-tutorial.md).
  
Nesta parte do tutorial, você migrará todo o aplicativo de Despesas da Contoso do .NET Framework 4.7.2 para [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Antes de começar, certifique-se de [abrir e criar o exemplo de ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) no Visual Studio 2019.

> [!NOTE]
> Para obter mais informações sobre como migrar um aplicativo WPF do .NET Framework para o .NET Core 3, confira [esta série de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrar o projeto ContosoExpenses para o .NET Core 3

Nesta seção, você migrará o projeto ContosoExpenses no aplicativo de despesas da Contoso para o .NET Core 3. Para isso, criará um arquivo de projeto que contenha os mesmos arquivos do projeto ContosoExpenses existente, mas que tenha como destino o .NET Core 3, em vez do .NET Framework 4.7.2. Isso permite que você mantenha uma única solução com as duas versões do aplicativo, .NET Framework e .NET Core.

1. Verifique se o projeto ContosoExpenses atualmente tem como destino o .NET Framework 4.7.2. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **ContosoExpenses**, escolha **Propriedades** e confirme se a propriedade **Estrutura de destino** na guia **Aplicativo** está definida como o .NET Framework 4.7.2.

    ![.NET Framework versão 4.7.2 para o projeto](images/wpf-modernize-tutorial/NETFramework472.png)

3. No Windows Explorer, abra a pasta **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** e crie um arquivo de texto chamado **ContosoExpenses.Core.csproj**.

4. Clique com o botão direito do mouse no arquivo, escolha **Abrir com** e abra-o em um editor de texto de sua escolha, como Bloco de Notas, Visual Studio Code ou Visual Studio.

5. Copie o texto a seguir, cole-o no arquivo e salve-o.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Feche o arquivo e retorne para a solução **ContosoExpenses** no Visual Studio.

7. Clique com o botão direito do mouse na solução **ContosoExpenses** e escolha **Adicionar > Projeto Existente**. Selecione o arquivo **ContosoExpenses.Core.csproj** que você acabou de criar na pasta `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` para adicioná-lo à solução.

O **ContosoExpenses.Core.csproj** inclui os seguintes elementos:

* O elemento **Project** especifica uma versão do SDK de **Microsoft.NET.Sdk.WindowsDesktop**. Isso se refere a aplicativos .NET da Área de Trabalho do Windows e inclui componentes para aplicativos WPF e Windows Forms.
* O elemento **PropertyGroup** contém elementos filho que indicam que a saída do projeto é um executável (não uma DLL), tem como destino o .NET Core 3 e usa WPF. Para um aplicativo Windows Forms, você usaria um elemento **UseWinForms**, em vez do elemento **UseWPF**.

> [!NOTE]
> Ao trabalhar com o formato .csproj, introduzido com o .NET Core 3.0, todos os arquivos na mesma pasta que o .csproj serão considerados parte do projeto. Portanto, você não precisa especificar todos os arquivos incluídos no projeto. Você precisa especificar somente os arquivos para os quais deseja definir uma ação de compilação personalizada ou que deseja excluir.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrar o projeto ContosoExpenses.Data para .NET Standard

A solução **ContosoExpenses** inclui uma biblioteca de classes do **ContosoExpenses.Data** que contém modelos e interfaces para serviços e destinos .NET 4.7.2. Os aplicativos .NET Core 3.0 podem usar bibliotecas do .NET Framework, desde que não usem APIs não disponíveis no .NET Core. No entanto, o melhor caminho de modernização é mover suas bibliotecas para .NET Standard. Isso garantirá que a biblioteca tenha suporte total do aplicativo .NET Core 3.0. Além disso, você pode reutilizar a biblioteca também com outras plataformas, como Web (por meio do ASP.NET Core) e móvel (por meio do Xamarin).

Para migrar o projeto **ContosoExpenses.Data** para o .NET Standard:

1. No Visual Studio, clique com o botão direito do mouse no projeto **ContosoExpenses.Data** e escolha **Descarregar Projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses.Data.csproj**.

2. Exclua todo o conteúdo do arquivo do projeto.

3. Copie e cole o XML a seguir e salve o arquivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Clique com o botão direito do mouse no projeto **ContosoExpenses.Data** e escolha **Recarregar projeto**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurar os pacotes e dependências do NuGet

Quando você migrou os projetos **ContosoExpenses.Core** e **ContosoExpenses.Data** nas seções anteriores, removeu as referências do pacote NuGet dos projetos. Nesta seção, você voltará a adicionar essas referências.

Para configurar pacotes NuGet para o projeto **ContosoExpenses.Data**:

1. No projeto **ContosoExpenses.Data**, expanda o nó de **Dependências**. Observe que a seção **NuGet** está ausente.

    ![Pacotes NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Se você abrir **Packages.config** no **Gerenciador de Soluções**, encontrará as referências "antigas" dos pacotes NuGet que usaram o projeto quando ele estava utilizando o .NET Framework completo.

    ![Dependências e pacotes](images/wpf-modernize-tutorial/Packages.png)

    Confira abaixo o conteúdo do arquivo **Packages.config**. Você notará que todos os pacotes NuGet têm como destino o .NET Framework 4.7.2 completo:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. No projeto **ContosoExpenses.Data**, exclua o arquivo **Packages.config**.

4. No projeto **ContosoExpenses.Data**, clique com o botão direito do mouse no nó **Dependências** e escolha **Gerenciar Pacotes NuGet**.

  ![Gerenciar Pacotes NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Na janela **Gerenciador de Pacotes NuGet**, clique em **Procurar**. Pesquise o pacote `Bogus` e instale a versão estável mais recente.

    ![Pacote NuGet Bogus](images/wpf-modernize-tutorial/Bogus.png)

6. Pesquise o pacote `LiteDB` e instale a versão estável mais recente.

    ![Pacote NuGet LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Você pode estar imaginando onde essa lista de pacotes NuGet está armazenada, já que o projeto não tem mais um arquivo packages.config. Os pacotes NuGet referenciados são armazenados diretamente no arquivo.csproj. Para verificar isso, exiba o conteúdo do arquivo do projeto **ContosoExpenses.Data.csproj** em um editor de texto. Você encontrará as seguintes linhas adicionadas ao final do arquivo:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Você também perceberá que está instalando os mesmos pacotes para esse projeto do .NET Core 3 que os usados pelos projetos do .NET Framework 4.7.2. Os pacotes NuGet dão suporte a vários destinos. Os autores da biblioteca podem incluir versões diferentes de uma biblioteca no mesmo pacote, compiladas para diferentes arquiteturas e plataformas. Esses pacotes dão suporte ao .NET Framework completo, bem como .NET Standard 2.0, que é compatível com projetos do .NET Core 3. Para obter mais informações sobre as diferenças do .NET Framework, .NET Core e .NET Standard, confira [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Para configurar pacotes NuGet para o projeto **ContosoExpenses.Core**:

1. No projeto **ContosoExpenses.Core**, abra o arquivo **packages.config**. Atualmente, ele contém as referências a seguir direcionadas ao .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    Nas etapas a seguir, você encontrará as versões .NET Standard dos pacotes `MvvmLightLibs` e `Unity`. As outras duas são dependências baixadas automaticamente pelo NuGet quando você instala essas duas bibliotecas.

2. No projeto **ContosoExpenses.Core**, exclua o arquivo **Packages.config**.

3. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Gerenciar Pacotes NuGet**.

4. Na janela **Gerenciador de Pacotes NuGet**, clique em **Procurar**. Pesquise o pacote `Unity` e instale a versão estável mais recente.

    ![Pacote Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Pesquise o pacote `MvvmLightLibsStd10` e instale a versão estável mais recente. Esta é a versão do .NET Standard do pacote `MvvmLightLibs`. No caso desse pacote, o autor optou por criar um pacote da versão do .NET Standard da biblioteca em um pacote separado da versão do .NET Framework.

    ![Pacote MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. No projeto **ContosoExpenses.Core**, clique com o botão direito do mouse no nó **Dependências** e escolha **Adicionar Referência**.

7. Na categoria **Projetos > Solução**, selecione **ContosoExpenses.Data** e clique em **OK**.

    ![Adicionar Referência](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Desabilitar atributos de assembly gerados automaticamente

Neste ponto do processo de migração, se você tentar criar o projeto **ContosoExpenses.Core**, verá alguns erros.

![Novos erros de build do .NET Core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Esse problema está acontecendo porque o novo formato .csproj, introduzido no .NET Core 3.0, armazena as informações do assembly no arquivo do projeto, e não no arquivo **AssemblyInfo.cs**. Para corrigir esses erros, desabilite esse comportamento e deixe o projeto continuar usando o arquivo **AssemblyInfo.cs**.

1. No Visual Studio, clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Descarregar Projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses.Core.csproj**.

1. Adicione o elemento a seguir na seção **PropertyGroup** e salve o arquivo.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Depois de adicionar esse elemento, a seção **PropertyGroup** deverá ser exibida assim:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Recarregar Projeto**.

4. Clique com o botão direito do mouse no projeto **ContosoExpenses.Data** e escolha **Descarregar Projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses.Data.csproj**.

5. Adicione a mesma entrada na seção **PropertyGroup** e salve o arquivo.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Depois de adicionar esse elemento, a seção **PropertyGroup** deverá ser exibida assim:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Clique com o botão direito do mouse no projeto **ContosoExpenses.Data** e escolha **Recarregar Projeto**.

## <a name="add-the-windows-compatibility-pack"></a>Adicionar o pacote de compatibilidade do Windows

Se você tentar compilar agora os projetos **ContosoExpenses.Core** e **ContosoExpenses.Data**, verá que os erros anteriores estão corrigidos, mas ainda são exibidos alguns erros na biblioteca de **ContosoExpenses.Data**  semelhante aos outros.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Esses erros resultam da conversão do projeto **ContosoExpenses.Data** de uma biblioteca .NET Framework (específica para Windows) para uma biblioteca .NET Standard, que pode ser executada em várias plataformas, incluindo Linux, Android, iOS, entre outras. O projeto **ContosoExpenses.Data** contém uma classe chamada **RegistryService**, que interage com o registro, um conceito somente para Windows.

Para resolver esses erros, instale o pacote NuGet de [Compatibilidade do Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility). Esse pacote dá suporte a muitas APIs específicas do Windows que devem ser usadas em uma biblioteca .NET Standard. Após a adoção desse pacote, a biblioteca deixará de ter uma plataforma cruzada, mas ainda terá como destino o .NET Standard. 

1. Clique com o botão direito do mouse no projeto **ContosoExpenses.Data**.
2. Escolha **Gerenciar Pacotes NuGet**.
3. Na janela **Gerenciador de Pacotes NuGet**, clique em **Procurar**. Pesquise o pacote `Microsoft.Windows.Compatibility` e instale a versão estável mais recente.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Agora, tente novamente compilar o projeto, clicando com o botão direito do mouse no projeto **ContosoExpenses.Data** e escolhendo **Build**.

Desta vez, o processo de build será concluído sem erros.

## <a name="test-and-debug-the-migration"></a>Testar e depurar a migração

Agora que os projetos estão sendo compilados com sucesso, você está pronto para executar e testar o aplicativo para ver se há algum erro de runtime.

1. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Definir como Projeto de Inicialização**.

2. Pressione F5 para inicializar o projeto **ContosoExpenses.Core** no depurador. Você verá uma exceção semelhante à seguinte.

    ![Exceção exibida no Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Essa exceção está sendo gerada porque, quando você excluiu o conteúdo do arquivo.csproj no início da migração, removeu as informações sobre a **Ação de Build** dos arquivos de imagem. Siga as etapas abaixo para corrigir esse problema.

3. Pare o depurador.

4. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Descarregar Projeto**. Clique com o botão direito do mouse no projeto novamente e escolha **Editar ContosoExpenses.Core.csproj**.

5. Antes de fechar o elemento **Project**, adicione a seguinte entrada:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Recarregar Projeto**.

7. Para atribuir o Contoso.ico ao aplicativo, clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Propriedades**. Na página aberta, clique no menu suspenso em **Ícone** e selecione `Images\contoso.ico`.

    ![Ícone Contoso nas propriedades do projeto](images/wpf-modernize-tutorial/ContosoIco.png)

8. Clique em **Salvar**.

9. Pressione F5 para inicializar o projeto **ContosoExpenses.Core** no depurador. Confirme se o aplicativo agora é executado.

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você migrou com sucesso o aplicativo de Despesas da Contoso para o .NET Core 3. Agora, você está pronto para a [Parte 2: Adicionar um controle InkCanvas da UWP usando Ilhas XAML](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Se você tiver uma tela de alta resolução, poderá notar que o aplicativo parece muito pequeno. Você abordará esse problema na próxima etapa do tutorial.
