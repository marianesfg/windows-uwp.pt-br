---
author: laurenhughes
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: Este artigo lista as opções de codificação que podem ser usadas com BitmapEncoder.
title: Referência de opções de BitmapEncoder
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 13f19ce909703b6748ab00aec1026e30d5c70a64
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5824102"
---
# <a name="bitmapencoder-options-reference"></a>Referência de opções de BitmapEncoder


Esse artigo lista as opções de codificação que podem ser usadas com [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). Uma opção de codificação é definida pelo seu nome, que é uma cadeia de caracteres e um valor em um tipo de dados particular ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Para obter informações sobre como trabalhar com imagens, consulte [Criar, editar e salvar imagens de bitmap](imaging.md).

| Nome                    | PropertyType | Observações de uso                                                                                        | Formatos válidos |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | único       | Valores válidos de 0 a 1.0. Valores maiores indicam maior qualidade                                 | JPEG, JPEG-XR |
| CompressionQuality      | único       | Valores válidos de 0 a 1.0. Valores maiores indicam um esquema de compactação mais eficiente e mais lento | TIFF          |
| Lossless                | booleano      | Se for definido como true, a opção ImageQuality será ignorada                                        | JPEG-XR       |
| InterlaceOption         | booleano      | Entrelaçar ou não a imagem                                                                    | PNG           |
| FilterOption            | uint8        | Use a enumeração [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389)                                | PNG           |
| TiffCompressionMethod   | uint8        | Use a enumeração [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399)                    | TIFF          |
| Luminância               | uint32Array  | Uma matriz de 64 elementos contendo contrastes de quantização de luminância                               | JPEG          |
| Crominância             | uint32Array  | Uma matriz de 64 elementos contendo contrastes de quantização de crominância                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Use a enumeração [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386)                    | JPEG          |
| SuppressApp0            | booleano      | Suprimir ou não a criação de um bloqueio de metadados App0                                        | JPEG          |
| EnableV5Header32bppBGRA | booleano      | Codificar ou não para uma versão de 5 BMP que tem suporte para alfa                                         | BMP           |

 

## <a name="related-topics"></a>Tópicos relacionados

* [Criar, editar e salvar imagens de bitmap](imaging.md)
* [Codecs compatíveis](supported-codecs.md)

 




