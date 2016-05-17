---
author: mijacobs
Description: Notificações periódicas, que são chamadas também de notificações de sondagem, atualizam blocos e selos em um intervalo fixo, baixando o conteúdo da nuvem.
title: Visão geral de notificações periódicas
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
label: TBD
template: detail.hbs
---

# Visão geral de notificações periódicas





Notificações periódicas, que são chamadas também de notificações de sondagem, atualizam blocos e selos em um intervalo fixo, baixando o conteúdo da nuvem. Para usar notificações periódicas, seu código de aplicativo cliente precisa fornecer duas partes de informações:

-   O URI (Uniform Resource Identifier) de um local na Web para o Windows sondar por atualizações de bloco ou notificação para o seu aplicativo
-   A frequência com que o URI deve ser sondado

As notificações periódicas permitem que seu aplicativo obtenha atualizações de bloco em tempo real com serviço de nuvem mínimo e pouco investimento do cliente. As notificações periódicas são um método de entrega excelente para distribuir o mesmo conteúdo a uma ampla audiência.

**Observação**   Você pode saber mais baixando a [amostra de notificações periódicas e por Push](http://go.microsoft.com/fwlink/p/?linkid=231476) para Windows 8.1 e reutilizando o código-fonte em seu aplicativo do Windows 10.

 

## <span id="How_it_works"></span><span id="how_it_works"></span><span id="HOW_IT_WORKS"></span>Como funciona


As notificações periódicas requerem que seu aplicativo hospede um serviço de nuvem. O serviço será sondado periodicamente por todos os usuários que tiverem o aplicativo instalado. A cada intervalo de sondagem, como uma vez por hora, o Windows envia uma solicitação HTTP GET para o URI, baixa o conteúdo da notificação ou do bloco solicitado (como XML), que é fornecido em resposta à solicitação, e exibe o conteúdo no bloco do aplicativo.

Observe que as atualizações periódicas não podem ser usadas com notificações do sistema. As notificações do sistema funcionam melhor por meio de notificações [programadas](https://msdn.microsoft.com/library/windows/apps/hh465417) ou [por push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## <span id="URI_location_and_XML_content"></span><span id="uri_location_and_xml_content"></span><span id="URI_LOCATION_AND_XML_CONTENT"></span>Local do URI e conteúdo XML


Qualquer endereço HTTP ou HTTPS válido pode ser usado como o URI a ser sondado.

A resposta do servidor de nuvem inclui o conteúdo baixado. O conteúdo retornado do URI deve estar em conformidade com a especificação de esquema XML do [Bloco](tiles-and-notifications-adaptive-tiles-schema.md) ou da [Notificação](https://msdn.microsoft.com/library/windows/apps/br212851) e deve estar codificado em UTF-8. Você pode usar cabeçalhos HTTP definidos para especificar o [tempo de expiração](#expiry) ou a [marca](#taggo) da notificação.

## <span id="Polling_Behavior"></span><span id="polling_behavior"></span><span id="POLLING_BEHAVIOR"></span>Comportamento de sondagem


Chame um destes métodos para começar a sondagem:

-   [
              **StartPeriodicUpdate**
            ](https://msdn.microsoft.com/library/windows/apps/hh701684) (Bloco)
-   [
              **StartPeriodicUpdate**
            ](https://msdn.microsoft.com/library/windows/apps/hh701611) (Selo)
-   [
              **StartPeriodicUpdateBatch**
            ](https://msdn.microsoft.com/library/windows/apps/hh967945) (Bloco)

Quando você chama um destes métodos, o URI é imediatamente sondado e o bloco ou a notificação será atualizada com o conteúdo recebido. Após essa sondagem inicial, o Windows continua fornecendo as atualizações no intervalo solicitado. A sondagem continua até você parar explicitamente (com [**TileUpdater.StopPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701697)), seu aplicativo ser desinstalado ou, no caso de um bloco secundário, o bloco ser removido. Caso contrário, o Windows continua a sondar atualizações para seu bloco ou notificação mesmo que seu aplicativo não seja mais iniciado.

### <span id="The_recurrence_interval"></span><span id="the_recurrence_interval"></span><span id="THE_RECURRENCE_INTERVAL"></span>O intervalo de recorrência

Especifique o intervalo de recorrência como um parâmetro dos métodos listados acima. Observe que embora o Windows se esforce para sondar conforme solicitado, o intervalo não é preciso. O intervalo de sondagem solicitado pode ser atrasado em até 15 minutos, a critério do Windows.

### <span id="The_start_time"></span><span id="the_start_time"></span><span id="THE_START_TIME"></span>O horário de início

Você também pode especificar um determinado horário do dia para começar uma sondagem. Considere um aplicativo que altera o conteúdo do bloco uma vez ao dia. Nesse caso, nós recomendamos que você faça sondagens perto do horário da atualização do seu serviço de nuvem. Por exemplo, se um site de compras diário publica as ofertas do dia às 08:00, sonde o novo conteúdo do bloco logo após às 08:00.

Se você fornecer o horário de início, a primeira chamada para o método fará a sondagem do conteúdo imediatamente. Então, a sondagem regular será iniciada dentro de 15 minutos após o horário de início fornecido.

### <span id="Automatic_retry_behavior"></span><span id="automatic_retry_behavior"></span><span id="AUTOMATIC_RETRY_BEHAVIOR"></span>Comportamento de repetição automática

O URI é sondado somente se o dispositivo estiver online. Se a rede estiver disponível, mas o URI não puder ser contatado por algum motivo, essa iteração do intervalo de sondagem é ignorada e o URI será sondado novamente no próximo intervalo. Se o dispositivo está em estado desligado, suspenso ou em hibernação quando um intervalo de sondagem é alcançado, o URI é sondado quando o dispositivo retorna do estado desligado ou suspenso.

## <span id="expiry"></span><span id="EXPIRY"></span>Expiração de notificações de selo e bloco


Por padrão, as notificações periódicas de bloco e de selo expiram três dias depois que são baixadas. Quando uma notificação expira, o conteúdo é removido do selo, do bloco ou da fila e não é mais mostrado para o usuário. É recomendável definir um horário de expiração explícito em todas as notificações de bloco e de selo periódicas usando um tempo que faça sentido para o aplicativo ou notificação, para garantir que  o conteúdo do bloco não continue além do tempo relevante. Um tempo de expiração explícito é essencial para conteúdo com tempo de vida definido. Isso também garante a remoção de conteúdo obsoleto se seu serviço de nuvem se tornar inacessível ou se o usuário se desconectar da rede por um período de tempo prolongado.

Seu serviço de nuvem configura uma data de expiração e hora para uma notificação incluindo o cabeçalho HTTP X-WNS-Expire na carga da resposta. O cabeçalho HTTP X-WNS-Expires está em conformidade com o [formato de data HTTP](http://go.microsoft.com/fwlink/p/?linkid=253706). Para saber mais, consulte [**StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701684) ou [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945)

Por exemplo, durante um dia de negociação ativo do mercado de ações, você pode definir a expiração de uma atualização de preços de ações para duas vezes mais do que seu intervalo de sondagem (por exemplo, uma hora após o recebimento, se estiver sondando a cada meia hora). Outro exemplo é um aplicativo de notícias que pode determinar que um dia é um período de expiração adequado para a atualização de blocos de notícias diárias.

## <span id="taggo"></span><span id="TAGGO"></span>Notificações periódicas na fila de notificações


Você pode usar atualizações de bloco periódicas com [ciclos de notificações](https://msdn.microsoft.com/library/windows/apps/hh781199). Por padrão, um bloco na tela inicial mostra o conteúdo de uma única notificação até que ela seja substituída por uma nova. Ao habilitar os ciclos de notificações, até cinco notificações são mantidas na fila e os bloco alterna entre elas.

Se a fila tiver atingido a capacidade de cinco notificações, a próxima notificação nova substituirá a mais antiga na fila. Porém, definindo marcas em suas notificações, você pode influenciar a política de substituição da fila. Uma marca é uma cadeia de caracteres de até 16 caracteres alfanuméricos específica do aplicativo e que não diferencia maiúsculas de minúsculas, especificada no cabeçalho HTTP [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) na carga da resposta. O Windows compara a marca de uma notificação de entrada com as marcas de todas as notificações que já estão na fila. Se for encontrada uma correspondência, a nova notificação substituirá a notificação da fila com a mesma marca. Se nenhuma correspondência for encontrada, a regra de substituição padrão será aplicada e a nova notificação substituirá a antiga na fila.

Você pode usar a marcação e a fila de notificações para implementar diversos cenários avançados de notificação. Por exemplo, um aplicativo de ações pode enviar cinco notificações, cada uma sobre uma ação diferente e marcada com um nome de ação. Isso evita que a fila contenha duas notificações para a mesma ação, uma delas mais antiga e desatualizada.

Para saber mais, consulte [Usando a fila de notificações](https://msdn.microsoft.com/library/windows/apps/hh781199)

### <span id="Enabling_the_notification_queue"></span><span id="enabling_the_notification_queue"></span><span id="ENABLING_THE_NOTIFICATION_QUEUE"></span>Habilitando a fila de notificações

Para implementar uma fila de notificações, primeiro habilite a fila para seu bloco (consulte [Como usar a fila de notificações com notificações locais](https://msdn.microsoft.com/library/windows/apps/hh465429)). A chamada para habilitar a fila precisa ser feita apenas uma vez durante a vida útil do seu aplicativo, mas não tem problema fazer isso cada vez que seu aplicativo é iniciado.

### <span id="Polling_for_more_than_one_notification_at_a_time"></span><span id="polling_for_more_than_one_notification_at_a_time"></span><span id="POLLING_FOR_MORE_THAN_ONE_NOTIFICATION_AT_A_TIME"></span>Sondando mais de uma notificação por vez

Você deve fornecer um URI exclusivo para cada notificação que quiser que o Windows baixe para o bloco. Ao usar o método [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945), você pode fornecer até cinco URIs por vez para usar com a fila de notificações. Cada URI é sondado para uma carga de notificação única, no mesmo tempo ou perto dele. Cada URI sondado pode retornar o próprio valor de expiração e marca.

## <span id="related_topics"></span>Tópicos relacionados


* [Diretrizes para notificações periódicas](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [Como configurar notificações periódicas para selos](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [Como configurar notificações periódicas para blocos](https://msdn.microsoft.com/library/windows/apps/hh761476)
 

 






<!--HONumber=May16_HO2-->


