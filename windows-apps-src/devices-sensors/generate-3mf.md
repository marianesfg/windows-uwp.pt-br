---
Description: Descreve a estrutura do tipo de arquivo de Formato de Manufatura 3D e como ele pode ser criado e manipulado com a API Windows.Graphics.Printing3D.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Gerar um pacote 3MF
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c69e8618aaba58fd3b8de163ce990371bbf3c945
ms.sourcegitcommit: ce7610916fd662d4bb95d4bfe5c4cf0e45303014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71681990"
---
# <a name="generate-a-3mf-package"></a>Gerar um pacote 3MF

**APIs importantes**

-   [**Windows. Graphics. Printing3D**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d)

Este guia descreve a estrutura do tipo de arquivo de Formato de Manufatura 3D e como ele pode ser criado e manipulado com a API [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d).

## <a name="what-is-3mf"></a>O que é 3MF?

O Formato de Manufatura 3D é um conjunto de convenções para usar XML para descrever a aparência e a estrutura de modelos 3D para fins de fabricação (impressão em 3D). Ele define um conjunto de partes (algumas necessárias e outras opcionais) e suas relações, com o objetivo de fornecer todas as informações necessárias a um dispositivo de fabricação 3D. Um conjunto de dados que segue o Formato de Manufatura 3D pode ser salvo como um arquivo com a extensão .3mf.

No Windows 10, a classe [**Printing3D3MFPackage**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3d3mfpackage) no namespace **Windows. Graphics. Printing3D** é análoga a um único arquivo. 3mf e outras classes são mapeadas para os elementos XML específicos no arquivo. Este guia descreve como cada uma das partes principais de um documento 3MF pode ser criada e definida de forma programática, como a Extensão de Materiais 3MF pode ser utilizada e como um objeto **Printing3D3MFPackage** pode ser convertido e salvo como um arquivo .3mf. Para saber mais sobre os padrões de 3MF ou a Extensão de Materiais 3MF, consulte a [Especificação 3MF](https://3mf.io/what-is-3mf/3mf-specification/).

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>Classes principais na estrutura 3MF

A classe **Printing3D3MFPackage** representa um documento 3MF completo, e no núcleo de um documento 3MF está sua parte do modelo, representada pela classe [**Printing3DModel**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3dmodel). A maioria das informações que desejamos especificar sobre um modelo 3D será armazenada por meio da definição das propriedades da classe **Printing3DModel** e das propriedades das classes subjacentes.

[!code-cs[InitClasses](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetInitClasses)]

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>Metadados

A parte do modelo de documento 3MF pode conter metadados na forma de pares de chave/valor de cadeias de caracteres armazenadas na propriedade **Metadados**. Há um número de nomes de metadados predefinidos, mas outros pares podem ser adicionados como parte de uma extensão (descrito mais detalhadamente em [Especificação 3MF](https://3mf.io/what-is-3mf/3mf-specification/)). Fica a cargo do receptor do pacote (um dispositivo de fabricação 3D) determinar se e como lidar com metadados, mas é uma prática recomendada incluir o máximo de informações básicas possível no pacote 3MF:

[!code-cs[Metadata](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMetadata)]

## <a name="mesh-data"></a>Dados de malha

No contexto deste guia, uma malha é um corpo de geometria tridimensional construído a partir de um único conjunto de vértices (embora ela não tenha que aparecer como um sólido único). Uma parte de malha é representada pela classe [**Printing3DMesh**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3dmesh). Um objeto de malha válido deve conter informações sobre a localização de todos os seus vértices, bem como todas as faces de triângulo que existem entre determinados conjuntos de vértices.

O método a seguir adiciona vértices a uma malha e dá a eles locais no espaço 3D:

[!code-cs[Vertices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetVertices)]

O próximo método define todos os triângulos a serem desenhados nesses vértices:

[!code-cs[TriangleIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTriangleIndices)]

> [!NOTE]
> Todos os triângulos devem ter seus índices definidos no sentido anti-horário (ao exibir o triângulo fora do objeto de malha), para que seus vetores de face normal apontem para fora.

Quando um objeto Printing3DMesh contém conjuntos de vértices e triângulos válidos, ele deve ser adicionado à propriedade **Malhas** do modelo. Todos os objetos **Printing3DMesh** em um pacote devem ser armazenados na propriedade **Malhas** da classe **Printing3DModel**.

[!code-cs[MeshAdd](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMeshAdd)]


## <a name="create-materials"></a>Criar materiais


Um modelo 3D pode armazenar dados de vários materiais. Essa convenção destina-se a tirar proveito dos dispositivos de fabricação 3D que podem usar vários materiais em uma única tarefa de impressão. Há também vários *tipos* de grupos de materiais, cada um capaz de oferecer suporte a um número de materiais individuais diferentes. Cada grupo de material deve ter um número de id de referência exclusivo, e cada material desse grupo também deve ter uma id exclusiva.

Os diferentes objetos de malha dentro de um modelo podem fazer referência a esses materiais. Além disso, triângulos individuais em cada malha podem especificar diferentes materiais. E mais, materiais diferentes podem ser representados dentro de um triângulo único, com cada vértice do triângulo tendo um material diferente atribuído a ele e o material de face calculado como o gradiente entre eles.

Este guia mostrará primeiramente como criar diferentes tipos de materiais dentro de seus respectivos grupos de materiais e armazená-los como recursos no objeto de modelo. Em seguida, atribuiremos materiais diferentes a malhas individuais e triângulos individuais.

### <a name="base-materials"></a>Materiais de base

O tipo de material padrão é **Material de base**, que tem tanto um valor de **Material de cor** (descrito abaixo) quanto um atributo de nome que se destina a especificar o *tipo* de material a ser usado.

[!code-cs[BaseMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetBaseMaterialGroup)]

> [!NOTE]
> o dispositivo de fabricação 3D determinará quais materiais físicos disponíveis são mapeados para quais elementos de material virtual armazenados no 3MF. Não é necessário que o mapeamento de material seja 1:1: se uma impressora 3D usa apenas um material, ela imprimirá todo o modelo nesse material, independentemente de quais objetos ou faces sejam atribuídos a materiais diferentes.

### <a name="color-materials"></a>Materiais de cor

**Materiais de cor** são semelhantes aos **Materiais de base**, mas eles não contêm um nome. Assim, não dão nenhuma instrução sobre que tipo de material deve ser usado pela máquina. Eles contêm apenas dados de cor e permitem que a máquina escolha o tipo de material (e a máquina pode solicitar que o usuário escolha). No código a seguir, os objetos `colrMat` do método anterior são usados de forma isolada.

[!code-cs[ColorMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetColorMaterialGroup)]

### <a name="composite-materials"></a>Materiais de composição

**Materiais de composição** simplesmente instruem o dispositivo de fabricação a usar uma mistura uniforme de diferentes **Materiais de base**. Cada **Grupo de Material de Composição** deve fazer referência a exatamente um **Grupo de Material de Base** do qual retira ingredientes. Além disso, os **Materiais de base** dentro desse grupo que serão disponibilizados devem estar em uma lista de **Índices de Materiais**, na qual cada **Material de composição** fará referência ao especificar as proporções (cada **Material de composição** é simplesmente uma proporção de **Materiais de Base**).

[!code-cs[CompositeMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetCompositeMaterialGroup)]

### <a name="texture-coordinate-materials"></a>Materiais de coordenadas de textura

3MF dá suporte ao uso de imagens 2D para colorir as superfícies de modelos 3D. Dessa forma, o modelo pode transmitir muito mais dados de cor por face de triângulo (ao contrário de ter apenas um valor de cor por vértice de triângulo). Semelhante a **Materiais de cor**, materiais de coordenadas de textura transmitem apenas dados de cor. Para usar uma textura 2D, um recurso de textura deve ser declarado primeiramente:

[!code-cs[TextureResource](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTextureResource)]

> [!NOTE]
> Os dados de textura pertencem ao pacote 3MF em si, não à parte do modelo dentro do pacote.

Em seguida, devemos preencher **Materiais Texture3Coord**. Cada um deles faz referência a um recurso de textura e especifica um ponto em particular na imagem (em coordenadas UV).

[!code-cs[Texture2CoordMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTexture2CoordMaterialGroup)]

## <a name="map-materials-to-faces"></a>Mapear materiais para faces

Para ditar quais materiais são mapeados para quais vértices em cada triângulo, precisamos trabalhar um pouco mais no objeto de malha do nosso modelo (se um modelo contiver várias malhas, cada uma delas deve ter seus materiais atribuídos separadamente). Conforme mencionado acima, materiais são atribuídos por vértice, por triângulo. Consulte o código a seguir para ver como essas informações são inseridas e interpretadas.

[!code-cs[MaterialIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMaterialIndices)]

## <a name="components-and-build"></a>Componentes e compilação

A estrutura do componente permite que o usuário coloque mais de um objeto de malha em um modelo 3D imprimível. O objeto [**Printing3DComponent**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3dcomponent) contém uma única malha e uma lista de referências a outros componentes. Isso é realmente uma lista de objetos [**Printing3DComponentWithMatrix**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3dcomponentwithmatrix). Cada um dos objetos **Printing3DComponentWithMatrix** contém um **Printing3DComponent** e, principalmente, uma matriz de transformação que é aplicável à malha e componentes contidos do dito **Printing3DComponent**.

Por exemplo, um modelo de um carro pode consistir em um "Corpo" **Printing3DComponent** que mantém a malha para o corpo do carro. O componente "Corpo" pode conter referências a quatro objetos **Printing3DComponentWithMatrix** diferentes, que fazem referência ao mesmo **Printing3DComponent** com a malha "Roda" e conter quatro matrizes de transformação diferente (mapeando as rodas para quatro posições diferentes no corpo do carro). Nesse cenário, a malha "Corpo" e a malha "Roda" só precisariam ser armazenadas uma vez, embora o produto final apresentasse cinco malhas no total.

Todos os objetos **Printing3DComponent** devem fazer referência diretamente na propriedade **Componentes** do modelo. Um componente específico a ser usado no trabalho de impressão é armazenado na propriedade **Compilar**.

[!code-cs[Components](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetComponents)]

## <a name="save-package"></a>Salvar um pacote
Agora que temos um modelo, com materiais definidos e componentes, podemos salvá-lo no pacote.

[!code-cs[SavePackage](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSavePackage)]

Essa função garante que a textura seja especificada corretamente.

[!code-cs[FixTexture](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetFixTexture)]

Daqui, podemos iniciar um trabalho de impressão dentro do aplicativo (veja [Impressão 3D a do seu aplicativo](https://docs.microsoft.com/windows/uwp/devices-sensors/3d-print-from-app)) ou salvar esse **Printing3D3MFPackage** como um arquivo .3mf.

O método a seguir pega um **Printing3D3MFPackage** concluído e salva os dados em um arquivo .3mf.

[!code-cs[SaveTo3mf](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSaveTo3mf)]

## <a name="related-topics"></a>Tópicos relacionados

[impressão 3D de seu aplicativo](https://docs.microsoft.com/windows/uwp/devices-sensors/3d-print-from-app)  
[exemplo de UWP de impressão 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 
