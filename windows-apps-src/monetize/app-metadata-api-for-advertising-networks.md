---
author: mcleanbyron
description: "Saiba como usar a API REST de metadados de app para acessar determinados metadados de tipos de app. Essa API destina-se a ser usada por redes de publicidade para recuperar informações sobre os aplicativos da Windows Store para que elas possam aprimorar a venda do espaço de anúncio para anunciantes."
title: API de metadados de app para redes de publicidade
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, rede de publicidade, metadados do app
ms.assetid: f0904086-d61f-4adb-82b6-25968cbec7f3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8ee555d335007a09c61247a929705aa2fff2469d
ms.lasthandoff: 02/08/2017

---

# <a name="app-metadata-api-for-advertising-networks"></a>API de metadados de app para redes de publicidade

As redes de anúncio podem usar a *API de metadados de app* para recuperar de forma programática metadados sobre os aplicativos da Windows Store, incluindo detalhes como a descrição e categoria para a listagem da Loja do app e se o app é direcionado para crianças menores de 13 anos. O acesso a essa API atualmente é restrito aos desenvolvedores que recebem permissão para a API pela Microsoft.

Este artigo fornece instruções sobre como solicitar acesso à API usando o [portal da API de metadados de app](https://admetadata.portal.azure-api.net/), como obter sua chave de assinatura para acessar a API e como chamar a API.

## <a name="request-access"></a>Solicitar acesso

As redes de anúncio podem solicitar acesso à API de metadados de app seguindo estas instruções:

1. Vá para a página [https://admetadata.portal.azure-api.net/signup](https://admetadata.portal.azure-api.net/signup) do portal da API de metadados de app.
2. Insira as informações necessárias e clique no botão **Inscrever-se**.
3. No mesmo site, clique na guia **Produtos** e, em seguida, clique em **Detalhes do app para publicidade**.
4. Na próxima página, clique no botão **Assinar**. Isso envia sua solicitação de acesso à API de metadados de app para a Microsoft.

Depois que sua solicitação for enviada, você receberá um email em aproximadamente 24 horas que avisa se sua solicitação foi concedida ou negada.

<span id="get-key" />
## <a name="get-your-subscription-key"></a>Obter sua chave de assinatura

Se você recebem acesso à API de metadados de app, siga estas instruções para obter sua chave de assinatura. Você deve transmitir essa chave no cabeçalho da solicitação de chamadas à API.

1. Vá para a página [https://admetadata.portal.azure-api.net/signin](https://admetadata.portal.azure-api.net/signin) do portal da API de metadados de app e entre com seu email e senha.
2. Clique em seu nome no canto superior direito do site e, em seguida, clique em **Perfil**.
3. Na seção **Suas assinaturas** da página, clique em **Mostrar** próximo a **Chave primária**. Isso é sua chave de assinatura. Copie a chave para que você possa usá-la mais tarde ao chamar a API.

<span id="call-the-api" />
## <a name="call-the-api"></a>Chamar a API

Depois de ter sua chave de assinatura, você estará pronto para chamar a API usando a sintaxe de HTTP REST da linguagem de programação de sua preferência. Para obter informações sobre a sintaxe da API, consulte a seção [Sintaxe da API](#syntax) abaixo. Para ver exemplos de código em C#, JavaScript, Python e várias outras linguagens, clique na guia **APIs** do portal da API de metadados de app, clique em **Detalhes do app** e, em seguida, veja a seção **Exemplos de código** na parte inferior da página.

Como alternativa, você pode chamar a API usando a interface do usuário fornecida pelo portal da API de metadados de app:
  1. No portal, clique na guia **APIs** e, em seguida, clique em **Detalhes do app**.
  2. Na página seguinte, insira o [app_id](#request-parameters) do app para o qual você deseja recuperar metadados no campo **app_id** e digite sua chave de assinatura no campo **Ocp_Apim_Subscription-Key**.
  3. Clique em **Enviar**. A resposta é exibida na parte inferior da página.


<span id="syntax" />
## <a name="api-syntax"></a>Sintaxe da API

Esse método tem a seguinte sintaxe de solicitação.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://admetadata.azure-api.net/v1/app/{app_id}``` |

<span/>
 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Ocp-Apim-Subscription-Key | string | Obrigatório. A chave de assinatura que você [recuperou no portal da API de metadados de app](#get-key).  |

<span/>

### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------|
| app_id | string | Obrigatório. A ID do app para o qual você deseja recuperar metadados. Ele pode ter um dos seguintes valores:<br/><br/><ul><li>A ID da Loja do app. Uma ID da Loja de exemplo é 9NBLGGH29DM8.</li><li>A ID do produto (também chamada de *ID do app*) para um app que foi criado originalmente para o Windows 8.x ou Windows Phone 8.x. A ID do produto é um GUID.</li></ul> |

<span/>

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como recuperar metadados para um app que tem a ID da Loja 9NBLGGH29DM8.

```syntax
GET https://admetadata.azure-api.net/v1/app/9NBLGGH29DM8 HTTP/1.1
Ocp-Apim-Subscription-Key: <subscription key>
```

### <a name="response-body"></a>Corpo da resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método.

```json
{
    "storeId":"9NBLGGH29DM8",
    "name":"Contoso Sports App",
    "description":"Catch the latest scores and replays using Contoso Sports App!",
    "phoneStoreGuid":"920217d7-90da-4019-99e8-46e4a6bd2594",
    "windowsStoreGuid":"d7e982e7-fbf3-42b5-9dad-72b65bd4e248",
    "storeCategory":"Sports",
    "iabCategory":"Sports",
    "iabCategoryId":"IAB17",
    "coppa":false,
    "downloadUrl":"https://www.microsoft.com/store/apps/9nblggh29dm8","isLive":true,
    "iconUrls":["//store-images.microsoft.com/image/apps.15753.13510798883747357.b6981489-6fdd-4fec-b655-a822247d5a76.33529096-a723-4204-9da9-a5dd8b328d4e"],
    "type":"App"
}
```

Para obter mais detalhes sobre os valores no corpo da resposta, veja a tabela a seguir.

| Valor      | Tipo   | Descrição    |
|------------|--------|--------------------|
| storeId           | string  | A ID da Loja do app. Uma ID da Loja de exemplo é 9NBLGGH29DM8.     |  
| name           | string  | O nome do app.   |
| description           | string  | A descrição da listagem da Loja para o app.  |
| phoneStoreGuid           | string  | A ID do produto (Windows Phone 8.x) para o app. É um GUID.  |
| windowsStoreGuid           | string  | A ID do produto (Windows 8.x) para o app. É um GUID. |
| storeCategory           | string  | A categoria do app na Loja. Para os valores aceitos, consulte a [tabela de categoria e subcategoria](../publish/category-and-subcategory-table.md) de apps na Loja.  |
| iabCategory           | string  | A categoria de conteúdo do app, conforme definido pelo Interactive Advertising Bureau (IAB). Por exemplo, **Notícias** ou **Esportes**. Para obter uma lista de categorias de conteúdo, consulte a página [IAB Tech Lab Content Taxonomy](https://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy) no site do IAB.   |
| iabCategoryId           | string  | A ID da categoria de conteúdo do app. Por exemplo, **IAB12** é a ID da categoria Notícias e **IAB17** é a ID da categoria Esportes. Para obter uma lista de IDs de categoria de conteúdo, consulte a seção 5.1 da [Especificação da API OpenRTB](http://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf). |
| coppa           | booleano  | True se o app for direcionado a crianças com menos de 13 anos e, portanto, tem obrigações segundo a Children's Online Privacy Protection Act (COPPA). Caso contrário, false.  |
| downloadUrl           | string  | O link para a listagem do app na Loja. Este link está no formato ```https://www.microsoft.com/store/apps/<Store ID>```.  |
| iconUrls           | string  |  O caminho relativo aos URLs de ícone associados a esse app. Para recuperar os ícones, coloque *http* ou *https* como prefixo nos URLs.  |
| type           | string  | Uma das seguintes strings: **Aplicativo** ou **Jogo**.  |

<span/>



 

 

