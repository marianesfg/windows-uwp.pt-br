---
author: DelfCo
Description: "Desenvolva um aplicativo preparado para ser usado globalmente. Formate adequadamente datas, horas, números, números de telefone e moedas."
title: Usar formatos prontos para o mundo
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 84e53b5093d2d288dbe95f51b0a3f9a8e5e06fe0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="use-global-ready-formats"></a>Usar formatos prontos para o mundo

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Desenvolva um aplicativo preparado para ser usado globalmente. Formate adequadamente datas, horas, números, números de telefone e moedas. Isso permitirá adaptar o aplicativo posteriormente a outras culturas, países/regiões e idiomas do mercado global.

<div class="important-apis" >
<b>APIs Importantes</b><br/>
<ul>
<li>[**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)</li>
<li>[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)</li>
<li>[**Windows.Globalization.PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting)</li>
</ul>
</div>


## <a name="introduction"></a>Introdução

Ao criar seus aplicativos, muitos desenvolvedores pensam só no próprio idioma e na própria cultura. Mas quando o aplicativo começa a crescer em outros mercados, sua adaptação para novos idiomas e regiões pode apresentar dificuldades de formas inesperadas. Datas, horas, números, calendários, moedas, números de telefone, unidades de medida e tamanhos de papel são todos itens que podem ser exibidos de maneira diferente dependendo da cultura.

Para simplificar o processo de adaptação a novos mercados, basta considerar alguns detalhes ao desenvolver o aplicativo.

## <a name="format-dates-and-times-appropriately"></a>Formate datas e horários de forma adequada

Há muitas maneiras diferentes de mostrar corretamente as datas e horários. Diferentes regiões e culturas usam diferentes convenções para a ordem de dias e meses em datas, para a separação de horas e minutos em horários e até mesmo para a pontuação usada como um separador. Além disso, as datas podem ser exibidas em vários formatos longos ("Quarta-feira, 28 de março de 2012") ou curtos ("28/03/2012"), o que pode variar entre as culturas. E, claro, os nomes e as abreviações de dias da semana e meses do ano diferem em cada idioma.

Se você precisa permitir que os usuários escolham uma data ou selecionem uma hora, use os controles padrão [Se você precisa permitir que os usuários escolham uma data ou selecionem uma hora, use os controles padrão](https://msdn.microsoft.com/library/windows/apps/hh465466). Eles usaram automaticamente os formatos de data e hora para o idioma e região preferenciais do usuário.

Se você precisar exibir datas ou horas, use os formatadores [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) e [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) para exibir automaticamente o formato preferido do usuário para datas, horários e números. O código a seguir formata uma Data/Hora usando o idioma e a região de preferência. Por exemplo, se a data atual for 3 de junho de 2012, o formatador produzirá "6/3/2012" se o usuário preferir inglês (Estados Unidos), mas usará "03.06.2012" se ele preferir alemão (Alemanha):

```CSharp
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

## <a name="format-numbers-and-currencies-appropriately"></a>Formate datas e moedas de forma adequada

Diferentes culturas formatam números de maneiras distintas. Diferenças de formato podem incluir o número de dígitos decimais a serem exibidos, os caracteres a serem usados como separadores decimais e o símbolo de moeda a ser usado. Use [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) para exibir números decimais, percentuais, em milhar e moedas. Na maioria dos casos, você simplesmente exibe números ou moedas de acordo com as preferências do usuário atual. Mas você também pode usar os formatadores para exibir uma moeda para uma determinada região ou formato

O código a seguir é um exemplo sobre como exibir moedas de acordo com o idioma e a região de preferência do usuário ou para um sistema monetário específico:

```CSharp
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

## <a name="use-a-culturally-appropriate-calendar"></a>Use um calendário culturalmente apropriado

O calendário também difere conforme a região e o idioma. O calendário gregoriano não é o padrão de toda região. Usuários de algumas regiões podem escolher calendários alternativos, como o calendário de eras japonês ou os calendários lunares árabes. Datas e horas no calendário também dependem dos vários fusos horários e do horário de verão.

Use os controles padrão do [selecionador de data e hora](https://msdn.microsoft.com/library/windows/apps/hh465466) para permitir que os usuários escolham uma data, garantindo assim o uso do formato de calendário preferido. Para cenários mais complexos, onde pode ser necessário trabalhar diretamente com operações de datas do calendário, o Windows.Globalization oferece uma classe [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) que fornece uma representação de calendário apropriada à cultura, região e ao tipo de calendário em questão.

## <a name="format-phone-numbers-appropriately"></a>Formate números de telefone apropriadamente
Os números de telefone são formatados de maneira diferente em regiões. O número de dígitos, como os dígitos são agrupados e a significância de determinadas partes do número de telefone variam de um país para o próximo. Desde o Windows 10, versão 1607, você pode usar [**PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting) para formatar números de telefone apropriadamente para a região atual.

[**PhoneNumberInfo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberinfo.aspx) analisa uma cadeia de dígitos e permite determinar se os dígitos são um número de telefone válido na região atual, comparar dois números em busca de igualdade e extrair as parte funcionais diferentes do número de telefone, como código do país ou código da área geográfica.

[**PhoneNumberFormatter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberformatter.aspx) formata uma cadeia de dígitos ou um PhoneNumberInfo para exibição, mesmo quando a cadeia de dígitos representa um número de telefone parcial. (Você pode usar essa formatação de número parcial para formatar um número à medida que um usuário insere o número.) 

O código abaixo mostra como usar PhoneNumberFormatter para formatar um número de telefone à medida que é inserido. Sempre que o texto muda em um TextBox chamado gradualInput, o conteúdo da caixa de texto é formatado usando-se a região padrão atual e exibido em um TextBlock chamado outBox. Para fins de demonstração, a cadeia de caracteres também é formatada usando-se a região da Nova Zelândia e exibida em um TextBlock chamado NZOutBox.
    
```csharp
    using Windows.Globalization;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        // Note that you must check the results of TryCreate before you use the formatter.
        PhoneNumberFormatter.TryCreate("NZ", out NZFormatter);

    }

    private void gradualInput_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region into outBox.
        outBox.Text = currentFormatter.FormatPartialString(gradualInput.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZOutBox.
        if(NZFormatter != null)
        {
            NZOutBox.Text = NZFormatter.FormatPartialString(gradualInput.Text);
        }
    }
```    

## <a name="respect-the-users-language-and-cultural-preferences"></a>Respeite as preferências de idioma e cultura do usuário

Para cenários em que você fornecer uma funcionalidade diferente com base no idioma, região ou preferências culturais do usuário, o Windows oferece uma forma de acessar essas preferências através de [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825). Quando necessário, use a classe **GlobalizationPreferences** para obter o valor atual do país/região geográfica do usuário, idiomas preferenciais, moedas preferenciais, etc.

## <a name="related-topics"></a>Tópicos relacionados

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
