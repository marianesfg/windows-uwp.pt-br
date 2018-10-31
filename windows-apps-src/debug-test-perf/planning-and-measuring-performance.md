---
author: jwmsft
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: Planejando para o desempenho
description: Os usuários esperam que seus apps mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e62e724cceb458ba922143e61058dffa8d16a0b8
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5823089"
---
# <a name="planning-for-performance"></a>Planejando para o desempenho



Os usuários esperam que seus aplicativos mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria. Tecnicamente, o desempenho é um requisito não funcional, mas tratar o desempenho como um recurso ajudará você atender às expectativas dos usuários. Especificar metas e mensurar são fatores importantes. Determine quais são os cenários críticos de desempenho; defina o que significa bom desempenho. Em seguida, faça medições o quanto antes e com frequência suficiente ao longo do ciclo de vida do projeto para cumprir suas metas.

## <a name="specifying-goals"></a>Especificando metas

A experiência do usuário é uma maneira básica de definir um bom desempenho. O tempo de inicialização de um aplicativo pode influenciar a percepção do usuário sobre seu desempenho. O usuário pode considerar o tempo de inicialização de aplicativo inferior a um segundo excelente, inferior a 5 segundos bom, e maior que 5 segundos ruim.

Outras métricas têm um impacto menos óbvio na experiência do usuário, como a memória. A probabilidade de um aplicativo ser encerrado enquanto suspenso ou inativo aumenta de acordo com a quantidade de memória que ele usa. É uma regra geral que alto uso de memória degrada a experiência de todos os aplicativos no sistema. Portanto, é sensato ter uma meta de consumo de memória. Leve em consideração o tamanho aproximado de seu aplicativo na percepção dos usuários: pequeno, médio ou grande. A expectativas em torno do desempenho serão correlacionadas a essa percepção. Por exemplo, convém que um aplicativo pequeno que não usa muitos recursos de mídia consuma menos de 100 MB de memória.

É melhor definir uma meta inicial e revisá-la mais tarde do que não ter meta alguma. As metas de desempenho do seu aplicativo devem ser específicas e mensuráveis e se enquadrar em três categorias: quanto tempo os usuários ou o aplicativo levam para concluir tarefas, a taxa e continuidade com que o aplicativo redesenha a si em resposta à interação do usuário (fluidez) e como o aplicativo se sai ao conservar recursos de sistema, incluindo a carga da bateria (eficiência).

## <a name="time"></a>Tempo

Pense nos intervalos aceitáveis de tempo decorrido (*classes de interação*) que leva para os usuários concluírem suas tarefas em seu aplicativo. Para cada classe de interação, atribua um rótulo, um sentimento na percepção do usuário e as durações ideal e máxima. A seguir estão algumas sugestões.

| Rótulo da classe de interação | Percepção do usuário                 | Ideal            | Máximo          | Exemplos                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| Rápido                    | Atraso minimamente perceptível      | 100 milissegundos | 200 milissegundos | Ativar a barra de aplicativos; pressionar um botão (primeira resposta)                        |
| Típico                 | Ágil, mas não rápido             | 300 milissegundos | 500 milissegundos | Redimensionar; zoom semântico                                                        |
| Responsivo              | Não rápido, mas perceptivelmente responsivo | 500 milissegundos | 1 segundo         | Navegar até uma página diferente; retomar o aplicativo de um estado suspenso          |
| Inicialização                  | Experiência competitiva          | 1 segundo         | 3 segundos        | Iniciar o aplicativo pela primeira vez ou depois que ele foi encerrado anteriormente |
| Contínuo              | Não há mais a percepção de resposta      | 500 milissegundos | 5 segundos        | Baixar um arquivo da Internet                                            |
| Cativo                 | Longo tempo; o usuário pode alternar para outro aplicativo    | 500 milissegundos | 10 segundos       | Instalar vários aplicativos da Loja                                         |

 

Agora você pode atribuir classes de interação aos cenários de desempenho de seu aplicativo. Você pode atribuir, por exemplo, a referência pontual do aplicativo, uma parte da experiência do usuário e uma classe de interação a cada cenário. Consulte a seguir algumas sugestões para um aplicativo de exemplo para alimentos e refeições.


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>Cenário</th><th>Ponto no tempo</th><th>Experiência do usuário</th><th>Classe de interação</th></tr>
<tr><td rowspan="3">Navegar até uma página de receita </td><td>Primeira resposta</td><td>Animação de transição de página iniciada</td><td>Rápido (100 a 200 milissegundos)</td></tr>
<tr><td>Responsivo</td><td>Lista de ingredientes carregada; sem imagens</td><td>Responsivo (500 milissegundos a 1 segundo)</td></tr>
<tr><td>Visibilidade completa</td><td>Todo o conteúdo carregado; imagens exibidas</td><td>Contínuo (500 milissegundos a 5 segundos)</td></tr>
<tr><td rowspan="2">Procurar uma receita</td><td>Primeira resposta</td><td>Botão de pesquisa clicado</td><td>Rápido (100 a 200 milissegundos)</td></tr>
<tr><td>Visibilidade completa</td><td>Lista de títulos de receitas locais exibida</td><td>Típico (300 a 500 milissegundos)</td></tr>
</table>

Se você estiver exibindo conteúdo ao vivo, considere também metas de renovação de conteúdo. A meta é atualizar o conteúdo a cada poucos segundos? Ou, atualizar o conteúdo a cada poucos minutos ou horas, ou até mesmo uma vez por dia, é uma experiência aceitável para o usuário?

Com suas metas especificadas, agora você está mais apto a testar, analisar e otimizar seu aplicativo.

## <a name="fluidity"></a>Fluidez

As metas de fluidez específicas e mensuráveis para seu aplicativo podem incluir:

-   Não há interrupções e reinícios para o redesenho da tela (falhas).
-   Renderização de animações em 60 quadros por segundo (FPS).
-   Quando um usuário faz um movimento panorâmico/rolagem, o aplicativo apresenta 3 a 6 páginas de conteúdo por segundo.

## <a name="efficiency"></a>Eficiência

As metas de eficiência específicas e mensuráveis para seu aplicativo podem incluir:

-   Para o processo de seu aplicativo, a porcentagem da CPU deve estar dentro ou abaixo de *N* e o uso de memória em MB deve ser de ou abaixo de *M* sempre.
-   Quando o aplicativo está inativo, *N* e *M* são zero para o processo do aplicativo.
-   Seu aplicativo pode ser usado ativamente por *X* horas na bateria. Quando ele está inativo, o dispositivo mantém sua carga por *Y* horas.

## <a name="design-your-app-for-performance"></a>Projetar seu aplicativo para o desempenho

Agora você pode usar suas metas de desempenho para influenciar o design de seu aplicativo. Usando o aplicativo de exemplo para alimentos e refeições, depois que o usuário navega para a página da receita, você pode optar por [atualizar itens de forma incremental](optimize-gridview-and-listview.md#update-items-incrementally) para que o nome da receita seja renderizado primeiro, a exibição dos ingredientes seja adiada e a exibição das imagens seja ainda mais adiada. Isso mantém a capacidade de resposta e uma interface do usuário flexível ao movimento panorâmico/rolagem, com a renderização de total fidelidade ocorrendo depois que a interação diminui a um ritmo que permita que o thread de interface do usuário acompanhe. Consulte a seguir outros aspectos que devem ser levados em consideração.

**Interface do Usuário**

-   Maximize o tempo de análise e carregamento e a eficiência da memória para cada página da interface do usuário do seu aplicativo (especialmente a página inicial) [otimizando sua marcação XAML](optimize-xaml-loading.md). Resumindo, adie o carregamento da interface do usuário e do código até que seja necessário.
-   Para [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705), crie todos os itens do mesmo tamanho e use o máximo de [técnicas de otimização de ListView e GridView](optimize-gridview-and-listview.md) possível.
-   Declare a interface do usuário na forma de marcação, que a estrutura pode carregar e reutilizar em blocos, em vez de construí-la imperativamente em código.
-   Atrase a criação de elementos da interface do usuário até que o usuário precise deles. Veja o atributo [**x:Load**](../xaml-platform/x-load-attribute.md).
-   Prefira as transições de tema e animações às animações de storyboard. Para obter mais informações, consulte [Visão geral das animações](https://msdn.microsoft.com/library/windows/apps/Mt187350). Lembre-se de que as animações de storyboard requerem atualizações constantes na tela e mantêm ativo o pipeline de CPU e elementos gráficos. Para preservar a bateria, não deixe animações em execução se o usuário não estiver interagindo com o aplicativo.
-   As imagens que você carregar devem ser carregadas em um tamanho apropriado ao modo de exibição de apresentação, usando o método [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210).

**CPU, memória e energia**

-   Agende o trabalho de prioridade mais baixa para execução em threads e/ou núcleos de prioridade mais baixa. Consulte [Programação assíncrona](https://msdn.microsoft.com/library/windows/apps/Mt187335), a propriedade [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) e a classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).
-   Minimize o consumo de memória do aplicativo liberando recursos caros (como mídia) em suspensão.
-   Minimize o conjunto de trabalho do seu código.
-   Evite vazamentos de memória, cancelando o registro de manipuladores de eventos e a referência a elementos da interface do usuário sempre que possível.
-   Para poupar a bateria, seja conservador na frequência da sondagem de dados, consulte um sensor ou agende trabalho na CPU quando ela estiver ociosa.

**Acesso a dados**

-   Se possível, faça uma pré-busca do conteúdo. Para pré-buscas automáticas, consulte a classe [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042). Para pré-buscas manuais, consulte o namespace [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) e a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517).
-   Se possível, armazene em cache conteúdo que seja caro para acessar. Consulte as propriedades [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) e [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622).
-   Para erros de cache, mostre o mais rápido possível uma interface do usuário de espaço reservado que indique que o aplicativo ainda está carregando conteúdo. Faça a transição do espaço reservado para o conteúdo dinâmico de uma forma que não seja chocante para o usuário. Por exemplo, não mude a posição do conteúdo ao carregar conteúdo dinâmico enquanto o usuário estiver interagindo por toque ou com o mouse.

**Inicialização e retomada do aplicativo**

-   Adie a tela inicial do aplicativo e só a estenda se for necessário. Para obter detalhes, consulte [Criando uma experiência rápida e fluida de inicialização de aplicativos](http://go.microsoft.com/fwlink/p/?LinkId=317595) e [Exibir uma tela inicial por mais tempo](https://msdn.microsoft.com/library/windows/apps/Mt187309).
-   Desabilite animações que aparecem logo depois que a tela inicial é ignorada, pois elas geram unicamente uma percepção de atraso no tempo de inicialização do aplicativo.

**Interface do usuário adaptável e orientação**

-   Use a classe [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021).
-   Conclua imediatamente apenas o trabalho exigido, adiando o trabalho de aplicativo intensivo — seu aplicativo tem entre 200 e 800 milissegundos para concluir o trabalho antes de o usuário ver a IU do aplicativo em um estado recortado.

Com seus designs de desempenho formulados, você pode iniciar o codificação do aplicativo.

## <a name="instrument-for-performance"></a>Instrumentar com objetivo no desempenho

Ao codificar, adicione código que registre mensagens e eventos em determinados pontos durante a execução. Mais tarde, quando estiver testando o aplicativo, você poderá usar ferramentas de criação de perfil, como o Windows Performance Recorder e o Windows Performance Analyzer (ambos incluídos no [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)) para criar e visualizar um relatório sobre o desempenho do seu aplicativo. Nesse relatório, você pode examinar essas mensagens e eventos para facilitar a análise dos resultados.

A Plataforma Universal do Windows (UWP) fornece APIs de registro em log com o suporte do [ETW (Rastreamento de Eventos para Windows)](https://msdn.microsoft.com/library/windows/desktop/Bb968803). Juntos, esses recursos oferecem uma solução aprimorada de rastreamento e registro de log de eventos. As APIs, que fazem parte do namespace [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677), incluem as classes [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138), [ **LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195), [**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) e [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217).

Para registrar uma mensagem em um relatório em um ponto específico enquanto o aplicativo é executado, crie um objeto **LoggingChannel** e, em seguida, chame o método [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingchannel.logmessage.aspx) do objeto, dessa forma.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel("MyLoggingChannel");

myLoggingChannel.LogMessage(LoggingLevel.Information, "Here' s my logged message.");

// ...
```

Para registrar eventos de início e interrupção no relatório ao longo de um período enquanto o aplicativo está em execução, crie um objeto **LoggingActivity** e, em seguida, chame o construtor [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingactivity.loggingactivity.aspx) desse objeto, dessa forma.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel is defined and initialized in the previous code example.
using (myLoggingActivity = new LoggingActivity("MyLoggingActivity"), myLoggingChannel))
{   // After this logging activity starts, a start event is logged.
    
    // Add code here to do something of interest.
    
}   // After this logging activity ends, an end event is logged.

// ...
```

Consulte também o [Exemplo de registro em log](http://go.microsoft.com/fwlink/p/?LinkId=529576).

Com o seu aplicativo instrumentado, você pode testar e medir o seu desempenho.

## <a name="test-and-measure-against-performance-goals"></a>Fazer testes e avaliações com base em metas de desempenho

Parte de seu plano de desempenho é definir os pontos durante o desenvolvimento onde você medirá o desempenho. Isso atende a diferentes finalidades, dependendo se você medirá durante a fase de protótipo, desenvolvimento ou implantação. Medir o desempenho durante os estágios iniciais da criação do protótipo pode ser extremamente valioso. Portanto, recomendamos que você faça isso assim que tiver o código que faz um trabalho significativo. Medições nos estágios iniciais dão uma boa ideia de onde estão os custos importantes no seu aplicativo, servindo de base para tomar decisões quanto ao design. Isso resulta em aplicativos de alto desempenho e escalabilidade. É geralmente mais caro alterar designs posteriormente do que inicialmente. A medição do desempenho em uma fase tardia do ciclo do produto pode resultar em reconfigurações de última hora e desempenho fraco.

Use essas técnicas e ferramentas para testar como seu aplicativo se compara às suas metas de desempenho originais.

-   Faça testes com base em várias configurações de hardware, incluindo PCs de desktop e multifuncionais, laptops e ultrabooks e tablets, entre outros dispositivos móveis.
-   Faça testes com base em uma ampla variedade de tamanhos de tela. Embora tamanhos de tela mais amplos possam mostrar muito mais conteúdo, trazer todo esse conteúdo extra pode prejudicar o desempenho.
-   Elimine o máximo possível das variáveis de teste.
    -   Desative aplicativos em segundo plano no dispositivo de teste. Para isso, no Windows, selecione **Configurações** no menu Iniciar &gt; **Personalização** &gt; **Tela de bloqueio**. Selecione cada aplicativo ativo e selecione **Nenhum**.
    -   Compile seu aplicativo no código nativo o criando na configuração da versão antes de implantá-lo no dispositivo de teste.
    -   Para garantir que a manutenção automática não afete o desempenho do dispositivo de teste, dispare-o manualmente e aguarde a conclusão. No Windows, no menu Iniciar, procure **Segurança e Manutenção**. Na área de **Manutenção**, em **Manutenção Automática**, selecione **Iniciar manutenção** e aguarde o status sair de **Manutenção em progresso**.
    -   Execute o aplicativo várias vezes para ajudar a eliminar variáveis de teste aleatórias e a garantir medições consistentes.
-   Teste a disponibilidade de redução de energia. O dispositivo de seus usuários pode ter significativamente menos energia de que seu computador de desenvolvimento. O Windows foi projetado para dispositivos de baixo consumo de energia, como dispositivos móveis. Os aplicativos que são executados na plataforma devem garantir um bom desempenho nesses dispositivos. Como aprendizado, espere que um dispositivo de baixa energia execute em cerca de um quarto da velocidade de um computador desktop e defina suas metas de acordo.
-   Use uma combinação de ferramentas, como o Microsoft Visual Studio e o Windows Performance Analyzer, para medir o desempenho do aplicativo. O Visual Studio foi projetado para fornecer análises centradas no aplicativo, como vinculação de código-fonte. O Windows Performance Analyzer foi projetado para fornecer análises centradas no sistema, como o fornecimento de informações do sistema, informações sobre eventos de manipulação de toque e informações sobre E/S (entrada e saída) de disco e custo da GPU (unidade de processamento gráfico). Ambas as ferramentas oferecem captura e exportação de rastreamento e podem reabrir rastreamentos compartilhados e post-mortem.
-   Antes de enviar seu aplicativo para a loja para certificação, certifique-se de incorporar em seus planos de teste os casos de teste relacionados ao desempenho, conforme descrito na seção "Testes de desempenho" dos [testes do Kit de certificação de aplicativo do Windows](windows-app-certification-kit-tests.md) e no "desempenho e seção de estabilidade"de [casos de teste de aplicativo UWP](https://msdn.microsoft.com/library/windows/apps/Dn275879).

Para obter mais informações, consulte esses recursos e ferramentas de criação de perfil.

-   [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)
-   [Analisar o desempenho usando ferramentas de diagnóstico do Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/hh696636.aspx)
-   A sessão //build/ [Desempenho do XAML](https://channel9.msdn.com/Events/Build/2015/3-698)
-   A sessão //build/ [Novas ferramentas XAML no Visual Studio 2015](https://channel9.msdn.com/Events/Build/2015/2-697)

## <a name="respond-to-the-performance-test-results"></a>Responder aos resultados do teste de desempenho

Depois de analisar os resultados de testes de desempenho, determine se alterações são necessárias, por exemplo:

-   Você deve alterar alguma de suas decisões de design do aplicativo ou otimizar seu código?
-   Você deve você adicionar, remover ou modificar qualquer uma das instrumentações no código?
-   Você deve repensar alguma das suas metas de performance?

Se alterações forem necessárias, faça-as e volte para a instrumentação ou testes e repita.

## <a name="optimizing"></a>Otimização

Otimize somente os caminhos de código críticos de desempenho em seu aplicativo: os que exigem maior dedicação de tempo. A criação do perfil lhe dirá quais são eles. Frequentemente, há uma compensação entre a criação de um software que siga boas práticas de design e a geração de código que opere com o máximo de otimização. Geralmente, é melhor priorizar a produtividade do desenvolvedor e um bom design de software em áreas onde o desempenho não é uma preocupação.