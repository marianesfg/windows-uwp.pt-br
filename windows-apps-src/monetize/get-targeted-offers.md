---
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: Use que este método na Microsoft Store direcionado oferece uma API para obter as ofertas de destino que estão disponíveis para o usuário atual no contexto do aplicativo atual.
title: Receber as ofertas direcionadas
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, serviços de Store, API de ofertas direcionadas para a Microsoft Store, obter ofertas direcionadas
ms.localizationpriority: medium
ms.openlocfilehash: 71cd6ce3b9736b812f8ccdf4d21d35357928c63c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622761"
---
# <a name="get-targeted-offers"></a>Receber as ofertas direcionadas

Use este método para obter as ofertas segmentadas disponíveis para o usuário atual, com base no fato de o usuário fazer parte do segmento de clientes ou não para a oferta segmentada. Para obter mais informações, consulte [Gerenciar ofertas direcionadas usando serviços da Loja](manage-targeted-offers-using-windows-store-services.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar esse método, você precisa primeiro [obter um token de conta da Microsoft](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token) para o usuário atual conectado do seu aplicativo. Você deve passar esse token no cabeçalho de solicitação ```Authorization``` para esse método. Esse código é usado pela Store para obter ofertas direcionadas para o usuário atual.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição  |
|---------------|--------|--------------|
| Autorização | cadeia de caracteres | Obrigatório. O token Account da Microsoft para o usuário atual conectado do seu aplicativo na forma **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

Esse método não possui parâmetros de solicitação ou URI.

### <a name="request-example"></a>Exemplo de solicitação

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>Resposta

Esse método retorna um corpo de resposta formatada em JSON que contém uma matriz de objetos com os campos a seguir. Cada objeto na matriz representa as ofertas direcionadas que estão disponíveis para o usuário especificado como parte de um segmento de cliente específico.

| Campo      | Tipo   | Descrição         |
|------------|--------|------------------|
| ofertas      | matriz  | Uma série de IDs de produtos para os complementos que estão associados às ofertas direcionadas disponíveis para o usuário atual. Essas IDs de produto são especificados na **direcionados ofertas** página do seu aplicativo no Partner Center.            |
| trackingId  | cadeia de caracteres | Um GUID que, opcionalmente, você pode usar para acompanhar a oferta de destino em seu próprio código ou serviços. |


### <a name="example"></a>Exemplo

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar as ofertas de destino usando os serviços de Store](manage-targeted-offers-using-windows-store-services.md)

 

 
