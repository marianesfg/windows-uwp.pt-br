---
title: Biometria por impressão digital
description: Este artigo explica como adicionar a biometria de impressão digital em seu aplicativo UWP (Plataforma Universal do Windows).
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 287b2b0fedac112f57f0342420a7830db5aa13be
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3231043"
---
# <a name="fingerprint-biometrics"></a>Biometria por impressão digital




Este artigo explica como adicionar a biometria de impressão digital em seu aplicativo UWP (Plataforma Universal do Windows). Incluir um pedido de autenticação por impressão digital quando o usuário precisar autorizar uma determinada ação aprimora a segurança do seu aplicativo. Por exemplo, você pode exigir autenticação por impressão digital antes de autorizar uma compra realizada em aplicativo ou acessar recursos restritos. A autenticação por impressão digital é gerenciada com o uso da classe [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134) no namespace [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356).

## <a name="check-the-device-for-a-fingerprint-reader"></a>Verifique se o dispositivo tem um leitor de impressão digital


Para saber se o dispositivo tem um leitor de impressão digital, chame [**UserConsentVerifier.CheckAvailabilityAsync**](https://msdn.microsoft.com/library/windows/apps/dn279138). Mesmo que um dispositivo dê suporte à autenticação por impressão digital, o aplicativo ainda deverá fornecer aos usuários uma opção nas Configurações para habilitá-la ou desabilitá-la.

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>Solicitar consentimento e retornar resultados


Para solicitar o consentimento do usuário a partir de uma digitalização de impressão digital, chame o método [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139). Para a autenticação por impressão digital funcionar, o usuário deve ter adicionado anteriormente uma "assinatura" de impressão digital ao banco de dados de impressões digitais.

Quando você chamar o [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139), o usuário verá uma caixa de diálogo modal solicitando uma digitalização de impressão digital. Você pode fornecer uma mensagem para o método **UserConsentVerifier.RequestVerificationAsync** que será exibida ao usuário como parte da caixa de diálogo modal, conforme mostrado na imagem a seguir.

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```