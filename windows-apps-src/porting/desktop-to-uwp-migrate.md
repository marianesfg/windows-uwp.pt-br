---
author: normesta
Description: Share code between a desktop application and a UWP app
Search.Product: eADQiWindows 10XVcnh
title: Compartilhar código entre um aplicativo da área de trabalho e um aplicativo UWP
ms.author: normesta
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a4eda15a18a441da2e06db17aaf7bea3d1842d1
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7442712"
---
# <a name="share-code-between-a-desktop-application-and-a-uwp-app"></a>Compartilhar código entre um aplicativo da área de trabalho e um aplicativo UWP

Você pode mover o código para bibliotecas .NET Standard e, em seguida, criar um aplicativo da Plataforma Universal do Windows (UWP) para alcançar todos os dispositivos Windows 10. Embora não haja nenhuma ferramenta que possa converter um aplicativo da área de trabalho para um aplicativo UWP, você pode reutilizar bastante de seu código existente, o que reduz o custo de criação de um novo. Este guia mostra para você como fazer isso.

## <a name="share-code-in-a-net-standard-20-library"></a>Compartilhar código em uma biblioteca .NET 2.0 padrão

Posicione o tanto de código que você puder nas bibliotecas de classe .NET Standard 2.0.  Contanto que seu código use APIs que são definidos no padrão, você pode reutilizá-los em um aplicativo UWP. É mais fácil do que nunca foi compartilhar código em uma biblioteca .NET padrão porque muitas APIs mais estão incluídas no .NET padrão 2.0.

Veja um vídeo excelente que informa mais sobre isso.
&nbsp;
> [!VIDEO https://www.youtube.com/embed/YI4MurjfMn8]

### <a name="add-net-standard-libraries"></a>Adicionar bibliotecas .NET Standard

Primeiro, adicione um ou mais bibliotecas de classe .NET Standard à sua solução.  

![Adicionar um projeto padrão dotnet](images/desktop-to-uwp/dot-net-standard-project-template.png)

O número de bibliotecas que você adicionar à sua solução depende de como você planeja organizar seu código.

Certifique-se de que cada biblioteca de classe do **.NET Standard 2.0** tenha um destino.

![Target .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

Você pode encontrar essa configuração nas páginas de propriedades do projeto de biblioteca de classe.

No seu projeto de aplicativo da área de trabalho, adicione uma referência para o projeto de biblioteca de classes.

![Referência da biblioteca de classe](images/desktop-to-uwp/class-library-reference.png)

Em seguida, use ferramentas para determinar quanto do seu código está em conformidade com o padrão. Dessa forma, antes de você mover o código para a biblioteca, você pode decidir quais partes você pode reutilizar, quais partes exigem modificação mínima, e quais partes permanecerão específicas do aplicativo.

### <a name="check-library-and-code-compatibility"></a>Verificar a compatibilidade de biblioteca e código

Vamos começar com pacotes Nuget e outros arquivos dll que você obteve de terceiros.

Se seu aplicativo usa qualquer um deles, determine se eles são compatíveis com o .NET Standard 2.0. Você pode usar uma extensão do Visual Studio ou um utilitário de linha de comando para fazer isso.

Use essas ferramentas para analisar seu código. Baixe as ferramentas aqui ([dotnet apiport](https://github.com/Microsoft/dotnet-apiport/releases)) e, em seguida, assista a este vídeo para saber como usá-las.
&nbsp;
> [!VIDEO https://www.youtube.com/embed/rzs_FGPyAlY]

Se seu código não for compatível com o padrão, considere a possibilidade de outras maneiras de poder implementar esse código. Comece abrindo o [Navegador .NET API](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0). Você pode usar esse navegador para examinar a API que está disponível no .NET Standard 2.0. Certifique-se de definir o escopo de lista para o .NET Standard 2.0.

![opção de ponto de internet](images/desktop-to-uwp/dot-net-option.png)

Parte de seu código será específica de plataforma e precisará permanecer em seu projeto de aplicativo da área de trabalho.

### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Exemplo: Migrando o código de acesso de dados a uma biblioteca .NET Standard 2.0

Vamos supor que temos um aplicativo Windows Forms muito básico que mostra os clientes de nosso banco de dados Northwind.

![Aplicativo de formulário do Windows](images/desktop-to-uwp/win-forms-app.png)

O projeto contém uma biblioteca de classes do .NET Standard 2.0 com uma classe estática chamada **Northwind**. Se nós movermos esse código na classe **Northwind**, ele não será compilado porque usa as classes ``SQLConnection``, ``SqlCommand`` e ``SqlDataReader``, e essas classes que não estão disponíveis no .NET Standard 2.0.

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
É possível usar o [Navegador de API .NET](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0) para encontrar uma alternativa. Use as classes ``DbConnection``, ``DbCommand`` e ``DbDataReader`` disponíveis no .NET Standard 2.0.  

Essa versão revisada usa essas classes para obter uma lista de clientes, mas para criar uma classe ``DbConnection``, será necessário passar um objeto de fábrica que criamos no aplicativo cliente.

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

Na página de code-behind do formulário Windows, podemos apenas criar a instância de fábrica e passá-la para nosso método.

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

## <a name="reach-all-windows-devices"></a>Alcance de todos os dispositivos Windows

Agora você está pronto para adicionar um aplicativo UWP à sua solução.

![desktop para imagem de ponte UWP](images/desktop-to-uwp/adaptive-ui.png)

Você ainda terá de projetar páginas de interface do usuário em XAML e gravar qualquer dispositivo ou o código específico de plataforma, mas quando tiver terminado, você poderá acessar toda a experiência do dispositivos Windows 10 e as páginas do aplicativo terão uma aparência moderna que se adapta bem a resoluções e tamanhos de tela diferentes.

Seu aplicativo responderá a mecanismos de entrada que não seja apenas um teclado e mouse, e recursos e configurações serão intuitivas em todos os dispositivos. Isso significa que os usuários aprendem a fazer as coisas uma vez e, em seguida, ele funciona de maneira bastante familiar, independentemente do dispositivo.

Essas são apenas algumas das novidades que vêm com UWP. Para saber mais, consulte [Criar ótimas experiências com o Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

### <a name="add-a-uwp-project"></a>Adicionar um projeto UWP

Primeiro, adicione um projeto UWP à sua solução.

![Projeto UWP](images/desktop-to-uwp/new-uwp-app.png)

Em seguida, no seu projeto UWP, adicione a referência do projeto de biblioteca .NET Standard 2.0.

![Referência da biblioteca de classe](images/desktop-to-uwp/class-library-reference2.png)

### <a name="build-your-pages"></a>Compilar suas páginas

Adicionar páginas XAML e chamar o código em sua biblioteca .NET Standard 2.0.

![Aplicativo UWP](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```


Para começar com UWP, consulte [O que é um aplicativo UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

## <a name="reach-ios-and-android-devices"></a>Acessar dispositivos iOS e Android

Você pode entrar em dispositivos Android e iOS adicionando projetos Xamarin.  

![Aplicativos Xamarin](images/desktop-to-uwp/xamarin-apps.png)

Esses projetos permitem que você use C# para criar aplicativos Android e iOS com total acesso às APIs específicas de plataforma e específicas do dispositivo. Esses aplicativos aproveitam a aceleração de hardware específica de plataforma e são compiladas para desempenho nativo.

Eles têm acesso a toda gama de funcionalidade exposta pela plataforma e dispositivo, incluindo funcionalidades específicas de plataforma como iBeacons e fragmentos Android subjacentes, e você usará controles de interface do usuário nativo padrão para criar interfaces do usuário que fiquem do jeito que os usuários esperam que sejam.

Assim como UWPs, o custo para adicionar um aplicativo iOS ou Android é menor porque você pode reutilizar a lógica de negócios em uma biblioteca de classe .NET Standard 2.0. Você terá de projetar suas páginas da interface do usuário em XAML e escrever qualquer dispositivo ou o código específico de plataforma.

### <a name="add-a-xamarin-project"></a>Adicionar um projeto Xamarin

Primeiro, adicione um projeto **Android**, **iOS**, ou **Plataforma cruzada** à sua solução.

Você pode encontrar esses modelos na caixa de diálogo **Adicionar novo projeto** no grupo **Visual C#**.

![Aplicativos Xamarin](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Projetos de plataforma cruzada são ótimos para aplicativos com pouca funcionalidade específica de plataforma. Você pode usá-los para criar uma IU baseada em XAML nativa que roda em iOS, Android e Windows. Saiba mais [aqui](https://www.xamarin.com/forms).

Em seguida, do Android, iOS ou do projeto de plataforma cruzada, adicione uma referência do projeto de biblioteca de classe.

![Referência da biblioteca de classe](images/desktop-to-uwp/class-library-reference3.png)

### <a name="build-your-pages"></a>Compilar suas páginas

Nosso exemplo mostra uma lista de clientes em um aplicativo do Android.

![Aplicativo Android](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Para começar com Android, iOS e projetos de plataforma cruzada, consulte o [Portal do desenvolvedor Xamarin](https://developer.xamarin.com/).

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
