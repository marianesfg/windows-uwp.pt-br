---
title: Criar um aplicativo de banco de dados do cliente
description: Criar um aplicativo de banco de dados do cliente e saiba como implementar funções de aplicativo empresarial básico.
keywords: cliente de tutorial, Enterprise, dados, criar autenticação de exclusão, REST, atualize, leia
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 9c09e0fb73e42fd8a3d0c70bbb5396be32624387
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623241"
---
# <a name="tutorial-create-a-customer-database-application"></a>Tutorial: Criar um aplicativo de banco de dados do cliente

Este tutorial cria um aplicativo simples para gerenciar uma lista de clientes. Dessa forma, ele apresenta uma seleção de conceitos básicos para aplicativos empresariais na UWP. Você aprenderá a:

* Implementar a criar, ler, atualizar e excluir operações em um banco de dados SQL local.
* Adicione uma grade de dados, para exibir e editar os dados do cliente na sua interface do usuário.
* Organiza os elementos de interface do usuário juntos em um layout de formulário básico.

O ponto de partida para este tutorial é um aplicativo de página única com o mínimo de interface do usuário e funcionalidade, com base em uma versão simplificada do [aplicativo de exemplo do banco de dados de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Ele é gravado no C# e o XAML e estamos esperando que você tem uma familiaridade básica com ambas as essas linguagens.

![A página principal do aplicativo de trabalho](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Pré-requisitos

* [Verifique se que você tem a versão mais recente do Visual Studio e SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Clonar ou baixar o exemplo de Tutorial de banco de dados do cliente](https://aka.ms/customer-database-tutorial)

Depois de clonado/baixar o repositório, você pode editar o projeto abrindo **CustomerDatabaseTutorial.sln** com o Visual Studio.

> [!NOTE]
> Confira a [exemplo completo de banco de dados de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver o aplicativo neste tutorial se baseia.

## <a name="part-1-code-of-interest"></a>Parte 1: Código de interesse

Se você executar seu aplicativo imediatamente após abri-lo, você verá alguns botões na parte superior de uma tela em branco. Embora não seja visível para você, o aplicativo já inclui um banco de dados SQLite local provisionado com alguns clientes de teste. A partir daqui, você vai começar com a implementação de um controle de interface do usuário para exibir os clientes e, em seguida, passar para a adição em operações no banco de dados. Antes de começar, aqui é onde você trabalhará.

### <a name="views"></a>Modos de exibição

**CustomerListPage.xaml** é o modo de exibição do aplicativo, que define a interface do usuário para a página única neste tutorial. Sempre que precisar adicionar ou alterar um elemento visual na interface do usuário, você fará isso neste arquivo. Este tutorial orientará você pela adição desses elementos:

* Um **RadDataGrid** para exibir e editar seus clientes. 
* Um **StackPanel** para definir os valores iniciais para um novo cliente.

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.cs** é onde se encontra a lógica fundamental do aplicativo. Cada ação do usuário executada no modo de exibição será passada para esse arquivo para processamento. Neste tutorial, você adicionará um novo código e implementar os seguintes métodos:

* **CreateNewCustomerAsync**, que inicializa um novo objeto CustomerViewModel.
* **DeleteNewCustomerAsync**, que remove um novo cliente antes de ser exibido na interface do usuário.
* **DeleteAndUpdateAsync**, que manipula a lógica do botão delete.
* **GetCustomerListAsync**, que recupera uma lista de clientes do banco de dados.
* **SaveInitialChangesAsync**, que adiciona as informações do novo cliente para o banco de dados.
* **UpdateCustomersAsync**, que atualiza a interface do usuário para refletir quaisquer clientes adicionados ou excluídos.

**CustomerViewModel** é um wrapper para as informações do cliente, que controla se ou não foi modificado recentemente. Você não precisará adicionar nada para essa classe, mas parte do código, você adicionará em outro lugar fará referência a ele.

Para obter mais informações sobre como o exemplo é construído, confira a [visão geral da estrutura de aplicativo](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>Parte 2: Adicione DataGrid

Antes de começar a operar em dados de clientes, você precisará adicionar um controle de interface do usuário para exibir os clientes. Para fazer isso, usaremos um terceiro pré-criado **RadDataGrid** controle. O **universalwindowsplatform** pacote NuGet já foi incluído neste projeto. Vamos adicionar a grade ao nosso projeto.

1. Abra **Views\CustomerListPage.xaml** do Gerenciador de soluções. Adicione a seguinte linha de código dentro de **página** marca para declarar um mapeamento para o namespace da Telerik que contém a grade de dados.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Abaixo de **CommandBar** dentro do principal **RelativePanel** da exibição, adicione uma **RadDataGrid** controle, com algumas opções de configuração básica:

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

3. Você adicionou a grade de dados, mas ele precisa de dados a serem exibidos. Adicione as seguintes linhas de código a ele:

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Agora que você definiu uma fonte de dados a serem exibidos, **RadDataGrid** irá manipular a maioria da lógica da interface do usuário para você. No entanto, se você executar seu projeto, você ainda não verá todos os dados na exibição. Isso ocorre porque o ViewModel não é carregá-la ainda.

![Aplicativo em branco, com nenhum cliente](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Parte 3: Clientes de leitura

Quando ele é inicializado, **ViewModels\CustomerListPageViewModel.cs** chamadas a **GetCustomerListAsync** método. Que método precisa recuperar o dados do cliente de teste do SQLite banco de dados que está incluído no tutorial.

1. Na **ViewModels\CustomerListPageViewModel.cs**, atualize seu **GetCustomerListAsync** método com este código:

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
    O **GetCustomerListAsync** método é chamado quando o ViewModel é carregado, mas antes desta etapa, ele não faz nada. Aqui, adicionamos uma chamada para o **GetAsync** método na **repositório/SqlCustomerRepository**. Isso permite que ele entre em contato com o repositório para recuperar uma coleção enumerável de objetos Customer. Ele, em seguida, analisa-os em objetos individuais, antes de adicioná-los ao seu interno **ObservableCollection** para que eles podem ser exibidos e editados.

2. Executar seu aplicativo - você agora verá a grade de dados, exibindo a lista de clientes.

![Lista inicial de clientes](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Parte 4: Editar os clientes

Você pode editar as entradas na grade de dados clicando duas vezes neles, mas você precisa garantir que as alterações feitas na interface do usuário também são feitas à sua coleção de clientes no code-behind. Isso significa que você terá que implementar a vinculação de dados bidirecional. Se você quiser saber mais sobre isso, Confira nossos [Introdução à associação de dados](../get-started/display-customers-in-list-learning-track.md).

1. Primeiro, declare que **ViewModels\CustomerListPageViewModel.cs** implementa a **INotifyPropertyChanged** interface:

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Em seguida, dentro do corpo principal da classe, adicione o evento e o método a seguir:

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    O **OnPropertyChanged** método torna mais fácil para seus setters gerar o **PropertyChanged** evento, que é necessário para a associação de dados bidirecional.

3. Atualizar o setter para **SelectedCustomer** com essa chamada de função:

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

4. Na **Views\CustomerListPage.xaml**, adicione o **SelectedCustomer** propriedade para a grade de dados.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Isso associa a seleção do usuário na grade de dados com o objeto de cliente correspondente no code-behind. O modo de ligação de TwoWay permite que as alterações feitas na interface do usuário sejam refletidas no objeto.

5. Execute seu aplicativo. Agora você pode ver os clientes exibidos na grade e fazer alterações nos dados subjacentes por meio de sua interface do usuário.

![Edição de um cliente na grade de dados](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Parte 5: Atualizar clientes

Agora que você pode ver e editar seus clientes, você precisará ser capaz de enviar por push suas alterações no banco de dados e para efetuar pull de todas as atualizações que foram feitas por outras pessoas.

1. Retorne ao **ViewModels\CustomerListPageViewModel.cs**e navegue até a **UpdateCustomersAsync** método. Atualize-o com esse código para enviar alterações por push para o banco de dados e para recuperar quaisquer novas informações:

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
    Esse código utiliza o **IsModified** propriedade de **ViewModels\CustomerViewModel.cs**, que é atualizado automaticamente sempre que o cliente é alterado. Isso permite que você evite chamadas desnecessárias e apenas enviar alterações por push de clientes atualizados no banco de dados.

## <a name="part-6-create-a-new-customer"></a>Parte 6: Criar um novo cliente

Adicionando um novo cliente apresenta um desafio, como o cliente será exibido como uma linha em branco se você adicioná-lo na interface do usuário antes de fornecer valores para suas propriedades. Isso não é um problema, mas aqui podemos torná-lo mais fácil de definir valores iniciais de um cliente. Neste tutorial, vamos adicionar um painel recolhível simple, mas se você tivesse mais informações para adicionar você pode criar uma página separada para essa finalidade.

### <a name="update-the-code-behind"></a>Atualizar o code-behind

1. Adicionar um novo campo particular e a propriedade pública a ser **ViewModels\CustomerListPageViewModel.cs**. Isso será usado para controlar se o painel está visível.

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

2. Adicionar uma nova propriedade pública ao ViewModel, um inverso do valor de **AddingNewCustomer**. Isso será usado para desabilitar os botões da barra de comando regular quando o painel está visível.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Agora, você precisará uma maneira de exibir o painel recolhível e para criar um cliente para editar dentro dele. 

3. Adicione um novo fiend privada e uma propriedade pública ao ViewModel, para manter o cliente recém-criado.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if {_newCustomer != value}
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Atualização de seu **CreateNewCustomerAsync** método para criar um novo cliente, adicioná-lo ao repositório e defini-lo como o cliente selecionado:

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Atualizar o **SaveInitialChangesAsync** método para adicionar um cliente recém-criado no repositório, atualize a interface do usuário e feche o painel.

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

    Isso garantirá que **EnableCommandBar** é atualizada automaticamente sempre que **AddingNewCustomer** é alterado.

### <a name="update-the-ui"></a>Atualizar a interface do usuário

1. Navegue de volta para **Views\CustomerListPage.xaml**e adicione um **StackPanel** com as seguintes propriedades entre seu **CommandBar** e sua grade de dados:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    O **x: carregar** atributo garante que esse painel só aparece quando você estiver adicionando um novo cliente.

2. Faça a seguinte alteração para a posição de grade de dados, para garantir que ele move para baixo quando o novo painel é exibido:

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Atualizar seu painel de pilha com quatro **caixa de texto** controles. Eles associar a propriedades individual do novo cliente e permitem que você edite os valores antes de adicioná-lo para a grade de dados.

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

4. Adicione um botão simples para seu novo painel de pilha para salvar o cliente recém-criado:

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Atualizar o **CommandBar**, de modo que a regular create, delete e botões de atualização são desabilitados quando o painel de pilha está visível:

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Execute seu aplicativo. Agora você pode criar um cliente e seus dados no painel de pilha de entrada.

![Criando um novo cliente](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Parte 7: Excluir um cliente

Excluir um cliente é a operação básica final que você precisa implementar. Quando você exclui um cliente que você selecionou dentro da grade de dados, você vai querer chamar imediatamente **UpdateCustomersAsync** para atualizar a interface do usuário. No entanto, você não precisa chamar esse método se você estiver excluindo um cliente que você acabou de criar.

1. Navegue até **ViewModels\CustomerListPageViewModel.cs**e atualizar o **DeleteAndUpdateAsync** método:

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

2. Na **Views\CustomerListPage.xaml**, atualize o painel de pilha para adicionar um novo cliente de modo que ele contém um segundo botão:

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

3. Na **ViewModels\CustomerListPageViewModel.cs**, atualize o **DeleteNewCustomerAsync** método para excluir o novo cliente:

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

4. Execute seu aplicativo. Agora você pode excluir os clientes, dentro da grade de dados ou no painel de pilha.

![Excluir um novo cliente](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Conclusão

Parabéns! Com tudo isso feito, o seu aplicativo agora tem uma gama completa de operações de banco de dados local. Você pode criar, ler, atualizar e excluir clientes dentro de sua interface do usuário, e essas alterações são salvas em seu banco de dados e persistirão entre diferentes versões do seu aplicativo.

Agora que você tiver concluído, considere o seguinte:
* Se você ainda não fez isso, confira a [visão geral da estrutura de aplicativo](../enterprise/customer-database-app-structure.md) para obter mais informações sobre por que o aplicativo é compilado como ele é.
* Explore os [exemplo completo de banco de dados de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver o aplicativo neste tutorial se baseia.

Ou, se você for para um desafio, você pode continuar em diante...

## <a name="going-further-connect-to-a-remote-database"></a>Indo mais além: Conectar um banco de dados remoto

Nós fornecemos instruções passo a passo de como implementar essas chamadas em relação a um banco de dados SQLite local. Mas e se você quiser usar um banco de dados remoto, em vez disso?

Se você quiser experimentar, você precisará sua própria conta do Azure Active Directory (AAD) e a capacidade de hospedar sua própria fonte de dados.

Você precisará adicionar a autenticação, as funções para lidar com chamadas REST e, em seguida, criar um banco de dados remoto para interagir com o. Há código no completo [exemplo de banco de dados de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) que você pode fazer referência para cada operação necessária.

### <a name="settings-and-configuration"></a>As configurações e definições

As etapas necessárias para se conectar ao banco de dados remoto são indicadas claramente na [Leiame da amostra](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Você precisará fazer o seguinte:

* Forneça sua Id do cliente de conta do Azure para [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Forneça a url do banco de dados remoto para [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Fornecer a cadeia de conexão para o banco de dados [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Associe o aplicativo com a Microsoft Store.
* Copiar sobre a [projeto de serviço](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) em seu aplicativo e implantá-lo para o Azure.

### <a name="authentication"></a>Authentication

Você precisará criar um botão para iniciar uma sequência de autenticação e um pop-up ou uma página separada para coletar informações do usuário. Depois de criado, você precisará fornecer o código que solicita informações do usuário e o utiliza para adquirir um token de acesso. O exemplo de banco de dados de pedidos de cliente encapsula chamadas do Microsoft Graph com o **contas** biblioteca para adquirir um token e lidar com a autenticação para uma conta do AAD.

* A lógica de autenticação é implementada no [ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* O processo de autenticação é exibido no personalizado [ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) controle.

### <a name="rest-calls"></a>Chamadas REST

Você não precisará modificar qualquer do código que adicionamos neste tutorial para implementar chamadas REST. Em vez disso, você precisará fazer o seguinte:

* Criar novas implementações do **ICustomerRepository** e **ITutorialRepository** interfaces, Implementando o mesmo conjunto de funções por meio de REST em vez do SQLite. Você precisará para serializar e desserializar JSON e pode encapsular as chamadas REST em um separado **HttpHelper** se você precisar de classe. Consulte a [o exemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) para obter informações específicas.
* Na **App.xaml.cs**, crie uma nova função para inicializar o repositório REST e chamá-lo em vez de **SqliteDatabase** quando o aplicativo é inicializado. Novamente, consulte a [o exemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Depois que todos os três dessas etapas forem concluídos, você deve ser capaz de autenticar sua conta do AAD por meio de seu aplicativo. Chamadas REST para o banco de dados remoto substituirá as chamadas de SQLite locais, mas a experiência do usuário deve ser o mesmo. Se você estiver se sentindo ainda mais ambicioso, você pode adicionar uma página de configurações para permitir que o usuário mudar dinamicamente entre os dois.