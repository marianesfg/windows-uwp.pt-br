---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: Criar um seletor de dispositivo
description: Construir um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67d83a66687bb8719dc374a2a8a3e30eaac82c71
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684830"
---
# <a name="build-a-device-selector"></a>Criar um seletor de dispositivo



**APIs importantes**

- [**Windows. Devices. Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

Construir um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos. Isso permitirá obter apenas resultados relevantes e também melhorará o desempenho do sistema. Na maioria das situações, você obtém um seletor de dispositivo de uma pilha de dispositivo. Por exemplo, você pode usar [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.usb.usbdevice.getdeviceselector) para dispositivos descobertos por USB. Esses seletores de dispositivo retornam uma cadeia de caracteres de Sintaxe de Consulta Avançada (AQS). Se você não estiver familiarizado com o formato AQS, leia [Usando a Sintaxe de Consulta Avançada de forma programática](https://docs.microsoft.com/windows/desktop/search/-search-3x-advancedquerysyntax).

## <a name="building-the-filter-string"></a>Criando a cadeia de caracteres de filtro

Há alguns casos em que você precisa enumerar dispositivos, e um seletor de dispositivo fornecido não está disponível para seu cenário. Um seletor de dispositivo é uma cadeia de caracteres de filtro AQS que contém as informações a seguir. Antes de criar uma cadeia de caracteres de filtro, você precisa saber algumas partes importantes de informações sobre os dispositivos que você deseja enumerar.

-   O [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) dos dispositivos em que você está interessado. Para saber mais sobre como o **DeviceInformationKind** impacta a enumeração de dispositivos, consulte [Enumerate devices](enumerate-devices.md).
-   Como criar uma cadeia de caracteres de filtro AQS, que é explicada neste tópico.
-   As propriedades em que você está interessado. As propriedades disponíveis dependerão do [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind). Consulte [Device information properties](device-information-properties.md) para obter mais informações.
-   Os protocolos que você está consultando. Isso só é necessário quando você está procurando dispositivos por uma rede com ou sem fio. Para saber mais sobre como fazer isso, consulte [Enumerate devices over a network](enumerate-devices-over-a-network.md).

Ao usar as APIs [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), você frequentemente combinará o seletor de dispositivo com o tipo de dispositivo em que você está interessado. A lista disponível de tipos de dispositivo é definida pela enumeração [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind). Essa combinação de fatores ajuda você a limitar os dispositivos que estão disponíveis para aqueles em que você está interessado. Se você não especificar o **DeviceInformationKind** ou se o método que você está usando não oferecer um parâmetro **DeviceInformationKind**, o tipo padrão será **DeviceInterface**.

As APIs [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) usam sintaxe AQS Canonical, mas nem todos os operadores são suportados. Para obter uma lista de propriedades que estão disponíveis quando você constrói sua cadeia de caracteres de filtro, consulte [Device information properties](device-information-properties.md).

**Cuidado**  Propriedades personalizadas que são definidas usando o formato de `{GUID} PID` não podem ser usadas durante a construção da cadeia de caracteres de filtro AQS. Isso é porque o tipo de propriedade é derivado do nome da propriedade conhecida.

 

A tabela a seguir lista os operadores AQS e os tipos de parâmetros a que dão suporte.

| Operador                       | Tipos com suporte                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_igual**                 | Cadeia de caracteres, booliano, GUID, UInt16, UInt32                                       |
| **COP\_não igual**              | Cadeia de caracteres, booliano, GUID, UInt16, UInt32                                       |
| **COP\_LESSTHAN**              | UInt16, UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16, UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **COP\_valor\_contém**       | Cadeia de caracteres, matriz de cadeia de caracteres, matriz booliana, matriz GUID, matriz UInt16, matriz UInt32 |
| **COP\_valor\_não contém**    | Cadeia de caracteres, matriz de cadeia de caracteres, matriz booliana, matriz GUID, matriz UInt16, matriz UInt32 |
| **COP\_valor\_STARTSWITH**     | Cadeia de caracteres                                                                      |
| **COP\_valor\_ENDSWITH**       | Cadeia de caracteres                                                                      |
| **COP\_DOSWILDCARDS**          | Sem suporte                                                               |
| **COP\_WORD\_igual**           | Sem suporte                                                               |
| **COP\_WORD\_STARTSWITH**      | Sem suporte                                                               |
| **COP\_aplicativo\_específico** | Sem suporte                                                               |


> **Dica**  você pode especificar **NULL** para **COP\_igual** ou **COP\_não igual**. Isso se traduz em uma propriedade sem valor ou cujo valor não existe. Em AQS, você especifica **NULL** usando colchetes vazios \[\].

> **Importante**  ao usar o **valor de COP\_\_contém** e **COP\_valor\_não contém** operadores, eles se comportam de forma diferente com cadeias de caracteres e matrizes de cadeia de seqüências. No caso de uma cadeia de caracteres, o sistema fará uma pesquisa sem diferenciação de maiúsculas de minúsculas para ver se o dispositivo contém a cadeia de caracteres indicada como uma subcadeia de caracteres. No caso de uma matriz de cadeia de caracteres, as subcadeias de caracteres não são pesquisadas. Com a matriz de cadeia de caracteres, a matriz é pesquisada para ver se ela contém a cadeia de caracteres especificada inteira. Não é possível pesquisar uma matriz de cadeia de caracteres para verificar se os elementos na matriz contém uma subcadeia de caracteres.

Se você não puder criar uma cadeia de caracteres de filtro AQS única que analise seus resultados de forma apropriada, poderá filtrar os resultados depois de recebê-los. Entretanto, se você escolher fazer isso, recomendamos que limite os resultados da sua cadeia de caracteres de filtro AQS inicial o quanto puder quando a fornecer para as APIs [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration). Isso ajudará a melhorar o desempenho do seu aplicativo.

## <a name="aqs-string-examples"></a>Exemplos de cadeia de caracteres AQS

Os exemplos a seguir demonstram como a sintaxe AQS pode ser usada para limitar dispositivos que você quer enumerar. Todas essas cadeias de caracteres de filtro são emparelhadas com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) para criar um filtro completo. Se nenhum tipo for especificado, lembre-se de que o tipo padrão é **DeviceInterface**.

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) de **DeviceInterface**, ele enumera todos os objetos que contêm a classe da interface de Captura de Áudio e os que estão habilitados no momento. **=** converte para **COP\_é igual**a.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) de **Device**, ele enumera todos os objetos que têm pelo menos um ID de hardware do GenCdRom. **~~** converte para o **valor de COP\_\_contém**.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) de **DeviceContainer**, ele enumera todos os objetos que tem um nome de modelo contendo a subcadeia de caracteres da Microsoft. **~~** converte para o **valor de COP\_\_contém**.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) de **DeviceInterface**, ele enumera todos os objetos que tem um nome que começa com a subcadeia de caracteres da Microsoft. **~&lt;** converte para **COP\_STARTSWITH**.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) de **Device**, ele enumera todos os objetos que têm um conjunto de propriedade **System.Devices.IpAddress**. **&lt;&gt;\[\]** se traduz em **COP\_** não é combinado com um valor **nulo** .

``` syntax
System.Devices.IpAddress:<>[]
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) de **Device**, ele enumera todos os objetos que não têm um conjunto de propriedade **System.Devices.IpAddress**. **=\[\]** se traduz em **COP\_é igual** a combinado com um valor **nulo** .

``` syntax
System.Devices.IpAddress:=[]
```

 

 
