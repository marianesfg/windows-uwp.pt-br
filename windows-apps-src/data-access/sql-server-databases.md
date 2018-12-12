---
title: Usar um banco de dados do SQL Server em um aplicativo UWP
description: Usar um banco de dados do SQL Server em um aplicativo UWP.
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, SQL Server, banco de dados
ms.localizationpriority: medium
ms.openlocfilehash: 5cb4b16cea3368660ffcb1bc4b252391a73ab13e
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8940807"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>Usar um banco de dados do SQL Server em um aplicativo UWP
Seu aplicativo pode se conectar diretamente a um banco de dados do SQL Server e, em seguida, armazenar e recuperar dados usando classes no namespace [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx).

Neste guia, mostraremos uma maneira de fazer isso. Se você instalar o banco de dados de exemplo [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) em sua instância do SQL Server e então usar esses trechos de código, você acabará com uma interface do usuário básica que mostra os produtos do banco de dados Northwind.

![Produtos Northwind](images/products-northwind.png)

Os trechos de código que aparecem neste guia se baseiam neste [exemplo mais completo](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo).

## <a name="first-set-up-your-solution"></a>Primeiro, configure sua solução

Para conectar seu aplicativo diretamente a um banco de dados do SQL Server, certifique-se de que a versão mínima do seu projeto tenha como alvo a atualização do Fall Creators.  Você pode encontrar essas informações na página Propriedades do seu projeto UWP.

![Versão mínima do SDK do Windows](images/min-version-fall-creators.png)

Abra o arquivo **Package.appxmanifest** do seu projeto UWP no designer do manifesto.

Na guia **Funcionalidades**, selecione a caixa de seleção **Autenticação de Empresa**.

![Funcionalidade de Autenticação de Empresa](images/enterprise-authentication.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>Adicionar e recuperar dados em um banco de dados do SQL Server

Nesta seção, nós faremos o seguinte:

:one: Adicionar uma sequência de conexão.

:two: Criar uma classe para armazenar dados do produto.

:three: Recuperar produtos do banco de dados do SQL Server.

:four: Adicionar uma interface do usuário básica.

:five: Preencher a interface do usuário com produtos.

>[!NOTE]
> Esta seção ilustra uma maneira de organizar seu código de acesso de dados. Ele deve apenas fornecer um exemplo de como você pode usar o [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) para armazenar e recuperar dados de um banco de dados do SQL Server. Você pode organizar seu código de qualquer maneira que faça mais sentido para o design do seu aplicativo.

### <a name="add-a-connection-string"></a>Adicionar uma sequência de conexão

No arquivo **App.xaml.cs**, adicione uma propriedade para a classe ``App``, que dá acesso a outras classes em sua solução na sequência de conexão.

Nossos pontos de sequência de conexão ao banco de dados Northwind em uma instância do SQL Server Express.

```csharp
sealed partial class App : Application
{
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>Criar uma classe para armazenar dados do produto

Vamos criar uma classe que implementa o evento [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx) para que possamos vincular os atributos em nossa interface do usuário XAML às propriedades nessa classe.

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        if (PropertyChanged != null)
        {
            PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
        }
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>Recuperar produtos do banco de dados do SQL Server

Crie um método que obtenha produtos do banco de dados Northwind e, em seguida, os devolve como uma coleção [ObservableCollection](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) de instâncias ``Product``.

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>Adicionar uma interface do usuário básica

 Adicione o seguinte XAML ao arquivo **MainPage.xaml** do projeto UWP.

 Este XAML cria um [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) para mostrar cada produto retornado no trecho anterior e vincula os atributos de cada linha no [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) às propriedades que definimos na classe ``Product``.

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>Mostrar produtos no ListView

Abra o arquivo **MainPage.xaml.cs** e adicione o código ao construtor da classe ``MainPage`` que define a propriedade do **ItemSource** do [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) para o [ObservableCollection](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) das ``Product``instâncias.

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

Inicie o projeto e veja os produtos do banco de dados Northwind aparecerem na interface do usuário.

![Produtos Northwind](images/products-northwind.png)

Explore o namespace [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) para ver que outras coisas você pode fazer com dados em seu banco de dados do SQL Server.

## <a name="trouble-connecting-to-your-database"></a>Problemas para se conectar ao seu banco de dados?

Na maioria dos casos, alguns aspectos da configuração do SQL Server precisam ser alterados. Se conseguir se conectar ao seu banco de dados de outro tipo de aplicativo da área de trabalho, como um aplicativo Windows Forms ou WPF, certifique-se de ter habilitado o TCP/IP para SQL Server. Você pode fazer isso no console de **Gerenciamento do computador**.

![Gerenciamento do computador](images/computer-management.png)

Em seguida, certifique-se de que o serviço do SQL Server Browser está em execução.

![Serviço do SQL Server Browser](images/sql-browser-service.png)

## <a name="next-steps"></a>Próximas etapas

**Use um banco de dados leve para armazenar dados no dispositivo de usuários**

Consulte [Usar um banco de dados do SQLite em um aplicativo UWP](sqlite-databases.md).

**Compartilhar código entre aplicativos diferentes em diferentes plataformas**

Consulte [Compartilhar código entre o desktop e o UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Adicionar páginas de detalhes mestre com back-ends Azure SQL**

Consulte [Exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database).
