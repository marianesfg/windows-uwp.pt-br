---
author: mijacobs
Description: Muitas empresas usam firewalls para bloquear o tráfego indesejado. Este documento descreve como permitir o tráfego WNS para passar por firewalls.
title: Adicionando o tráfego WNS para a lista de permissões do Firewall
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: o Windows 10, uwp, WNS, o serviço de notificações do windows, notificação, windows, firewall, solução de problemas, IP, o tráfego, enterprise, rede, IPv4, o VIP, o FQDN, o endereço IP público
ms.localizationpriority: medium
ms.openlocfilehash: 9ed4ad6ed828abda9d487ef96beca9b655c92421
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366674"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>Permitindo o tráfego de notificação do Windows por meio de firewalls corporativos

## <a name="background"></a>Histórico
Muitas empresas usam firewalls para bloquear o tráfego de rede indesejado; Infelizmente, isso também pode bloquear coisas importantes, como comunicações do serviço de notificação do Windows. Isso significa que todas as notificações enviadas por meio de WNS serão removidas. Para evitar isso, os administradores de rede podem adicionar a lista de canais WNS aprovados à sua lista de isenção para permitir o tráfego WNS passar pelo firewall. Abaixo estão mais detalhes sobre como e o que adicionar. 


## <a name="what-information-should-be-added-to-the-allowlist"></a>Quais informações devem ser adicionadas para a lista de permissões
Abaixo está uma lista que contém os FQDNs, VIPs e IP intervalos de endereços usados pelo serviço de notificação do Windows. 

> [!IMPORTANT]
> Sugerimos que você permita a lista pelo FQDN, porque eles não mudarão. Se você permitir que a lista pelo FQDN, você não precisará também permitem que os intervalos de endereços IP.

> [!IMPORTANT]
> Os intervalos de endereços IP serão alterado periodicamente; Por isso, eles não estão incluídos nesta página. Se você quiser ver a lista de intervalos de IP, você pode baixar o arquivo do Centro de Download: [Intervalos de serviço de notificação do Windows (WNS) VIP e IP](https://www.microsoft.com/download/details.aspx?id=44238). Verifique regularmente para garantir que as informações mais atualizadas. 


### <a name="fqdns-vips-and-ips"></a>FQDNs, VIPs e IPs
Cada um dos elementos no documento XML a seguir é explicada na tabela que vem a seguir (em [termos e as notações](#terms-and-notations). Os intervalos de IP foram intencionalmente tornados fora deste documento encorajá-lo a usar somente os FQDNs conforme os FQDNs permanecerá constantes. No entanto, você pode baixar o arquivo XML que contém a lista completa do Centro de Download: [Intervalos de serviço de notificação do Windows (WNS) VIP e IP](https://www.microsoft.com/download/details.aspx?id=44238). Novo VIPs ou intervalos de IP será ser **efetivo uma semana após serem carregados**.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>Termos e as notações
Abaixo estão as explicações sobre as notações e os elementos usados no trecho de XML acima.

| Termo | Explicação |
|---|---|
| **Notação de ponto decimal (ou seja, 64.4.28.0/26)** | Notação de ponto decimal é uma maneira de descrever o intervalo de endereços IP. Por exemplo, 64.4.28.0/26 significa que os primeiros bits de 26 de 64.4.28.0 são constantes, enquanto os últimos 6 bits são variáveis.  Nesse caso, o intervalo de IPv4 é 64.4.28.0 - 64.4.28.63. |
| **ClientDNS** | Esses são os filtros de nome de domínio totalmente qualificado (FQDN) para os dispositivos de cliente (PCs com Windows, áreas de trabalho) receber notificações do WNS. Eles devem ser permitidos através do firewall para clientes WNS usar a funcionalidade de WNS.  Recomenda-se à lista de permissões por FQDNs em vez dos intervalos IP/VIP, já que eles nunca serão alterado. |
| **ClientIPsIPv4** | Esses são os endereços IPv4 dos servidores acessados por dispositivos de cliente (PCs com Windows, áreas de trabalho) receber notificações do WNS. |
| **CloudServiceDNS** | Esses são os filtros de nome de domínio totalmente qualificado (FQDN) para os servidores WNS que seu serviço de nuvem se comunicará para enviar notificações por WNS. Eles devem ser permitidos através do firewall para que os serviços para enviar notificações do WNS.  Recomenda-se à lista de permissões por FQDNs em vez dos intervalos IP/VIP, já que eles nunca serão alterado.|
| **CloudServiceIPs** | CloudServiceIPs são os endereços IPv4 dos servidores usados para serviços de nuvem para enviar notificações para WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Intervalos IP públicos do serviço de notificações por Push da Microsoft (MPNS)
Se você estiver usando o serviço de notificação herdados, MPNS, intervalos de endereços IP que você precisará adicionar à lista de permissões estão disponíveis no Centro de Download: [Intervalos de IP público do serviço de notificações por Push da Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Tópicos relacionados

* [Guia de início rápido: Enviar uma notificação por push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Como solicitar, criar e salvar um canal de notificação](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Como interceptar as notificações para aplicativos em execução](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Como autenticar com o serviço de notificação por Push o Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Cabeçalhos de solicitação e resposta do serviço de notificação de push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Diretrizes e lista de verificação para notificações por push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
