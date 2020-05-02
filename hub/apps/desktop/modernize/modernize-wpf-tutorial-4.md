---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo WPF.
title: Adicionar atividades e notificações de usuário do Windows 10
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67420123"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Parte 4: Adicionar atividades e notificações de usuário do Windows 10

Esta é a quarta parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho WPF de exemplo chamado Despesas da Contoso. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo, confira [Tutorial: modernizar um aplicativo WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu a [parte 3](modernize-wpf-tutorial-3.md).

Nas partes anteriores deste tutorial, você adicionou controles de XAML UWP ao aplicativo usando as Ilhas XAML. Como resultado disso, você também habilitou o aplicativo para chamar qualquer API do WinRT. Isso oferece a oportunidade para o aplicativo usar vários dos outros recursos fornecidos pelo Windows 10, não apenas os controles XAML UWP.

No cenário fictício deste tutorial, a equipe de desenvolvimento da Contoso decidiu adicionar dois novos recursos ao aplicativo: atividades e notificações. Esta parte do tutorial mostra como implementar esses recursos.

## <a name="add-a-user-activity"></a>Adicionar uma atividades de usuário

No Windows 10, os aplicativos podem acompanhar atividades executadas pelo usuário, como abrir um arquivo ou exibir uma página específica. Essas atividades são disponibilizadas na Linha do Tempo, um recurso introduzido no Windows 10 versão 1803, que permite ao usuário voltar com rapidez ao passado e retomar uma atividade iniciada anteriormente.

![Imagem da Linha do Tempo do Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

As atividades do usuário são monitoradas usando o [Microsoft Graph](https://developer.microsoft.com/graph/). No entanto, ao criar um aplicativo do Windows 10, você não precisa interagir diretamente com os pontos de extremidade REST fornecidos pelo Microsoft Graph. Em vez disso, você pode usar um conjunto conveniente de APIs do WinRT. Usaremos essas APIs do WinRT no aplicativo de Despesas da Contoso para controlar toda vez que o usuário abrir uma despesa dentro do aplicativo e usar Cartões Adaptáveis para permitir que os usuários criem a atividade.

### <a name="introduction-to-adaptive-cards"></a>Introdução aos Cartões Adaptáveis

Esta seção fornece uma visão geral rápida dos [Cartões Adaptáveis](https://docs.microsoft.com/adaptive-cards/). Se você não precisar dessas informações, poderá ignorar isso e ir direto para as instruções sobre como [adicionar um Cartão Adaptável](#add-an-adaptive-card).

Os Cartões Adaptáveis permitem que os desenvolvedores troquem o conteúdo do cartão de maneira comum e consistente. Um Cartão Adaptável é descrito por uma carga JSON que define seu conteúdo, que pode incluir texto, imagens, ações e muito mais.

Um Cartão Adaptável define apenas o conteúdo, e não a aparência visual do conteúdo. A plataforma na qual o Cartão Adaptável é recebido pode renderizar o conteúdo usando o estilo mais apropriado. Os Cartões Adaptáveis são criados por meio de [um renderizador](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started), que é capaz de pegar a carga JSON e convertê-la na interface do usuário nativa. Por exemplo, a interface do usuário pode ser XAML para um aplicativo WPF ou UWP, AXML para um aplicativo Android ou HTML para um site ou um bate-papo de bot.

Aqui está um exemplo de uma carga simples de Cartão Adaptável.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

A imagem abaixo mostra como esse JSON é renderizado de maneiras diferentes por canal do Teams, da Cortana e por uma notificação do Windows.

![Imagem de renderização de Cartão Adaptável](images/wpf-modernize-tutorial/AdaptiveCards.png)

Os cartões adaptáveis desempenham um papel importante na Linha do Tempo porque são uma maneira de renderização de atividades pelo Windows. Cada miniatura exibida na Linha do Tempo é, na verdade, um Cartão Adaptável. Assim, quando você quiser criar uma atividade de usuário dentro do aplicativo, terá que fornecer um Cartão Adaptável para renderizá-la.

> [!NOTE]
> Uma ótima maneira de levantar ideias sobre o design de um Cartão Adaptável é usar [o designer online](https://adaptivecards.io/designer/). Você terá a chance de projetar o cartão com blocos de construção (imagens, textos, colunas etc.) e obter o JSON correspondente. Depois de ter uma ideia do design final, você pode usar uma biblioteca chamada [Cartões Adaptáveis](https://www.nuget.org/packages/AdaptiveCards/) para facilitar a criação de seu Cartão Adaptável usando classes C# em vez de JSON simples, que pode ser difícil de depurar e compilar.

### <a name="add-an-adaptive-card"></a>Adicionar um Cartão Adaptável

1. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** no Gerenciador de Soluções e escolha **Gerenciar Pacotes do NuGet**.

2. Na janela **Gerenciador de Pacotes NuGet**, clique em **Procurar**. Pesquise pelo pacote `Newtonsoft.Json` e instale a versão mais recente disponível. Essa é uma biblioteca de manipulação JSON popular que você usará para ajudar a manipular as cadeias de caracteres JSON exigidas pelos Cartões Adaptáveis.

    ![Pacote NuGet NewtonSoft.Json](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Se você não instalar o pacote `Newtonsoft.Json` separadamente, a biblioteca de Cartões Adaptáveis fará referência a uma versão mais antiga do pacote `Newtonsoft.Json` que não é compatível com o .NET Core 3.0.

2. Na janela **Gerenciador de Pacotes NuGet**, clique em **Procurar**. Pesquise pelo pacote `AdaptiveCards` e instale a versão mais recente disponível.

    ![Pacote NuGet de Cartões Adaptáveis](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Adicionar -> Classe**. Nomeie a classe **TimelineService.cs** e clique em **OK**.

4. No arquivo **TimelineService.cs**, adicione as instruções a seguir na parte superior do arquivo.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Altere o namespace declarado no arquivo de `ContosoExpenses.Core` para `ContosoExpenses`.

5. Adicione o método a seguir à classe `TimelineService`.

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>Sobre o código

Esse método recebe um objeto **Expense** com todas as informações sobre a despesa a serem renderizadas e cria um objeto **AdaptiveCard**. O método adiciona o seguinte ao cartão:

- Um título, que usa a descrição da despesa.
- Uma imagem, que é o logotipo da Contoso.
- O valor da despesa.
- A data da despesa.

Os últimos três elementos são divididos em duas colunas diferentes, para que o logotipo da Contoso e os detalhes sobre a despesa possam ser colocados lado a lado. Depois que o objeto é compilado, o método retorna a cadeia de caracteres JSON correspondente com a ajuda do método **ToJson**.

### <a name="define-the-user-activity"></a>Definir a atividade do usuário

Agora que você definiu o Cartão Adaptável, é possível criar uma atividade de usuário com base nele.

1. Adicione as seguintes instruções na parte superior do arquivo **TimelineService.cs**:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Esses são os namespaces da UWP. Isso resolve porque o pacote NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que você instalou na etapa 2 inclui uma referência ao pacote `Microsoft.Windows.SDK.Contracts`, que habilita o projeto **ContosoExpenses.Core** para referenciar as APIs do WinRT, embora seja um projeto do .NET Core 3.

2. Adicione as declarações de campo a seguir à classe `TimelineService`.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Adicione o método a seguir à classe `TimelineService`.

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. Salve as alterações em **TimelineService.cs**.

#### <a name="about-the-code"></a>Sobre o código

O método `AddToTimeline` obtém primeiro um objeto **UserActivityChannel**, que é necessário para armazenar as atividades do usuário. Em seguida, ele cria uma nova atividade de usuário usando o método **GetOrCreateUserActivityAsync**, que requer um identificador exclusivo. Dessa forma, se uma atividade já existir, o aplicativo poderá atualizá-la; caso contrário, ele criará uma. O identificador a ser passado depende do tipo de aplicativo que você está criando:

* Se você quiser atualizar sempre a mesma atividade para que a Linha do Tempo mostre apenas a mais recente, poderá usar um identificador fixo (como **Expenses**).
* Se você quiser acompanhar todas as atividades como diferentes, para que a Linha do Tempo exiba todas elas, poderá usar um identificador dinâmico.

Nesse cenário, o aplicativo acompanhará cada despesa aberta como uma atividade de usuário diferente, para que o código crie cada identificador usando a palavra-chave **Expense-** seguida pela ID de despesa exclusiva.

Depois que o método cria um objeto **UserActivity**, ele o preenche com as seguintes informações:

* Um **ActivationUri** que é invocado quando o usuário clica na atividade na Linha do Tempo. O código usa um protocolo personalizado chamado **contosoexpenses** com o qual o aplicativo lidará posteriormente.
* O objeto **VisualElements**, que contém um conjunto de propriedades definindo a aparência visual da atividade. Esse código define o **DisplayText** (que é o título exibido na parte superior da entrada na Linha do Tempo) e o **Content**. 

É aí que o Cartão Adaptável definido anteriormente desempenha uma função. O aplicativo passa o Cartão Adaptável que você criou como conteúdo para o método. No entanto, o Windows 10 usa um objeto diferente para representar um cartão em comparação com aquele usado pelo pacote NuGet `AdaptiveCards`. Portanto, o método recria o cartão usando o método **CreateAdaptiveCardFromJson** exposto pela classe **AdaptiveCardBuilder**. Depois que o método cria a atividade do usuário, ele a salva e cria uma sessão.

Quando um usuário clicar em uma atividade na Linha do Tempo, o protocolo **contosoexpenses://** será ativado, e a URL incluirá as informações de que o aplicativo precisa para recuperar a despesa selecionada. Como tarefa opcional, você pode implementar a ativação do protocolo para que o aplicativo reaja corretamente quando o usuário usar a Linha do Tempo.

### <a name="integrate-the-application-with-timeline"></a>Integrar o aplicativo com a Linha do Tempo

Agora que você criou uma classe que interage com a Linha do Tempo, podemos começar a usá-la para aprimorar a experiência do aplicativo. O melhor momento para usar o método **AddToTimeline** exposto pela classe **TimelineService** é quando o usuário abre a página de detalhes de uma despesa.

1. No projeto **ContosoExpenses.Core**, expanda a pasta **ViewModels** e abra o arquivo **ExpenseDetailViewModel.cs**. Esse é o ViewModel que dá suporte à janela de detalhes das despesas.

2. Localize o construtor público da classe **ExpenseDetailViewModel** e adicione o código a seguir ao final do construtor. Sempre que a janela de despesas é aberta, o método chama o método **AddToTimeline** e passa a despesa atual. A classe **TimelineService** usa essas informações para criar uma atividade de usuário usando as informações de despesa.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Quando você terminar, o construtor deverá ter esta aparência.

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário na lista e uma despesa. Na página de detalhes, observe a descrição da despesa, a data e a quantidade.

4. Pressione **Iniciar+Tab** para abrir a Linha do Tempo.

5. Role para baixo na lista de aplicativos abertos no momento até ver a seção intitulada **Anteriormente hoje**. Essa seção mostra algumas de suas atividades de usuário mais recentes. Clique no link **Ver todas as atividades** ao lado do título **Anteriormente hoje**.

6. Confirme se há um novo cartão com as informações sobre a despesa que você acabou de selecionar no aplicativo.

    ![Linha do Tempo de Despesas da Contoso](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Se você abrir outras despesas agora, verá novos cartões sendo adicionados como atividades do usuário. Lembre-se de que o código usa um identificador diferente para cada atividade, portanto, ele cria um cartão para cada despesa que você abre no aplicativo.

8. Feche o aplicativo.

## <a name="add-a-notification"></a>Adicionar uma notificação

O segundo recurso que a equipe de desenvolvimento da Contoso deseja adicionar é uma notificação mostrada ao usuário sempre que uma nova despesa é salva no banco de dados. Para fazer isso, você pode aproveitar o sistema de notificações interno do Windows 10, que é exposto aos desenvolvedores por meio das APIs do WinRT. Esse sistema de notificação tem muitas vantagens:

- As notificações são consistentes com o restante do sistema operacional.
- Elas são acionáveis.
- Elas são armazenadas na Central de Ações para que possam ser revisadas posteriormente.

Para adicionar uma notificação ao aplicativo:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Adicionar -> Classe**. Nomeie a classe **NotificationService.cs** e clique em **OK**.

2. No arquivo **NotificationService.cs**, adicione as instruções a seguir na parte superior do arquivo.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Altere o namespace declarado no arquivo de `ContosoExpenses.Core` para `ContosoExpenses`.

4. Adicione o método a seguir à classe `NotificationService`.

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    As notificações do sistema são representadas por uma carga XML, que pode incluir texto, imagens, ações e muito mais. Você pode encontrar todos os elementos com suporte [aqui](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Esse código usa um esquema muito simples com duas linhas de texto: o título e o corpo. Depois que o código define a carga XML e a carrega em um objeto **XmlDocument**, ele encapsula o XML em um objeto **ToastNotification** e o mostra usando a classe **ToastNotificationManager**.

5. No projeto **ContosoExpenses.Core**, expanda a pasta **ViewModels** e abra o arquivo **AddNewExpenseViewModel.cs**. 

6. Localize o método `SaveExpenseCommand`, que é disparado quando o usuário pressiona o botão para salvar uma nova despesa. Adicione o código a seguir a esse método, logo após a chamada para o método `SaveExpense`.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Quando você terminar, o método `SaveExpenseCommand` deverá ter esta aparência.

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário na lista e clique no botão **Adicionar nova despesa**. Preencha todos os campos no formulário e pressione **Salvar**.

8. Você receberá a exceção a seguir.

    ![Erro de notificação do sistema](images/wpf-modernize-tutorial/ToastNotificationError.png)

Essa exceção é causada pelo fato de que o aplicativo de Despesas da Contoso ainda não tem o identificador de pacote. Algumas APIs do WinRT, incluindo a API de notificações, exigem o identificador de pacote antes que possam ser usadas em um aplicativo. Os aplicativos UWP recebem o identificador de pacote por padrão, pois eles só podem ser distribuídos por meio de pacotes MSIX. Outros tipos de aplicativos do Windows, incluindo aplicativos WPF, também podem ser implantados por meio de pacotes MSIX para obter o identificador de pacote. A próxima parte deste tutorial apresentará como fazer isso.

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você adicionou com êxito uma atividade de usuário ao aplicativo que se integra à Linha do Tempo do Windows e também adicionou uma notificação ao aplicativo que é acionada quando os usuários criam uma despesa. No entanto, a notificação ainda não funciona porque o aplicativo requer o identificador de pacote para usar a API de notificações. Para saber como criar um pacote MSIX para o aplicativo obter o identificador de pacote e outros benefícios de implantação, confira [Parte 5: empacotar e implantar com o MSIX](modernize-wpf-tutorial-5.md).
