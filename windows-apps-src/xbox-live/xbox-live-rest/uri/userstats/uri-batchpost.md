---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
description: " POST (/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a854fc830c87afbf675a379599916bf3db919539
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645831"
---
# <a name="post-batch"></a>POST (/batch)
Método que funciona como um método GET para solicitações em lote complexos para várias estatísticas do player em vários títulos de POST. O domínio para esses URIs é `userstats.xboxlive.com`.
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Comentários
 
Os desenvolvedores de título podem marcar as estatísticas como aberta ou restrita com XDP ou Partner Center. Placar de líderes é abertos de estatísticas. Abrir estatísticas podem ser acessadas por Smartglass, bem como iOS, Android, Windows, Windows Phone e aplicativos web, desde que o usuário está autorizado para a área restrita. Autorização de usuário para uma área restrita é gerenciada por meio de XDP ou Partner Center.
  
  * [Comentários](#ID4ET)
  * [Comentários](#ID4EFB)
  * [Autorização](#ID4EUB)
  * [Cabeçalhos de solicitação necessários](#ID4ETC)
  * [Cabeçalhos de solicitação opcionais](#ID4E3D)
  * [Corpo da solicitação](#ID4EAF)
  * [Códigos de status HTTP](#ID4EWF)
  * [Corpo da resposta](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>Comentários
 
O chamador fornecer um corpo de mensagem com uma matriz de usuários, a configuração de serviço IDs (SCIDs) e uma lista de nomes de estatísticas por SCIDs para o qual recuperar essas estatísticas.
 
Você pode encontrá-lo mais útil para examinar o simples, a única estatística [obter](uri-usersxuidscidsscidstatsget.md) método antes de ler esta página mais complexa, de modo de lote.
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>Autorização
 
Não há lógica de autorização implementada para cenários de isolamento de conteúdo e controle de acesso.
 
   * Estatísticas de placares de líderes e o usuário podem ser lido de clientes em qualquer plataforma, desde que o chamador envia um token válido do XSTS com a solicitação. Gravações são, obviamente, limitadas a clientes com suporte pela.
   * Os desenvolvedores de título podem marcar as estatísticas como aberta ou restrita com XDP ou Partner Center. Placar de líderes é abertos de estatísticas. Abrir estatísticas podem ser acessadas por Smartglass, bem como iOS, Android, Windows, Windows Phone e aplicativos web, desde que o usuário está autorizado para a área restrita. Autorização de usuário para uma área restrita é gerenciada por meio de XDP ou Partner Center.
  
O exemplo a seguir é o pseudocódigo para a verificação:
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nome/número do serviço ao qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 
o seguinte corpo do POST informa ao serviço que quatro estatísticas estão sendo solicitadas de dois SCIDs diferentes para dois usuários diferentes.
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 304| Não modificado| Recurso não foi modificado desde a última solicitado.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação| Não há suporte para a versão do recurso; deve ser rejeitada pela camada de MVC.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent 

[/batch](uri-batch.md)

   