---
author: jnHs
Description: "A página Pacotes é onde você carrega todos os arquivos de pacote (.xap, .appx, .appxupload e/ou .appxbundle) para o aplicativo que você está enviando. Você pode carregar pacotes para qualquer sistema operacional visado por seu aplicativo nesta etapa."
title: Carregar pacotes de aplicativo
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
translationtype: Human Translation
ms.sourcegitcommit: 9440d1343141edf92f5d9e8aafc63ca88b176354
ms.openlocfilehash: 2ca755568ff3697d5e0fe94900799f9dd98b7f72

---

# Carregar pacotes de aplicativo


A página **Pacotes** é onde você carrega todos os arquivos de pacote (.appx, .appxupload, .appxbundle e/ou .xap) para o aplicativo que você está enviando. Você pode carregar pacotes para qualquer sistema operacional visado por seu aplicativo nesta etapa. Quando um cliente baixar o aplicativo, a Loja fornecerá automaticamente para cada cliente o pacote que funciona melhor para os dispositivos. Depois de carregar os pacotes, você verá uma tabela indicando [quais pacotes serão oferecidos para famílias de dispositivos Windows 10 específicas](#device-family-availability) (e versões anteriores do sistema operacional, se aplicável) na ordem de classificação.

Para obter detalhes sobre o que um pacote inclui e como ele deve ser estruturado, veja [Requisitos do pacote do aplicativo](app-package-requirements.md). Você também vai querer saber [como os números de versão podem afetar quais pacotes são entregues a clientes específicos](package-version-numbering.md) e saber [como os pacotes são distribuídos para diferentes sistemas operacionais](guidance-for-app-package-management.md).

## Carregando pacotes para seu envio

Para carregar os pacotes, arraste-os para o campo de carregamento ou clique em para procurar os arquivos. A página **Pacotes** permitirá que você carregue arquivos .xap, .appx, .appxupload e/ou .appxbundle.

Caso tenha criado [pacotes de pré-lançamento](package-flights.md) para seu aplicativo, você verá uma lista suspensa com a opção para copiar pacotes de um dos pacotes de pré-lançamento. Selecione o pacote de pré-lançamento que tiver os pacotes que você deseja puxar. Em seguida, você pode selecionar qualquer um ou todos os seus pacotes para incluir nesse envio.

> **Importante**  Para o Windows 10, você deve sempre carregar o arquivo .appxupload aqui e não o .appx nem o .appxbundle. Para obter mais informações sobre o empacotamento de aplicativos UWP da Loja, consulte [Empacotando aplicativos Universais do Windows para Windows 10](../packaging/packaging-uwp-apps.md).

Se detectarmos problemas com seus pacotes ao validá-los, você precisará remover o pacote, corrigir o problema e tentar carregá-lo novamente. Para saber mais, consulte [Corrigir erros de carregamento de pacote](resolve-package-upload-errors.md).

Você também pode ver avisos para que saiba quais questões podem causar problemas, mas não impedem que você continue o seu envio.

## Disponibilidade da família de dispositivos

Depois que os pacotes tiverem sido carregados com êxito, a seção **Disponibilidade da família de dispositivos** exibirá uma tabela que indica quais pacotes serão oferecidos a famílias de dispositivos Windows 10 específicas (e versões anteriores do sistema operacional, se aplicável), em ordem de classificação. Esta seção também permite escolher se você deseja oferecer o envio para clientes em famílias de dispositivos Windows 10 específicas.

> **Observação** Se você ainda não tiver carregado pacotes, a seção **Disponibilidade da família de dispositivos** mostrará as famílias de dispositivos Windows 10 com caixas de seleção para indicar se o envio será oferecido ou não para clientes nessas famílias de dispositivos. A tabela não será exibida até você carregar os pacotes.

Você também verá uma caixa de seleção na qual pode indicar se deseja permitir que a Microsoft disponibilize o aplicativo para famílias de dispositivos Windows 10 futuras. É recomendável manter essa caixa marcada para que seu aplicativo possa estar disponível para clientes em potencial, conforme novas famílias de dispositivos forem introduzidas.

### Escolha de quais famílias de dispositivos são compatíveis

Você poderá desmarcar a caixa de seleção de qualquer família de dispositivos Windows 10, se não quiser oferecer o envio para clientes desse tipo de dispositivo. Se caixa de uma família de dispositivos estiver desmarcada, os novos clientes nesse tipo de dispositivo não conseguirão adquirir o aplicativo (embora os clientes que já tenham o aplicativo ainda possam usá-lo e recebam todas as atualizações enviadas). 

> **Observação** Não há caixas de seleção para **Windows 8/8.1** e **Windows Phone 8.x e versões anteriores**. Se o envio incluir pacotes que possam ser executados nessas versões do sistema operacional, esses pacotes estarão disponíveis para os clientes. Para interromper a oferta do aplicativo para clientes em versões anteriores do sistema operacional, você precisará remover os pacotes correspondentes do envio.

Se o aplicativo der suporte às famílias de dispositivos móveis e desktop, será recomendável manter as caixas **Windows 10 Mobile** e **Windows 10 Desktop** marcadas, a menos que tenha um motivo específico para limitar os tipos de dispositivos Windows 10 que podem adquirir o aplicativo. Por exemplo, você pode ter criado pacotes universais do Windows, mas sabe que ainda precisa testar alguns problemas com o aplicativo em dispositivos móveis. Para impedir que novos clientes baixem o aplicativo em dispositivos móveis do Windows 10, você pode desmarcar a caixa de seleção **Windows 10 Mobile** aqui. Em seguida, se você decidir mais tarde que está pronto para oferecê-lo aos clientes em dispositivos móveis do Windows 10, crie um novo envio com a caixa **Windows 10 Mobile** marcada.

Se o aplicativo não for um jogo (ou se for um jogo e você tiver passado pelo processo de [aprovação do conceito](../gaming/concept-approval.md)) e o envio incluir pacotes UWP compilados usando-se o SDK do Windows 10 versão 14393 ou posterior, você poderá marcar a caixa **Windows 10 Xbox** para oferecer o aplicativo aos clientes em Xbox. 

> **Importante** Para que o aplicativo seja inicializado em dispositivos Xbox, o pacote deve ser compilado com o SDK do Windows versão 14393 ou superior. No entanto, se você marcar Windows 10 Xbox, o pacote com a versão mais alta aplicável ao Xbox (ou seja, um pacote destinado à família de dispositivos Xbox ou Universal) sempre será oferecido aos clientes em Xbox, mesmo se ele estiver compilado com uma versão do SDK anterior. Por isso, é importante garantir que o pacote com a versão mais alta aplicável ao Xbox seja compilado com o SDK do Windows versão 14393 ou superior. Do contrário, você verá uma mensagem de erro indicando que os clientes do Xbox não serão capazes de iniciar o aplicativo. 
> 
> Para resolver esse erro, você pode fazer um dos seguintes:
> - Substitua os pacotes aplicáveis por novos compilados usando-se o SDK do Windows versão 14393 ou superior.
> - Se você já tiver um pacote compatível com Xbox e compilado com o SDK do Windows versão 14393 ou superior, aumente o número da versão, de maneira que ele seja o pacote com a versão mais alta no envio.
> - Desmarque a caixa de **Windows 10 Xbox**.
>   
> Se ainda assim você não conseguir resolver o problema, contate o suporte.

Se você testou o aplicativo para garantir que ele seja executado corretamente no Microsoft HoloLens, também poderá marcar a caixa **Windows 10 Holographic** para oferecer o aplicativo aos clientes do HoloLens. Para saber mais sobre como criar, testar e publicar aplicativos holográficos, consulte a [Visão geral de Desenvolvimento Holográfico do Windows](http://dev.windows.com/holographic/development_overview).

> **Importante** Para evitar completamente que uma família de dispositivos do Windows 10 específica obtenha seu aplicativo, você precisa atualizar o elemento [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) em seu manifesto appx para direcionar somente à família de dispositivos a qual você deseja dar suporte (isto é, Windows.Mobile ou Windows.Desktop), em vez de deixá-lo como o valor Windows.Universal (para a família de dispositivos universal) que o Microsoft Visual Studio inclui no manifesto appx por padrão.

É importante estar ciente de que as seleções feitas aqui serão aplicadas apenas a novas aquisições. Quem já tem o seu aplicativo pode continuar a usá-lo e receberá as atualizações enviadas, mesmo se você remover essa família de dispositivos aqui. Isso se aplica inclusive aos clientes que compraram o aplicativo antes de atualizar para o Windows 10. Por exemplo, se você tiver um aplicativo publicado com pacotes do Windows Phone 8.1, e mais tarde adicionar um pacote do Windows 10 (UWP) ao mesmo aplicativo direcionado à família de dispositivos universal, será oferecido para os clientes móveis do Windows 10 que tinham o pacote do Windows Phone 8.1 uma atualização para esse pacote do Windows 10 (UWP), mesmo se você tiver desmarcado a caixa **Windows 10 Mobile** (já que essa não é uma nova aquisição mas uma atualização). No entanto, se você não fornecer qualquer pacote do Windows 10 (UWP) destinado à família de dispositivos universal ou móveis, os clientes móveis do Windows 10 permanecerão com o pacote do Windows Phone 8.1.

Para saber mais sobre famílias de dispositivos, veja [Guia para aplicativos UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/windows/apps/dn894631) e [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

### Noções básicas sobre a classificação

Além de permitir indicar quais famílias de dispositivos Windows 10 podem baixar o envio, esta seção também mostra quais dos pacotes específicos serão disponibilizados para famílias de dispositivos específicas. Se você tiver mais de um pacote que possa ser executado em uma determinada família de dispositivos, a tabela indicará a ordem na qual os pacotes serão oferecidos, com base nos números de versão dos pacotes. Para obter mais informações sobre como a Loja classifica pacotes com base em números de versão, consulte [Numeração de versão do pacote](package-version-numbering.md). 

Por exemplo, digamos que você tenha dois pacotes: Package_A.appxupload e Package_B.appxupload. Para uma determinada família de dispositivos, se Package_A.appxupload tiver classificação 1 e Package_B.appxupload tiver classificação 2, isso significará que, quando um cliente nesse tipo de dispositivo adquirir o aplicativo, a Loja tentará primeiramente entregar Package_A.appxupload. Se o dispositivo do cliente não for capaz de executar Package_A.appxupload, a Loja oferecerá Package_B.appxupload. Se o dispositivo do cliente não conseguir executar nenhum dos pacotes dessa família de dispositivos – por exemplo, se a **MinVersion** compatível com o aplicativo for maior do que a versão no dispositivo do cliente, o cliente não conseguirá baixar o aplicativo nesse dispositivo.

> **Observação** Os números de versão em pacotes .xap não são considerados ao determinar qual pacote fornecer a um determinado cliente. Por isso, se tiver mais de um pacote .xap de mesma classificação, você verá um asterisco, e não um número, e os clientes poderão receber qualquer um dos pacotes. Para atualizar clientes de um pacote .xap para um mais recente, certifique-se de remover o .xap mais antigo no novo envio.



## Detalhes do pacote

Depois que os pacotes tiverem sido carregados com êxito, eles serão listados, agrupados por sistema operacional de destino. O nome, a versão e a arquitetura do pacote serão exibidos. Para obter mais informações, como os idiomas compatíveis, os recursos do aplicativo e o tamanho do arquivo de cada pacote, clique em **Mostrar detalhes**.

Se estiver usando a [Mediação de anúncios do Windows](../monetize/use-ad-mediation-to-maximize-revenue.md), você também verá um link para configurar a mediação de anúncios de cada pacote.

Se você precisar remover um pacote do envio, clique no link **Remover** na parte inferior da seção **Detalhes** de cada pacote.

## Removendo pacotes redundantes

Se detectarmos que um ou mais dos seus pacotes são redundantes, exibiremos um aviso sugerindo que você remova os pacotes redundantes deste envio. Geralmente, isso ocorre quando você já tiver carregado pacotes e agora está fornecendo pacotes com versões superiores que dão suporte ao mesmo conjunto de clientes. Neste caso, nenhum cliente jamais obteria o pacote redundante porque agora há um pacote melhor para dar suporte a esses clientes (com maior número de versão).

Quando detectarmos que você tem pacotes redundantes, forneceremos uma opção para remover todos os pacotes redundantes deste envio automaticamente. Você também pode remover pacotes individualmente deste envio, se preferir.

## Distribuição gradual de pacote

Se o envio for uma atualização para um aplicativo publicado anteriormente, você verá uma caixa de seleção que diz **Roll out update gradually after this submission is published (to Windows 10 customers only)**. Isso permite escolher um percentual de clientes que receberão os pacotes do envio, de maneira que você possa monitorar comentários e dados analíticos para se certificar da atualização antes de implantá-la mais amplamente. Você pode aumentar a porcentagem (ou parar a atualização) a qualquer tempo sem precisar criar um novo envio. 

Para obter mais informações, consulte [Distribuição gradual de pacote](gradual-package-rollout.md).

## Atualização obrigatória

Se o envio for uma atualização para um aplicativo publicado anteriormente, você verá uma caixa de seleção que diz **Make this update mandatory**. Isso permite definir a data e a hora para uma atualização obrigatória, pressupondo que você tenha usado as APIs Windows.Services.Store para permitir que o aplicativo verifique programaticamente se há atualizações de pacote, baixe e instale os pacotes atualizados. O aplicativo deve segmentar o Windows 10, versão 1607, ou posterior para usar essa opção.

Para obter mais informações, consulte [Baixar e instalar atualizações de pacote para seu aplicativo](../packaging/self-install-package-updates.md).

 







<!--HONumber=Aug16_HO5-->


