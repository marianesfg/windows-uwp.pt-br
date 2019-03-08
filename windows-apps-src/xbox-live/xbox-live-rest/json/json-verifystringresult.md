---
title: VerifyStringResult (JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599481"
---
# <a name="verifystringresult-json"></a>VerifyStringResult (JSON)
Correspondente a cada cadeia de caracteres enviada para códigos de resultado [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

O objeto VerifyStringResult tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| resultCode| inteiro sem sinal de 32 bits| Obrigatório. Código de HResult correspondente à cadeia de caracteres foi enviada.|
| offendingString| cadeia de caracteres| Obrigatório. Valor de cadeia de caracteres que causou uma cadeia de caracteres a serem rejeitadas.|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>Valores HResult comuns

| Valor| Nome do erro|
| --- | --- | --- | --- | --- |
| 0| Êxito|
| 1| Cadeia de caracteres ofensiva|
| 2| Cadeia de caracteres muito longa|
| 3| Erro desconhecido|

<a id="ID4ELD"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4END"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Referência

[POST (/ sistema/cadeias de caracteres/validar)](../uri/stringserver/uri-systemstringsvalidatepost.md)
