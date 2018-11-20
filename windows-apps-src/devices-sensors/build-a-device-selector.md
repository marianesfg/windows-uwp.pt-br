---
author: muhsinking
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: Criar um seletor de dispositivo
description: Construir um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 036ea8b7d9797112dca9b6594e9bc1e33e923588
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7291143"
---
# <a name="build-a-device-selector"></a>Criar um seletor de dispositivo



**APIs importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Construir um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos. Isso permitirá obter apenas resultados relevantes e também melhorará o desempenho do sistema. Na maioria das situações, você obtém um seletor de dispositivo de uma pilha de dispositivo. Por exemplo, você pode usar [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Dn264015) para dispositivos descobertos por USB. Esses seletores de dispositivo retornam uma cadeia de caracteres de Sintaxe de Consulta Avançada (AQS). Se você não estiver familiarizado com o formato AQS, leia [Usando a Sintaxe de Consulta Avançada de forma programática](https://msdn.microsoft.com/library/windows/desktop/Bb266512).

## <a name="building-the-filter-string"></a>Criando a cadeia de caracteres de filtro

Há alguns casos em que você precisa enumerar dispositivos, e um seletor de dispositivo fornecido não está disponível para seu cenário. Um seletor de dispositivo é uma cadeia de caracteres de filtro AQS que contém as informações a seguir. Antes de criar uma cadeia de caracteres de filtro, você precisa saber algumas partes importantes de informações sobre os dispositivos que você deseja enumerar.

-   O [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) dos dispositivos em que você está interessado. Para saber mais sobre como o **DeviceInformationKind** impacta a enumeração de dispositivos, consulte [Enumerate devices](enumerate-devices.md).
-   Como criar uma cadeia de caracteres de filtro AQS, que é explicada neste tópico.
-   As propriedades em que você está interessado. As propriedades disponíveis dependerão do [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991). Consulte [Device information properties](device-information-properties.md) para obter mais informações.
-   Os protocolos que você está consultando. Isso só é necessário quando você está procurando dispositivos por uma rede com ou sem fio. Para saber mais sobre como fazer isso, consulte [Enumerate devices over a network](enumerate-devices-over-a-network.md).

Ao usar as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), você frequentemente combinará o seletor de dispositivo com o tipo de dispositivo em que você está interessado. A lista disponível de tipos de dispositivo é definida pela enumeração [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991). Essa combinação de fatores ajuda você a limitar os dispositivos que estão disponíveis para aqueles em que você está interessado. Se você não especificar o **DeviceInformationKind** ou se o método que você está usando não oferecer um parâmetro **DeviceInformationKind**, o tipo padrão será **DeviceInterface**.

As APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) usam sintaxe AQS Canonical, mas nem todos os operadores são suportados. Para obter uma lista de propriedades que estão disponíveis quando você constrói sua cadeia de caracteres de filtro, consulte [Device information properties](device-information-properties.md).

**Cuidado**as propriedades personalizadas que são definidas usando o `{GUID} PID` formato não pode ser usado ao construir a sua cadeia de caracteres de filtro AQS. Isso é porque o tipo de propriedade é derivado do nome da propriedade conhecida.

 

A tabela a seguir lista os operadores AQS e os tipos de parâmetros a que dão suporte.

| Operador                       | Tipos com suporte                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_EQUAL**                 | Cadeia de caracteres, booliano, GUID, UInt16, UInt32                                       |
| **COP\_NOTEQUAL**              | Cadeia de caracteres, booliano, GUID, UInt16, UInt32                                       |
| **COP\_LESSTHAN**              | UInt16, UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16, UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **COP\_VALUE\_CONTAINS**       | Cadeia de caracteres, matriz de cadeia de caracteres, matriz booliana, matriz GUID, matriz UInt16, matriz UInt32 |
| **COP\_VALUE\_NOTCONTAINS**    | Cadeia de caracteres, matriz de cadeia de caracteres, matriz booliana, matriz GUID, matriz UInt16, matriz UInt32 |
| **COP\_VALUE\_STARTSWITH**     | Cadeia de caracteres                                                                      |
| **COP\_VALUE\_ENDSWITH**       | Cadeia de caracteres                                                                      |
| **COP\_DOSWILDCARDS**          | Sem suporte                                                               |
| **COP\_WORD\_EQUAL**           | Sem suporte                                                               |
| **COP\_WORD\_STARTSWITH**      | Sem suporte                                                               |
| **COP\_APPLICATION\_SPECIFIC** | Sem suporte                                                               |


> **Dica**você pode especificar **Nulo** para **COP\_EQUAL** ou **COP\_NOTEQUAL**. Isso se traduz em uma propriedade sem valor ou cujo valor não existe. No AQS, você especifica **NULL** usando colchetes vazios \[\].

> **Importante**ao usar os operadores **COP\_VALUE\_CONTAINS** e **COP\_VALUE\_NOTCONTAINS** , eles se comportam de forma diferente com cadeias de caracteres e matrizes de cadeia de caracteres. No caso de uma cadeia de caracteres, o sistema fará uma pesquisa sem diferenciação de maiúsculas de minúsculas para ver se o dispositivo contém a cadeia de caracteres indicada como uma subcadeia de caracteres. No caso de uma matriz de cadeia de caracteres, as subcadeias de caracteres não são pesquisadas. Com a matriz de cadeia de caracteres, a matriz é pesquisada para ver se ela contém a cadeia de caracteres especificada inteira. Não é possível pesquisar uma matriz de cadeia de caracteres para verificar se os elementos na matriz contém uma subcadeia de caracteres.

Se você não puder criar uma cadeia de caracteres de filtro AQS única que analise seus resultados de forma apropriada, poderá filtrar os resultados depois de recebê-los. Entretanto, se você escolher fazer isso, recomendamos que limite os resultados da sua cadeia de caracteres de filtro AQS inicial o quanto puder quando a fornecer para as APIs [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459). Isso ajudará a melhorar o desempenho do seu aplicativo.

## <a name="aqs-string-examples"></a>Exemplos de cadeia de caracteres AQS

Os exemplos a seguir demonstram como a sintaxe AQS pode ser usada para limitar dispositivos que você quer enumerar. Todas essas cadeias de caracteres de filtro são emparelhadas com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) para criar um filtro completo. Se nenhum tipo for especificado, lembre-se de que o tipo padrão é **DeviceInterface**.

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **DeviceInterface**, ele enumera todos os objetos que contêm a classe da interface de Captura de Áudio e os que estão habilitados no momento. **=
              ** é convertido para **COP\_EQUALS**.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **Device**, ele enumera todos os objetos que têm pelo menos um ID de hardware do GenCdRom. **~~
              ** é convertido para **COP\_VALUE\_CONTAINS**.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **DeviceContainer**, ele enumera todos os objetos que tem um nome de modelo contendo a subcadeia de caracteres da Microsoft. **~~
              ** é convertido para **COP\_VALUE\_CONTAINS**.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **DeviceInterface**, ele enumera todos os objetos que tem um nome que começa com a subcadeia de caracteres da Microsoft. **~&lt;
              ** é convertido para **COP\_STARTSWITH**.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **Device**, ele enumera todos os objetos que têm um conjunto de propriedade **System.Devices.IpAddress**. **&lt;&gt;\[\]** é convertido para **COP\_NOTEQUALS** combinado com um valor **NULL**.

``` syntax
System.Devices.IpAddress:<>[]
```

Quando esse filtro é emparelhado com um [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **Device**, ele enumera todos os objetos que não têm um conjunto de propriedade **System.Devices.IpAddress**. **=\[\]** é convertido para **COP\_EQUALS** combinado com um valor **NULL**.

``` syntax
System.Devices.IpAddress:=[]
```

 

 
