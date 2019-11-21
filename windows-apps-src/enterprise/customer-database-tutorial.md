---
title: Criar um aplicativo de banco de dados de clientes
description: Crie um aplicativo de banco de dados do cliente e saiba como implementar funções básicas do aplicativo empresarial.
keywords: Enterprise, tutorial, cliente, dados, criar leitura atualizar excluir, REST, autenticação
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258613"
---
# <a name="tutorial-create-a-customer-database-application"></a>Tutorial: Criar um aplicativo de banco de dados de clientes

Este tutorial cria um aplicativo simples para gerenciar uma lista de clientes. Ao fazer isso, ele introduz uma seleção de conceitos básicos para aplicativos empresariais na UWP. Você aprenderá a:

* Implemente operações de criação, leitura, atualização e exclusão em um banco de dados SQL local.
* Adicione uma grade de dados para exibir e editar dados do cliente em sua interface do usuário.
* Organize elementos da interface do usuário em um layout de formulário básico.

O ponto de partida para este tutorial é um aplicativo de página única com interface do usuário e funcionalidade mínimas, com base em uma versão simplificada do [aplicativo de exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Ele é escrito em C# e XAML, e estamos esperando que você tenha uma familiaridade básica com ambas as linguagens.

![A página principal do aplicativo de trabalho](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Pré-requisitos

* [Verifique se você tem a versão mais recente do Visual Studio e do SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Clonar ou baixar o exemplo de tutorial de banco de dados do cliente](https://github.com/microsoft/windows-tutorials-customer-database)

Depois de clonar/baixar o repositório, você pode editar o projeto abrindo **CustomerDatabaseTutorial. sln** com o Visual Studio.

> [!NOTE]
> Confira o [exemplo completo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver o aplicativo no qual este tutorial se baseou.

## <a name="part-1-code-of-interest"></a>Parte 1: código de interesse

Se você executar seu aplicativo imediatamente após abri-lo, verá alguns botões na parte superior de uma tela em branco. Embora não esteja visível para você, o aplicativo já inclui um banco de dados SQLite local provisionado com alguns clientes de teste. A partir daqui, você começará implementando um controle de interface do usuário para exibir esses clientes e, em seguida, passará para a adição de operações no banco de dados. Antes de começar, aqui está onde você trabalhará.

### <a name="views"></a>Modos de exibição

**CustomerListPage. XAML** é a exibição do aplicativo, que define a interface do usuário para a página única neste tutorial. Sempre que você precisar adicionar ou alterar um elemento visual na interface do usuário, você o fará nesse arquivo. Este tutorial orientará você pela adição desses elementos:

* Um **RadDataGrid** para exibir e editar seus clientes. 
* Um **StackPanel** para definir os valores iniciais para um novo cliente.

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.cs** é onde a lógica fundamental do aplicativo está localizada. Cada ação do usuário realizada na exibição será passada para esse arquivo para processamento. Neste tutorial, você adicionará um novo código e implementará os seguintes métodos:

* **CreateNewCustomerAsync**, que inicializa um novo objeto CustomerViewModel.
* **DeleteNewCustomerAsync**, que remove um novo cliente antes que ele seja exibido na interface do usuário.
* **DeleteAndUpdateAsync**, que manipula a lógica do botão de exclusão.
* **GetCustomerListAsync**, que recupera uma lista de clientes do banco de dados.
* **SaveInitialChangesAsync**, que adiciona as informações do novo cliente ao banco de dados.
* **UpdateCustomersAsync**, que atualiza a interface do usuário para refletir todos os clientes adicionados ou excluídos.

**CustomerViewModel** é um wrapper para as informações de um cliente, que controla se ele foi modificado recentemente ou não. Você não precisará adicionar nada a essa classe, mas parte do código que você adicionará em outro lugar fará referência a ela.

Para obter mais informações sobre como o exemplo é construído, confira a [visão geral da estrutura do aplicativo](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>Parte 2: Adicionar o DataGrid

Antes de começar a operar nos dados do cliente, você precisará adicionar um controle de interface do usuário para exibir esses clientes. Para fazer isso, usaremos um controle **RadDataGrid** de terceiros previamente criado. O pacote NuGet **Telerik. UI. for. UniversalWindowsPlatform** já foi incluído neste projeto. Vamos adicionar a grade ao nosso projeto.

1. Abra **Views\CustomerListPage.XAML** no Gerenciador de soluções. Adicione a seguinte linha de código dentro da marca **Page** para declarar um mapeamento para o namespace Telerik que contém a grade de dados.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Abaixo do **CommandBar** no **RelativePanel** principal da exibição, adicione um controle **RadDataGrid** , com algumas opções básicas de configuração:

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. Você adicionou a grade de dados, mas precisa de dados para exibir. Adicione as seguintes linhas de código a ela:

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Agora que você definiu uma fonte de dados a ser exibida, o **RadDataGrid** tratará a maior parte da lógica da interface do usuário para você. No entanto, se você executar o projeto, ainda não verá nenhum dado na exibição. Isso ocorre porque o ViewModel ainda não está carregando.

![Aplicativo em branco, sem clientes](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Parte 3: ler clientes

Quando inicializado, **ViewModels\CustomerListPageViewModel.cs** chama o método **GetCustomerListAsync** . Esse método precisa recuperar os dados do cliente de teste do banco de dados SQLite que está incluído no tutorial.

1. Em **ViewModels\CustomerListPageViewModel.cs**, atualize seu método **GetCustomerListAsync** com este código:

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    O método **GetCustomerListAsync** é chamado quando o ViewModel é carregado, mas antes dessa etapa, ele não fez nada. Aqui, adicionamos uma chamada para o método **getasync** no **Repository/SqlCustomerRepository**. Isso permite que ele entre em contato com o repositório para recuperar uma coleção enumerável de objetos Customer. Em seguida, ele os analisa em objetos individuais, antes de adicioná-los a sua **ObservableCollection** interna para que possam ser exibidos e editados.

2. Executar seu aplicativo-agora você verá a grade de dados exibindo a lista de clientes.

![Lista inicial de clientes](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Parte 4: editar clientes

Você pode editar as entradas na grade de dados clicando duas vezes nelas, mas precisa garantir que todas as alterações feitas na interface do usuário também sejam feitas na sua coleção de clientes no code-behind. Isso significa que você precisará implementar a ligação de dados bidirecional. Se você quiser obter mais informações sobre isso, Confira nossa [introdução à vinculação de dados](../get-started/display-customers-in-list-learning-track.md).

1. Primeiro, declare que **ViewModels\CustomerListPageViewModel.cs** implementa a interface **INotifyPropertyChanged** :

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Em seguida, no corpo principal da classe, adicione o seguinte evento e método:

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    O método **OnPropertyChanged** torna mais fácil para seus setters gerarem o evento **PropertyChanged** , que é necessário para a ligação de dados bidirecional.

3. Atualize o setter para **SelectedCustomer** com esta chamada de função:

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. No **Views\CustomerListPage.XAML**, adicione a propriedade **SelectedCustomer** à sua grade de dados.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Isso associa a seleção do usuário na grade de dados com o objeto Customer correspondente no code-behind. O modo de associação TwoWay permite que as alterações feitas na interface do usuário sejam refletidas nesse objeto.

5. Execute seu aplicativo. Agora você pode ver os clientes exibidos na grade e fazer alterações nos dados subjacentes por meio da interface do usuário.

![Editando um cliente na grade de dados](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Parte 5: atualizar os clientes

Agora que você pode ver e editar seus clientes, você precisará poder enviar suas alterações para o banco de dados e efetuar pull de todas as atualizações que foram feitas por outras pessoas.

1. Volte para **ViewModels\CustomerListPageViewModel.cs**e navegue para o método **UpdateCustomersAsync** . Atualize-o com esse código para enviar por push as alterações ao banco de dados e recuperar as novas informações:

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    Esse código utiliza a propriedade **IsModified** de **ViewModels\CustomerViewModel.cs**, que é atualizada automaticamente sempre que o cliente é alterado. Isso permite que você evite chamadas desnecessárias e envie por push as alterações de clientes atualizados para o banco de dados.

## <a name="part-6-create-a-new-customer"></a>Parte 6: criar um novo cliente

Adicionar um novo cliente apresenta um desafio, pois o cliente aparecerá como uma linha em branco se você adicioná-lo à interface do usuário antes de fornecer valores para suas propriedades. Isso não é um problema, mas aqui vamos tornar mais fácil definir os valores iniciais do cliente. Neste tutorial, vamos adicionar um painel recolhível simples, mas se você tivesse mais informações para adicionar, poderia criar uma página separada para essa finalidade.

### <a name="update-the-code-behind"></a>Atualizar o code-behind

1. Adicione um novo campo privado e uma propriedade pública a **ViewModels\CustomerListPageViewModel.cs**. Isso será usado para controlar se o painel está visível ou não.

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. Adicione uma nova propriedade pública ao ViewModel, um inverso do valor de **AddingNewCustomer**. Isso será usado para desabilitar os botões de barra de comandos regulares quando o painel estiver visível.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Agora, você precisará de uma maneira de exibir o painel recolhível e criar um cliente para editar nele. 

3. Adicione uma nova propriedade pública e Fiend privada ao ViewModel para manter o cliente recém-criado.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Atualize seu método **CreateNewCustomerAsync** para criar um novo cliente, adicione-o ao repositório e defina-o como o cliente selecionado:

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Atualize o método **SaveInitialChangesAsync** para adicionar um cliente recém-criado ao repositório, atualize a interface do usuário e feche o painel.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Adicione a seguinte linha de código como a linha final no setter para **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Isso garantirá que o **EnableCommandBar** seja atualizado automaticamente sempre que **AddingNewCustomer** for alterado.

### <a name="update-the-ui"></a>Atualizar a interface do usuário

1. Navegue de volta para **Views\CustomerListPage.XAML**e adicione um **StackPanel** com as seguintes propriedades entre o **CommandBar** e sua grade de dados:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    O atributo **x:Load** garante que esse painel seja exibido somente quando você estiver adicionando um novo cliente.

2. Faça a seguinte alteração na posição da sua grade de dados, para garantir que ela se mova para baixo quando o novo painel for exibido:

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Atualize seu painel de pilha com quatro controles **TextBox** . Eles serão vinculados às propriedades individuais do novo cliente e permitem que você edite seus valores antes de adicioná-lo à grade de dados.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. Adicione um botão simples ao novo painel de pilha para salvar o cliente criado recentemente:

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Atualize o **CommandBar**, para que os botões criar, excluir e atualizar regulares sejam desabilitados quando o painel de pilha estiver visível:

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Execute seu aplicativo. Agora você pode criar um cliente e inserir seus dados no painel pilha.

![Criando um novo cliente](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Parte 7: excluir um cliente

A exclusão de um cliente é a operação básica final que você precisa implementar. Ao excluir um cliente que você selecionou na grade de dados, você desejará chamar o **UpdateCustomersAsync** imediatamente para atualizar a interface do usuário. No entanto, você não precisa chamar esse método se estiver excluindo um cliente que acabou de criar.

1. Navegue até **ViewModels\CustomerListPageViewModel.cs**e atualize o método **DeleteAndUpdateAsync** :

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. No **Views\CustomerListPage.XAML**, atualize o painel de pilha para adicionar um novo cliente para que ele contenha um segundo botão:

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. No **ViewModels\CustomerListPageViewModel.cs**, atualize o método **DeleteNewCustomerAsync** para excluir o novo cliente:

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. Execute seu aplicativo. Agora você pode excluir clientes, seja dentro da grade de dados ou no painel pilha.

![Excluir um novo cliente](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Conclusão

Parabéns! Com tudo isso feito, seu aplicativo agora tem uma gama completa de operações de banco de dados local. Você pode criar, ler, atualizar e excluir clientes em sua interface do usuário, e essas alterações são salvas em seu banco de dados e persistirão em diferentes lançamentos do seu aplicativo.

Agora que você terminou, considere o seguinte:
* Se você ainda não fez isso, confira a [visão geral da estrutura do aplicativo](../enterprise/customer-database-app-structure.md) para saber mais sobre por que o aplicativo foi criado como está.
* Explore o [exemplo completo do banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver o aplicativo no qual este tutorial se baseou.

Ou, se você for um desafio, poderá continuar em diante...

## <a name="going-further-connect-to-a-remote-database"></a>Indo além: conectar-se a um banco de dados remoto

Fornecemos uma explicação passo a passo de como implementar essas chamadas em um banco de dados SQLite local. Mas e se você quiser usar um banco de dados remoto, em vez disso?

Se você quiser experimentar, precisará de sua própria conta Azure Active Directory (AAD) e a capacidade de hospedar sua própria fonte de dados.

Você precisará adicionar autenticação, funções para lidar com chamadas REST e, em seguida, criar um banco de dados remoto com o qual interagir. Há código no exemplo completo de [banco de dados Orders do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) que você pode referenciar para cada operação necessária.

### <a name="settings-and-configuration"></a>Configurações e configuração

As etapas necessárias para se conectar ao seu próprio banco de dados remoto são escritas no [Leiame do exemplo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Você precisará fazer o seguinte:

* Forneça a ID do cliente da conta do Azure para [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Forneça a URL do banco de dados remoto para [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Forneça a cadeia de conexão do banco de dados para [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Associe seu aplicativo ao Microsoft Store.
* Copie sobre o [projeto de serviço](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) em seu aplicativo e implante-o no Azure.

### <a name="authentication"></a>Authentication

Você precisará criar um botão para iniciar uma sequência de autenticação e um pop-up ou uma página separada para coletar informações de um usuário. Depois de criar isso, você precisará fornecer um código que solicite as informações de um usuário e o use para adquirir um token de acesso. O exemplo de banco de dados Orders do cliente encapsula chamadas de Microsoft Graph com a biblioteca **accountmanager** para adquirir um token e manipular a autenticação para uma conta do AAD.

* A lógica de autenticação é implementada em [**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* O processo de autenticação é exibido no controle [**AuthenticationControl. XAML**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) personalizado.

### <a name="rest-calls"></a>Chamadas REST

Você não precisará modificar nenhum código que adicionamos neste tutorial para implementar chamadas REST. Em vez disso, você precisará fazer o seguinte:

* Crie novas implementações das interfaces **ICustomerRepository** e **ITutorialRepository** , implementando o mesmo conjunto de funções por meio do REST em vez do SQLite. Você precisará serializar e desserializar o JSON e pode encapsular suas chamadas REST em uma classe **HttpHelper** separada, se necessário. Consulte [a amostra completa](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) para obter informações específicas.
* No **app.XAML.cs**, crie uma nova função para inicializar o repositório REST e chame-o em vez de **SqliteDatabase** quando o aplicativo for inicializado. Novamente, consulte [o exemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Depois que todas as três etapas forem concluídas, você poderá se autenticar em sua conta do AAD por meio de seu aplicativo. As chamadas REST para o banco de dados remoto substituirão as chamadas do SQLite local, mas a experiência do usuário deverá ser a mesma. Se você estiver se sentindo ainda mais ambicioso, poderá adicionar uma página de configurações para permitir que o usuário alterne dinamicamente entre os dois.
