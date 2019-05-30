---
title: Nível 2
description: O suporte do nível 2 a recursos de streaming adiciona funcionalidades além do nível 1, como a garantia de um mipmap de textura não empacotada quando o tamanho é pelo menos uma forma de bloco padrão; instruções de sombreador para fixação de nível de detalhe (LOD) e para a obtenção de status sobre a operação de sombreador; Além disso, a leitura de blocos mapeados em NULL trata o valor de amostragem como zero.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Nível 2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c48b02de34bd37acced8ef65859708f31fd78ca2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370866"
---
# <a name="tier-2"></a>Nível 2


O suporte do nível 2 a recursos de streaming adiciona funcionalidades além do nível 1, como a garantia de um mipmap de textura não empacotada quando o tamanho é pelo menos uma forma de bloco padrão; instruções de sombreador para fixação de nível de detalhe (LOD) e para a obtenção de status sobre a operação de sombreador; Além disso, a leitura de blocos mapeados em NULL trata o valor de amostragem como zero.

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Suporte geral de camada 2


O suporte de Nível 2 inclui o seguinte:

-   Hardware em nível de recurso mínimo 11.1.
-   Todos os recursos do nível anterior (sem as limitações específicas do [Nível 1](tier-1.md)) além de adições aos itens a seguir:
-   As instruções do sombreador para fixação de nível de detalhe e feedback de status mapeado estão disponíveis. Consulte [Exposição de recursos de streaming HLSL](hlsl-streaming-resources-exposure.md).

Além desses, há alguns problemas de suporte específicos que serão descritos a seguir.

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Blocos não mapeado


As leituras de blocos não mapeados retornam 0 em todos os componentes não ausentes do formato e o padrão para os componentes ausentes.

As gravações em blocos não mapeados são impedidas de chegar à memória, mas podem acabar parando em caches que as leituras subsequentes para o mesmo endereço podem ou não capturar.

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Filtragem de textura


A filtragem de textura com um volume que abrange blocos **NULOS** e não **NULOS** contribui para 0 (com padrões para componentes de formato ausentes) de texels em blocos **NULOS** na operação de filtragem geral. Alguns hardwares mais antigos não atender a esse requisito e retornam 0 (com padrões para componentes de formato ausentes) para o resultado de filtro completo se quaisquer texels (com peso diferente de zero) se enquadrarem em um bloco **NULO**. Nenhum outro hardware terá permissão para não atender ao requisito de incluir todos os texels (peso diferente de zero) na operação de filtragem.

Os acessos a texels **NULOS** fazem com que a operação [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) no feedback de status de uma leitura de textura retorne false. Isso não depende de como o resultado do acesso à textura pode fazer com que a gravação seja mascarada no sombreador e de quantos componentes estão no formato da textura (a combinação deles pode dar a impressão de que a textura não precisa ser acessada).

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Restrições de alinhamento


Restrições de alinhamento para formas de bloco padrão: Mipmaps que preenchem a pelo menos um bloco padrão em todas as dimensões são garantidos para usar a padrão lado a lado, com o restante considerado empacotada como uma **unidade** em blocos de N (N relatado para o aplicativo). O aplicativo pode mapear os blocos N para locais arbitrariamente separados em um pool de blocos, mas deve mapear todos ou nenhum dos blocos compactados. A compactação MIP é um conjunto exclusivo de blocos compactados por fatia de matriz.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtragem de redução de Min/Max


Há suporte para a filtragem de redução mínima/máxima. Consulte [Recursos de amostragem de textura de recursos de streaming](streaming-resources-texture-sampling-features.md).

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Limitações


Recursos de streaming com mipmaps menores que o tamanho do bloco padrão em qualquer dimensão não podem ter um tamanho de matriz maior que 1.

Ainda se aplicam limitações sobre como os blocos podem ser acessados quando há mapeamentos duplicados. Consulte [Limitações de acesso a blocos com mapeamentos duplicados](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[As camadas de recursos de recursos de streaming](streaming-resources-features-tiers.md)

 

 




