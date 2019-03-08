---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606671"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
Obtém o token de campos. O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EGC)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Entretenimento descoberta de serviços (EDS) APIs, por padrão, retorna um conjunto mínimo muito pequeno de campos para cada item:
 
   * Tipo de item de mídia
   * Grupo de mídia
   * Id
   * Nome
  
Para obter mais informações, as APIs de aceitam um **campos** parâmetro que especifica quais partes extras de dados devem ser retornados. Como há muitos campos possíveis, especificar seu nome completo para cada chamada à API seria inchar consideravelmente a solicitação. Em vez disso, os nomes podem ser passados para dentro dessa API que irá gerar um muito valor menor que pode ser passado para outras APIs.
 
Para qualquer API que aceita esse parâmetro, o valor fornecido deve ser o superconjunto de todos os campos em todos os tipos de item de mídia especificada. Não é possível especificar diferentes conjuntos de campos de tipos de item de mídia diferente. No entanto, se um campo se aplica ao tipo de item de uma mídia, mas não em outra, ele só será exibido na mídia de tipos de item de onde os dados existem (por exemplo, se "AvatarBodyType" estiver incluído na chamada para [obter (/media/ {marketplaceId} / campos)](uri-medialocalefields.md), somente AvatarItems conterá o campo).
 
Os valores retornados dessa API são altamente armazenáveis em cache – na verdade, eles não devem alterar exceto entre implantações da EDS. É recomendável que, se o cache for desejado, o cache durar não mais do que a sessão do usuário.
 
Além de aceitar os nomes de campo reais, essa API aceita "all" como um valor válido. Isso irá gerar um valor que contém cada campo que é possível especificar. Uso do valor "all" é recomendado apenas para desenvolvimento, depuração e fins de teste.
 
Como alternativa, você pode enviar `desired={list of fields separated by a '.'}` a qualquer API que aceita o **campos** token.
 
Você não pode passar em ambas **desejado** e **campos** juntos.
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Desejado| cadeia de caracteres| Obrigatório. O '.' -lista separada de campos que devem ser retornados, além do conjunto mínimo. Nem todos os campos especificados têm garantia de serem retornadas para cada objeto (dados poderão simplesmente não existir para determinados itens), mas nenhum campo fora do conjunto mínimo que não é especificado aqui será retornado. | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   