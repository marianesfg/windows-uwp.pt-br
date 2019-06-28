---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Adicionar um controle de exibição de calendário UWP usando ilhas de XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420109"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Parte 3: Adicionar um controle de exibição de calendário UWP usando ilhas de XAML

Isso é a terceira parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso despesas. Para obter uma visão geral do tutorial, os pré-requisitos e instruções para baixar o aplicativo de exemplo, consulte [Tutorial: Modernize um aplicativo WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu [parte 2](modernize-wpf-tutorial-2.md).

O cenário fictício deste tutorial, a equipe de desenvolvimento do Contoso deseja tornar mais fácil escolher a data para um relatório de despesas em um dispositivo habilitado para toque. Nesta parte do tutorial, você adicionará um UWP [exibição de calendário](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) controle ao aplicativo. Isso é o mesmo controle que é usado a funcionalidade de data e hora do Windows 10 na barra de tarefas.

![Imagem de CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

Ao contrário de **InkCanvas** controle que você adicionou na [parte 2](modernize-wpf-tutorial-2.md), o Kit de ferramentas de comunidade do Windows não fornece uma versão empacotada da UWP **exibição de calendário** que pode ser usado em aplicativos WPF . Como alternativa, você hospedará um **InkCanvas** no genérico [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) controle. Você pode usar esse controle para hospedar qualquer controle UWP fornecido pelo fornecedor de terceiros de plataforma ou uma 3ª. O **WindowsXamlHost** o controle é fornecido pelo `Microsoft.Toolkit.Wpf.UI.XamlHost` pacote do pacote NuGet. Este pacote está incluído com o `Microsoft.Toolkit.Wpf.UI.Controls` que você instalou o pacote do NuGet [parte 2](modernize-wpf-tutorial-2.md).

Para usar o **WindowsXamlHost** controle, você precisará chamar diretamente APIs do WinRT do código no aplicativo do WPF. O `Microsoft.Windows.SDK.Contracts` pacote NuGet contém as referências necessárias para que você possa chamar APIs do WinRT do aplicativo. Este pacote também está incluído na `Microsoft.Toolkit.Wpf.UI.Controls` que você instalou o pacote do NuGet [parte 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Adicionar o controle WindowsXamlHost

1. No **Gerenciador de soluções**, expanda o **exibições** pasta no **ContosoExpenses.Core** do projeto e clique duas vezes o **AddNewExpense.xaml**arquivo. Este é o formulário usado para adicionar uma nova despesa à lista. Aqui está como ele aparece na versão atual do aplicativo.

    ![Adicionar nova despesa](images/wpf-modernize-tutorial/AddNewExpense.png)

    O controle de seletor de data incluído no WPF se destina a computadores tradicionais com o mouse e teclado. Escolher uma data com uma tela sensível ao toque não é viável, devido ao tamanho pequeno do controle e o espaço limitado entre cada dia no calendário.

2. Na parte superior a **AddNewExpense.xaml** arquivo, adicione o seguinte atributo para o **janela** elemento.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Depois de adicionar esse atributo, o **janela** elemento deve ficar assim.

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

3. Alterar o **altura** atributo da **janela** elemento de 450 para 800. Isso é necessário porque a UWP **exibição de calendário** controle utiliza mais espaço do que o selecionador de data do WPF.

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

4. Localize o `DatePicker` elemento perto da parte inferior do arquivo e substitua esse elemento com o XAML a seguir.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Esse XAML adiciona os **WindowsXamlHost** controle. O **InitialTypeName** propriedade indica o nome completo do controle UWP que você deseja hospedar (nesse caso, **Windows.UI.Xaml.Controls.CalendarView**).

5. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário da lista e, em seguida, pressione a **adicionar novo despesa** botão. Confirme que a seguinte página hospeda a nova UWP **exibição de calendário** controle.

    ![Wrapper de exibição de calendário](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Feche o aplicativo.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagir com o controle WindowsXamlHost

Em seguida, você atualizará o aplicativo para processar a data selecionada, exibi-lo na tela e preencher a **despesas** objeto a ser salvo no banco de dados.

A UWP [exibição de calendário](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) contém dois membros que são relevantes para este cenário:

- O **SelectedDates** propriedade contém a data selecionada pelo usuário.
- O **SelectedDatesChanged** é gerado quando o usuário seleciona uma data.

No entanto, o **WindowsXamlHost** é um controle de host genérico para *qualquer* tipo de controle UWP. Como tal, ele não expõe uma propriedade chamada **SelectedDates** ou um evento chamado **SelectedDatesChanged**, pois elas são específicas da **exibição de calendário** controle. Para acessar esses membros, você deve escrever código que converte o **WindowsXamlHost** para o **exibição de calendário** tipo. O melhor lugar para fazer isso é em resposta à **ChildChanged** eventos da **WindowsXamlHost** controle, que é gerado quando o controle hospedado foi renderizado.

1. No **AddNewExpense.xaml** do arquivo, adicione um manipulador de eventos para o **ChildChanged** evento do **WindowsXamlHost** controle adicionado anteriormente. Quando você terminar, o **WindowsXamlHost** elemento deve ter esta aparência.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. No mesmo arquivo, localize o **Grid. RowDefinitions** elemento da janela principal **grade**. Adicionar mais **RowDefinition** elemento com o **altura** igual a **automático** no final da lista de elementos filho. Quando você terminar, o **Grid. RowDefinitions** elemento deve ter esta aparência (agora deve ser 9 **RowDefinition** elementos).

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

4. Adicione o seguinte XAML após a **WindowsXamlHost** elemento e antes do **botão** elemento perto do fim do arquivo.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Localize o **botão** elemento perto do fim do arquivo e altere o **Row** propriedade de **7** para **8**. Isso muda o botão para baixo uma linha na grade de como você adicionou uma nova linha.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Abra o **AddNewExpense.xaml.cs** arquivo de código.

7. Adicione a seguinte instrução na parte superior do arquivo.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Adicione o seguinte manipulador de eventos para o `AddNewExpense` classe. Esse código implementa o **ChildChanged** eventos da **WindowsXamlHost** você declarada anteriormente no arquivo XAML do controle.

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

    Esse código usa o **filho** propriedade da **WindowsXamlHost** controle para acessar a UWP **exibição de calendário** controle. O código, em seguida, assina os **SelectedDatesChanged** evento, que é disparado quando o usuário seleciona uma data do calendário. Esse manipulador de eventos passa a data selecionada para o ViewModel em que o novo **despesas** objeto é criado e salvo no banco de dados. Para fazer isso, o código usa a infraestrutura de mensagens fornecida pelo pacote NuGet do MVVM Light. O código envia uma mensagem de chamada **SelectedDateMessage** ao ViewModel, que irá recebê-la e definir o **data** propriedade com o valor selecionado. Nesse cenário a **exibição de calendário** controle é configurado para o modo de seleção única, portanto, a coleção conterá apenas um elemento.

    Neste ponto, o projeto ainda não será compilado porque está faltando a implementação para o **SelectedDateMessage** classe. As etapas a seguir implementam essa classe.

9. Na **Gerenciador de soluções**, clique com botão direito do **mensagens** pasta e escolha **Adicionar -> classe**. Nomeie a nova classe **SelectedDateMessage** e clique em **Add**.

10. Substitua o conteúdo do **SelectedDateMessage.cs** arquivo de código com o código a seguir. Esse código adiciona um usando instrução para o `GalaSoft.MvvmLight.Messaging` herda de namespace (do MVVM Light pacote do NuGet), o **MessageBase** classe e adiciona **DateTime** propriedade que é inicializada por meio do construtor público. Quando você terminar, o arquivo deve ter esta aparência.

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

11. Em seguida, você atualizará o ViewModel para receber essa mensagem e preencher a **data** propriedade de ViewModel. Na **Gerenciador de soluções**, expanda o **ViewModels** pasta e abra o **AddNewExpenseViewModel.cs** arquivo.

12. Localize o construtor público para o `AddNewExpenseViewModel` de classe e adicione o seguinte código ao final do construtor. Esse código registra para receber o **SelectedDateMessage**, extrair a data selecionada dele por meio de **SelectedDate** propriedade e podemos usá-lo para definir o **data** propriedade expostos pelo ViewModel. Porque essa propriedade está associada com o **TextBlock** controle que você adicionou anteriormente, você pode ver a data selecionada pelo usuário.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Quando você terminar, o `AddNewExpenseViewModel` construtor deve ter esta aparência. 

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
    > O `SaveExpenseCommand` método no mesmo arquivo de código faz o trabalho de salvar as despesas no banco de dados. Esse método usa o **data** propriedade para criar o novo **despesas** objeto. Essa propriedade agora está sendo populada pela **exibição de calendário** controlar por meio do `SelectedDateMessage` você acabou de mensagem criada.

13. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um dos funcionários disponíveis e, em seguida, clique no **adicionar novo despesa** botão. Preencha todos os campos no formulário e escolha uma data a partir do novo **exibição de calendário** controle. Confirme se que a data selecionada é exibida como uma cadeia de caracteres abaixo do controle.

14. Pressione a **salvar** botão. O formulário será fechado e a despesa novo é adicionada ao final da lista de despesas. A primeira coluna com a data de despesa deve ser a data selecionada na **exibição de calendário** controle.

## <a name="next-steps"></a>Próximas etapas

Neste ponto no tutorial, você substituiu com sucesso um controle de tempo de data do WPF com a UWP **exibição de calendário** controle, que dá suporte a toque e canetas digitais, além de mouse e teclado de entrada. Embora o Kit de ferramentas de comunidade do Windows não fornece uma versão empacotada da UWP **exibição de calendário** controle que pode ser usado diretamente em um aplicativo do WPF, era possível hospedar o controle usando o genérico [WindowsXamlHost ](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) controle.

Agora você está pronto para [parte 4: Adicionar atividades de usuário do Windows 10 e notificações](modernize-wpf-tutorial-4.md).
