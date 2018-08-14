---
author: normesta
Description: Este é um tópico central que abrange toda a imagem do desenvolvedor de como a Proteção de Informações do Windows (WIP) se relaciona com arquivos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Proteção de Informações do Windows (WIP)
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, proteção de informações do Windows, dados corporativos, proteção de dados corporativos, edp, aplicativos habilitados
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.openlocfilehash: f624d20d33f560f151b40bd1a405711d697fd4cb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.locfileid: "199362"
---
# <a name="windows-information-protection-wip"></a>Proteção de Informações do Windows (WIP)

__Observação__ A política Proteção de Informações do Windows (WIP) pode ser aplicada ao Windows 10, versão 1607.

A WIP protege os dados que pertencem a uma organização aplicando políticas que são definidas pela organização. Se seu aplicativo estiver incluído nessas políticas, todos os dados produzidos por seu aplicativo estarão sujeitos às restrições de política. Este tópico ajuda você a criar aplicativos que impõem essas políticas de forma mais branda sem afetar os dados pessoais do usuário.
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>Primeiro, o que é WIP?

WIP é um conjunto de recursos em desktops, notebooks, tablets e telefones que dão suporte ao sistema Gerenciamento de Dispositivos Móveis (MDM) e Gerenciamento de Aplicativos Móveis (MAM) da organização.

A WIP, juntamente com o MDM, oferece à organização maior controle sobre como os dados são tratados em dispositivos gerenciados pela organização. Às vezes, os usuários trazem dispositivos para trabalhar e não os registram no MDM da organização  Nesses casos, as organizações podem usar o MAM para ter maior controle sobre como seus dados são manipulados em aplicativos de linha de negócios específicos que os usuários instalam em seus dispositivos.

Usando o MDM ou MAM, os administradores podem identificar quais aplicativos têm permissão para acessar os arquivos que pertencem à organização e se os usuários podem copiar dados dos arquivos e, em seguida, colá-los em seus documentos pessoais.

Veja como funciona. Os usuários registram seus dispositivos no sistema de gerenciamento de dispositivos móveis (MDM) da organização. O administrador da organização de gerenciamento usa o Microsoft Intune ou o System Center Configuration Manager (SCCM) para definir e, em seguida, implantar uma política nos dispositivos registrados.

Se os usuários não forem obrigados a registrar seus dispositivos, os administradores usarão o sistema MAM para definir e implantar uma política que se aplique a aplicativos específicos. Quando os usuários instalarem qualquer um desses aplicativos, eles receberão a política associada.

Essa política identifica os aplicativos que podem acessar dados corporativos (denominados *lista de permissão* da política). Esses aplicativos podem acessar arquivos empresariais protegidos, redes virtuais privadas (VPN) e dados corporativos na área de transferência ou por meio de um contrato de compartilhamento. A política também define as regras que controlam os dados. Por exemplo, se os dados podem ser copiados dos arquivos de propriedade da empresa e depois colados em arquivos que não pertencem à empresa.

Se os usuários cancelarem o registro de seus dispositivos no sistema MDM da organização ou desinstalarem os aplicativos identificados pelo sistema MAM da organização, os administradores poderão apagar remotamente os dados corporativos do dispositivo.

![Ciclo de vida do Wip](images/wip-lifecycle.png)

> **Leia mais sobre WIP** <br>
* [Apresentando a Proteção de Informações do Windows](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [Proteger os dados corporativos usando a WIP (Proteção de Informações do Windows)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

Se seu aplicativo estiver na lista de permissões, todos os dados produzidos por seu aplicativo estarão sujeitos às restrições de política. Isso significa que, se os administradores revogarem o acesso do usuário aos dados corporativos, esses usuários perderão acesso a todos os dados produzidos por seu aplicativo.

Isso será bom se seu aplicativo se destina apenas a uso empresarial. Mas se seu aplicativo cria dados que os usuários consideram pessoais, é possível *capacitar* seu aplicativo para distinguir, de forma inteligente, entre dados pessoais e corporativos. Chamamos esse tipo de aplicativo *habilitado para empresas* porque ele pode impor a política corporativa, mas preservar a integridade dos dados pessoais do usuário.

## <a name="create-an-enterprise-enlightened-app"></a>Criar um aplicativo habilitado para empresas

Use APIs do WIP para habilitar seu aplicativo e, em seguida, declará-lo como habilitado para empresas.

Capacite seu aplicativo se ele for usado para fins organizacionais e pessoais.

Capacite seu aplicativo se você quiser lidar normalmente com a imposição dos elementos da política.

Por exemplo, se a política permitir que os usuários colem dados corporativos em um documento pessoal, você poderá impedir que os usuários precisem responder a uma caixa de diálogo de consentimento antes de colarem os dados. Da mesma forma, você pode apresentar caixas de diálogo informativas personalizadas em resposta a esses tipos de eventos.

Se você estiver pronto para capacitar seu aplicativo, consulte um destes procedimentos:

**Para aplicativos da Plataforma Universal do Windows (UWP) criados com C#**

[Guia do desenvolvedor de Proteção de Informações do Windows (WIP)](wip-dev-guide.md).

**Para aplicativos da área de trabalho que você criar usando C++**

[Guia do desenvolvedor de Proteção de Informações do Windows (WIP) (C++)](http://go.microsoft.com/fwlink/?LinkId=822192).


## <a name="create-non-enlightened-enterprise-app"></a>Criar um aplicativo corporativo não habilitado

Se você estiver criando um aplicativo de linha de negócios (LOB) não destinado ao uso pessoal, talvez não seja preciso habilitá-lo.

### <a name="windows-desktop-apps"></a>Aplicativos da área de trabalho do Windows
Você não precisa habilitar um aplicativo da área de trabalho do Windows, mas deve testá-lo para garantir que ele funcione corretamente sob a política. Por exemplo, inicie seu aplicativo, use-o e cancele o registro do dispositivo no MDM. Em seguida, verifique se o aplicativo pode ser iniciado novamente. Se os arquivos essenciais para o funcionamento do aplicativo estiverem criptografados, o aplicativo poderá não ser iniciado. Além disso, revise os arquivos com os quais seu aplicativo interage para garantir que ele não criptografe inadvertidamente arquivos pessoais para o usuário. Isso pode incluir arquivos de metadados, imagens e outras coisas.

Depois de testar seu aplicativo, adicione esse sinalizador ao arquivo de recursos ou ao seu projeto e recompile o aplicativo.

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
Embora as políticas de MDM não exijam o sinalizador, as políticas de MAM exigem.

### <a name="uwp-apps"></a>Aplicativos UWP

Se você espera que seu aplicativo seja incluído em uma política de MAM, habilite-o. As políticas implantadas em dispositivos sob o MDM não exigirão isso, mas se você distribuir seu aplicativo para consumidores organizacionais, será difícil, se não impossível, determinar qual tipo de sistema de gerenciamento de política eles usarão. Para garantir que seu aplicativo funcione nos dois tipos de sistemas de gerenciamento de políticas (MDM e MAM), você deve habilitar seu aplicativo.






 
