---
title: Serviços sociais de programação
description: Fornece um código de exemplo de como usar a API do Gerenciador de Social Xbox Live.
ms.assetid: 101d059a-e03f-472c-8300-800aa5730ee2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, manager social, exemplo
ms.localizationpriority: medium
ms.openlocfilehash: 5039d9ed205cadfee3b2e64527a1f58f1624accc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592051"
---
# <a name="programming-social-services"></a>Serviços sociais de programação

> [!NOTE]
> Este artigo demonstra o uso avançado de API.  Como ponto de partida, dê uma olhada na [Introdução à API do Gerenciador de Social](../intro-to-social-manager.md) que simplifica significativamente o desenvolvimento.  Informe sua mãe se você encontrar um cenário sem suporte no Gerenciador de Social.

O exemplo de código a seguir demonstra como recuperar uma relação de redes sociais com o Xbox Live. Ele gera uma lista de todos os usuários do sistema e recupera o primeiro deles. Em seguida, ele recupera todos os relacionamentos de social do usuário. Por fim, ele exibe as propriedades públicas de cada uma dessas relações.

```cpp
XboxLiveContext^ xboxLiveContext = NULL;

// An XboxLiveContext for a user should be created only once, or you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_SocialService_GetSocialRelationshipsAsync()
{
    // Generate a list of users on the system.
    // Create an XboxLiveContext from user 0 (the first one returned).
    // This user's authentication token will be used for service requests.
    XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

    // Asynchronously retrieve all social relationships from that context.
    auto asyncOp = xboxLiveContext->SocialService->GetSocialRelationshipsAsync();

    create_task( asyncOp )
    .then([](XboxSocialRelationshipResult^ result)
    {
        // For each social relationship of the specified user...
        for( XboxSocialRelationship^ xboxSocialRelationship : result->Items )
        {
            // ...display the public properties of the relationship.
            LogComment( xboxSocialRelationship->XboxUserId );
            LogComment( xboxSocialRelationship->IsFavorite.ToString() );
        }

    })
    .wait();
}
```
