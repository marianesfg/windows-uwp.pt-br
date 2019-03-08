---
title: UserSettings (JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655041"
---
# <a name="usersettings-json"></a>UserSettings (JSON)
Retorna as configurações para o usuário autenticado atual. 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
O objeto de UserSettings tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| id| inteiro sem sinal de 32 bits| O identificador da configuração.| 
| Código-fonte| inteiro sem sinal de 32 bits| Representa a origem da configuração. | 
| titleId| inteiro sem sinal de 32 bits| O identificador do título associado à configuração. | 
| value| matriz de inteiro sem sinal de 8 bits| Representa o valor da configuração. Recuperando configurações de clientes devem entender o formato de representação para ser capaz de ler os dados. | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   