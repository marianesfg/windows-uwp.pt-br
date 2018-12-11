---
title: Escolhendo um recurso
description: Um recurso é uma coleção de dados que são usados pelo pipeline de 3D.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Escolhendo um recurso
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ccc99395dba2f2d1894db81fb48abb59f9a8ba4f
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8899010"
---
# <a name="choosing-a-resource"></a>Escolhendo um recurso


Um recurso é uma coleção de dados que são usados pelo pipeline de 3D. Criar recursos e definir seu comportamento é o primeiro passo para programar seu app. Este guia aborda tópicos básicos para escolher os recursos exigidos pelo seu app.

## <a name="span-ididentifybindingspanspan-ididentifybindingspanspan-ididentifybindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identificar os estágios de pipeline que precisam de recursos


A primeira etapa é escolher o estágio de [pipeline gráfico](graphics-pipeline.md) (ou estágios) que usa um recurso. Ou seja, identificar cada estágio que fará a leitura de dados de um recurso, bem como os estágios que gravarão dados em um recurso. Conhecer os estágios de pipeline onde os recursos serão usados determina as APIs que serão chamadas para associar o recurso ao estágio.

Esta tabela lista os tipos de recursos que podem ser associados a cada estágio do pipeline. Ela inclui se o recurso pode ser vinculado como uma entrada ou uma saída.

| Estágio de pipeline  | Entrada/saída | Recurso               | Tipo de recurso                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Assembler de entrada | Entrada     | Buffer de vértice          | Buffer                                  |
| Assembler de entrada | Entrada     | Buffer de índice           | Buffer                                  |
| Estágios de sombreador   | Entrada     | Shader-ResourceView    | Buffer, Texture1D, Texture2D, Texture3D |
| Estágios de sombreador   | Entrada     | Buffer de constantes de sombreador | Buffer                                  |
| Saída de fluxo   | Saída    | Buffer                 | Buffer                                  |
| Fusão de saída   | Saída    | Modo de exibição de destino de renderização     | Buffer, Texture1D, Texture2D, Texture3D |
| Fusão de saída   | Saída    | Modo de exibição de estêncil/profundidade     | Texture1D, Texture2D                    |

 

## <a name="span-ididentifyusagespanspan-ididentifyusagespanspan-ididentifyusagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Identificar como cada recurso será usado


Depois de escolher os estágios de pipeline que seu app usará (e, portanto, os recursos que cada estágio exigirá), a próxima etapa é determinar como cada recurso será usado, ou seja, se um recurso pode ser acessado por CPU ou GPU.

O hardware em que seu app está sendo executado terá um mínimo de pelo menos uma CPU e uma GPU. Para escolher um valor de uso, considere qual tipo de processador precisa ler ou gravar no recurso a partir das opções a seguir.

| Uso de recursos | Pode ser atualizado por                    | Frequência de atualização |
|----------------|--------------------------------------|---------------------|
| Padrão        | GPU                                  | com pouca frequência        |
| Dinâmico        | CPU                                  | com frequência          |
| Preparando        | GPU                                  | n/a                 |
| Imutável      | CPU (somente no momento da criação de recursos) | n/a                 |

 

O uso padrão deve ser usado para um recurso que deve ser atualizado pela CPU com pouca frequência (menor do que uma vez por quadro). Idealmente, a CPU nunca escreveria diretamente em um recurso com o uso padrão para evitar possíveis penalidades de desempenho.

O uso dinâmico deve ser usado para um recurso que a CPU atualiza relativamente com frequência (uma vez ou mais por quadro). Um cenário típico para um recurso dinâmico seria criar buffers de índice e vértice dinâmico que podem ser preenchidos em tempo de execução com dados sobre a geometria visível do ponto de vista do usuário para cada quadro. Esses buffers seriam usados para renderizar somente a geometria visível ao usuário para esse quadro.

O uso de preparo deve ser usado para copiar dados de e para outros recursos. Um cenário típico seria copiar dados em um recurso com o uso padrão (que a CPU não consegue acessar) para um recurso com o uso de preparo (que pode acessar a CPU).

Recursos imutáveis devem ser usados quando os dados no recurso não forem sofrer alteração.

Outra maneira de observar a mesma ideia é pensar o que um app faz com um recurso.

| Como o app usa o recurso     | Uso de recursos       |
|---------------------------------------|----------------------|
| Carregar uma vez e nunca atualizar            | Imutável ou padrão |
| Aplicativo preenche recurso repetidamente | Dinâmico              |
| Renderizar a textura                     | Padrão              |
| Acesso da CPU aos dados da GPU                | Preparando              |

 

Se você não tiver certeza de quais usos escolher, comece pelo uso do padrão, pois ele deve ser o caso mais comum. Um buffer constante de sombreador é um tipo de recurso que sempre deve ter o uso padrão.

## <a name="span-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Associação de recursos para os estágios de pipeline


Um recurso pode ser associado a mais de um estágio de pipeline ao mesmo tempo, desde que sejam atendidas as restrições especificadas quando o recurso foi criado. Essas restrições são especificadas como sinalizadores de uso, sinalizadores de associação, ou sinalizadores de acesso de cpu. Mais especificamente, um recurso pode ser vinculado como uma entrada e uma saída simultaneamente, contanto que a parte de leitura e gravação do recurso não possa ocorrer ao mesmo tempo.

Ao vincular um recurso, pense em como a GPU e a CPU terrão acesso ao recurso. Os recursos projetados para uma única finalidade (não use vários sinalizadores de uso, associação, e de acesso de cpu) terão maior chance de resultar em melhor desempenho.

Por exemplo, considere o caso de um destino de renderização usado como uma textura várias vezes. Ele pode ser mais rápido com dois recursos: um destino de renderização e uma textura usada como um recurso do sombreador. Cada recurso usaria somente um sinalizador de associação, indicando o "destino de renderização" ou "recurso de sombreador". Os dados seriam copiados da textura de destino de renderização para a textura do sombreador.

Essa técnica neste exemplo pode melhorar o desempenho, isolando a gravação de destino de renderização da leitura da textura de sombreador. A única forma de ter certeza é implementando ambas as abordagens e medindo a diferença de desempenho em seu app específico.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos](resources.md)

 

 




