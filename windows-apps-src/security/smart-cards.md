---
title: Cartões inteligentes
description: Este tópico explica como os aplicativos da Plataforma Universal do Windows (UWP) podem usar cartões inteligentes para conectar os usuários à serviços de rede seguros, incluindo como acessar os leitores de cartão inteligente físicos, criar cartões inteligentes virtuais, se comunicar com cartões inteligentes, autenticar usuários, redefinir PINs de usuário e como remover ou desconectar cartões inteligentes.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 5498480e0dbe2c8be96d92df766b15676a3e6b7b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371931"
---
# <a name="smart-cards"></a>Cartões inteligentes




Este tópico explica como os aplicativos da Plataforma Universal do Windows (UWP) podem usar cartões inteligentes para conectar os usuários à serviços de rede seguros, incluindo como acessar os leitores de cartão inteligente físicos, criar cartões inteligentes virtuais, se comunicar com cartões inteligentes, autenticar usuários, redefinir PINs de usuário e como remover ou desconectar cartões inteligentes. 

## <a name="configure-the-app-manifest"></a>Configure o manifesto do aplicativo


Para que o seu aplicativo possa autenticar usuários usando cartões inteligentes ou cartões inteligentes virtuais, você deve configurar o recurso **Certificados de Usuário Compartilhado** no arquivo do projeto Package.appxmanifest.

## <a name="access-connected-card-readers-and-smart-cards"></a>Acesse leitores de cartões conectados e cartões inteligentes


Você pode consultar leitores e cartões inteligentes conectados passando a ID de dispositivo (especificada em [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)) para o método [**SmartCardReader.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync). Para acessar os cartões inteligentes atualmente conectados a um dispositivo leitor retornado, chame [**SmartCardReader.FindAllCardsAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync).

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

Você deve também habilitar seu aplicativo para observar eventos [**CardAdded**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardadded) e implementar uma função para determinar o comportamento do aplicativo na inserção do cartão.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

Em seguida, você pode passar cada objeto [**SmartCard**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCard) retornado para [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) acessar os métodos que permitem ao seu aplicativo acessar e personalizar sua configuração.

## <a name="create-a-virtual-smart-card"></a>Crie um cartão inteligente virtual


Para criar um cartão inteligente virtual usando [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning), seu aplicativo precisará primeiro fornecer um nome amigável, uma chave de administração e um [**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy). O nome amigável é geralmente algo fornecido para o aplicativo, mas o seu aplicativo ainda precisará fornecer uma chave de administração e gerar uma instância do **SmartCardPinPolicy** atual antes de passar os três valores para [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

1.  Crie uma nova instância de um [**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy)
2.  Gere o valor da chave de administração chamando [**CryptographicBuffer.GenerateRandom**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) no valor da chave de administração fornecido pelo serviço ou pela ferramenta de gerenciamento.
3.  Passe esses valores junto com a cadeia *FriendlyNameText* para [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

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

Depois que [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) tiver retornado o objeto [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) associado, o cartão inteligente virtual será provisionado e estará pronto para o uso.

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

1.  Para começar o desafio, chame [**GetChallengeContextAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync) do objeto [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) associado ao cartão inteligente. Isso irá gerar uma instância do [**SmartCardChallengeContext**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext), que contém o valor [**Challenge**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge) do cartão.

2.  Em seguida, passe o valor do desafio do cartão e a chave de administração fornecidos pelo serviço ou pela ferramenta de gerenciamento ao **ChallengeResponseAlgorithm** que definimos no exemplo anterior.

3.  [**VerifyResponseAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync) retornará **verdadeiro** se a autenticação for bem-sucedida.

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

1.  Acesse o cartão e gere o objeto [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) associado.
2.  Chame [**RequestPinChangeAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) para exibir uma interface do usuário para que o usuário conclua essa operação.
3.  Se o PIN foi alterado com êxito, a chamada retornará **true**.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

Para solicitar uma redefinição de PIN:

1.  Chame [**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) para iniciar a operação. Essa chamada inclui um método [**SmartCardPinResetHandler**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler) que representa o cartão inteligente e a solicitação de redefinição de PIN.
2.  [**SmartCardPinResetHandler** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler) fornece informações que nossos **ChallengeResponseAlgorithm**encapsulada em um [ **SmartCardPinResetDeferral** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral) chamar usa para comparar o valor do desafio do cartão e a chave de administração fornecido pela serviço ou ferramenta de gerenciamento para autenticar a solicitação.

3.  Se o desafio for bem-sucedido, a chamada [**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) estará concluída; retornando **true** se o PIN teve uma redefinição bem-sucedida.

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


Quando um cartão inteligente físico é removido, um evento [**CardRemoved**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved) é disparado quando o cartão é excluído.

Associe o disparo desse evento ao leitor de cartão ao método que define o comportamento do seu aplicativo na remoção do cartão ou do leitor como um manipulador de eventos. Esse comportamento pode ser algo tão simples quanto fornecer notificação ao usuário de que o cartão foi removido.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

A remoção de um cartão inteligente virtual é tratada de forma programática, primeiro recuperando o cartão e, em seguida, chamando [**RequestVirtualSmartCardDeletionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync) a partir do objeto retornado [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning).

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```