---
title: Prepare seu aplicativo para a mudança de era japonesa
description: Saiba mais sobre a mudança de era japonesa em maio de 2019 e como preparar seu aplicativo.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 4/26/2019
ms.topic: article
keywords: windows 10, uwp, localizability, localization, japanese, era
ms.localizationpriority: high
ms.openlocfilehash: 54d66d0426e5f0c41d48b93ba96781786d6fab92
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66363774"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Prepare seu aplicativo para a mudança de era japonesa

> [!NOTE]
> Em 1º de abril de 2019, o nome da nova era foi anunciado: Reiwa (令和). Em 25 de abril, a Microsoft lançou pacotes para diferentes sistemas operacionais do Windows contendo a chave do Registro atualizada com o nome da nova era. Atualize seu dispositivo e verifique seu Registro para ver se ele tem a nova chave e, em seguida, teste seu aplicativo. Confira [este artigo de suporte](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) para garantir que seu sistema operacional recebeu a chave do Registro atualizada.

O calendário japonês é dividido em eras e, durante quase toda a era moderna da computação, estivemos na era Heisei. No entanto, em 1 de maio de 2019, uma nova era será iniciada. Como esta é a primeira mudança de era em décadas, os softwares compatíveis com o calendário japonês precisarão ser testados para se garantir que funcionarão corretamente quando a nova era começar.

Nas seções a seguir, você aprenderá o que pode fazer para preparar e testar seu aplicativo para a nova era.

> [!NOTE]
> Recomendamos que você use um computador de teste para isso, pois as alterações feitas afetarão o comportamento de todo o computador.

## <a name="add-a-registry-key-for-the-new-era"></a>Adicionar uma chave do Registro para a nova era

> [!NOTE]
> As instruções a seguir destinam-se a dispositivos que ainda não foram atualizados com a nova chave do Registro. Primeiro, verifique se o dispositivo contém a nova chave do Registro e, se não contiver, tente usar as instruções a seguir.

É importante testar os problemas de compatibilidade antes da mudança de era. Você pode fazer isso agora mesmo usando o nome da nova era. Para isso, adicione uma chave do Registro para a nova era usando o **Editor do Registro**:

1. Navegue até **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Selecione **Editar > Novo > Valor da cadeia** e nomeie como **2019 05 01**.
3. Clique com o botão direito do mouse na chave e selecione **Modificar**.
4. No campo **Dados de valor**, insira **令和_令_Reiwa_R** (você pode copiar e colar daqui para ficar mais fácil).

Confira [Gerenciamento de eras para o calendário japonês](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar) para ler mais sobre o formato dessas chaves do Registro.

Em 1º de abril de 2019, o nome da nova era foi anunciado. Em 25 de abril, foi lançada uma atualização com uma nova chave do Registro para as versões com suporte do Windows contendo o nome, permitindo que você valide que seu aplicativo lidará com isso adequadamente. Esta atualização está sedo propagada para versões anteriores compatíveis do Windows 10, bem como do Windows 8 e 7.

Você pode excluir a chave do Registro de espaço reservado depois que terminar de testar seu aplicativo. Isso garantirá que ele não interfira com a nova chave do Registro que será adicionada quando o Windows for atualizado.

## <a name="change-your-devices-calendar-format"></a>Alterar o formato do calendário de seu dispositivo

Depois de adicionar a chave do Registro para a nova era, você precisará configurar seu dispositivo para usar o calendário japonês. Você não precisa ter um dispositivo no idioma japonês para fazer isso. Para testes extensivos, é interessante instalar o pacote de idioma japonês também, mas isso não é necessário para os testes básicos.

Para configurar seu dispositivo para usar o calendário japonês:

1. Acesse **intl.cpl** (pesquise na barra de pesquisa do Windows).
2. No menu suspenso **Formato**, selecione **Japonês (Japão)** .
3. Selecione **Configurações adicionais**.
4. Selecione a guia **Data**.
5. No menu suspenso **Tipo de calendário**, selecione **和暦** (*wareki*, o calendário japonês). Ele deve aparecer como a segunda opção.
6. Clique em **OK**.
7. Clique em **OK** na janela **Região**.

Agora, seu dispositivo deve estar configurado para usar o calendário japonês e ele refletirá a era que estiver no Registro. Abaixo está um exemplo do que você poderá ver agora no canto inferior direito da tela:

![Data e hora no formato do calendário japonês](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Ajuste o relógio de seu dispositivo

Para testar se o seu aplicativo funciona com a nova era, é necessário avançar o relógio do computador para 1º de maio de 2019 ou depois. As seguintes instruções são para Windows 10, mas o Windows 8 e 7 devem funcionar de forma semelhante:

1. Clique com o botão direito do mouse na data e hora no canto inferior direito da tela.
2. Selecione **Ajustar data/hora**.
3. No aplicativo Configurações, em **Alterar data e hora**, selecione **Alterar**.
4. Altere a data para 1º de maio de 2019 ou depois.

> [!NOTE]
> Talvez você não possa alterar a data com base nas configurações da organização. Se esse for o caso, fale com seu administrador. Como alternativa, você pode editar a chave do Registro de espaço reservado para ter uma data no passado.

## <a name="test-your-application"></a>Testar o aplicativo

Agora, teste como seu aplicativo lida com a nova era. Verifique os locais em que a data é exibida, como carimbos de data e hora e seletores de data. Certifique-se de que a era esteja correta antes de 1º de maio de 2019 (Heisei, 平成) e depois de (Reiwa, 令和).

### <a name="gannen-"></a>*Gannen* (元年)

Normalmente, o formato do calendário japonês é **&lt;Nome da era&gt; &lt;Ano da era&gt;** . Por exemplo, o ano 2018 é **Heisei 30** (平成30年).  No entanto, o primeiro ano de uma era é especial; em vez de ser **&lt;Nome da era&gt; 1**, é **&lt;Nome da era&gt; 元年** (*gannen*). Portanto, o primeiro ano da era Heisei seria 平成元年 (*Heisei gannen*). Certifique-se de que seu aplicativo lide corretamente com o primeiro ano da nova era e gere corretamente 令和元年.

## <a name="related-apis"></a>APIs relacionadas

Há várias APIs do WinRT, .NET e Win32 que serão atualizadas para lidarem com a mudança de era. Caso você as use, não precisará se preocupar. No entanto, mesmo se você depender inteiramente dessas APIs, ainda será uma boa ideia testar seu aplicativo e garantir que você obtenha o comportamento desejado, especialmente se estiver fazendo algo importante com elas, como análises.

Você pode acompanhar as atualizações do sistema operacional e do SDK em [Atualizações para a mudança de era japonesa de maio de 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

As seguintes APIs serão afetadas:

### <a name="winrt"></a>WinRT

* [Namespace Windows.Globalization](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Classe Calendar](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
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
    * [Propriedade Era](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [Método EraAsString](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [Propriedade FirstYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [Propriedade LastEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [Propriedade LastYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [Propriedade NumberOfYearsInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Namespace Windows.Globalization.DateTimeFormatting](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [Classe DateTimeFormatter](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Método Format](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [Namespace System](https://docs.microsoft.com/dotnet/api/system)
  * [Estrutura DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [Estrutura DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [Namespace System.Globalization](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Classe Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [Classe DateTimeFormatInfo](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [Classe JapaneseCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [Classe JapaneseLunisolarCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [Cabeçalho datetimeapi.h](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
  * [Função GetDateFormatA](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [Função GetDateFormatEx](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [Função GetDateFormatW](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [Cabeçalho winnls.h](https://docs.microsoft.com/windows/desktop/api/winnls/)
  * [Função EnumDateFormatsA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [Função EnumDateFormatsExA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [Função EnumDateFormatsExEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [Função EnumDateFormatsExW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [Função EnumDateFormatsW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [Função GetCalendarInfoA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [Função GetCalendarInfoEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [Função GetCalendarInfoW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Consulte também

* [Gerenciamento de eras para o calendário japonês](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [O momento dos anos 2000 do calendário japonês](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [Como usar o Registro para testar a nova era japonesa no Windows](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Gannen vs Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [Atualizações para a mudança de era japonesa em maio de 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Como lidar com uma nova era do calendário japonês no .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
