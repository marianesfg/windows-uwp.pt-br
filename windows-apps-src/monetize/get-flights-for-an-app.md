---
author: mcleanbyron
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: "Use este método na API de envio da Windows Store para recuperar informações de pacote de pré-lançamento de um aplicativo que está registrado na sua conta do Centro de Desenvolvimento do Windows."
title: "Obter pacotes de pré-lançamento para um aplicativo usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: a49e4f2cf7110e12dd33a5baa37e328a39bae348

---

# Obter pacotes de pré-lançamento para um aplicativo usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para listar os pacotes de pré-lançamento de um aplicativo que está registrado na sua conta do Centro de Desenvolvimento do Windows. Para obter mais informações sobre pacotes de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).

## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` |

<span/>
 
### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição  |  Obrigatório  |    
|---------------|--------|----------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo para o qual você deseja recuperar os pacotes de pré-lançamento. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Sim  |
|  top  |  int  |  O número de itens a serem retornados na solicitação (ou seja, o número de pacotes de pré-lançamento a serem retornados). Se o aplicativo tiver mais pacotes de pré-lançamento que o valor especificado na consulta, o corpo da resposta incluirá um caminho relativo do URI que você pode acrescentar ao URI do método para solicitar a próxima página de dados.  |  Não  |
|  skip  |  int  |  O número de itens a serem ignorados na consulta antes de retornar os itens restantes. Use este parâmetro para percorrer conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam os itens de 1 a 10, top=10 e skip=10 recuperam os itens de 11 a 20 e assim por diante.  |  Não  |

<span/>

### Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### Exemplos de solicitação

O exemplo a seguir demonstra como listar todos os pacotes de pré-lançamento de um aplicativo.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

O exemplo a seguir demonstra como listar o primeiro pacote de pré-lançamento de um aplicativo.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

O exemplo a seguir demonstra o corpo da resposta JSON retornado por uma solicitação bem-sucedida para o primeiro pacote de pré-lançamento de um aplicativo com três pacotes de pré-lançamento no total. Para obter mais detalhes sobre os valores no corpo da resposta, veja a seção a seguir.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres terá um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para solicitar a próxima página de dados. Por exemplo, se o parâmetro *top* do corpo da solicitação inicial for definido como 2, mas houver 4 pacotes de pré-lançamento para o aplicativo, o corpo da resposta incluirá um valor @nextLink ```applications/{applicationid}/listflights/?skip=2&top=2```, o que indica que você pode chamar ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2``` para solicitar os próximos 2 pacotes de pré-lançamento. |
| value      | array  | Uma matriz de objetos que fornecem informações sobre pacotes de pré-lançamento para o aplicativo especificado. Para obter mais informações sobre os dados em cada objeto, consulte [Recurso da versão de pré-lançamento](get-app-data.md#flight-object).                                                                                                                           |
| totalCount | int    | O número total de linhas no resultado dos dados da consulta (ou seja, o número total de pacotes de pré-lançamento do aplicativo especificado).                                                                                                                                                                                                                             |

<span/>

## Códigos de erro

Se não foi possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | Nenhum pacote de pré-lançamento foi encontrado. |
| 409  | O aplicativo usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter todos os aplicativos](get-all-apps.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter complementos para um aplicativo](get-add-ons-for-an-app.md)



<!--HONumber=Aug16_HO5-->

