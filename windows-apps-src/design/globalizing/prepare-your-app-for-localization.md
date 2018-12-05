---
Description: A localized app is one that can be localized to other markets, languages, or regions without uncovering any functional defects in the app. The most essential property of a localizable app is that its executable code has been cleanly separated from its localizable resources.
title: Tornar seu app localizável
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: c0df06458bf70599be657fe2812b2fb3e2b44ed6
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8713258"
---
# <a name="make-your-app-localizable"></a>Tornar seu app localizável

Um app localizado é aquele que pode ser localizado em outros mercados, idiomas ou regiões sem revelar defeitos funcionais no app. A propriedade mais essencial de um aplicativo localizável é que seu código executável tenha sido separado cuidadosamente de seus recursos localizáveis. Assim, você deve determinar quais dos recursos do seu app precisam ser localizados. Pergunte a si mesmo o que deve ser modificado se o app precisar ser localizado em outros mercados.

Também recomendamos que você se familiarize com as [diretrizes de globalização](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="put-your-strings-into-resources-files-resw"></a>Coloque suas cadeias de caracteres nos Arquivos de Recursos (.resw)

Não codifique os literais de cadeia de caracteres no código imperativo, na marcação XAML, nem no manifesto do pacote do aplicativo. Em vez disso, coloque suas cadeias de caracteres em Arquivos de Recursos (.resw) para que elas se adaptem a diferentes mercados locais independentemente dos binários criados do seu app. Para mais informações, consulte [Localizar as cadeias de caracteres em seu IU e o manifesto do pacote do aplicativo](../../app-resources/localize-strings-ui-manifest.md).

Esse tópico também mostra como adicionar comentários ao seu Arquivo de Recursos (.resw) padrão. Por exemplo, se você estiver adotando uma voz ou tom informal, certifique-se de explicar isso nos comentários. Além disso, para minimizar despesas, certifique-se de que somente as cadeias de caracteres que precisam ser traduzidas sejam fornecidas aos tradutores.

Defina o idioma padrão do seu app adequadamente no arquivo de origem do manifesto do pacote do aplicativo (arquivo `Package.appxmanifest`). O idioma padrão determina o idioma que é usado quando os idiomas preferenciais do usuário não correspondem a nenhum dos idiomas aceitos pelo aplicativo. Marque todos os seus recursos com o idioma dos recursos (mesmo aqueles do seu idioma padrão, por exemplo `\Assets\en-us\Logo.png`), para que o sistema possa saber em qual idioma o recurso está e como ele é usado em situações específicas.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Personalize suas imagens e outros recursos de arquivos para idioma

O ideal é que você seja capaz de globalizar suas imagens&mdash;, ou seja, torná-las independentes de cultura. Para as imagens e outros recursos de arquivo em que isso não for possível, crie quantas variantes diferentes forem necessárias e coloque os qualificadores de idioma adequados em seus nomes de arquivos ou pastas. Para aprender mais, consulte [Personalizar os recursos de acordo com idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)).

Para minimizar os custos de localização, não coloque textos nem materiais culturalmente confidenciais em imagens. Uma imagem adequada na sua própria cultura pode ser ofensiva e mal interpretada em outras culturas. Evite o uso de imagens específicas de cultura, como caixas de correio, que não são comuns em todo o mundo. Evite símbolos religiosos, animais, políticos ou imagens específicas de gênero. A exibição de carne, partes do corpo ou gestos com as mãos também pode ser uma questão delicada. Se você não puder evitá-los, suas imagens precisarão ser localizadas com cuidado. Se você estiver localizando para um idioma com ordem de leitura diferente do seu idioma, o uso de imagens simétricas e efeitos facilitam o suporte a espelhamento.

Além disso, evite o uso de texto em imagens e fala em arquivos de áudio/vídeo.

## <a name="the-use-of-color-in-your-app"></a>O uso da cores no seu app

Preste atenção ao usar cores. Usar combinações de cores que sejam associadas a bandeiras nacionais ou movimentos políticos pode ser problemático. Opções de cores podem precisar da revisão de especialistas em cultura. Também há problemas de acessibilidade com o uso de cores. Se você usar cores para transmitir o significado, também deverá transmitir essas mesmas informações por outros meios, tais como tamanho, forma ou rótulo.

## <a name="consider-factoring-your-strings-into-sentences"></a>Considere a fatoração de suas cadeias de caracteres em sentenças

Use cadeias de caracteres dimensionadas adequadamente. As cadeias de caracteres curtas são mais fáceis de traduzir e possibilitam a reciclagem da tradução (o que economiza despesas, pois a mesma cadeia de caracteres não é enviada ao tradutor mais de uma vez). Além disso, as cadeias de caracteres extremamente longas poderão não ter suporte de ferramentas de localização.

No entanto, em tensão com essa diretriz, existe o risco de reutilizar uma cadeias de caracteres em diferentes contextos. Mesmo palavras simples, como &quot;on&quot; e &quot;off&quot;, podem ser traduzidas de forma diferente, dependendo do contexto. Em inglês, "on" e "off" podem ser usados como alternância para o modo avião, Bluetooth e dispositivos. Mas em italiano, a tradução dependerá do contexto do que está sendo ativado e desativado. Você precisaria criar um par de cadeias de caracteres para cada contexto. Você pode reutilizar cadeias de caracteres se os dois contextos forem iguais. Por exemplo, você pode reutilizar a cadeia de caracteres "Volume" tanto para volume de efeito de som quanto para volume de música porque ambos se referem à intensidade do som. Você não deve reutilizar aquela mesma cadeia de caracteres quando se referir ao volume de disco, pois o contexto e o significado são diferentes e a palavra pode ser traduzida de forma diferente.

Além disso, uma cadeia de caracteres como "text" ou "fax" podem ser usadas tanto como verbo quanto como substantivo em inglês, o que pode confundir o processo de tradução. Em vez disso, crie uma cadeia de caracteres separada para o formato de verbo e substantivo. Quando você não tiver certeza se os contextos são os mesmos, por precaução, use uma cadeia de caracteres distinta.

Resumindo, decomponha as cadeias de caracteres em partes que funcionam em todos os contextos. Haverá casos em que uma cadeia de caracteres precisará ser uma frase inteira.

Considere a seguinte cadeia de caracteres: "o {0} não foi possível sincronizar."

Diversas palavras poderiam substituir {0}, como "compromisso", "tarefa" ou "documento". Apesar desse exemplo funcionar em português, essa estrutura gramatical pode não funcionar em uma sentença correspondente, por exemplo, em alemão. Perceba que nas frases em alemão a seguir, algumas palavras na cadeia de caracteres de modelo ("Der", "Die", "Das") precisam corresponder à palavra nos parâmetros:

| Português                                    | Alemão                                           |
|:------------------------------------------ |:------------------------------------------------ |
| Não foi possível sincronizar o compromisso. | Der Termin konnte nicht synchronisiert werden.   |
| Não foi possível sincronizar a tarefa.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| Não foi possível sincronizar o documento.    | Das Dokument konnte nicht synchronisiert werden. |

Como outro exemplo, considere a frase "Lembre-me em {0} minuto (s)." Usar "minuto(s)" funciona em português, mas outros idiomas podem usar termos diferentes. Por exemplo, o idioma polonês usa "minuta", "minuty" ou "minut", dependendo do contexto.

Para resolver este problema, localize a frase inteira em vez de apenas uma palavra. Fazer isso pode parecer um trabalho redobrado e uma solução inadequada, mas essa é a melhor solução porque:

-   Uma mensagem gramaticalmente correta será exibida para todos os idiomas.
-   Seu tradutor não precisará perguntar pelo o que as cadeias de caracteres serão substituídas.
-   Você não precisará implementar uma correção de código custosa quando um problema assim aparecer depois de o seu aplicativo estar concluído.

## <a name="other-considerations-for-strings"></a>Outras considerações para cadeias de caracteres

Evite coloquialismos e metáforas em cadeias de caracteres que você criar no seu idioma padrão. A linguagem específica de um grupo demográfico, como cultura e idade, pode ser difícil de compreender ou traduzir, porque só as pessoas daquele grupo demográfico usam essa linguagem. Da mesma forma, as metáforas podem fazer sentido para uma pessoa, mas não significar nada para outra pessoa. Por exemplo, um &quot;meu&quot; é algo específico da parte de uma cultura local, mas outros que não pertençam àquela cultura podem não entender a expressão.

Não use jargão técnico, abreviações ou acrônimos. É menos provável que a linguagem técnica seja compreendida pelos públicos que não são técnicos ou pessoas de outras culturas ou regiões, além disso, é difícil de traduzir. As pessoas não usam esses tipos de palavras na comunicação cotidiana. A linguagem técnica geralmente aparece em mensagens de erro para identificar problemas de hardware e software, mas suas cadeias de caracteres devem ser técnicas *apenas se o usuário precisar desse nível de informação e você for capaz de fazer isso ou encontrar alguém que seja*.

Usar uma voz ou tom informal em suas cadeias de caracteres é uma opção válida. Você pode usar comentários no seu Arquivo de Recursos (.resw) padrão para indicar essa intenção.

## <a name="pseudo-localization"></a>Pseudolocalização

Pseudolocalize seu app para revelar quaisquer problemas de localização. A pseudolocalização é um tipo de simulação de localização, ou teste de divulgação. Você produz um conjunto de recursos que não estão realmente traduzidos; eles só parecem estar. As cadeias de caracteres são aproximadamente 40% mais longas do que no idioma padrão, por exemplo, e elas têm delimitadores para que você possa ver rapidamente se foram truncadas na interface do usuário.

## <a name="geopolitical-awareness"></a>Consciência geopolítica

Evite ofensas políticas em mapas ou quando estiver se referindo a regiões. Os mapas podem incluir regiões controversas ou limites nacionais e são uma fonte frequente de ofensas políticas. Tome cuidado para que nenhuma interface de usuário usada para selecionar uma nação se refira a ela como um &quot;país/região&quot;. Colocar um território em disputa em uma lista rotulada &quot;países&quot;&mdash;, como em um formulário de endereço, &mdash;pode ofender alguns usuários.

## <a name="language--and-region-changed-events"></a>Eventos alterados por idioma e região

Assine eventos que são acionados quando alterar as configurações de idioma e região do sistema. Faça isso para que você possa carregar novamente recursos, se apropriado. Para obter detalhes, consulte [Atualizando cadeias de caracteres em resposta aos eventos de alteração de valor de qualificador](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) e [Atualizando imagens em resposta aos eventos de alteração de valor de qualificador](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Verifique a ordem correta do parâmetro ao formatar as cadeias de caracteres

Não suponha que todos os idiomas expressam parâmetros na mesma ordem. Por exemplo, considere este formato.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

A cadeia de caracteres de formato neste exemplo funciona para inglês (Estados Unidos). Entretanto, não é adequado para alemão (Alemanha), por exemplo, onde o dia e mês são exibidos na ordem inversa. Certifique-se de que o tradutor saiba a intenção de cada um dos parâmetros para que ele possa inverter a ordem dos itens de formato na cadeia de caracteres de formato (por exemplo, "{1} {0}") conforme apropriado para o idioma de destino.

## <a name="dont-over-localize"></a>Não faça traduções em excesso

Envie somente linguagem natural aos tradutores; não linguagem de programação nem marcação. A marca `<link>` não é linguagem natural. Considere estes exemplos.

| Não traduza isto                   | Traduza isto |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;termos de uso&lt;/link&gt;   | termos de uso               |
| &lt;link&gt;política de privacidade&lt;/link&gt; | política de privacidade             |

Incluir a marca `<link>` em seu Arquivo de Recursos (.resw) significa que ela provavelmente será traduzida também. Isso deixaria a marca inválida. Se você tem cadeias de caracteres longas que precisam incluir marcação para manter o contexto e assegurar a ordem, deixe claro nos comentários o que não traduzir.

## <a name="choose-an-appropriate-translation-approach"></a>Escolher uma abordagem de tradução adequada

Depois que as cadeias de caracteres forem separadas nos arquivos de recurso, elas poderão ser traduzidas. O momento ideal para traduzir as cadeias de caracteres é depois que elas forem finalizadas, o que geralmente acontece no final de um projeto. Você pode abordar o processo de tradução de várias formas. Isso vai depender do volume de cadeias de caracteres a ser traduzido, da quantidade de idiomas a ser traduzida e do modo como a tradução será feita (por exemplo, internamente ou por contratação de fornecedor externo).

Considere essas opções.

- **Os arquivos de recursos podem ser traduzidos abrindo-os diretamente no projeto.** Essa abordagem funciona bem para um projeto com um volume pequeno de cadeias de caracteres que precisa ser traduzido para dois ou três idiomas. Ela pode ser adequada para um cenário em que um desenvolvedor fale mais de um idioma e esteja disposto a se envolver no processo de tradução. Essa abordagem é vantajosa por ser rápida, não exigir ferramentas e minimizar o risco de traduções incorretas. Mas não é escalável. Em particular, os recursos em idiomas diferentes podem facilmente ficar fora de sincronia, causando experiências ruins para o usuário e dores de cabeça para executar a manutenção.
- **Os arquivos de recursos de cadeia de caracteres estão no formato texto XML ou ResJSON, então podem ser entregues para tradução usando qualquer editor de texto. Os arquivos traduzidos, em seguida, seriam copiados de volta no projeto.** Essa abordagem tem o risco de os tradutores editarem acidentalmente as marcações XML, mas permite que o trabalho ocorra fora do projeto do Microsoft Visual Studio. Tal abordagem pode funcionar bem para projetos que precisam ser traduzidos para poucos idiomas. O formato XLIFF é um formato XML especificamente projetado para o uso em localização e tem um bom suporte de alguns fornecedores de localização ou de ferramentas de localização. Você pode usar o [Kit de Ferramentas de Aplicativo Multilíngue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) para gerar arquivos XLIFF de outros arquivos de recursos, como. resw ou .resjson.

Pode ser necessário que outros arquivos, como arquivos de imagem ou de áudio, sejam enviados para os tradutores.

Além disso, considere estas sugestões.

- **Use uma ferramenta de localização.** Há várias ferramentas de localização disponíveis para analisar arquivos de recursos e permitir que somente as cadeias de caracteres transmissíveis sejam editadas por tradutores. Essa abordagem reduz o risco de um tradutor editar acidentalmente as marcas de XML. A desvantagem é que é preciso introduzir uma nova ferramenta e um novo processo à localização. Uma ferramenta de localização é boa para projetos com grande volume de cadeias de caracteres e poucos idiomas. Para saber mais, consulte [Como usar o Kit de Ferramentas de Aplicativo Multilíngue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
- **Use um fornecedor de localização.** Considere o uso de um fornecedor de localização se o seu projeto tiver um grande volume de cadeias de caracteres que precisam ser traduzidas para muitos idiomas. Um fornecedor de localização pode recomendar ferramentas e processos, bem como traduzir seus arquivos de recurso. Essa é a solução ideal, mas também é a opção mais dispendiosa, e pode aumentar o tempo de retorno do conteúdo traduzido.

## <a name="keep-access-keys-and-labels-consistent"></a>Mantenha teclas de acesso e rótulos consistentes

É um desafio "sincronizar" as chaves de acesso usadas em acessibilidade com a exibição de chaves de acesso localizadas, porque os recursos com duas cadeias de caracteres são categorizados em duas seções separadas. Certifique-se de fornecer comentários para a cadeia de caracteres de rótulo como: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Forneça suporte a furigana para cadeias de caracteres japoneses que possam ser classificadas

Os caracteres kanji japoneses apresentam a propriedade de ter mais de uma leitura (pronúncia), dependendo da palavra em que são usados. Isso é um problema quando tentamos classificar os objetos nomeados em japonês, como nomes de aplicativos, arquivos, músicas etc. No passado, o kanji japonês era classificado em uma ordem chamada XJIS, que podia ser compreendida pelo computador. Infelizmente, por essa ordem de classificação não ser fonética, ela não é muito útil para humanos.

A *furigana* resolve esse problema permitindo que o usuário ou criador especifique a fonética dos caracteres em uso. Se você usar o seguinte procedimento para adicionar a furigana ao nome do aplicativo, poderá garantir que ele seja classificado no local adequado da lista de aplicativos. Se o nome do aplicativo contiver os caracteres kanji e a furigana não for fornecida quando o idioma da interface do usuário ou a ordem de classificação for definida em japonês, o Windows se esforçará ao máximo para gerar a pronúncia adequada. Entretanto, existe a possibilidade de os nomes de aplicativos, que contêm leituras raras ou exclusivas, serem classificados em uma leitura mais comum. Portanto, a melhor prática para aplicativos em japonês (principalmente os que contêm caracteres kanji em seus nomes) é fornecer uma versão em furigana do nome do aplicativo como parte do processo de localização para japonês.

1.  Adicione "ms-resource:Appname" como o Nome de exibição do pacote e o Nome de exibição do aplicativo.
2.  Crie uma pasta ja-JP em cadeias de caracteres e adicione dois arquivos de recursos, como a seguir:

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  Em Resources.resw para ja-JP geral: adicione um recurso de cadeia de caracteres para Appname "希蒼"
4.  Em Resources.altform-msft-phonetic.resw para recursos Furigana para japonês: adicione o valor furigana para AppName "のあ"

O usuário pode pesquisar o nome do aplicativo "希蒼" usando tanto o valor furigana "のあ" (noa), quanto o valor fonético (usando a função **GetPhonetic** do Editor de Método de Entrada (IME)) "まれあお" (mare-ao).

A classificação segue o formato do **Painel de Controle Regional**.

-   Em um local de usuário japonês,
    -   Se a furigana estiver habilitada, o "希蒼" estará classificado sob "の".
    -   Se a furigana estiver faltando, o "希蒼" estará classificado sob "ま".
-   Em um local de usuário não japonês,
    -   Se a furigana estiver habilitada, o "希蒼" estará classificado sob "の".
    -   Se a furigana estiver faltando, o "希蒼" estará classificado sob "漢字".

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes de globalização](guidelines-and-checklist-for-globalizing-your-app.md)
* [Localizar as cadeias de caracteres em seu IU e o manifesto do pacote do aplicativo](../../app-resources/localize-strings-ui-manifest.md)
* [Personalizar os recursos de acordo com idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Ajustar layout e fontes e fornecer suporte a RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [Atualizando imagens em resposta aos eventos de alteração de valor de qualificador](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Exemplos

* [Exemplo de recursos e localização de aplicativos](http://go.microsoft.com/fwlink/p/?linkid=254478)
