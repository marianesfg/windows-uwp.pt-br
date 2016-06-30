---
author: DelfCo
Description: "Use a API Windows.Globalization.DateTimeFormatting com padrões personalizados para exibir datas e horas no formato exato que você deseja."
title: "Usar padrões para formatar datas e horas"
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 17cd1619a13adced643b4c8983dbf874bebaa740

---

# Usar padrões para formatar datas e horas





**APIs importantes**

-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)
-   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)

Use a API [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) com padrões personalizados para exibir datas e horas no formato exato que você deseja.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introdução


[
              **Windows.Globalization.DateTimeFormatting**
            ](https://msdn.microsoft.com/library/windows/apps/br206859) fornece várias maneiras de formatar corretamente datas e horas para vários idiomas e países/regiões ao redor do mundo. Você pode usar formatos padrão para ano, mês, dia e assim por diante, ou você pode usar modelos de cadeias de caracteres padrão, como "longdate" ou "month day".

Mas quando quiser mais controle sobre a ordem e o formato dos componentes da cadeia de caracteres [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) que deseja exibir, você pode usar uma sintaxe especial para o parâmetro de modelo da cadeia de caracteres, chamada"pattern". A sintaxe de padrões permite que você obtenha os componentes individuais de um objeto **DateTime**, como o nome do mês ou o ano, a fim de exibi-los em qualquer formato personalizado que você escolher. Além disso, o padrão pode ser traduzido para se adaptar a outros idiomas e países/regiões.

**Observação** Esta é uma visão geral dos padrões de formato. Para saber mais sobre modelos e padrões de formato, consulte a seção Comentários da classe [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828).

 

## <span id="What_you_need_to_know"></span><span id="what_you_need_to_know"></span><span id="WHAT_YOU_NEED_TO_KNOW"></span>O que você precisa saber


É importante notar que, ao usar padrões, você está essencialmente construindo um formato personalizado que não tem garantia de ser válido entre culturas. Por exemplo, considere o modelo "month day":

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Isso cria um formatador com base no valor do idioma ou da região do contexto atual. Portanto, ele sempre exibe o mês e o dia juntos em um formato global apropriado. Por exemplo, ele exibe "January 1" para inglês (EUA), mas "1 janvier" para francês (França) e "1月1日" para japonês. Isso se deve ao fato de o modelo ser baseado em uma cadeia de caracteres de padrão específico a uma cultura, que pode ser acessada por meio da propriedade do padrão:

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

Isso produz resultados diferentes, dependendo do idioma e da região do formatador. Note que diferentes regiões podem usar diferentes componentes, em ordens diferentes, com ou sem espaçamento de caracteres adicionais:

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Você pode usar padrões para construir um [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828) personalizado, como o que você vê abaixo, com base no padrão inglês dos EUA:

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

O Windows retorna valores específicos a cada cultura para os componentes individuais dentro de colchetes {}. Mas, com a sintaxe de padrões, a ordem dos componentes é invariável. Você obtém exatamente o que deseja, o que talvez não seja apropriado em termos de cultura:

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

Além disso, não há garantia de que os padrões permanecerão consistentes ao longo do tempo. Os países ou regiões podem alterar seus sistemas de calendário, o que modifica um modelo de formato. O Windows atualiza a saída dos formatadores para acomodar essas alterações. Portanto, você só deve usar a sintaxe de padrões para formatar [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) quando:

-   Você não depende de determinada saída para um formato.
-   Você não precisa do formato para seguir algum padrão de cultura específico.
-   Você pretende especificamente que o padrão seja invariável entre culturas.
-   Você pretende traduzir o padrão.

Para resumir as diferenças entre os modelos de cadeia de caracteres padrão e padrões de cadeia de caracteres não padrão:

**Modelos de cadeias de caracteres, como "month day":**

-   Representação abstrata de um formato de [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) que inclui valores para o mês e o dia, em alguma ordem.
-   Garante retornar um formato padrão válido em todos os valores da região de idioma com suporte do Windows.
-   Garante fornecer uma cadeia de caracteres formatada culturalmente de forma adequada  para a região de idioma determinada.
-   Nem todas as combinações de componentes são válidas. Por exemplo, não há modelo de cadeia de caracteres para "dayofweek dia".

**Padrões de cadeias de caracteres, como "{month.full} {day.integer}":**

-   Ordenou explicitamente a cadeia de caracteres que expressa o nome completo do mês, seguido por um espaço, seguido pelo número inteiro de dias, nessa ordem.
-   Pode não corresponder a um formato padrão válido para qualquer par de idioma-região.
-   Não garante ser culturalmente apropriada.
-   Qualquer combinação de componentes pode ser especificada, em qualquer ordem.

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tarefas


Suponha que você queira mostrar o mês e o dia atuais juntamente com a hora atual, em um formato específico. Por exemplo, você deseja que os usuários de inglês dos EUA vejam algo parecido com isto:

``` syntax
June 25 | 1:38 PM
```

A parte da data corresponde ao modelo "month day", e a parte da hora corresponde ao modelo "hour minute". Assim, você pode criar um formato personalizado para concatenar os padrões que formam esses modelos.

Primeiro, obtenha os formatadores para os modelos de hora e data relevantes e obtenha os padrões desses modelos:

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

Você deve armazenar seu formato personalizado como uma cadeia de recurso localizável. Por exemplo, a cadeia de caracteres para inglês (Estados Unidos) seria "{date} | {time}". Os localizadores podem ajustar essa cadeia de caracteres conforme necessário. Por exemplo, eles podem mudar a ordem dos componentes, caso pareça mais natural em algum idioma ou região que a hora preceda a data. Ou eles podem substituir "|" por algum outro caractere separador. No tempo de execução, você substituir as partes de {date} e {time} da cadeia de caracteres pelo padrão relevante:

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

Você pode agora construir um novo formatador com base no padrão personalizado:

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <span id="related_topics"></span>Tópicos relacionados


* [Exemplo de formatação de data e hora](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 






<!--HONumber=Jun16_HO4-->


