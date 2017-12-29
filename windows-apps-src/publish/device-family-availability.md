---
author: jnHs
Description: "Depois de seus pacotes serem carregados com sucesso, você verá uma tabela que indica quais pacotes serão oferecidos para famílias de dispositivos Windows 10 específicas (e versões anteriores do sistema operacional, se aplicável), na ordem de classificação."
title: "Disponibilidade da família de dispositivos"
ms.author: wdg-dev-content
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, pacotes, carregar, disponibilidade da família de dispositivos"
localizationpriority: high
ms.openlocfilehash: 4125d60c02dd70e1f04701f9364dc28245bc7a98
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2017
---
# <a name="device-family-availability"></a>Disponibilidade da família de dispositivos

Depois que os pacotes tiverem sido carregados com êxito na página **Pacotes**, a seção **Disponibilidade da família de dispositivos** exibirá uma tabela que indica quais pacotes serão oferecidos a famílias de dispositivos Windows 10 específicas (e versões anteriores do sistema operacional, se aplicável), em ordem de classificação. Esta seção também permite escolher se você deseja oferecer ou não o envio para clientes em famílias de dispositivos Windows 10 específicas.

> [!NOTE]
> Se você ainda não tiver carregado pacotes, a seção **Disponibilidade da família de dispositivos** mostrará as famílias de dispositivos Windows 10 com caixas de seleção que permitem que você indique se o envio será oferecido ou não para clientes nessas famílias de dispositivos. A tabela será exibida após carregamento de um ou mais pacotes.

Esta seção também inclui uma caixa de seleção na qual você pode indicar se deseja permitir que a Microsoft disponibilize o aplicativo para quaisquer famílias de dispositivos Windows 10 futuras. É recomendável manter essa caixa marcada para que seu aplicativo possa estar disponível para clientes em potencial, conforme novas famílias de dispositivos forem introduzidas.


## <a name="choosing-which-device-families-to-support"></a>Escolha de quais famílias de dispositivos são compatíveis

Se você carregar pacotes para uma família de dispositivos individuais, marcaremos a caixa para tornar esses pacotes disponíveis para novos clientes nesse tipo de dispositivo. Por exemplo, se um pacote tiver como destino Windows.Desktop, a caixa **Área de trabalho do Windows 10** será marcada para esse pacote (e você não conseguirá marcar as caixas para outras famílias de dispositivos).

Pacotes destinados à família de dispositivos Windows.Universal podem ser executados em qualquer dispositivo Windows 10 (incluindo Xbox One). Por padrão, deixaremos esses pacotes disponíveis a novos clientes em todos os tipos de dispositivo *exceto* para Xbox.

Você poderá desmarcar a caixa de seleção de qualquer família de dispositivos Windows 10, se não quiser oferecer o envio para clientes desse tipo de dispositivo. Se caixa de uma família de dispositivos estiver desmarcada, os novos clientes nesse tipo de dispositivo não conseguirão adquirir o aplicativo (embora os clientes que já tenham o aplicativo ainda possam usá-lo e recebam todas as atualizações enviadas).

Se seu aplicativo oferece suportem a eles, recomendamos deixar todas as caixas marcadas, a menos que você tenha um motivo específico para limitar os tipos de dispositivos do Windows 10 que podem adquirir o seu aplicativo. Por exemplo, se você sabe que seu aplicativo não oferece uma boa experiência em [Surface Hub](https://developer.microsoft.com/windows/surfacehub) e/ou [Microsoft HoloLens](http://dev.windows.com/holographic/development_overview), você pode desmarcar a caixa **Windows 10 Team** e/ou **Windows 10 Holographic**. Isso impede que novos clientes adquiram o aplicativo nesses dispositivos. Se você decidir mais tarde que está pronto para oferecê-lo a esses clientes, você pode criar um novo envio com as caixas marcadas.

<span id="xbox" />
A única família de dispositivos Windows 10 que não está selecionada por padrão para os pacotes Windows.Universal é **Windows 10 Xbox**. Se o aplicativo não for um jogo (ou se for um jogo e você tiver ativado o [Programa de Criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) ou passado pelo processo de [aprovação do conceito](../gaming/concept-approval.md)) e o envio incluir pacotes UWP neutros e/ou x64 compilados usando o SDK do Windows 10, versão 14393 ou posterior, você poderá marcar a caixa **Windows 10 Xbox** para oferecer o aplicativo aos clientes em Xbox One.

> [!IMPORTANT]
> Para que o aplicativo seja inicializado em dispositivos Xbox, você deve incluir um pacote neutro ou x64 compilado com o SDK do Windows versão 14393 ou superior. No entanto, se você marcar **Windows 10 Xbox**, o pacote com a versão mais alta aplicável ao Xbox (ou seja, um pacote neutro ou x64 destinado à família de dispositivos Xbox ou Universal) sempre será oferecido aos clientes em Xbox, mesmo se ele estiver compilado com uma versão do SDK anterior. Por isso, é importante garantir que o pacote com a versão mais alta aplicável ao Xbox seja compilado com o SDK do Windows versão 14393 ou superior. Do contrário, você verá uma mensagem de erro indicando que os clientes do Xbox não serão capazes de iniciar o aplicativo. 
> 
> Para resolver esse erro, você pode fazer um dos seguintes:
> - Substitua os pacotes aplicáveis por novos compilados usando-se o SDK do Windows versão 14393 ou superior.
> - Se você já tiver um pacote compatível com Xbox e compilado com o SDK do Windows versão 14393 ou superior, aumente o número da versão, de maneira que ele seja o pacote com a versão mais alta no envio.
> - Desmarque a caixa de **Windows 10 Xbox**.
>   
> Se ainda assim você não conseguir resolver o problema, contate o suporte.

Se seu envio incluir pacotes que podem ser executados no **Windows 8/8.1** e **Windows Phone 8.x e anterior**, esses pacotes ficarão disponíveis aos clientes, como exibido na tabela. Não há caixas de seleção para essas versões do sistema operacional. Para interromper a oferta do aplicativo a esses clientes, remova os pacotes correspondentes do envio.

> [!IMPORTANT]
> Para evitar completamente que uma família de dispositivos do Windows 10 específica receba seu envio, atualize o elemento [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) em seu manifesto appx para direcionar somente à família de dispositivos a qual você deseja dar suporte (isto é, Windows.Mobile ou Windows.Desktop), em vez de deixá-lo como o valor Windows.Universal (para a família de dispositivos universal) que o Microsoft Visual Studio inclui no manifesto appx por padrão.

É importante também estar ciente de que as seleções feitas na seção **Disponibilidade da família de dispositivos** se aplica apenas a novas aquisições. Quem já tem o seu aplicativo pode continuar a usá-lo e receberá as atualizações enviadas, mesmo se você remover a família de dispositivos aqui. Isso se aplica inclusive aos clientes que compraram o aplicativo antes de atualizar para o Windows 10.

Por exemplo, se você tiver um aplicativo publicado com pacotes do Windows Phone 8.1, e mais tarde adicionar um pacote do Windows 10 (UWP) ao mesmo aplicativo direcionado à família de dispositivos universal, será oferecido para os clientes móveis do Windows 10 que tinham o pacote do Windows Phone 8.1 uma atualização para esse pacote do Windows 10 (UWP), mesmo se você tiver desmarcado a caixa **Windows 10 Mobile** (já que essa não é uma nova aquisição mas uma atualização). No entanto, se você não fornecer qualquer pacote do Windows 10 (UWP) destinado à família de dispositivos universal ou móveis, os clientes móveis do Windows 10 permanecerão com o pacote do Windows Phone 8.1.

Para saber mais sobre famílias de dispositivos, consulte [Introdução à UWP (Plataforma Universal do Windows)](../get-started/universal-application-platform-guide.md) e [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily).


## <a name="understanding-ranking"></a>Noções básicas sobre a classificação

Além de permitir indicar quais famílias de dispositivos Windows 10 podem baixar o envio, a seção **Disponibilidade da família de dispositivos** também mostra quais dos pacotes específicos serão disponibilizados para diferentes famílias de dispositivos. Se você tiver mais de um pacote que possa ser executado em uma determinada família de dispositivos, a tabela indicará a ordem na qual os pacotes serão oferecidos, com base nos números de versão dos pacotes. Para obter mais informações sobre como a Store classifica pacotes com base em números de versão, consulte [Numeração de versão do pacote](package-version-numbering.md). 

Por exemplo, digamos que você tenha dois pacotes: Package_A.appxupload e Package_B.appxupload. Para uma determinada família de dispositivos, se Package_A.appxupload tiver classificação 1 e Package_B.appxupload tiver classificação 2, isso significará que, quando um cliente nesse tipo de dispositivo adquirir o aplicativo, a Store tentará primeiramente entregar Package_A.appxupload. Se o dispositivo do cliente não for capaz de executar Package_A.appxupload, a Store oferecerá Package_B.appxupload. Se o dispositivo do cliente não conseguir executar nenhum dos pacotes dessa família de dispositivos – por exemplo, se a **MinVersion** compatível com o aplicativo for maior do que a versão no dispositivo do cliente, o cliente não conseguirá baixar o aplicativo nesse dispositivo.

> [!NOTE]
> Os números de versão em pacotes .xap não são considerados ao determinar qual pacote fornecer a um determinado cliente. Por isso, se tiver mais de um pacote .xap de mesma classificação, você verá um asterisco, e não um número, e os clientes poderão receber qualquer um dos pacotes. Para atualizar clientes de um pacote .xap para um mais recente, certifique-se de remover o .xap mais antigo no novo envio.
