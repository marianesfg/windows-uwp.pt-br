---
title: Recursos de amostragem de textura de recursos de streaming
description: Os recursos de amostragem de textura de recursos de streaming incluem obter feedback de status do sombreador sobre as áreas mapeadas, verificar se todos os dados que estão sendo acessados foram mapeados no recurso, fixação para ajudar os sombreadores a evitar áreas em recursos de streaming mipmapped que são conhecidos por não serem mapeados, e descobrir o que será o LED mínimo que está totalmente mapeado para um volume de filtro de textura inteiro.
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Recursos de amostragem de textura de recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0066d38aaa3f5802ff5b1d380d405e60d90cad49
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6267419"
---
# <a name="streaming-resources-texture-sampling-features"></a>Recursos de amostragem de textura de recursos de streaming


Os recursos de amostragem de textura de recursos de streaming incluem obter feedback de status do sombreador sobre as áreas mapeadas, verificar se todos os dados que estão sendo acessados foram mapeados no recurso, fixação para ajudar os sombreadores a evitar áreas em recursos de streaming mipmapped que são conhecidos por não serem mapeados, e descobrir o que será o LED mínimo que está totalmente mapeado para um volume de filtro de textura inteiro.

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Requisitos dos recursos de amostragem de textura de recursos de streaming


Os recursos de amostragem de textura descritos aqui exigem o nível [Camada 2](tier-2.md) de suporte a recursos de streaming.

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Comentários de status do sombreador sobre áreas mapeadas


Qualquer instrução do sombreador que lê e/ou grava em um recurso de streaming faz com que informações de status sejam registradas. Esse status é exposto como um valor de retorno extra opcional em cada instrução de acesso do recurso que entra em um registro temporário de 32 bits. O conteúdo do valor de retorno é opaco. Ou seja, leitura direta pelo programa de sombreador não é permitida. Mas, você pode usar a função [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) para extrair as informações de status.

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Seleção totalmente mapeada


A função [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) interpreta o status retornado de um acesso de memória e indica se todos os dados que estão sendo acessados foram mapeados no recurso. **CheckAccessFullyMapped** retorna true (0xFFFFFFFF) se os dados foram mapeados ou false (0x00000000) se os dados não foram não mapeados.

Durante as operações de filtragem, às vezes, o peso de um determinado texel acaba sendo 0,0. Um exemplo é uma amostra linear com coordenadas de textura que se enquadram diretamente em um centro de texel: 3 outros texels (os quais podem variar por hardware) contribuem para o filtro, mas com peso 0. Esses texels de peso 0 não contribuem em nada com o resultado do filtro, sendo assim, se eles se encontrarem em blocos **NULL**, eles não serão contados como um acesso não mapeado. Observe que a mesma garantia se aplica aos filtros de textura que incluem diversos níveis de mip; se os texels em um dos mipmaps não for mapeado mas o peso desses texels for 0, esses texels não serão contados como acesso não mapeado.

Ao fazer a amostragem de um formato que tem menos de 4 componentes (por exemplo, DXGI\_FORMAT\_R8\_UNORM), qualquer texel que se enquadrar nos blocos **NULL** resultará em um acesso mapeado **NULL** sendo relatado, independentemente de quais componentes o sombreador realmente examina no resultado. Por exemplo, fazer a leitura de R8\_UNORM e mascarar o resultado da leitura no sombreador com .gba/.yzw faz com que não pareça ser necessário ler a textura. Mas, se o endereço do texel for um bloco mapeado **NULL**, a operação ainda conta como um acesso de mapa **NULL**.

O sombreador pode verificar o status e buscar qualquer curso de ação desejado em caso de falha. Por exemplo, um curso de ação pode ser o registro de "erros" (por exemplo, via gravação UAV) e/ou a emissão de outra leitura fixada a um LOD áspero conhecido por estar mapeado. Um aplicativo pode querer controlar acessos bem-sucedidos também para obter uma noção de qual parte do conjunto de blocos mapeado foi acessado.

Uma complicação para registrar em log é que não existe nenhum mecanismo para relatar o conjunto exato de blocos que teria sido acessado. O aplicativo pode fazer suposições conservadoras com base no conhecimento das coordenadas usadas para o acesso, bem como usando a instrução LOD; por exemplo, [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)) retorna o cálculo de LOD de hardware.

Outra complicação é que muitos acessos ocorrerão nos mesmos blocos, portanto, ocorrerão muitos registros em log redundantes e possivelmente uma contenção de memória. Ele pode ser conveniente se o hardware puder ter a opção de não precisar relatar os acessos de bloco se eles tiverem sido relatados em outro lugar antes. Talvez o estado de tal rastreamento pudesse ser redefinido na API (provavelmente nos limites do quadro).

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Clamp de MinLOD por amostra


Para evitar os sombreadores de áreas de recursos de streaming mipmapped que são conhecidos por não estarem mapeados, a maioria das instruções do sombreador que envolvem o uso de uma amostra (filtragem) têm um modo que permite que o sombreador passe um parâmetro de clamp MinLOD float32 adicional para a amostra de textura. Esse valor encontra-se no espaço numérico do modo de exibição mipmap, em oposição ao recurso subjacente.

O hardware executa` max(fShaderMinLODClamp,fComputedLOD) `no mesmo local no cálculo LOD onde ocorre o clamp de MinLOD por recurso, que também é um [**max**](https://msdn.microsoft.com/library/windows/desktop/bb509624)().

Se o resultado da aplicação do clamp LOD por amostra e quaisquer outros clamps LOD definidos na amostra for de um conjunto vazio, o resultado será o mesmo resultado de acesso sem limites que o clamp minLOD por recurso: 0 para componentes no formato de superfície e padrões para os componentes que estão faltando.

A instrução LOD (por exemplo, [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)), que antecede o clamp minLOD por amostra descrito aqui, retorna um LOD vinculado e não vinculado. O LOD vinculado retornado dessa instrução LOD reflete todas as vinculações, incluindo a vinculação por recurso, mas não uma vinculação por amostra. A vinculação por amostra é controlada e conhecida pelo sombreador de qualquer maneira, portanto, o autor do sombreador pode aplicar manualmente essa vinculação ao valor de retorno da instrução LOD, se desejado.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtragem de redução de min/max


Aplicativos podem optar por gerenciar suas próprias estruturas de dados que os informa sobre a aparência dos mapeamentos para um recurso de streaming. Um exemplo seria uma superfície que contém um texel para armazenar informações para cada bloco em um recurso de streaming. Um pode armazenar o primeiro LOD que é mapeado em um determinado local de bloco. Pela amostragem cuidadosa dessa estrutura de dados de uma forma semelhante àquela em que o recurso de streaming se destina a ser amostrado, pode-se descobrir o que será o LOD mínimo que está totalmente mapeado para um volume de filtro de textura inteiro. Para ajudar a facilitar esse processo, o Direct3D 11.2 apresenta um novo modo de amostra de finalidade geral, a filtragem min/max

O utilitário de filtragem min/max para o rastreamento LOD pode ser útil para outras finalidades, como, por exemplo, a filtragem de superfícies de profundidade.

A filtragem de redução min/max é um modo em amostras que busca o mesmo conjunto de texels que um filtro de textura normal buscaria. Mas, em vez de mesclar os valores para produzir uma resposta, ele retorna o min() ou max() de texels buscados, em uma base por componente (por exemplo, o mínimo de todos os valores de R, separadamente do mínimo de todos os valores de G e assim por diante).

As operações de min/max seguem regras de precisão aritmética de Direct3D. A ordem das comparações não importa.

Durante as operações de filtragem que não são min/max, às vezes, o peso de um determinado texel acaba sendo 0,0. Um exemplo é uma amostra linear com coordenadas de textura que se enquadram diretamente em um centro de texel; nesse caso, 3 outros texels (os quais podem variar por hardware) contribuem para o filtro, mas com peso 0. Para qualquer um desses texels que teria peso 0 em um filtro sem a função min/max, se o filtro for min/max, esses texels ainda não contribuem para o resultado (e os pesos não afetam de outra forma a operação de filtro min/max).

O suporte para esse recurso depende do suporte de [Camada 2](tier-2.md) para recursos de streaming.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Acesso pipeline aos recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




