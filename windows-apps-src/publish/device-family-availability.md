---
Description: After your packages have been successfully uploaded, you'll see a table that indicates which packages will be offered to specific Windows 10 device families (and earlier OS versions, if applicable), in ranked order.
title: Disponibilidade da família de dispositivos
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, pacotes, carregar, disponibilidade da família de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 217a6ab9f25ee533a754138db5cf83c2ac81e3e9
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8893546"
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

Se seu aplicativo oferece suportem a eles, recomendamos deixar todas as caixas marcadas, a menos que você tenha um motivo específico para limitar os tipos de dispositivos do Windows 10 que podem adquirir o seu aplicativo. Por exemplo, se você sabe que seu aplicativo não oferece uma boa experiência em [Surface Hub](https://developer.microsoft.com/windows/surfacehub) e/ou [Microsoft HoloLens](https://developer.microsoft.com/windows/mixed-reality), você pode desmarcar a caixa **Windows 10 Team** e/ou **Windows 10 Holographic**. Isso impede que novos clientes adquiram o aplicativo nesses dispositivos. Se você decidir mais tarde que está pronto para oferecê-lo a esses clientes, você pode criar um novo envio com as caixas marcadas.

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

Se você estiver enviando um aplicativo UWP para o Windows 10 IoT Core, não deverá fazer alterações nas seleções padrão depois de carregar seus pacotes; não há nenhuma caixa de seleção separada para o Windows 10 IoT. Para obter mais informações sobre como publicar aplicativos UWP do IoT Core, consulte [Suporte da Microsoft Store para aplicativos UWP do IoT Core](https://docs.microsoft.com/windows/iot-core/commercialize-your-device/installingandservicing).

Se o envio para um aplicativo publicado anteriormente incluir pacotes que podem ser executados no **Windows 8/8.1** e **do Windows Phone 8. x e versões anteriores**, esses pacotes estarão disponíveis para clientes nessas versões do sistema operacional. Para interromper a oferta do aplicativo a esses clientes, remova os pacotes correspondentes do envio.

> [!IMPORTANT]
> Para evitar completamente que uma família de dispositivos Windows 10 específica Obtenha seu envio, atualize o elemento [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) no manifesto para direcionar somente a família de dispositivos que você deseja dar suporte (isto é, Windows ou Windows. desktop), em vez disso, que deixá-lo como o valor Windows. universal (para a família de dispositivos universal) que o Microsoft Visual Studio inclui no manifesto por padrão.

É importante também estar ciente de que as seleções feitas na seção **Disponibilidade da família de dispositivos** se aplica apenas a novas aquisições. Quem já tem o seu aplicativo pode continuar a usá-lo e receberá as atualizações enviadas, mesmo se você remover a família de dispositivos aqui. Isso se aplica inclusive aos clientes que compraram o aplicativo antes de atualizar para o Windows 10. Por exemplo, se você tiver um aplicativo publicado com pacotes do Windows Phone 8.1 e adicionar um pacote do Windows 10 (UWP) à família de dispositivos Windows. universal, clientes móveis do Windows 10 que tinham o pacote do Windows Phone 8.1 serão oferecidos uma atualização para esta Windows Pacote de 10 (UWP), mesmo se você tiver desmarcado a caixa para **Windows 10 Mobile**.

Para saber mais sobre as famílias de dispositivos, consulte [**Visão geral das famílias de dispositivos**](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview).


## <a name="understanding-ranking"></a>Noções básicas sobre a classificação

Além de permitir indicar quais famílias de dispositivos Windows 10 podem baixar o envio, a seção de **disponibilidade da família de dispositivo** mostra os pacotes específicos que serão disponibilizados para diferentes famílias de dispositivos. Se você tiver mais de um pacote que possa ser executado em uma determinada família de dispositivos, a tabela indicará a ordem na qual os pacotes serão oferecidos, com base nos números de versão dos pacotes. Para obter mais informações sobre como a Store classifica pacotes com base em números de versão, consulte [Numeração de versão do pacote](package-version-numbering.md). 

Por exemplo, digamos que você tenha dois pacotes: Package_A.appxupload e Package_B.appxupload. Para uma determinada família de dispositivos, se Package_A.appxupload tiver classificação 1 e Package_B.appxupload tiver classificação 2, isso significará que, quando um cliente nesse tipo de dispositivo adquirir o aplicativo, a Store tentará primeiramente entregar Package_A.appxupload. Se o dispositivo do cliente não for capaz de executar Package_A.appxupload, a Store oferecerá Package_B.appxupload. Se o dispositivo do cliente não conseguir executar nenhum dos pacotes dessa família (por exemplo, se o **MinVersion** seu aplicativo dá suporte a é maior que a versão no dispositivo do cliente), em seguida, o cliente não conseguirá baixar o aplicativo nesse dispositivo.

> [!NOTE]
> Os números de versão em pacotes. xap (para aplicativos publicados anteriormente) não são considerados ao determinar qual pacote fornecer a um determinado cliente. Por isso, se tiver mais de um pacote .xap de mesma classificação, você verá um asterisco, e não um número, e os clientes poderão receber qualquer um dos pacotes. Para atualizar clientes de um pacote .xap para um mais recente, certifique-se de remover o .xap mais antigo no novo envio.

