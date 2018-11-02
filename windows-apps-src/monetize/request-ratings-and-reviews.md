---
author: Xansky
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: Solicite classificações e opiniões para seu aplicativo
ms.author: mhopkins
ms.date: 06/15/2018
ms.topic: article
keywords: Windows 10, uwp, classificações, opiniões
ms.localizationpriority: medium
ms.openlocfilehash: d736fa47251c85491a29b324a3ed59181a5060c8
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5935090"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Solicite classificações e opiniões para seu aplicativo

Você pode adicionar código ao seu aplicativo da Plataforma Universal do Windows (UWP) para solicitar programaticamente que os clientes classifiquem ou analisem seu aplicativo. Há diversas maneiras de fazer isso:
* Você pode mostrar um diálogo de classificação e opinião diretamente no contexto de seu aplicativo.
* Programaticamente, você pode abrir a página de classificação e opinião do seu app na Microsoft Store.

Quando você estiver pronto para analisar seus dados de classificações e opiniões, poderá exibir os dados no painel do Centro de Desenvolvimento do Windows ou usar a API de análise de Microsoft Store para recuperar esses dados programaticamente.

> [!IMPORTANT]
> Ao adicionar uma função de classificação dentro de seu aplicativo, todas as análises devem enviar o usuário para mecanismos de classificação da loja, independentemente de estrela escolhida. Se você coletar comentários ou comentários dos usuários, ele deve ser claro que ele não está relacionado à classificação do aplicativo ou críticas na loja, mas é enviado diretamente para o desenvolvedor do aplicativo. Consulte o código desenvolvedor de conduta para obter mais informações relacionadas à [Fraudulent ou atividades desonestos](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Mostrar um diálogo de classificação e opinião em seu app

Para mostrar programaticamente um diálogo do seu app que peça ao cliente para classificar seu app e enviar uma opinião, chame o método [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Passe o inteiro 16 para o parâmetro *requestKind* e uma sequência para o parâmetro *parametersAsJson* como mostrado neste exemplo de código. Este exemplo requer a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft e requer instruções using para os namespaces **Windows.Services.Store**, **System.Threading.Tasks** e **Newtonsoft.Json.Linq**.

> [!IMPORTANT]
> A solicitação para mostrar o diálogo de classificação e opinião deve ser chamada no thread da interface do usuário em seu app.

```csharp
public async Task<bool> ShowRatingReviewDialog()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 16, String.Empty);

    if (result.ExtendedError == null)
    {
        JObject jsonObject = JObject.Parse(result.Response);
        if (jsonObject.SelectToken("status").ToString() == "success")
        {
            // The customer rated or reviewed the app.
            return true;
        }
    }

    // There was an error with the request, or the customer chose not to
    // rate or review the app.
    return false;
}
```

O método **SendRequestAsync** usa um sistema simples de solicitação baseadas em inteiros e parâmetros de dados baseados em JSON para expor diversas operações da Store para apps. Quando você passa o inteiro 16 para o parâmetro *requestKind*, emite uma solicitação para mostrar o diálogo de classificação e opiniões e envia os dados relacionados para a Store. Esse método foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Para obter uma visão geral desse método, consulte [Enviar solicitações para a Store](send-requests-to-the-store.md).

### <a name="response-data-for-the-rating-and-review-request"></a>Dados de resposta para a solicitação de classificação e opinião

Depois de enviar essa solicitação para exibir o diálogo de classificação e opinião, a propriedade [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) do valor de retorno [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contém uma sequência formatada em JSON que indica se a solicitação obteve êxito.

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

|  Campo  |  Descrição  |
|----------------------|---------------|
|  *status*                   |  Uma sequência que indica se o cliente enviou com êxito uma classificação ou opinião. Os valores com suporte são **success** e **aborted**.   |
|  *data*                   |  Um objeto que contém um único valor booliano chamado *updated*. Este valor indica se o cliente atualizou uma classificação ou opinião existente. O objeto *data* é incluído somente em respostas de êxito.   |
|  *errorDetails*                   |  Uma sequência que contém os detalhes do erro para a solicitação. |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Inicie a página de classificação e opinião para seu app na Store

Se você quiser abrir programaticamente a página de classificação e opinião para seu app na Store, poderá usar o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) com o esquema ```ms-windows-store://review``` URI como demonstrado neste exemplo de código.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Para obter mais informações, consulte [Iniciar o app da Microsoft Store](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analisar seus dados de classificações e opiniões

Para analisar os dados de classificações e opiniões de seus clientes, você tem várias opções:
* Você pode usar o relatório [Opiniões](../publish/reviews-report.md) no painel do Centro de Desenvolvimento do Windows para ver classificações e opiniões de seus clientes. Você também pode baixar esse relatório para exibi-lo offline.
* Você pode usar os métodos [Obter classificações do app](get-app-ratings.md) e [Obter opiniões do app](get-app-reviews.md) na API de análise da Store para recuperar programaticamente as classificações e opiniões de seus clientes no formato JSON.

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar solicitações para a Store](send-requests-to-the-store.md)
* [Iniciar o aplicativo da Microsoft Store](../launch-resume/launch-store-app.md)
* [Relatório de opiniões](../publish/reviews-report.md)
