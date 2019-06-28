---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Migrar o Contoso aplicativo de despesas para o .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e718de7a22873ccf347e60c661f724ce3abdd2cf
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420129"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Parte 1: Migrar o Contoso aplicativo de despesas para o .NET Core 3

Isso é a primeira parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso despesas. Para obter uma visão geral do tutorial, os pré-requisitos e instruções para baixar o aplicativo de exemplo, consulte [Tutorial: Modernize um aplicativo WPF](modernize-wpf-tutorial.md).
  
Nesta parte do tutorial, você irá migrar todo o aplicativo de despesas de Contoso do .NET Framework 4.7.2 para [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Antes de iniciar esta parte do tutorial, certifique-se de que você faça a seguir:

* [Abrir e criar o exemplo ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) no Visual Studio de 2019.
* Se você estiver usando uma versão de lançamento do Visual Studio de 2019, habilite as versões de visualização do SDK do .NET Core. No Visual Studio, acesse **Ferramentas > Opções**, digite "Preview" na caixa de pesquisa e selecione **usar visualizações do SDK do .NET Core**. Se você estiver usando um [versão prévia do Visual Studio de 2019](https://visualstudio.microsoft.com/vs/preview/), não é necessário selecionar essa opção, porque versões prévias do .NET Core são habilitadas por padrão.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrar o projeto ContosoExpenses para .NET Core 3

Nesta seção, você vai migrar o projeto ContosoExpenses o aplicativo de despesas da Contoso para o .NET Core 3. Você fará isso criando um novo arquivo de projeto que contém os mesmos arquivos que o projeto existente do ContosoExpenses mas destinos do .NET Core 3 em vez do .NET Framework 4.7.2. Isso permite que você mantenha uma única solução com as versões do .NET Framework e .NET Core do aplicativo.

1. Verifique se que o ContosoExpenses projeto atualmente destinado ao .NET Framework 4.7.2. No Gerenciador de soluções, clique com botão direito a **ContosoExpenses** do projeto, escolha **propriedades**e confirme se o **estrutura de destino** propriedade o  **Aplicativo** for definido como o .NET Framework 4.7.2.

    ![.NET framework versão 4.7.2 para o projeto](images/wpf-modernize-tutorial/NETFramework472.png)

3. No Windows Explorer, navegue até a **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** pasta e crie um novo arquivo de texto chamado **ContosoExpenses.Core.csproj**.

4. Clique com botão direito no arquivo, escolha **abrir com**e, em seguida, abra-o em um editor de texto de sua escolha, como o bloco de notas, o Visual Studio Code ou o Visual Studio.

5. Copie o seguinte texto ao arquivo e salvá-lo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Feche o arquivo e retornar para o **ContosoExpenses** solução no Visual Studio.

7. Clique com botão direito do **ContosoExpenses** solução e escolha **Adicionar -> projeto existente**. Selecione o **ContosoExpenses.Core.csproj** arquivo que você acabou de criar no `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` pasta para adicioná-lo à solução.

O **ContosoExpenses.Core.csproj** inclui os seguintes elementos:

* O **Project** elemento Especifica uma versão do SDK do **Microsoft.NET.Sdk.WindowsDesktop**. Isso se refere a aplicativos .NET para Windows Desktop e inclui componentes para aplicativos WPF e Windows Forms.
* O **PropertyGroup** elemento contém elementos filho que indicam a saída do projeto é um executável (não uma DLL), tem como alvo o .NET Core 3 e usa o WPF. Para um aplicativo Windows Forms, você usaria um **UseWinForms** elemento, em vez da **UseWPF** elemento.

> [!NOTE]
> Ao trabalhar com o formato. csproj introduzido com o .NET Core 3.0, todos os arquivos na mesma pasta. csproj são considerados parte do projeto. Portanto, você não precisa especificar todos os arquivos incluídos no projeto. Você deve especificar somente os arquivos para o qual você deseja definir uma ação de compilação personalizada ou que você deseja excluir.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrar o projeto ContosoExpenses.Data para .NET Standard

O **ContosoExpenses** solução inclui um **ContosoExpenses.Data** biblioteca de classes que contém os modelos e interfaces para serviços e tem como alvo o .NET 4.7.2. Aplicativos .NET core 3.0 podem usar bibliotecas do .NET Framework, desde que eles não usem APIs que não estão disponíveis no .NET Core. No entanto, é o melhor caminho de modernização mover suas bibliotecas para .NET Standard. Isso garantirá que sua biblioteca é totalmente com suporte pelo seu aplicativo .NET Core 3.0. Além disso, você pode reutilizar a biblioteca também com outras plataformas, como web (por meio do ASP.NET Core) e dispositivos móveis (por meio do Xamarin).

Para migrar os **ContosoExpenses.Data** projeto para o .NET Standard:

1. No Visual Studio, clique com botão direito do **ContosoExpenses.Data** do projeto e escolha **descarregar projeto**. Clique com botão direito no projeto novamente e, em seguida, escolha **ContosoExpenses.Data.csproj editar**.

2. Exclua todo o conteúdo do arquivo de projeto.

3. Copie e cole o XML a seguir e salve o arquivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Clique com botão direito do **ContosoExpenses.Data** do projeto e escolha **recarregar projeto**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurar as dependências e pacotes do NuGet

Quando você migrou os **ContosoExpenses.Core** e **ContosoExpenses.Data** projetos nas seções anteriores, você removeu as referências de pacote do NuGet dos projetos. Nesta seção, você adicionará novamente essas referências.

Para configurar pacotes do NuGet para o **ContosoExpenses.Data** projeto:

1.  No **ContosoExpenses.Data** do projeto, expanda o **dependências** nó. Observe que o **NuGet** seção está ausente.

    ![Pacotes do NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Se você abrir o **Packages. config** na **Gerenciador de soluções** você encontrará as referências 'old' dos pacotes NuGet usados no projeto quando ele estava usando o .NET Framework completo.

    ![Pacotes e dependências](images/wpf-modernize-tutorial/Packages.png)

    Aqui está o conteúdo a **Packages. config** arquivo. Você observará que todos os pacotes do NuGet direcionar o .NET Framework 4.7.2 completo:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. No **ContosoExpenses.Data** do projeto, exclua o **Packages. config** arquivo.

4. No **ContosoExpenses.Data** do projeto, clique com botão direito do **dependências** nó e escolha **Manage NuGet Packages**.

  ![Gerencie pacotes NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. No **Gerenciador de pacotes NuGet** janela, clique em **procurar**. Pesquise o `Bogus` do pacote e instalá-lo.

    ![Pacote do NuGet falso](images/wpf-modernize-tutorial/Bogus.png)

6. Pesquise o `LiteDB` do pacote e instalá-lo.

    ![Pacote do LiteDB NuGet](images/wpf-modernize-tutorial/LiteDB.png)

    Você pode estar se perguntando onde esses lista de pacotes do NuGet está armazenada, já que o projeto não tem mais um arquivo Packages. config. Os pacotes NuGet referenciados são armazenados diretamente no arquivo. csproj. Você pode verificar isso exibindo o conteúdo do **ContosoExpenses.Data.csproj** arquivo de projeto em um editor de texto. Você encontrará as seguintes linhas adicionadas ao final do arquivo:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Você também pode observar que você estiver instalando os mesmos pacotes para este projeto .NET Core 3 como aqueles usados por projetos do .NET Framework 4.7.2. Pacotes do NuGet dá suporte a vários destinos. Autores de biblioteca podem incluir versões diferentes de uma biblioteca no mesmo pacote, compilado para plataformas e arquiteturas diferentes. Esses pacotes dão suporte a .NET Framework completo, bem como o .NET Standard 2.0, que é compatível com projetos do .NET Core 3. Para obter mais informações sobre as diferenças do .NET Framework, .NET Core e .NET Standard, consulte [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Para configurar pacotes do NuGet para o **ContosoExpenses.Core** projeto:

1. No **ContosoExpenses.Core** projeto, abra o **Packages. config** arquivo. Observe que, atualmente, ele contém as seguintes referências destinados ao .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    Nas etapas a seguir, você vai versões do .NET Standard do `MvvmLightLibs` e `Unity` pacotes. As outras duas são automaticamente baixadas pelo NuGet quando você instala essas duas bibliotecas de dependências.

2. No **ContosoExpenses.Core** do projeto, exclua o **Packages. config** arquivo.

3. Clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **Manage NuGet Packages**.

4. No **Gerenciador de pacotes NuGet** janela, clique em **procurar**. Pesquise o `Unity` do pacote e instalá-lo.

    ![Pacote do Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Pesquise o `MvvmLightLibsStd10` do pacote e instalá-lo. Essa é a versão do .NET Standard do `MvvmLightLibs` pacote. Para este pacote, o autor escolhido empacotar o .NET Standard versão da biblioteca em um pacote separado que a versão do .NET Framework.

    ! Pacote de MvvmLightsLibs[](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. No **ContosoExpenses.Core** do projeto, clique com botão direito do **dependências** nó e escolha **adicionar referência**.

7. No **projetos > solução** categoria, selecione **ContosoExpenses.Data** e clique em **Okey**.

    ![Adicionar Referência](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Desabilitar os atributos do assembly gerado automaticamente

Nesse momento, no processo de migração, se você tentar compilar a **ContosoExpenses.Core** projeto, você verá alguns erros.

![Novos erros da compilação do .NET core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Esse problema ocorre porque o novo formato. csproj introduzido com o .NET Core 3.0 armazena as informações de assembly no arquivo de projeto em vez de **AssemblyInfo.cs** arquivo. Para corrigir esses erros, desabilitar esse comportamento e permitir que o projeto continuar a usar o **AssemblyInfo.cs** arquivo.

1. No Visual Studio, clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **descarregar projeto**. Clique com botão direito no projeto novamente e, em seguida, escolha **ContosoExpenses.Core.csproj editar**.

1. Adicione o seguinte elemento na **PropertyGroup** seção e salve o arquivo.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Depois de adicionar esse elemento, o **PropertyGroup** seção agora deve ser assim:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **recarregar projeto**.

4. Clique com botão direito do **ContosoExpenses.Data** do projeto e escolha **descarregar projeto**. Clique com botão direito no projeto novamente e, em seguida, escolha **ContosoExpenses.Data.csproj editar**.

5. Adicionar a mesma entrada na **PropertyGroup** seção e salve o arquivo.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Depois de adicionar esse elemento, o **PropertyGroup** seção agora deve ser assim:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Clique com botão direito do **ContosoExpenses.Data** do projeto e escolha **recarregar projeto**.

## <a name="add-the-windows-compatibility-pack"></a>Adicionar o pacote de compatibilidade do Windows

Se você tentar compilar agora o **ContosoExpenses.Core** e **ContosoExpenses.Data** projetos, você verá que agora estão corrigidos os erros anteriores, mas ainda há alguns erros no  **ContosoExpenses.Data** biblioteca semelhante a estas.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Esses erros são o resultado da conversão de **ContosoExpenses.Data** projeto de uma biblioteca do .NET Framework (que é específico para Windows) para uma biblioteca .NET Standard, que pode ser executados em várias plataformas, incluindo Linux, Android, iOS, e muito mais. O **ContosoExpenses.Data** projeto contém uma classe chamada **RegistryService**, que interage com o registro, um conceito somente para Windows.

Para resolver esses erros, instale o [compatibilidade do Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) pacote do NuGet. Esse pacote fornece suporte para várias APIs específicas do Windows a ser usado em uma biblioteca .NET Standard. A biblioteca deixarão de ser multiplataforma depois usando esse pacote, mas ainda será direcionar .NET Standard. 

1. Clique com botão direito no **ContosoExpenses.Data** projeto.
2. Escolher **gerenciar pacotes NuGet**.
3. No **Gerenciador de pacotes NuGet** janela, clique em **procurar**. Pesquise o `Microsoft.Windows.Compatibility` do pacote e instalá-lo. 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Agora, tente novamente compilar o projeto, com o botão direito clicando na **ContosoExpenses.Data** projeto e escolhendo **Build**.

Desta vez o processo de compilação será concluída sem erros.

## <a name="test-and-debug-the-migration"></a>Testar e depurar a migração

Agora que os projetos estão criando com êxito, você está pronto para executar e testar o aplicativo para ver se há quaisquer erros de tempo de execução.

1. Clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **definir como projeto de inicialização**.

2. Pressione F5 para iniciar o **ContosoExpenses.Core** projeto no depurador. Você verá uma exceção similar à seguinte.

    ![Exceção exibida no Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Essa exceção está sendo gerada, porque quando você excluir o conteúdo do arquivo. csproj no início da migração, você removeu as informações sobre o **ação de Build** para os arquivos de imagem. As etapas a seguir corrigir esse problema.

3. Pare o depurador.

4. Clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **descarregar projeto**. Clique com botão direito no projeto novamente e, em seguida, escolha **ContosoExpenses.Core.csproj editar**.

5. Antes do fechamento **projeto** elemento, adicione a seguinte entrada:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **recarregar projeto**.

7. Para atribuir o Contoso.ico para o aplicativo, clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **propriedades**. Na página aberta, clique em lista suspensa, sob **ícone** e selecione `Images\contoso.ico`.

    ![Ícone de Contoso nas propriedades do projeto](images/wpf-modernize-tutorial/ContosoIco.png)

8. Clique em **Salvar**.

9. Pressione F5 para iniciar o **ContosoExpenses.Core** projeto no depurador. Confirme se o aplicativo agora é executado.

## <a name="next-steps"></a>Próximas etapas

Neste ponto no tutorial, você migrou com êxito o aplicativo de despesas da Contoso para 3 do .NET Core. Agora você está pronto para [parte 2: Adicionar um controle InkCanvas UWP usando XAML Ilhas](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Se você tiver uma tela de alta resolução, você pode perceber que o aplicativo se parece muito pequeno. Você vai resolver esse problema na próxima etapa do tutorial.
