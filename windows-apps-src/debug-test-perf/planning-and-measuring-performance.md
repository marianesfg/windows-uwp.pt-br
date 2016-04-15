---
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: Planejando para o desempenho
description: Os usuários esperam que seus aplicativos mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria.
---
# Planejando para o desempenho

\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Os usuários esperam que seus aplicativos mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria. Tecnicamente, o desempenho é um requisito não funcional, mas tratar o desempenho como um recurso ajudará você atender às expectativas dos usuários. Especificar metas e mensurar são fatores importantes. Determine quais são os cenários críticos de desempenho; defina o que significa bom desempenho. Em seguida, faça medições o quanto antes e com frequência suficiente ao longo do ciclo de vida do projeto para cumprir suas metas.

## Especificando metas

A experiência do usuário é uma maneira básica de definir um bom desempenho. O tempo de inicialização de um aplicativo pode influenciar a percepção do usuário sobre seu desempenho. O usuário pode considerar o tempo de inicialização de aplicativo inferior a um segundo excelente, inferior a 5 segundos bom, e maior que 5 segundos ruim.

Outras métricas têm um impacto menos óbvio na experiência do usuário, como a memória. A probabilidade de um aplicativo ser encerrado enquanto suspenso ou inativo aumenta de acordo com a quantidade de memória que ele usa. É uma regra geral que alto uso de memória degrada a experiência de todos os aplicativos no sistema. Portanto, é sensato ter uma meta de consumo de memória. Leve em consideração o tamanho aproximado de seu aplicativo na percepção dos usuários: pequeno, médio ou grande. A expectativas em torno do desempenho serão correlacionadas a essa percepção. Por exemplo, convém que um aplicativo pequeno que não usa muitos recursos de mídia consuma menos de 100 MB de memória.

É melhor definir uma meta inicial e revisá-la mais tarde do que não ter meta alguma. As metas de desempenho do seu aplicativo devem ser específicas e mensuráveis e se enquadrar em três categorias: quanto tempo os usuários ou o aplicativo levam para concluir tarefas, a taxa e continuidade com que o aplicativo redesenha a si em resposta à interação do usuário (fluidez) e como o aplicativo se sai ao conservar recursos de sistema, incluindo a carga da bateria (eficiência).

## Tempo

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

## Fluidez

Metas específicas e mensuráveis de fluidez para o seu aplicativo podem incluir:

-   Interrupções e inícios no redesenho de tela (falhas).
-   Animações de renderização em 60 quadros por segundo (FPS).
-   Quando um usuário faz um movimento panorâmico ou de rolagem, o aplicativo apresenta de 3 a 6 páginas de conteúdo por segundo.

## Eficiência

Metas específicas e mensuráveis de eficiência para o seu aplicativo podem incluir:

-   Para o processo do seu aplicativo, a porcentagem da CPU é de até *N* e o uso de memória em MB é de até *M* o tempo todo.
-   Quando o aplicativo está inativo, *N* e *M* são zero para o processo do aplicativo.
-   Seu aplicativo pode ser usado ativamente por *X* horas na bateria. Quando ele está inativo, o dispositivo mantém sua carga por *Y* horas.

## Projetar seu aplicativo para desempenho

Agora você pode usar suas metas de desempenho para influenciar o design do seu aplicativo. Usando o exemplo do aplicativo de comidas e refeições, depois que o usuário navegar para a página da receita, você poderá optar por [update items incrementally](optimize-gridview-and-listview.md#update-items-incrementally) para que o nome da receita seja renderizado primeiro. A exibição dos ingredientes é adiada e a exibição de imagens é adiada ainda mais. Isso mantém a capacidade de resposta e uma interface do usuário flexível ao movimento panorâmico/rolagem, com a renderização de total fidelidade ocorrendo depois que a interação diminui a um ritmo que permita que o thread de interface do usuário acompanhe. Consulte a seguir outros aspectos que devem ser levados em consideração.

**IU**

-   Maximize o tempo de análise e de carregamento e a eficiência de memória para cada página de interface do usuário do seu aplicativo (especialmente a página inicial) em [optimizing your XAML markup](optimize-xaml-loading.md). Resumindo, adie o carregamento da interface do usuário e do código até que seja necessário.
-   Para [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705), deixe todos os itens do mesmo tamanho e use o máximo de [ListView and GridView optimization techniques](optimize-gridview-and-listview.md) que puder.
-   Declare a IU na forma de marcação, que a estrutura pode carregar e reutilizar em blocos, em vez de construí-la obrigatoriamente em código.
-   Recolha elementos de IU até que o usuário precise deles. Consulte a propriedade [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992).
-   Prefira animações e transições de tema a animações de storyboard. Para saber mais, consulte [Animations overview](https://msdn.microsoft.com/library/windows/apps/Mt187350). Lembre-se de que as animações de storyboard exigem atualizações constantes na tela e mantêm a CPU e pipeline de elementos gráficos ativos. Para preservar a bateria, não tenha animações em execução se o usuário não estiver interagindo com o aplicativo.
-   As imagens devem ser carregadas em um tamanho adequado à exibição em que você as apresenta, usando o método [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210).

**CPU, memória e energia**

-   Agende o trabalho de menor prioridade para execução em threads de prioridade mais baixa e/ou núcleos. Consulte [Asynchronous programming](https://msdn.microsoft.com/library/windows/apps/Mt187335), a propriedade [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) e a classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).
-   Minimize o volume de memória do seu aplicativo liberando recursos caros (como mídia) em suspensão.
-   Minimize o conjunto de trabalho do seu código.
-   Evite vazamentos de memória desfazendo o registro de manipuladores de eventos e desfazendo a referência a elementos de IU sempre que possível.
-   Para fins de bateria, seja cauteloso com a frequência da sondagem de dados, consulta um sensor ou agenda trabalho na CPU quando ela está ociosa.

**Acesso a dados**

-   Se possível, faça uma pré-busca do conteúdo. Para pré-buscas automáticas, consulte a classe [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042). Para pré-buscas manuais, consulte o namespace [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) e a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517).
-   Se possível, salve conteúdos caros de acessar. Consulte as propriedades [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) e [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622).
-   Para perdas no cache, mostre uma IU de espaço reservado o mais rápido possível que indique que o aplicativo ainda está carregando conteúdo. Faça a transição do espaço reservado para o conteúdo dinâmico de uma forma que não seja chocante para o usuário. Por exemplo, não mude a posição do conteúdo ao carregar conteúdo dinâmico enquanto o usuário estiver interagindo por toque ou com o mouse.

**Inicialização e retomada do aplicativo**

-   Adie a tela inicial do aplicativo e não a estenda, a não ser que seja necessário. Para obter detalhes, consulte [Creating a fast and fluid app launch experience](http://go.microsoft.com/fwlink/p/?LinkId=317595) e [Display a splash screen for more time](https://msdn.microsoft.com/library/windows/apps/Mt187309).
-   Desabilite animações que ocorrem imediatamente após a tela inicial ser ignorada, já que elas levarão à percepção de atraso no tempo de inicialização do aplicativo.

**Interface do usuário adaptável e orientação**

-   Use a classe [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021).
-   Conclua imediatamente apenas o trabalho exigido, adiando o trabalho de aplicativo intensivo—seu aplicativo tem entre 200 e 800 milissegundos para concluir o trabalho antes de o usuário ver a IU do aplicativo em um estado recortado.

Com seus designs de desempenho formulados, você pode iniciar o codificação do aplicativo.

## Instrumento de execução

Enquanto insere códigos, adicione o código que conecte mensagens e eventos em certos pontos enquanto o seu aplicativo é executado. Depois, quanto estiver testando o seu aplicativo, você pode usar ferramentas de criação de perfil como Windows Performance Recorder e Windows Performance Analyzer (ambas incluídas no [Windows Performance Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh162945.aspx)) para criar e visualizar um relatório sobre o desempenho do seu aplicativo. Nesse relatório, você pode procurar por essas mensagens e eventos para ajudá-lo a analisar com mais facilidade os resultados do relatório.

A Plataforma Universal do Windows (UWP) oferece APIs de log, suportadas pelo [Event Tracing for Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/Bb968803), que, juntas, oferecem um rico log de eventos e solução de rastreamento. As APIs, que fazem parte do namespace [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677), incluem as classes [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138), [ **LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195), [**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) e [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217).

Para registrar uma mensagem em um relatório em um ponto específico enquanto o aplicativo é executado, crie um objeto **LoggingChannel** e, em seguida, chame o método [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/Dn264202-logmessage) do objeto, assim.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel(&quot;MyLoggingChannel&quot;);

myLoggingChannel.LogMessage(LoggingLevel.Information, &quot;Here' s my logged message.&quot;);

// ...
```

Para registrar eventos de inicialização e de interrupção no relatório em um período de tempo em que o aplicativo está sendo executado, crie um objeto **LoggingActivity** e chame o construtor [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195-loggingactivity) do objeto, assim.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel é definido e inicializado no exemplo de código anterior.
using (myLoggingActivity = new LoggingActivity(&quot;MyLoggingActivity&quot;), myLoggingChannel))
{   // Depois de essa atividade de log iniciar, um evento de inicialização é registrado.
    
    // Adicione o código aqui para fazer algo do seu interesse.
    
}   // Depois de essa atividade de log terminar, um evento de término é registrado.

// ...
```

Consulte também o [Logging sample](http://go.microsoft.com/fwlink/p/?LinkId=529576).

Com o seu aplicativo instrumentado, você pode testar e medir o seu desempenho.

## Teste e meça com base em metas de desempenho

Parte do seu plano de desempenho é definir os pontos durante o desenvolvimento onde você medirá o desempenho. Isso atende a diferentes finalidades, dependendo se você medirá durante a fase de protótipo, desenvolvimento ou implantação. Medir o desempenho durante os estágios iniciais da criação do protótipo pode ser extremamente valioso. Portanto, recomendamos que você faça isso assim que tiver o código que faz um trabalho significativo. Medições nos estágios iniciais dão uma boa ideia de onde estão os custos importantes no seu aplicativo, servindo de base para tomar decisões quanto ao design. Isso resulta em aplicativos de alto desempenho e escalabilidade. É geralmente mais caro alterar designs posteriormente do que inicialmente. A medição do desempenho em uma fase tardia do ciclo do produto pode resultar em reconfigurações de última hora e desempenho fraco.

Use essas técnicas e ferramentas para testar como o seu aplicativo se compara às suas metas de desempenho originais.

-   Teste com base em uma ampla variedade de configurações de hardware, incluindo computadores multifuncionais e desktop, notebooks, ultrabooks, tablets e outros dispositivos móveis.
-   Teste com base em uma ampla variedade de tamanhos de tela. Apesar de tamanhos de tela maiores poderem mostrar muito mais conteúdos, trazer todo aquele conteúdo extra pode impactar o desempenho de forma negativa.
-   Elimine o máximo de variáveis de teste possíveis.
    -   Desligue aplicativos em segundo plano no dispositivo de teste. Para fazer isso, no Windows, selecione **Configurações** no menu Iniciar &gt; **Personalização** &gt; **tela de bloqueio**. Selecione cada aplicativo ativo e selecione **Nenhum**.
    -   Compile o seu aplicativo a códigos nativos, construindo-o em configurações de versão antes de implantá-lo no dispositivo de teste.
    -   Para garantir que a manutenção automática não afeta o desempenho do dispositivo de teste, acione-o manualmente e aguarde a conclusão. No Windows, no menu Iniciar, procure **Segurança e Manutenção**. Na área de **Manutenção**, em **Manutenção Automática**, selecione **Iniciar manutenção** e aguarde o status sair de **Manutenção em progresso**.
    -   Execute o aplicativo várias vezes para ajudar a eliminar variáveis de teste aleatórias e a garantir medições consistentes.
-   Teste a disponibilidade de energia reduzida. O dispositivo dos seus usuários pode ter significativamente menos energia que a sua máquina de desenvolvimento. O Windows foi projetado para dispositivos de baixo consumo de energia, como dispositivos móveis. Os aplicativos que são executados na plataforma devem garantir um bom desempenho nesses dispositivos. Como aprendizado, espere que um dispositivo de baixa energia execute em cerca de um quarto da velocidade de um computador desktop e defina suas metas de acordo.
-   Use uma combinação de ferramentas como Microsoft Visual Studio e Windows Performance Analyzer para medir o desempenho do aplicativo. O Visual Studio foi projetado para fornecer análises centradas no aplicativo, como vinculação de código-fonte. O Windows Performance Analyzer foi projetado para fornecer análises centradas no sistema, como o fornecimento de informações do sistema, informações sobre eventos de manipulação de toque e informações sobre E/S (entrada e saída) de disco e custo da GPU (unidade de processamento gráfico). Ambas ferramentas oferecem exportação e captura de rastreamento e podem reabrir rastreamentos post-mortem e compartilhados.
-   Antes de enviar o seu aplicativo para a Loja para certificação, incorpore aos seus planos de teste os casos de teste relacionados ao desempenho, como descrito na seção "Testes de desempenho" de [Windows App Certification Kit tests](windows-app-certification-kit-tests.md) e na seção "Desempenho e estabilidade" de [Windows Store app test cases](https://msdn.microsoft.com/library/windows/apps/Dn275879).

Para obter mais informações, consulte esses recursos e ferramentas de criação de perfil.

-   [Windows Performance Analyzer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh162945.aspx)
-   [Analyze performance using Visual Studio diagnostic tools](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh696636.aspx)
-   A sessão //build/ [XAML Performance](https://channel9.msdn.com/Events/Build/2015/3-698)
-   A sessão //build/ [New XAML Tools in Visual Studio 2015](https://channel9.msdn.com/Events/Build/2015/2-697)

## Responder aos resultados do teste de desempenho

Depois de analisar os resultados do teste de desempenho, determine se são necessárias alterações, por exemplo:

-   Você deve alterar alguma das decisões de design do seu aplicativo ou otimizar seu código?
-   Você deve adicionar, remover ou alterar alguma instrumentação no código?
-   Você deve repensar alguma das suas metas de performance?

Se mudanças forem necessárias, faça-as e, depois, volte a instrumentar ou a testar e repita.

## Otimizando

Otimize somente os caminhos de código críticos de desempenho no seu aplicativo: nos quais se gasta mais tempo. A criação do perfil lhe dirá quais são eles. Frequentemente, há uma compensação entre a criação de um software que siga boas práticas de design e a geração de código que opere com o máximo de otimização. Geralmente, é melhor priorizar a produtividade do desenvolvedor e um bom design de software em áreas onde o desempenho não é uma preocupação.



<!--HONumber=Mar16_HO1-->


