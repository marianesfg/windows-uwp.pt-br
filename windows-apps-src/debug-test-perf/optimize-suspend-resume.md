---
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: Otimizar suspensão/retomada
description: Crie aplicativos da Plataforma Universal do Windows (UWP) que agilizem o uso do sistema de tempo de vida de processo para continuar de modo eficiente após suspensão ou término.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 610b6237071c9d7435ca167c1a89b4ef7c40b333
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "71339579"
---
# <a name="optimize-suspendresume"></a>Otimizar suspensão/retomada


Crie aplicativos da Plataforma Universal do Windows (UWP) que agilizem o uso do sistema de tempo de vida de processo para continuar de modo eficiente após suspensão ou término.

## <a name="launch"></a>Inicializar

Quando reativar um aplicativo após uma suspensão/encerramento, verifique se já se passou muito tempo. Nesse caso, considere a possibilidade de retornar à página de aterrissagem principal do aplicativo em vez de mostrar os dados obsoletos do usuário. Isso também resultará em uma inicialização mais rápida.

Durante a ativação, sempre verifique o PreviousExecutionState do parâmetro args do evento (por exemplo, para ativações iniciadas, verifique LaunchActivatedEventArgs.PreviousExecutionState). Se o valor for ClosedByUser ou NotRunning, não perca tempo restaurando o estado salvo anteriormente. Nesse caso, a coisa certa a fazer é oferecer uma experiência nova – e isso resultará em uma inicialização mais rápida.

Em vez de restaurar avidamente o estado salvo anteriormente, considere manter o controle desse estado e restaurá-lo somente sob demanda. Por exemplo, considere uma situação em que seu aplicativo foi suspenso anteriormente, salvou o estado para 3 páginas e, em seguida, foi encerrado. Após a reinicialização, se você decidir retornar o usuário à 3ª página, não restaure avidamente o estado para as 2 primeiras páginas. Em vez disso, mantenha esse estado e use-o somente depois que você constatar que precisa dele.

## <a name="while-running"></a>Durante a execução

Como prática recomendada, não aguarde o evento de suspensão e mantenha uma grande quantidade de estados. Em vez disso, seu aplicativo deve persistir incrementalmente quantidades menores de estados conforme é executado. Isso é especialmente importante para grandes aplicativos que correm o risco atingir o tempo limite durante a suspensão se tentar salvar tudo de uma vez.

No entanto, você precisa encontrar um bom equilíbrio entre salvamento incremental e desempenho de seu aplicativo durante a execução. É um bom equilíbrio é controlar incrementalmente os dados que foram alterados (e que, portanto, precisam ser salvos) – e usar o evento de suspensão para salvar de fato esses dados (o que é mais rápido do que salvar todos os dados ou examinando todo o estado do aplicativo para decidir o que salvar).

Não use a janela Activated ou eventos VisibilityChanged para decidir quando salvar o estado. Quando o usuário muda para outro aplicativo, a janela é desativada, mas o sistema aguarda um curto período (cerca de 10 segundos) antes de suspender o aplicativo. Isso é para proporcionar uma experiência mais responsiva caso o usuário volte para seu aplicativo rapidamente. Aguarde o evento de suspensão antes de executar a lógica de suspensão.

## <a name="suspend"></a>Suspender

Durante a suspensão, reduza o volume de memória do seu aplicativo. Se seu aplicativo usa menos memória enquanto é suspenso, todo o sistema será mais responsivo e menos aplicativos suspensos (incluindo o seu) serão encerrados. No entanto, equilibre isso com a necessidade de retomadas rápidas: não reduza o volume de memória demais a ponto de retardar a retomada consideravelmente enquanto seu aplicativo recarrega grandes quantidades de dados na memória.

Para aplicativos gerenciados, o sistema executará uma passagem de coleta de lixo depois que os manipuladores de suspensão do aplicativo forem concluídos. Tire proveito disso liberando referências a objetos que ajudam a reduzir o volume de memória do aplicativo enquanto ele está suspenso.

O ideal é que seu aplicativo conclusa a lógica de suspensão em menos de 1 segundo. Quanto mais rápido for a suspensão, melhor – isso resultará em uma experiência de usuário mais ágil para outros aplicativos e partes do sistema. Se necessário, sua lógica de suspensão pode demorar até 5 segundos em dispositivos desktop ou 10 segundos em dispositivos móveis. Se esses tempos forem excedidos, seu aplicativo será encerrado abruptamente. Você não quer que isso aconteça – pois se acontecer, quando o usuário voltar ao seu aplicativo, um novo processo será iniciado e a experiência parecerá muito mais lenta em comparação à retomada de um aplicativo suspenso.

## <a name="resume"></a>Retomar

A maioria dos aplicativos não precisa fazer nada de especial quando é retomada. Então, normalmente você não manipula esse evento. Alguns aplicativos usam a retomada para restaurar conexões que foram fechadas durante a suspensão ou para atualizar os dados que podem estar obsoletos. Em vez de fazer esse tipo de trabalho avidamente, projete seu aplicativo para iniciar essas atividades sob demanda. Isso resultará em uma experiência mais rápida quando o usuário retornar para um aplicativo suspenso, além de garantir que você realize somente o trabalho que o usuário necessita.

## <a name="avoid-unnecessary-termination"></a>Evitar o encerramento desnecessário

O sistema de tempo de vida de processo da UWP pode suspender ou encerrar um aplicativo por vários motivos. Esse processo é designado para retornar rapidamente um aplicativo ao estado em que estava quando suspenso ou encerrado. Quando bem feito, o usuário não perceberá que o aplicativo parou de ser executado. Aqui estão alguns truques que seu aplicativo UWP pode usar para ajudar o sistema a simplificar as transições no tempo de vida do aplicativo.

Um aplicativo pode ser suspenso quando o usuário o move para a tela de fundo ou quando o sistema entra no estado de baixo consumo de energia. Quando o aplicativo está sendo suspenso, ele aciona o evento de suspensão e tem até 5 segundos para salvar seus dados. Se o manipulador de eventos de suspensão do aplicativo não concluir dentro de 5 segundos, o sistema assume que o aplicativo parou de responder e o encerra. Um aplicativo encerrado precisa passar pelo longo processo de inicialização novamente em vez de ser imediatamente carregado na memória quando um usuário alterna para ele.

### <a name="serialize-only-when-necessary"></a>Serialize somente quando for necessário

Muitos aplicativos serializam todos os dados durante a suspensão. Contudo, se você precisa armazenar apenas uma pequena quantidade de dados de configuração de aplicativo, deve usar o armazenamento [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) em vez de seriar os dados. Use a serialização para quantidades maiores de dados e para dados que não sejam de configuração.

Depois de serializar seus dados, evite serializá-los novamente, se não foram alterados. Serializar e salvar os dados custa tempo extra, além do tempo adicional para ler e desserializar os dados quando o aplicativo é ativado outra vez. Em vez disso, recomendamos que o aplicativo determine se o seu estado realmente mudou e, se assim for, serialize e desserialize somente os dados alterados. Uma boa maneira de garantir que isso aconteça é serializar os dados periodicamente, em segundo plano, quando eles são alterados. Quando você usa essa técnica, tudo o que precisar ser serializado em suspensão já terá sido salvo, e não haverá trabalho a ser feito, fazendo com que o aplicativo seja suspenso rapidamente.

### <a name="serializing-data-in-c-and-visual-basic"></a>Serializando dados em C# e Visual Basic

As opções disponíveis de tecnologia de serialização para aplicativos .NET são as classes [**System.Xml.Serialization.XmlSerializer**](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer), [**System.Runtime.Serialization.DataContractSerializer**](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.datacontractserializer) e [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer).

A partir de uma perspectiva de desempenho, é recomendável usar a classe [**XmlSerializer**](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer). O **XmlSerializer** tem os menores tempos de serialização e desserialização, além de manter um baixo volume de memória. O **XmlSerializer** tem poucas dependências na estrutura .NET, o que significa que comparado a outras tecnologias de serialização, menos módulos precisam ser carregados em seu aplicativo para usar o **XmlSerializer**.

O [**DataContractSerializer**](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.datacontractserializer) facilita a serialização de classes personalizadas, embora tenha um impacto de desempenho maior do que o **XmlSerializer**. Caso precise de desempenho melhor, considere mudar. No geral, você não deve carregar mais do que um serializador e deve preferir **XmlSerializer**, a menos que precise dos recursos de outro serializador.

### <a name="reduce-memory-footprint"></a>Reduzir o volume de memória

O sistema tenta manter tantos aplicativos suspensos na memória quanto possível para que os usuários possam alternar entre eles de modo rápido e confiável. Quando um aplicativo está suspenso e permanece na memória do sistema, pode ser trazido rapidamente para o primeiro plano para o usuário interagir com ele, sem ter que exibir uma tela inicial ou executar uma operação de carregamento longa. Se não houver recursos suficientes para manter um aplicativo na memória, o aplicativo será encerrado. Isso torna o gerenciamento de memória importante por dois motivos:

-   Liberar o máximo de memória possível na suspensão minimiza a chance de que seu aplicativo seja encerrado por causa de falta de recursos enquanto estiver suspenso.
-   A redução da quantidade geral de memória usada por seu aplicativo reduz a chance de que outros aplicativos sejam encerrados enquanto estiverem suspensos.

### <a name="release-resources"></a>Liberar recursos

Determinados objetos, como arquivos e dispositivos, ocupam uma grande quantidade de memória. Recomendamos que, durante a suspensão, um aplicativo libere manipuladores para esses objetos e os recriem quando necessário. Esse também é um bom momento para limpar qualquer cache que não será válido quando o aplicativo for retomado. Uma etapa adicional executada pela estrutura XAML em seu nome para aplicativos C# e Visual Basic é a coleta de lixo, se for necessária. Dessa forma, os objetos que não são mais mencionados no código do aplicativo são liberados.

## <a name="resume-quickly"></a>Retomar rapidamente

Um aplicativo de suspensão pode ser retomado quando o usuário o move para o primeiro plano ou quando o sistema sai de um estado de baixo consumo de energia. Quando um aplicativo é retomado do estado suspenso, ele continua de onde estava quando foi suspenso. Não há perda de dados de aplicativo porque eles foram armazenados na memória, mesmo que o aplicativo tenha sido suspenso por um longo período de tempo.

A maioria dos aplicativos precisa manipular o evento [**Resuming**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.resuming). Quando seu aplicativo é retomado, variáveis e objetos têm exatamente o mesmo estado que tinham quando o aplicativo foi suspenso. Manipule o evento **Resuming** apenas se precisar atualizar dados ou objetos que possam ter sido alterados entre o tempo que seu aplicativo foi suspenso e quando ele foi retomado, como: conteúdo (por exemplo, dados de feed atualizados), conexões de rede que possam ter ficado obsoletas ou se você precisar readquirir acesso a um dispositivo (por exemplo, uma webcam).

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para suspensão e retomada de aplicativos](https://docs.microsoft.com/windows/uwp/launch-resume/index)
 

 




