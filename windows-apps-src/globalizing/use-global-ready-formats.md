---
author: DelfCo
Description: "Desenvolva um aplicativo preparado para uso global formatando adequadamente datas, horas, números e moedas."
title: Usar formatos prontos para o mundo
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 77b5e7bd412936dd5d8c4bc252771631d6b884cf

---

# <span id="dev_globalizing.use_global-ready_formats"></span>Usar formatos prontos para o mundo





**APIs importantes**

-   [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)

Desenvolva um aplicativo preparado para uso global formatando adequadamente datas, horas, números e moedas. Isso permitirá adaptá-lo posteriormente a outras culturas, países/regiões e idiomas do mercado global.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introdução


Ao criar seus aplicativos, muitos desenvolvedores pensam só no próprio idioma e na própria cultura. Mas quando o aplicativo começa a crescer em outros mercados, sua adaptação para novos idiomas e regiões pode apresentar dificuldades de formas inesperadas. Datas, horas, números, calendários, moedas, números de telefone, unidades de medida e tamanhos de papel são todos itens que podem ser exibidos de maneira diferente dependendo da cultura.

Para simplificar o processo de adaptação a novos mercados, basta considerar alguns detalhes ao desenvolver o aplicativo.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Pré-requisitos


[Plano para um mercado global](https://msdn.microsoft.com/library/windows/apps/hh465405)
## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tarefas


1.  **Formate datas e horários de forma adequada.**

    Há muitas maneiras diferentes de mostrar corretamente as datas e horários. Diferentes regiões e culturas usam diferentes convenções para a ordem de dias e meses em datas, para a separação de horas e minutos em horários e até mesmo para a pontuação usada como um separador. Além disso, as datas podem ser exibidas em vários formatos longos ("Quarta-feira, 28 de março de 2012") ou curtos ("28/03/2012"), o que pode variar entre as culturas. E, claro, os nomes e as abreviações de dias da semana e meses do ano diferem em cada idioma.

    Se você precisa permitir que os usuários escolham uma data ou selecionem uma hora, use os controles padrão [Se você precisa permitir que os usuários escolham uma data ou selecionem uma hora, use os controles padrão](https://msdn.microsoft.com/library/windows/apps/hh465466). Eles usaram automaticamente os formatos de data e hora para o idioma e região preferenciais do usuário.

    Se você precisar exibir datas ou horas, use os formatadores [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) e [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) para exibir automaticamente o formato preferido do usuário para datas, horários e números. O código a seguir formata uma Data/Hora usando o idioma e a região de preferência. Por exemplo, se a data atual for 3 de junho de 2012, o formatador produzirá "6/3/2012" se o usuário preferir inglês (Estados Unidos), mas usará "03.06.2012" se ele preferir alemão (Alemanha):

    **C#**
    ```    CSharp
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = DateTime.Now;

    // Perform the actual formatting.
    var sdate = sdatefmt.Format(dateToFormat);
    var stime = stimefmt.Format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```
    **JavaScript**
    ```    JavaScript
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = new Date();

    // Perform the actual formatting.
    var sdate = sdatefmt.format(dateToFormat);
    var stime = stimefmt.format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```

2.  **Formate datas e moedas de forma adequada.**

    Diferentes culturas formatam números de maneiras distintas. Diferenças de formato podem incluir o número de dígitos decimais a serem exibidos, os caracteres a serem usados como separadores decimais e o símbolo de moeda a ser usado. Use [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) para exibir números decimais, percentuais, em milhar e moedas. Na maioria dos casos, você simplesmente exibe números ou moedas de acordo com as preferências do usuário atual. Mas você também pode usar os formatadores para exibir uma moeda para uma determinada região ou formato

    O código a seguir é um exemplo sobre como exibir moedas de acordo com o idioma e a região de preferência do usuário ou para um sistema monetário específico:

    **C#**
    ```    CSharp
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyEuroFR = currencyFormatEuroFR.Format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```
    **JavaScript**
    ```    JavaScript
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.currencies;

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", ["fr-FR"], "FR");
    var currencyEuroFR = currencyFormatEuroFR.format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```

3.  **Use um calendário culturalmente apropriado.**

    O calendário também difere conforme a região e o idioma. O calendário gregoriano não é o padrão de toda região. Usuários de algumas regiões podem escolher calendários alternativos, como o calendário de eras japonês ou os calendários lunares árabes. Datas e horas no calendário também dependem dos vários fusos horários e do horário de verão.

    Use os controles padrão do [selecionador de data e hora](https://msdn.microsoft.com/library/windows/apps/hh465466) para permitir que os usuários escolham uma data, garantindo assim o uso do formato de calendário preferido. Para cenários mais complexos, onde pode ser necessário trabalhar diretamente com operações de datas do calendário, o Windows.Globalization oferece uma classe [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) que fornece uma representação de calendário apropriada à cultura, região e ao tipo de calendário em questão.

4.  **Respeite as preferências de idioma e cultura do usuário.**

    Para cenários em que você fornecer uma funcionalidade diferente com base no idioma, região ou preferências culturais do usuário, o Windows oferece uma forma de acessar essas preferências através de [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825). Quando necessário, use a classe **GlobalizationPreferences** para obter o valor atual do país/região geográfica do usuário, idiomas preferenciais, moedas preferenciais, etc.

## <span id="related_topics"></span>Tópicos relacionados


* [Plano para um mercado global](https://msdn.microsoft.com/library/windows/apps/hh465405)
* [Diretrizes para os controles de data e hora](https://msdn.microsoft.com/library/windows/apps/hh465466)

**Referência**
* [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
* [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)

**Exemplos**
* [Detalhes do calendário e exemplo de matemática](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Exemplo de formatação de data e hora](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Exemplo de preferências de globalização](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Exemplo de formatação de número e análise](http://go.microsoft.com/fwlink/p/?linkid=231620)
 

 






<!--HONumber=Jun16_HO4-->


