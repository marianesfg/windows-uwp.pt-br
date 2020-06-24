---
title: IPC (Comunicação Entre Processos)
description: Este tópico explica várias maneiras de executar a IPC (comunicação entre processos) entre os aplicativos Plataforma Universal do Windows (UWP) e os aplicativos Win32.
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 5db029db3ffb538802f39aa616c96dbe75601eac
ms.sourcegitcommit: bf7d4f6739aeeaac735aae3dd0dcbda63a8c5e69
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256376"
---
# <a name="interprocess-communication-ipc"></a>IPC (Comunicação Entre Processos)

Este tópico explica várias maneiras de executar a IPC (comunicação entre processos) entre os aplicativos Plataforma Universal do Windows (UWP) e os aplicativos Win32.

## <a name="app-services"></a>Serviços de app

Os serviços de aplicativos permitem que os aplicativos exponham serviços que aceitam e retornam pacotes de propriedades de primitivos ([**valor**](/uwp/api/Windows.Foundation.Collections.ValueSet)) em segundo plano. Os objetos avançados podem ser passados se forem [serializados](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu).

Os serviços de aplicativos podem ser executados [fora do processo](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) como uma tarefa em segundo plano ou [em processo](/windows/uwp/launch-resume/convert-app-service-in-process) dentro do aplicativo em primeiro plano.

Os serviços de aplicativos são mais bem usados para compartilhar pequenas quantidades de dados em que a latência quase em tempo real não é necessária.

## <a name="com"></a>COM

O [com](/windows/win32/com/component-object-model--com--portal) é um sistema distribuído orientado a objeto para a criação de componentes de software binários que podem interagir e se comunicar. Como desenvolvedor, você usa COM para criar componentes de software reutilizáveis e camadas de automação para um aplicativo. Os componentes COM podem estar em processo ou fora do processo e podem se comunicar por meio de um modelo de [cliente e de servidor](/windows/win32/com/com-clients-and-servers) . Os servidores COM fora do processo têm sido usados há muito tempo como um meio de [comunicação entre objetos](/windows/win32/com/inter-object-communication).

Os aplicativos empacotados com o recurso [runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) podem registrar servidores com fora do processo para IPC por meio do [manifesto do pacote](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension). Isso é conhecido como [com empacotado](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/).

## <a name="filesystem"></a>Sistema de arquivos

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

Os aplicativos empacotados podem executar o IPC usando o amplo sistema de arquivos, declarando o recurso restrito [broadFileSystemAccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations) . Esse recurso concede ao [Windows. Storage](/uwp/api/Windows.Storage) APIs e às APIs do [xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) do Win32 acesso ao amplo sistema de arquivos.

Por padrão, o IPC por meio do sistema de arquivos para aplicativos empacotados é restrito aos outros mecanismos descritos nesta seção.

### <a name="publishercachefolder"></a>PublisherCacheFolder

O [PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder) permite que aplicativos empacotados declarem pastas em seu manifesto que podem ser compartilhadas com outros pacotes pelo mesmo editor.

A pasta de armazenamento compartilhado tem os seguintes requisitos e restrições:

* Não é feito backup ou roaming dos dados na pasta de armazenamento compartilhado.
* O usuário pode limpar o conteúdo da pasta de armazenamento compartilhado.
* Você não pode usar a pasta de armazenamento compartilhado para compartilhar dados entre aplicativos de Publicadores diferentes.
* Você não pode usar a pasta de armazenamento compartilhado para compartilhar dados entre usuários diferentes.
* A pasta de armazenamento compartilhado não tem gerenciamento de versão.

Se você publicar vários aplicativos e estiver procurando um mecanismo simples para compartilhar dados entre eles, o PublisherCacheFolder será uma opção simples baseada em FileSystem.

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[SharedAccessStorageManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) é usado em conjunto com os serviços de aplicativos, ativações de protocolo (por exemplo, LaunchUriForResultsAsync), etc., para compartilhar StorageFiles por meio de tokens.

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

Com o recurso [runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) , os aplicativos empacotados podem [iniciar processos de confiança total](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher) dentro do mesmo pacote.

Para cenários em que as restrições de pacote são um fardo, ou as opções de IPC não estão disponíveis, um aplicativo pode usar um processo de confiança total como um proxy para interagir com o sistema e, em seguida, IPC com o processo de confiança total por meio dos serviços de aplicativos ou algum outro mecanismo IPC com suporte.

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](/windows/uwp/launch-resume/how-to-launch-an-app-for-results) é usado para troca de dados simples ([ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)) com outros aplicativos empacotados que implementam o contrato de ativação [ProtocolForResults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results) . Ao contrário dos serviços de aplicativos, que normalmente são executados em segundo plano, o aplicativo de destino é iniciado em primeiro plano.

Os arquivos podem ser compartilhados passando tokens [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) para o aplicativo por meio do valorset.

## <a name="loopback"></a>Loopback

Loopback é o processo de comunicação com um servidor de rede que escuta no localhost (o endereço de loopback).

Para manter o isolamento de segurança e rede, as conexões de loopback para IPC são bloqueadas por padrão para aplicativos empacotados. Você pode habilitar conexões de loopback entre aplicativos de pacote confiáveis usando [as propriedades](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)de [recursos](/previous-versions/windows/apps/hh770532(v=win.10)) e de manifesto.

* Todos os aplicativos empacotados que participam de conexões de loopback precisarão declarar o `privateNetworkClientServer` recurso em seus [manifestos de pacote](/uwp/schemas/appxpackage/uapmanifestschema/element-capability).
* Dois aplicativos empacotados podem se comunicar por loopback declarando [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) em seus manifestos de pacote.
    * Cada aplicativo deve listar o outro em seu [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules). O cliente declara uma regra "out" para o servidor e o servidor declara as regras "in" para seus clientes com suporte.

> [!NOTE]
> O nome da família de pacotes necessário para identificar um aplicativo nessas regras pode ser encontrado por meio do editor de manifesto de pacote no Visual Studio durante o tempo de desenvolvimento, por meio do [Partner Center](/windows/uwp/publish/view-app-identity-details) para aplicativos publicados por meio da Microsoft Store ou por meio do comando [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) do PowerShell para aplicativos que já estão instalados.

Os aplicativos e serviços desempacotados não têm a identidade do pacote, portanto, eles não podem ser declarados em [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules). Você pode configurar um aplicativo empacotado para se conectar por meio de auto-retorno com aplicativos e serviços desempacotados por meio de [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)), no entanto, isso só é possível para cenários de Sideload ou de depuração em que você tem acesso local ao computador e tem privilégios de administrador.

* Todos os aplicativos empacotados que participam de conexões de loopback precisam declarar a `privateNetworkClientServer` funcionalidade em seus [manifestos de pacote](/uwp/schemas/appxpackage/uapmanifestschema/element-capability).
* Se um aplicativo empacotado estiver se conectando a um aplicativo ou serviço não empacotado, execute `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` para adicionar uma isenção de loopback para o aplicativo empacotado.
* Se um aplicativo ou serviço não empacotado estiver se conectando a um aplicativo empacotado, execute `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` para permitir que o aplicativo empacotado receba conexões de loopback de entrada.
    * [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) deve estar em execução continuamente enquanto o aplicativo empacotado está escutando conexões.
    * O `-is` sinalizador foi introduzido no Windows 10, versão 1607 (10,0; Build 14393).

> [!NOTE]
> O nome da família de pacotes necessário para o `-n` sinalizador de [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) pode ser encontrado por meio do editor de manifesto de pacote no Visual Studio durante o tempo de desenvolvimento, por meio do [Partner Center](/windows/uwp/publish/view-app-identity-details) para aplicativos publicados por meio do Microsoft Store ou por meio do comando [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) do PowerShell para aplicativos que já estão instalados.

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) também é útil para [depurar problemas de isolamento de rede](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues).

## <a name="pipes"></a>Pipes

Os [pipes](/windows/win32/ipc/pipes) permitem uma comunicação simples entre um servidor de pipe e um ou mais clientes de pipe.

[Pipes anônimos](/windows/win32/ipc/anonymous-pipes) e [pipes nomeados](/windows/win32/ipc/named-pipes) têm suporte com as seguintes restrições:

* Por padrão, os pipes nomeados em aplicativos empacotados só têm suporte entre os processos no mesmo pacote, a menos que um processo seja confiança total.
* Pipes nomeados podem ser compartilhados entre pacotes seguindo as diretrizes para [compartilhar objetos nomeados](/windows/uwp/communication/sharing-named-objects).
* Pipes nomeados em aplicativos empacotados devem usar a sintaxe `\\.\pipe\LOCAL\` para o nome do pipe.

## <a name="registry"></a>Registro

O uso [do registro](/windows/win32/sysinfo/registry-functions) para IPC geralmente não é recomendado, mas tem suporte para o código existente. Os aplicativos empacotados podem acessar somente as chaves do registro que têm permissão para acessar.

[Os aplicativos de desktop empacotados como MSIX](/windows/msix/desktop/desktop-to-uwp-root) aproveitam a [virtualização do registro](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) , de modo que as gravações globais do registro estão contidas em um Hive privado dentro do pacote MSIX. Isso permite a compatibilidade do código-fonte, minimizando o impacto do registro global e pode ser usado para IPC entre os processos no mesmo pacote. Se você precisar usar o registro, esse modelo é preferencial em oposição à manipulação do registro global.

## <a name="rpc"></a>RPC

O [RPC](/windows/win32/rpc/rpc-start-page) pode ser usado para conectar um aplicativo empacotado a um ponto de extremidade RPC do Win32, desde que o aplicativo empacotado tenha os recursos corretos para corresponder as ACLs no ponto de extremidade RPC.

Recursos personalizados permitem que OEMs e IHVs [definam recursos arbitrários](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability), [ACL seus pontos de extremidade RPC com eles](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)e, em seguida, [conceda esses recursos a aplicativos cliente autorizados](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file). Para obter um aplicativo de exemplo completo, consulte o exemplo [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) .

Os pontos de extremidade RPC também podem ser ACLeddos a aplicativos empacotados específicos para limitar o acesso ao ponto final apenas aos aplicativos sem exigir a sobrecarga de gerenciamento de recursos personalizados. Você pode usar a API [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) para derivar um SID de um nome de família de pacotes e, em seguida, ACL o ponto de extremidade RPC com o Sid, conforme mostrado no exemplo de [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp) .

## <a name="shared-memory"></a>Memória compartilhada

O [mapeamento de arquivo](/windows/win32/memory/sharing-files-and-memory) pode ser usado para compartilhar um arquivo ou memória entre dois ou mais processos com as seguintes restrições:

* Por padrão, os mapeamentos de arquivo em aplicativos empacotados têm suporte apenas entre os processos no mesmo pacote, a menos que um processo seja confiança total.
* Mapeamentos de arquivo podem ser compartilhados entre pacotes seguindo as diretrizes para [compartilhar objetos nomeados](/windows/uwp/communication/sharing-named-objects).

A memória compartilhada é recomendada para compartilhar e manipular grandes quantidades de dados com eficiência.
