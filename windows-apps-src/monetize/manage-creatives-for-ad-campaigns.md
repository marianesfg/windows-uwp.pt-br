---
author: mcleanbyron
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: "Use este método na API de promoções da Windows Store para gerenciar criativos de campanhas publicitárias promocionais."
title: "Gerenciar criativos de campanhas publicitárias"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de promoções da Windows Store, campanhas publicitárias"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1e0755134a47b6acfb48f735ea56c4aa3c46be14
ms.lasthandoff: 02/08/2017

---

# <a name="manage-creatives-for-ad-campaigns"></a>Gerenciar criativos de campanhas publicitárias

Use esses métodos na API de promoções da Windows Store a fim de carregar seus próprios criativos personalizados para usar em campanhas publicitárias promocionais ou obter um criativo existente. Um criativo pode ser associado a uma ou mais linhas de entrega, mesmo em campanhas publicitárias, considerando sempre representa o mesmo app.

Para saber mais sobre a relação entre criativos e campanhas publicitárias, linhas de entrega e perfis de direcionamento, consulte [Veicular campanhas publicitárias usando serviços da Windows Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Pré-requisitos

Para usar esses métodos, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](run-ad-campaigns-using-windows-store-services.md#prerequisites) da API de promoções da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usar no cabeçalho da solicitação desses métodos. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esses métodos têm os seguintes URIs.

| Tipo de método | URI da solicitação     |  Descrição  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Gera um novo criativo.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Obtém o criativo especificado pelo *creativeId*.  |

>**Observação**&nbsp;&nbsp;Esta API não suporta um método PUT no momento.

<span/> 
### <a name="header"></a>Cabeçalho

| Cabeçalho        | Tipo   | Descrição         |
|---------------|--------|---------------------|
| Autorização | cadeia | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |
| ID de rastreamento   | GUID   | Opcional. Uma ID que rastreia o fluxo de chamada.                                  |


<span/>
### <a name="request-body"></a>Corpo da solicitação

O método POST requer um corpo da solicitação JSON com os campos obrigatórios de um objeto [Criativo](#creative).

<span/>
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

<span/>
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
|  nome   |  cadeia   |   O nome do criativo.    |    Não   |      |  Sim     |       
|  content   |  cadeia   |  O conteúdo da imagem do criativo, no formato codificado de Base64.     |  Não     |      |   Sim    |       
|  height   |  número inteiro   |   A altura do criativo.    |    Não    |      |   Sim    |       
|  width   |  número inteiro   |  A largura do criativo.     |  Não    |     |    Sim   |       
|  landingUrl   |  cadeia   |  O URL inicial do creative (esse valor deve ser um URI válido).     |  Não    |     |   Sim    |       
|  format   |  cadeia   |   O formato da publicidade. Atualmente, o único valor com suporte é **Faixa de notificação**.    |   Não    |  Faixa   |  Não     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   Fornece atributos para o criativo.     |   Não    |      |   Sim    |       
|  storeProductId   |  cadeia   |   A [ID da loja](in-app-purchases-and-trials.md#store-ids) do app ao qual a campanha publicitária está associada. Um exemplo de ID da loja para um produto é 9nblggh42cfd.    |   Não    |    |  Não     |   |  

<span id="image-attributes"/>
## <a name="imageattributes-object"></a>Objeto de ImageAttributes

| Campo        | Tipo   |  Descrição      |  Somente leitura  | Valor padrão  | Obrigatório para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   cadeia  |   A extensão da imagem (como PNG ou JPG).    |    Não   |      |   Sim    |       |


## <a name="related-topics"></a>Tópicos relacionados

* [Veicular campanhas publicitárias usando os serviços da Windows Store](run-ad-campaigns-using-windows-store-services.md)
* [Gerenciar campanhas publicitárias](manage-ad-campaigns.md)
* [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Obter dados de desempenho da campanha publicitária](get-ad-campaign-performance-data.md)

