---
author: DelfCo
Description: "Prepare o seu aplicativo para localização para outros mercados, idiomas ou regiões."
title: "Preparar um app para localização"
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
label: Prepare your app for localization
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 923f6ac6ec656c668eef662cb254b2b28d445548
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="prepare-your-app-for-localization"></a>Preparar um app para localização


<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Prepare o seu aplicativo para localização para outros mercados, idiomas ou regiões. Antes de começar, certifique-se de ler [o que fazer e o que não fazer](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="use-resource-files-and-qualifiers"></a>Use qualificadores e arquivos de recurso.


Certifique-se de especificar as cadeias de caracteres de interface do usuário do seu aplicativo nos arquivos de recurso, em vez de colocá-las no seu código. Para obter mais detalhes, consulte [Colocar cadeias de caracteres da interface do usuário em recursos](put-ui-strings-into-resources.md).

Especifique imagens ou outros recursos de arquivo com a marca do idioma adequado em seu arquivo ou pasta. Saiba que localizar imagens, áudio e vídeo consome uma grande quantidade de recursos do sistema, portanto é melhor usar ativos de mídia neutros sempre que possível. Para saber mais, consulte [Como nomear recursos usando qualificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

## <a name="add-contextual-comments"></a>Adicione comentários contextuais.


Adicione comentários de localização aos arquivos de recurso do aplicativo. Os comentários ficam visíveis para o tradutor e devem fornecer informações contextuais que ajudem o tradutor a traduzir os recursos com precisão. Os comentários também devem fornecer informações de restrições suficientes sobre o recurso, para que a tradução não corrompa o software. Opcionalmente, os comentários podem ser registrados pela ferramenta Makepri.exe.

**XAML:** arquivos Resw (recursos criados no Visual Studio para aplicativos usando XAML) têm um elemento de comentário. Por exemplo:

```XML
<data name="String1">
    <value>Hello World</value>
    <comment>A greeting (This is a comment to the localizer)</comment>
</data>
```

## <a name="localize-sentences-instead-of-words"></a>Localize frases em vez de palavras.


Considere a seguinte cadeia de caracteres: "Não foi possível sincronizar o {0}."

Diversas de palavras poderiam substituir {0}, como compromisso, tarefa ou documento. Apesar desse exemplo funcionar em português, essa estrutura gramatical pode não funcionar em uma sentença correspondente em alemão. Perceba que nas frases em alemão a seguir, algumas palavras na cadeia de caracteres de modelo ("Der", "Die", "Das") precisam corresponder à palavra nos parâmetros:

| Português                                    | Alemão                                           |
|:------------------------------------------ |:------------------------------------------------ |
| Não foi possível sincronizar o compromisso. | Der Termin konnte nicht synchronisiert werden.   |
| Não foi possível sincronizar a tarefa.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| Não foi possível sincronizar o documento.    | Das Dokument konnte nicht synchronisiert werden. |

 

Como outro exemplo, considere a frase "Lembre-me em {0} minuto(s)." Embora usar "minuto(s)" funcione em português, outros idiomas podem usar termos diferentes. Por exemplo, o idioma polonês usa "minuta", "minuty" ou "minut", dependendo do contexto.

Para resolver este problema, localize a frase inteira em vez de apenas uma palavra. Fazer isso pode parecer um trabalho redobrado e uma solução inadequada, mas essa é a melhor solução porque:

-   Uma mensagem sem erros será exibida para todos os idiomas.
-   Seu tradutor não precisará perguntar pelo o que as cadeias de caracteres serão substituídas.
-   Você não precisará implementar uma correção de código custosa quando um problema assim aparecer depois de o seu aplicativo estar concluído.

## <a name="ensure-the-correct-parameter-order"></a>Assegure a ordem correta do parâmetros.


Não suponha que todos os idiomas usam parâmetros na mesma ordem. Por exemplo, considere a cadeia de caracteres "Every %s %s", em que o primeiro %s seja substituído pelo nome de um mês e o segundo %s seja substituído pela data de um mês. Esse exemplo funciona em inglês, mas falhará quando o aplicativo for localizado para o português, em que a data e o mês são exibidos na ordem contrária.

Para resolver esse problema, mude a cadeia de caracteres para "Every %1 %2", para que a ordem seja intercambiável de acordo com o idioma.

## <a name="dont-over-localize"></a>Não faça traduções em excesso.


Traduza cadeias de caracteres específicas, não marcas. Considere os exemplos a seguir:

| Cadeia de caracteres desnecessariamente localizada                   | Cadeia de caracteres corretamente localizada |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;termos de uso&lt;/link&gt;   | termos de uso               |
| &lt;link&gt;política de privacidade&lt;/link&gt; | política de privacidade             |

 

A inclusão da marca acima &lt;link&gt; nos recursos significa que ela também será localizada. Isso renderiza a marcação como inválida. Só as cadeias de caracteres devem ser traduzidas. De um modo geral, você deve considerar as marcas como um código, que devem ser mantidas separadas do conteúdo localizável. Entretanto, algumas cadeias de caracteres longas devem incluir marcação para manter o contexto e a assegurar a ordem.

## <a name="do-not-use-the-same-strings-in-dissimilar-contexts"></a>Não use as mesmas cadeias de caracteres em contextos diferentes,


Reutilizar uma cadeia de caracteres pode parecer a melhor solução, mas isso poderá causar problemas de localização se a mesma palavra ou frase tiver diferentes significados ou contextos.

Você pode reutilizar cadeias de caracteres se os dois contextos forem iguais. Por exemplo, você pode reutilizar a cadeia de caracteres "Volume" tanto para volume de efeito de som quanto para volume de música porque ambos se referem à intensidade do som. Você não deve reutilizar aquela mesma cadeia de caracteres quando se referir ao volume de disco, pois o contexto e o significado são diferentes e a palavra pode ser traduzida de forma diferente.

Outro exemplo é o uso das cadeias de caracteres "on" e "off". Em inglês, "on" e "off" podem ser usados como alternância para o modo avião, Bluetooth e dispositivos. Mas em italiano, a tradução dependerá do contexto do que está sendo ativado e desativado. Você precisaria criar um par de cadeias de caracteres para cada contexto.

Além disso, uma cadeia de caracteres como "text" ou "faz" podem ser usadas tanto como verbo quanto como substantivo em inglês, o que pode confundir o processo de tradução. Em vez disso, crie uma cadeia de caracteres separada para o formato de verbo e substantivo. Quando você não tiver certeza se os contextos são os mesmos, por precaução, use uma cadeia de caracteres distinta.

## <a name="identify-resources-with-unique-attributes"></a>Identifique recursos com atributos exclusivos.


Os identificadores de recurso não diferenciam maiúsculas e minúsculas e devem ser exclusivos por arquivo de recurso. Ao acessar um recurso, use o identificador de recursos, não o valor real do recurso. Os identificadores de recurso não mudam, mas os valores reais dos recursos mudam dependendo do idioma.

Use identificadores de recurso significativos para fornecer contexto adicional para a tradução.

Não mude os identificadores de recurso depois que os recursos da cadeia de caracteres forem enviados para a tradução. Equipes de localização usam o identificador de recursos para rastrear adições, exclusões e atualizações nos recursos. Alterações em identificadores de recursos—também conhecidas como "deslocamento de identificadores de recursos"—exigem que cadeias de caracteres sejam traduzidas novamente, pois parecerá que cadeias de caracteres foram deletadas e outras foram adicionadas.

## <a name="choose-an-appropriate-translation-approach"></a>Escolha uma abordagem de tradução adequada.


Depois que as cadeias de caracteres forem separadas nos arquivos de recurso, elas poderão ser traduzidas. O momento ideal para traduzir as cadeias de caracteres é depois que elas forem finalizadas, o que geralmente acontece no final de um projeto. Você pode abordar o processo de tradução de várias formas. Isso vai depender do volume de cadeias de caracteres a ser traduzido, da quantidade de idiomas a ser traduzida e do modo como a tradução será feita (por exemplo, internamente ou por contratação de fornecedor externo).

Considere as opções a seguir:

-   **Os arquivos de recursos podem ser traduzidos abrindo-os diretamente no projeto.** Essa abordagem funciona bem para um projeto com um volume pequeno de cadeias de caracteres que precisa ser traduzido para dois ou três idiomas. Ela pode ser adequada para um cenário em que um desenvolvedor fale mais de um idioma e esteja disposto a se envolver no processo de tradução. Essa abordagem é vantajosa por ser rápida, não exigir ferramentas e minimizar o risco de traduções incorretas, mas ela não é escalável. Em particular, os recursos em idiomas diferentes podem facilmente ficar fora de sincronia, causando experiências ruins para o usuário e dores de cabeça para executar a manutenção.
-   **Os arquivos de recursos de cadeia de caracteres estão no formato texto XML ou ResJSON, então podem ser entregues para tradução usando qualquer editor de texto. Os arquivos traduzidos, em seguida, seriam copiados de volta no projeto.** Essa abordagem tem o risco de os tradutores editarem acidentalmente as marcações XML, mas permite que o trabalho ocorra fora do projeto do Microsoft Visual Studio. Tal abordagem pode funcionar bem para projetos que precisam ser traduzidos para poucos idiomas. O formato XLIFF é um formato XML especificamente projetado para o uso em localização e tem um bom suporte de alguns fornecedores de localização ou de ferramentas de localização. Você pode usar o [Kit de Ferramentas de Aplicativo Multilíngue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) para gerar arquivos XLIFF de outros arquivos de recursos, como. resw ou .resjson.

Pode ser necessário que outros arquivos, como arquivos de imagem ou de áudio, sejam enviados para os tradutores. Normalmente, não recomendamos a criação de arquivos culturalmente dependentes, pois isso dificulta a tradução desses arquivos.

Além disso, considere as seguintes sugestões:

-   **Use uma ferramenta de localização.** Há várias ferramentas de localização disponíveis para analisar arquivos de recursos e permitir que somente as cadeias de caracteres transmissíveis sejam editadas por tradutores. Essa abordagem reduz o risco de um tradutor editar acidentalmente as marcas de XML. A desvantagem é que é preciso introduzir uma nova ferramenta e um novo processo à localização. Uma ferramenta de localização é boa para projetos com grande volume de cadeias de caracteres e poucos idiomas. Para saber mais, consulte [Como usar o Kit de Ferramentas de Aplicativo Multilíngue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
-   **Use um fornecedor de localização.** Considere o uso de um fornecedor de localização se o seu projeto tiver um grande volume de cadeias de caracteres que precisam ser traduzidas para muitos idiomas. Um fornecedor de localização pode recomendar ferramentas e processos, bem como traduzir seus arquivos de recurso. Essa é a solução ideal, mas também é a opção mais dispendiosa, e pode aumentar o tempo de retorno do conteúdo traduzido.
-   **Mantenha os tradutores informados.** Informe os tradutores sobre cadeias de caracteres que podem ser consideradas um substantivo ou um verbo. Explique as palavras fabricadas para seus tradutores usando as ferramentas de terminologia. Mantenha as cadeias de caracteres gramaticalmente corretas, sem ambiguidade e o menos técnicas possível para evitar confusão.

## <a name="keep-access-keys-and-labels-consistent"></a>Mantenha teclas de acesso e rótulos consistentes.


É um desafio "sincronizar" as chaves de acesso usadas em acessibilidade com a exibição de chaves de acesso localizadas, porque os recursos com duas cadeias de caracteres são categorizados em duas seções separadas. Certifique-se de fornecer comentários para a cadeia de caracteres de rótulo como: `Make sure that the emphasized shortcut key  is synchronized with the access key.`


## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Forneça suporte para Furigana para cadeias de caracteres japoneses que possam ser classificadas.


Os caracteres Kanji japoneses apresentam a propriedade exclusiva de ter mais de uma pronúncia, dependendo da palavra e/ou do contexto em que são usados. Isso é um problema quanto tentamos classificar os objetos nomeados em japonês, como nomes de aplicativos, arquivos, músicas etc. No passado, o Kanji japonês era classificado em uma ordem chamada XJIS, que podia ser compreendida pelo computador. Infelizmente, por essa ordem de classificação não ser fonética, ela não é muito útil para humanos.

O Furigana resolve esse problema permitindo que o usuário ou criador especifique a fonética dos caracteres em uso. Se você usar o seguinte procedimento para adicionar Furigana ao nome do aplicativo, poderá garantir que ele seja classificado no local adequado da lista de aplicativos. Se o nome do aplicativo contiver os caracteres Kanji e o Furigana não for fornecido quando o idioma da interface do usuário ou a ordem de classificação for definida em japonês, o Windows se esforçará ao máximo para gerar a pronúncia adequada. Entretanto, existe a possibilidade de os nomes de aplicativos, que contêm leituras raras ou exclusivas, serem classificados em uma leitura mais comum. Portanto, a melhor prática para aplicativos em japonês (principalmente os que contêm caracteres Kanji em seus nomes) é fornecer uma versão em Furigana do nome do aplicativo como parte do processo de localização para japonês.

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
4.  Em Resources.altform-msft-phonetic.resw para recursos Furigana para japonês: adicione o valor Furigana para AppName "のあ"

O usuário pode pesquisar o nome do aplicativo "希蒼" usando tanto o valor Furigana "のあ" (noa), quanto o valor fonético (usando a função **GetPhonetic** do Editor de Método de Entrada (IME)) "まれあお" (mare-ao).

A classificação segue o formato do **Painel de Controle Regional**.

-   Em local de usuário japonês,
    -   Se Furigana estiver habilitado, o "希蒼" estará classificado sob "の".
    -   Se Furigana estiver faltando, o "希蒼" estará classificado sob "ま".
-   Em local de usuário não japonês,
    -   Se Furigana estiver habilitado, o "希蒼" estará classificado sob "の".
    -   Se Furigana estiver faltando, o "希蒼" estará classificado sob "漢字".

## <a name="related-topics"></a>Tópicos relacionados


* [O que fazer e o que não fazer em globalização e localização](guidelines-and-checklist-for-globalizing-your-app.md)
* [Colocar cadeias de caracteres da interface do usuário em recursos](put-ui-strings-into-resources.md)
* [Como nomear recursos usando qualificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
 

 



