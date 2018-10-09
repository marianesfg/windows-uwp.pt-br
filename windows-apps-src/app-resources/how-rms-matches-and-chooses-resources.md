---
author: stevewhims
Description: When a resource is requested, there may be several candidates that match the current resource context to some degree. The Resource Management System will analyze all of the candidates and determine the best candidate to return. This topic describes that process in detail and gives examples.
title: Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: bb1168401aaa715f8d1c459691dfa1b1ca38ccbe
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: Auto
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690422"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe
Quando um recurso é solicitado, pode haver vários candidatos que correspondam em um certo grau ao contexto de recurso atual. O Sistema de Gerenciamento de Recursos irá analisar todos os candidatos e determinar o melhor deles para ser retornado. Para isso, todos os qualificadores são levados em consideração para classificar todos os candidatos.

Nesse processo de classificação, os diferentes qualificadores recebem prioridades distintas: o idioma tem o maior impacto sobre a classificação geral, seguido por contraste, depois escala e assim por diante. Para cada qualificador, qualificadores candidatos são comparados com o valor do qualificador de contexto para determinar uma qualidade de correspondência. A forma como a comparação é feita depende do qualificador.

Para obter detalhes específicos sobre como a correspondência de marca de idioma é feita, consulte [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](how-rms-matches-lang-tags.md).

Para alguns qualificadores, como escala e contraste, há sempre um grau mínimo de correspondência. Por exemplo, um candidato qualificado como "scale-100" corresponde em certo grau a um contexto "scale-400", embora não tanto quanto um candidato qualificado como "scale-200" ou, para uma correspondência perfeita, como "scale-400".

No entanto, para outros qualificadores, como idioma ou região de residência, é possível ter uma comparação de não correspondência (bem como graus de correspondência). Por exemplo, um candidato qualificado para idioma como "en-US" é uma correspondência parcial de um contexto "en-GB", mas um candidato qualificado como "fr" não é uma correspondência. Da mesma forma, um candidato qualificado para região de residência como "155" (Europa Ocidental) corresponde relativamente bem ao contexto de um usuário cuja configuração de região de residência seja "FR". Porém, um candidato qualificado como "US" não oferece nenhuma correspondência.

Quando um candidato é avaliado, se houver uma comparação de não correspondência para qualquer qualificador, esse candidato receberá uma classificação geral de não correspondência e não será selecionado. Dessa forma, os qualificadores de maior prioridade podem ter a maior ponderação na seleção da melhor correspondência, mas até mesmo um qualificador de baixa prioridade pode eliminar um candidato devido a uma não correspondência.

Um candidato é neutro em relação a um qualificador quando ele não está marcado para esse qualificador. Para qualquer qualificador, um candidato neutro é sempre uma correspondência para o valor do qualificador de contexto, mas com uma qualidade de correspondência inferior à de qualquer candidato que tenha sido marcado para esse qualificador e tenha um certo grau de correspondência (exata ou parcial). Por exemplo, se tivermos candidatos qualificados como "en-US", "en" e "fr", e um candidato de idioma neutro, em um contexto com o valor de qualificador de idioma "en-GB", os candidatos serão classificados na seguinte ordem: "en", "en-US", neutro e "fr". Nesse caso, "fr" não oferece correspondência, mas os outros candidatos oferecem um certo grau de correspondência.

O processo global de classificação começa avaliando candidatos em relação ao qualificador de maior prioridade, que é o idioma. Os que não corresponderem serão eliminados. Os demais candidatos são classificados em relação à sua qualidade de correspondência de idioma. Se houver empate, o qualificador de maior prioridade seguinte, contraste, será levado em consideração, usando a qualidade de correspondência para contraste com o objetivo de diferenciar entre os candidatos empatados. Após o contraste, o qualificador de escala é usado para diferenciar os demais empates, e assim por diante em quantos qualificadores forem necessários para chegar a uma classificação bem ordenada.

Se todos os candidatos são eliminados da consideração porque os qualificadores não correspondem ao contexto, o carregador de recursos faz uma segunda passada procurando um candidato padrão para exibir. Os candidatos padrão são determinados durante a criação do arquivo PRI e devem garantir que sempre haverá um candidato a ser selecionado em qualquer contexto de tempo de execução. Se um candidato tiver algum qualificador não correspondente e não padrão, esse candidato a recurso será descartado definitivamente.

Para todos os candidatos a recurso ainda em consideração, o carregador de recursos examina o valor do qualificador de contexto de prioridade mais alta e escolhe aquele que corresponde melhor ou com a melhor pontuação padrão. Qualquer correspondência real é considerada melhor que uma pontuação padrão.

Se houver um vínculo, o valor do qualificador de contexto com a próxima prioridade mais alta será inspecionado e o processo continua, até que uma correspondência melhor seja encontrada.

## <a name="example-of-choosing-a-resource-candidate"></a>Exemplo de escolha de um candidato a recurso
Considere estes arquivos.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

Suponha que estas sejam as configurações no contexto atual.

```
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

O Sistema de Gerenciamento de Recursos elimina três dos arquivos, pois o alto contraste e o idioma alemão não correspondem ao contexto definido pelas configurações. Isso descarta esses candidatos.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

Para os candidatos restantes, o Sistema de Gerenciamento de Recursos usa o qualificador de contexto de prioridade mais alta, que é idioma. Os recursos em inglês são uma correspondência mais próxima do que os recursos em francês, pois o inglês está listado antes do francês nas configurações.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

Em seguida, o Sistema de Gerenciamento de Recursos usa o qualificador de contexto com a próxima prioridade mais alta, que é escala. Portanto, este será o recurso retornado.

```
en/images/logo.scale-400.jpg
```

Você pode usar o método avançado [**NamedResource.ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) para recuperar todos os candidatos na ordem em que eles correspondem às configurações de contexto. No exemplo que acabamos de analisar, **ResolveAll** retorna candidatos nesta ordem.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>Exemplo de produção de uma opção de fallback
Considere estes arquivos.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Suponha que estas sejam as configurações no contexto atual.

```
User language: de-DE;
Scale: 400
Contrast: High
```

Todos os arquivos são eliminados porque não correspondem ao contexto. Então, entramos em uma passagem padrão, na qual o padrão (consulte [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md)) durante a criação do arquivo PRI era esse.

```
Language: fr-FR;
Scale: 400
Contrast: Standard
```

Isso descarta todos os recursos que correspondem ao usuário atual ou ao padrão.

```
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

O Sistema de Gerenciamento de Recursos usa o qualificador de contexto de prioridade mais alta, idioma, para retornar o recurso nomeado com a pontuação mais alta.

```
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>APIs importantes
* [NamedResource.ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>Tópicos relacionados
* [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md)