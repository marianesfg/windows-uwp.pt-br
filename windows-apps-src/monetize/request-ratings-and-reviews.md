---
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: Solicite classificações e opiniões para seu aplicativo
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10, uwp, classificações, opiniões
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: 7a1899358cd5ce9d2f9fa1bd174a123740f98e7a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042632"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Solicite classificações e opiniões para seu aplicativo

Você pode adicionar código ao seu aplicativo da Plataforma Universal do Windows (UWP) para solicitar programaticamente que os clientes classifiquem ou analisem seu aplicativo. Há diversas maneiras de fazer isso:
* Você pode mostrar um diálogo de classificação e opinião diretamente no contexto de seu aplicativo.
* Programaticamente, você pode abrir a página de classificação e opinião do seu app na Microsoft Store.

Quando você estiver pronto para analisar seus dados de classificações e críticas, você pode exibir os dados no Partner Center ou usar a API de análise da Microsoft Store para recuperar esses dados programaticamente.

> [!IMPORTANT]
> Ao adicionar uma função de classificação dentro de seu aplicativo, todas as análises devem enviar o usuário para mecanismos de classificação da loja, independentemente de estrela escolhida. Se você coletar comentários ou comentários dos usuários, ele deve ser claro que ele não está relacionado a classificação do aplicativo ou críticas na loja, mas é enviado diretamente para o desenvolvedor do aplicativo. Consulte o código desenvolvedor de conduta para obter mais informações relacionadas à [Fraudulent ou atividades desonestos](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Mostrar um diálogo de classificação e opinião em seu app

Para mostrar programaticamente uma caixa de diálogo do seu app que peça ao cliente para classificar seu app e enviar uma opinião, chame o método [RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) . 

> [!IMPORTANT]
> A solicitação para mostrar o diálogo de classificação e opinião deve ser chamada no thread da interface do usuário em seu app.

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

O método **RequestRateAndReviewAppAsync** foi introduzido no Windows 10, versão 1809, e pode ser usada somente em projetos **Windows 10 de outubro de 2018 Update (10.0; Compilação 17763)** ou uma versão posterior no Visual Studio.

### <a name="response-data-for-the-rating-and-review-request"></a>Dados de resposta para a solicitação de classificação e opinião

Depois de enviar a solicitação para exibir a classificação e examine a caixa de diálogo, a propriedade [ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) da classe [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) contém uma cadeia de caracteres formatada em JSON que indica se a solicitação foi bem-sucedida.

O exemplo a seguir demonstra o valor de retorno para essa solicitação depois que o cliente envia com êxito uma classificação ou opinião.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

O exemplo a seguir demonstra o valor de retorno para essa solicitação depois que o cliente opta por não enviar uma classificação ou opinião.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

A tabela a seguir descreve os campos na sequência de dados formatados em JSON.

| Campo          | Descrição                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *status*       | Uma sequência que indica se o cliente enviou com êxito uma classificação ou opinião. Os valores com suporte são **success** e **aborted**. |
| *data*         | Um objeto que contém um único valor booliano chamado *updated*. Este valor indica se o cliente atualizou uma classificação ou opinião existente. O objeto *data* é incluído somente em respostas de êxito. |
| *errorDetails* | Uma sequência que contém os detalhes do erro para a solicitação.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Inicie a página de classificação e opinião para seu app na Store

Se você quiser abrir programaticamente a página de classificação e opinião para seu app na Store, poderá usar o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) com o esquema ```ms-windows-store://review``` URI como demonstrado neste exemplo de código.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Para obter mais informações, consulte [Iniciar o app da Microsoft Store](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analisar seus dados de classificações e opiniões

Para analisar os dados de classificações e opiniões de seus clientes, você tem várias opções:
* Você pode usar o relatório [de análises](../publish/reviews-report.md) no Partner Center para ver as classificações e opiniões de seus clientes. Você também pode baixar esse relatório para exibi-lo offline.
* Você pode usar os métodos [Obter classificações do app](get-app-ratings.md) e [Obter opiniões do app](get-app-reviews.md) na API de análise da Store para recuperar programaticamente as classificações e opiniões de seus clientes no formato JSON.

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar solicitações para a Store](send-requests-to-the-store.md)
* [Iniciar o aplicativo da Microsoft Store](../launch-resume/launch-store-app.md)
* [Relatório de opiniões](../publish/reviews-report.md)
