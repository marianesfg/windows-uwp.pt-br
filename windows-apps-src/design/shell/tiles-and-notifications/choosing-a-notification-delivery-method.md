---
author: mijacobs
Description: This article covers the four notification options&\#8212;local, scheduled, periodic, and push&\#8212;that deliver tile and badge updates and toast notification content.
title: Escolher um método de entrega de notificação
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: Choose a notification delivery method
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c01b96f70bd39c43f321935aa47393ada0400319
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7433176"
---
# <a name="choose-a-notification-delivery-method"></a>Escolher um método de entrega de notificação

 


Este artigo aborda as quatro opções de notificação - local, agendada, periódica e por push - que fornecem atualizações de blocos e notificação e conteúdo de notificações do sistema. Um bloco ou uma notificação do sistema pode obter informações para o usuário mesmo enquanto o usuário não está diretamente envolvido com o aplicativo. A natureza e o conteúdo do aplicativo e as informações que você deseja fornecer podem ajudá-lo a determinar qual método de notificação é melhor para o seu cenário.

## <a name="notification-delivery-methods-overview"></a>Visão geral de métodos de entrega de notificações


Existem quatro mecanismos que um aplicativo pode usar para entregar uma notificação:

-   **Local**
-   **Agendado**
-   **Periódico**
-   **Por push**

Esta tabela resume os tipos de entrega de notificação.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método de entrega</th>
<th align="left">Usar com</th>
<th align="left">Descrição</th>
<th align="left">Exemplos</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Local</td>
<td align="left">Bloco, Emblema, Caixa de Informações</td>
<td align="left">Conjunto de chamadas de API que envia notificações enquanto o aplicativo está sendo executado, atualizando diretamente o bloco ou o emblema ou enviando uma notificação do sistema.</td>
<td align="left"><ul>
<li>Um aplicativo de música atualiza seu bloco para mostrar o que está &quot;Em Execução&quot;.</li>
<li>Um aplicativo de jogos atualiza o seu bloco com o recorde de pontuação do usuário quando o usuário deixa o jogo.</li>
<li>Um emblema cujo glifo indica que há novas informações no aplicativo é limpo quando o aplicativo é ativado.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Agendado</td>
<td align="left">Bloco, Emblema</td>
<td align="left">Um conjunto de chamadas de API que agenda uma notificação com antecedência para atualizar na hora que você especificou.</td>
<td align="left"><ul>
<li>Um aplicativo de calendário configura um lembrete de notificação do sistema para uma futura reunião.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Periódico</td>
<td align="left">Bloco, Emblema</td>
<td align="left">As notificações que atualizam blocos e emblemas regularmente em um intervalo de tempo fixo sondando um serviço na nuvem em busca de novo conteúdo.</td>
<td align="left"><ul>
<li>Um aplicativo de condições climáticas atualiza seu bloco, que mostra a previsão em intervalos de 30 minutos.</li>
<li>Um site de &quot;promoções do dia&quot; atualiza a oferta do dia a cada manhã.</li>
<li>Um bloco que exibe os dias até que um evento atualize a contagem regressiva exibida de cada dia à meia-noite.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Por push</td>
<td align="left">Bloco, Notificação, Notificação do Sistema, Notificação Bruta</td>
<td align="left">As notificações são enviadas de um servidor em nuvem, mesmo se o aplicativo não estiver em execução.</td>
<td align="left"><ul>
<li>Um aplicativo de compras envia uma notificação do sistema para permitir que um usuário saiba sobre uma promoção de um item que eles estão acompanhando.</li>
<li>Um aplicativo de notícias envia seu bloco com as últimas notícias assim que elas ocorrem.</li>
<li>Um aplicativo de esportes mantém seu bloco atualizado durante um jogo em andamento.</li>
<li>Um aplicativo de comunicações fornece alertas sobre as mensagens ou telefonemas de entrada.</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <a name="local-notifications"></a>Notificações locais


Atualizar o bloco ou emblema do aplicativo ou acionar uma notificação do sistema enquanto o aplicativo está sendo executado é o mecanismo mais simples de entrega de notificações. Todo aplicativo possui informações úteis ou interessantes exibidas no bloco, mesmo se esse conteúdo mudar depois que o usuário ativou e interagiu com o aplicativo. As notificações locais são uma boa maneira de manter o bloco do aplicativo atualizado, mesmo se você também usar um dos outros mecanismos de notificação. Por exemplo, um bloco de aplicativo de fotos poderia mostrar as fotos de um álbum adicionado recentemente.

Recomendamos que seu aplicativo atualize o respectivo bloco localmente na primeira inicialização ou, pelo menos, logo depois que o usuário faz uma alteração que seu aplicativo geralmente refletirá no bloco. Essa atualização não é vista até que o usuário saia do aplicativo, porém ao fazer a alteração enquanto o aplicativo está sendo usado já garante que o bloco seja atualizado quando o usuário sair dele.

Embora as chamadas de API sejam locais, as notificações podem fazer referência a imagens da Web. Se a imagem da Web não estiver disponível para transferência, estiver corrompida ou não atender às especificações da imagem, os blocos e a notificação do sistema responderão de maneira diferente:

-   Blocos: as atualizações não são mostradas
-   Notificação do sistema: a notificação é exibida, mas sua imagem será removida

Por padrão, as notificações do sistema local expiram em três dias e as notificações de bloco local nunca expiram. Recomendamos substituir esses padrões por um prazo de expiração explícito que seja compatível com suas notificações (as notificações do sistema têm um prazo máximo de três dias). 

Para saber mais, consulte estes tópicos:

-   [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md)
-   [Enviar uma notificação do sistema local](send-local-toast.md)
-   [Exemplos de códigos de notificações da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="scheduled-notifications"></a>Notificações agendadas


As notificações agendadas estão no subconjunto de notificações locais que pode especificar a hora precisa em que um bloco deve ser atualizado ou em que uma notificação do sistema deve ser mostrada. Notificações agendadas são ideais em situações onde o conteúdo a ser atualizado já é conhecido, como o convite de uma reunião. Se você não tiver conhecimento antecipado do conteúdo da notificação, deverá usar uma notificação periódica ou por push.

Observe que as notificações agendadas não podem ser usadas para as notificações de selo; as notificações de selo são melhor veiculadas pelas notificações locais, periódicas ou por push.

Por padrão, as notificações agendadas expiram três dias depois que são entregues. Você pode substituir esse prazo de expiração padrão em notificações de bloco agendada, mas não pode substituir o prazo de expiração em notificações do sistema agendadas.

Para saber mais, consulte estes tópicos:

-   [Exemplos de códigos de notificações da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="periodic-notifications"></a>Notificações periódicas


As notificações periódicas fornecem atualizações de bloco com um mínimo serviço na nuvem e investimento do cliente. Eles também são um método excelente para distribuir o mesmo conteúdo para um amplo público-alvo. O código do lado do cliente especifica a URL de um local na nuvem que o Windows sonda para atualizações de bloco ou emblema e especifica a frequência de sondagem do local. Em cada intervalo de sondagem, o Windows contata a URL para baixar o conteúdo XML especificado e exibi-lo no bloco.

As notificações periódicas exigem que o aplicativo hospede um serviço na nuvem e este serviço será sondado no intervalo especificado por todos os usuários que têm o aplicativo instalado. Observe que as atualizações periódicas não podem ser usadas para as notificações do sistema; as notificações do sistema são melhor atendidas pelas notificações agendadas ou por push.

Por padrão, as notificações periódicas expiram três dias depois que a sondagem ocorre. Se necessário, você pode substituir esse padrão por um prazo de expiração explícito.

Para saber mais, consulte estes tópicos:

-   [Visão geral de notificações periódicas](periodic-notification-overview.md)
-   [Exemplos de códigos de notificações da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="push-notifications"></a>Notificações por push


Notificações por push são ideais para comunicar dados em tempo real ou dados personalizados para o seu usuário. As notificações por push também são usadas para o conteúdo que é gerado em horas imprevisíveis, como notícias da rede social ou mensagens instantâneas. Essas notificações também são úteis nas situações em que os dados são suscetíveis ao tempo de uma maneira que não seria adequada às notificações periódicas, como pontuações de esportes durante um jogo.

As notificações por push exigem um serviço na nuvem que gerenciará os canais de notificação por push e escolherá quando e a quem as notificações serão enviadas.

Por padrão, as notificações por push expiram três dias depois que são recebidas pelo dispositivo. Se necessário, você pode substituir esse padrão por um prazo de expiração explícito (as notificações do sistema têm um prazo máximo de três dias).

Para obter mais informações, consulte:

-   [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
-   [Diretrizes para notificações por push](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [Exemplos de códigos de notificações da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)


## <a name="related-topics"></a>Tópicos relacionados


* [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md)
* [Enviar uma notificação do sistema local](send-local-toast.md)
* [Diretrizes para notificações por push](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Diretrizes para notificações do sistema](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [Visão geral de notificações periódicas](periodic-notification-overview.md)
* [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
* [Exemplos de códigos de notificações da Plataforma Universal do Windows (UWP) no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 




