---
author: mcleanbyron
Description: "Você pode usar o método SendRequestAsync a fim de enviar solicitações para a Windows Store para operações que ainda não têm uma API disponível no SDK do Windows."
title: "Enviar solicitações para Windows Store"
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.openlocfilehash: a949b3c93bb5b4a056f9e1fa4679e8ddca05d899
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2017
---
# <a name="send-requests-to-the-windows-store"></a>Enviar solicitações para Windows Store

A partir do Windows 10, versão 1607, o SDK do Windows fornece APIs para operação relacionadas à Loja (como compras no aplicativo) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). No entanto, embora os serviços que oferecem suporte à Loja sejam constantemente atualizados, ampliados e melhorados entre as versões do sistema operacional, normalmente, as novas APIs são adicionadas ao SDK do Windows somente nas versões principais do sistema operacional.

Nós fornecemos o método [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_) como uma maneira flexível de disponibilizar novas operações da Loja para aplicativos da UWP (Plataforma Universal do Windows) antes do lançamento da nova versão do SDK do Windows. Você pode usar esse método a fim de enviar solicitações à Loja para novas operações que ainda não têm uma API correspondente disponível na versão mais recente do SDK do Windows.

> [!NOTE]
> O método **SendRequestAsync** está disponível somente para aplicativos destinados ao Windows 10, versão 1607 ou posterior. Algumas das solicitações de suporte por esse método têm suporte apenas nas versões após o Windows 10, versão 1607.

**SendRequestAsync** é um método estático da classe [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper). Para chamar esse método, você deve passar as seguintes informações para esse método:
* O objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) que fornece informações sobre o usuário para o qual você deseja executar a operação. Para saber mais sobre esse objeto, consulte [Introdução à classe StoreContext](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Um inteiro que identifica a solicitação que você deseja enviar para a loja.
* Se a solicitação oferece suporte a argumentos, você também pode passar uma cadeia de caracteres formatada em JSON com os argumentos a serem transmitidos juntamente com a solicitação.

O exemplo a seguir demonstra como chamar esse método. Este exemplo requer o uso de instruções para os namespaces **Windows.Services.Store** e **System.Threading.Tasks**.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

Consulte as seções a seguir para obter informações sobre as solicitações que estão disponíveis no momento pelo método **SendRequestAsync**. Atualizaremos este artigo quando o suporte para novas solicitações for adicionado.

## <a name="requests-for-flight-group-scenarios"></a>Solicitações para cenários de grupo de versão de pré-lançamento

> [!IMPORTANT]
> No momento, todas as solicitações de grupo de versão de pré-lançamento descritas nesta seção não estão disponíveis para a maioria das contas de desenvolvedor. Ocorre falha nessas solicitações, exceto quando sua conta de desenvolvedor seja especialmente provisionada pela Microsoft.

O método **SendRequestAsync** oferece suporte a um conjunto de solicitações para cenários de grupo de versão de pré-lançamento, como a adição de um usuário ou dispositivo a um grupo de versão de pré-lançamento. Para enviar essas solicitações, passe o valor 7 ou 8 para o parâmetro *requestKind* juntamente com uma cadeia de caracteres formatada em JSON para o parâmetro *parametersAsJson* que indica a solicitação a qual você deseja enviar juntamente com os argumentos relacionados. Esses valores *requestKind* diferem das seguintes maneiras.

|  Valor de tipo de solicitação  |  Descrição  |
|----------------------|---------------|
|  7                   |  As solicitações são executadas no contexto do dispositivo atual. Esse valor pode ser usado somente no Windows 10, versão 1703 ou posterior.  |
|  8                   |  As solicitações são executadas no contexto do usuário que fez logon na Loja. Esse valor pode ser usado no Windows 10, versão 1607 ou posterior.  |

As solicitações de grupo de versão de pré-lançamento a seguir são implementadas no momento.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Recuperar variáveis remotas para o grupo de versão de pré-lançamento melhor classificado

> [!IMPORTANT]
> No momento, essa solicitação não está disponível para a maioria das contas de desenvolvedor. Ocorre falha dessa solicitação, exceto quando sua conta de desenvolvedor seja especialmente provisionada pela Microsoft.

Essa solicitação recupera as variáveis do grupo de versão de pré-lançamento melhor classificado para o usuário ou o dispositivo atual. Para enviar essa solicitação, passe as seguintes informações para os parâmetros *requestKind* e *parametersAsJson* do método **SendRequestAsync**.

|  Parâmetro  |  Descrição  |
|----------------------|---------------|
|  *requestKind*                   |  Especifique 7 para retornar o grupo de versão de pré-lançamento melhor classificado para o dispositivo ou 8 para retornar o grupo de versão de pré-lançamento melhor classificado para o usuário e o dispositivo atual. É recomendável usar o valor 8 para o parâmetro *requestKind*, pois esse valor retornará o grupo de versão de pré-lançamento melhor classificado em todos os membros para o usuário e o dispositivo atual.  |
|  *parametersAsJson*                   |  Passe uma cadeia de caracteres formatada em JSON que contém os dados mostrados no exemplo a seguir.  |

O exemplo a seguir mostra o formato dos dados JSON para passar a *parametersAsJson*. O campo *type* deve ser atribuído à cadeia de caracteres *GetRemoteVariables*. Atribuir o campo *projectId* à ID do projeto no qual você definiu as variáveis remotas no painel do Centro de Desenvolvimento do Windows.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Depois de enviar essa solicitação, a propriedade [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_Response) do valor de retorno [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contém uma cadeia de caracteres formatada em JSON com os campos a seguir.

|  Campo  |  Descrição  |
|----------------------|---------------|
|  *anonymous*                   |  Um valor Booleano, onde **true** indica que a identidade do usuário ou do dispositivo não estava presente na solicitação, e **false** indica que a identidade do usuário ou dispositivo estava presente na solicitação.  |
|  *name*                   |  Uma cadeia de caracteres que contém o nome do grupo de versão de pré-lançamento melhor classificado ao qual o dispositivo ou usuário pertence.  |
|  *settings*                   |  Um dicionário de pares de chave/valor que contêm o nome e o valor das variáveis remotas que o desenvolvedor configurou para o grupo de versão de pré-lançamento.  |

O exemplo a seguir demonstra um valor de retorno para essa solicitação.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Adicionar o dispositivo ou o usuário atual a um grupo de versão de pré-lançamento

> [!IMPORTANT]
> No momento, essa solicitação não está disponível para a maioria das contas de desenvolvedor. Ocorre falha dessa solicitação, exceto quando sua conta de desenvolvedor seja especialmente provisionada pela Microsoft.

Para enviar essa solicitação, passe as seguintes informações para os parâmetros *requestKind* e *parametersAsJson* do método **SendRequestAsync**.

|  Parâmetro  |  Descrição  |
|----------------------|---------------|
|  *requestKind*                   |  Especifique 7 para adicionar o dispositivo a um grupo de versão de pré-lançamento ou 8 para adicionar o usuário conectado no momento à Loja a um grupo de versão de pré-lançamento.  |
|  *parametersAsJson*                   |  Passe uma cadeia de caracteres formatada em JSON que contém os dados mostrados no exemplo a seguir.  |

O exemplo a seguir mostra o formato dos dados JSON para passar a *parametersAsJson*. O campo *type* deve ser atribuído à cadeia de caracteres *AddToFlightGroup*. Atribua o campo *flightGroupId* à ID do grupo de versão de pré-lançamento à qual você deseja adicionar o dispositivo ou usuário.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Se houver um erro com a solicitação, a propriedade [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode) do valor de retorno [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contém o código de resposta.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Remova o dispositivo atual ou o usuário de um grupo de versão de pré-lançamento

> [!IMPORTANT]
> No momento, essa solicitação não está disponível para a maioria das contas de desenvolvedor. Ocorre falha dessa solicitação, exceto quando sua conta de desenvolvedor seja especialmente provisionada pela Microsoft.

Para enviar essa solicitação, passe as seguintes informações para os parâmetros *requestKind* e *parametersAsJson* do método **SendRequestAsync**.

|  Parâmetro  |  Descrição  |
|----------------------|---------------|
|  *requestKind*                   |  Especifique 7 para remover o dispositivo de um grupo de versão de pré-lançamento ou 8 para remover o usuário conectado no momento à Loja de um grupo de versão de pré-lançamento.  |
|  *parametersAsJson*                   |  Passe uma cadeia de caracteres formatada em JSON que contém os dados mostrados no exemplo a seguir.  |

O exemplo a seguir mostra o formato dos dados JSON para passar a *parametersAsJson*. O campo *type* deve ser atribuído à cadeia de caracteres *RemoveFromFlightGroup*. Atribua o campo *flightGroupId* à ID do grupo de versão de pré-lançamento a partir da qual você deseja remover o dispositivo ou usuário.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Se houver um erro com a solicitação, a propriedade [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode) do valor de retorno [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contém o código de resposta.

## <a name="related-topics"></a>Tópicos relacionados

* [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_)
