---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Adicionar notificações e atividades de usuário do Windows 10
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420123"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Parte 4: Adicionar notificações e atividades de usuário do Windows 10

Isso é a quarta parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso despesas. Para obter uma visão geral do tutorial, os pré-requisitos e instruções para baixar o aplicativo de exemplo, consulte [Tutorial: Modernize um aplicativo WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu [parte 3](modernize-wpf-tutorial-3.md).

Na parte anterior deste tutorial, você adicionou que UWP XAML controla ao aplicativo usando ilhas de XAML. Como um por produto isso, você habilitou o aplicativo chamar qualquer API do WinRT também. Isso abre a oportunidade para que o aplicativo use muitos outros recursos oferecidos pelo Windows 10, não apenas os controles UWP XAML.

No cenário fictício deste tutorial, a equipe de desenvolvimento do Contoso decidiu adicionar dois novos recursos ao aplicativo: atividades e notificações. Esta parte do tutorial mostra como implementar esses recursos.

## <a name="add-a-user-activity"></a>Adicionar uma atividade de usuário

No Windows 10, os aplicativos podem acompanhar as atividades executadas pelo usuário como abrir um arquivo ou exibir uma página específica. Essas atividades são então disponibilizadas por meio da linha do tempo, um recurso introduzido no Windows 10 versão 1803, que permite ao usuário voltar para o passado e retomar uma atividade que iniciados anteriormente rapidamente.

![Imagem da linha do tempo do Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

Atividades do usuário são controladas usando [Microsoft Graph](https://developer.microsoft.com/graph/). No entanto, quando você estiver criando um aplicativo do Windows 10, você não precisa interagir diretamente com os pontos de extremidade REST fornecidos pelo Microsoft Graph. Em vez disso, você pode usar um conjunto prático de APIs do WinRT. Vamos usar essas APIs do WinRT no aplicativo de despesas da Contoso para rastrear toda vez que o usuário abre uma despesa dentro do aplicativo e usar os cartões adaptáveis para permitir que os usuários criar a atividade.

### <a name="introduction-to-adaptive-cards"></a>Introdução aos cartões adaptáveis

Esta seção fornece uma visão geral dos [os cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/). Se você não precisará dessas informações, você pode ignorar essa etapa e vá diretamente para o [adicionar um cartão adaptável](#add-an-adaptive-card) instruções.

Os cartões adaptáveis permitem aos desenvolvedores trocar o conteúdo do cartão de uma maneira comum e consistente. Um cartão adaptável é descrito por uma carga JSON que define seu conteúdo, que pode incluir texto, imagens, ações e muito mais.

Um cartão adaptável define apenas o conteúdo e não a aparência visual do conteúdo. A plataforma em que o cartão adaptável é recebido pode renderizar o conteúdo usando o estilo mais apropriado. A maneira como os cartões adaptáveis são criados é por meio [um renderizador](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started), que é capaz de levar o conteúdo JSON e convertê-la para a interface do usuário nativa. Por exemplo, a interface do usuário pode ser o XAML para um WPF ou UWP aplicativo, AXML para um aplicativo do Android ou HTML para um site ou um chat bot.

Aqui está um exemplo de uma carga de cartão adaptável simple.

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

A imagem a seguir mostra como esse JSON é renderizado de diferentes maneiras por ta canal equipes, Cortana e uma notificação do Windows.

![Imagem de renderização adaptável do cartão](images/wpf-modernize-tutorial/AdaptiveCards.png)

Os cartões adaptáveis desempenham um papel importante na linha do tempo, porque ele é a maneira como o Windows processa as atividades. Cada miniatura exibida dentro da linha do tempo é, na verdade, um cartão adaptável. Dessa forma, quando você vai criar uma atividade de usuário dentro de seu aplicativo, você deverá fornecer um cartão adaptável para renderizá-lo.

> [!NOTE]
> Uma ótima maneira para debater o design de um cartão adaptável é usando [designer online](https://adaptivecards.io/designer/). Você terá a chance de criar o cartão com blocos de construção (imagens, textos, colunas, etc.) e obter o JSON correspondente. Depois de ter uma ideia do design final, você pode usar uma biblioteca chamada [os cartões adaptáveis](https://www.nuget.org/packages/AdaptiveCards/) para torná-lo mais fácil de compilar seu cartão adaptável usando C# classes em vez de JSON simples, que pode ser difícil de depurar e criar.

### <a name="add-an-adaptive-card"></a>Adicionar um cartão adaptável

1. Clique com botão direito do **ContosoExpenses.Core** do projeto no Gerenciador de soluções e escolha **gerenciar pacotes NuGet**.

2. No **Gerenciador de pacotes NuGet** janela, clique em **procurar**. Pesquise o `Newtonsoft.Json` empacotar e instalar a versão mais recente disponível. Isso é uma biblioteca popular de manipulação de JSON que você usará para ajudar a mainipulate as cadeias de caracteres JSON exigidas pelo cartões adaptáveis.

    ![Pacote do NuGet newtonsoft. JSON](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Se você não instalar o `Newtonsoft.Json` empacotar separadamente, a biblioteca os cartões adaptáveis fará referência a uma versão anterior da `Newtonsoft.Json` pacote que não dá suporte a .NET Core 3.0.

2. No **Gerenciador de pacotes NuGet** janela, clique em **procurar**. Pesquise o `AdaptiveCards` empacotar e instalar a versão mais recente disponível.

    ![Pacote de NuGet de cartões adaptável](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. Na **Gerenciador de soluções**, clique com botão direito do **ContosoExpenses.Core** do projeto, escolha **Adicionar -> classe**. Nomeie a classe **TimelineService.cs** e clique em **Okey**.

4. No **TimelineService.cs** arquivo, adicione as seguintes instruções à parte superior do arquivo.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Alterar o namespace declarado no arquivo a partir `ContosoExpenses.Core` para `ContosoExpenses`.

5. Adicione o seguinte método para o `TimelineService` classe.

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

Esse método recebe um **despesas** objeto com todas as informações sobre a despesa para renderizar e ele compila um novo **AdaptiveCard** objeto. O método adiciona o seguinte ao cartão:

- Um título, que usa a descrição da despesa.
- Uma imagem, que é o logotipo da Contoso.
- O valor da despesa.
- A data da despesa.

Os últimos 3 elementos são divididos em duas colunas diferentes, para que o logotipo da Contoso e os detalhes sobre a despesa podem ser colocados lado a lado. Depois que o objeto é criado, o método retorna a cadeia de caracteres JSON correspondente com a Ajuda do **ToJson** método.

### <a name="define-the-user-activity"></a>Definir a atividade do usuário

Agora que você definiu o cartão adaptável, você pode criar uma atividade de usuário com base nele.

1. Adicione as seguintes instruções na parte superior do **TimelineService.cs** arquivo:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Esses são os namespaces UWP. Esses resolver porque o `Microsoft.Toolkit.Wpf.UI.Controls` pacote do NuGet que você instalou na etapa 2 inclui uma referência para o `Microsoft.Windows.SDK.Contracts` do pacote, que permite que o **ContosoExpenses.Core** projeto à referência de APIs do WinRT, mesmo que seja um .NET Projeto de núcleo 3.

2. Adicione as seguintes declarações de campo para o `TimelineService` classe.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Adicione o seguinte método para o `TimelineService` classe.

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

O `AddToTimeline` método primeiro obtém um **UserActivityChannel** objeto que é necessário para armazenar as atividades do usuário. Em seguida, ele cria uma nova atividade de usuário usando o **GetOrCreateUserActivityAsync** método, que requer um identificador exclusivo. Dessa forma, se já existir uma atividade, o aplicativo pode atualizá-lo; Caso contrário, ele criará um novo. Depende do identificador para passar pelo tipo de aplicativo que você está criando:

* Se você quiser atualizar sempre a mesma atividade para que a linha do tempo mostrará apenas o mais recente, você pode usar um identificador fixado (como **despesas**).
* Se você quiser acompanhar todas as atividades como um diferente, para que a linha do tempo exibirá todos eles, você pode usar um identificador dinâmico.

Nesse cenário, o aplicativo controlará todas as despesas aberta como uma atividade de usuário diferente, por isso, o código cria cada identificador usando a palavra-chave **despesas -** seguido pela ID da despesa exclusivo.

Depois que o método cria uma **UserActivity** do objeto, ele preenche o objeto com as seguintes informações:

* Uma **ActivationUri** que é invocado quando o usuário clica na atividade na linha do tempo. O código usa um protocolo personalizado chamado **contosoexpenses** que o aplicativo serão tratadas mais tarde.
* O **VisualElements** objeto, que contém um conjunto de propriedades que definem a aparência visual da atividade. Esse código define a **DisplayText** (que é o título exibido na parte superior a entrada na linha do tempo) e o **conteúdo**. 

Isso é onde o cartão adaptável que você definiu anteriormente desempenha um papel. O aplicativo passa o cartão adaptável criado anteriormente como conteúdo para o método. No entanto, o Windows 10 usa um objeto diferente para representar um cartão em comparação comparado aquele usado pela `AdaptiveCards` pacote do NuGet. Portanto, o método recria o cartão usando o **CreateAdaptiveCardFromJson** método exposto pelo **AdaptiveCardBuilder** classe. Depois que o método cria a atividade do usuário, ele salva a atividade e cria uma nova sessão.

Quando um usuário clica em uma atividade na linha do tempo, o **contosoexpenses: / /** protocolo será ativado e a URL incluirá as informações que o aplicativo precisa recuperar a despesa selecionada. Como uma tarefa opcional, você poderia implementar a ativação de protocolo para que o aplicativo reage corretamente quando o usuário utiliza a linha do tempo.

### <a name="integrate-the-application-with-timeline"></a>Integrar o aplicativo com a linha do tempo

Agora que você criou uma classe que interage com a linha do tempo, podemos começar a usá-lo para aprimorar a experiência do aplicativo. O melhor lugar para usar o **AddToTimeline** método exposto pelo **TimelineService** classe é quando o usuário abre a página de detalhes de uma despesa.

1. No **ContosoExpenses.Core** do projeto, expanda o **ViewModels** pasta e abra o **ExpenseDetailViewModel.cs** arquivo. Esse é o ViewModel que dá suporte a janela de detalhes de despesas.

2. Localize o construtor público do **ExpenseDetailViewModel** de classe e adicione o seguinte código ao final do construtor. Sempre que a janela de despesas é aberta, o método chama o **AddToTimeline** método e passa a despesa atual. O **TimelineService** classe usa essas informações para criar uma atividade de usuário usando as informações de despesa.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Quando você terminar, o construtor deve ter esta aparência.

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

3. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário da lista e, em seguida, escolha uma despesa. Na página de detalhes, observe a descrição da despesa, a data e a quantidade.

4. Pressione **iniciar + guia** para abrir a linha do tempo.

5. Role para baixo a lista de aplicativos atualmente abertos até que você veja a seção intitulada **hoje**. Esta seção mostra algumas das atividades de usuário mais recentes. Clique no **ver todas as atividades** link ao lado de **hoje** título.

6. Confirme que você vê um novo cartão com as informações sobre a despesa que você selecionou no aplicativo.

    ![Linha do tempo de despesas de Contoso](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Se você abrir o outras despesas agora, você verá novos cartões do que está sendo adicionados como atividades do usuário. Lembre-se de que o código usa um identificador diferente para cada atividade, portanto, ele cria um cartão para todas as despesas que você abrir no aplicativo.

8. Feche o aplicativo.

## <a name="add-a-notification"></a>Adicionar uma notificação

O segundo recurso, que a equipe de desenvolvimento do Contoso deseja adicionar é uma notificação que é mostrada ao usuário sempre que uma nova despesa é salvo no banco de dados. Para fazer isso, você pode aproveitar o sistema interno de notificações no Windows 10, que é exposta aos desenvolvedores por meio de APIs do WinRT. Este sistema de notificação tem muitas vantagens:

- As notificações são consistentes com o restante do sistema operacional.
- Eles são acionáveis.
- Eles são armazenados na Central de ações para que eles podem ser analisados posteriormente.

Para adicionar uma notificação ao aplicativo:

1. Na **Gerenciador de soluções**, clique com botão direito do **ContosoExpenses.Core** do projeto, escolha **Adicionar -> classe**. Nomeie a classe **NotificationService.cs** e clique em **Okey**.

2. No **NotificationService.cs** arquivo, adicione as seguintes instruções à parte superior do arquivo.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Alterar o namespace declarado no arquivo a partir `ContosoExpenses.Core` para `ContosoExpenses`.

4. Adicione o seguinte método para o `NotificationService` classe.

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

    Notificações do sistema são representadas por uma carga de XML, que pode incluir texto, imagens, ações e muito mais. Você pode encontrar todos os elementos com suporte [aqui](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Esse código usa um esquema muito simples com duas linhas de texto: o título e o corpo. Depois que o código define a carga XML e carrega-o em uma **XmlDocument** do objeto, ele encapsula o XML em uma **ToastNotification** do objeto e mostra-la usando um o **ToastNotificationManager** classe.

5. No **ContosoExpenses.Core** do projeto, expanda o **ViewModels** pasta e abra o **AddNewExpenseViewModel.cs** arquivo. 

6. Localize o `SaveExpenseCommand` método, que é disparado quando o usuário pressiona o botão para salvar uma nova despesa. Adicione o código a seguir para esse método, logo após a chamada para o `SaveExpense` método.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Quando você terminar, o `SaveExpenseCommand` método deve ter esta aparência.

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

7. Pressione F5 para compilar e executar o aplicativo no depurador. Escolha um funcionário da lista e, em seguida, clique no **adicionar novo despesa** botão. Preencha todos os campos no formulário e pressione **salvar**.

8. Você receberá a seguinte exceção.

    ![Erro de notificação do sistema](images/wpf-modernize-tutorial/ToastNotificationError.png)

Essa exceção é causada pelo fato de que o aplicativo de despesas de Contoso ainda não tem a identidade do pacote. Algumas APIs do WinRT, incluindo as notificações de API, exigem a identidade do pacote antes que possam ser usados em um aplicativo. Aplicativos UWP recebem a identidade do pacote por padrão porque elas só podem ser distribuídas por meio de pacotes MSIX. Outros tipos de aplicativos do Windows, incluindo aplicativos do WPF, também podem ser implantados por meio de pacotes MSIX para obter a identidade do pacote. A próxima parte deste tutorial explorará como fazer isso.

## <a name="next-steps"></a>Próximas etapas

Neste ponto no tutorial, você adicionou com êxito uma atividade de usuário para o aplicativo que se integra a linha do tempo do Windows, e você também adicionou uma notificação para o aplicativo que é disparado quando os usuários criam uma nova despesa. No entanto, a notificação ainda não funciona porque o aplicativo exigir uma identidade do pacote para usar as notificações de API. Para saber como compilar um pacote MSIX ao obter a identidade do pacote e obter outros benefícios de implantação do aplicativo, consulte [parte 5: Empacotar e implantar com MSIX](modernize-wpf-tutorial-5.md).
