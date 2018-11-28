---
title: Usar um banco de dados do SQLite em um aplicativo UWP
description: Usar um banco de dados do SQLite em um aplicativo UWP.
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, SQLite, banco de dados
ms.localizationpriority: medium
ms.openlocfilehash: 1588dfbfb1c33b246caba0816c584135f2094f35
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7840032"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>Usar um banco de dados do SQLite em um aplicativo UWP
Você pode usar o SQLite para armazenar e recuperar dados em um banco de dados leve nos dispositivos dos usuários. Esse guia mostra como fazer isso.

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>Alguns benefícios do uso do SQLite para armazenamento local

:heavy_check_mark: SQLite é leve e autossuficiente. É uma biblioteca de código sem nenhuma outra dependência. Não há nada a ser configurado.

:heavy_check_mark: Não há servidor de banco de dados. O cliente e o servidor são executados no mesmo processo.

:heavy_check_mark: SQLite é de domínio público, então você pode usá-lo livremente e distribuí-lo com seu aplicativo.

:heavy_check_mark: SQLite funciona em várias arquiteturas e plataformas.

Você pode ler mais sobre o SQLite [aqui](https://sqlite.org/about.html).

## <a name="choose-an-abstraction-layer"></a>Escolha uma camada de abstração

Recomendamos que você use o Entity Framework Core ou a [biblioteca SQLite](https://github.com/aspnet/Microsoft.Data.Sqlite/), de código aberto, criado pela Microsoft.

### <a name="entity-framework-core"></a>Entity Framework Core

O Entity Framework (EF) é um mapeador relacional de objeto que você pode usar para trabalhar com dados relacionais usando objetos específicos de domínio. Se você já usou essa estrutura para trabalhar com dados em outros aplicativos .NET, você pode migrar esse código para um aplicativo UWP e ele funcionará com as alterações apropriadas feitas na sequência de conexão.

Para experimentar, consulte [Introdução ao EF Core na Plataforma Universal do Windows (UWP) com um novo banco de dados](https://docs.microsoft.com/ef/core/get-started/uwp/getting-started).

### <a name="sqlite-library"></a>Biblioteca SQLite

A biblioteca [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) implementa as interfaces no namespace [System.Data.Common](https://msdn.microsoft.com/library/system.data.common.aspx). A Microsoft mantém ativamente essas implementações, e elas fornecem um wrapper intuitivo em torno da API do SQLite nativo de baixo nível.

O restante deste guia ajuda você a usar essa biblioteca.

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>Configure sua solução para usar a biblioteca Microsoft.Data.SQlite

Vamos começar com um projeto UWP básico, adicionar uma biblioteca de classe e, em seguida, instalar os pacotes Nuget apropriados.

O tipo de biblioteca de classes que você adicionar à sua solução e os pacotes específicos que você instala depende da versão mínima do SDK do Windows a que seu aplicativo se destina. Você pode encontrar essas informações na página Propriedades do seu projeto UWP.

![Versão mínima do SDK do Windows](images/min-version.png)

Use uma das seguintes seções dependendo da versão mínima do SDK do Windows a que seu projeto UWP se destina.

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>A versão mínima do seu projeto não se destina à atualização do Fall Creators

Se você estiver usando o Visual Studio 2015, clique em **Ajuda**->**Sobre o Microsoft Visual Studio**. Em seguida, na lista de programas instalados, certifique-se de que você tenha a versão do gerenciador de pacotes NuGet de **3.5** ou posterior. Se o número de sua versão for menor que isso, instale uma versão posterior do NuGet [aqui](https://www.nuget.org/downloads). Nessa página, você encontrará todas as versões do Nuget listadas abaixo do título **Visual Studio 2015**.

Em seguida, adicione uma biblioteca de classes à sua solução. Você não precisa usar uma biblioteca de classes para conter o código de acesso de dados, mas vamos usar uma em nosso exemplo. Chamaremos a biblioteca de **DataAccessLibrary** e chamaremos a classe na biblioteca de **DataAccess**.

![Biblioteca de classes](images/class-library.png)

Clique com o botão direito do mouse na solução e clique em **Gerenciar pacotes NuGet para solução**.

![Gerenciar pacotes NuGet](images/manage-nuget.png)

Se você estiver usando o Visual Studio 2015, escolha a guia **instalado** e certifique-se de que o número da versão do pacote **Microsoft.NETCore.UniversalWindowsPlatform** seja **5.2.2** ou posterior.

![Versão do .NETCore](images/package-version.png)

Se não for, atualize o pacote para uma versão mais recente.

Escolha a guia **Procurar** e procure o pacote **Microsoft.Data.SQLite**. Instale a versão **1.1.1** (ou inferior) do mesmo pacote.

![Pacote SQLite](images/sqlite-package.png)

Mover para a seção [Adicionar e recuperar dados em um banco de dados SQLite](#use-data) deste guia.

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>A versão mínima do seu projeto se destina à atualização do Fall Creators

Há alguns benefícios em aumentar a versão mínima do seu projeto UWP para a atualização do Fall Creators.

Primeiro, você pode usar as bibliotecas do .NET Standard 2.0 em vez de bibliotecas de classes regulares. Isso significa que você pode compartilhar seu código de acesso de dados com qualquer outro aplicativo com base em .NET, como um aplicativo WPF, Windows Forms, Android, iOS ou ASP.NET.

Em segundo lugar, seu aplicativo não tem bibliotecas do pacote SQLite. Em vez disso, seu aplicativo pode usar a versão do SQLite que vem instalado com o Windows. Isso ajuda você de algumas maneiras.

:heavy_check_mark: Reduz o tamanho do seu aplicativo, pois você não precisa baixar o binário do SQLite e empacotá-lo como parte do seu aplicativo.

:heavy_check_mark: Impede que você precise enviar uma nova versão do seu aplicativo para os usuários no caso de a SQLite publicar correções críticas para bugs e vulnerabilidades de segurança no SQLite. A versão Windows do SQLite é mantida pela Microsoft em coordenação com o SQLite.org.

:heavy_check_mark: O tempo de carregamento do aplicativo tem o potencial de ser mais rápido porque, provavelmente, a versão do SDK do SQLite já será carregada na memória.

Vamos começar adicionando uma biblioteca de classes do .NET Standard 2.0 à sua solução. Não é necessário usar uma biblioteca de classes para conter o código de acesso de dados, mas vamos usar uma em nosso exemplo. Chamaremos a biblioteca de **DataAccessLibrary** e chamaremos a classe na biblioteca de **DataAccess**.

![Biblioteca de classes](images/dot-net-standard.png)

Clique com o botão direito do mouse na solução e clique em **Gerenciar pacotes NuGet para solução**.

![Gerenciar pacotes NuGet](images/manage-nuget-2.png)

Neste ponto, você tem uma opção. Você pode usar a versão do SQLite incluso no Windows ou, se você tiver algum motivo para usar uma versão específica do SQLite, você pode incluir a biblioteca SQLite no pacote.

Vamos começar com como usar a versão do SQLite incluída no Windows.

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>Para usar a versão do SQLite instalada no Windows

Escolha a guia **Procurar** e procure o pacote **Microsoft.Data.SQLite.core** e instale-o.

![Pacote SQLite Core](images/sqlite-core-package.png)

Procure o pacote **SQLitePCLRaw.bundle_winsqlite3** e instale-o somente no projeto UWP em sua solução.

![Pacote SQLite PCL Raw](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>Para incluir o SQLite com seu aplicativo

Você não precisa fazer isso. Mas, se você tiver um motivo para incluir uma versão específica do SQLite com seu aplicativo, escolha a guia **Procurar** e procure o pacote **Microsoft.Data.SQLite**. Instale a versão **2.0** (ou inferior) do mesmo pacote.

![Pacote SQLite](images/sqlite-package-v2.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>Adicionar e recuperar dados em um banco de dados do SQLite

Vamos fazer o seguinte:

:one: Preparar a classe de acesso de dados.

:two: Inicializar o banco de dados SQLite.

:three: Inserir dados no banco de dados do SQLite.

:four: Recuperar dados do banco de dados SQLite.

:five: Adicionar uma interface do usuário básica.

### <a name="prepare-the-data-access-class"></a>Preparar a classe de acesso de dados

No seu projeto UWP, adicione uma referência para o projeto **DataAccessLibrary** em sua solução.

![Biblioteca de classes de acesso de dados](images/ref-class-library.png)

Adicione a seguinte instrução ``using`` aos arquivos **App.xaml.cs** e **MainPage.xaml.cs** em seu projeto UWP.

```csharp
using DataAccessLibrary;
```

Abra a classe **DataAccess** em sua solução **DataAccessLibrary** e faça essa classe ficar estática.

>[!NOTE]
>Embora nosso exemplo coloque o código de acesso de dados em uma classe estática, isso é apenas uma opção de design e é totalmente opcional.

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

Adicione a instrução a seguir na parte superior deste arquivo.

```csharp
using Microsoft.Data.Sqlite;
```

<a id="initialize" />

### <a name="initialize-the-sqlite-database"></a>Inicializar o banco de dados SQLite

Adicione um método para a classe **DataAccess** que inicialize o banco de dados SQLite.

```csharp
public static void InitializeDatabase()
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

Este código cria o banco de dados SQLite e o armazena no armazenamento de dados local do aplicativo.

Neste exemplo, nomeamos o banco de dados ``sqlliteSample.db``, mas você pode usar qualquer nome que desejar, desde que use esse nome em todos os objetos [SqliteConnection](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0) em que criar uma instância.

No construtor do arquivo **App.xaml.cs** do seu projeto UWP, chame o método ``InitializeDatabase`` da classe **DataAccess**.

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```

<a id="insert" />

### <a name="insert-data-into-the-sqlite-database"></a>Inserir dados no banco de dados do SQLite

Adicione um método para a classe **DataAccess** que insira dados no banco de dados SQLite. Este código usa parâmetros na consulta para evitar ataques de injeção do SQL.

```csharp
public static void AddData(string inputText)
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```

<a id="retrieve" />

### <a name="retrieve-data-from-the-sqlite-database"></a>Recuperar dados do banco de dados SQLite

Adicione um método que obtenha linhas de dados de um banco de dados SQLite.

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

O método [Leitura](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_Read) avança por linhas de dados retornados. Ele retorna **true** se houver mais linhas restantes; caso contrário, ele retorna **false **.

O método [GetString](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_) retorna o valor da coluna especificada como uma sequência. Ele aceita um valor inteiro que representa o ordinal de coluna baseada em zero dos dados que você deseja. Você pode usar métodos semelhantes, como [GetDataTime](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) e [GetBoolean ](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_). Escolha um método com base em qual tipo de dados a coluna contém.

O parâmetro ordinal não é tão importante neste exemplo porque estamos selecionando todas as entradas em uma única coluna. No entanto, se várias colunas fizerem parte da sua consulta, use o valor ordinal para obter a coluna da qual você deseja puxar dados.

## <a name="add-a-basic-user-interface"></a>Adicionar uma interface do usuário básica

No arquivo **MainPage.xaml** do projeto UWP, adicione o seguinte XAML.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

Essa interface do usuário básica dá ao usuário um ``TextBox`` que eles podem usar para digitar uma sequência que adicionaremos ao banco de dados SQLite. Nós conectaremos o ``Button`` nessa interface do usuário a um manipulador de eventos que irá recuperar dados do banco de dados SQLite e, em seguida, mostrar esses dados no ``ListView``.

No arquivo **MainPage.xaml.cs**, adicione o seguinte manipulador. Este é o método que associamos ao ``Click``evento da ``Button`` interface do usuário.

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

Isso é tudo. Explore o [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) para ver o que mais você pode fazer com o banco de dados SQLite. Confira os links a seguir para saber mais sobre outras maneiras de usar dados em seu aplicativo UWP.

## <a name="next-steps"></a>Próximas etapas

**Conectar seu aplicativo diretamente a um banco de dados do SQL Server**

Consulte [Usar um banco de dados do SQL Server em um aplicativo UWP](sql-server-databases.md).

**Compartilhar código entre aplicativos diferentes em diferentes plataformas**

Consulte [Compartilhar código entre o desktop e o UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Adicionar páginas de detalhes mestre com back-ends Azure SQL**

Consulte [Exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database).
