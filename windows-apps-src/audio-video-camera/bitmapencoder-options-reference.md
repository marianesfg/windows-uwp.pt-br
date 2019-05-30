---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: Este artigo lista as opções de codificação que podem ser usadas com BitmapEncoder.
title: Referência de opções de BitmapEncoder
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359048"
---
# <a name="bitmapencoder-options-reference"></a>Referência de opções de BitmapEncoder


Esse artigo lista as opções de codificação que podem ser usadas com [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Uma opção de codificação é definida pelo seu nome, que é uma cadeia de caracteres e um valor em um tipo de dados particular ([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType)). Para obter informações sobre como trabalhar com imagens, consulte [Criar, editar e salvar imagens de bitmap](imaging.md).

| Nome                    | PropertyType | Observações de uso                                                                                        | Formatos válidos |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | único       | Valores válidos de 0 a 1.0. Valores maiores indicam maior qualidade                                 | JPEG, JPEG-XR |
| CompressionQuality      | único       | Valores válidos de 0 a 1.0. Valores maiores indicam um esquema de compactação mais eficiente e mais lento | TIFF          |
| Lossless                | boolean      | Se for definido como true, a opção ImageQuality será ignorada                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Entrelaçar ou não a imagem                                                                    | PNG           |
| FilterOption            | uint8        | Use a enumeração [**PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode)                                | PNG           |
| TiffCompressionMethod   | uint8        | Use a enumeração [**TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode)                    | TIFF          |
| Luminância               | uint32Array  | Uma matriz de 64 elementos contendo contrastes de quantização de luminância                               | JPEG          |
| Crominância             | uint32Array  | Uma matriz de 64 elementos contendo contrastes de quantização de crominância                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Use a enumeração [**JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode)                    | JPEG          |
| SuppressApp0            | boolean      | Suprimir ou não a criação de um bloqueio de metadados App0                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Codificar ou não para uma versão de 5 BMP que tem suporte para alfa                                         | BMP           |

 

## <a name="related-topics"></a>Tópicos relacionados

* [Criar, editar e salvar imagens de bitmap](imaging.md)
* [Codecs compatíveis](supported-codecs.md)

 




