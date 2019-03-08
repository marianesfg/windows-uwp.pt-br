---
title: XboxServices.config
description: Descreve o arquivo XboxServices.config para associar seu jogo UWP com uma configuração do Xbox Live.
ms.date: 03/29/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, configuração de serviço, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606681"
---
# <a name="xboxservicesconfig-file-description"></a>XboxServices.config file description

Quando você desenvolve um Xbox Live habilitado jogo UWP, seu projeto deve incluir um arquivo XboxServices.config.  Esse arquivo permite que o Xbox Live SDK associar seu jogo com seu aplicativo do Partner Center e sua configuração de serviços do Xbox Live. Esse arquivo contém um objeto JSON que informações de detalhes como a ID de configuração de serviço, ID de título, etc.

Se você estiver usando o Unity para criar um jogo de programa de criadores do Xbox Live, usando o Xbox Live plug-in, esse arquivo é criado automaticamente para você pelo Assistente de associação do Xbox Live.

## <a name="xboxservicesconfig-fields"></a>XboxServices.config fields

>[!NOTE]
> O arquivo criado pelo Assistente de associação do Xbox Live pode incluir campos adicionais além daqueles descritos abaixo, mas elas não são usadas pelo serviço.

Os seguintes campos são definidos no objeto JSON no arquivo de configuração:

Campo | Descrição
--- | ---
PrimaryServiceConfigId  |  A Xbox Live serviço configuração de ID (SCID). Na [Partner Center](https://partner.microsoft.com/dashboard), você pode encontrar esse valor na **Xbox Live** página (para o programa de criadores) ou **Xbox Live instalação** página (completas jogos Xbox Live), sob o **Serviços** seção para seu aplicativo.
TitleId  |  A ID decimal de título para seu aplicativo. Na [Partner Center](https://partner.microsoft.com/dashboard), você pode encontrar esse valor na **Xbox Live** página (para o programa de criadores) ou **Xbox Live instalação** página (completas jogos Xbox Live), sob o **Serviços** seção para seu aplicativo.
XboxLiveCreatorsTitle  |  Se "true", indica que o aplicativo é um aplicativo de programa de criadores do Xbox Live. Caso contrário, "false".
Escopo  |  **(Opcional)**  Define o escopo de funcionalidade usada pelo aplicativo. Consulte abaixo para obter mais detalhes.

### <a name="scope-field"></a>Campo de escopo

O **escopo** campo é um campo opcional que você pode usar para indicar a funcionalidade usada pelo seu jogo.


Se o **escopo** campo não for especificado e, em seguida, o escopo é definido como um valor padrão que depende do valor da **XboxLiveCreatorsTitle** campo, conforme descrito na tabela a seguir:

XboxLiveCreatorsTitle value | Valor do escopo padrão
--- | ---
"true"  |  "xbl.signin xbl.friends"
"false"  |  "xboxlive.signin"



A lista a seguir descreve os valores válidos para o **escopo** campo.

Valor do escopo | Descrição
--- | ---
xbl.signin  | Inclui a entrada na funcionalidade para jogos do programa de criadores. Necessário para jogos do programa de criadores.
xbl.friends | Inclui a funcionalidade social placares de líderes de jogos do programa de criadores e amigos.
xboxlive.signin | Inclui a entrada na funcionalidade para jogos que acessar a funcionalidade completa do Xbox Live. Necessário para jogos do programa de criadores.

Atualmente, o único motivo para especificar o **escopo** campo é se você estiver fazendo um programa de criadores do Xbox Live jogos e seu jogo não precisa acessar listas de amigos ou social placares de líderes (placares de líderes que têm o escopo a seus amigos). Se esse for o caso, você pode adicionar a seguinte linha ao seu arquivo XboxServices.config:

```
  "Scope" : "xbl.signin"
```

Adicionando esta linha impede que o aplicativo UWP solicitando permissão para acessar listas de amigo quando você inicia o aplicativo pela primeira vez.

## <a name="example-xboxservicesconfig-file"></a>Exemplo de arquivo XboxServices.config

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
