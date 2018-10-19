---
author: eliotcowley
title: Obter e compreender os dados de tarja magnética
description: Saiba como obter e interpretar os dados de um tarja magnética.
ms.author: elcowle
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, ponto de serviço, pos, leitor de tarja magnética
ms.localizationpriority: medium
ms.openlocfilehash: ad954e8c03d92307fa72ead236d5428ac2bdddab
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4960331"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Obter e compreender os dados de tarja magnética

Depois que você configurou o leitor de tarja magnética em seu aplicativo usando as etapas descritas no [guia de Introdução do ponto de serviço](pos-basics.md), você está pronto para começar a receber dados dele.

## <a name="subscribe-to-datareceived-events"></a>Assine * DataReceived eventos

Sempre que o leitor reconhece um dedo nelas cartão, ele acionará um dos três eventos:

* [Evento AamvaCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): ocorre quando um cartão de veículo a motor é arrastado.
* [Evento BankCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): ocorre quando um cartão bancário é arrastado.
* [Evento VendorSpecificDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): ocorre quando um cartão específico do fornecedor é arrastado.

Seu aplicativo só precisa assinar os eventos que são suportados pelo leitor de tarja magnética. Você pode ver quais tipos de cartões são compatíveis com [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
).

O código a seguir demonstra assinando os três ***DataReceived** eventos:

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

O manipulador de eventos será passado para o [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) e um objeto de *argumentos* , cujo tipo varia de acordo com o evento:

* Evento **AamvaCardDataReceived** : [MagneticStripeReaderAamvaCardDataReceivedEventArgs classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* Evento **BankCardDataReceived** : [MagneticStripeReaderBankCardDataReceivedEventArgs classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* Evento **VendorSpecificDataReceived** : [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Obter os dados

Para os eventos **AamvaCardDataReceived** e **BankCardDataReceived** , você pode obter alguns dos dados diretamente do objeto *args* . O exemplo a seguir demonstra obtendo algumas propriedades e atribuí-las a variáveis de membro:

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

No entanto, alguns dados, incluindo todos os dados do evento **VendorSpecificDataReceived** , devem ser recuperados através do objeto de **relatório** , que é uma propriedade do parâmetro *args* . Isso é do tipo [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Você pode usar a propriedade [CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) para descobrir qual tipo de cartão foi arrastado e usá-lo para informar ao como interpretar os dados de [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)e [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Os dados de cada uma das faixas são representados como objetos [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) . A partir dessa classe, você pode obter os seguintes tipos de dados:

* [Dados](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): os dados brutos ou decodificados.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): os dados condicionais. 
* [EncryptedData completamente](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): os dados criptografados.

O trecho de código a seguir obtém o relatório e os dados de faixa e, em seguida, verifica o tipo de cartão:

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte também

* [Leitor de tarjas magnéticas](pos-magnetic-stripe-reader.md)
* [Classe ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [Classe MagneticStripeReaderAamvaCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [Classe MagneticStripeReaderBankCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [Classe MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)