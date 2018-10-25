---
title: Copiando e acessando dados de recursos
description: Sinalizadores de uso indicam como o app pretende usar os dados de recurso para colocar os recursos na área mais eficiente de memória possível. Os dados de recursos são copiados em todos os recursos para que a CPU ou GPU possa acessá-los sem afetar o desempenho.
ms.assetid: 6A09702D-0FF2-4EA6-A353-0F95A3EE34E2
keywords:
- Copiando e acessando dados de recursos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e7b0f06711b4a908f8990dfb16968400c685c15f
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5542373"
---
# <a name="copying-and-accessing-resource-data"></a>Copiando e acessando dados de recursos


Sinalizadores de uso indicam como o app pretende usar os dados de recurso para colocar os recursos na área mais eficiente de memória possível. Os dados de recursos são copiados em todos os recursos para que a CPU ou GPU possa acessá-los sem afetar o desempenho.

Não é necessário pensar nos recursos como sendo criados na memória de vídeo ou memória do sistema, ou para decidir se o tempo de execução deve ou não gerenciar a memória. Com a arquitetura do WDDM (Windows Display Driver Model), os apps criam recursos Direct3D com sinalizadores de uso de diferentes para indicar como o app pretende usar os dados de recurso. Esse modelo de driver virtualiza a memória usada por recursos; é responsabilidade do gerenciador de memória/driver/sistema operacional colocar os recursos na área mais eficiente possível da memória, dado o uso esperado.

Por padrão, os recursos estão disponíveis para a GPU. Há momentos quando os dados de recurso precisam estar disponíveis para a CPU. Copiar os dados de recurso ao redor para que o processador apropriado possa acessá-los sem afetar o desempenho exige algum conhecimento de como funcionam os métodos da API.

## <a name="span-idcopyingspanspan-idcopyingspanspan-idcopyingspancopying-resource-data"></a><span id="Copying"></span><span id="copying"></span><span id="COPYING"></span>Copiando dados de recursos


Os recursos são criados na memória quando o Direct3D executa uma chamada de criação. Eles podem ser criados na memória de vídeo, memória do sistema ou qualquer outro tipo de memória. Como o modelo de driver WDDM virtualiza essa memória, apps não precisam mais acompanhar que tipo de recursos de memória são criados.

Idealmente, todos os recursos devem estar localizados na memória de vídeo para que a GPU possa ter acesso imediato a eles. No entanto, às vezes, é necessário que a CPU leia os dados de recurso ou que a GPU acesse os dados de recurso nos quais a CPU realizou gravações. O Direct3D lida com esses cenários diferentes, solicitando que o app especifique um uso e, em seguida, oferece vários métodos de copiar dados de recursos quando necessário.

Dependendo de como o recurso foi criado, ele nem sempre é capaz de acessar diretamente os dados subjacentes. Isso pode significar que os dados de recurso devem ser copiados do recurso de origem para outro recurso que seja acessível pelo processador apropriado. Em termos de Direct3D, os recursos padrão podem ser acessados diretamente pela GPU, os recursos de preparo e dinâmica podem ser acessados diretamente pela CPU.

Depois que um recurso é criado, sua utilização não pode ser alterada. Em vez disso, copie o conteúdo de um recurso para outro recurso que foi criado com um uso diferente. Copiar dados de recursos de um recurso para outro, ou copiar dados da memória para um recurso.

Existem dois tipos principais de recursos: mapeáveis e não mapeáveis. Os recursos criados com usos dinâmico ou de preparo são mapeáveis, enquanto os recursos criados com os usos padrão ou imutável são não mapeáveis.

Copiar dados entre recursos não mapeáveis é muito rápido porque esse é o caso mais comum e foi otimizado para uma boa execução. Como esses recursos não são diretamente acessíveis pela CPU, eles são otimizados para que a GPU possa manipulá-los rapidamente.

Copiar dados entre recursos mapeáveis é mais problemático porque o desempenho depende do uso com o qual o recurso foi criado. Por exemplo, a GPU pode ler um recurso dinâmico rapidamente, mas não pode gravá-los, e a GPU não pode ler ou gravar recursos de preparo diretamente.

Os apps que desejam copiar dados de um recurso com uso do padrão para um recurso com uso de preparo (para permitir que a CPU leia os dados, ou seja, o problema de reprodução da GPU) devem fazer isso com cuidado. Consulte [Acessando dados de recursos](#accessing)abaixo.

## <a name="span-idaccessingspanspan-idaccessingspanspan-idaccessingspanaccessing-resource-data"></a><span id="Accessing"></span><span id="accessing"></span><span id="ACCESSING"></span>Acessando dados de recursos


Acessar um recurso requer o mapeamento do recurso; mapeando significa essencialmente que o app está tentando dar acesso à CPU para a memória. O processo de mapeamento de um recurso para que a CPU possa acessar a memória subjacente pode causar alguns afunilamentos de desempenho e por esse motivo, é obrigatório ter cautela com a forma e o momento da execução dessa tarefa.

O desempenho pode ser interrompidas se o app tentar mapear um recurso no momento errado. Se o app tentar acessar os resultados de uma operação antes que essa operação esteja concluída, ocorrerá uma vaga de pipeline.

Executar uma operação de mapeamento no momento errado pode causar uma queda grave no desempenho, forçando a GPU e CPU a sincronizarem entre si. Essa sincronização ocorrerá se o app quiser acessar um recurso antes que a GPU termine de copiá-lo para um recurso que a CPU pode mapear.

### <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Considerações sobre desempenho

É melhor pensar em um computador como sendo uma máquina funcionando como uma arquitetura paralela com dois tipos principais de processadores: uma ou mais CPUs e uma ou mais GPUs. Como em qualquer arquitetura paralela, o melhor desempenho é atingido quando cada processador está programado tarefas o suficiente para impedir que ele fique ocioso e que o trabalho de um processador não fique aguardando o trabalho do outro.

O pior cenário para o paralelismo de GPU/CPU é a necessidade de forçar um processador a esperar pelos resultados do trabalho feito pelo outro. O Direct3D remove esse custo, tornando os métodos de cópia assíncronos; a cópia não é necessariamente executada pelos retornos de método e tempo.

A vantagem disso é que o app não paga o custo de desempenho de copiar realmente os dados até que a CPU acesse os dados, que é quando é chamado o mapa. Se o método de mapa for chamado depois que os dados realmente foram copiados, nenhuma perda de desempenho ocorre. Por outro lado, se o método de mapa for chamado antes dos dados terem sido copiados, uma vaga de pipeline ocorrerá.

As chamadas assíncronas no Direct3D (que são a grande maioria dos métodos e, especialmente, chamadas de renderização) são armazenadas no que é chamado de *buffer de comando*. Esse buffer encontra-se no interior do driver de elementos gráficos e é usado para compilar em lotes as chamadas para o hardware subjacente, para que a alternância dispendiosa do modo de usuário para o modo de kernel no Microsoft Windows ocorra com a menor frequência possível.

O buffer de comando é liberado, causando uma alternância de modo de usuário/kernel, em uma das quatro situações, que são as seguintes.

1.  A apresentação é chamada.
2.  A liberação é chamada.
3.  O buffer de comando está cheio; seu tamanho é dinâmico e é controlado pelo sistema operacional e pelo driver gráfico.
4.  A CPU requer acesso aos resultados de um comando que está aguardando para ser executado no buffer de comando.

Das quatro situações acima, a número quatro é a mais crítica para o desempenho. Se o app emitir uma chamada para copiar um recurso ou sub-recurso, essa chamada é enfileirada no buffer de comando.

Se o app tentar mapear o recurso de preparo que era o destino da chamada de cópia antes que o buffer de comando tenha sido liberado, uma vaga de pipeline ocorrerá, porque não apenas a chamada do método de cópia precisa ser executa, mas todos os outros comandos armazenados no buffer de comando também devem ser executados. Isso fará com que a GPU e a CPU sincronizem porque a CPU aguardará para acessar o recurso de preparo enquanto a GPU esvazia o buffer de comando e finalmente preenche o recurso que a CPU precisa. Depois que a GPU finalizar a cópia, a CPU começará a acessar o recurso de preparo, mas durante esse tempo, a GPU estará ociosa.

Fazer isso com frequência em tempo de execução prejudicará gravemente o desempenho. Por esse motivo, o mapeamento de recursos criado com o uso padrão deve ser feito com cautela. O app precisa esperar tempo suficiente para que o buffer de comando seja esvaziado e, portanto, espera que todos esses comandos encerrem a execução antes de tentar mapear o recurso de preparo correspondente.

Quanto tempo o app deve esperar? Pelo menos dois quadros porque isso permitirá que o paralelismo entre as CPUs e a GPU seja utilizado ao máximo. A GPU funciona da seguinte forma: enquanto o app está processando o quadro N enviando chamadas para o buffer de comando, a GPU está ocupada executando as chamadas do quadro anterior, N-1.

Sendo assim, se um app quiser mapear um recurso que tem origem na memória de vídeo e copiar um recurso no quadro N, essa chamada realmente começará a ser executada no quadro N+1, quando o app estiver enviando chamadas para o próximo quadro. A cópia deve ser concluída quando o app estiver processando o quadro N+2.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Quadro</th>
<th align="left">Status da GPU/CPU</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">N</td>
<td align="left"><ul>
<li>A CPU emite chamadas de renderização para o quadro atual.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+1</td>
<td align="left"><ul>
<li>GPU executando chamadas enviadas da CPU durante o quadro N.</li>
<li>A CPU emite chamadas de renderização para o quadro atual.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+2</td>
<td align="left"><ul>
<li>A GPU encerrou as chamadas enviadas da CPU durante o quadro N. Resultados prontos.</li>
<li>GPU executando chamadas enviadas da CPU durante o quadro N+1.</li>
<li>A CPU emite chamadas de renderização para o quadro atual.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+3</td>
<td align="left"><ul>
<li>A GPU encerrou as chamadas enviadas da CPU durante o quadro N+1. Resultados prontos.</li>
<li>GPU executando chamadas enviadas da CPU durante o quadro N+2.</li>
<li>A CPU emite chamadas de renderização para o quadro atual.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+4</td>
<td align="left">...</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos](resources.md)

 

 




