---
title: Dispositivos perdidos
description: Um dispositivo Direct3D pode estar em um estado operacional ou perdido.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Dispositivos perdidos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f0b42a10c2cdd61aef84e08d6bd4f6408a978c3
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8754145"
---
# <a name="lost-devices"></a>Dispositivos perdidos


Um dispositivo Direct3D pode estar em um estado operacional ou um estado perdido. O estado *operacional* é o estado normal do dispositivo no qual o dispositivo é executado e apresenta toda a renderização conforme o esperado. O dispositivo faz uma transição para o estado *perdido* quando um evento, como a perda de foco do teclado em um app de tela inteira, impossibilita a renderização. O estado perdido é caracterizado pela falha silenciosa de todas as operações de renderização, ou seja, os métodos de renderização podem retornar códigos de sucesso mesmo em caso de falha das operações de renderização.

Por padrão, o conjunto completo de cenários que pode provocar a perda do dispositivo não é especificado. Alguns exemplos típicos incluem perda do foco, como quando o usuário pressionar ALT + TAB ou quando uma caixa de diálogo do sistema é inicializada. Os dispositivos também podem ser perdidos devido a um evento de gerenciamento de energia ou quando outro app assume a operação de tela inteira. Além disso, qualquer falha de redefinição de dispositivo o coloca em um estado perdido.

Todos os métodos derivados de [**IDesconhecido**](https://msdn.microsoft.com/library/windows/desktop/ms680509) têm garantia de funcionar depois que um dispositivo é perdido. Após a perda de dispositivo, cada função geralmente tem três opções:

-   Falhar com um erro "dispositivo perdido": isso significa que o app precisa reconhecer que o dispositivo foi perdido para que o app identifique que algo não está funcionando como o esperado.
-   Falhar de modo silencioso, retornando S\_OK ou qualquer outro código de retorno: se uma função falhar silenciosamente, em geral, o app não consegue diferenciar entre o resultado de "sucesso" e "falha silenciosa".
-   Retornar um código de retorno.

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Responder a um dispositivo perdido


Um dispositivo perdido deve recriar recursos (incluindo os recursos de memória de vídeo) depois que for redefinido. Se um dispositivo for perdido, o app consulta o dispositivo para verificar se é possível restaurar o estado operacional. Caso contrário, o app aguarda até que o dispositivo poder ser restaurado.

Se for possível restaurar o dispositivo, o app o prepara ao destruir todos os recursos de memória de vídeo e qualquer cadeias de permuta. A redefinição é o único procedimento com efeito quando um dispositivo é perdido, além de ser a única maneira pela qual um app pode alterar o estado do dispositivo de perdido para operacional. A restauração falha exceto quando o app libera todos os recursos alocados, incluindo destinos de renderização e superfícies de estêncil de profundidade.

Na maioria das vezes, as chamadas de alta frequência do Direct3D não retornam informações sobre a perda do dispositivo. O app pode continuar a chamar métodos de renderização sem receber a notificação de um dispositivo perdido. Internamente, essas operações serão descartadas depois que o dispositivo for redefinido para o estado operacional.

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Operações de bloqueio


Internamente, o Direct3D trabalha o suficiente para garantir que uma operação de bloqueio será bem-sucedida depois que um dispositivo for perdido. No entanto, não é garantido que os dados do recurso de memória de vídeo serão precisos durante a operação de bloqueio. É garantido que nenhum código de erro será retornado. Isso permite que os apps sejam gravados sem se preocupar com a perda de dispositivo durante uma operação de bloqueio.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Recursos


Os recursos podem consumir a memória de vídeo. Como um dispositivo perdido está desconectado da memória de vídeo do adaptador, não é possível garantir a alocação da memória de vídeo quando o dispositivo for perdido. Como resultado, todos os métodos de criação de recursos são implementados para funcionarem, mas na verdade alocam somente a memória fictícia do sistema. Como qualquer recurso de memória de vídeo deve ser destruído antes de redimensionar o dispositivo, não há nenhum problema em sobrealocar a memória de vídeo. Essas superfícies fictícias permitem o funcionamento normal das operações de bloqueio e cópia até o app detectar que o dispositivo foi perdido.

Toda a memória de vídeo deve ser liberada antes que um dispositivo possa ser restaurado de um estado perdido para operacional. Outros dados de estado são destruídos automaticamente pela transição para um estado operacional.

É recomendado desenvolver apps com um único caminho de código para responder à perda de dispositivo. Esse caminho de código é provavelmente semelhante, ou idêntico, ao caminho de código necessário para inicializar o dispositivo durante a inicialização.

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Dados recuperados


O Direct3D permite que os apps validem a textura e os estados de renderização em comparação a uma única passagem de renderização pelo hardware.

O Direct3D também permite que apps copiem imagens geradas ou gravadas anteriormente de recursos de memória de vídeo para recursos de memória não volátil do sistema. Como as imagens de origem dessas transferências podem ser perdidas a qualquer momento, o Direct3D permite que essas operações de cópia falhem quando o dispositivo for perdido.

As operações de cópia podem falhar, pois não há nenhuma superfície principal quando o dispositivo for perdido. A criação de cadeias de permuta também pode falhar, pois um buffer de fundo não pode ser criado quando o dispositivo for perdido.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Dispositivos](devices.md)

 

 




