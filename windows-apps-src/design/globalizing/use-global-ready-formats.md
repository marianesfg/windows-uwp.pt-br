---
author: stevewhims
Description: Design your app to be global-ready by appropriately formatting dates, times, numbers, phone numbers, and currencies. You'll then be able later to adapt your app for additional cultures, regions, and languages in the global market.
title: Globalize seus formatos data/hora/número.
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 173198c2c61530704dad02e2e92e6a7e47aae420
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047837"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globalize seus formatos data/hora/número.

Projete seu app de modo a ser usado globalmente. Formate adequadamente datas, horas, números, números de telefone e moedas. Você poderá, posteriormente, adaptar seu app a outras culturas, regiões e idiomas do mercado global.

## <a name="introduction"></a>Introdução

Ao criar seu app, se você pensar mais de forma mais ampla do que em um único idioma e cultura, você terá pequenos problemas inesperados (se houver) quando seu app se desenvolver em novos mercados. Datas, horas, números, calendários, moedas, números de telefone, unidades de medida e tamanhos de papel são todos itens que podem ser exibidos de maneira diferente dependendo da cultura.

Diferentes regiões e culturas usam diferente formatos de data e hora. Isso inclui convenções para a ordem de dias e meses em datas, para a separação de horas e minutos em horários e até mesmo para a pontuação usada como um separador. Além disso, as datas podem ser exibidas em vários formatos longos ("Quarta-feira, 28 de março de 2012") ou curtos ("28/03/2012"), o que varia entre as culturas. E, claro, os nomes e as abreviações de dias da semana e meses do ano diferem entre idiomas.

Você pode visualizar os formatos usados para diferentes idiomas. Vá para **Configurações** > **Hora e idioma** > **Região e idioma**, e clique em **Configurações adicionais de data, hora e regionais** > **Alterar formatos de data, hora ou número**. Na guia **Formatos**, selecione um idioma da lista suspensa **Formato** e visualize os formatos em **Exemplos**.

Este tópico usa os termos "lista de idiomas do perfil do usuário", "lista de idiomas de manifesto do app" e "lista de idiomas de tempo de execução do aplicativo". Para obter detalhes sobre o que significam exatamente esses termos e como acessar seus valores, consulte [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Formatar datas e horas para a lista de idiomas de tempo de execução do app

Se você precisar permitir que os usuários escolham uma data ou selecionem uma hora, use os [controles padrão de calendário, data e hora](../controls-and-patterns/date-and-time.md). Eles usam automaticamente o melhor formato de data e hora para a lista de idiomas de tempo de execução do app.

Se precisar exibir datas ou horas por conta própria, você pode usar a classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live). Por padrão, **DateTimeFormatter** usa automaticamente o melhor formato de data e hora para a lista de idiomas de tempo de execução do app. Então, o código a seguir formata um determinado **DateTime** para essa lista da melhor maneira. Por exemplo, supondo que sua lista de idiomas de manifesto do app inclua inglês (Estados Unidos), que também é o idioma padrão, e alemão (Alemanha). Se a data corrente for 6 de novembro de 2017 e a lista de idiomas do perfil de usuário contenha alemão (Alemanha) primeiro, o formatador produzirá "06.11.2017". Se a lista de idiomas do perfil do usuário contiver primeiro inglês (Estados Unidos) (ou se ela não contiver inglês nem alemão), o formatador produzirá "11/6/2017" (já que "en-US" corresponde ou é usado como padrão).

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

Você pode testar o código acima em seu próprio computador desta forma.

- Certifique-se de que você tenha arquivos de recursos em seu projeto, qualificados para "en-US" e "de-DE" (confira [Personalizar os recursos de acordo com idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Altere sua lista de idiomas do perfil do usuário em **Configurações** > **Hora e idioma** > **Região e idioma** > **Idiomas**. Adicione alemão (Alemanha), torne-o padrão e execute o código novamente.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Formatar datas e horas para a lista de idiomas do perfil do usuário

Lembre-se de que, por padrão, **DateTimeFormatter** corresponde à lista de idiomas de tempo de execução do app. Dessa forma, se você exibir cadeias de caracteres, como "A data é &lt;data&gt;", o idioma corresponderá ao formato de data.

Se por algum motivo você desejar formatar datas e/ou horários apenas de acordo com a lista de idiomas do perfil do usuário, você pode fazer isso usando o código, como no exemplo a seguir. Mas se fizer isso, entenda que o usuário pode escolher um idioma para o qual seu app não tem cadeias de caracteres traduzidas. Por exemplo, se seu app não está localizado em alemão (Alemanha), mas o usuário escolher esse idioma como preferencial, isso pode resultar na exibição possivelmente estranha de cadeias de caracteres, como "A data é 06.11.2017".

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Formate datas e moedas de forma adequada

Diferentes culturas formatam números de maneiras distintas. Diferenças de formato podem incluir o número de dígitos decimais a serem exibidos, os caracteres a serem usados como separadores decimais e o símbolo de moeda a ser usado. Use classes no namespace [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) para exibir números decimais, percentuais, em milhar e moedas. Em geral, será conveniente que essas classes do formatador usem o melhor formato para o perfil do usuário. No entanto, você pode usar os formatadores para exibir uma moeda para qualquer região ou formato

Este exemplo mostra como exibir moedas de acordo com o perfil do usuário e para um determinado sistema monetário.

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

Você pode testar o código acima em seu próprio computador, alterando o país ou região em **Configurações** > **Hora e idioma** > **Região e idioma** > **País ou região**. Escolha um país ou região (Islândia, por exemplo) e execute o código novamente.

## <a name="use-a-culturally-appropriate-calendar"></a>Use um calendário culturalmente apropriado

O calendário também difere conforme a região e o idioma. O calendário gregoriano não é o padrão de toda região. Usuários de algumas regiões podem escolher calendários alternativos, como o calendário de eras japonês ou os calendários lunares árabes. Datas e horas no calendário também dependem dos vários fusos horários e do horário de verão.

Para garantir o uso do formato de calendário preferido, você pode usar os [controles padrão de calendário, data, e hora](../controls-and-patterns/date-and-time.md). Para cenários mais complexos, onde pode ser necessário trabalhar diretamente com operações de datas do calendário, o **Windows.Globalization** oferece uma classe [**Calendário**](/uwp/api/windows.globalization.calendar?branch=live) que fornece uma representação de calendário apropriada à cultura, região e ao tipo de calendário em questão.

## <a name="format-phone-numbers-appropriately"></a>Formate números de telefone apropriadamente

Os números de telefone são formatados de maneira diferente em regiões. O número de dígitos, como os dígitos são agrupados e a significância de determinadas partes do número de telefone variam entre países. Desde o Windows 10, versão 1607, você pode usar classes no namespace [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) para formatar números de telefone apropriadamente para a região corrente.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) analisa uma cadeia de dígitos e permite determinar se os dígitos são um número de telefone válido na região atual, comparar dois números em busca de igualdade e extrair as parte funcionais diferentes do número de telefone, como código do país ou código da área geográfica.

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) formata uma cadeia de dígitos ou um **PhoneNumberInfo** para exibição, mesmo quando a cadeia de dígitos representa um número de telefone parcial. Você pode usar essa formatação de número parcial para formatar um número à medida que um usuário insere o número.

O exemplo abaixo mostra como usar **PhoneNumberFormatter** para formatar um número de telefone à medida que é inserido. Sempre que o texto muda em um **TextBox** chamado phoneNumberInputTextBox, o conteúdo da caixa de texto é formatado usando-se a região padrão atual e exibido em um **TextBlock** chamado phoneNumberOutputTextBlock. Para fins de demonstração, a cadeia de caracteres também é formatada usando-se a região da Nova Zelândia e exibida em um TextBlock chamado phoneNumberOutputTextBlockNZ.
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

Você pode testar o código acima em seu próprio computador, alterando o país ou região em **Configurações** > **Hora e idioma** > **Região e idioma** > **País ou região**. Escolha um país ou região (Nova Zelândia, por exemplo, para confirmar se os formatos correspondem) e executar o código novamente. Para dados de teste, você pode fazer uma pesquisa na Web pelo número de telefone de uma empresa na Nova Zelândia.

## <a name="the-users-language-and-cultural-preferences"></a>As preferências de idioma e cultura do usuário

Para cenários em que você deseja fornecer uma funcionalidade diferente, que baseie-se exclusivamente no idioma, região ou preferências culturais do usuário, o Windows oferece uma forma de acessar essas preferências através de [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live). Quando necessário, use a classe **GlobalizationPreferences** para obter o valor atual do país/região geográfica do usuário, idiomas preferenciais, moedas preferenciais, etc. Entretanto, lembre-se de que se as cadeias de caracteres/imagens do seu app não são localizadas para o idioma de preferência do usuário, as datas e horas e outros dados formatados para esse idioma de preferência não corresponderão às cadeias de caracteres que você exibir.

## <a name="important-apis"></a>APIs Importantes

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Calendário](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Tópicos relacionados

* [Controles de calendário, data e hora](../controls-and-patterns/date-and-time.md)
* [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md)
* [Personalizar os recursos de acordo com idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Exemplos

* [Detalhes do calendário e exemplo de matemática](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Exemplo de formatação de data e hora](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Exemplo de preferências de globalização](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Exemplo de formatação de número e análise](http://go.microsoft.com/fwlink/p/?linkid=231620)
