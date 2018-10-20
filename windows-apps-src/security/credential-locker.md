---
title: Cofre de credenciais
description: Este artigo descreve como aplicativos UWP (Plataforma Universal do Windows) podem usar o Cofre de credenciais para armazenar com segurança e recuperar credenciais do usuário, e alternar entre os dispositivos com a conta da Microsoft do usuário.
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: c6412f28e60ed0401fb96098fd38128a37491c8d
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/20/2018
ms.locfileid: "5167291"
---
# <a name="credential-locker"></a>Cofre de credenciais




Este artigo descreve como aplicativos UWP (Plataforma Universal do Windows) podem usar o Cofre de credenciais para armazenar com segurança e recuperar credenciais do usuário, e alternar entre os dispositivos com a conta da Microsoft do usuário.

Por exemplo, você possui um aplicativo que se conecta a um serviço para acessar recursos protegidos, como arquivos de mídia ou rede social. O seu serviço requer informações de logon para cada usuário. Você criou a interface do usuário em seu aplicativo que obtém o nome do usuário e a senha que são então usados para fazer logon do usuário no serviço. Usando a API do Cofre de credenciais, você pode armazenar o nome do usuário e a senha do seu usuário e recuperá-los com facilidade e fazer logon do usuário automaticamente na próxima vez em que ele abrir o aplicativo, independentemente do aplicativo em que ele está.

As credenciais do usuário armazenadas no CredentialLocker *não* expiram, *não* são afetadas por [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) e *não* serão limpas devido a inatividade como dados de roaming tradicionais. No entanto, você só pode armazenar até 20 credenciais por app no CredentialLocker.

O Cofre de credenciais funciona um pouco diferente para contas de domínio. Se houver credenciais armazenadas à sua conta da Microsoft e você associar essa conta a uma conta de domínio (como a conta que você usa no trabalho), suas credenciais se moverão para essa conta de domínio. No entanto, quaisquer credenciais novas adicionadas ao se conectar à conta de domínio não se moverão. Isso garante que credenciais privadas para o domínio não serão expostas fora do domínio.

## <a name="storing-user-credentials"></a>Armazenando credenciais do usuário


1.  Obtenha uma referência para o Cofre de credenciais usando o objeto [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) do namespace [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089).
2.  Crie um objeto [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) que contenha um identificador para o seu aplicativo, o nome de usuário e a senha, e passe isso para o método [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) para adicionar a credencial ao cofre.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>Recuperando credenciais do usuário


Você tem várias opções para recuperar credenciais do usuário do Cofre de credenciais depois que tiver uma referência para o objeto [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081).

-   É possível recuperar todas as credenciais do usuário fornecidas para o seu aplicativo no cofre com o método [**PasswordVault.RetrieveAll**](https://msdn.microsoft.com/library/windows/apps/br227088).

-   Se você souber o nome do usuário para as credenciais armazenadas, poderá recuperar todas elas para esse nome de usuário com o método [**PasswordVault.FindAllByUserName**](https://msdn.microsoft.com/library/windows/apps/br227084).

-   Se você souber o nome do recurso para as credenciais armazenadas, poderá recuperar todas elas para esse nome de recurso com o método [**PasswordVault.FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083).

-   Finalmente, se você souber o nome do usuário e o nome do recurso de uma credencial, poderá recuperar apenas essa credencial, com o método [**PasswordVault.Retrieve**](https://msdn.microsoft.com/library/windows/apps/br227087).

Vejamos um exemplo em que armazenamos o nome do recurso globalmente em um aplicativo e faremos logon automaticamente do usuário se encontrarmos uma credencial para ele. No caso de encontrarmos várias credenciais para o mesmo usuário, pediremos a ele para selecionar uma credencial padrão a ser usada ao fazer logon.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    var loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }

    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}


private Windows.Security.Credentials.PasswordCredential GetCredentialFromLocker()
{
    Windows.Security.Credentials.PasswordCredential credential = null;

    var vault = new Windows.Security.Credentials.PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);
    if (credentialList.Count > 0)
    {
        if (credentialList.Count == 1)
        {
            credential = credentialList[0];
        }
        else
        {
            // When there are multiple usernames,
            // retrieve the default username. If one doesn't
            // exist, then display UI to have the user select
            // a default username.

            defaultUserName = GetDefaultUserNameUI();

            credential = vault.Retrieve(resourceName, defaultUserName);
        }
    }

    return credential;
}
```

## <a name="deleting-user-credentials"></a>Excluindo credenciais do usuário


Eliminar credenciais do usuário no Cofre de credenciais é um processo rápido em duas etapas.

1.  Obtenha uma referência para o Cofre de credenciais usando o objeto [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) do namespace [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089).

2.  Passe a credencial a ser excluída para o método [**PasswordVault.Remove**](https://msdn.microsoft.com/library/windows/apps/hh701242).

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>Práticas recomendadas


Use o cofre de credenciais para senhas e não para blobs de dados maiores.

Salve senhas no cofre de credenciais somente se os seguintes critérios forem atendidos:

-   O usuário entrou com êxito.
-   O usuário optou por salvar senhas.

Nunca armazene credenciais em texto sem formatação usando dados de aplicativo ou configurações de roaming.
