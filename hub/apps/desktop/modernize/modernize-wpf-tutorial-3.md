---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo WPF.
title: Adicionar um controle CalendarView da UWP usando ilhas de XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "69643368"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Parte 3: Adicionar um controle CalendarView da UWP usando ilhas de XAML

Esta é a terceira parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Despesas da Contoso. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo, confira [Tutorial: modernizar um aplicativo WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu a [parte 2](modernize-wpf-tutorial-2.md).

No cenário fictício deste tutorial, a equipe de desenvolvimento da Contoso deseja facilitar a escolha da data de um relatório de despesas em um dispositivo habilitado para toque. Nesta parte do tutorial, você adicionará um controle [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) da UWP ao aplicativo. Esse é o mesmo controle usado na funcionalidade de data e hora do Windows 10 na barra de tarefas.

![Imagem do CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

Diferentemente do controle **InkCanvas** adicionado na [parte 2](modernize-wpf-tutorial-2.md), o Kit de Ferramentas da Comunidade do Windows não fornece uma versão agrupada de **CalendarView** da UWP que pode ser usada nos aplicativos WPF. Como alternativa, você hospedará um **InkCanvas** no controle genérico [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost). Você pode usar esse controle para hospedar qualquer controle UWP primário fornecido pela biblioteca SDK do Windows ou WinUI ou qualquer controle da UWP personalizado criado por terceiros. O controle **WindowsXamlHost** é fornecido pelo pacote NuGet do pacote `Microsoft.Toolkit.Wpf.UI.XamlHost`. Esse pacote está incluído no pacote NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que você instalou na [parte 2](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Este tutorial demonstra apenas como usar [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar o controle **CalendarView** primário fornecido pelo SDK do Windows. Para uma explicação passo a passo que demonstra como hospedar um controle personalizado, confira [Hospedar um controle UWP personalizado em um aplicativo WPF usando as Ilhas XAML](host-custom-control-with-xaml-islands.md).

Para usar o controle **WindowsXamlHost**, você precisará chamar diretamente as APIs do WinRT do código no aplicativo WPF. O pacote NuGet `Microsoft.Windows.SDK.Contracts` contém as referências necessárias para permitir que você chame APIs do WinRT do aplicativo. Esse pacote também está incluído com o pacote NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que você instalou na [parte 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Adicionar o controle WindowsXamlHost

1. No **Gerenciador de Soluções**, expanda a pasta **Exibições** no projeto **ContosoExpenses.Core** e clique duas vezes no arquivo **AddNewExpense.xaml.xaml**. Esse é o formulário usado para adicionar uma nova despesa à lista. Veja aqui como ele aparece na versão atual do aplicativo.

    ![Adicionar nova despesa](images/wpf-modernize-tutorial/AddNewExpense.png)

    O controle do seletor de data incluído no WPF é destinado a computadores tradicionais com mouse e teclado. Escolher uma data com uma tela touch não é muito viável, devido ao tamanho pequeno do controle e ao espaço limitado entre cada dia no calendário.

2. Na parte superior do arquivo **AddNewExpense.xaml**, adicione o atributo a seguir ao elemento **Window**.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Após a adição desse atributo, o elemento **Window** deverá ter esta aparência.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. Altere o atributo **Height** do elemento **Window** de 450 para 800. Isso é necessário porque o controle **CalendarView** da UWP ocupa mais espaço do que o seletor de data do WPF.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. Localize o elemento `DatePicker` próximo à parte inferior do arquivo e substitua esse elemento pelo XAML a seguir.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Esse XAML adiciona o controle **WindowsXamlHost**. A propriedade **InitialTypeName** indica o nome completo do controle da UWP que você deseja hospedar (nesse caso, **Windows.UI.Xaml.Controls.CalendarView**).

5. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário na lista e pressione o botão **Adicionar nova despesa**. Confirme se a página a seguir hospeda o novo controle **CalendarView** da UWP.

    ![Encapsulamento do CalendarView](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Feche o aplicativo.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagir com o controle WindowsXamlHost

Em seguida, você atualizará o aplicativo para processar a data selecionada, exibi-la na tela e preencher o objeto **Expense** para salvá-lo no banco de dados.

O [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) da UWP contém dois membros que são relevantes para este cenário:

- A propriedade **SelectedDates** contém a data selecionada pelo usuário.
- O evento **SelectedDatesChanged** é gerado quando o usuário seleciona uma data.

No entanto, o controle **WindowsXamlHost** é um controle de host genérico para *qualquer* tipo de controle UWP. Dessa forma, ele não expõe uma propriedade chamada **SelectedDates** ou um evento chamado **SelectedDatesChanged**, porque eles são específicos do controle **CalendarView**. Para acessar esses membros, você deve escrever um código que converta o **WindowsXamlHost** para o tipo **CalendarView**. O melhor momento para fazer isso é em resposta ao evento **ChildChanged** do controle **WindowsXamlHost**, que é gerado quando o controle hospedado é renderizado.

1. No arquivo **AddNewExpense.xaml**, adicione um manipulador de eventos para o evento **ChildChanged** do controle **WindowsXamlHost** adicionado anteriormente. Quando você terminar, o elemento **WindowsXamlHost** deverá ter esta aparência.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. No mesmo arquivo, localize o elemento **Grid.RowDefinitions** da **Grid** principal. Adicione mais um elemento de **RowDefinition** com **Height** igual a **Auto** no final da lista de elementos filho. Ao terminar, o elemento **Grid.RowDefinitions** deverá ter esta aparência (agora, deve haver nove elementos **RowDefinition**).

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. Adicione o XAML a seguir após o elemento **WindowsXamlHost** e antes do elemento **Button** próximo ao final do arquivo.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Localize o elemento **Button** próximo ao final do arquivo e altere a propriedade **Grid.Row** de **7** para **8**. Isso desloca o botão uma linha para baixo na grade porque você adicionou uma nova linha.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Abra o arquivo de código **AddNewExpense.xaml.cs**.

7. Adicione a instrução a seguir na parte superior do arquivo.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Adicione o manipulador de eventos a seguir à classe `AddNewExpense`. Esse código implementa o evento **ChildChanged** do controle **WindowsXamlHost** que você declarou anteriormente no arquivo XAML.

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    Esse código usa a propriedade **Child** do controle **WindowsXamlHost** para acessar o controle **CalendarView** da UWP. Depois, o código assina o evento **SelectedDatesChanged**, que é acionado quando o usuário seleciona uma data no calendário. Esse manipulador de eventos passa a data selecionada para o ViewModel, em que o novo objeto **Expense** é criado e salvo no banco de dados. Para fazer isso, o código usa a infraestrutura de mensagens fornecida pelo pacote NuGet de MVVM Light. O código envia uma mensagem chamada **SelectedDateMessage** ao ViewModel, que a receberá e definirá a propriedade **Date** com o valor selecionado. Neste cenário, o controle **CalendarView** é configurado para o modo de seleção única, portanto, a coleção conterá apenas um elemento.

    Neste ponto, o projeto ainda não será compilado porque está faltando a implementação da classe **SelectedDateMessage**. As etapas a seguir implementam essa classe.

9. No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta **Mensagens** e escolha **Adicionar -> Classe**. Nomeie a nova classe como **SelectedDateMessage** e clique em **Adicionar**.

10. Substitua o conteúdo do arquivo de código **SelectedDateMessage.cs** pelo código a seguir. Esse código adiciona uma instrução using para o namespace `GalaSoft.MvvmLight.Messaging` (do pacote NuGet de MVVM Light), herda da classe **MessageBase** e adiciona a propriedade **DateTime** que é inicializada por meio do construtor público. Quando você terminar, o arquivo deverá ter esta aparência.

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. Em seguida, você atualizará o ViewModel para receber essa mensagem e preencherá a propriedade de **Date** do ViewModel. No **Gerenciador de Soluções**, expanda a pasta **ViewModels** e abra o arquivo **AddNewExpenseViewModel.cs**.

12. Localize o construtor público da classe `AddNewExpenseViewModel` e adicione o código a seguir ao final do construtor. Esse código se registra para receber a **SelectedDateMessage**, extrai dela a data selecionada por meio da propriedade **SelectedDate**, e nós a usamos para definir a propriedade **Date** exposta pelo ViewModel. Como essa propriedade é associada ao controle **TextBlock** adicionado anteriormente, você pode ver a data selecionada pelo usuário.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Quando você terminar, o construtor `AddNewExpenseViewModel` deverá ter esta aparência. 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > O método `SaveExpenseCommand` no mesmo arquivo de código faz o trabalho de salvar as despesas no banco de dados. Esse método usa a propriedade **Date** para criar o novo objeto **Expense**. Essa propriedade agora está sendo preenchida pelo controle **CalendarView** por meio da mensagem `SelectedDateMessage` que você acabou de criar.

13. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um dos funcionários disponíveis e clique no botão **Adicionar nova despesa**. Preencha todos os campos no formulário e escolha uma data no novo controle **CalendarView**. Confirme se a data selecionada é exibida como uma cadeia de caracteres abaixo do controle.

14. Pressione o botão **Salvar**. O formulário será fechado, e a nova despesa será adicionada ao final da lista de despesas. A primeira coluna com a data de despesa deve ser a data que você selecionou no controle **CalendarView**.

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você substituiu com êxito um controle de data e hora do WPF pelo controle **CalendarView** da UWP, que dá suporte à entrada por toque e canetas digitais, além do mouse e do teclado. Embora o Kit de Ferramentas da Comunidade do Windows não forneça uma versão encapsulada do controle **CalendarView** da UWP que possa ser usada diretamente em um aplicativo WPF, você conseguiu hospedar o controle usando o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) genérico.

Agora você está pronto para a [Parte 4: adicionar atividades e notificações de usuário do Windows 10](modernize-wpf-tutorial-4.md).
