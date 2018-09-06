---
title: Cartões inteligentes
description: Este tópico explica como os aplicativos da Plataforma Universal do Windows (UWP) podem usar cartões inteligentes para conectar os usuários à serviços de rede seguros, incluindo como acessar os leitores de cartão inteligente físicos, criar cartões inteligentes virtuais, se comunicar com cartões inteligentes, autenticar usuários, redefinir PINs de usuário e como remover ou desconectar cartões inteligentes.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: dda2b580a82c72ad2e31c771a9c76f8d770049ec
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3404888"
---
# <a name="smart-cards"></a>Cartões inteligentes




Este tópico explica como os aplicativos da Plataforma Universal do Windows (UWP) podem usar cartões inteligentes para conectar os usuários à serviços de rede seguros, incluindo como acessar os leitores de cartão inteligente físicos, criar cartões inteligentes virtuais, se comunicar com cartões inteligentes, autenticar usuários, redefinir PINs de usuário e como remover ou desconectar cartões inteligentes. 

## <a name="configure-the-app-manifest"></a>Configure o manifesto do aplicativo


Para que o seu aplicativo possa autenticar usuários usando cartões inteligentes ou cartões inteligentes virtuais, você deve configurar o recurso **Certificados de Usuário Compartilhado** no arquivo do projeto Package.appxmanifest.

## <a name="access-connected-card-readers-and-smart-cards"></a>Acesse leitores de cartões conectados e cartões inteligentes


Você pode consultar leitores e cartões inteligentes conectados passando a ID de dispositivo (especificada em [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)) para o método [**SmartCardReader.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn263890). Para acessar os cartões inteligentes atualmente conectados a um dispositivo leitor retornado, chame [**SmartCardReader.FindAllCardsAsync**](https://msdn.microsoft.com/library/windows/apps/dn263887).

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

Você deve também habilitar seu aplicativo para observar eventos [**CardAdded**](https://msdn.microsoft.com/library/windows/apps/dn263866) e implementar uma função para determinar o comportamento do aplicativo na inserção do cartão.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

Em seguida, você pode passar cada objeto [**SmartCard**](https://msdn.microsoft.com/library/windows/apps/dn297565) retornado para [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) acessar os métodos que permitem ao seu aplicativo acessar e personalizar sua configuração.

## <a name="create-a-virtual-smart-card"></a>Crie um cartão inteligente virtual


Para criar um cartão inteligente virtual usando [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801), seu aplicativo precisará primeiro fornecer um nome amigável, uma chave de administração e um [**SmartCardPinPolicy**](https://msdn.microsoft.com/library/windows/apps/dn297642). O nome amigável é geralmente algo fornecido para o aplicativo, mas o seu aplicativo ainda precisará fornecer uma chave de administração e gerar uma instância do **SmartCardPinPolicy** atual antes de passar os três valores para [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830).

1.  Crie uma nova instância de um [**SmartCardPinPolicy**](https://msdn.microsoft.com/library/windows/apps/dn297642)
2.  Gere o valor da chave de administração chamando [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392) no valor da chave de administração fornecido pelo serviço ou pela ferramenta de gerenciamento.
3.  Passe esses valores junto com a cadeia *FriendlyNameText* para [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830).

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

Depois que [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830) tiver retornado o objeto [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) associado, o cartão inteligente virtual será provisionado e estará pronto para o uso.

## <a name="handle-authentication-challenges"></a>Trate os desafios da autenticação


Para autenticar com cartões inteligentes ou cartões inteligentes virtuais, o seu aplicativo deve fornecer o comportamento para concluir os desafios entre os dados da chave de administração armazenados no cartão e os dados da chave de administração mantidos pelo servidor de autenticação ou pela ferramenta de gerenciamento.

O código a seguir mostra como dar suporte à autenticação de cartão inteligente para serviços ou modificação de detalhes do cartão físico ou virtual. Se os dados gerados usando a chave de administração no cartão ("desafio") forem iguais aos dados da chave de administração fornecidos pelo servidor ou pela ferramenta de gerenciamento ("adminkey"), a autenticação será bem-sucedida.

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

Você verá esse código referenciado em todo o restante deste tópico enquanto revisamos como concluir a ação de autenticação e como aplicar mudanças nas informações do cartão inteligente e do cartão inteligente virtual.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>Verifique a resposta de autenticação do cartão inteligente ou do cartão inteligente virtual


Agora que temos a lógica definida para os desafios de autenticação, podemos nos comunicar com o leitor para acessar o cartão inteligente, ou então, acessar um cartão inteligente virtual para autenticação.

1.  Para começar o desafio, chame [**GetChallengeContextAsync**](https://msdn.microsoft.com/library/windows/apps/dn263811) do objeto [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) associado ao cartão inteligente. Isso irá gerar uma instância do [**SmartCardChallengeContext**](https://msdn.microsoft.com/library/windows/apps/dn297570), que contém o valor [**Challenge**](https://msdn.microsoft.com/library/windows/apps/dn297578) do cartão.

2.  Em seguida, passe o valor do desafio do cartão e a chave de administração fornecidos pelo serviço ou pela ferramenta de gerenciamento ao **ChallengeResponseAlgorithm** que definimos no exemplo anterior.

3.  [**VerifyResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn297627) retornará **true** se a autenticação for bem-sucedida.

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>Altere ou redefina um PIN do usuário


Para alterar o PIN associado a um cartão inteligente:

1.  Acesse o cartão e gere o objeto [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) associado.
2.  Chame [**RequestPinChangeAsync**](https://msdn.microsoft.com/library/windows/apps/dn263823) para exibir uma interface do usuário para que o usuário conclua essa operação.
3.  Se o PIN foi alterado com êxito, a chamada retornará **true**.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

Para solicitar uma redefinição de PIN:

1.  Chame [**RequestPinResetAsync**](https://msdn.microsoft.com/library/windows/apps/dn263825) para iniciar a operação. Essa chamada inclui um método [**SmartCardPinResetHandler**](https://msdn.microsoft.com/library/windows/apps/dn297701) que representa o cartão inteligente e a solicitação de redefinição de PIN.
2.  [**SmartCardPinResetHandler**](https://msdn.microsoft.com/library/windows/apps/dn297701) fornece informações de que nosso **ChallengeResponseAlgorithm**, encapsulado em uma chamada [**SmartCardPinResetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn297693), usa para comparar o valor do desafio do cartão e a chave de administração fornecida pelo serviço ou pela ferramenta de gerenciamento para autenticar a solicitação.

3.  Se o desafio for bem-sucedido, a chamada [**RequestPinResetAsync**](https://msdn.microsoft.com/library/windows/apps/dn263825) estará concluída; retornando **true** se o PIN teve uma redefinição bem-sucedida.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>Remova um cartão inteligente ou um cartão inteligente virtual


Quando um cartão inteligente físico é removido, um evento [**CardRemoved**](https://msdn.microsoft.com/library/windows/apps/dn263875) é disparado quando o cartão é excluído.

Associe o disparo desse evento ao leitor de cartão ao método que define o comportamento do seu aplicativo na remoção do cartão ou do leitor como um manipulador de eventos. Esse comportamento pode ser algo tão simples quanto fornecer notificação ao usuário de que o cartão foi removido.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

A remoção de um cartão inteligente virtual é tratada de forma programática, primeiro recuperando o cartão e, em seguida, chamando [**RequestVirtualSmartCardDeletionAsync**](https://msdn.microsoft.com/library/windows/apps/dn263850) a partir do objeto retornado [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801).

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```