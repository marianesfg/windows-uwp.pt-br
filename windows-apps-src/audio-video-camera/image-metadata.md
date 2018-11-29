---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: Este artigo mostra como ler e gravar propriedades de metadados de imagem e como adicionar marca de localização em arquivos usando a classe de utilitário GeotagHelper.
title: Metadados de imagem
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ab1279a8744d6dc9cddc88abaa064058f1259c2
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8192640"
---
# <a name="image-metadata"></a>Metadados de imagem



Este artigo mostra como ler e gravar propriedades de metadados de imagem e como adicionar marca de localização em arquivos usando a classe de utilitário [**GeotagHelper**](https://msdn.microsoft.com/library/windows/apps/dn903683).

## <a name="image-properties"></a>Propriedades de imagem

A propriedade [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) retorna um objeto [**StorageItemContentProperties**](https://msdn.microsoft.com/library/windows/apps/hh770642) que fornece acesso às informações relacionadas ao conteúdo sobre o arquivo. Obtenha as propriedades específicas da imagem chamando [**GetImagePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770646). O objeto retornado [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/br207718) expõe membros que contêm campos de metadados de imagem básicos, como o título da imagem e a data de captura.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Para acessar um conjunto maior de metadados do arquivo, use o Windows Property System, um conjunto de propriedades de metadados de arquivo que pode ser recuperado com um identificador de cadeia de caracteres exclusivo. Crie uma lista de cadeias de caracteres e adicione o identificador para cada propriedade que você deseja recuperar. O método [**ImageProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br207732) usa essa lista de cadeias de caracteres e retorna um dicionário de pares de chave/valor onde a chave é o identificador da propriedade e o valor é o valor da propriedade.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Para obter uma lista completa das Propriedades do Windows, incluindo os identificadores e o tipo de cada propriedade, consulte [Propriedades do Windows](https://msdn.microsoft.com/library/windows/desktop/dd561977).

-   Algumas propriedades só são suportadas para determinados codecs de imagem e contêineres de arquivos. Para obter uma lista dos metadados de imagem com suporte para cada tipo de imagem, consulte [Políticas de Metadados de Fotos](https://msdn.microsoft.com/library/windows/desktop/ee872003).

-   Como as propriedades que não são suportadas podem retornar um valor nulo quando recuperado, sempre verifique para null antes de usar um valor de metadados retornados.

## <a name="geotag-helper"></a>GeotagHelper

GeotagHelper é uma classe de utilitário que facilita a marcação de imagens com dados geográficos usando as APIs [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) diretamente, sem precisar analisar ou construir manualmente o formato de metadados.

Se você já tiver um objeto [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) que representa o local que deseja marcar na imagem, de uma utilização anterior das APIs de geolocalização ou de alguma outra fonte, poderá definir os dados de marca de localização chamando [**GeotagHelper.SetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903685) e passando [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) e o **Geopoint**.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Para definir os dados de marca de localização usando a localização atual do dispositivo, crie um novo objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) e chame [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) passando **Geolocator** e o arquivo a ser marcado.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Você deve incluir a capacidade do dispositivo **location** no manifesto do aplicativo para usar a API [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686).

-   Você deve chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de chamar [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) para garantir que o usuário tenha concedido permissão a seu aplicativo para usar sua localização.

-   Para obter mais informações sobre as APIs de geolocalização, consulte [Mapas e localização](https://msdn.microsoft.com/library/windows/apps/mt219699).

Para obter um GeoPoint que represente o local de um arquivo de imagem com marca de localização, chame [**GetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903684).

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>Decodificar e codificar metadados de imagem

A maneira mais avançada de trabalhar com dados de imagem é ler e gravar as propriedades no nível do fluxo usando um [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) ou um [BitmapEncoder](bitmapencoder-options-reference.md). Para essas operações, você pode usar Propriedades do Windows para especificar os dados que está lendo ou gravando, mas também pode usar a linguagem de consulta de metadados fornecida pelo Windows Imaging Component (WIC) para especificar o caminho para uma propriedade solicitada.

Ler metadados de imagem usando essa técnica exige que você tenha um [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) que foi criado com o fluxo de arquivo de imagem de origem. Para obter informações sobre como fazer isso, consulte [Imagens](imaging.md).

Assim que você tiver o decodificador, crie uma lista de cadeias de caracteres e adicione uma nova entrada para cada propriedade de metadados que deseja recuperar usando a cadeia de caracteres de identificador da Propriedade do Windows ou uma consulta de metadados do WIC. Chame o método [**BitmapPropertiesView.GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) no membro [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/br226248) do decodificador para solicitar as propriedades especificadas. As propriedades são retornadas em um dicionário de pares de chave/valor que contém o nome ou caminho da propriedade e o valor da propriedade.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Para obter informações sobre a linguagem de consulta de metadados do WIC e as propriedades com suporte, consulte [Consultas de metadados nativos de formato de imagem do WIC](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   Muitas propriedades de metadados são suportadas apenas por um subconjunto de tipos de imagem. [**GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) falhará com o código de erro 0x88982F41 se não houver suporte para uma das propriedades solicitadas pela imagem associada ao decodificador e 0x88982F81 se a imagem não der suporte a metadados. As constantes associadas a esses códigos de erro são WINCODEC\_ERR\_PROPERTYNOTSUPPORTED e WINCODEC\_ERR\_UNSUPPORTEDOPERATION e são definidas no arquivo de cabeçalho winerror.h.
-   Como uma imagem pode ou não conter um valor de uma propriedade específica, use o **IDictionary.ContainsKey** para verificar se uma propriedade está presente nos resultados antes de tentar acessá-la.

Gravar metadados de imagem no fluxo requer um **BitmapEncoder** associado ao arquivo de saída da imagem.

Crie um objeto [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) para conter os valores de propriedade que você deseja definir. Crie um objeto [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) para representar o valor da propriedade. Esse objeto usa um **object** como o valor e o membro da enumeração [**PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871) que define o tipo do valor. Adicione **BitmapTypedValue** a **BitmapPropertySet** e, em seguida, chame [**BitmapProperties.SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) para fazer com que o codificador grave as propriedades no fluxo.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Para obter detalhes sobre quais propriedades são suportadas para quais tipos de arquivo de imagem, consulte [Propriedades do Windows](https://msdn.microsoft.com/library/windows/desktop/dd561977), [Políticas de Metadados de Fotos](https://msdn.microsoft.com/library/windows/desktop/ee872003) e [Consultas de metadados nativos de formato de imagem do WIC](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) falhará com o código de erro 0x88982F41 se não houver suporte para uma das propriedades solicitadas pela imagem associada ao codificador.

## <a name="related-topics"></a>Tópicos relacionados

* [Imagens](imaging.md)
 

 




