---
author: stevewhims
Description: Use the Windows.Globalization.DateTimeFormatting API with custom templates and patterns to display dates and times in exactly the format you wish.
title: Usar padrões para formatar datas e horas
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 04a0288d0b28c12eb68cf56225747224e8df9777
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5710375"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Usar modelos e padrões para formatar datas e horas

Usar classes no namespace [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) com modelos e padrões personalizados para exibir datas e horas no formato exato que você deseja.

## <a name="introduction"></a>Introdução

A classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) fornece várias maneiras de formatar corretamente datas e horas para vários idiomas e regiões no mundo todo. Você pode usar formatos padrão para ano, mês, dia e assim por diante. Ou você pode passar um modelo de formato para o argumento *formatTemplate* do construtor **DateTimeFormatter**, como "longdate" ou "month day".

No entanto, quando quiser ainda mais controle sobre a ordem e o formato dos componentes do objeto [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) que deseja exibir, você pode passar um padrão de formato para o argumento *formatTemplate* do construtor. Um padrão de formato usa uma sintaxe especial que permite a você obter componentes individuais de um objeto **DateTime** &mdash;como o nome do mês ou o ano,&mdash; a fim de exibi-los em qualquer formato personalizado que você escolher. Além disso, o padrão pode ser traduzido para se adaptar a outros idiomas e países/regiões.

**Observação**isso é apenas uma visão geral dos padrões de formato. Para saber mais sobre modelos e padrões de formato, consulte a seção Comentários da classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live).

## <a name="the-difference-between-format-templates-and-format-patterns"></a>A diferença entre modelos de formato e padrões de formato

Um modelo de formato é uma cadeia de caracteres de formato independente de cultura. Portanto, se você criar um **DateTimeFormatter** usando um modelo de formato, em seguida, o formatador exibe seus componentes de formato na ordem certa para o idioma corrente. Por outro lado, um padrão de formato é específico a cada cultura. Se você criar um **DateTimeFormatter** usando um padrão de formato, o formatador usará o padrão exatamente como indicado. Consequentemente, um padrão não é necessariamente válido entre culturas.

Vamos ilustrar essa distinção com um exemplo. Vamos passar um modelo de formato simples (não um padrão) para o construtor **DateTimeFormatter**. Trata-se do modelo de formato "month day".

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Isso cria um formatador com base no valor do idioma ou da região do contexto atual. A ordem dos componentes em um modelo de formato não importa; o formatador exibe-os na ordem certa para o idioma corrente. Assim, ele exibe "January 1" para inglês (Estados Unidos), mas "1 janvier" para francês (França), e "1月1日" para japonês.

Por outro lado, um padrão de formato é específico a cada cultura. Vamos acessar o padrão de formato para o nosso modelo de formato.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Isso produz resultados diferentes dependendo do idioma e da região de tempo de execução. Diferentes regiões podem usar diferentes componentes, em ordens diferentes, com ou sem espaçamento de caracteres adicionais.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

No exemplo acima, nós inseridos uma cadeia de caracteres de formato independente de cultura e obtivemos uma cadeia de caracteres de formato específico a cada cultura (que era uma função do idioma e região que, por acaso, estava em vigor quando chamamos `dateFormatter.Patterns`). Verifica-se, portanto, que se você construir um **DateTimeFormatter** de um padrão de formato específico a cada cultura, em seguida, ele só será válido para regiões/idiomas específicos.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

O formatador acima retorna valores específicos de cultura para os componentes individuais dentro dos colchetes {}. No entanto, a ordem dos componentes no padrão de formato é invariável. Você obtém exatamente o que deseja, o que pode ou não ser apropriado em termos de cultura. Este formatador é válido para inglês (Estados Unidos), mas não para francês (França), nem para japonês.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

Além disso, um padrão que está correto hoje talvez não esteja correto futuramente. Os países ou regiões podem alterar seus sistemas de calendário, o que modifica um modelo de formato. O Windows atualiza a saída dos formatadores com base nos modelos de formato para acomodar essas alterações. Portanto, você só deve usar a sintaxe de padrões em uma ou mais destas condições.

-   Você não depende de determinada saída para um formato.
-   Você não precisa do formato para seguir algum padrão de cultura específico.
-   Você pretende especificamente que o padrão seja invariável entre culturas.
-   Você pretende localizar a própria e real cadeia de caracteres de formato padrão.

Veja a seguir um resumo da distinção entre modelos de formato e padrões de formato.

**Modelos de formato, como "month day"**

-   Representação abstrata de um formato de [DateTime](/uwp/api/windows.foundation.datetime?branch=live) que inclui valores para o mês, dia, etc., em qualquer ordem.
-   Garante retornar um formato padrão válido em todos os valores da região de idioma com suporte do Windows.
-   Garante fornecer uma cadeia de caracteres formatada culturalmente de forma adequada para a região de idioma determinada.
-   Nem todas as combinações de componentes são válidas. Por exemplo, "dayofweek day" não é válido.

**Padrões de formato, como "{month.full} {day.integer}"**

-   Cadeia de caracteres ordenada explicitamente que expressa o nome completo do mês, seguido por um espaço, seguido pelo número inteiro de dias, nessa ordem, ou qualquer padrão de formato específico que você estipular.
-   Pode não corresponder a um formato padrão válido para qualquer par de idioma-região.
-   Não garante ser culturalmente apropriada.
-   Qualquer combinação de componentes pode ser especificada, em qualquer ordem.

## <a name="examples"></a>Exemplos

Suponha que você queira mostrar o mês e o dia atuais juntamente com a hora atual, em um formato específico. Por exemplo, você deseja que os usuários de inglês dos EUA vejam algo parecido com isto:

``` syntax
June 25 | 1:38 PM
```

A parte da data corresponde ao modelo de formato "month day", e a parte da hora corresponde ao modelo de formato "hour minute". Assim, você pode construir formatadores para a modelos de formato de data e hora relevantes e, em seguida, concatenar a saída deles juntos usando uma cadeia de caracteres de formato localizável.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` é um identificador de recurso que faz referência a um recurso localizável em um Arquivo de Recursos (.resw). Para um idioma padrão do inglês (Estados Unidos), isso seria definido como um valor de "{0} | {1}"juntamente com um comentário indicando que"{0}"é a data e"{1}"é o tempo. Dessa forma, os tradutores podem ajustar os itens de formato conforme necessário. Por exemplo, eles podem mudar a ordem dos itens, caso pareça mais natural em algum idioma ou região que a hora preceda a data. Ou eles podem substituir "|" por algum outro caractere separador.

Outra maneira de implementar esse exemplo é consultar os dois formatadores com relação aos padrões de formato, concatenar juntos e, em seguida, construir um terceiro formatador a partir do padrão de formato resultante.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>APIs Importantes

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo de formatação de data e hora](http://go.microsoft.com/fwlink/p/?LinkId=231618)