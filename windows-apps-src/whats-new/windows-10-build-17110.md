---
author: QuinnRadich
title: Novidades no Windows 10 para desenvolvedores, ferramentas e recursos
description: O Windows 10, compilação 17110 e as novas ferramentas de desenvolvedor fornecem as ferramentas, os recursos e as experiências da UWP (Plataforma Universal do Windows).
keywords: novidades, novidade, atualização, atualizações, recursos, novos, Windows 10, mais recente, desenvolvedores, 17110, prévia
ms.author: quradic
ms.date: 3/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 518cc64fec2524bb3cc02daca72a990d016c8ce5
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-windows-10-for-developers-sdk-preview-build-17110"></a>Novidades no Windows 10 para desenvolvedores, SDK versão prévia 17110

O SDK do Windows 10, versão prévia 17110, em combinação com o Visual Studio 2017 e o SDK atualizado, fornece as ferramentas, os recursos e as experiências para tornar notáveis os aplicativos da Plataforma Universal do Windows. [Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

Esta é uma coleção de recursos novos e aprimorados e diretrizes de interesse para os desenvolvedores do Windows nesta prévia do SDK. Por enquanto, esses recursos são acessíveis aos membros do [Programa Windows Insider](https://insider.windows.com/)e serão disponibilizados publicamente na próxima atualização principal do Windows 10. Para obter uma lista completa dos novos namespaces adicionados ao SDK do Windows, consulte as [alterações de API do Windows 10 build 17110](windows-10-build-17110-api-diff.md). Para obter mais informações sobre os recursos em destaque do Windows 10, consulte [O que é interessante no Windows 10](http://go.microsoft.com/fwlink/?LinkId=823181). Além disso, consulte os [recursos da Windows Developer Platform](https://developer.microsoft.com/windows/platform/features) para obter uma visão geral de alto nível das adições passadas e futuras feitas na plataforma do Windows.

## <a name="design--ui"></a>Design e interface do usuário

Recurso | Descrição
 :------ | :------
Notificações do sistema interativas e adaptáveis | Aprimore seu app com notificações interativas e adaptáveis. Tenha uma introdução com as nossas [diretrizes atualizadas sobre notificações do sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)e explore as novas informações sobre as restrições de tamanho de imagem, as barras de progresso e a adição de opções de entrada.
Links de conteúdo | O novo controle [Links de conteúdo](../design/controls-and-patterns/content-links.md) oferece uma maneira de inserir dados avançados em seus controles de texto, o que permite que um usuário encontre e use mais informações sobre uma pessoa ou um local sem sair do contexto do seu aplicativo.
Exemplos de design | O exemplo BuildCast foi adicionado à página [Projetar kits de ferramentas e amostras](../design/downloads/index.md). O BuildCast é uma amostra completa criada para demonstrar o Sistema Design Fluente e outros recursos da Plataforma Universal do Windows.
Manuscrito inserido | O recurso de entrada à caneta foi adicionado aos [controles de texto](../design/controls-and-patterns/text-controls.md), permitindo que os usuários escrevam diretamente nas caixas de texto com o Windows Ink. Conforme o usuário escreve, o texto é convertido em um script que mantém a sensação de escrita.
Atualizações do Design Fluente | Atualizamos muitos das nossas páginas de Design Fluente com novas informações e diretrizes: </br> * A [visão geral do Design Fluente](../design/fluent-design-system/index.md) foi atualizada para se alinhar aos recursos mais recentes do Fluent. </br> * [Realce do Revelação](../design/style/reveal.md) tem novas diretrizes sobre temas e controles personalizados. </br> O * [Histórico de navegação e navegação regressiva](../design/basics/navigation-history-and-backwards-navigation.md) foi reformulado, com exemplos detalhados, diretrizes para otimização de dispositivo e diretrizes para comportamento personalizado.
Layouts de página | Atualizamos nossos documentos de [layout de página XAML](../design/layout/layouts-with-xaml.md) com novas informações sobre layouts fluidos e estados visuais. Esses recursos permitem maior controle sobre como a posição dos elementos em seu app responde e se adapta ao espaço visual disponível.
Puxar para atualizar | O controle [Efetuar pull para atualizar](../design/controls-and-patterns/pull-to-refresh.md) permite que um usuário use uma lista suspensa de dados para recuperar mais dados. Ele é amplamente usado em dispositivos com tela touch.
Modo de exibição de navegação | O controle [modo de exibição de Navegação](../design/controls-and-patterns/navigationview.md) fornece um menu de navegação recolhível para a navegação de nível superior em seu app. Esse controle implementa o padrão de painel de navegação, ou menu hambúrguer, e adapta automaticamente o modo de exibição do painel a diferentes tamanhos de janela.
Foco do Revelação | O novo efeito [Foco do Revelação](../design/style/reveal-focus.md) fornece iluminação para experiências como o Xbox One e as telas de televisão. Ele anima a borda de elementos focalizáveis, como botões, quando o usuário move o foco do gamepad ou do teclado até eles.
Som | Agora, o XAML dá suporte a Áudio 3D com a propriedade **SpatialAudioMode**. Consulte [Som](../design/style/sound.md) para obter informações sobre como ele pode ser configurado.
Modo de exibição de árvore | O controle [TreeView](../design/controls-and-patterns/tree-view.md) habilita uma lista hierárquica com nós em expansão e em colapso que contêm itens aninhados. Ele pode ser usado para ilustrar uma estrutura de pastas ou relacionamentos aninhados em sua interface do usuário.
Estilo de escrita | Nós fizemos upgrade e expandimos nosso artigo em voz e tom, transformando-o em [Diretrizes de estilo de escrita](../design/style/writing-style.md). Essas novas informações fornecem princípios para a criação de texto eficaz em seu app e mostram as práticas recomendadas para escrever para controles como mensagens de erro ou caixas de diálogo.

## <a name="gaming"></a>Jogos
Recurso | Descrição
 :------ | :------
Introdução ao desenvolvimento de jogos | Interessado em desenvolver jogos para o Windows 10? A nova página [Introdução ao desenvolvimento de jogos](../gaming/getting-started.md) fornece uma visão geral completa sobre o que você precisa fazer para se preparar, se registrar e ficar pronto para enviar seus apps e jogos.
Adaptadores gráficos | As APIs de DXGI a seguir foram adicionadas, o que está relacionado à preferência e à remoção de adaptadores gráficos: </br> * A interface [IDXGIFactory6](https://msdn.microsoft.com/library/windows/desktop/mt814823) habilita um método único que enumera adaptadores gráficos com base em uma determinada preferência de GPU. </br> * A função [DXGIDeclareAdapterRemovalSupport](https://msdn.microsoft.com/library/windows/desktop/mt814821) permite que um processo indique que é resiliente a qualquer um dos seus dispositivos gráficos sendo removidos. </br> * A enumeração [DXGI_GPU_PREFERENCE](https://msdn.microsoft.com/library/windows/desktop/mt814822) descreve a preferência de GPU para executar o app.


## <a name="develop-windows-apps"></a>Desenvolver aplicativos do Windows

Recurso | Descrição
 :------ | :------
Placas adaptáveis | Os [cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/) é um formato aberto de troca de placas que permite aos desenvolvedores trocar o conteúdo da interface do usuário de uma maneira comum e consistente. Eles descrevem seu conteúdo como um objeto JSON que pode ser renderizado para se adaptar automaticamente à aparência do aplicativo host.
Grupo de Recursos de App | A classe [AppResourceGroupInfo](https://docs.microsoft.com/uwp/api/windows.system.appresourcegroupinfo) tem novos métodos que você pode usar para iniciar a transição para os estados de app suspenso, ativo (retomado) e encerrado.
Amplo acesso ao sistema de arquivos | A funcionalidade **broadFileSystemAccess** concede aos apps o mesmo acesso ao sistema de arquivos do usuário que está executando o app, sem nenhuma solicitação adicional de estilo de seletor de arquivos durante o tempo de execução. Para obter mais informações, consulte [Permissões de acesso a arquivo](../files/file-access-permissions.md) e a entrada **broadFileSystemAccess** nas [Declarações de funcionalidade do app](../packaging/app-capability-declarations.md).
Aplicativos UWP de console | Agora você pode escrever apps de console UWP C++ /WinRT ou /CX que são executados em uma janela de console, como a janela de console do DOS ou do PowerShell. Os apps de console usam a janela do console para entrada e saída. Os apps de console UWP podem ser publicados para a Microsoft Store, tem uma entrada na lista de aplicativos e um bloco principal que pode ser fixado no menu Iniciar. Para obter mais informações, consulte [Criar um app de console da Plataforma Universal do Windows](../launch-resume/console-uwp.md)
Marcos e títulos com suporte para a AT (tecnologia acessível) | Os marcos e títulos definem seções de uma interface do usuário que auxiliam na navegação eficiente para os usuários de tecnologia adaptativa, como leitores de tela. Para obter mais informações, consulte [Marcos e títulos](../design/accessibility/landmarks-and-headings.md).
Aprendizado de Máquina | O Windows Machine Learning permite que você crie apps que avaliam previamente modelos treinados localmente em seus dispositivos Windows 10. Para saber mais sobre a plataforma, consulte [Windows Machine Learning](../machine-learning/index.md). </br> O namespace [MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.preview) contém classes que permitem que os apps carreguem modelos de aprendizado de máquina, associem dados como entradas e avaliem os resultados.
Controles de mapa | A classe [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) tem uma nova propriedade chamada **Region**, que você pode usar para mostrar conteúdo em um controle de mapa com base no idioma de uma região específica (por exemplo, o estado ou a província).
Elementos de mapa | A classe [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) tem uma nova propriedade chamada **IsEnabled**, que você pode usar para especificar se os usuários podem interagir com o [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement).
Informações de locais do mapa | A classe [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) contém um novo método **CreateFromAddress**, que você pode usar para criar um [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) usando um endereço e um nome de endereço.
Serviços de mapa | A classe [MapRouteDrivingOptions](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutedrivingoptions) contém uma nova propriedade chamada **DepartureTime**, que você pode usar para calcular uma rota com as condições de trânsito típicas do dia e hora específicos.
Aplicativos UWP de várias instâncias | Um aplicativo UWP pode aceitar o suporte a várias instâncias. Se uma instância de um aplicativo UWP de várias instâncias estiver em execução, e uma solicitação de ativação subsequente chegar, a plataforma não ativará a instância existente. Em vez disso, ela criará uma nova instância, executada em um processo separado. Para obter mais informações, consulte [Criar um Aplicativo Universal do Windows de várias instâncias](../launch-resume/multi-instance-uwp.md)
PlayReady | O Microsoft PlayReady é um conjunto de tecnologias para a proteção do conteúdo digital contra uso não autorizado. O PlayReady é executado em todos os tipos de dispositivos e apps e em todos os sistemas operacionais. [Saiba como incorporar o PlayReady a seu app.](https://docs.microsoft.com/playready/)
Captura de tela | O [namespace Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) fornece APIs para adquirir quadros de uma tela ou de uma janela de aplicativo para criar fluxos de vídeo ou instantâneos para criar experiências colaborativas e interativas. Consulte [captura de tela](../audio-video-camera/screen-capture.md) para obter mais informações.
Gatilhos do sistema | O [CustomSystemEventTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.customsystemeventtrigger) permite que você defina um gatilho do sistema quando o sistema operacional não fornece um gatilho do sistema necessário. Por exemplo, quando um driver de hardware e o aplicativo UWP pertencerem a terceiros, e o driver de hardware precisar gerar um evento personalizado que seu app possa manipular Por exemplo, uma placa de áudio que precisa notificar um usuário quando uma tomada de áudio está conectada.
Atividades do usuário | A classe **UserActivitySessionHistoryItem** tem novos métodos que recuperam as atividades recentes do usuário. Consulte [GetRecentUserActivitiesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel.getrecentuseractivitiesasync) e sua sobrecarga para obter detalhes.
Windows Mixed Reality | Para dar suporte à crescente plataforma Windows Mixed Reality, novas APIs foram adicionadas aos namespaces [Windows.Graphic.Holographic](https://docs.microsoft.com/uwp/api/Windows.Graphics.Holographic) e [Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Spatial).

## <a name="publish--monetize-windows-apps"></a>Publique e monetize aplicativos do Windows

Recurso | Descrição
 :------ | :------
Inserir preços de forma livre na moeda local de um mercado específico | Quando você substituir o preço base do seu app para um mercado específico, não estará limitado a escolher uma das faixas de preço padrão; agora você tem a opção de inserir um preço de forma livre na moeda local do mercado. Para obter mais informações, consulte [Definir e agendar preço do aplicativo](../publish/set-and-schedule-app-pricing.md). **Esse recurso está disponível para todos os desenvolvedores do Windows e não exige o SDK atualizado.**
Contexto da Store | A classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) foi atualizada com uma seleção de novos métodos. Esses métodos gerenciam o download e a instalação de atualizações de pacote e complementos para um app.
Agora, complementos de assinatura estão disponíveis para todos os desenvolvedores | Crie e publique complementos de assinatura para vender produtos digitais em seus apps e jogos (como os recursos do app ou o conteúdo digital) com períodos de cobrança recorrentes automatizados. Para obter mais detalhes, consulte [Habilitar complementos de assinatura para o aplicativo](../monetize/enable-subscription-add-ons-for-your-app.md). **Esse recurso está disponível para todos os desenvolvedores do Windows e não exige o SDK atualizado.**

## <a name="videos"></a>Vídeos

Os vídeos a seguir foram publicados desde a Fall Creator's Update, realçando os recursos novos e aprimorados no Windows 10 para desenvolvedores.

### <a name="package-a-net-app-in-visual-studio"></a>Empacotar um aplicativo .NET no Visual Studio

Ficou muito mais fácil trazer seu aplicativo da área de trabalho para a Plataforma Universal do Windows. [Assista ao vídeo](https://www.youtube.com/watch?v=fJkbYPyd08w) para saber como empacotar seu aplicativo .NET para distribuição e depois [confira esta página](../porting/desktop-to-uwp-packaging-dot-net.md) para obter mais informações.

### <a name="xbox-live-creators-program"></a>Programa de Criadores do Xbox Live

O Programa de Criadores do Xbox Live permite que os desenvolvedores publiquem rapidamente seus jogos UWP no Xbox One e no Windows 10. [Assista ao vídeo](https://www.youtube.com/watch?v=zpFfHHBkVq4) para saber mais sobre o programa e então [confira esta página](https://www.xbox.com/developers/creators-program) para obter uma introdução.

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>Como criar inicializadores de apps 3D para Windows Mixed Reality

Os inicializadores 3D fornecem um modo exclusivo para que os usuários façam uma representação verdadeiramente volumétrica do seu app no ambiente inicial do Mixed Reality. [Assista ao vídeo](https://www.youtube.com/watch?v=TxIslHsEXno) para aprender a preparar seu modelo 3D e atribuí-lo como o inicializador de seu app, então [leia os documentos de desenvolvedor](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers) e [confira nossas diretrizes de design](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance) para obter mais informações.

### <a name="motion-controller-tracking"></a>Rastreamento de controlador de movimentos

Os controladores de movimento representam as mãos de um usuário no Windows Mixed Reality. [Assista ao vídeo](https://www.youtube.com/watch?v=rkDpRllbLII) para saber como os controladores de movimentos funcionam quando estão dentro e fora do campo de visão do headset de realidade misturada e [leia mais sobre como rastrear o controlador aqui.](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)

### <a name="accessibility-tools-for-windows-developers"></a>Ferramentas de acessibilidade para desenvolvedores do Windows

O SDK do Windows 10 inclui várias ferramentas para ajudar você a testar e a melhorar a acessibilidade do seu app. As ferramentas Inspect e AccEvent ajudarão você a garantir que seus apps estejam disponíveis para todos. [Assista ao vídeo](https://www.youtube.com/watch?v=ce0hKQfY9B8&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF&t=0s&index=1) para saber mais sobre essas ferramentas e então [Leia mais sobre testes de acessibilidade](../design/accessibility/accessibility-testing.md) para obter mais informações.