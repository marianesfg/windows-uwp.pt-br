---
title: Simbologias do scanner de código de barras da câmera
description: Simbologias do scanner de código de barras da câmera compatíveis
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 481d10f2fea076f45124a3c75819dfe6494300bf
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448396"
---
# <a name="symbologies"></a>Simbologias

Este tópico fornece amostras de códigos de barras para cada uma das simbologias compatíveis com o decodificador de códigos barras de software que acompanha o Windows 10, incluindo: UPC/EAN, código 39, código 128, intercalado 2 de 5, DataBar omnidirecional, Databar empilhado , código QR e GS1DWCode.

O Windows 10 usa uma câmera de lente padrão combinada com um decodificador de software para gerar um scanner de código de barras. Este artigo refere-se aos simbologias com suporte do decodificador de software. Os simbologias adicionais podem ser suportados por dispositivos de scanner de código de barras dedicados que têm decodificadores de hardware internos, entre em contato com o fabricante do scanner de código de barras para obter detalhes. As simbologias listadas têm suporte em todas as edições do Windows 10 Build 17134 ou posterior, a menos que especificado de outra forma.

Use [GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync) para determinar os simbologias específicos com suporte em um scanner de código de barras.

> [!NOTE]
> O decodificador de software interno do Windows 10 é fornecido pela [*Digimarc Corporation*](https://www.digimarc.com/).

## <a name="1d-symbologies"></a>Simbologias 1D

### <a name="code-39"></a>Código 39
![Exemplo de código de barras - código 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Código 128
![Exemplo de código de barras - código 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>DataBar omnidirecional
![Exemplo de código de barras - DataBar omnidirecional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Databar empilhado
![Exemplo de código de barras - DataBar empilhado](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![Exemplo de código de barras - EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![Exemplo de código de barras - EAN-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Intercalado 2 de 5
![Exemplo de código de barras - intercalado 2 de 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![Exemplo de código de barras - UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![Exemplo de código de barras - UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>Simbologias 2D
### <a name="qr-code"></a>Código QR
![Exemplo de código de barras - código QR](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>Marcas d'água digitais
### <a name="gs1-dwcode"></a>GS1-DWCode

Digitalize a imagem de um pacote abaixo com seu aplicativo de scanner de código de barras de câmera para ver o GS1DWCode em ação.  A imagem é codificada com UPCA 856107006854.  Visite http://www.digimarc.com para obter mais informações sobre as funcionalidades do GS1DWCode.

![Exemplo de código de barras - GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>Veja também

### <a name="samples"></a>Exemplos

- [Exemplo de scanner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
