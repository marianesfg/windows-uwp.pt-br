---
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: Use este método na API de promoções da Microsoft Store para gerenciar criativos de campanhas publicitárias promocionais.
title: Gerenciar criativos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de promoções da Microsoft Store, campanhas publicitárias
ms.localizationpriority: medium
ms.openlocfilehash: 41c11ee9c5decffff57a2d443e1385398ce40d89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658461"
---
# <a name="manage-creatives"></a>Gerenciar criativos

Use esses métodos na API de promoções da Microsoft Store a fim de carregar seus próprios criativos personalizados para usar em campanhas publicitárias promocionais ou obter um criativo existente. Um criativo pode ser associado a uma ou mais linhas de entrega, mesmo em campanhas publicitárias, considerando sempre representa o mesmo app.

Para saber mais sobre a relação entre criativos e campanhas publicitárias, linhas de entrega e perfis de direcionamento, consulte [Veicular campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

> [!NOTE]
> Ao usar essa API para carregar seu próprios criativos, o tamanho máximo permitido para o criativo será de 40 KB. Se você enviar um arquivo criativo maior do que isso, essa API não retornará um erro, mas a campanha não será criada com êxito.

## <a name="prerequisites"></a>Pré-requisitos

Para usar esses métodos, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](run-ad-campaigns-using-windows-store-services.md#prerequisites) da API de promoções da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usar no cabeçalho da solicitação desses métodos. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.


## <a name="request"></a>Solicitação

Esses métodos têm os seguintes URIs.

| Tipo de método | URI da solicitação     |  Descrição  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Gera um novo criativo.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Obtém o criativo especificado pelo *creativeId*.  |

> [!NOTE]
> No momento, esta API não dá suporte a um método PUT.


### <a name="header"></a>Cabeçalho

| Cabeçalho        | Tipo   | Descrição         |
|---------------|--------|---------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |
| ID de rastreamento   | GUID   | Opcional. Uma ID que rastreia o fluxo de chamada.                                  |


### <a name="request-body"></a>Corpo da solicitação

O método POST requer um corpo da solicitação JSON com os campos obrigatórios de um objeto [Criativo](#creative).


### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como chamar o método POST para gerar um criativo. Neste exemplo, o valor *conteúdo* foi reduzido por motivos de redução.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

O exemplo a seguir demonstra como chamar o método GET para recuperar um criativo.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Resposta

Esses métodos retornam um corpo de resposta JSON com um objeto [Criativo](#creative) que contém informações sobre o criativo criado ou recuperado. O exemplo a seguir demonstra um corpo de resposta para esses métodos. Neste exemplo, o valor *conteúdo* foi reduzido por motivos de redução.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>Objeto criativo

O corpo da solicitação e resposta desses métodos contêm os campos a seguir. Essa tabela mostra quais campos são somente de leitura (ou seja, eles não podem ser alterados no método PUT) e quais são necessários no corpo da solicitação para o método POST.

| Campo        | Tipo   |  Descrição      |  Somente leitura  | Padrão  |  Obrigatório para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número inteiro   |  A ID do criativo.     |   Sim    |      |    Não   |       
|  name   |  cadeia de caracteres   |   O nome do criativo.    |    Não   |      |  Sim     |       
|  content   |  cadeia de caracteres   |  O conteúdo da imagem do criativo, no formato codificado de Base64.<br/><br/>**Observação**&nbsp;&nbsp;o número máximo permitido de tamanho para o creative é de 40 KB. Se você enviar um arquivo criativo maior do que isso, essa API não retornará um erro, mas a campanha não será criada com êxito.     |  Não     |      |   Sim    |       
|  height   |  número inteiro   |   A altura do criativo.    |    Não    |      |   Sim    |       
|  width   |  número inteiro   |  A largura do criativo.     |  Não    |     |    Sim   |       
|  landingUrl   |  cadeia de caracteres   |  Se você estiver usando uma campanha de rastreamento de serviço como Kochava, AppsFlyer ou Tune para medir análises de instalação do seu aplicativo, atribua a URL de rastreamento nesse campo quando você chama o método POST (se especificado, esse valor deve ser um URI válido). Se você não estiver usando uma serviço de rastreamento de campanha, ao omitir esse valor quando você chama o método POST (nesse caso, a URL será criada automaticamente).   |  Não    |     |   Sim    |       
|  format   |  cadeia de caracteres   |   O formato da publicidade. Atualmente, o único valor com suporte é **Faixa de notificação**.    |   Não    |  Faixa   |  Não     |       
|  imageAttributes   | [imageAttributes](#image-attributes)    |   Fornece atributos para o criativo.     |   Não    |      |   Sim    |       
|  storeProductId   |  cadeia de caracteres   |   A [ID da loja](in-app-purchases-and-trials.md#store-ids) do app ao qual a campanha publicitária está associada. Um exemplo de ID da loja para um produto é 9nblggh42cfd.    |   Não    |    |  Não     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>Objeto de ImageAttributes

| Campo        | Tipo   |  Descrição      |  Somente leitura  | Valor padrão  | Obrigatório para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   cadeia de caracteres  |   Um dos seguintes valores: **PNG** ou **JPG**.    |    Não   |      |   Sim    |       |


## <a name="related-topics"></a>Tópicos relacionados

* [Executar campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gerenciar campanhas publicitárias](manage-ad-campaigns.md)
* [Gerenciar linhas de entrega para campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar perfis de direcionamento para campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Obter dados de desempenho de campanha de anúncio](get-ad-campaign-performance-data.md)
