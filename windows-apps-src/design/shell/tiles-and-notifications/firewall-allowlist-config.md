---
author: mijacobs
Description: Muitas empresas usam firewalls para bloquear o tráfego indesejado. Este documento descreve como permitir que o tráfego WNS passe por firewalls.
title: Adicionando o tráfego WNS àlist de permissão do firewall
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
keywords: Windows 10, UWP, WNS, serviço de notificações do Windows, notificação, Windows, firewall, solução de problemas, IP, tráfego, Enterprise, rede, IPv4, VIP, FQDN, endereço IP público
ms.localizationpriority: medium
ms.openlocfilehash: 817dec616a88f0b374bc019cbbf23f56fad94484
ms.sourcegitcommit: 820704a73174e81da3dc00cf0603c0c82471cdfe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69908459"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>Configurações de proxy e firewall corporativo para dar suporte ao tráfego WNS

## <a name="background"></a>Informações preliminares
Muitas empresas usam firewalls para bloquear o tráfego de rede indesejado; Infelizmente, isso também pode bloquear coisas importantes, como comunicações do serviço de notificação do Windows. Isso significa que todas as notificações enviadas pelo WNS serão descartadas em determinadas configurações de rede. Para evitar isso, os administradores de rede podem adicionar a lista de canais WNS aprovados à lista de isenção para permitir que o tráfego WNS passe pelo firewall. Abaixo estão mais detalhes sobre como e o que adicionar, bem como suporte para tipos de proxy diferentes.

## <a name="proxy-support"></a>Suporte a proxy

> [!Note] 
Os clientes do Windows **não** dão suporte a todos os proxies, a conexão com o WNS deve ser uma conexão direta.

**Em breve!** Estamos investigando ativamente diferentes configurações de rede, proxies e firewalls. Atualizaremos esta página com mais detalhes sobre cenários empresariais comuns e suporte a WNS em breve.


## <a name="what-information-should-be-added-to-the-allowlist"></a>Quais informações devem ser adicionadas àlist de permissões
Abaixo está uma lista que contém os FQDNs, VIPs e os intervalos de endereços IP usados pelo serviço de notificação do Windows. 

> [!IMPORTANT]
> É altamente recomendável que você permita a lista pelo FQDN, pois eles não serão alterados. Se você permitir listar por FQDN, não será necessário também permitir os intervalos de endereços IP.

> [!IMPORTANT]
> Os intervalos de endereços IP serão alterados periodicamente; por isso, eles não estão incluídos nesta página. Se você quiser ver a lista de intervalos de IP, você pode baixar o arquivo do centro de download: Os [intervalos de IP e VIP do Windows Notification Service (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Verifique regularmente para certificar-se de que você tem as informações mais atualizadas. 


### <a name="fqdns-vips-and-ips"></a>FQDNs, VIPs e IPs
Cada um dos elementos no documento XML a seguir é explicado na tabela que o segue (em [termos e notações](#terms-and-notations). Os intervalos de IP foram intencionalmente deixados para fora deste documento para incentivar você a usar apenas os FQDNs, pois os FQDNs permanecerão constantes. No entanto, você pode baixar o arquivo XML que contém a lista completa no centro de download: Os [intervalos de IP e VIP do Windows Notification Service (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Novos VIPs ou intervalos de IP entrarão em **vigor uma semana após serem carregados**.

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

### <a name="terms-and-notations"></a>Termos e notações
Veja abaixo as explicações sobre as notações e os elementos usados no trecho de código XML acima.

| Termo | Explicação |
|---|---|
| **Notação de ponto decimal (ou seja, 64.4.28.0/26)** | Notação de ponto decimal é uma maneira de descrever o intervalo de endereços IP. Por exemplo, 64.4.28.0/26 significa que os primeiros 26 bits de 64.4.28.0 são constantes, enquanto os últimos 6 bits são variáveis.  Nesse caso, o intervalo de IPv4 é 64.4.28.0-64.4.28.63. |
| **ClientDNS** | Esses são os filtros de FQDN (nome de domínio totalmente qualificado) para os dispositivos cliente (computadores Windows, desktops) que recebem notificações do WNS. Eles devem ser permitidos por meio do firewall para que os clientes WNS usem a funcionalidade WNS.  É recomendável permitir-List pelos FQDNs em vez dos intervalos de IP/VIP, pois eles nunca serão alterados. |
| **ClientIPsIPv4** | Esses são os endereços IPv4 dos servidores acessados por dispositivos cliente (computadores Windows, desktops) que recebem notificações do WNS. |
| **CloudServiceDNS** | Esses são os filtros de FQDN (nome de domínio totalmente qualificado) para os servidores WNS aos quais seu serviço de nuvem se comunicará para enviar notificatios para WNS. Eles devem ser permitidos por meio do firewall para que os serviços enviem notificações WNS.  É recomendável permitir-List pelos FQDNs em vez dos intervalos de IP/VIP, pois eles nunca serão alterados.|
| **CloudServiceIPs** | CloudServiceIPs são os endereços IPv4 dos servidores usados para que os serviços de nuvem enviem notificações para o WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Intervalos de IP público do MPNS (serviço de notificações por push da Microsoft)
Se você estiver usando o serviço de notificação herdado, o MPNS, os intervalos de endereços IP que você precisará adicionar à lista de permissões estão disponíveis no centro de download: [Intervalos de IP público do MPNS (serviço de notificações por push da Microsoft)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Tópicos relacionados

* [Início Rápido: Enviando uma notificação por push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Como solicitar, criar e salvar um canal de notificação](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Como interceptar notificações para executar aplicativos](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Como autenticar com o serviço de notificação por push do Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Cabeçalhos de solicitação e resposta do serviço de notificação por push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Diretrizes e lista de verificação para notificações por push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
