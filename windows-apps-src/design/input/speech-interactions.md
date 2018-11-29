---
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Interações de controle por voz
ms.assetid: 646DB3CE-FA81-4727-8C21-936C81079439
label: Speech interactions
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dfd829881892eece08c30fcd665bdf21a1f43471
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7991498"
---
# <a name="speech-interactions"></a>Interações de controle por voz

Integre o reconhecimento de fala e a conversão de texto em fala (também conhecida como TTS ou sintetização de voz) diretamente à experiência do usuário do seu aplicativo.

**Reconhecimento de fala:** converte palavras faladas pelo usuário em texto para entrada de formulário e ditado de texto, para especificar uma ação ou um comando e realizar tarefas. As gramáticas predefinidas para ditado de texto livre e pesquisa na Web e as gramáticas personalizadas criadas usando SRGS (Especificação de Gramática de Reconhecimento de Fala) Versão 1.0 têm suporte.

**TTS:** usa um mecanismo de sintetização de voz (voz) para converter uma cadeia de caracteres de texto em palavras faladas. A cadeia de caracteres de entrada pode ser texto básico e simples ou SSML (Linguagem de Marcação de Sintetização de Voz) mais complexa. A SSML fornece uma forma padrão de controlar as características da saída de fala, como pronúncia, volume, tom, taxa ou velocidade e ênfase.

**Outros componentes relacionados à fala:** Em aplicativos do Windows, a 
**Cortana** usa comandos de voz personalizados (falados ou digitados) para iniciar seu aplicativo em primeiro plano (o aplicativo recebe o foco, como se ele tivesse sido inicializado no menu Iniciar) ou ativá-lo como um serviço em segundo plano (a **Cortana** retém o foco, mas fornece resultados do aplicativo). Consulte as [Diretrizes para comando de voz da Cortana (VCD)](https://docs.microsoft.com/en-us/cortana/voice-commands/vcd) se você estiver expondo a funcionalidade do aplicativo na interface do usuário da **Cortana**.

## <a name="speech-interaction-design"></a>Design de interação de fala

Cuidadosamente projetado e implementado, o controle por voz pode ser um modo robusto e interessante para as pessoas interagirem com seu aplicativo, complementando ou até mesmo substituindo o teclado, o mouse, o toque e os gestos.

Essas diretrizes e recomendações descrevem como integrar melhor o reconhecimento de fala e a TTS à experiência de interação do seu aplicativo.

Se você estiver considerando oferecer suporte a interações por fala em seu aplicativo:

-   Quais ações podem ser executadas por meio da fala? Um usuário pode navegar entre páginas, invocar comandos ou inserir dados como campos de texto, anotações rápidas ou mensagens longas?
-   A entrada de fala é uma boa opção para concluir uma tarefa?
-   Como um usuário sabe quando a entrada de fala está disponível?
-   O aplicativo tem a capacidade de sempre escutar, ou o usuário precisa executar uma ação para que o aplicativo entre no modo de escuta?
-   Quais frases iniciam uma ação ou um comportamento? As frases e as ações precisam ser enumeradas na tela?
-   Telas de prompt, de confirmação e de desambiguidade ou TTS são necessárias?
-   Qual é o diálogo de interação entre o aplicativo e o usuário?
-   Um vocabulário personalizado ou restrito é necessário (como medicina, ciências ou localidade) para o contexto do seu aplicativo?
-   É necessária conectividade de rede?

## <a name="text-input"></a>Entrada de texto

A fala para entrada de texto pode variar de forma curta (única palavra ou frase) a forma longa (ditado contínuo). A entrada de forma curta deve ter menos de 10 segundos, enquanto a sessão de entrada de forma longa pode ter até dois minutos. (A entrada de forma longa pode ser reiniciada sem a intervenção do usuário para dar a impressão de ditado contínuo.)

Você deve fornecer uma indicação visual para mostrar que o reconhecimento de fala é compatível e está disponível para o usuário e se o usuário precisa ativá-lo. Por exemplo, um botão da barra de comandos com um glifo de microfone (consulte [Barras de comandos](../controls-and-patterns/app-bars.md)) pode ser usado para mostrar a disponibilidade e o estado.

Forneça feedback de reconhecimento contínuo para minimizar qualquer aparente falta de resposta enquanto o reconhecimento está sendo executado.

Permita que os usuários revisem o texto de reconhecimento usando a entrada do teclado, prompts de desambiguação, sugestões ou reconhecimento de fala adicional.

Pare o reconhecimento se for detectada entrada de um dispositivo que não seja o reconhecimento de fala, como toque ou teclado. Isso provavelmente indica que o usuário passou para outra tarefa, como corrigir o texto do reconhecimento ou interagir com outros campos de formulário.

Especifique o período de tempo durante o qual a ausência de entrada de fala indica que o reconhecimento terminou. Não reinicie automaticamente o reconhecimento após esse período de tempo, pois ele geralmente indica que o usuário parou de interagir com seu aplicativo.

Desabilite todas as interfaces do usuário de reconhecimento contínuo e encerre a sessão de reconhecimento se uma conexão de rede não estiver disponível. O reconhecimento contínuo requer uma conexão de rede.

## <a name="commanding"></a>Execução de comandos

A entrada de fala pode iniciar ações, invocar comandos e realizar tarefas.

Se houver espaço, considere a possibilidade de exibir as respostas com suporte para o contexto atual do aplicativo, com exemplos de entrada válida. Isso reduz as possíveis respostas que seu aplicativo precisará processar e também elimina a confusão para o usuário.

Tente formular suas perguntas de forma que elas obtenham a resposta mais específica possível. Por exemplo, "O que você deseja fazer hoje?" é uma pergunta muito aberta e exige uma definição de gramática muito extensa por causa da variedade de respostas. Como alternativa, "Você gostaria de jogar ou ouvir música?" restringe a resposta a uma de duas respostas válidas com uma definição de gramática correspondentemente pequena. Uma gramática pequena é muito mais fácil de criar e gera resultados de reconhecimento muito mais precisos.

Solicite a confirmação do usuário quando a confiança no reconhecimento de fala for baixa. Se a intenção do usuário não for clara, é melhor obter esclarecimento do que iniciar uma ação não intencional.

Você deve fornecer uma indicação visual para mostrar que o reconhecimento de fala é compatível e está disponível para o usuário e se o usuário precisa ativá-lo. Por exemplo, um botão da barra de comandos com um glifo de microfone (consulte [Diretrizes para barras de comandos](../controls-and-patterns/app-bars.md)) pode ser usado para mostrar a disponibilidade e o estado.

Se a opção de reconhecimento de fala normalmente fica fora da exibição, considere a possibilidade de exibir um indicador de estado na área de conteúdo do aplicativo.

Se o reconhecimento for iniciado pelo usuário, considere usar a experiência interna de reconhecimento para manter a consistência. A experiência interna inclui telas personalizáveis com prompts, exemplos, desambiguações, confirmações e erros.

As telas variam de acordo com as restrições especificadas:

-   Gramática predefinida (ditado ou pesquisa na Web)

    -   A tela **Ouvindo**.
    -   A tela **Pensando** .
    -   A tela **Ouvi você dizer** ou a tela de erro.
-   Lista de palavras ou frases, ou um arquivo de gramática SRGS

    -   A tela **Ouvindo**.
    -   A tela **Você disse**, se o que o usuário disse puder ser interpretado como mais de um resultado potencial.
    -   A tela **Ouvi você dizer** ou a tela de erro.

Na tela **Ouvindo**, você pode:

-   Personalizar o texto do cabeçalho.
-   Fornecer um texto de exemplo do que o usuário pode dizer.
-   Especificar se a tela **Ouvi você dizer** é mostrada.
-   Ler a cadeia de caracteres reconhecida para o usuário na tela **Ouvi você dizer**.

Veja um exemplo do fluxo de reconhecimento interno de um reconhecedor de fala que utiliza uma restrição definida pela SRGS. Nesse exemplo, o reconhecimento de fala foi bem-sucedido.

![tela de reconhecimento inicial para uma restrição baseada em um arquivo de gramática sgrs](images/speech/speech-listening-initial.png)

![reconhecimento intermediário para uma restrição baseada em um arquivo de gramática sgrs](images/speech/speech-listening-intermediate.png)

![tela de reconhecimento final para uma restrição baseada em um arquivo de gramática sgrs](images/speech/speech-listening-complete.png)

## <a name="always-listening"></a>Sempre ouvindo

Seu aplicativo pode ouvir e reconhecer a entrada de fala assim que ele é iniciado, sem a intervenção do usuário.

Você deve personalizar as restrições da gramática com base no contexto do aplicativo. Isso mantém a experiência de reconhecimento de fala muito direcionada e relevante para a tarefa atual, além de minimizar erros.

## <a name="what-can-i-say"></a>"O que posso falar?"

Quando a entrada de fala está habilitada, é importante ajudar os usuários a descobrir o que exatamente pode ser entendido e quais ações podem ser executadas.

Se o reconhecimento de fala for habilitado pelo usuário, considere usar a barra de comandos ou um comando de menu para mostrar todas as palavras e frases com suporte no contexto atual.

Se o reconhecimento de fala estiver sempre ativado, considere incluir a frase "O que posso falar?" em todas as páginas. Quando o usuário falar essa frase, exiba todas as palavras e frases com suporte no contexto atual. Usar essa frase fornece uma maneira consistente para os usuários descobrirem os recursos de fala no sistema.

## <a name="recognition-failures"></a>Falhas no reconhecimento

Haverá falhas no reconhecimento de fala. As falhas ocorrem quando a qualidade do áudio é ruim, quando somente uma parte da frase é reconhecida, ou quando nenhuma entrada é detectada.

Manipule falhas com delicadeza, ajude o usuário a entender por que houve falha no reconhecimento e restabeleça.

Seu aplicativo deve informar ao usuário que ele não foi compreendido e que precisa tentar novamente.

Considere fornecer exemplos de uma ou mais frases com suporte. É provável que o usuário repita uma frase sugerida, o que aumenta o sucesso do reconhecimento.

Você deve exibir uma lista de possíveis correspondências de onde o usuário fará a seleção. Isso pode ser muito mais eficiente do que passar pelo processo de reconhecimento novamente.

Você sempre deve oferecer suporte a tipos de entrada alternativos, o que é especialmente útil para lidar com falhas repetidas no reconhecimento. Por exemplo, você pode sugerir que o usuário tente usar um teclado, toque ou um mouse para selecionar em uma lista de possíveis correspondências.

Use a experiência interna de reconhecimento de fala porque ela inclui telas que informam o usuário de que o reconhecimento não teve sucesso e permite que ele faça outra tentativa de reconhecimento.

Ouça e tente corrigir problemas na entrada de áudio. O reconhecedor de fala pode detectar problemas com a qualidade do áudio que podem afetar negativamente a precisão do reconhecimento de fala. Você pode usar as informações fornecidas pelo reconhecedor de fala para informar o usuário sobre o problema e permitir que ele execute uma ação corretiva, se possível. Por exemplo, se a configuração de volume do microfone for muito baixa, você pode solicitar que o usuário fale mais alto ou aumente o volume.

## <a name="constraints"></a>Restrições

Restrições, ou gramáticas, definem as palavras e frases faladas a que o reconhecedor de fala pode fazer correspondência. Você pode especificar uma das gramáticas de serviço Web predefinidas ou criar uma gramática personalizada que é instalada com seu aplicativo.

### <a name="predefined-grammars"></a>Gramáticas pré-definidas

Ditado predefinido e gramáticas de pesquisa na Web fornecem o reconhecimento de fala de seu aplicativo sem precisar que você crie uma gramática. Ao utilizar essas gramáticas, o reconhecimento de fala é realizado por um serviço Web remoto, e os resultados são retornados ao dispositivo.

-   A gramática de ditado de texto livre padrão pode reconhecer a maioria das palavras e frases que um usuário pode dizer em um determinado idioma e é otimizada para reconhecer frases curtas. O ditado de texto livre é útil quando você não deseja limitar os tipos de coisas que um usuário pode dizer. Os usos típicos incluem criação de notas ou ditado de conteúdo para uma mensagem.
-   A gramática de pesquisa na Web, assim como uma gramática de ditado, contém um grande número de palavras e frases que um usuário pode dizer. No entanto, ela é otimizada para reconhecer termos que as pessoas normalmente usam ao pesquisar na Web.

> [!NOTE]
> Uma vez que as gramáticas de pesquisa na Web e de ditado predefinidas podem ser grandes e online (não no dispositivo), o desempenho pode não ser tão rápido quanto o das gramáticas personalizadas instaladas no dispositivo.

Essas gramáticas predefinidas podem ser usadas para reconhecer até 10 segundos de entrada de fala e não exigem nenhum esforço de criação de sua parte. No entanto, elas exigem conexão com uma rede.

### <a name="custom-grammars"></a>Gramáticas personalizadas

Uma gramática personalizada é projetada e criada por você e instalada com o seu aplicativo. O reconhecimento de fala usando uma restrição personalizada é realizado no dispositivo.

-   Restrições de lista programática fornecem uma abordagem leve para criar gramáticas simples usando uma lista de palavras ou frases. Uma lista de restrições funciona bem para o reconhecimento de frases curtas e distintas. Especificar explicitamente todas as palavras em uma gramática também melhora a precisão do reconhecimento, porque o mecanismo de reconhecimento de fala deve processar somente a fala para confirmar uma correspondência. A lista também pode ser atualizada programaticamente.
-   Uma gramática SRGS é um documento estático que, ao contrário de uma restrição de lista programática, usa o formato XML definido pela [SRGS Versão 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302). Uma gramática SRGS oferece maior controle sobre a experiência de reconhecimento de fala, permitindo a você capturar diversos significados semânticos em um único reconhecimento.

    Veja algumas dicas para criar gramáticas SRGS:

    -   Cada gramática deve ser pequena. Gramáticas com poucas frases tendem a oferecer um reconhecimento mais preciso do que gramáticas maiores com muitas frases. É preferível ter várias gramáticas menores para cenários específicos a ter uma única gramática para o aplicativo inteiro.
    -   Permita que os usuários saibam o que dizer para cada contexto do aplicativo e habilitem e desabilitem gramáticas conforme necessário.
    -   Projete cada gramática para que os usuários possam falar um comando de diversas maneiras. Por exemplo, você pode usar a regra **GARBAGE** para fazer correspondência da entrada de fala que sua gramática não define. Isso permite que os usuários digam palavras adicionais que não tenham significado para seu aplicativo. Por exemplo, "me dê", "e", "hum", "talvez", etc.
    -   Use o elemento [sapi:subset](http://msdn.microsoft.com/library/windowsphone/design/jj572474.aspx) para ajudar na correspondência da entrada de fala. Essa é uma extensão da Microsoft para a especificação SRGS para ajudar na correspondência de frases parciais.
    -   Evite definir frases que contenham apenas uma sílaba na sua gramática. O reconhecimento tende a ser mais preciso em frases com duas ou mais sílabas.
    -   Evite frases que pareçam iguais. Por exemplo, palavras como "cumprimento" e "comprimento" podem confundir o mecanismo de reconhecimento e resultar em reconhecimento pouco preciso.

> [!NOTE]
> O tipo de restrição que você usa depende da complexidade da experiência de reconhecimento que você deseja criar. Qualquer tipo pode ser a melhor escolha para uma tarefa específica de reconhecimento, e você pode encontrar usos para todos os tipos de restrição em seu aplicativo.

### <a name="custom-pronunciations"></a>Pronúncias personalizadas

Se seu aplicativo tiver vocabulário especializado com palavras incomuns ou fictícias, ou ainda, palavras com pronúncias incomuns, você poderá melhorar o desempenho do reconhecimento para essas palavras ao definir pronúncias personalizadas.

Para uma pequena lista de palavras e frases, ou uma lista de palavras e frases usadas com pouca frequência, você pode criar pronúncias personalizadas em uma gramática SRGS. Consulte [elemento token](http://msdn.microsoft.com/library/windowsphone/design/hh361600.aspx) para obter mais informações.

Para listas maiores de palavras e frases, ou palavras e frases usadas com frequência, você pode criar documentos separados de léxico de pronúncias. Consulte [Sobre léxicos e alfabetos fonéticos](http://msdn.microsoft.com/library/windowsphone/design/hh361646.aspx) para obter mais informações.

## <a name="testing"></a>Testes

Teste a precisão do reconhecimento de fala e qualquer interface do usuário de suporte com o público-alvo do seu aplicativo. Esta é a melhor maneira de determinar a eficiência da experiência de interação de fala no seu aplicativo. Por exemplo, os usuários estão obtendo resultados de reconhecimento insatisfatórios porque seu aplicativo não está ouvindo uma frase comum?

Modifique a gramática para oferecer suporte a essa frase ou forneça aos usuários uma lista de frases com suporte. Se você já fornece a lista de frases com suporte, certifique-se de que ela seja facilmente detectável.

## <a name="text-to-speech-tts"></a>Conversão de texto em fala (TTS)

A TTS gera saída de fala de texto sem formatação nem SSML.

Tente elaborar solicitações educadas e incentivadoras.

Considere se você deve ler cadeias de caracteres de texto longas. Uma coisa é ouvir a uma mensagem de texto, mas outra bem diferente é ouvir uma longa lista de resultados de pesquisa que são difíceis de serem lembradas.

Você deve fornecer controles de mídia para permitir que os usuários pausem, ou parem, a TTS.

Você deve ouvir todas as cadeias de caracteres da TTS para garantir que sejam inteligíveis e soem naturais.

-   Encadear uma sequência incomum de palavras ou falar números de peças ou pontuação podem tornar uma frase inteligível.
-   A fala pode parecer artificial quando a prosódia ou a cadência é diferente de como um falante nativo diria a frase.

Os dois problemas podem ser atendidos com SSML em vez de texto sem formatação como entrada para o sintetizador de voz. Para obter mais informações sobre SSML, consulte [Usar SSML para controlar a fala sintetizada](http://msdn.microsoft.com/library/windowsphone/design/hh378454.aspx) e [Referência da linguagem de marcação da sintetização de voz](http://msdn.microsoft.com/library/windowsphone/design/hh378377.aspx).

## <a name="other-articles-in-this-section"></a>Outros artigos nesta seção 

| Tópico | Descrição |
| --- | --- |
| [Reconhecimento de fala](speech-recognition.md) | Use o reconhecimento de fala para fornecer entrada, especificar uma ação ou um comando e realizar tarefas. |
| [Especificar o idioma do reconhecedor de fala](specify-the-speech-recognizer-language.md) | Saiba como selecionar um idioma instalado para usá-lo para reconhecimento de fala. |
| [Definir restrições de reconhecimento personalizadas](define-custom-recognition-constraints.md) | Saiba como definir e usar restrições personalizadas para reconhecimento de fala. |
| [Habilitar o ditado contínuo](enable-continuous-dictation.md) |Saiba como capturar e reconhecer entrada de fala de ditado contínuo de formato longo. |
| [Gerenciar problemas com entrada de áudio](manage-issues-with-audio-input.md) | Saiba como gerenciar problemas com precisão do reconhecimento de fala causados pela qualidade da entrada de áudio. |
| [Definir tempos limite de reconhecimento de fala](set-speech-recognition-timeouts.md) | Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala. |

## <a name="related-articles"></a>Artigos relacionados

* [Interações de controle por voz](https://msdn.microsoft.com/library/windows/apps/mt185614)
* [Interações da Cortana](https://msdn.microsoft.com/library/windows/apps/mt185598)

 **Exemplos**

* [Exemplo de reconhecimento de fala e sintetização de controle por voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 



