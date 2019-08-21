---
description: Este tutorial demonstra como adicionar interfaces de usuário do UWP XAML, criar pacotes do MSIX e incorporar outros componentes modernos ao seu aplicativo do WPF.
title: Adicionar um controle CalendarView da UWP usando ilhas de XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643368"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Parte 3: Adicionar um controle CalendarView da UWP usando ilhas de XAML

Esta é a terceira parte de um tutorial que demonstra como modernizar um aplicativo de área de trabalho WPF de exemplo chamado contoso despesas. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo [, consulte o tutorial: Modernizar um aplicativo](modernize-wpf-tutorial.md)do WPF. Este artigo pressupõe que você já concluiu a [parte 2](modernize-wpf-tutorial-2.md).

No cenário fictício deste tutorial, a equipe de desenvolvimento da Contoso deseja facilitar a escolha da data de um relatório de despesas em um dispositivo habilitado para toque. Nesta parte do tutorial, você adicionará um controle [CALENDARVIEW](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) UWP ao aplicativo. Esse é o mesmo controle usado na funcionalidade de data e hora do Windows 10 na barra de tarefas.

![Imagem CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

Ao contrário do controle **InkCanvas** que você adicionou na [parte 2](modernize-wpf-tutorial-2.md), o kit de ferramentas da Comunidade do Windows não fornece uma versão encapsulada do UWP **CalendarView** que pode ser usada em aplicativos do WPF. Como alternativa, você hospedará um **InkCanvas** no controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) genérico. Você pode usar esse controle para hospedar qualquer controle UWP de primeira empresa fornecido pela biblioteca SDK do Windows ou WinUI ou qualquer controle UWP personalizado criado por terceiros. O controle **WindowsXamlHost** é fornecido pelo pacote `Microsoft.Toolkit.Wpf.UI.XamlHost` NuGet do pacote. Esse pacote está incluído `Microsoft.Toolkit.Wpf.UI.Controls` no pacote NuGet que você instalou na [parte 2](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Este tutorial demonstra apenas como usar o [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar o controle **CalendarView** de primeira empresa fornecido pelo SDK do Windows. Para obter instruções que demonstram como hospedar um controle personalizado, consulte [hospedar um controle UWP personalizado em um aplicativo WPF usando ilhas XAML](host-custom-control-with-xaml-islands.md).

Para usar o controle **WindowsXamlHost** , você precisará chamar diretamente as APIs do WinRT do código no aplicativo do WPF. O `Microsoft.Windows.SDK.Contracts` pacote NuGet contém as referências necessárias para permitir que você chame APIs do WinRT do aplicativo. Esse pacote também está incluído no `Microsoft.Toolkit.Wpf.UI.Controls` pacote NuGet que você instalou na [parte 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Adicionar o controle WindowsXamlHost

1. Em **Gerenciador de soluções**, expanda a pasta exibições no projeto **ContosoExpenses. Core** e clique duas vezes no arquivo **AddNewExpense. XAML** . Esse é o formulário usado para adicionar uma nova despesa à lista. Veja como ele aparece na versão atual do aplicativo.

    ![Adicionar nova despesa](images/wpf-modernize-tutorial/AddNewExpense.png)

    O controle do seletor de data incluído no WPF é destinado a computadores tradicionais com o mouse e o teclado. Escolher uma data com uma tela sensível ao toque não é realmente viável, devido ao pequeno tamanho do controle e ao espaço limitado entre cada dia no calendário.

2. Na parte superior do arquivo **AddNewExpense. XAML** , adicione o atributo a seguir ao elemento **Window** .

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Depois de adicionar esse atributo, o elemento **Window** agora deve ser assim.

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

3. Altere o atributo **Height** do elemento **Window** de 450 para 800. Isso é necessário porque o controle **CALENDARVIEW** UWP ocupa mais espaço do que o seletor de data do WPF.

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

4. Localize o `DatePicker` elemento próximo à parte inferior do arquivo e substitua esse elemento pelo XAML a seguir.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Esse XAML adiciona o controle **WindowsXamlHost** . A propriedade **InitialTypeName** indica o nome completo do controle UWP que você deseja hospedar (nesse caso, **Windows. UI. XAML. Controls. CalendarView**).

5. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário na lista e, em seguida, pressione o botão **Adicionar nova despesa** . Confirme se a página a seguir hospeda o novo controle UWP **CalendarView** .

    ![CalendarView Wrapper](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Feche o aplicativo.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagir com o controle WindowsXamlHost

Em seguida, você atualizará o aplicativo para processar a data selecionada, exibi-la na tela e preencher o objeto de **despesa** para salvar no banco de dados.

O UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) contém dois membros que são relevantes para este cenário:

- A propriedade **SelectedDates** contém a data selecionada pelo usuário.
- O evento SelectedDatesChanged é gerado quando o usuário seleciona uma data.

No entanto, o controle **WindowsXamlHost** é um controle de host genérico para *qualquer* tipo de controle UWP. Como tal, ele não expõe uma propriedade chamada **SelectedDates** ou um evento chamado **SelectedDatesChanged**, pois elas são específicas do controle **CalendarView** . Para acessar esses membros, você deve escrever um código que converta o **WindowsXamlHost** para o tipo **CalendarView** . O melhor lugar para fazer isso é em resposta ao evento **ChildChanged** do controle **WindowsXamlHost** , que é gerado quando o controle hospedado é renderizado.

1. No arquivo **AddNewExpense. XAML** , adicione um manipulador de eventos para o evento ChildChanged do controle **WindowsXamlHost** que você adicionou anteriormente. Quando terminar, o elemento **WindowsXamlHost** deverá ser assim.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. No mesmo arquivo, localize o elemento **Grid.** teledefinitions da **grade**principal. Adicione mais um elemento de seleção com a **altura** igual a **automática** no final da lista de elementos filho. Quando terminar, o elemento **Grid.** datadefinitions deverá ter esta aparência (agora, deve haver 9 elementos de **teledefinição** ).

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

5. Localize o elemento **Button** próximo ao final do arquivo e altere a propriedade **Grid. Row** de **7** para **8**. Isso desloca o botão uma linha para baixo na grade porque você adicionou uma nova linha.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Abra o arquivo de código **AddNewExpense.XAML.cs** .

7. Adicione a seguinte instrução à parte superior do arquivo.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Adicione o manipulador de eventos a seguir à classe `AddNewExpense`. Esse código implementa o evento ChildChanged do controle **WindowsXamlHost** que você declarou anteriormente no arquivo XAML.

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

    Esse código usa a propriedade **Child** do controle **WindowsXamlHost** para acessar o controle **CalendarView** UWP. Em seguida, o código assina o evento SelectedDatesChanged, que é disparado quando o usuário seleciona uma data no calendário. Esse manipulador de eventos passa a data selecionada para o ViewModel em que o novo objeto de **despesa** é criado e salvo no banco de dados. Para fazer isso, o código usa a infraestrutura de mensagens fornecida pelo pacote NuGet do MVVM Light. O código envia uma mensagem chamada **SelectedDateMessage** para o ViewModel, que irá recebê-la e definir a propriedade **Date** com o valor selecionado. Nesse cenário, o controle **CalendarView** é configurado para o modo de seleção única, portanto, a coleção conterá apenas um elemento.

    Neste ponto, o projeto ainda não será compilado porque está faltando a implementação para a classe **SelectedDateMessage** . As etapas a seguir implementam essa classe.

9. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **mensagens** e escolha a **classe add->** . Nomeie a nova classe **SelectedDateMessage** e clique em **Adicionar**.

10. Substitua o conteúdo do arquivo de código **SelectedDateMessage.cs** pelo código a seguir. Esse código adiciona uma instrução using para o `GalaSoft.MvvmLight.Messaging` namespace (do pacote MVVM Light NuGet), herda da classe **MessageBase** e adiciona a propriedade **DateTime** que é inicializada por meio do construtor público. Quando terminar, o arquivo deverá ter a seguinte aparência.

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

11. Em seguida, você atualizará o ViewModel para receber essa mensagem e preencher a propriedade **Date** do ViewModel. No **Gerenciador de soluções**, expanda a pasta ViewModels e abra o arquivo **AddNewExpenseViewModel.cs** .

12. Localize o construtor público para a `AddNewExpenseViewModel` classe e adicione o código a seguir ao final do construtor. Esse código registra-se para receber o **SelectedDateMessage**, extrair a data selecionada dela por meio da propriedade **SelectedDate** e usamos-a para definir a propriedade **Date** exposta pelo ViewModel. Como essa propriedade é associada ao controle **TextBlock** que você adicionou anteriormente, você pode ver a data selecionada pelo usuário.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Quando terminar, o Construtor deverá `AddNewExpenseViewModel` ter a seguinte aparência. 

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
    > O `SaveExpenseCommand` método no mesmo arquivo de código faz o trabalho de salvar as despesas no banco de dados. Esse método usa a propriedade **Date** para criar o novo objeto de **despesa** . Essa propriedade agora está sendo populada pelo controle **CalendarView** por `SelectedDateMessage` meio da mensagem que você acabou de criar.

13. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um dos funcionários disponíveis e, em seguida, clique no botão **Adicionar nova despesa** . Preencha todos os campos no formulário e escolha uma data no novo controle **CalendarView** . Confirme se a data selecionada é exibida como uma cadeia de caracteres abaixo do controle.

14. Pressione o botão **salvar** . O formulário será fechado e a nova despesa será adicionada ao final da lista de despesas. A primeira coluna com a data de despesa deve ser a data que você selecionou no controle **CalendarView** .

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você substituiu com êxito um controle de data e hora do WPF pelo controle **CALENDARVIEW** UWP, que dá suporte a canetas sensíveis ao toque e à digital, além de entrada do mouse e do teclado. Embora o kit de ferramentas da Comunidade do Windows não forneça uma versão encapsulada do controle **CALENDARVIEW** UWP que pode ser usado diretamente em um aplicativo do WPF, você conseguiu hospedar o controle usando o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) genérico.

Agora você está pronto para [a parte 4: Adicionar notificações](modernize-wpf-tutorial-4.md)e atividades de usuário do Windows 10.
