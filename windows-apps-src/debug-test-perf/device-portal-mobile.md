---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal para celulares
description: Saiba como o Windows Device Portal permite que você configure e gerencie seu dispositivo móvel remotamente.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 0531cbefef509f7bc323829031b366bec3c798d8
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3663007"
---
# <a name="device-portal-for-mobile"></a>Device Portal para celulares

A partir do Windows 10, versão 1511, outros recursos de desenvolvedor estão disponíveis para a família de dispositivos móveis. Esses recursos estão disponíveis apenas quando o Modo de desenvolvedor está habilitado no dispositivo.

Para saber mais sobre como habilitar o Modo de desenvolvedor, consulte [Habilitar seu dispositivo para desenvolvimento](../get-started/enable-your-device-for-development.md).

![Configurações do Device Portal](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>Configurar o Device Portal no Windows Phone

### <a name="turn-on-device-discovery-and-pairing"></a>Ativar a descoberta e o emparelhamento de dispositivos

Para se conectar ao Device Portal, você deve habilitar a Descoberta de dispositivo e o Device Portal nas configurações do seu telefone. Isso permite emparelhar seu telefone com um computador ou outro dispositivo Windows 10. Ambos os dispositivos devem estar conectados à mesma sub-rede da rede por uma conexão com ou sem fio, ou eles devem estar conectados por USB.

Na primeira vez que você se conectar ao Device Portal, será solicitado a fornecer um código de segurança de 6 caracteres que diferencia maiúsculas de minúsculas. Isso garante que você tenha acesso ao telefone e o mantém protegido contra invasores. Pressione o botão Emparelhar no seu telefone para gerar e exibir o código. Em seguida, insira os 6 caracteres na caixa de texto no navegador.

![Configurações da descoberta de dispositivos do Modo de desenvolvedor](images/device-portal/mob-dev-mode-pairing.png)

Você pode escolher entre 3 maneiras de se conectar ao Device Portal: USB, host local e pela rede local (incluindo VPN e compartilhamento).

**Para se conectar ao Device Portal**

1. Em seu navegador, insira o endereço mostrado aqui para o tipo de conexão que você está usando.

    - USB: `http://127.0.0.1:10080`

    Use esse endereço quando o telefone estiver conectado a um computador por meio de uma conexão USB. Ambos os dispositivos devem ter o Windows 10, versão 1511 ou posterior.
    
    - Localhost: `http://127.0.0.1`

    Use esse endereço para exibir o Device Portal localmente no telefone no Microsoft Edge para Windows 10 Mobile.
    
    - Rede local: `https://<The IP address or hostname of the phone>`

    Use esse endereço para se conectar por uma rede local.

    O endereço IP do telefone é mostrado nas configurações do Device Portal no telefone. O HTTPS é necessário para autenticação e comunicação segura. O nome do host (editável em Configurações > Sistema > Sobre) também pode ser usado para acessar o Portal de Dispositivos na rede local (por exemplo, http://Phone360), o que é útil para dispositivos que possam alterar redes ou endereços IP com frequência, ou que precisem ser compartilhados. 

2. Pressione o botão Emparelhar no seu telefone para gerar e exibir o código de segurança necessário.

3. Insira o código de segurança de 6 caracteres na caixa de senha do Device Portal no navegador.

4. (Opcional) Marque a caixa Remember my computer no seu navegador para lembrar esse emparelhamento no futuro.

Aqui está a seção do Device Portal da página de configurações do desenvolvedor no Windows Phone.

![Configurações do Device Portal](images/device-portal/mob-dev-mode-portal.png)

Se você estiver usando o Device Portal em um ambiente protegido, como um laboratório de teste, onde confia em todos na rede local, não tem informações pessoais no dispositivo e tem requisitos exclusivos, você poderá desabilitar a autenticação. Isso permite a comunicação não criptografada e que qualquer pessoa com o endereço IP do seu telefone controle-o.

## <a name="tool-notes"></a>Notas da ferramenta

## <a name="device-portal-pages"></a>Páginas do Device Portal
### <a name="processes"></a>Processos

A capacidade de encerrar processos arbitrários não está incluída no Windows Mobile Device Portal. 

O Device Portal em dispositivos móveis fornece o conjunto padrão de páginas. Para obter descrições detalhadas, consulte [Visão geral do Windows Device Portal](device-portal.md).

- Gerenciador de aplicativos
- Explorador de arquivos de aplicativo (explorador de armazenamento isolado)
- Processos
- Gráficos de desempenho
- Rastreamento de Eventos para Windows (ETW)
- Rastreamento de desempenho (WPR) 
- Dispositivos
- Rede

## <a name="see-also"></a>Veja também

* [Visão geral do Portal de Dispositivos do Windows](device-portal.md)
* [Referência de API central do Portal de Dispositivos](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)