---
title: Prepare seu aplicativo para a mudança de era japonesa
description: Saiba mais sobre a mudança de era japonesa em maio de 2019 e como preparar seu aplicativo.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 4/26/2019
ms.topic: article
keywords: windows 10, uwp, localizabilidade, localização, japonesa, era
ms.localizationpriority: high
ms.openlocfilehash: 54d66d0426e5f0c41d48b93ba96781786d6fab92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363774"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Prepare seu aplicativo para a mudança de era japonesa

> [!NOTE]
> Em 1 de abril de 2019, o nome da nova era foi anunciado: Reiwa (令和). Em 25 de abril, a Microsoft lançou pacotes para diferentes sistemas de operacionais do Windows que contém a chave do registro atualizado com o nome da nova era. Atualizar seu dispositivo e verificar seu registro para verificar se ele tem a nova chave e, em seguida, teste seu aplicativo. Verifique [este artigo de suporte](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) para garantir que seu sistema operacional deve ter recebido a chave do registro atualizado.

O calendário japonês é dividido em eras, e para a maioria da era da computação moderna, já estamos na era Heisei; No entanto, no dia 1 de maio de 2019, uma nova era começará. Como esta é a primeira mudança de era em décadas, os softwares compatíveis com o calendário japonês precisarão ser testados para se garantir que eles funcionarão corretamente quando a nova era começar.

Nas seções a seguir, você aprenderá o que pode fazer para preparar e testar seu aplicativo para a nova era.

> [!NOTE]
> É recomendável que você use uma máquina de teste para isso, pois as alterações feitas afetarão o comportamento de todo o computador.

## <a name="add-a-registry-key-for-the-new-era"></a>Adicione uma chave de registro para a nova era

> [!NOTE]
> As instruções a seguir destinam-se para dispositivos que ainda não foram atualizados com a nova chave do registro. Primeiro, verifique se o seu dispositivo contém a nova chave do registro e se não estiver, teste usando as instruções a seguir.

É importante testar problemas de compatibilidade antes da era foi alterado, e você pode fazer isso agora usando o nome da nova era. Para isso, adicione uma chave de registro para a nova era usando o **Editor de Registro**:

1. Navegue até **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Selecione **Editar > Novo > Valor da sequência** e nomeie como **2019 05 01**.
3. Clique com o botão direito na chave e selecione **Modificar**.
4. No **dados do valor** , insira**令和_令_Reiwa_R** (você pode copiar e colar aqui para tornar mais fácil).

Consulte [Gerenciamento de eras para o calendário japonês](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar) para ler mais sobre o formato dessas chaves de registro.

Em 1 de abril de 2019, o nome da nova era foi anunciado. Em 25 de abril, uma atualização com uma nova chave de registro para versões com suporte do Windows que contém o nome foi lançada, permitindo que você valide que seu aplicativo lida com isso adequadamente. Essa atualização está sendo propagada para 7 e versões anteriores do Windows 10, bem como o Windows 8 com suporte.

Você pode excluir a chave de registro de espaço reservado depois que terminar de testar seu aplicativo. Isso garantirá que ele não interfira com a nova chave de registro que será adicionada quando o Windows for atualizado.

## <a name="change-your-devices-calendar-format"></a>Alterar o formato do calendário de seu dispositivo

Depois de adicionar a chave de registro para a nova era, você precisará configurar seu dispositivo para usar o calendário japonês. Você não precisa ter um dispositivo no idioma japonês para fazer isso. Para testes extensivos, é interessante instalar o pacote de idioma japonês também, mas isso não é necessário para os testes básicos.

Para configurar seu dispositivo para usar o calendário japonês:

1. Acesse **intl.cpl** (busque isso na barra de pesquisa do Windows).
2. No menu suspenso **Formato**, selecione **Japonês (Japão)** .
3. Selecione **Configurações adicionais**.
4. Selecione a guia **Data**.
5. No menu suspenso **Tipo de calendário**, selecione **和暦** (*wareki*, o calendário japonês). Ele deve aparecer como a segunda opção.
6. Clique em **OK**.
7. Clique em **OK** na janela **Região**.

Agora, seu dispositivo deve estar configurado para usar o calendário japonês e ele refletirá a era que estiver no registro. Abaixo está um exemplo do que você poderá ver agora no canto inferior direito da tela:

![Data e hora no formato do calendário japonês](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Ajuste o relógio de seu dispositivo

Para testar se o seu aplicativo funciona com a nova era, é necessário avançar o relógio do computador para 1º de maio de 2019 ou depois. As seguintes instruções são para Windows 10, mas o Windows 8 e 7 devem funcionar de forma semelhante:

1. Clique com o botão direito na data e hora no canto inferior direito da tela.
2. Selecione **Ajustar data/hora**.
3. No app Configurações, em **Alterar data e hora**, selecione **Alterar**.
4. Altere a data para 1º de maio de 2019 ou depois.

> [!NOTE]
> Você não poderá alterar a data com base nas configurações da organização; Se esse for o caso, fale com seu administrador. Como alternativa, você pode editar sua chave de registro de espaço reservado para ter uma data que já passou.

## <a name="test-your-application"></a>Teste seu aplicativo

Agora, teste como seu aplicativo lida com a nova era. Verifique os lugares em que a data é exibida, como carimbos de data e hora e seletores de data. Certifique-se de que a era está correta antes de 1 de maio de 2019 (Heisei 平成) e depois (Reiwa 令和).

### <a name="gannen-"></a>*Gannen* (元年)

O formato para o calendário japonês é geralmente  **&lt;nome da Era&gt; &lt;ano da era&gt;** . Por exemplo, o ano 2018 é **Heisei 30** (平成30年).  No entanto, o primeiro ano de uma era é especial; em vez de ser **&lt;Nome da era&gt; 1**, é **&lt;Nome da era&gt; 元年** (*gannen*). Portanto, o primeiro ano da era Heisei seria 平成元年 (*Heisei gannen*). Certifique-se de que seu aplicativo manipula adequadamente o primeiro ano da nova era e gera corretamente 令和元年.

## <a name="related-apis"></a>APIs relacionadas

Há várias APIs do WinRT, .NET e Win32 que serão atualizadas para lidarem com a mudança de era. Caso você as use, não precisará se preocupar. No entanto, mesmo se você depender inteiramente dessas APIs, ainda é uma boa ideia testar seu aplicativo e certificar-se de que você obtenha o comportamento desejado, especialmente se você estiver fazendo algo importante com elas, como análises.

Você pode seguir junto com as atualizações para o sistema operacional e os SDKs no [atualizações para talvez 2019 Japão Era alteração](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

As seguintes APIs serão afetadas:

### <a name="winrt"></a>WinRT

* [Namespace Windows.Globalization](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Classe de calendário](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
    * [Método AddDays](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
    * [Método AddEras](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
    * [Método AddHours](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
    * [Método AddMinutes](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
    * [Método AddMonths](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
    * [Método AddNanoseconds](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
    * [Método AddPeriods](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
    * [Método AddSeconds](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
    * [Método AddWeeks](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
    * [Método AddYears](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
    * [Propriedade era](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [Método EraAsString](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [Propriedade FirstYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [Propriedade LastEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [Propriedade LastYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [Propriedade NumberOfYearsInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Namespace Windows.Globalization.DateTimeFormatting](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [Classe DateTimeFormatter](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Método Format](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [Namespace do sistema](https://docs.microsoft.com/dotnet/api/system)
  * [Estrutura DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [Estrutura DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [Namespace System. Globalization](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Classe de calendário](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [Classe DateTimeFormatInfo](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [Classe JapaneseCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [Classe JapaneseLunisolarCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [cabeçalho datetimeapi.h](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
  * [GetDateFormatA function](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [Função GetDateFormatEx](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [GetDateFormatW function](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [cabeçalho winnls.h](https://docs.microsoft.com/windows/desktop/api/winnls/)
  * [Função EnumDateFormatsA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [EnumDateFormatsExA function](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [Função EnumDateFormatsExEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [Função EnumDateFormatsExW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [Função EnumDateFormatsW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [Função GetCalendarInfoA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [Função GetCalendarInfoEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [Função GetCalendarInfoW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Consulte também

* [Era tratamento para o calendário japonês](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [Momento do calendário japonês de Y2K](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [Usando o registro para testar a nova Era de japonês no Windows](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Gannen vs Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [Atualizações para 2019 Japão Era podem mudar](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Tratamento de uma nova era no calendário japonês no .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
