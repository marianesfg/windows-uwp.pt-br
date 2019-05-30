---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: Este artigo mostra como ler e gravar propriedades de metadados de imagem e como adicionar marca de localização em arquivos usando a classe de utilitário GeotagHelper.
title: Metadados de imagem
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 589dd40282fc3186f225e3873295863cc41b6a0c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361713"
---
# <a name="image-metadata"></a>Metadados de imagem



Este artigo mostra como ler e gravar propriedades de metadados de imagem e como adicionar marca de localização em arquivos usando a classe de utilitário [**GeotagHelper**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.GeotagHelper).

## <a name="image-properties"></a>Propriedades da imagem

A propriedade [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) retorna um objeto [**StorageItemContentProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) que fornece acesso às informações relacionadas ao conteúdo sobre o arquivo. Obtenha as propriedades específicas da imagem chamando [**GetImagePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync). O objeto retornado [**ImageProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.ImageProperties) expõe membros que contêm campos de metadados de imagem básicos, como o título da imagem e a data de captura.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Para acessar um conjunto maior de metadados do arquivo, use o Windows Property System, um conjunto de propriedades de metadados de arquivo que pode ser recuperado com um identificador de cadeia de caracteres exclusivo. Crie uma lista de cadeias de caracteres e adicione o identificador para cada propriedade que você deseja recuperar. O método [**ImageProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) usa essa lista de cadeias de caracteres e retorna um dicionário de pares de chave/valor onde a chave é o identificador da propriedade e o valor é o valor da propriedade.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Para obter uma lista completa das Propriedades do Windows, incluindo os identificadores e o tipo de cada propriedade, consulte [Propriedades do Windows](https://docs.microsoft.com/windows/desktop/properties/props).

-   Algumas propriedades só são suportadas para determinados codecs de imagem e contêineres de arquivos. Para obter uma lista dos metadados de imagem com suporte para cada tipo de imagem, consulte [Políticas de Metadados de Fotos](https://docs.microsoft.com/windows/desktop/wic/photo-metadata-policies).

-   Como as propriedades que não são suportadas podem retornar um valor nulo quando recuperado, sempre verifique para null antes de usar um valor de metadados retornados.

## <a name="geotag-helper"></a>GeotagHelper

GeotagHelper é uma classe de utilitário que facilita a marcação de imagens com dados geográficos usando as APIs [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) diretamente, sem precisar analisar ou construir manualmente o formato de metadados.

Se você já tiver um objeto [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) que representa o local que deseja marcar na imagem, de uma utilização anterior das APIs de geolocalização ou de alguma outra fonte, poderá definir os dados de marca de localização chamando [**GeotagHelper.SetGeotagAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) e passando [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) e o **Geopoint**.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Para definir os dados de marca de localização usando a localização atual do dispositivo, crie um novo objeto [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) e chame [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) passando **Geolocator** e o arquivo a ser marcado.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Você deve incluir a capacidade do dispositivo **location** no manifesto do aplicativo para usar a API [**SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync).

-   Você deve chamar [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) antes de chamar [**SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) para garantir que o usuário tenha concedido permissão a seu aplicativo para usar sua localização.

-   Para obter mais informações sobre as APIs de geolocalização, consulte [Mapas e localização](https://docs.microsoft.com/windows/uwp/maps-and-location/index).

Para obter um GeoPoint que represente o local de um arquivo de imagem com marca de localização, chame [**GetGeotagAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync).

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>Decodificar e codificar metadados de imagem

A maneira mais avançada de trabalhar com dados de imagem é ler e gravar as propriedades no nível do fluxo usando um [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) ou um [BitmapEncoder](bitmapencoder-options-reference.md). Para essas operações, você pode usar Propriedades do Windows para especificar os dados que está lendo ou gravando, mas também pode usar a linguagem de consulta de metadados fornecida pelo Windows Imaging Component (WIC) para especificar o caminho para uma propriedade solicitada.

Ler metadados de imagem usando essa técnica exige que você tenha um [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) que foi criado com o fluxo de arquivo de imagem de origem. Para obter informações sobre como fazer isso, consulte [Imagens](imaging.md).

Assim que você tiver o decodificador, crie uma lista de cadeias de caracteres e adicione uma nova entrada para cada propriedade de metadados que deseja recuperar usando a cadeia de caracteres de identificador da Propriedade do Windows ou uma consulta de metadados do WIC. Chame o método [**BitmapPropertiesView.GetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) no membro [**BitmapProperties**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapProperties) do decodificador para solicitar as propriedades especificadas. As propriedades são retornadas em um dicionário de pares de chave/valor que contém o nome ou caminho da propriedade e o valor da propriedade.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Para obter informações sobre a linguagem de consulta de metadados do WIC e as propriedades com suporte, consulte [Consultas de metadados nativos de formato de imagem do WIC](https://docs.microsoft.com/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   Muitas propriedades de metadados são suportadas apenas por um subconjunto de tipos de imagem. [**GetPropertiesAsync** ](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) falhará com o código de erro 0x88982F41 se uma das propriedades solicitadas não é compatível com a imagem associada com o decodificador e 0x88982F81 se a imagem não der suporte a metadados em todos os. As constantes associadas com esses códigos de erro são WINCODEC\_ERR\_PROPERTYNOTSUPPORTED e WINCODEC\_ERR\_UNSUPPORTEDOPERATION e são definidos no arquivo de cabeçalho Winerror. h.
-   Como uma imagem pode ou não conter um valor de uma propriedade específica, use o **IDictionary.ContainsKey** para verificar se uma propriedade está presente nos resultados antes de tentar acessá-la.

Gravar metadados de imagem no fluxo requer um **BitmapEncoder** associado ao arquivo de saída da imagem.

Crie um objeto [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) para conter os valores de propriedade que você deseja definir. Crie um objeto [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) para representar o valor da propriedade. Esse objeto usa um **object** como o valor e o membro da enumeração [**PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType) que define o tipo do valor. Adicione **BitmapTypedValue** a **BitmapPropertySet** e, em seguida, chame [**BitmapProperties.SetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) para fazer com que o codificador grave as propriedades no fluxo.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Para obter detalhes sobre quais propriedades são suportadas para quais tipos de arquivo de imagem, consulte [Propriedades do Windows](https://docs.microsoft.com/windows/desktop/properties/props), [Políticas de Metadados de Fotos](https://docs.microsoft.com/windows/desktop/wic/photo-metadata-policies) e [Consultas de metadados nativos de formato de imagem do WIC](https://docs.microsoft.com/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   [**SetPropertiesAsync** ](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) falhará com o código de erro 0x88982F41 se uma das propriedades solicitadas não é compatível com a imagem associada com o codificador.

## <a name="related-topics"></a>Tópicos relacionados

* [Geração de imagens](imaging.md)
 

 




