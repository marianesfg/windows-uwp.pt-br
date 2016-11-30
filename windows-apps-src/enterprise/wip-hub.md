---
author: normesta
Description: "Este é um tópico central que abrange toda a imagem do desenvolvedor de como a Proteção de Informações do Windows (WIP) se relaciona com arquivos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio."
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Proteção de Informações do Windows (WIP)"
translationtype: Human Translation
ms.sourcegitcommit: 724d9edf67d0f73ceb3eb2ac323e0a0f42f2dd0d
ms.openlocfilehash: f9cfa8d1d7ea4e78208a4fb3fc853884a13a676c

---

# Proteção de Informações do Windows (WIP)

__Observação__ A política Proteção de Informações do Windows (WIP) pode ser aplicada ao Windows 10, versão 1607.

A WIP protege os dados que pertencem a uma organização aplicando políticas que são definidas pela organização. Se seu aplicativo estiver incluído nessas políticas, todos os dados produzidos por seu aplicativo estarão sujeitos às restrições de política. Este tópico ajuda você a criar aplicativos que impõem essas políticas de forma mais branda sem afetar os dados pessoais do usuário.

## Primeiro, o que é WIP?

WIP é um conjunto de recursos em desktops, notebooks, tablets e telefones que dão suporte ao sistema Gerenciamento de Dispositivos Móveis (MDM) da organização. A WIP oferece à organização maior controle sobre como os dados são tratados em dispositivos gerenciados pela organização. Por exemplo, os administradores podem identificar quais aplicativos têm permissão para acessar os arquivos que pertencem à organização e se os usuários podem copiar dados dos arquivos e, em seguida, colá-los em seus documentos pessoais.

Veja como funciona. Os usuários registram seus dispositivos no sistema de gerenciamento de dispositivos móveis (MDM) da organização. O administrador da organização de gerenciamento usa o Microsoft Intune ou o System Center Configuration Manager (SCCM) para definir e, em seguida, implantar uma política nos dispositivos registrados.

Essa política identifica os aplicativos que podem acessar dados corporativos (denominados *lista de permissão* da política). Esses aplicativos podem acessar arquivos empresariais protegidos, redes virtuais privadas (VPN) e dados corporativos na área de transferência ou por meio de um contrato de compartilhamento. A política também define as regras que controlam os dados. Por exemplo, se os dados podem ser copiados dos arquivos de propriedade da empresa e depois colados em arquivos que não pertencem à empresa.

Se os usuários cancelarem o registro de seus dispositivos do sistema MDM da organização, os administradores poderão limpar remotamente os dados corporativos do dispositivo.

![Ciclo de vida do Wip](images/wip-lifecycle.png)

> **Leia mais sobre WIP** <br>
* [Apresentando a Proteção de Informações do Windows](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [Proteger os dados corporativos usando a WIP (Proteção de Informações do Windows)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

Se seu aplicativo estiver na lista de permissões, todos os dados produzidos por seu aplicativo estarão sujeitos às restrições de política. Isso significa que, se os administradores revogarem o acesso do usuário aos dados corporativos, esses usuários perderão acesso a todos os dados produzidos por seu aplicativo.

Isso será bom se seu aplicativo se destina apenas a uso empresarial. Mas se seu aplicativo cria dados que os usuários consideram pessoais, é possível *capacitar* seu aplicativo para distinguir, de forma inteligente, entre dados pessoais e corporativos. Chamamos esse tipo de aplicativo *habilitado para empresas* porque ele pode impor a política corporativa, mas preservar a integridade dos dados pessoais do usuário.

## Criar um aplicativo habilitado para empresas

Use APIs do WIP para habilitar seu aplicativo e, em seguida, declará-lo como habilitado para empresas.

Capacite seu aplicativo se ele for usado para fins organizacionais e pessoais.

Capacite seu aplicativo se você quiser lidar normalmente com a imposição dos elementos da política.

Por exemplo, se a política permitir que os usuários colem dados corporativos em um documento pessoal, você poderá impedir que os usuários precisem responder a uma caixa de diálogo de consentimento antes de colarem os dados. Da mesma forma, você pode apresentar caixas de diálogo informativas personalizadas em resposta a esses tipos de eventos.

Se você estiver pronto para capacitar seu aplicativo, consulte um destes procedimentos:

**Para aplicativos da Plataforma Universal do Windows (UWP) criados com C#**

[Crie um aplicativo capacitado que consuma dados corporativos e pessoais](wip-dev-guide.md).

**Para aplicativos de desktop que você criar usando C++**

[Crie um aplicativo capacitado que consuma dados corporativos e pessoais (C++)](http://go.microsoft.com/fwlink/?LinkId=822192).

Entre outras coisas, os aplicativos empresariais capacitados compartilham estas qualidades:

* Eles mantêm dados corporativos protegidos, estejam eles em repouso, em uso ou em trânsito.
* Eles reconhecem dados pessoais e impedem que eles sejam submetidos a restrições de política.
* Reconhecem dados corporativos e os protegem quando chegam ao aplicativo.
* Eles protegem os dados corporativos que saem do aplicativo.

  Por exemplo, eles impedem que os dados sejam enviados para uma rede não empresarial, encapsulam os dados em um formato criptografado portátil antes de permitir a sua transferência móvel e possivelmente (dependendo das configurações da política) enviam um prompt para o usuário antes de ele colar dados empresariais em um aplicativo que não esteja na lista de permissões.







 



<!--HONumber=Nov16_HO1-->


