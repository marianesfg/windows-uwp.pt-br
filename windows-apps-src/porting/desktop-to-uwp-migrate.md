---
Description: Compartilhar código entre um aplicativo de área de trabalho e um aplicativo UWP
title: Compartilhar código entre um aplicativo de área de trabalho e um aplicativo UWP
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c07a3bbff4b29d2b59ef7d6d8a5912ce3675a4e
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730350"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>Mover de um aplicativo de área de trabalho para UWP

Se você tiver um aplicativo de área de trabalho existente criado usando o .NET Framework (incluindo WPF e Windows Forms) ou APIs Win32 do C++, terá várias opções para mudar para o Plataforma Universal do Windows (UWP) e para o Windows 10.

## <a name="package-your-desktop-application-in-an-msix-package"></a>Empacotar seu aplicativo de desktop em um pacote MSIX

Você pode empacotar seu aplicativo de área de trabalho em um pacote MSIX para obter acesso a muitos outros recursos do Windows 10. MSIX é um formato de pacote do aplicativo do Windows moderno, que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, incluindo aplicativos UWP, WPF, Windows Forms e Win32. Empacotar seus aplicativos da área de trabalho do Windows em pacotes MSIX fornece a você acesso a uma experiência robusta de instalação e atualização, um modelo de segurança gerenciado com um sistema de capacidade flexível, suporte à Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizados. Você pode empacotar seu aplicativo se tiver o código-fonte ou se tiver apenas um arquivo do instalador existente (como um MSI ou um instalador do App-V). Depois de empacotar seu aplicativo, você pode integrar recursos de UWP, como extensões de pacote e outros componentes UWP.

Para obter mais informações, consulte [empacotar aplicativos de área de trabalho (ponte de desktop)](/windows/msix/desktop/desktop-to-uwp-root) e [recursos que exigem a identidade do pacote](/windows/apps/desktop/modernize/modernize-packaged-apps).

## <a name="use-windows-runtime-apis"></a>Usar Windows Runtime APIs

É possível chamar várias APIs do Windows Runtime diretamente no seu aplicativo da área de trabalho Win32 do WPF, do Windows Forms ou do C++ para integrar experiências modernas interessantes para usuários do Windows 10. Por exemplo, você pode chamar APIs do Windows Runtime para adicionar notificações do sistema ao seu aplicativo da área de trabalho.

Para saber mais, confira [Usar APIs do Windows Runtime em aplicativos de área de trabalho](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>Migrar um aplicativo .NET Framework para um aplicativo UWP

Se seu aplicativo for executado no .NET Framework, você poderá migrá-lo para um aplicativo UWP aproveitando .NET Standard 2,0. Mova o máximo de código que você pode em .NET Standard bibliotecas de classe 2,0 e, em seguida, crie um aplicativo UWP que referencie suas bibliotecas de .NET Standard 2,0. 

### <a name="share-code-in-a-net-standard-20-library"></a>Compartilhar código em uma biblioteca .NET 2.0 padrão

Se o seu aplicativo for executado no .NET Framework, coloque o mesmo código que você pode em .NET Standard bibliotecas de classes 2,0. Contanto que seu código use APIs que são definidos no padrão, você pode reutilizá-los em um aplicativo UWP. É mais fácil do que nunca foi compartilhar código em uma biblioteca .NET padrão porque muitas APIs mais estão incluídas no .NET padrão 2.0.

Aqui está um vídeo que explica mais sobre isso.

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>Adicionar bibliotecas .NET Standard

Primeiro, adicione um ou mais bibliotecas de classe .NET Standard à sua solução.  

![Adicionar um projeto padrão dotnet](images/desktop-to-uwp/dot-net-standard-project-template.png)

O número de bibliotecas que você adicionar à sua solução depende de como você planeja organizar seu código.

Certifique-se de que cada biblioteca de classe do **.NET Standard 2.0** tenha um destino.

![Target .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

Você pode encontrar essa configuração nas páginas de propriedades do projeto de biblioteca de classe.

No seu projeto de aplicativo da área de trabalho, adicione uma referência para o projeto de biblioteca de classes.

![Referência da biblioteca de classe](images/desktop-to-uwp/class-library-reference.png)

Em seguida, use ferramentas para determinar quanto do seu código está em conformidade com o padrão. Dessa forma, antes de você mover o código para a biblioteca, você pode decidir quais partes você pode reutilizar, quais partes exigem modificação mínima, e quais partes permanecerão específicas do aplicativo.

#### <a name="check-library-and-code-compatibility"></a>Verificar a compatibilidade de biblioteca e código

Vamos começar com pacotes Nuget e outros arquivos dll que você obteve de terceiros.

Se seu aplicativo usa qualquer um deles, determine se eles são compatíveis com o .NET Standard 2.0. Você pode usar uma extensão do Visual Studio ou um utilitário de linha de comando para fazer isso.

Use essas ferramentas para analisar seu código. Baixe as ferramentas aqui ([dotnet apiport](https://github.com/Microsoft/dotnet-apiport/releases)) e, em seguida, assista a este vídeo para saber como usá-las.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

Se seu código não for compatível com o padrão, considere a possibilidade de outras maneiras de poder implementar esse código. Comece abrindo o [Navegador .NET API](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0). Você pode usar esse navegador para examinar a API que está disponível no .NET Standard 2.0. Certifique-se de definir o escopo de lista para o .NET Standard 2.0.

![opção de ponto de internet](images/desktop-to-uwp/dot-net-option.png)

Parte de seu código será específica de plataforma e precisará permanecer em seu projeto de aplicativo da área de trabalho.

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Exemplo: Migrando o código de acesso de dados a uma biblioteca .NET Standard 2.0

Vamos supor que tenhamos um aplicativo de Windows Forms muito básico que mostra os clientes de nosso banco de dados de exemplo Northwind.

![Aplicativo de formulário do Windows](images/desktop-to-uwp/win-forms-app.png)

O projeto contém uma biblioteca de classes do .NET Standard 2.0 com uma classe estática chamada **Northwind**. Se movermos esse código para a classe **Northwind** , ele não compilará porque ele usa ``SQLConnection``as ``SqlCommand``classes, ``SqlDataReader`` , e, e as classes que não estão disponíveis no .net Standard 2,0.

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

### <a name="create-a-uwp-app"></a>Criar um aplicativo UWP

Agora você está pronto para adicionar um aplicativo UWP à sua solução.

![desktop para imagem de ponte UWP](images/desktop-to-uwp/adaptive-ui.png)

Você ainda terá de projetar páginas de interface do usuário em XAML e gravar qualquer dispositivo ou o código específico de plataforma, mas quando tiver terminado, você poderá acessar toda a experiência do dispositivos Windows 10 e as páginas do aplicativo terão uma aparência moderna que se adapta bem a resoluções e tamanhos de tela diferentes.

Seu aplicativo responderá a mecanismos de entrada que não seja apenas um teclado e mouse, e recursos e configurações serão intuitivas em todos os dispositivos. Isso significa que os usuários aprendem a fazer as coisas uma vez e, em seguida, ele funciona de maneira bastante familiar, independentemente do dispositivo.

Essas são apenas algumas das novidades que vêm com UWP. Para saber mais, consulte [Criar ótimas experiências com o Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

#### <a name="add-a-uwp-project"></a>Adicionar um projeto UWP

Primeiro, adicione um projeto UWP à sua solução.

![Projeto UWP](images/desktop-to-uwp/new-uwp-app.png)

Em seguida, no seu projeto UWP, adicione a referência do projeto de biblioteca .NET Standard 2.0.

![Referência da biblioteca de classe](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>Compilar suas páginas

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

### <a name="reach-ios-and-android-devices"></a>Acessar dispositivos iOS e Android

Você pode entrar em dispositivos Android e iOS adicionando projetos Xamarin.  

![Aplicativos Xamarin](images/desktop-to-uwp/xamarin-apps.png)

Esses projetos permitem que você use C# para criar aplicativos Android e iOS com total acesso às APIs específicas de plataforma e específicas do dispositivo. Esses aplicativos aproveitam a aceleração de hardware específica de plataforma e são compiladas para desempenho nativo.

Eles têm acesso a toda gama de funcionalidade exposta pela plataforma e dispositivo, incluindo funcionalidades específicas de plataforma como iBeacons e fragmentos Android subjacentes, e você usará controles de interface do usuário nativo padrão para criar interfaces do usuário que fiquem do jeito que os usuários esperam que sejam.

Assim como UWPs, o custo para adicionar um aplicativo iOS ou Android é menor porque você pode reutilizar a lógica de negócios em uma biblioteca de classe .NET Standard 2.0. Você terá de projetar suas páginas da interface do usuário em XAML e escrever qualquer dispositivo ou o código específico de plataforma.

#### <a name="add-a-xamarin-project"></a>Adicionar um projeto Xamarin

Primeiro, adicione um projeto **Android**, **iOS**, ou **Plataforma cruzada** à sua solução.

Você pode encontrar esses modelos na caixa de diálogo **Adicionar novo projeto** no grupo **Visual C#**.

![Aplicativos Xamarin](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Projetos de plataforma cruzada são ótimos para aplicativos com pouca funcionalidade específica de plataforma. Você pode usá-los para criar uma IU baseada em XAML nativa que roda em iOS, Android e Windows. Saiba mais [aqui](https://docs.microsoft.com/xamarin/xamarin-forms/).

Em seguida, do Android, iOS ou do projeto de plataforma cruzada, adicione uma referência do projeto de biblioteca de classe.

![Referência da biblioteca de classe](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>Compilar suas páginas

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

Para começar com Android, iOS e projetos de plataforma cruzada, consulte o [Portal do desenvolvedor Xamarin](https://docs.microsoft.com/xamarin).

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Confira [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
