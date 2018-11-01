---
author: stevewhims
Description: The previous topic (How the Resource Management System matches and chooses resources) looks at qualifier-matching in general. This topic focuses on language-tag-matching in more detail.
title: Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma
template: detail.hbs
ms.author: stwhi
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: f64072f2f04b5cd45b3b75ccad908ef6906c00f5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5942196"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma

O tópico anterior ([Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe](how-rms-matches-and-chooses-resources.md)) analisa a correspondência de qualificador em geral. Este tópico aborda a correspondência de marca de idioma mais detalhadamente.

## <a name="introduction"></a>Introdução

Os recursos com qualificadores de marca de idioma são comparados e classificados com base na lista de idiomas de tempo de execução do app. Para obter definições das diferentes listas de idiomas, consulte [Noções básicas sobre idiomas de perfil de usuário e idiomas de manifesto do app](../design/globalizing/manage-language-and-region.md). A correspondência do primeiro idioma em uma lista ocorre antes da correspondência do segundo idioma em uma lista, mesmo para outras variações regionais. Por exemplo, um recurso de en-GB será escolhido em detrimento de um recurso de fr-CA se o idioma de tempo de execução do app for en-US. Somente se não houver nenhum recurso para o formato en, um recurso para fr-CA será escolhido (observe que o idioma padrão do app não pôde ser definido para nenhum formato en nesse caso).

O mecanismo de pontuação usa os dados incluídos no Registro de submarca [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302), além de outras fontes de dados. Ele permite um gradiente de pontuação com diferentes qualidades de correspondência e, quando vários candidatos estão disponíveis, seleciona aquele com a melhor pontuação de correspondência.

Dessa forma, você pode marcar o conteúdo de idioma em termos genéricos, mas pode ainda especificar determinado conteúdo quando necessário. Por exemplo, o app pode ter várias cadeias de caracteres em inglês que são comuns aos Estados Unidos, à Grã-Bretanha e a outras regiões. A marcação dessas cadeias de caracteres como "en" (inglês) economiza espaço e sobrecarga de localização. Quando é necessário fazer distinções, como em uma cadeia de caracteres com a palavra "color/colour", as versões dos Estados Unidos e da Grã-Bretanha podem ser marcadas separadamente usando as submarcas de idioma e de região, como "en-US" e "en-GB", respectivamente.

## <a name="language-tags"></a>Marcas de idioma

Os idiomas são identificados por meio de marcas de idioma BCP-47 normalizadas e válidas. Os componentes das submarcas são definidos no Registro da submarca BCP-47. A estrutura normal de uma marca de idioma BCP-47 consiste em um ou mais dos elementos de submarca a seguir.

- Submarca de idioma (obrigatória).
- Submarca de script (que pode ser inferida por meio do padrão especificado no Registro de submarca).
- Submarca de região (opcional).
- Submarca de variante (opcional).

Outros elementos de submarca poderão estar presentes, mas terão um efeito insignificante na correspondência de idioma. Não há variações de idioma definidas com o caractere curinga ("*"), por exemplo, "en-*".

## <a name="matching-two-languages"></a>Fazendo a correspondência de dois idiomas

Sempre que o Windows compara dois idiomas, geralmente isso é feito no contexto de um processo maior. Essa comparação pode ser feita no contexto da avaliação de vários idiomas, por exemplo, quando o Windows gera a lista de idiomas de aplicativo (consulte [Noções básicas sobre idiomas de perfil de usuário e idiomas de manifesto do app](../design/globalizing/manage-language-and-region.md)). O Windows faz isso comparando vários idiomas das preferências do usuário com os idiomas especificados no manifesto do app. A comparação também pode ser feita no contexto da avaliação do idioma, juntamente com outros qualificadores de um recurso específico. Um exemplo é quando o Windows resolve um recurso de arquivo específico para um determinado contexto de recurso, com a localização de residência do usuário ou a escala ou o dpi atual do dispositivo como outros fatores (além do idioma) que são levados em consideração na seleção de recursos.

Quando duas marcas de idioma são comparadas, a comparação recebe uma pontuação com base na proximidade da correspondência.

| Correspondência | Pontuação | Exemplo |
| ----- | ----- | ------- |
| Correspondência exata | Mais alta | en-AU : en-AU |
| Correspondência variante (idioma, script, região, variante) |  | en-AU-variante1 : en-AU-variante1-t-ja |
| Correspondência de região (idioma, script, região) |  | en-AU : en-AU-variante1 |
| Correspondência parcial (idioma, script) |  |  |
| - Correspondência de macrorregião |  | en-AU : en-053 |
| - Correspondência de região neutra |  | en-AU : en |
| - Correspondência de afinidade ortográfica (suporte limitado) |  | en-AU : en-GB |
| - Correspondência de região preferencial |  | en-AU : en-US |
| - Qualquer correspondência de região |  | en-AU : en-CA |
| Idioma indeterminado (qualquer correspondência de idioma) |  | en-AU : und |
| Nenhuma correspondência (incompatibilidade de script ou incompatibilidade de marca de idioma principal) | Mais baixa | en-AU : fr-FR |

### <a name="exact-match"></a>Correspondência exata

As marcas são exatamente iguais (correspondência de todos os elementos de submarca). Uma comparação pode ser promovida para esse tipo de correspondência com base em uma correspondência de região ou variante. Por exemplo, en-US corresponde a en-US.

### <a name="variant-match"></a>Correspondência de variante

As marcas correspondem às submarcas de idioma, script, região e variante, mas diferem em algum outro aspecto.

### <a name="region-match"></a>Correspondência de região

As marcas correspondem às submarcas de idioma, script e região, mas diferem em algum outro aspecto. Por exemplo, de-DE-1996 corresponde a de-DE, enquanto en-US-x-Pirate corresponde a en-US.

### <a name="partial-matches"></a>Correspondências parciais

As marcas correspondem às submarcas de idioma e script, mas diferem na região ou em alguma outra submarca. Por exemplo, en-US corresponde a en ou en-US corresponde a en-\*.

#### <a name="macro-region-match"></a>Correspondência de macrorregião

As tags correspondem às submarcas de idioma e script; ambas possuem submarcas de região, uma das quais denota uma macrorregião que abrange outra região. As submarcas de macroregião são sempre numéricas e derivadas dos códigos de área de país/região M.49 da Divisão de Estatística das Nações Unidas. Para ver detalhes sobre como englobar relacionamentos, consulte [Composição de macrorregiões geográficas (continentais), sub-regiões geográficas e agrupamentos econômicos e outros agrupamentos selecionados](http://go.microsoft.com/fwlink/p/?LinkId=247929).

**Observação** Os códigos das Nações Unidas para "agrupamentos econômicos" ou "outros agrupamentos" não têm suporte na BCP-47.
 
**Observação** Uma marca com a submarca de macrorregião "001" é considerada equivalente a uma marca de região neutra. Por exemplo, "es-001" e "es" são tratadas como sinônimos.

#### <a name="region-neutral-match"></a>Correspondência de região neutra

As marcas correspondem às submarcas de idioma e script, e apenas uma delas tem uma marca de região. Uma correspondência pai tem preferência sobre as outras correspondências parciais.

#### <a name="orthographic-affinity-match"></a>Correspondência de afinidade ortográfica

As marcas correspondem às submarcas de idioma e script, e as submarcas de região têm afinidade ortográfica. A afinidade depende dos dados mantidos no Windows que definem regiões relacionadas específicas de idioma, por exemplo, "en-IE" e "en-GB".

#### <a name="preferred-region-match"></a>Correspondência de região preferencial

As marcas correspondem às submarcas de idioma e script, e uma das submarcas de região é a submarca de região padrão do idioma. Por exemplo, "fr-FR" é a região padrão da submarca "fr". Portanto, fr-FR é uma correspondência melhor para fr-BE do que para fr-CA. Isso depende dos dados mantidos no Windows que definem uma região padrão para cada idioma em que o Windows é localizado.

#### <a name="sibling-match"></a>Correspondência irmã

As marcas correspondem às submarcas de idioma e de script, e as duas têm submarcas de região, mas nenhum outro relacionamento é definido entre elas. No caso de várias correspondências irmãs, a última irmã enumerada será a vencedora, na ausência de uma correspondência mais alta.

### <a name="undetermined-language"></a>Idioma indeterminado

Um recurso pode ser marcado como "und" para indicar que corresponde a qualquer idioma. Essa marca também pode ser usada com uma marca de script para filtrar as correspondências baseadas no script. Por exemplo, "und-Latn" corresponderá a qualquer marca de idioma que usa o script latino. Veja abaixo para obter mais detalhes.

### <a name="script-mismatch"></a>Incompatibilidade de script

Quando as marcas corresponderem apenas à marca de idioma principal, mas não ao script, o par será considerado não correspondente e será classificado abaixo do nível de uma correspondência válida.

### <a name="no-match"></a>Nenhuma correspondência

As submarcas de idioma principais incompatíveis serão classificadas abaixo do nível de uma correspondência válida. Por exemplo, zh-Hant não corresponde a zh-Hans.

## <a name="examples"></a>Exemplos

Um idioma de usuário "zh-Hans-CN" (chinês simplificado (China)) corresponde aos seguintes recursos, na ordem de prioridade indicada. Um X indica nenhuma correspondência.

![Correspondência para chinês simplificado (China)](images/language_matching_1.png)

1. Correspondência exata; 2. e 3. Correspondência de região; 4. Correspondência pai; 5. Correspondência irmã.

Quando uma submarca de idioma tem um valor de Script de Supressão definido no Registro de submarca BCP-47, a respectiva correspondência ocorre, assumindo o valor do código de script suprimido. Por exemplo, en-Latn-US corresponde a en-US. No exemplo a seguir, o idioma do usuário é "en-AU" (Inglês (Austrália)).

![Correspondência para inglês (Austrália)](images/language_matching_2.png)

1. Correspondência exata; 2. Correspondência de macrorregião; 3. Correspondência de região neutra; 4. Correspondência de afinidade ortográfica; 5. Correspondência de região preferencial; 6. Correspondência irmã.

## <a name="matching-a-language-to-a-language-list"></a>Fazendo a correspondência de um idioma a uma lista de idiomas

Às vezes, a correspondência ocorre como parte de um processo maior de correspondência de um único idioma com uma lista de idiomas. Por exemplo, pode haver uma correspondência de um recurso baseado em um único idioma com a lista de idiomas de um aplicativo. A pontuação da correspondência é ponderada pela posição do primeiro idioma correspondente na lista. Quanto mais baixa a posição do idioma na lista, mas baixa é a pontuação.

Quando a lista de idiomas contém duas ou mais variantes regionais com as mesmas submarcas de idioma e script, as comparações da primeira marca de idioma são classificadas apenas para correspondências exatas, variantes e de região. A pontuação de correspondências parciais é adiada para a última variante regional. Dessa forma, os usuários podem controlar perfeitamente o comportamento da correspondência em sua lista de idiomas. O comportamento da correspondência pode incluir a precedência da correspondência exata de um item secundário na lista sobre uma correspondência parcial do primeiro item na lista, se houver um terceiro item que corresponda ao idioma e ao script do primeiro. Veja um exemplo a seguir.

- Lista de idiomas (em ordem): "pt-PT" (Português (Portugal)), "en-US" (Inglês (Estados Unidos)), "pt-BR" (Português (Brasil)).
- Recursos: "en-US", "pt-BR".
- Recurso com pontuação mais alta: "en-US".
- Descrição: a comparação inicia com "pt-PT", mas não encontra uma correspondência exata. Devido à presença de "pt-BR" na lista de idiomas do usuário, a correspondência parcial é adiada para a comparação com "pt-BR". A próxima comparação de idioma é "en-US", que tem uma correspondência exata. Portanto, o recurso vencedor é "en-US".

OU

- Lista de idiomas (em ordem): "es-MX" (Espanhol (México)), "es-HO" (Espanhol (Honduras)).
- Recursos: "en-ES", "es-HO".
- Recurso com a pontuação mais alta: "es-HO".

## <a name="undetermined-language-und"></a>Idioma indeterminado ("und")

A marca de idioma "und" pode ser usada para especificar um recurso que corresponda a qualquer idioma na ausência de uma correspondência melhor. Ela pode ser considerada similar ao intervalo de idiomas BCP-47 "*" ou "*-&lt;script&gt;". Veja um exemplo a seguir.

- Lista de idiomas: "en-US", "zh-Hans-CN".
- Recursos: "zh-Hans-CN", "und".
- Recurso com a pontuação mais alta: "und".
- Descrição: a comparação inicia com "en-US", mas não encontra uma correspondência baseada em "en" (parcial ou melhor). Como há um recurso marcado com "und", o algoritmo correspondente usará esse.

A marca "und" permite que vários idiomas compartilhem um único recurso e permite que idiomas individuais sejam tratados como exceções. Por exemplo:

- Lista de idiomas: "zh-Hans-CN", "en-US".
- Recursos: "zh-Hans-CN", "und".
- Recurso com a pontuação mais alta: "zh-Hans-CN".
- Descrição: a comparação localiza uma correspondência exata do primeiro item e, portanto, não verifica se há recurso rotulado como "und".

Você pode usar "und" com uma marca de script para filtrar recursos por script. Por exemplo:

- Lista de idiomas: "ru".
- Recursos: "und-Latn", "und-Cyrl", "und-Arab".
- Recurso com a pontuação mais alta: "und-Cyrl".
- Descrição: a comparação não localiza uma correspondência para "ru" (parcial ou melhor) e, portanto, corresponde à marca de idioma "und". O valor de script de supressão "Cyrl" associado à marca de idioma "ru" corresponde ao recurso "und-Cyrl".

## <a name="orthographic-regional-affinity"></a>Afinidade regional ortográfica

Quando duas marcas de idioma com diferenças de submarca de região são correspondentes, pares específicos de regiões podem ter afinidade mais alta um com outro do que outros. Os únicos grupos relacionados compatíveis são para inglês ("en"). As submarcas de região "PH" (Filipinas) e "LR" (Libéria) têm afinidade ortográfica com a submarca de região "US". Todas as outras submarcas de região são relacionadas à submarca de região "GB" (Reino Unido). Portanto, quando os recursos "en-US" e "en-GB" estiverem disponíveis, uma lista de idiomas de "en-HK" (Inglês – RAE de Hong Kong) obterá uma pontuação mais alta com recursos "en-GB" do que com recursos "en-US".

## <a name="handling-languages-with-many-regional-variants"></a>Tratando idiomas com diversas variantes regionais

Determinados idiomas têm comunidades extensas de falantes em regiões distintas que usam diferentes variedades desse idioma, como inglês, francês e espanhol, que estão entre os idiomas com maior suporte em aplicativos multilíngues. As diferenças regionais podem incluir variações de ortografia (por exemplo, "color" versus "colour") ou diferenças de dialeto, como vocabulário (por exemplo, "truck" versus "lorry").

Esses idiomas com variantes regionais significativas apresentam alguns desafios à criação de um app para uso internacional: "Quantas variantes regionais diferentes devem ser compatíveis?" "Quais?" "Qual é a maneira mais econômica de gerenciar esses ativos de variante regional para meu app?" Está além do escopo deste tópico responder a todas essas perguntas. No entanto, os mecanismos de correspondência de idioma no Windows fornecem recursos que podem ajudar você a tratar variantes regionais.

Os apps geralmente darão suporte a uma única variante de um determinado idioma. Suponha que um app tenha recursos para apenas uma variante do inglês, planejados para uso por falantes do inglês independentemente da região. Nesse caso, a marca "en" sem submarca de região refletirá essa expectativa. Porém, os apps podem ter usado historicamente uma marca como "en-US", que inclui uma submarca de região. Nesse caso, isso também funcionará: o app usa apenas uma variante do inglês, e o Windows lida com a correspondência de um recurso marcado por uma variante regional com uma preferência de idioma de usuário para uma variante regional diferente de maneira adequada.

No entanto, se houver suporte para duas ou mais variantes regionais, uma diferença como "en" versus "en-US" poderá ter um impacto significativo sobre a experiência do usuário, e passará a ser importante considerar quais submarcas de região serão usadas.

Suponhamos que você queira fornecer localizações separadas para o francês usado no Canadá e para o francês europeu. Para o francês canadense, é possível usar "fr-CA". Para os falantes na Europa, a localização usará o Francês (França) e, portanto, será possível usar "fr-FR" para isso. Mas, e se um determinado usuário for belga, com a preferência de idioma "fr-BE"? O que ele receberá? A região "BE" é diferente de "FR" e "CA", sugerindo uma correspondência de "qualquer região" para ambos. No entanto, França é a região preferencial para o francês e, portanto, "fr-FR" será considerado a melhor correspondência nesse caso.

Suponhamos que você tenha localizado primeiramente o app para uma única variante do francês, usando cadeias de caracteres do tipo Francês (França), mas qualificando-as genericamente como "fr" e, em seguida, você queira adicionar suporte para o francês canadense. Provavelmente, apenas alguns recursos precisarão ser traduzidos novamente para o francês canadense. Você pode continuar usando todos os ativos originais, mantendo-os qualificados como "fr", e adicionar apenas o pequeno conjunto de novos ativos usando "fr-CA". Se a preferência de idioma do usuário for "fr-CA", o ativo "fr-CA" terá uma pontuação de correspondência maior do que o ativo "fr". Porém, se a preferência de idioma do usuário for qualquer outra variante do francês, o ativo de região neutra "fr" será uma correspondência melhor que o ativo "fr-CA".

Suponhamos também que você queira fornecer localizações em espanhol separadas para os falantes da Espanha e da América Latina. Suponha ainda que as traduções para a América Latina tenham sido feitas por um fornecedor do México. Você deve usar "es-ES" (Espanha) e "es-MX" (México) para os dois conjuntos de recursos? Se você fizesse isso, poderia criar problemas para os falantes de outras regiões da América Latina, como Argentina ou Colômbia, pois eles receberiam os recursos "es-ES". Nesse caso, há uma alternativa melhor: você pode usar uma submarca de macrorregião, "es-419", para indicar que os ativos sejam usados por falantes de qualquer parte da América Latina ou do Caribe.

Marcas de idioma de região neutra e submarcas de macroregião podem ser muito eficazes quando você deseja dar suporte a várias variantes regionais. Para minimizar o número de ativos diferentes necessários, você pode qualificar um determinado ativo de uma maneira que reflita a cobertura mais ampla possível à qual ele é aplicável. Em seguida, complemente um ativo amplamente aplicável com uma variante mais específica, conforme necessário. Um ativo com um qualificador de idioma de região neutra será usado para usuários de qualquer variante regional, a menos que exista outro ativo com um qualificador regional mais específico aplicável a esse usuário. Por exemplo, um ativo "en" corresponderá a um usuário falante do inglês australiano, mas um ativo com "en-053" (inglês usado na Austrália ou na Nova Zelândia) será uma correspondência melhor para esse usuário, enquanto um ativo com "en-AU" será a melhor correspondência possível.

O idioma inglês merece consideração especial. Se um app adicionar localização para duas variantes do inglês, estas serão provavelmente para o inglês dos EUA e do Reino Unido, ou inglês "internacional". Como observado acima, certas regiões fora dos EUA seguem as convenções ortográficas dos Estados Unidos, e a correspondência de idioma do Windows leva isso em consideração. Neste cenário, não é recomendável usar a marca de região neutra "en" para uma das variantes. Em vez disso, use "en-GB" e "en-US". (No entanto, se um recurso não exigir variantes separadas, "en" poderá ser usado.) Se "en-GB" ou "en-US" for substituído por "en", isso causará interferência com a afinidade regional ortográfica fornecida pelo Windows. Se uma terceira localização em inglês for adicionada, use uma submarca específica ou de macrorregião para as variantes adicionais, conforme necessário (por exemplo, "en-CA", "en-AU" ou "en-053"), mas continue usando "en-GB" e "en-US".

## <a name="related-topics"></a>Tópicos relacionados

* [Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe](how-rms-matches-and-chooses-resources.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Noções básicas sobre idiomas de perfil de usuário e idiomas de manifesto do app](../design/globalizing/manage-language-and-region.md)
* [Composição de macrorregiões geográficas (continentais), sub-regiões geográficas, e agrupamentos econômicos e outros agrupamentos selecionados](http://go.microsoft.com/fwlink/p/?LinkId=247929)
