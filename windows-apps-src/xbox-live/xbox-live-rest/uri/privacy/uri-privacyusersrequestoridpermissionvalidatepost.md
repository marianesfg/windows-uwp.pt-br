---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623421"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
Obtém um conjunto de respostas de Sim ou não sobre se o usuário tem permissão para executar as ações especificadas com um conjunto de usuários de destino.

  * [Comentários](#ID4EQ)
  * [Parâmetros de URI](#ID4ECB)
  * [Autorização](#ID4ENB)
  * [Cabeçalhos de solicitação necessários](#ID4ESC)
  * [Corpo da solicitação](#ID4E4D)
  * [Códigos de status HTTP](#ID4ETE)
  * [Cabeçalhos de resposta necessária](#ID4EIG)
  * [Corpo da resposta](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Comentários

O corpo da solicitação usa uma lista de usuários e uma lista de configurações e o resultado é um resultado permitido/bloqueado para cada par de configuração do usuário.

Em cenários com vários participantes de entre redes (em que as verificações de comunicações de privacidade devem ser executadas entre os usuários que têm uma ID de usuário do Xbox (XUID) e os usuários de fora da rede que não), consulte [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md) para tipos de usuário.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| requestorId| cadeia de caracteres| Obrigatório. Identificador do usuário que está executando a ação. Os valores possíveis são <code>xuid({xuid})</code> e <code>me</code>. Isso deve ser um usuário que fez logon. Valor de exemplo: <code>xuid(0987654321)</code>.|

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorização

Declarações de autorização usadas | Declaração| Tipo| Necessário?| Valor de exemplo|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| inteiro com sinal de 64 bits| sim| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor de exemplo: 1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>Corpo da solicitação

<a id="ID4EDE"></a>


### <a name="required-members"></a>Membros necessários

Ver [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md).


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 400| A solicitação é inválida.| Exemplos: IDs de configuração incorreta, URIs incorreto, etc.|
| 404| O usuário especificado no URI não existe.| O recurso especificado não pôde ser encontrado.|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| cadeia de caracteres| O tipo MIME do corpo da solicitação. Valor de exemplo: <code>application/json</code>|
| Content-Length| cadeia de caracteres| O número de bytes que estão sendo enviados na resposta. Valor de exemplo: 34|
| Cache-Control| cadeia de caracteres| Cortês solicitação do servidor para especificar o comportamento do cache. Exemplo: <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>Corpo da resposta

Ver [PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md).

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [Enumeração PermissionId](../../enums/privacy-enum-permissionid.md)
