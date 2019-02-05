---
Description: Design and develop your app in such a way that it functions appropriately on systems with different language and culture configurations.
Search.Refinement.TopicID: 180
title: Diretrizes de globalização
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: bdc7e5de3be941f2622c04d515e5e1211247b9a2
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9047238"
---
# <a name="guidelines-for-globalization"></a>Diretrizes de globalização

Projete e desenvolva seu app de forma que ele funciona corretamente em sistemas com diferentes configurações de idioma e cultura. Use APIs [**Globalização**](/uwp/api/Windows.Globalization?branch=live) para formatar dados; e evite suposições no seu código sobre idioma, região, classificação de caractere, sistema de escrita, formatação de data/hora, números, moedas, espessuras e regras de classificação.

| Recomendação | Descrição |
| ------------- | ----------- |
| Leve a cultura em consideração ao manipular e comparar cadeias de caracteres. | Por exemplo, não altere a ocorrência de cadeias de caracteres antes de compará-las. Consulte [Recomendações para o uso de cadeias de caracteres](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Ao agrupar (ordenar) cadeias de caracteres e outros dados, não pressuponha que eles estejam sempre em ordem alfabética. | Para idiomas que não são latinos, o agrupamento é baseado em fatores como pronúncia ou número de traços de caneta. Mesmo os idiomas que são latinos, nem sempre usam a classificação em ordem alfabética. Por exemplo, em algumas culturas, um catálogo telefônico talvez não seja classificado em ordem alfabética. O Windows pode cuidar da classificação, mas se você criar seu próprio algoritmo de classificação, assegure-se de levar em conta os métodos de classificação usados em seus mercados de destino. |
| Formate adequadamente números, datas, horas, endereços e números de telefone. | Esses formatos variam entre culturas, regiões, idiomas e mercados. Se você estiver exibindo esses datas, use APIs [**Globalization**](/uwp/api/Windows.Globalization?branch=live) para obter o formato apropriado para uma audiência específica. Consulte [Globalize seus formatos data/hora/número](use-global-ready-formats.md). A ordem na qual os nomes e sobrenomes são exibidos, bem como o formato dos endereços, também pode variar. Use exibições padrão de data, hora e número. Use controles de seletor de data e hora padrão. Use informações de endereço padrão. |
| Ofereça suporte às unidades de medidas e moedas internacionais. | Diferentes unidades e escalas são usadas em diferentes países, embora as mais populares sejam o sistema métrico e o sistema imperial. Ofereça suporte à medição de sistema correta se você lidar com medidas como extensão, temperatura e área. Use a propriedade [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) para obter o conjunto de moedas em uso em uma região. |
| Use Unicode para a codificação de caracteres. | Por padrão, o Microsoft Visual Studio usa a codificação de caracteres Unicode para todos os documentos. Se você estiver usando um editor diferente, assegure-se de salvar os arquivos de origem nas codificações de caracteres do Unicode adequadas. Todas as APIs UWP retornarão cadeias de caracteres codificadas UTF-16. |
| Permita tamanhos internacionais de papel. | Os tamanhos mais comuns de papel diferem entre os países, portanto se você incluir recursos que dependam do tamanho do papel, como impressão, assegure-se de permitir e testar tamanhos internacionais comuns. |
| Registre o idioma do teclado ou IME. | Quando seu app pedir entrada de texto ao usuário, registre a marca do idioma para o layout do teclado ou IME habilitado no momento. Isso garante que quando a entrada for exibida posteriormente ela será apresentada ao usuário com a formatação adequada. Use a propriedade [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) para obter o idioma de entrada atual. |
| Não use o idioma para pressupor a região de um usuário, nem a região para pressupor o idioma do usuário. | Idioma e região são conceitos distintos. Um usuário pode falar uma variação regional específica de um idioma, como en-GB para inglês como falado no Reino Unido, mas o usuário pode estar em um país ou região completamente diferente. Considere se o app requer conhecimento sobre o idioma do usuário (para texto da interface do usuário, por exemplo) ou região (para licenciamento, por exemplo). Para obter mais informações, consulte [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md). |
| As regras para comparar marcas de idioma são não triviais. | As [marcas de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302) são complexas. Há uma série de problemas para comparar marcas de idioma, incluindo problemas para corresponder informações de script, marcas herdadas e diversas variações regionais. O Sistema de Gerenciamento de Recursos no Windows cuida da correspondência para você. Você pode especificar um conjunto de recursos em quaisquer idiomas, e o sistema escolhe o idioma adequado para o usuário e para o aplicativo. Consulte [Os recursos de app e o Sistemas de Gerenciamento de Recursos](../../app-resources/index.md) e [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](../../app-resources/how-rms-matches-lang-tags.md). |
| Crie sua interface do usuário de modo a acomodar tamanhos de texto e tamanhos de fonte diferentes para rótulos e controles de entrada de texto. | As cadeias de caracteres traduzidas para idiomas diferentes podem variar muito de comprimento, portanto, será necessário que os controles de interface do usuário sejam dimensionados dinamicamente ao conteúdo. Caracteres comuns em outros idiomas incluem marcas acima ou abaixo do que é usado normalmente em inglês (como Å ou Ņ). Use tamanhos de fonte e alturas de linha padrão para fornecer um espaço vertical adequado. Lembre-se de que as fontes de outros idiomas podem exigir tamanhos de fonte mínimos maiores para permanecerem legíveis. Veja as classes no namespace [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live). |
| Suporte o espelhamento da ordem de leitura. | O alinhamento do texto e a ordem de leitura pode ser da esquerda para a direita (como em português, por exemplo), ou da direita para a esquerda (RTL) (como em árabe ou hebraico, por exemplo). Se você estiver localizando seu produto em vários idiomas que usem uma ordem de leitura diferente do seu idioma, assegure-se de que o layout dos elementos da interface do usuário permitam espelhamento. Até itens como botões Voltar, efeitos de transição da interface do usuário e imagens podem ser espelhados. Para obter mais informações, consulte [Ajustar layout e fontes e fornecer suporte a RTL](adjust-layout-and-fonts--and-support-rtl.md). |
| Exiba o texto e as fontes corretamente. | A fonte ideal, o tamanho da fonte e a direção do texto variam entre diferentes mercados. Para obter mais informações, consulte [**Ajustar layout e fontes e fornecer suporte a RTL**](adjust-layout-and-fonts--and-support-rtl.md) e [Fontes internacionais](loc-international-fonts.md). |

## <a name="important-apis"></a>APIs Importantes
 
* [Globalização](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Tópicos relacionados

* [Recomendações para o uso de cadeias de caracteres](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globalize seus formatos data/hora/número.](use-global-ready-formats.md)
* [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md)
* [Marcas de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [Os recursos de app e o Sistemas de Gerenciamento de Recursos](../../app-resources/index.md)
* [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](../../app-resources/how-rms-matches-lang-tags.md)
* [Ajustar layout e fontes e fornecer suporte a RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [Fontes internacionais](loc-international-fonts.md)
* [Tornar seu app localizável](prepare-your-app-for-localization.md)

## <a name="samples"></a>Exemplos

* [Exemplo de preferências de globalização](https://go.microsoft.com/fwlink/p/?linkid=231608)
