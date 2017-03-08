---
author: mcleblanc
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: "Otimizar suspensão/retomada"
description: "Crie apps da Plataforma Universal do Windows (UWP) que agilizem o uso do sistema de tempo de vida de processo para continuar de modo eficiente após suspensão ou término."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2771d7dae6df171855297ab988bbd00ecfa55d1d
ms.lasthandoff: 02/07/2017

---
# <a name="optimize-suspendresume"></a>Otimizar suspensão/retomada

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Crie apps da Plataforma Universal do Windows (UWP) que agilizem o uso do sistema de tempo de vida de processo para continuar de modo eficiente após suspensão ou término.

## <a name="launch"></a>Inicialização

Quando reativar um app após uma suspensão/encerramento, verifique se já se passou muito tempo. Nesse caso, considere a possibilidade de retornar à página de aterrissagem principal do app em vez de mostrar os dados obsoletos do usuário. Isso também resultará em uma inicialização mais rápida.

Durante a ativação, sempre verifique o PreviousExecutionState do parâmetro args do evento (por exemplo, para ativações iniciadas, verifique LaunchActivatedEventArgs.PreviousExecutionState). Se o valor for ClosedByUser ou NotRunning, não perca tempo restaurando o estado salvo anteriormente. Nesse caso, a coisa certa a fazer é oferecer uma experiência nova – e isso resultará em uma inicialização mais rápida.

Em vez de restaurar avidamente o estado salvo anteriormente, considere manter o controle desse estado e restaurá-lo somente sob demanda. Por exemplo, considere uma situação em que seu app foi suspenso anteriormente, salvou o estado para 3 páginas e, em seguida, foi encerrado. Após a reinicialização, se você decidir retornar o usuário à 3ª página, não restaure avidamente o estado para as 2 primeiras páginas. Em vez disso, mantenha esse estado e use-o somente depois que você constatar que precisa dele.

## <a name="while-running"></a>Durante a execução

Como prática recomendada, não aguarde o evento de suspensão e mantenha uma grande quantidade de estados. Em vez disso, seu app deve persistir incrementalmente quantidades menores de estados conforme é executado. Isso é especialmente importante para grandes apps que correm o risco atingir o tempo limite durante a suspensão se tentar salvar tudo de uma vez.

No entanto, você precisa encontrar um bom equilíbrio entre salvamento incremental e desempenho de seu app durante a execução. É um bom equilíbrio é controlar incrementalmente os dados que foram alterados (e que, portanto, precisam ser salvos) – e usar o evento de suspensão para salvar de fato esses dados (o que é mais rápido do que salvar todos os dados ou examinando todo o estado do app para decidir o que salvar).

Não use a janela Activated ou eventos VisibilityChanged para decidir quando salvar o estado. Quando o usuário muda para outro app, a janela é desativada, mas o sistema aguarda um curto período (cerca de 10 segundos) antes de suspender o app. Isso é para proporcionar uma experiência mais responsiva caso o usuário volte para seu app rapidamente. Aguarde o evento de suspensão antes de executar a lógica de suspensão.

## <a name="suspend"></a>Suspender

Durante a suspensão, reduza o volume de memória do seu app. Se seu app usa menos memória enquanto é suspenso, todo o sistema será mais responsivo e menos apps suspensos (incluindo o seu) serão encerrados. No entanto, equilibre isso com a necessidade de retomadas rápidas: não reduza o volume de memória demais a ponto de retardar a retomada consideravelmente enquanto seu app recarrega grandes quantidades de dados na memória.

Para apps gerenciados, o sistema executará uma passagem de coleta de lixo depois que os manipuladores de suspensão do app forem concluídos. Tire proveito disso liberando referências a objetos que ajudam a reduzir o volume de memória do app enquanto ele está suspenso.

O ideal é que seu app conclusa a lógica de suspensão em menos de 1 segundo. Quanto mais rápido for a suspensão, melhor – isso resultará em uma experiência de usuário mais ágil para outros apps e partes do sistema. Se necessário, sua lógica de suspensão pode demorar até 5 segundos em dispositivos desktop ou 10 segundos em dispositivos móveis. Se esses tempos forem excedidos, seu app será encerrado abruptamente. Você não quer que isso aconteça – pois se acontecer, quando o usuário voltar ao seu app, um novo processo será iniciado e a experiência parecerá muito mais lenta em comparação à retomada de um app suspenso.

## <a name="resume"></a>Retomar

A maioria dos apps não precisa fazer nada de especial quando é retomada. Então, normalmente você não manipula esse evento. Alguns apps usam a retomada para restaurar conexões que foram fechadas durante a suspensão ou para atualizar os dados que podem estar obsoletos. Em vez de fazer esse tipo de trabalho avidamente, projete seu app para iniciar essas atividades sob demanda. Isso resultará em uma experiência mais rápida quando o usuário retornar para um app suspenso, além de garantir que você realize somente o trabalho que o usuário necessita.

## <a name="avoid-unnecessary-termination"></a>Evitar o encerramento desnecessário

O sistema de tempo de vida de processo da UWP pode suspender ou encerrar um app por vários motivos. Esse processo é designado para retornar rapidamente um app ao estado em que estava quando suspenso ou encerrado. Quando bem feito, o usuário não perceberá que o app parou de ser executado. Aqui estão alguns truques que seu app UWP pode usar para ajudar o sistema a simplificar as transições no tempo de vida do app.

Um app pode ser suspenso quando o usuário o move para a tela de fundo ou quando o sistema entra no estado de baixo consumo de energia. Quando o app está sendo suspenso, ele aciona o evento de suspensão e tem até 5 segundos para salvar seus dados. Se o manipulador de eventos de suspensão do app não concluir dentro de 5 segundos, o sistema assume que o app parou de responder e o encerra. Um app encerrado precisa passar pelo longo processo de inicialização novamente em vez de ser imediatamente carregado na memória quando um usuário alterna para ele.

### <a name="serialize-only-when-necessary"></a>Serialize somente quando for necessário

Muitos apps serializam todos os dados durante a suspensão. Contudo, se você precisa armazenar apenas uma pequena quantidade de dados de configuração de app, deve usar o armazenamento [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) em vez de seriar os dados. Use a serialização para quantidades maiores de dados e para dados que não sejam de configuração.

Depois de serializar seus dados, evite serializá-los novamente, se não foram alterados. Serializar e salvar os dados custa tempo extra, além do tempo adicional para ler e desserializar os dados quando o app é ativado outra vez. Em vez disso, recomendamos que o app determine se o seu estado realmente mudou e, se assim for, serialize e desserialize somente os dados alterados. Uma boa maneira de garantir que isso aconteça é serializar os dados periodicamente, em segundo plano, quando eles são alterados. Quando você usa essa técnica, tudo o que precisar ser serializado em suspensão já terá sido salvo, e não haverá trabalho a ser feito, fazendo com que o app seja suspenso rapidamente.

### <a name="serializing-data-in-c-and-visual-basic"></a>Serializando dados em C# e Visual Basic

As opções disponíveis de tecnologia de serialização para apps .NET são as classes [**System.Xml.Serialization.XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx), [**System.Runtime.Serialization.DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) e [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.json.datacontractjsonserializer.aspx).

A partir de uma perspectiva de desempenho, é recomendável usar a classe [**XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx). O **XmlSerializer** tem os menores tempos de serialização e desserialização, além de manter um baixo volume de memória. O **XmlSerializer** tem poucas dependências na estrutura .NET, o que significa que comparado a outras tecnologias de serialização, menos módulos precisam ser carregados em seu app para usar o **XmlSerializer**.

[**DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) facilita a serialização de classes personalizadas, embora tenha um impacto de desempenho maior do que **XmlSerializer**. Caso precise de desempenho melhor, considere mudar. No geral, você não deve carregar mais do que um serializador e deve preferir **XmlSerializer**, a menos que precise dos recursos de outro serializador.

### <a name="reduce-memory-footprint"></a>Reduzir o volume de memória

O sistema tenta manter tantos apps suspensos na memória quanto possível para que os usuários possam alternar entre eles de modo rápido e confiável. Quando um app está suspenso e permanece na memória do sistema, pode ser trazido rapidamente para o primeiro plano para o usuário interagir com ele, sem ter que exibir uma tela inicial ou executar uma operação de carregamento longa. Se não houver recursos suficientes para manter um app na memória, o app será encerrado. Isso torna o gerenciamento de memória importante por dois motivos:

-   Liberar o máximo de memória possível na suspensão minimiza a chance de que seu app seja encerrado por causa de falta de recursos enquanto estiver suspenso.
-   A redução da quantidade geral de memória usada por seu app reduz a chance de que outros apps sejam encerrados enquanto estiverem suspensos.

### <a name="release-resources"></a>Liberar recursos

Determinados objetos, como arquivos e dispositivos, ocupam uma grande quantidade de memória. Recomendamos que, durante a suspensão, um app libere manipuladores para esses objetos e os recriem quando necessário. Esse também é um bom momento para limpar qualquer cache que não será válido quando o app for retomado. Uma etapa adicional executada pela estrutura XAML em seu nome para apps C# e Visual Basic é a coleta de lixo, se for necessária. Dessa forma, os objetos que não são mais mencionados no código do app são liberados.

## <a name="resume-quickly"></a>Retomar rapidamente

Um app de suspensão pode ser retomado quando o usuário o move para o primeiro plano ou quando o sistema sai de um estado de baixo consumo de energia. Quando um app é retomado do estado suspenso, ele continua de onde estava quando foi suspenso. Não há perda de dados de app porque eles foram armazenados na memória, mesmo que o app tenha sido suspenso por um longo período de tempo.

A maioria dos apps precisa manipular o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/BR205859). Quando seu app é retomado, variáveis e objetos têm exatamente o mesmo estado que tinham quando o app foi suspenso. Manipule o evento **Resuming** apenas se precisar atualizar dados ou objetos que possam ter sido alterados entre o tempo que seu app foi suspenso e quando ele foi retomado, como: conteúdo (por exemplo, dados de feed atualizados), conexões de rede que possam ter ficado obsoletas ou se você precisar readquirir acesso a um dispositivo (por exemplo, uma webcam).

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para suspensão e retomada de app](https://msdn.microsoft.com/library/windows/apps/Hh465088)
 

 





