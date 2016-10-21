---
author: drewbatgit
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "Este artigo lista as opções de codificação que podem ser usadas com BitmapEncoder."
title: "Referência de opções de BitmapEncoder"
translationtype: Human Translation
ms.sourcegitcommit: de54d389488d8298ea1341b0a6f27a476d38584e
ms.openlocfilehash: 0ccaf55215cb82633313e145a126db92aa6d16e6

---

# Referência de opções de BitmapEncoder

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

 

## Tópicos relacionados

* [Criar, editar e salvar imagens de bitmap](imaging.md)
 

 







<!--HONumber=Aug16_HO3-->


