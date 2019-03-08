---
title: Profile (JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Profile (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607511"
---
# <a name="profile-json"></a>Profile (JSON)
As configurações de perfil pessoal para um usuário. 
<a id="ID4EN"></a>

 
## <a name="profile"></a>Perfil
 
O objeto de perfil tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| AppDisplayName| cadeia de caracteres| Nome para exibição em aplicativos. Isso pode ser o nome do usuário"real" ou seu nome de jogador, dependendo de privacidade. Essa configuração representa a cadeia de identidade do usuário que deve ser usada para exibição em aplicativos.| 
| GameDisplayName| cadeia de caracteres| Nome para exibição em jogos. Isso pode ser o nome do usuário"real" ou seu nome de jogador, dependendo de privacidade. Essa configuração representa a cadeia de identidade do usuário que deve ser usada para exibição em jogos.| 
| Gamertag| cadeia de caracteres| Gamertag do usuário.| 
| AppDisplayPicRaw| cadeia de caracteres| URL do aplicativo brutos exibição pic (veja abaixo).| 
| GameDisplayPicRaw| cadeia de caracteres| Exibição de jogos brutos pic URL (veja abaixo).| 
| AccountTier| cadeia de caracteres| O usuário tem o tipo de conta? Ouro, Prata ou FamilyGold?| 
| TenureLevel| inteiro sem sinal de 32 bits| O número de anos o usuário tiver sido com o Xbox Live?| 
| jogos| inteiro sem sinal de 32 bits| Jogos do usuário.| 
  


> [!NOTE] 
> Imagens podem ser a imagem do usuário' real' ou seu gamerpic XboxOne, dependendo de privacidade. Essas configurações representam a url da imagem do usuário que deve ser usado para exibição no cliente. Essa imagem pode estar vazia (indicando que o usuário ainda não definir qualquer imagem). 


 
A URL bruta é uma URL redimensionável. Ele pode ser usado para especificar um dos seguintes tamanhos e formatos usando acrescentando `&format={format}&w={width}&h={height}` para o URI:
 
Formato: png
 
Tamanhos: 64x64, 208x208, 424x424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   