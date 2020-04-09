---
title: Compartilhando objetos nomeados
description: Este tópico explica como compartilhar objetos nomeados entre aplicativos Plataforma Universal do Windows (UWP) e aplicativos Win32.
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 95bbecd85b1dfa6f6e12766c082f3338de549677
ms.sourcegitcommit: 2d375e1c34473158134475af401532cc55fc50f4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888568"
---
# <a name="sharing-named-objects"></a>Compartilhando objetos nomeados

Este tópico explica como compartilhar objetos nomeados entre aplicativos Plataforma Universal do Windows (UWP) e aplicativos Win32.

## <a name="named-objects-in-packaged-applications"></a>Objetos nomeados em aplicativos empacotados

Os [objetos nomeados](/windows/win32/sync/object-names) fornecem uma maneira fácil para os processos compartilharem identificadores de objeto. Depois que um processo tiver criado um objeto nomeado, outros processos poderão usar o nome para chamar a função apropriada para abrir um identificador para o objeto. Os objetos nomeados são comumente usados para [sincronização de threads](/windows/win32/sync/interprocess-synchronization) e [comunicação entre processos](/windows/uwp/communication/interprocess-communication).

Por padrão, os aplicativos empacotados só podem acessar objetos nomeados que eles criaram. Para compartilhar objetos nomeados com aplicativos empacotados, as permissões devem ser definidas quando os objetos são criados e os nomes devem ser qualificados quando os objetos são abertos.

## <a name="creating-named-objects"></a>Criando objetos nomeados

Os objetos nomeados são criados com uma API de `Create` correspondente:

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

Todas essas APIs compartilham um parâmetro `LPSECURITY_ATTRIBUTES` que permite que o chamador especifique [ACLs (listas de controle de acesso)](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) para controlar quais processos podem acessar o objeto. Para compartilhar objetos nomeados com aplicativos empacotados, a permissão deve ser concedida dentro das ACLs quando os objetos nomeados são criados.

Identificadores de segurança (SIDs) representam identidades dentro de ACLs. Cada aplicativo empacotado tem seu próprio SID com base em seu nome de família de pacotes. Você pode gerar o SID para um aplicativo empacotado passando seu nome de família de pacotes para [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername).

> [!NOTE]
> O nome da família de pacotes pode ser encontrado por meio do editor de manifesto de pacote no Visual Studio durante o tempo de desenvolvimento, por meio do [Partner Center](/windows/uwp/publish/view-app-identity-details) para aplicativos publicados por meio da Microsoft Store ou por meio do comando [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) do PowerShell para aplicativos que já estão instalados.

[Este exemplo](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples) demonstra o padrão básico necessário para a ACL de um objeto nomeado. Para compartilhar objetos nomeados com aplicativos empacotados, crie uma estrutura de [EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w) para cada aplicativo:

* `grfAccessMode = GRANT_ACCESS`
* `grfAccessPermissions =` permissões apropriadas com base no objeto e uso pretendido
    * [Direitos de acesso genéricos](/windows/win32/secauthz/generic-access-rights)
    * [Segurança do objeto de sincronização e direitos de acesso](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [Segurança de mapeamento de arquivo e direitos de acesso](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =` o SID adquirido de [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)

Ao popular o parâmetro `LPSECURITY_ATTRIBUTES` em chamadas `Create` com regras de `EXPLICIT_ACCESS` para aplicativos empacotados, você pode conceder acesso a esses aplicativos para abrir o objeto nomeado.

> [!NOTE]
> Os aplicativos Win32 podem acessar todos os objetos nomeados criados por aplicativos empacotados contanto que qualifiquem os nomes de objeto ao [abri-los](#opening-named-objects). Eles não precisam receber acesso.

## <a name="opening-named-objects"></a>Abrindo objetos nomeados

Os objetos nomeados são abertos passando um nome para uma API de `Open` correspondente:

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

Os objetos nomeados criados por um aplicativo empacotado são criados dentro do namespace do aplicativo, também conhecido como o caminho do objeto nomeado. Ao abrir objetos nomeados criados por um aplicativo empacotado, os nomes de objeto devem ser prefixados com o caminho de objeto nomeado do aplicativo de criação.

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath) retornará o caminho do objeto nomeado para um aplicativo empacotado com base em seu Sid. Você pode gerar o SID para um aplicativo empacotado passando seu nome de família de pacotes para [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername).

> [!NOTE]
> O nome da família de pacotes pode ser encontrado por meio do editor de manifesto de pacote no Visual Studio durante o tempo de desenvolvimento, por meio do [Partner Center](/windows/uwp/publish/view-app-identity-details) para aplicativos publicados por meio da Microsoft Store ou por meio do comando [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) do PowerShell para aplicativos que já estão instalados.

Ao abrir objetos nomeados criados por um aplicativo empacotado, use o formato `<PATH>\<NAME>`:

* Substitua `<PATH>` pelo caminho do objeto nomeado do aplicativo de criação.
* Substitua `<NAME>` pelo nome do objeto.

> [!NOTE]
> A prefixação de nomes de objeto com `<PATH>` só será necessária se um aplicativo empacotado tiver criado o objeto. Os objetos nomeados criados por aplicativos Win32 não precisam ser qualificados, embora o acesso ainda deve ser concedido quando os objetos são [criados](#creating-named-objects).

## <a name="remarks"></a>Comentários

Os objetos nomeados em aplicativos empacotados são isolados por padrão para preservar a segurança e garantir o suporte para eventos de ciclo de vida do aplicativo, como suspensão e encerramento. Compartilhar objetos nomeados entre aplicativos introduz restrições de controle de versão e ligação e exige que cada aplicativo seja resiliente ao ciclo de vida de outros. Por esses motivos, é recomendável compartilhar somente objetos nomeados entre aplicativos do mesmo editor.
