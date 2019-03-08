---
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: Use este método na API de promoções da Microsoft Store para gerenciar perfis de direcionamento de campanhas publicitárias promocionais.
title: Gerenciar perfis de direcionamento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de promoções da Microsoft Store, campanhas publicitárias
ms.localizationpriority: medium
ms.openlocfilehash: 0d84c6eb678bf884709e13ecefd81e64097ee738
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630201"
---
# <a name="manage-targeting-profiles"></a>Gerenciar perfis de direcionamento


Use esses métodos na API de promoções da Microsoft Store para selecionar usuários, regiões geográficas e tipos de inventário que você deseja destinar para cada linha de entrega de uma campanha publicitária promocional. Os perfis de direcionamento podem ser criados e reutilizados em diversas linhas de entrega.

Para saber mais sobre a relação entre perfis de direcionamento e campanhas publicitárias, linhas de entrega e criativos, consulte [Veicular campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Pré-requisitos

Para usar esses métodos, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](run-ad-campaigns-using-windows-store-services.md#prerequisites) da API de promoções da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usar no cabeçalho da solicitação desses métodos. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esses métodos têm os seguintes URIs.

| Tipo de método | URI da solicitação                                                      |  Descrição  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  Cria um novo perfil de direcionamento.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Edita o perfil de direcionamento especificado por *targetingProfileId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Obtém o perfil de direcionamento especificado por *targetingProfileId*.  |


### <a name="header"></a>Cabeçalho

| Cabeçalho        | Tipo   | Descrição         |
|---------------|--------|---------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |
| ID de rastreamento   | GUID   | Opcional. Uma ID que rastreia o fluxo de chamada.                                  |


### <a name="request-body"></a>Corpo da solicitação

Os métodos POST e PUT exigem um corpo da solicitação JSON com os campos obrigatórios de um objeto [Perfil de direcionamento](#targeting-profile) e quaisquer campos adicionais que você deseja definir ou alterar.


### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como chamar o método POST para criar um perfil de direcionamento.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

O exemplo a seguir demonstra como chamar o método GET para recuperar um perfil de direcionamento.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>Resposta

Esses métodos retornam um corpo de resposta JSON com um objeto [Perfil de direcionamento](#targeting-profile) que contém informações sobre o perfil de direcionamento criado, atualizado ou recuperado. O exemplo a seguir demonstra um corpo de resposta para esses métodos.

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>Objeto de direcionamento de perfil

O corpo da solicitação e resposta desses métodos contêm os campos a seguir. Essa tabela mostra quais campos são somente de leitura (ou seja, eles não podem ser alterados no método PUT) e quais são necessários no corpo da solicitação para o método POST.

| Campo        | Tipo   |  Descrição      |  Somente leitura  | Padrão  | Obrigatório para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número inteiro   |  A ID do perfil de direcionamento.     |   Sim    |       |   Não      |       
|  name   |  cadeia de caracteres   |   O nome do perfil de direcionamento.    |    Não   |      |  Sim     |       
|  targetingType   |  cadeia de caracteres   |  Um dos seguintes valores: <ul><li>**Auto**: Especifique esse valor para permitir que a Microsoft escolher o perfil de direcionamento com base nas configurações do aplicativo no Partner Center.</li><li>**Manual**: Especifique esse valor para definir seu próprio perfil de destino.</li></ul>     |  Não     |  Automático    |   Sim    |       
|  idade   |  matriz   |   Um ou mais números inteiros que identificam os intervalos de idade dos usuários de destino. Para obter uma lista completa de números inteiros, consulte [Valores de idade](#age-values) neste artigo.    |    Não    |  null    |     Não    |       
|  gender   |  matriz   |  Um ou mais números inteiros que identificam os sexos dos usuários de destino. Para obter uma lista completa de números inteiros, consulte [Valores de sexo](#gender-values) neste artigo.       |  Não    |  null    |     Não    |       
|  país   |  matriz   |  Um ou mais números inteiros que identificam os códigos de país dos usuários de destino. Para obter uma lista completa de números inteiros, consulte [Códigos de país](#country-code-values) neste artigo.    |  Não    |  null   |      Não   |       
|  osVersion   |  matriz   |   Um ou mais números inteiros que identificam as versões do sistema operacional dos usuários de destino. Para obter uma lista completa de números inteiros, consulte [Valores da versão do sistema operacional](#osversion-values) neste artigo.     |   Não    |  null   |     Não    |       
|  deviceType   | matriz    |  Um ou mais números inteiros que identificam os tipos de dispositivo dos usuários de destino. Para obter uma lista completa de números inteiros, consulte [Valores do tipo de dispositivo](#devicetype-values) neste artigo.       |   Não    |  null    |    Não     |       
|  supplyType   |  matriz   |  Um ou mais números inteiros que identificam o tipo de inventário no qual os anúncios da campanha serão mostrados. Para obter uma lista completa de números inteiros, consulte [Valores do tipo de fornecimento](#supplytype-values) neste artigo.      |   Não    |  null   |     Não    |   |  


<span id="age-values"/>

### <a name="age-values"></a>Valores de idade

O campo *idade* no objeto [TargetingProfile](#targeting-profile) contém um ou mais dos seguintes inteiros que identificam as faixas etárias dos usuários de destino.

|  Valor inteiro do campo *idade*  |  Faixa etária correspondente  |  
|---------------------------------|---------------------------|
|     651     |            13 a 17             |
|     652     |           18 a 24             |
|     653     |            25 a 34             |
|     654     |            35 a 49             |
|     655     |            50 e acima             |

Para obter os valores aceitos pelo campo *idade* de modo programático, você pode chamar o método GET a seguir.  Para o ```Authorization``` cabeçalho, passar o token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

O exemplo de código a seguir mostra o corpo de resposta desse método.

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>Valores de sexo

O campo *sexo* no objeto [TargetingProfile](#targeting-profile) contém um ou mais dos seguintes inteiros que identificam os sexos dos usuários de destino.

|  Valor inteiro do campo *sexo*  |  Sexo correspondente  |  
|---------------------------------|---------------------------|
|     Etapas de resolução para o seguinte evento ID 700     |            Masculino             |
|     701     |           Feminino             |

Para obter os valores aceitos pelo campo *sexo* de modo programático, você pode chamar o método GET a seguir.  Para o ```Authorization``` cabeçalho, passar o token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

O exemplo de código a seguir mostra o corpo de resposta desse método.

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>Valores de versão do sistema operacional

O campo *osVersion* no objeto [TargetingProfile](#targeting-profile) contém um ou mais dos seguintes inteiros que identificam as versões do sistema operacional dos usuários de destino.

|  Valor inteiro do campo *osVersion*  |  Versão do sistema operacional correspondente  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7.8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows 8.0             |
|     507     |           Windows 8.1             |
|     508     |           Windows 10             |
|     509     |           Windows 10 Mobile             |

Para obter os valores aceitos pelo campo *osVersion* de modo programático, você pode chamar o método GET a seguir.  Para o ```Authorization``` cabeçalho, passar o token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

O exemplo de código a seguir mostra o corpo de resposta desse método.

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>Valores de tipo de dispositivo

O campo *deviceType* no objeto [TargetingProfile](#targeting-profile) contém um ou mais dos seguintes inteiros que identificam os tipos de dispositivo dos usuários de destino.

|  Valor inteiro do campo *deviceType*  |  Tipo de dispositivo correspondente  |  Descrição  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  Isso representa dispositivos que executam uma versão do Windows 10 ou Windows 8. x para computadores desktop.  |
|     711     |  Phone     |  Isso representa dispositivos que executam o Windows 10 Mobile, Windows Phone 8. x ou Windows Phone 7. x.

Para obter os valores aceitos pelo campo *deviceType* de modo programático, você pode chamar o método GET a seguir.  Para o ```Authorization``` cabeçalho, passar o token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

O exemplo de código a seguir mostra o corpo de resposta desse método.

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>Fornecer valores de tipo

O campo *supplyType* no objeto [TargetingProfile](#targeting-profile) contém um ou mais dos seguintes inteiros que identificam o tipo de inventário no qual os anúncios da campanha serão mostrados.

|  Valor inteiro do campo *supplyType*  |  Tipo de fornecimento correspondente  |  Descrição  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  Aplicativo        |  Isso se refere aos anúncios que aparecem somente em apps.  |
|     11471     |  Universal        |  Isso se refere a anúncios que aparecem em apps, na Web e em outras superfícies de exibição.  |

Para obter os valores aceitos pelo campo *supplyType* de modo programático, você pode chamar o método GET a seguir.  Para o ```Authorization``` cabeçalho, passar o token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

O exemplo de código a seguir mostra o corpo de resposta desse método.

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>Valores de código do país

O campo *país* no objeto [TargetingProfile](#targeting-profile) contém um ou mais dos seguintes inteiros que identificam os códigos de país [ISO 3166-1 alfa-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) dos usuários de destino.

|  valor inteiro do campo *país*  |  código do país correspondente  |  
|-------------------------------------|------------------------------|
|     1      |            EUA                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            TI                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NÃO                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            RH                  |
|     60      |            RH                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            N/D                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            PA                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

Para obter os valores aceitos pelo campo *país* de modo programático, você pode chamar o método GET a seguir.  Para o ```Authorization``` cabeçalho, passar o token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

O exemplo de código a seguir mostra o corpo de resposta desse método.

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Executar campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gerenciar campanhas publicitárias](manage-ad-campaigns.md)
* [Gerenciar linhas de entrega para campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar criativos para campanhas publicitárias](manage-creatives-for-ad-campaigns.md)
* [Obter dados de desempenho de campanha de anúncio](get-ad-campaign-performance-data.md)
