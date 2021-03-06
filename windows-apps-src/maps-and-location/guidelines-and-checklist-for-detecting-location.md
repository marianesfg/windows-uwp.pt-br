---
Description: Este tópico descreve as diretrizes de desempenho para aplicativos que exigem acesso ao local de um usuário.
title: Diretrizes de aplicativos com reconhecimento de local
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, localização, mapa, localização geográfica
ms.localizationpriority: medium
ms.openlocfilehash: 575974db003048c6a248feab32ddb6c2c8f4f7f7
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606315"
---
# <a name="guidelines-for-location-aware-apps"></a>Diretrizes de aplicativos com reconhecimento de local




**APIs importantes**

-   [**Geolocalização**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation)
-   [**Geolocalizador**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)

Este tópico descreve as diretrizes de desempenho para aplicativos que exigem acesso ao local de um usuário.

## <a name="recommendations"></a>Recomendações


-   Comece a usar o objeto de local somente quando o aplicativo exigir os dados de localização.

    Chame [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) antes de acessar o local do usuário. Nesse momento, seu aplicativo deve estar em primeiro plano, e **RequestAccessAsync** deve ser chamado do thread da interface do usuário. Até que o usuário conceda permissão para a localização a seu aplicativo, o aplicativo não pode acessar os dados de localização.

-   Se a localização não for essencial para o seu aplicativo, não a acesse até o usuário tentar concluir uma tarefa que a exige. Por exemplo, se um aplicativo de rede social tiver um botão para "Fazer check-in com a minha localização", o aplicativo não deverá acessar a localização até o usuário clicar no botão. Não há problema em acessar a localização imediatamente se isso for necessário para a função principal de seu aplicativo.

-   O primeiro uso do objeto [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) deve ser feito no thread principal da interface do usuário do aplicativo em primeiro plano, para acionar a solicitação de consentimento ao usuário. O primeiro uso do **Geolocator** pode ser a primeira chamada ao [**getGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) ou o primeiro registro de um manipulador para o evento [**positionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged).

-   Informe o usuário sobre como os dados de localização serão usados.
-   Forneça a interface do usuário para permitir que o usuário atualize manualmente sua localização.
-   Exiba uma barra ou toque de progresso enquanto aguarda para obter os dados da localização. <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   Mostre mensagens de erro ou caixas de diálogo adequadas quando os serviços de localização estiverem desabilitados ou não estiverem disponíveis.

    Se as configurações de localização não permitirem que seu aplicativo acesse a localização do usuário, é recomendável fornecer um link conveniente para as **configurações de privacidade de localização** no aplicativo **Configurações**. Por exemplo, você pode usar um controle de hiperlink ou chamar o método [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) para iniciar o aplicativo **Configurações** no código usando o URI `ms-settings:privacy-location`. Para obter mais informações, consulte [Iniciar o aplicativo Configurações do Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-settings-app).

-   Limpe os dados de localização armazenados em cache e libere o [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) quando o usuário desabilitar o acesso às informações de localização.

    Libere o objeto [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) se o usuário desativar o acesso às informações de localização por meio de Configurações. O aplicativo receberá resultados **de\_acesso negado** para qualquer chamada à API de localização. No caso de o aplicativo salvar ou armazenar dados de localização em cache, limpe todos os dados do cache quando o usuário revogar o acesso às informações de localização. Ofereça um jeito alternativo de inserir manualmente as informações de localização quando os dados da localização não estiverem disponíveis via serviços de localização.

-   Forneça a interface do usuário para reabilitar os serviços de localização. Por exemplo, forneça um botão Atualizar que recrie a instância do objeto [**geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) e tente obter informações de localização novamente.

    Forneça em seu aplicativo a interface do usuário para reabilitar os serviços de localização—

    -   Se o usuário reabilitar o acesso à localização depois de desabilitá-lo, não haverá nenhuma notificação para o aplicativo. A propriedade [**status**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) não será modificada e não haverá nenhum evento [**statusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged). O aplicativo deve criar um novo objeto [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) e chamar [**getGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) para tentar obter os dados de localização atualizados ou assinar novamente eventos [**positionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged). Se o status indicar que a localização foi reabilitada, limpe qualquer interface do usuário por meio da qual o aplicativo tenha notificado o usuário anteriormente sobre a desabilitação dos serviços de localização e responda adequadamente ao novo status.
    -   O aplicativo também deve tentar obter novamente os dados de localização na ativação, ou quando o usuário tentar usar explicitamente a funcionalidade que exige informações de localização ou em qualquer outro momento adequado ao cenário.

**Desempenho**

-   Use solicitações de localização única se atualizações de localização não são necessárias. Por exemplo, um aplicativo que adiciona uma marca de localização a uma fotografia não precisa receber eventos de atualização de localização. Em vez disso, ele deve solicitar a localização usando [**getGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync), conforme descrito em [Obter a localização atual](https://docs.microsoft.com/windows/uwp/maps-and-location/get-location).

    Ao fazer uma única solicitação de localização, você deve definir os seguintes valores.

    -   Especifique a precisão solicitada por seu aplicativo definindo o [**DesiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) ou o [**DesiredAccuracyInMeters**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters). Consulte as recomendações sobre o uso desses parâmetros a seguir
    -   Defina o parâmetro de idade máxima de [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) para especificar há quanto tempo um local pode ter sido obtido para ser útil ao seu aplicativo. Se seu aplicativo puder usar uma posição que tenha alguns segundos ou minutos de idade, seu aplicativo poderá receber uma posição quase que imediatamente e contribuir para a economia de energia do dispositivo.
    -   Defina o parâmetro de tempo limite de [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync). Esse é o tempo que seu aplicativo pode esperar pelo retorno de uma posição ou de um erro. Você precisará descobrir as compensações entre precisão e capacidade de resposta ao usuário que o seu aplicativo precisa.
-   Use a sessão de localização contínua quando atualizações de posição frequentes forem necessárias. Use os eventos [**positionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged) e [**statusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged) para detectar movimento que ultrapassou um limite específico ou para atualizações contínuas de localização assim que elas ocorrerem.

    Ao solicitar atualizações de localização, você pode considerar conveniente especificar a precisão requerida por seu aplicativo definindo o [**DesiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) ou o [**DesiredAccuracyInMeters**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters). Você também deve definir a frequência com que são necessárias as atualizações de localização, utilizando o [**MovementThreshold**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) ou o [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval).

    -   Especifique o limite de movimento. Alguns aplicativos precisam de atualizações de localização somente quando o usuário se moveu por uma longa distância. Por exemplo, um aplicativo que fornece notícias locais ou atualizações do clima talvez não precise de atualizações de localização, a menos que a localização do usuário tenha sido alterada para uma cidade diferente. Nesse caso, você deve ajustar o movimento mínimo exigido para um evento de atualização de localização configurando a propriedade [**MovementThreshold**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.movementthreshold). Isso tem o efeito de filtrar os eventos [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged). Esses eventos surgem apenas quando a alteração na posição excede o limite de movimento.

    -   Use [**reportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval) que se alinha à experiência do aplicativo e minimiza o uso de recursos do sistema. Por exemplo, um aplicativo meteorológico talvez precise atualizar dados apenas a cada 15 minutos. A maioria dos aplicativos, desde que não sejam aplicativos de navegação em tempo real, não exige um fluxo constante de alta precisão para as atualizações de localização. Se seu aplicativo não exigir o fluxo de dados mais exato possível ou se precisar de atualizações com pouca frequência, defina a propriedade **ReportInterval** para indicar a frequência mínima de atualizações de local necessária para seu aplicativo. Assim, a origem da localização pode economizar energia calculando o local somente quando necessário.

        Os aplicativos que não exigem dados em tempo real devem definir [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval) como 0 para indicar que nenhum intervalo mínimo está especificado. O intervalo de relatório padrão é um segundo ou a frequência suportada pelo hardware, o que for menor.

        Dispositivos que fornecem dados de localização podem monitorar o intervalo de relatórios solicitado por diferentes aplicativos e fornecer relatórios de dados no menor intervalo solicitado. Dessa forma, o aplicativo com a maior necessidade de precisão recebe os dados necessários. Portanto, é possível que o provedor de localização gere atualizações com uma frequência mais alta do que a solicitada pelo aplicativo, caso outro aplicativo solicite atualizações mais frequentes.

        **Observe**  que não é garantido que a origem do local honrará a solicitação para o intervalo de relatório especificado. Nem todos os dispositivos de provedores de localização rastreiam o intervalo de relatório, mas você deve fornecê-lo para os que o fazem.

    -   Para ajudar a economizar energia, defina a propriedade [**desiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) para indicar a plataforma de localização, independentemente de seu aplicativo precisar ou não de dados da maior exatidão possível. Quando nenhum aplicativo exige dados de alta precisão, o sistema pode economizar energia não habilitando os provedores de GPS.

        -   Defina [**desiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) como **HIGH** para permitir que o GPS adquira os dados.
        -   Defina [**desiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) como **Default** e use apenas um padrão de chamada de disparo único para minimizar o consumo de energia se seu aplicativo usar as informações de localização exclusivamente para direcionamento de anúncios.

        Se seu aplicativo tiver necessidades específicas quanto à precisão, você poderá considerar conveniente usar a propriedade [**DesiredAccuracyInMeters**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) em vez de usar [**DesiredAccuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy). Isso é particularmente útil no Windows Phone, onde a posição geralmente pode ser obtida com base em sinalizadores de celulares, sinalizadores de Wi-Fi e satélites. Escolher um valor de precisão mais específico ajudará o sistema a identificar as tecnologias certas para usar com o menor custo de energia ao fornecer uma posição.

        Por exemplo: 

        -   Se o seu aplicativo está obtendo localização para ajuste de anúncios, notícias, tempo, etc, uma precisão de 5000 metros geralmente é suficiente.
        -   Se seu aplicativo estiver exibindo acordos próximos na vizinhança, uma precisão do medidor de 300 geralmente é boa para fornecer resultados.
        -   Se o usuário está à procura de recomendações de restaurantes nas proximidades, provavelmente queremos obter uma posição dentro de um quarteirão; portanto, uma precisão de 100 metros é suficiente.
        -   Se o usuário estiver tentando compartilhar sua posição, o aplicativo deverá solicitar uma precisão de cerca de 10 metros.
    -   Use a propriedade [**Geocoordinate.accuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) se seu aplicativo tiver requisitos de precisão específicos. Por exemplo, aplicativos de navegação devem utilizar a propriedade **Geocoordinate.accuracy** para determinar se os dados de localização disponíveis atendem aos requisitos do aplicativo.

-   Considere o atraso de inicialização. Na primeira vez que um aplicativo solicitar dados de localização, pode haver um pequeno atraso (de 1 a 2 segundos) enquanto o provedor de localização é iniciado. Considere isso no design da interface do usuário do seu aplicativo. Por exemplo, você pode querer evitar o bloqueio de outras tarefas que dependem da conclusão da chamada de [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync).

-   Considere o comportamento em segundo plano. Se seu aplicativo não tiver foco, ele não receberá eventos de atualização de localização enquanto estiver suspenso em segundo plano. Se o aplicativo fizer rastreamento de atualizações de localização registrando-as, lembre-se disso. Quando o aplicativo entrar novamente em foco, ele receberá apenas eventos novos. Ele não obterá as atualizações realizadas enquanto estava inativo.

-   Use sensores raw e fusion com eficiência. Há dois tipos de sensores: *raw* e *fusion*.

    -   Os sensores raw incluem o acelerômetro, o girômetro e o magnetômetro.
    -   Os sensores fusion incluem o de orientação, o inclinômetro e a bússola. Os sensores fusion obtêm seus dados de combinações dos sensores brutos.

    As APIs do Windows Runtime podem acessar todos esses sensores, com exceção do magnetômetro. Os sensores fusion são mais precisos e estáveis do que os sensores raw, mas usam mais energia. Você deve usar os sensores corretos para cada finalidade. Para saber mais, consulte [Sensores](https://docs.microsoft.com/windows/uwp/devices-sensors/sensors).

**Modo de espera conectado**
- Quando o computador está conectado no modo de espera, objetos [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) sempre podem ser instanciados. No entanto, o objeto **Geolocator** não encontrará nenhum sensor a ser agregado e, portanto, as chamadas para [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) atingirão seu tempo limite após sete segundos, os ouvintes do evento [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged) nunca serão chamados, e os ouvintes do evento [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged) serão chamados uma vez com o status **NoData**.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional


### <a name="detecting-changes-in-location-settings"></a>Detectando alterações nas configurações de localização

O usuário pode desativar a funcionalidade de localização usando as **configurações de privacidade de localização** no aplicativo **Configurações**.

-   Para saber quando o usuário desabilita ou reabilita os serviços de localização:
    -   Trate o evento [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged). A propriedade [**Status**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) do argumento para o evento **StatusChanged** tem o valor **Disabled** quando o usuário desabilita os serviços de localização.
    -   Verifique os códigos de erro retornados de [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync). Se o usuário tiver desabilitado os serviços de localização, as chamadas para **GetGeopositionAsync** falharão com um erro de **\_acesso negado** e a propriedade [**LocationStatus**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.locationstatus) terá o valor **desabilitado**.
-   Se você tiver um aplicativo cujos dados de localização são essenciais, por exemplo, um aplicativo de mapa, faça o seguinte:
    -   Trate o evento [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged) para obter atualizações se houver mudança na localização do usuário.
    -   Trate o evento [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged), conforme descrito anteriormente, para detectar mudanças nas configurações de localização.

Observe que o serviço de localização retornará os dados quando estiver disponível. Ela pode retornar primeiro um local com um raio de erro maior e atualizar o local com mais informações precisas assim que estiver disponível. Os aplicativos que exibem o local do usuário normalmente desejam atualizar o local quando informações precisas estiverem disponíveis.

### <a name="graphical-representations-of-location"></a>Representações gráficas de localização

Faça com que seu aplicativo use [**Geocoordinate.accuracy**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) para denotar a localização atual do usuário no mapa claramente. Existem três bandas principais de precisão, um raio de erro de aproximadamente 10 metros, outro de aproximadamente 100 metros e um terceiro superior a 1 quilômetro. Usando as informações de precisão você pode assegurar que seu aplicativo exiba a localização com precisão no contexto dos dados disponíveis. Para obter informações gerais sobre como usar o controle de mapa, consulte [Exiba mapas em modos de exibição 2D, 3D e Streetside](https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps).

-   Para uma precisão de aproximadamente 10 metros (resolução do GPS), a localização pode ser indicada por um ponto ou pino no mapa. Com essa precisão, as coordenadas de latitude e longitude e o endereço também podem ser mostrados.

    ![exemplo de mapa exibido na precisão de gps de aproximadamente 10 metros.](images/10metererrorradius.png)

-   Para uma precisão entre 10 e 500 metros (aproximadamente 100 metros), a localização é, em geral, recebida com resolução Wi-Fi. A localização obtida a partir de celular tem uma precisão de cerca de 300 metros. Em tais casos, recomendamos que o aplicativo mostre um raio de erro. Para aplicativos que mostram direções que não exigem um ponto central, esse ponto pode ser mostrado com um raio de erro em torno dele.

    ![exemplo de mapa exibido na precisão de wi-fi de aproximadamente 100 metros.](images/100metererrorradius.png)

-   Se a precisão retornada for superior a um quilômetro, você provavelmente estará recebendo as informações de localização com resolução de nível IP. Em regra, esse nível de precisão é muito baixo para a identificação de um ponto específico no mapa. Seu aplicativo deve aumentar o zoom para o nível de cidade no mapa ou para a área adequada com base no raio de erro (por exemplo, o nível de região).

    ![exemplo de mapa exibido na precisão de wi-fi de aproximadamente um quilômetro.](images/1000metererrorradius.png)

Quando a precisão da localização mudar de uma banda para outra, faça uma transição suave entre as diferentes representações gráficas. Isso pode ser feito das seguintes maneiras:

-   Suavizando a animação da transição e mantendo a transição rápida e fluida
-   Aguardando alguns relatórios consecutivos para confirmar a mudança na precisão, para ajudar a evitar ampliações indesejadas e muito frequentes.

### <a name="textual-representations-of-location"></a>Representações textuais de localização

Alguns tipos de aplicativos, por exemplo, aplicativos de previsão de tempo ou de informações locais, precisam de maneiras de representar textualmente a localização em diferentes bandas de precisão. Verifique se a localização está sendo exibida claramente e somente no nível de precisão fornecidos nos dados.

-   Para uma precisão de aproximadamente 10 metros (resolução de GPS), os dados de localização recebidos são suficientemente precisos e podem ser comunicados ao nível do nome do bairro. Também é possível usar o nome da cidade, estado ou província e país/região.
-   Para uma precisão de aproximadamente 100 metros (resolução de Wi-Fi), os dados de localização recebidos são moderadamente precisos, por isso recomendamos a exibição de informações como o nome da cidade. Evite usar o nome do bairro.
-   Para uma precisão superior a um quilômetro (resolução de IP), exiba somente o nome do estado ou província ou do país/região.

### <a name="privacy-considerations"></a>Considerações sobre privacidade

A localização geográfica do usuário faz parte das PII (informações de identificação do usuário). O site a seguir fornece orientações para proteger a privacidade do usuário.

-   [Privacidade da Microsoft]( https://www.microsoft.com/privacy/dpd/default.aspx)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>Tópicos relacionados

* [Configurar uma cerca geográfica](https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)
* [Obter a localização atual](https://docs.microsoft.com/windows/uwp/maps-and-location/get-location)
* [Exibir mapas com modos de exibição 2D, 3D e Streetside](https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [Amostra de localização UWP (geolocalização)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
