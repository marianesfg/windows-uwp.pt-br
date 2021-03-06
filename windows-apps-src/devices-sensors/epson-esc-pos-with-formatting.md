---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: Epson ESC/POS com formatação
description: Saiba como usar a linguagem de comando ESC/POS para formatar texto, como caracteres em negrito e tamanho dobrado para sua impressora Ponto de Serviço.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0731f551afaa2420451521b186515255c3724c36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370141"
---
# <a name="epson-escpos-with-formatting"></a>Epson ESC/POS com formatação


**APIs importantes**

-   [**PointofService Printer**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)
-   [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)

Saiba como usar a linguagem de comando ESC/POS para formatar texto, como caracteres em negrito e tamanho dobrado para sua impressora Ponto de Serviço.

## <a name="escpos-usage"></a>Uso de ESC/POS

O Ponto do Serviço do Windows proporciona o uso de uma variedade de impressoras, incluindo várias impressoras da série Epson TM (para obter uma lista completa de impressoras com suporte, consulte a página [Impressora PointofService](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) ). O Windows dá suporte à impressão usando a linguagem de controle de impressora ESC/POS, que fornece comandos eficientes e funcionais para comunicação com sua impressora.

ESC/POS é um sistema de comando criado pela Epson usado em diversos sistemas de impressoras POS, cujo objetivo é evitar conjuntos de comandos incompatíveis fornecendo aplicabilidade universal. A maioria das impressoras modernas dá suporte a ESC/POS.

Todos os comandos começam com o caractere ESC (ASCII 27, HEX 1B) ou GS (ASCII 29, HEX 1D), seguido por outro caractere que especifica o comando. O texto normal é simplesmente enviado à impressora, separado por quebras de linha.

A [**API PointOfService do Windows** ](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) fornece uma grande parte dessa funcionalidade para você por meio dos métodos **Print()** ou **PrintLine()** . Entretanto, para obter uma determinada formatação ou para enviar comandos específicos, você deve usar comandos ESC/POS, criados como uma cadeia de caracteres e enviados à impressora.

## <a name="example-using-bold-and-double-size-characters"></a>Exemplo usando caracteres em negrito e de tamanho duplo

O exemplo a seguir mostra como usar comandos ESC/POS para imprimir caracteres em negrito e de tamanho duplo. Observe que cada comando é criado como uma cadeia de caracteres, e depois inserido nas chamadas a printJob.

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

Para saber mais sobre ESC/POS, incluindo os comandos disponíveis, consulte as [Perguntas frequentes sobre ESC/POS Epson](https://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf). Para obter detalhes sobre [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) e todas as funcionalidades disponíveis, consulte [Impressora PointofService](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) no MSDN.
