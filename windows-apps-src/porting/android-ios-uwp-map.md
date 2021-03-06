---
Description: Compare os recursos entre as plataformas iOS, Android e Windows 10.
title: Mapeamento de conceitos de aplicativos do Windows para desenvolvedores Android e iOS
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: 23d090422c7c5aabb146d9cd1ff40cc64a485bdb
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683969"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Mapeamento de conceitos de aplicativos do Windows para desenvolvedores Android e iOS

Se você for um desenvolvedor com habilidades de Android ou iOS e/ou de código e quiser mudar para o Windows 10 e a Plataforma Universal do Windows (UWP), este método tem tudo o que você precisa para mapear recursos de plataforma — e seu conhecimento — entre as três plataformas.

Consulte também o conteúdo de portabilidade em [Move from iOS to UWP](ios-to-uwp-root.md). Este documento também está disponível como um [download](https://www.microsoft.com/download/details.aspx?id=52041).

## <a name="user-interface-ui"></a>Interface do usuário (IU)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Linguagem de design.</strong><br><br>Um conjunto de convenções que estabelece como deve ser a aparência e o comportamento dos apps na plataforma.</td>
<td align="left">Diretrizes de <strong>design de material do Android</strong> oferecem uma linguagem visual para os desenvolvedores e designers do Android seguirem.</td>
<td align="left"><strong>Diretrizes de interface humana</strong> oferecem consultoria para designers e desenvolvedores de iOS.</td>
<td align="left">O <a href="https://developer.microsoft.com/windows/apps/design"><strong>design de aplicativos UWP Windows</strong></a> mostra como criar um aplicativo que parece fantástico em todos os dispositivos Windows 10. Você encontrará fundamentos de design da interface do usuário (IU), técnicas de design responsivo e uma lista completa de diretrizes detalhadas.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Linguagem de marcação de interface do usuário.</strong> <br><br>Uma linguagem de marcação que renderiza e descreve uma interface do usuário e seus componentes. Cada plataforma fornece um editor para edição visual e de marcação.<br/></td>
<td align="left"><strong>Layouts XML</strong>, editados usando <strong>Android Studio</strong> ou <strong>Eclipse</strong>.</td>
<td align="left"><strong>XIB</strong> e <strong>Storyboards</strong> editados usando <strong>o Interface Builder</strong> dentro do Xcode.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>, editado usando <strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> e <strong><a href="https://docs.microsoft.com/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend para Visual Studio</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/index">Plataforma XAML</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui">Criar uma interface do usuário com XAML</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">Definir layouts com XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Controles de interface do usuário internos.</strong> <br><br>Elementos de interface do usuário reutilizáveis fornecidos pela plataforma, como botões, controles de lista e controles de texto.</td>
<td align="left">Classes pré-construídas de <strong>visualização</strong> e <strong>grupo de visualização</strong> conhecidas como widgets, layouts, campos de texto, contêineres, controles de data/hora e controles especializados.</td>
<td align="left"><strong>Visualizações</strong> e <strong>controles</strong> encontrados na biblioteca de objetos do Xcode e listados no catálogo de interface de usuário do UIKit. As visualizações incluem visualizações de imagem, visualizações de seletor e visualizações de rolagem. Os controles incluem botões, seletores de data e campos de texto.</td>
<td align="left">A plataforma XAML fornece um conjunto generoso de <strong>controles internos</strong>, como botões, controles de lista, painéis, controles de texto, barras de comando, seletores, mídia e escrita à tinta.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">Adicionar controles e manipular eventos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Controlar o tratamento de eventos.</strong> <br><br>Definir a lógica que é executada quando os eventos são acionados dentro de controles de interface do usuário.</td>
<td align="left"><strong>Manipuladores de evento</strong> e <strong>ouvintes de evento</strong> são adicionados em XML ou de forma programática.</td>
<td align="left">Os controles enviam mensagens <strong>de ação</strong> para <strong>alvos</strong>.</td>
<td align="left">Você pode definir métodos para tratar os eventos de um controle XAML em um <strong>arquivo code-behind</strong> anexado à página XAML. <strong>Manipuladores de evento</strong> sempre são escritos em código. Mas você pode enganchar esses manipuladores de evento em marcação XAML ou em código.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">Adicionar controles e manipular eventos</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview">Visão geral de eventos e eventos roteados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Associação de dados.</strong> <br><br>Um padrão de design de software que permite que a interface do usuário de seu app renderize dados e, opcionalmente, fique em sincronia com esses dados.</td>
<td align="left">Uma <strong>biblioteca de vinculação de dados</strong> é fornecida, mas ainda está em versão beta.</td>
<td align="left">Não há nenhum sistema de vinculação interno no iOS. <strong>A observação de valor-chave</strong> pode ser construída para realizar a vinculação de dados com o uso de uma biblioteca de terceiros ou com a escrita de um código adicional. Os controles usam uma abordagem de delegação/retorno de chamada para obtenção de dados.</td>
<td align="left">A plataforma UWP manipula a <strong>vinculação de dados</strong> para você. Você usa a extensão de marcação <strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension">{x:Bind}</a></strong> para tirar proveito da associação de alto desempenho ou <strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension">{Binding}</a></strong> para aproveitar mais recursos. Em seguida, basta configurar sua associação para escolher se a plataforma usa <strong>associação unidirecional</strong> para exibir valores de uma fonte de dados em sua interface do usuário, ou se ela também observa esses valores e atualiza a interface do usuário quando mudam com a <strong>associação bidirecional</strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/data-binding/index">Vinculação de dados</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Automação da interface do usuário.</strong> <br><br>Acesso programático a elementos de interface do usuário, tornando os apps acessíveis para produtos de tecnologia adaptativa e habilitando scripts de teste automatizados para interagirem com a interface do usuário.</td>
<td align="left"><strong>Rótulos de texto</strong>, <strong>e os valores contentDescription</strong> e <strong>hint</strong> ajudam a garantir que os elementos de interface do usuário possam ser encontrados por automação. O Android Studio permite que você escreva testes de interface do usuário usando as estruturas de teste do <strong>UI Automator</strong> e do <strong>Espresso</strong>.</td>
<td align="left">O <strong>instrumento de automação</strong> permite que você escreva scripts de testes de interface do usuário automatizados que identifiquem elementos usando as configurações de <strong>acessibilidade</strong> ou a posição do elemento na <strong>hierarquia de elementos</strong>.</td>
<td align="left">Você obtém acesso programático imediato a elementos de interface do usuário internos na UWP com a <strong><a href="https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview">automação de interface do usuário</a></strong>.<br/>Os <strong><a href="https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers">colegas de automação personalizados</a></strong> permitem fornecer suporte de automação para suas próprias classes de interface do usuário personalizadas. O <strong><a href="https://docs.microsoft.com/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">projeto de teste de IU codificado</a></strong> no Visual Studio permite que você teste automaticamente seu app inteiro por meio da interface do usuário, ou teste a interface do usuário isoladamente.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Alterando a aparência de um controle.</strong> <br><br>Edição de tamanho, cor e outros atributos.</td>
<td align="left">Os controles têm <strong>propriedades</strong> que podem ser editadas usando a ferramenta de designer, em marcação XML ou programaticamente.</td>
<td align="left">Os controles têm <strong>atributos</strong> que você pode editar usando o <strong>Inspetor de atributos</strong> no Construtor de interface ou programaticamente.</td>
<td align="left">Você pode editar as <strong>propriedades</strong> dos controles na marcação XAML ou programaticamente, usando o Visual Studio e o Blend for Visual Studio.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">Adicionar controles e manipular eventos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Estilos visuais reutilizáveis.</strong> <br><br>Aplicam mudanças visuais a uma quantidade de controles, em um formato reutilizável.</td>
<td align="left"><strong>Estilos XML</strong> são conjuntos de propriedades que são aplicadas a um ou mais controles.</td>
<td align="left">O iOS não oferece suporte a estilos visuais reutilizáveis por padrão, mas o protocolo UIAppearance permite que vários controles compartilhem atributos comuns.</td>
<td align="left">Você pode criar <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style">estilos</a></strong> reutilizáveis, que podem ser aplicados a vários controles e armazenados em um <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> para reutilização fácil.<br/><br/><a href="https://docs.microsoft.com/previous-versions/windows/apps/hh465381(v=win.10)">Guia de início rápido: Controles de estilo</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Editando a estrutura visual de controles.</strong> <br><br>Personalize a estrutura visual de um controle além de apenas modificar propriedades ou atributos, por exemplo, movendo o texto da caixa de seleção para baixo da caixa de seleção.</td>
<td align="left">Não existe um método simples de edição da estrutura visual dos controles no Android.</td>
<td align="left">Não existe um método simples de edição da estrutura visual dos controles no iOS.</td>
<td align="left">Para personalizar a estrutura visual de um controle, você pode copiar e editar seu <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">2modelo de controle</a></strong> em marcação XAML.<br/><br/><a href="https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)">Início rápido: modelos de controle</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Gestos de toque internos.</strong> <br><br>Fornece suporte a toque personalizado, manipulando eventos de gestos abstratos de alto nível, como toque e toque duplo em visualizações e controles.</td>
<td align="left"><strong>Detectores de gesto</strong> detectam gestos de toque comuns, incluindo rolagem, pressão longa, toque, toque duplo e deslize rápido.</td>
<td align="left">A estrutura UIKit fornece <strong>reconhecedores de gestos</strong> internos que detectam toques de gestos incluindo tocar, pinçar, aplicar panorâmica, passar o dedo, girar e pressionar longamente.</td>
<td align="left"><strong>Elementos de interface do usuário</strong> permitem que você manipule <strong>eventos de gestos estáticos</strong> incluindo tocar, dar um toque duplo, tocar e segurar à direita, assim como <strong>eventos de gestos de manipulação</strong> incluindo deslizar, passar o dedo, rodar, pinçar e ampliar. Eventos de gestos são <strong>eventos roteados</strong> e podem ser manipulados por objetos pai que contenham o filho UIElement.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions">Interações por toque</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/layout/index">Interações personalizadas do usuário - gestos, manipulações e interações</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">Navegação e estrutura de aplicativos</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Layouts.</strong> <br><br>O layout define a estrutura da interface do usuário.</td>
<td align="left">O layout é composto por <strong>grupos de visualização</strong>, como o <strong>LinearLayout</strong> e o <strong>RelativeLayout</strong>, que podem aninhar outros grupos de visualização ou visualizações.</td>
<td align="left">O layout é composto por um <strong>UIViewController</strong> contendo <strong>UIViews</strong> que podem ser aninhadas.</td>
<td align="left">XAML que fornece um sistema de layout flexível composto por <strong>classes de painel de layout</strong> como <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> para layouts dinâmicos e estáticos. <strong><a href="https://docs.microsoft.com/visualstudio/ide/reference/properties-window?view=vs-2015">As propriedades</a></strong> são usadas para controlar o tamanho e a posição dos elementos.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">Definir layouts com XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Navegação de mesmo nível.</strong> <br><br>Apresentar ao usuário métodos de navegação entre páginas de importância hierárquica igual.</td>
<td align="left"><strong>Guias</strong>, <strong>visualizações de deslizar o dedo</strong> e <strong>gavetas de navegação</strong> fornecem <strong>navegação lateral</strong>.</td>
<td align="left"><strong>Controladores de barra de guias</strong>, <strong>controladores de modo divisão</strong> e <strong>controladores de exibição de página</strong> permitem a navegação entre as visualizações de hierarquia igual.</td>
<td align="left">Você pode exibir uma lista persistente de links/guias acima do conteúdo usando <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot">guias/pivôs</a></strong>. O <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/split-view">painel de navegação/modo divisão</a></strong> permite que você exiba uma lista de links junto com o conteúdo.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-basics">Navegação</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigate-between-two-pages">Navegar entre duas páginas</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Navegação hierárquica.</strong> <br><br>Navegação entre páginas pai e filho de uma hierarquia.</td>
<td align="left"><strong>Listas</strong> e <strong>listas de grade</strong>, <strong>botões</strong> e outros controles fornecem <strong>navegação descendente</strong> quando usados com <strong>intenções</strong> de carregar outras <strong>atividades</strong>.</td>
<td align="left"><strong>Controladores de navegação</strong> permitem que os usuários naveguem entre níveis de uma hierarquia.</td>
<td align="left">Os <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub">hubs</a></strong> permitem mostrar ao usuário uma visualização do conteúdo que pode ser selecionado para navegar até páginas filho. <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/master-details">Mestre/detalhes</a></strong> permitem que os usuários escolham em uma lista de resumos de itens que são exibidos ao lado da seção de detalhes correspondente.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-basics">Navegação</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigate-between-two-pages">Navegar entre duas páginas</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Navegação do botão voltar.</strong> <br><br>Navegar de volta através de um app.</td>
<td align="left">Os botões <strong>voltar</strong> e <strong>subir</strong> dentro da barra de ação oferecem navegação <strong>ancestral</strong> e <strong>temporal</strong> usando a <strong>pilha voltar</strong>.</td>
<td align="left">O <strong>controlador de navegação</strong> pode ter um botão Voltar adicionado a ele.<br/></td>
<td align="left">Você pode tratar as pressões do botão Voltar de software ou hardware facilmente usando a <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack">propriedade pilha voltar</a></strong> que permite que os usuários percorram o <strong>histórico de navegação</strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-history-and-backwards-navigation">Navegação do botão Voltar</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Tela inicial.</strong> <br><br>Exibição de uma imagem na inicialização do app, utilizada principalmente para identidade visual.</td>
<td align="left">As telas iniciais não são fornecidas por padrão e são implementadas editando a primeira <strong>tela de fundo do tema</strong> das atividades.</td>
<td align="left">Os apps devem ter uma <strong>imagem de inicialização estática</strong> ou um <strong>arquivo de inicialização XIB/storyboard</strong>.</td>
<td align="left">Você cria uma tela inicial usando uma <strong>imagem</strong> e uma tela de fundo colorida. <a href="https://docs.microsoft.com/windows/uwp/launch-resume/create-a-customized-splash-screen">A tela inicial pode ser exibida por mais tempo</a>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/add-a-splash-screen">Adicionar uma tela inicial</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">Entradas personalizadas</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Voz.</strong> <br><br>Reconhecimento de fala para entrada de fala e recursos adicionais de voz.</td>
<td align="left">A entrada de fala pode ser fornecida por qualquer app que implemente um <strong>RecognizerIntent</strong>, como a <strong>Pesquisa por voz do Google</strong>. A classe <strong>SpeechRecognizer</strong> permite que os apps usem a API de reconhecimento de fala do Google.</td>
<td align="left">Os aplicativos podem usar a classe <strong>SFSpeechRecognizer</strong> para implementar o reconhecimento de fala e entrada de fala.</td>
<td align="left">Você pode usar a API de <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/speech-recognition">reconhecimento de fala</a></strong> para interagir com seu app em primeiro plano. Você pode usar <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/cortana-interactions">interações da Cortana</a></strong> baseadas na fala para iniciar apps em primeiro ou segundo plano e interagir com apps em segundo plano.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions">Interações de controle por voz</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Entradas de usuário personalizadas.</strong> <br><br>Manipulação de teclado, mouse, caneta e outras entradas.</td>
<td align="left">O suporte para interações inclui <strong>toque</strong>, <strong>touchpad</strong>, <strong>caneta</strong>, <strong>mouse</strong> e <strong>teclado</strong>. Movimentos e entradas são relatados da mesma forma que o toque, mas é possível detectar mais informações sobre o <strong>dispositivo de entrada</strong>.</td>
<td align="left">É oferecido suporte para <strong>toque</strong>, <strong>Apple Pencil</strong> e <strong>teclados</strong> de hardware.</td>
<td align="left">Você encontrará suporte para uma ampla variedade de interações, incluindo <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions">toque</a></strong>, <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touchpad-interactions">touchpad</a></strong>, <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions">caneta/stylus</a></strong> com tinta digital, <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/mouse-interactions">mouse</a></strong> e <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions">teclado</a></strong>. Seus aplicativos podem manipular os dados sem precisar saber qual dispositivo de entrada foi usado e os dados brutos de dispositivos de entrada podem ser acessados quando necessário.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input">Identificar entrada do ponteiro</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/layout/index">Interações personalizadas do usuário</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Dados</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Dados do aplicativo local.</strong> <br><br>Armazenamento de configurações e arquivos relacionados a seu app localmente.</td>
<td align="left">Os arquivos locais podem ser salvos usando <strong>openFileOutput</strong> e <strong>openFileInput</strong>. As configurações em um <strong>arquivo de preferências compartilhadas</strong> podem ser acessadas usando <strong>getSharedPreferences</strong>.</td>
<td align="left">Os arquivos locais podem ser armazenados no diretório de <strong>suporte a apps</strong>, acessado por meio da classe <strong>NSFileManager</strong>. As configurações nos arquivos de <strong>preferências</strong> podem ser acessadas pela classe <strong>NSUserDefaults</strong>.</td>
<td align="left">As classes do <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Storage">Windows.Storage</a></strong> manipulam o armazenamento de dados locais para você de maneira unificada. Você armazena configurações como um objeto <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer">ApplicationDataContainer</a></strong> acessado por meio da propriedade <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings">ApplicationData.LocalSettings</a></strong> . Você armazena arquivos em um objeto <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.storagefolder">StorageFolder</a></strong> acessado por meio da propriedade <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData.LocalFolder</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">Armazene e recupere configurações e outros dados de aplicativo</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Armazenamento de banco de dados local.</strong> <br><br>Armazenamento de dados de apps em um banco de dados relacional, com mapeadores relacionais de objetos (ORM), se aplicável.</td>
<td align="left">O banco de dados <strong>SQLite</strong> é fornecido. Nenhum ORM é interno. As consultas SQL são executadas usando a classe <strong>SQLiteDatabase</strong>.</td>
<td align="left">O banco de dados <strong>SQLite</strong> é fornecido. <strong>CoreData</strong> é a estrutura de gráfico de objeto interna que pode ser usada com o SQLite e fornece funcionalidade comparável a um ORM.</td>
<td align="left">Você pode armazenar dados usando <strong>SQLite</strong>. <strong><a href="https://docs.microsoft.com/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> é um ORM interno que elimina a necessidade de escrever muitos códigos de acesso a dados e permite que você consulte facilmente o banco sem escrever o SQL. Você pode executar consultas SQL diretamente com a <a href="https://docs.microsoft.com/windows/uwp/data-access/sqlite-databases">biblioteca SQLite</a>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/data-access/index">Acesso a dados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bibliotecas http para acesso REST.</strong> <br><br>Bibliotecas internas que permitem que você se comunique com serviços web e servidores web usando HTTP(S).<br/></td>
<td align="left">Bibliotecas HTTP <strong>HttpURLConnection</strong> e <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> e <strong>NSURLDownload</strong>.</td>
<td align="left">Você pode usar a API interna <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient">HttpClient</a></strong> para acessar funcionalidades comuns de HTTP, incluindo GET, DELETE, PUT, POST, padrões comuns de autenticação, SSL, cookies e informações de progresso.</td>
</tr>
<tr class="even">
<td align="left"><strong>Serviços de backup na nuvem.</strong> <br><br>Serviços de backup fornecidos pela plataforma para dados de app.</td>
<td align="left">O <strong>gerenciador de backup</strong> do Android manipula o backup de dados de app no <strong>Android Backup Service</strong> do Google.</td>
<td align="left"><strong>o iCloud Backup</strong> pode ser configurado por um usuário para manipular seus backups, incluindo dados de app. Aplicativos que usam <strong>Core Data</strong> compatíveis com iCloud, o <strong>repositório de valor-chave do iCloud</strong> e o <strong>armazenamento de documentos do iCloud</strong>.</td>
<td align="left">Quaisquer dados de apps que você armazenar usando as <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata">APIs ApplicationData</a></strong> em roaming (incluindo <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> e <a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a>) serão automaticamente sincronizados na nuvem e nos outros dispositivos do usuário também. A sincronização é feita pela conta da Microsoft do usuário.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data">Diretrizes de dados de aplicativo em roaming</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Downloads de arquivo http.</strong> <br><br>Baixando arquivos grandes e pequenos por HTTP.</td>
<td align="left"><strong>URLConnection</strong> e <strong>HTTPURLConnection</strong> são usados para baixar por HTTP e FTP, também é possível usar o <strong>gerenciador de download</strong> do sistema para baixar em segundo plano.</td>
<td align="left"><strong>NSURLSession</strong> e <strong>NSURLConnection</strong> podem ser usados para baixar arquivos em HTTP e FTP.</td>
<td align="left">A <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer">API de transferência em segundo plano</a></strong> permite que você transfira arquivos por HTTP(S) e FTP fiavelmente, levando em consideração suspensão de app, perda de conectividade e ajustes com base na conectividade e na duração da bateria. Você também pode usar <strong><a href="https://docs.microsoft.com/uwp/api/windows.web.http.httpclient">HttpClient</a></strong> que é ideal para arquivos menores.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">Qual tecnologia de rede?</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/background-transfers">Transferências em segundo plano</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Soquetes.</strong> <br><br>Criação de datagramas UDP e soquetes TCP de baixo nível para se comunicar com outros dispositivos usando seu próprio protocolo.</td>
<td align="left">A classe <strong>Socket</strong> fornece soquetes TCP, a classe <strong>DatagramSocket</strong> fornece um soquete UDP.</td>
<td align="left"><strong>NSStream</strong> e <strong>CFStream</strong> fornecem soquetes TCP, <strong>CFSocket</strong> fornece soquetes UDP.</td>
<td align="left">Você pode usar a classe <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> para se comunicar usando um soquete de datagrama UDP e a classe <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket">StreamSocket</a></strong> para se comunicar por TCP ou Bluetooth RFCOMM.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/networking-basics">Noções básicas de rede</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">Qual tecnologia de rede?</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/sockets">Visão geral de soquetes</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Fornece comunicação bidirecional entre um cliente e servidor, permitindo a transferência de dados em tempo real.</td>
<td align="left">Não há bibliotecas de WebSockets internas no Android.</td>
<td align="left">Não há bibliotecas de WebSockets internas no iOS.</td>
<td align="left">Conexões seguras para servidores que oferecem suporte a WebSockets podem ser feitas com a classe <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.sockets.messagewebsocket">MessageWebSocket</a></strong> para mensagens menores com notificações de confirmação e <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocket">StreamWebSocket</a></strong> para transferências binárias de arquivos maiores que podem ser lidas em seções.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/networking-basics">Noções básicas de rede</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">Qual tecnologia de rede?</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/websockets">Visão geral de WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Bibliotecas OAuth.</strong> <br><br>As bibliotecas OAuth permitem acesso a provedores OAuth de terceiros e qualquer gerenciamento de conta criado na plataforma.</td>
<td align="left">Nenhuma biblioteca OAuth genérica é fornecida. A classe <strong>GoogleAuthUtil</strong> é fornecida para autenticação OAuth com o Google Play Services.<br/></td>
<td align="left">Nenhuma biblioteca OAuth genérica é fornecida. A <strong>estrutura de contas</strong> fornece acesso a contas de usuário já armazenados no dispositivo, como Facebook e Twitter.</td>
<td align="left">A biblioteca OAuth genérica <strong><a href="https://docs.microsoft.com/windows/uwp/security/web-authentication-broker">agente de autenticação da Web</a></strong> permite que você se conecte a serviços de provedores de identidade de terceiros. O <strong><a href="https://docs.microsoft.com/windows/uwp/security/credential-locker">Cofre de credenciais</a></strong> permite que os usuários salvem o logon deles e utilizem-no em vários dispositivos. O namespace <strong><a href="https://docs.microsoft.com/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft.Live</a></strong> permite que você acesse facilmente o OAuth Live SDK para obter acesso aos serviços Microsoft.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/security/authentication-and-user-identity">Autenticação e identidade do usuário</a><br/><br/><a href="https://docs.microsoft.com/uwp/api/windows.security.authentication.web">Documentação API do Windows.Security.Authentication.Web</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">Exemplo de código do WebAuthenticationBroker</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Ferramentas</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>O conjunto de ferramentas usado para criar seu app.</td>
<td align="left"><strong>Android Studio</strong> e <strong>Eclipse</strong>, com o Google impulsionando os desenvolvedores em direção ao uso do Android Studio.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left">O <strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> e o <strong><a href="https://docs.microsoft.com/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend para Visual Studio</a></strong> tem todas as ferramentas de que você precisa para codificar, projetar, conectar, depurar, analisar, otimizar e testar aplicativos UWP. O Visual Studio também oferece <strong><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/test-with-the-emulator">emuladores</a></strong> para dispositivos Windows 10, para que você possa testar seu app em uma ampla gama de dispositivos emulados.<br/><br/><a href="https://developer.microsoft.com/windows/downloads">Downloads e ferramentas para UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Organização de código.</strong> <br><br>A estrutura de pastas básica de um app, geralmente criada a partir de um modelo inicial.</td>
<td align="left"><strong>Arquivo AndroidManifest</strong>, pasta <strong>java</strong> contendo arquivos de origem, pasta <strong>res</strong> com recursos, incluindo layouts e valores, scripts criados em <strong>Gradle</strong> no Android Studio e scripts <strong>Ant</strong> criados em Eclipse.</td>
<td align="left">Arquivos de origem e <strong>arquivos de suporte</strong>, arquivo <strong>info.plist</strong>, <strong>Main.storyboard</strong> e <strong>LaunchScreen.storyboard</strong>. As imagens são armazenadas em <strong>bibliotecas de ativos</strong>.</td>
<td align="left">Seu aplicativo UWP contém arquivos de código e XAML para seu aplicativo chamado Example.xaml e Example.xaml.cs, várias imagens na <strong>pasta de ativos</strong>, uma página inicial como <strong>MainPage. xaml</strong> e <strong>MainPage.xaml.cs</strong> e um manifesto.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">Criar um aplicativo "Hello, world"</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">Ciclo de vida do app</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ciclo de vida do aplicativo.</strong> <br><br>Tratar eventos na inicialização, na suspensão, na retomada e no fechamento do app, fornecendo uma oportunidade de salvar/restaurar o estado do app e executar outras tarefas.</td>
<td align="left">Cada atividade tem seu próprio <strong>ciclo de vida de atividade</strong> com estados como <strong>reiniciado</strong>. <strong>Retornos de chamada do ciclo de vida</strong> , como <strong>OnContinue</strong> , são implementados em suas <strong>classes de atividade</strong>.</td>
<td align="left">O <strong>ciclo de vida do app</strong> tem estados como <strong>suspenso</strong>. Métodos como <strong>applicationDidEnterBackground:</strong> são implementados no <strong>objeto delegado do app</strong> para executar códigos em alterações de estado.</td>
<td align="left">Seu app tem os <strong>estados de execução do app</strong> NotRunning, Activated, Running, Suspending, Suspended e Resuming.<br/><br/>Você pode implementar os métodos da <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application">classe de app</a></strong> OnLaunched, OnActivated, Suspending ou Resuming em seu app para executar códigos quando o estado é alterado.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle">Ciclo de vida do aplicativo</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Tarefas em segundo plano.</strong> <br><br>Tarefas que executam operações em segundo plano e continuam a ser executada quando o app não está mais em primeiro plano.</td>
<td align="left">Os apps podem iniciar <strong>serviços</strong> que executem operações em segundo plano quando o app não estiver mais em primeiro plano. Os serviços têm seu próprio <strong>ciclo de vida</strong> e são registrados no manifesto.</td>
<td align="left"><strong>Execução de plano de fundo</strong> é permitida somente para tipos de tarefa específicos.<br/><br/>Os apps declaram as <strong>tarefas em segundo plano suportadas</strong> no arquivo Info.plist usando o <strong>UIBackgroundModes</strong>.<br/><br/>O sistema controla quando as tarefas em segundo plano são executadas e por quanto tempo.</td>
<td align="left">Você pode criar uma tarefa em segundo plano implementando a interface <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask">IBackgroundTask</a></strong> interface e registrando a tarefa no manifesto do app. Você pode definir uma tarefa para ser disparada com um <a href="https://docs.microsoft.com/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>temporizador</strong></a>, <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>gatilho de sistema</strong></a> e um <a href="https://docs.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>gatilho de manutenção</strong></a>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks">Oferecer suporte a tarefas em segundo plano em seu aplicativo</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task">Criar e registrar uma tarefa em segundo plano</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/guidelines-for-background-tasks">Diretrizes para tarefas em segundo plano</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">Desempenho</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Práticas recomendadas de desempenho.</strong> <br><br>Diretrizes para criar apps rápidos, responsivos, sensíveis à duração da bateria e com tempo de início rápido.</td>
<td align="left">O Android oferece o guia de treinamento para <strong>Práticas recomendadas de desempenho</strong>.</td>
<td align="left">O iOS fornece o documento <strong>Visão geral de desempenho</strong>.</td>
<td align="left">Você pode ler o <strong><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui">guia de desempenho</a></strong> detalhado com seções que abordam tópicos como definição de metas de desempenho, medição de desempenho, gerenciamento de memória, animações suaves, acesso eficiente ao sistema de arquivos e as ferramentas disponíveis para perfil e desempenho.</td>
</tr>
<tr class="even">
<td align="left"><strong>Exibir otimização para uma interface do usuário responsiva.</strong> <br><br>Melhorar o desempenho por meio da otimização dos modos de exibição.</td>
<td align="left">Otimizar as <strong>hierarquias de layout</strong> usando a ferramenta Hierarchy Viewer, <strong>reutilizar layouts</strong> e carregar <strong>modos de exibição sob demanda</strong> são técnicas para ajudar a manter o thread de interface do usuário responsivo e evitar caixas de diálogo &quot;Aplicativo não está respondendo&quot; (<strong>ANR</strong>).<br/></td>
<td align="left">Corrigir problemas da interface do usuário com <strong>renderização fora da tela</strong>, <strong>camadas combinadas</strong>, <strong>rasterização</strong> usando a ferramenta <strong>Core Animation</strong> ajuda a manter o thread de interface do usuário responsivo.</td>
<td align="left">Você pode <strong>otimizar</strong> com facilidade <strong>marcações</strong> e <strong>layouts</strong> XAML seguindo algumas etapas simples. As técnicas incluem reduzir a estrutura de layout, minimizar a contagem de elementos e minimizar o excesso de desenhos. <br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">Mantenha o thread de interface do usuário responsivo</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-xaml-loading">Otimizar sua marcação XAML</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-your-xaml-layout">Otimizar o layout XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Threading.</strong> <br><br>Uso de threading para manter uma <strong>interface de usuário responsiva</strong> e executar várias <strong>tarefas em paralelo</strong>.</td>
<td align="left">O threading é obtido usando as classes <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong>e, em nível mais alto, <strong>AsyncTask</strong>.</td>
<td align="left">O threading é obtido usando <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong>e, em nível mais alto, <strong>NSOperation</strong>.</td>
<td align="left">Você pode trabalhar com threads enviando <strong>itens de trabalho</strong> para o <strong>threadpool</strong> com <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync">RunAsync</a></strong>. Você pode usar um temporizador para enviar um item de trabalho com <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer">CreateTimer</a></strong> e criar um item de trabalho repetitivo com <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">Enviar um item de trabalho ao pool de threads</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">Usar um temporizador para enviar um item de trabalho</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/create-a-periodic-work-item">Criar um item de trabalho periódico</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">Práticas recomendadas para usar o pool de threads</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Programação assíncrona.</strong> <br><br>Evite complexidade de threading aproveitando os padrões de programação assíncrona para manter o thread de interface do usuário responsivo.</td>
<td align="left">O uso de <strong>threading é obrigatório</strong> para criar suas próprias classes assíncronas. Algumas classes internas são assíncronas.</td>
<td align="left">O uso de <strong>threading é obrigatório</strong> para criar suas próprias classes assíncronas. Algumas classes internas são assíncronas.</td>
<td align="left">Você pode usar padrões assíncronos para evitar o bloqueio do thread principal ao criar suas próprias APIs, por exemplo, usando <strong>Async</strong> e <strong>await</strong> no C# e Visual Basic. Você pode usar as APIs internas assíncronas que terminam com a palavra <strong>Async</strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Programação assíncrona</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">Chamar APIs assíncronas no Visual Basic ou C#</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Otimização da exibição de lista.</strong> <br><br>Padrões internos para ajudar com a otimização de listas de dados, que costumam ter desempenho insatisfatório quando grandes quantidades de dados precisam ser mostradas</td>
<td align="left">O padrão de design <strong>ViewHolder</strong> é usado para evitar várias pesquisas de modo de exibição, permitindo que você use elementos de interface do usuário reutilizáveis.</td>
<td align="left">Várias otimizações podem ser feitas para melhorar o desempenho do <strong>UITableView</strong>, sendo que nada é interno.</td>
<td align="left">Você pode usar os controles <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview">ListView</a> e <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview">GridView</a> que fornecem <strong>virtualização de interface do usuário</strong> imediata, fornecendo uma experiência fluida de movimento panorâmico e rolagem e um tempo de inicialização mais rápido. Você também pode implementar <a href="https://docs.microsoft.com/dotnet/api/system.collections.ilist">IList</a> e <a href="https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a> em sua fonte de dados, fornecendo <strong>virtualização de dados</strong> e melhorando ainda mais o desempenho.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview">Otimização das interfaces do usuário ListView e GridView</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">Virtualização de dados de ListView e GridView</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">Monetização</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Compras no aplicativo.</strong> <br><br>Recursos da plataforma que permitem aos usuários fazer compras em seus apps.</td>
<td align="left"><strong>In-app billing</strong> é fornecida pelos serviços do Google. Os produtos são adicionados ao <strong>Console do desenvolvedor do Google Play</strong>. As compras no app são implementadas com a <strong>Biblioteca de cobranças do Google Play</strong>.</td>
<td align="left">Os produtos são adicionados ao <strong>iTunes Connect</strong>. As compras no app são implementadas usando a estrutura do <strong>StoreKit</strong>.<br/><br/>Os produtos são adquiridos usando <strong>SKMutablePayment</strong> e <strong>SKPaymentQueue</strong>.</td>
<td align="left">Você cria compras de produto no app para seu app <a href="https://docs.microsoft.com/windows/uwp/publish/iap-submissions">adicionando-os ao seu app e enviando-os para a Loja</a>. <br/><br/>Você usa a <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp">classe CurrentApp</a></strong> para definir as compras no app. <br/><br/>Você usa <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">CurrentApp.RequestProductPurchaseAsync</a></strong> para exibir a interface do usuário que permite que os clientes comprem o produto.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases">Habilitar compras de produtos no aplicativo</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Compras no aplicativo consumíveis.</strong> <br><br>Os produtos no app que podem ser comprados, usados e depois comprados novamente.</td>
<td align="left">As compras de consumíveis são habilitadas fazendo uma compra normal e consumindo-a com <strong>consumePurchase</strong>, permitindo que ele seja comprado, usado e depois comprado novamente.</td>
<td align="left">Produtos consumíveis são <strong>definidos como produtos consumíveis</strong> no iTunes Connect.</td>
<td align="left">Você pode dar suporte a consumíveis <a href="https://docs.microsoft.com/windows/uwp/publish/enter-iap-properties">definindo o tipo de produto como Consumível ao enviá-lo para</a> a Loja. Em seguida, chame <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">CurrentApp.ReportConsumableFulfillmentAsync</a></strong> após uma compra de consumível ter sido feita para permitir que o cliente o acesse.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Habilitar compras de produtos consumíveis no aplicativo</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Testando compras no aplicativo.</strong> <br><br>Permite que você teste seu código de compra no app sem colocar seu app na Loja.</td>
<td align="left">A <strong>área restrita de cobrança no app</strong> é usada para testes.</td>
<td align="left"><strong>As contas de testador da área restrita</strong> são usadas para teste.</td>
<td align="left">Você pode testar compras no app simplesmente usando a classe <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator">CurrentAppSimulator</a></strong> em vez de CurrentApp.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Avaliações.</strong> <br><br>Permite que você limite o conteúdo ou remova anúncios facilmente com base em uma versão de avaliação de um app.</td>
<td align="left">O Google Play <strong>oficialmente não dá suporte a versões de avaliação do app</strong>. As avaliações ou a remoção de publicidade são obtidas com a criação de uma compra no app e colocando o caminho de código adequado no momento da confirmação de compra bem-sucedida.</td>
<td align="left">A App Store <strong>oficialmente não dá suporte a versões de avaliação do app</strong>. As avaliações ou a remoção de publicidade são obtidas com a criação de uma compra no app e colocando o caminho de código adequado no momento da confirmação de compra bem-sucedida.</td>
<td align="left">Você pode oferecer uma versão de avaliação gratuita do seu app usando a <strong><a href="https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability">opção de avaliação gratuita</a></strong> ao enviar seu app para a Loja. Você pode usar <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation.IsTrial</a></strong> para verificar o status de avaliação do app e apresentar diferentes caminhos de código adequadamente. Você pode se registrar no <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">evento LicenseChanged</a> para ser notificado quando o usuário altera o status de avaliação enquanto o app está sendo executado.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">Excluir ou limitar recursos em uma versão de avaliação</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">Adaptação a várias plataformas</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interface do usuário adaptável: layouts flexíveis.</strong> <br><br>Oferece suporte a diferentes tamanhos de tela com altura e largura flexíveis.</td>
<td align="left">Os layouts flexíveis podem ser alcançados usando os valores <strong>wrap_content</strong> e <strong>match_parent</strong> em objetos de LinearLayout ou fazendo uso de objetos RelativeLayout para alinhamento.</td>
<td align="left">Os layouts flexíveis podem ser alcançados usando o <strong>modelo adaptável</strong> com Storyboards universais, fazendo uso do <strong>Auto Layout</strong> com <strong>restrições</strong> e <strong>características</strong> como horizontalSizeClass e displayScale que são aplicadas para exibir controladores.</td>
<td align="left">Você pode criar um layout fluido usando <strong>propriedades</strong> e <strong>painéis</strong> de layout com uma combinação de tamanho fixo e dinâmico.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">Definir layouts com XAML - Painéis e propriedades de layout</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Design responsivo 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Interface do usuário adaptável: layouts personalizados.</strong> <br><br>Oferece suporte a diferentes tamanhos de tela com layouts de destino separados.</td>
<td align="left">Fornecer arquivos de layout alternativos para configurações de tela diferentes no diretório de recursos usando <strong>qualificadores de configuração</strong> como <strong>pequeno</strong>, <strong>grande</strong>, <strong>ldpi</strong> e <strong>hdpi</strong> permite que você se direcione a layouts personalizados para telas de tamanhos e densidade variados.</td>
<td align="left">Definir um <strong>Storyboard separado para iPhone e iPad</strong> para adaptar layouts personalizados para diferentes famílias de dispositivos em um app universal.</td>
<td align="left">Você pode criar um layout adaptado definindo <strong>diferentes arquivos de marcação XAML</strong> por família de dispositivos.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">Definir layouts com XAML - layouts adaptados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interface do usuário adaptável: layouts responsivos.</strong> <br><br>Responde a alterações no tamanho da tela, como rotação, ou uma mudança no tamanho de uma janela.</td>
<td align="left">O uso de layouts flexíveis com <strong>LinearLayout</strong> e <strong>RelativeLayout</strong> ou o fornecimento de arquivos de layout alternativos para diferentes orientações permitem layouts dinâmicos.</td>
<td align="left">Quando o <strong>tamanho</strong> ou as <strong>características</strong> de um modo de exibição mudam, as <strong>restrições</strong> especificadas em storyboards são aplicadas.</td>
<td align="left">Você pode facilmente refluir, reposicionar, redimensionar, revelar ou substituir seções de sua interface do usuário em tempo de execução em resposta às alterações no tamanho de janela usando <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate">VisualState</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">Definir layouts com XAML - estados visuais e disparadores de estado</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Design responsivo 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Suporte a diferentes recursos de dispositivo.</strong> <br><br>Tire proveito de recursos de hardware avançados e ainda ofereça suporte a dispositivos sem eles.</td>
<td align="left">O teste de recursos de dispositivo em tempo de execução usando <strong>PackageManager.hasSystemFeature</strong> permite que você decida se é possível executar um código específico de hardware.</td>
<td align="left">Não há <strong>nenhuma verificação única</strong> que você possa executar em tempo de execução para testar recursos de dispositivo, você faz o teste para cada recurso de uma maneira específica para decidir se um código específico de hardware pode ser executado.</td>
<td align="left">Você pode adicionar <strong>SDKs de extensão de plataforma</strong> a seu pacote para atingir a funcionalidade adicional encontrada em famílias de dispositivos diferentes, incluindo celular, desktop e IoT. Você usa a <strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation">API ApiInformation</a></strong> para testar a presença de tipos e membros em tempo de execução e pode chamar esses tipos e membros somente se eles estiverem presentes.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Suporte a diferentes recursos de dispositivo.</strong> <br><br>Tire proveito de recursos de hardware avançados e ainda ofereça suporte a dispositivos sem eles.</td>
<td align="left">A <strong>biblioteca de suporte do Android</strong> pode ser empacotada com seu app para tornar algumas APIs mais recentes disponíveis para aqueles com versões mais antigas do Android. Os testes para o nível de API em tempo de execução podem ser feitos usando <strong>Build.Version.SDK_INT</strong>.</td>
<td align="left">As verificações de tempo de execução padrão são usadas para descobrir se APIs estão disponíveis, como o método de <strong>classe</strong> para verificar se existe uma classe e <strong>respondsToSelector:</strong> para verificar se há métodos nas classes.</td>
<td align="left">Você pode usar <strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">ApiInformation.IsApiContractPresent</a></strong> para identificar se há um contrato de API com um número principal e secundário especificado. Você também usa a <strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation">API ApiInformation</a></strong> para testar a presença de tipos e membros em tempo de execução e pode chamar esses tipos e membros somente se eles estiverem presentes.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">Notificações</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Blocos e notificações.</strong> <br><br>Apresentar atualizações aos usuários na tela inicial.</td>
<td align="left"><strong>App Widgets</strong> são modos de exibição em seu app que podem ser inseridos na tela inicial e podem receber atualizações periódicas. Não há <strong>sistema de selos</strong> no Android. Não há nenhum sistema idêntico a blocos.</td>
  <td align="left"><strong>Widgets</strong> no iOS são mapeados no Centro de Notificações e implementados como <strong>Extensões de aplicativo</strong>. Você também pode adicionar um <strong>selo</strong> em seu ícone com um número que pode mudar em resposta a notificações locais ou remotas. Não há um sistema de blocos.</td>
<td align="left">Seu app tem um <strong>bloco</strong> que pode ser fixado na tela inicial e é usado para exibir sua opção de texto, imagens e um <strong>selo</strong> com glifos e números. Você pode atualizar o conteúdo dos blocos do app por meio de notificações por push ou em agendamentos predefinidos. Os blocos podem ser adaptáveis e podem mudar de acordo com onde eles estão sendo exibidos.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Criar blocos</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">Criar blocos adaptáveis</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Escolher um método de entrega de notificação</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Diretrizes de blocos e notificações</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Exibindo notificações.</strong> <br><br>Tipos de notificações que podem ser exibidas.</td>
<td align="left">As notificações podem ser mostradas na <strong>área de notificação</strong> e na <strong>gaveta de notificação</strong>, as <strong>notificações heads-up</strong> apresentam uma notificação em uma pequena janela flutuante. As notificações podem ter ações adicionadas a elas, definindo um <strong>PendingIntent</strong>.</td>
<td align="left">As notificações pop-up aparecem como <strong>banners</strong> ou <strong>alertas</strong>. Você pode adicionar botões de ação personalizados para <strong>notificações acionáveis</strong> que são definidas com <strong>UIMutableUserNotificationAction</strong>.</td>
<td align="left">Você pode criar notificações pop-up adaptáveis chamadas <strong>notificações do sistema</strong>. Você pode definir as notificações do sistema em XML com conteúdo visual, <strong>ações</strong> que podem ser botões, ou entradas e áudio.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Notificações do sistema interativas e adaptáveis</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Escolher um método de entrega de notificação</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications">Diretrizes para notificações do sistema</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Agendando notificações locais.</strong> <br><br>Notificações locais enviadas pelo seu app em um horário agendado.</td>
<td align="left">As notificações e as ações são definidas usando um <strong>NotificationCompat.Builder</strong> e podem ser agendadas e manipuladas no app usando <strong>AlarmManager</strong> e <strong>BroadcastReceiver</strong>.</td>
<td align="left">Notificações locais são criadas usando <strong>UILocalNotification</strong> e podem ser agendadas com <b>UILocalNotification.scheduleLocalNotification:<strong>. | Você pode agendar uma notificação do sistema usando</strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a><strong>. Você pode enviar uma notificação de bloco a partir do seu app usando a</strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification">classe TileNotification</a><strong>, ou agendar uma notificação de bloco com <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Notificações do sistema interativas e adaptáveis</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">Enviar uma notificação de bloco local</a> | | </strong>enviar notificações por push.</b> Uma notificação enviada de um servidor de notificação por push e, opcionalmente, manipulada no aplicativo.</td>
<td align="left"><strong>Google Cloud Messaging</strong> oferece suporte a notificação por push para Android.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">Captura e renderização de mídia</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Capturando mídia.</strong> <br><br>Gravação de conteúdo de áudio e vídeo.</td>
<td align="left">Usar uma <strong>intenção</strong> como MediaStore.ACTION_VIDEO_CAPTURE permite que a mídia seja capturada com um app de câmera existente. Usar a <strong>android.hardware.camera2</strong> ou a biblioteca de <strong>câmera</strong> permite a implementação de uma interface de câmera personalizada. APIs <strong>MediaRecorder</strong> podem ser usadas para capturar áudio.</td>
<td align="left">O <strong>UIImagePickerController</strong> permite a captura de vídeo e fotos com a interface do usuário do sistema. As classes <strong>AVFoundation</strong> como <strong>AVCaptureSession</strong> permitem acesso direto à câmera. <br/>A classe <strong>AVAudioRecorder</strong> permite a gravação de áudio.</td>
<td align="left">Você pode capturar fotos e vídeos enquanto usa a interface do usuário da câmera interna com a <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI">classe CameraCaptureUI</a></strong>. Você pode interagir com a câmera em baixo nível e capturar áudio com classes no <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture">Windows.Media.Capture</a></strong> como a <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture">API MediaCapture</a></strong>. <br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">Capturar fotos e vídeos com CameraCaptureUI</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">Capturar fotos e vídeos com o MediaCapture</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Reprodução de mídia.</strong> <br><br>Reprodução de arquivos de áudio e vídeo.</td>
<td align="left">As classes <strong>MediaPlayer</strong> e <strong>AudioManager</strong> são usadas para reproduzir arquivos de áudio e vídeo.</td>
<td align="left">A <strong>estrutura AVKit</strong>, o <strong>AVAudioPlayer</strong> e a <strong>estrutura Media Player</strong> são usados para reproduzir arquivos de áudio e vídeo.</td>
<td align="left">Você pode usar as classes <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource">MediaSource class</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> para reproduzir áudio e vídeo de fontes como arquivos locais e remotos.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource">Reprodução de mídia com o MediaSource</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Editando mídia.</strong> <br><br>Composição de novos arquivos de mídia a partir de gravações existentes e aplicação de efeitos especiais.</td>
<td align="left">Classes de baixo nível, como <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> e <strong>android.media.effect</strong>, podem ser usadas para edição de conteúdo.</td>
<td align="left">As classes na estrutura <strong>AV Foundation</strong>, como <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> e <strong>AVMutableAudioMix</strong> podem ser usadas para edição de conteúdo.</td>
<td align="left">Você pode usar as APIs <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing">Windows.Media.Editing</a></strong> como <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition">MediaComposition</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip">MediaClip</a></strong> para criar composições de mídia a partir de arquivos de áudio e vídeo. Você é capaz de adicionar sobreposições de vídeo e imagem, combinar videoclipes, adicionar áudio em segundo plano e aplicar efeitos de áudio e vídeo.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-compositions-and-editing">Composições de mídia e edição</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">Sensores</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Sensores.</strong> <br><br>Detectam os movimentos e a posição do dispositivo, além das condições ambientais.</td>
<td align="left">A <strong>estrutura de sensor</strong> é usada para acessar sensores de hardware e software com classes como <strong>SensorManager</strong> e <strong>SensorEvent</strong>.</td>
<td align="left">A <strong>estrutura Core Motion</strong> é usada para acessar dados de sensores brutos e processados.</td>
<td align="left">Você pode usar classes em <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.sensors">Windows.Devices.Sensors</a></strong> para acessar leituras do sensor e eventos disparados quando novos dados de leitura são recebidos do sensor.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/devices-sensors/sensors">Sensores</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">Localização e mapeamento</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Local.</strong> <br><br>Encontrar a localização <strong>atual</strong> do dispositivo e rastrear as <strong>alterações</strong>.</td>
<td align="left">As APIs de localização do Google Play Services fornecem acesso de alto nível à <strong>última localização conhecida</strong> com o <strong>provedor de localização fundido</strong> usando os métodos <strong>getLastLocation</strong> e <strong>requestLocationUpdates</strong>. O acesso de baixo nível é fornecido nas bibliotecas do Android com o <strong>LocationManager</strong>.</td>
<td align="left">A classe <strong>CLLocationManager</strong> do <strong>local principal</strong> é usada para monitorar o local de um dispositivo, com <strong>startUpdatingLocation</strong> para o serviço local padrão e <strong>startMonitoringSignificantLocationChanges</strong> para o serviço de localização de <strong>alteração significativa</strong> .</td>
<td align="left">Você pode controlar a localização do dispositivo com as classes no <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation">Windows.Devices.Geolocation</a></strong>. Use <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">Geolocator.GetGeopositionAsync</a></strong> para uma única leitura. Use <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged">Geolocator.PositionChanged</a></strong> para obter a localização usando o temporizador regularmente ou ser informado quando a localização for alterada.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/get-location">Obter a localização do usuário</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Exibindo mapas.</strong> <br><br>Exibir um <strong>mapa interativo interno</strong> e adicionar <strong>pontos de interesse</strong>.</td>
<td align="left">As classes <strong>GoogleMap</strong>, <strong>MapFragment</strong> e <strong>MapView</strong> dentro da <strong>API Google Maps Android</strong> permitem que mapas sejam inserido em apps. Os pontos de interesse podem ser exibidos usando <strong>marcadores</strong> e a classe personalizável <strong>Marker</strong>.</td>
<td align="left">Os mapas são incorporados em apps do iOS com a classe <strong>MKMapView</strong> na <strong>estrutura MapKit</strong>. <strong>1Anotações</strong> podem ser adicionadas a aplicativos para exibir pontos de interesse usando classes de objeto como <strong>MKPointAnnotation</strong> e classes de exibição como <strong>MKPinAnnotationView</strong>.</td>
<td align="left">Você pode incorporar mapas em seus apps usando o controle XAML integrado <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">MapControl</a></strong> que fornece visualizações 2D, 3D e streetside. Você pode adicionar pontos de interesse com um alfinete, uma imagem ou uma forma usando classes como <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon">MapIcon</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline">MapPolyline</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps">Exibir mapas com modos de exibição 2D, 3D e Streetside</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/display-poi">Exibir pontos de interesse (POI) em um mapa</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Isolamento geográfico.</strong> <br><br>Monitora a entrada e a saída de uma determinada região geográfica.</td>
<td align="left">As cercas geográficas são monitoradas usando os <strong>serviços de localização</strong> no SDK do Google Play Services.</td>
<td align="left">As regiões são monitoradas com a classe <strong>CLCircularRegion</strong> e registradas com o <strong>CLLocationManager.startMonitoringForRegion:</strong>.</td>
<td align="left">Você pode criar uma cerca geográfica com a classe <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence">Geofence</a></strong> e definir seus <strong>estados monitorados</strong> como entrada ou saída de uma região. Manipule eventos de cerca geográfica em primeiro plano com a <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">classe GeofenceMonitor</a></strong>, e em segundo plano com a <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.locationtrigger">classe de segundo plano LocationTrigger</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence">Configurar uma cerca geográfica</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Geocodificação e inverter geocodificação.</strong> <br><br>Conversão de endereços em localizações geográficas (geocodificação) e conversão de localizações geográficas em endereços (geocodificação reversa).<br/></td>
<td align="left">A classe <strong>Geocoder</strong> é usada para geocodificação e geocodificação reversa.</td>
<td align="left">A classe <strong>CLGeocoder</strong> é usada para geocodificação.</td>
<td align="left">Você pode executar geocodificação usando a <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder">classe MapLocationFinder</a></strong> no <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps">Windows.Services.Maps</a></strong>. Você usa <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> para geocodificação e <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> para geocodificação reversa.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/geocoding">Executar geocodificação e geocodificação reversa</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Rotas e direções.</strong> <br><br>Fornecem rotas, distâncias e trajetos entre duas localizações geográficas.</td>
<td align="left">O Google fornece o serviço web <strong>Google Maps Directions API</strong> que pode ser usado no Android, embora nenhum SDK seja fornecido.</td>
<td align="left">O Map Kit fornece a API <strong>MKDirections</strong> que pode ser usada para obter informações sobre uma rota e trajetos.</td>
<td align="left">Você pode solicitar uma rota a pé ou de carro com a classe <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder</a></strong> no <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps">Windows.Services.Maps</a></strong>. As rotas são retornadas como uma instância <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproute">MapRoute</a></strong> que pode ser facilmente mostrada em um MapControl. Os trajetos são retornados dentro do objeto <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/routes-and-directions">Exibir rotas e trajetos em um mapa</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">Comunicação entre aplicativos</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Invocando outro aplicativo.</strong> <br><br>Iniciar outro app e, opcionalmente, compartilhar dados como links, texto, fotos, vídeos e arquivos.</td>
<td align="left">Uma <strong>intenção implícita</strong> é usada para iniciar outro app, definindo uma <strong>ação</strong> e dados opcionais em uma <strong>intenção</strong> e chamando com <strong>startActivityForResult</strong>.<br/></td>
<td align="left"><strong>Extensões de app</strong> podem ser usadas para fornecer acesso a dados de app para outro app. <strong>Esquemas de URL</strong> habilitam uma URL a ser enviada para outro app.</td>
<td align="left">Você pode iniciar outro app que foi registrado para um URI com <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync">Launcher.LaunchUriAsync</a></strong> ou <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriforresultsasync">Launcher.LaunchUriForResultsAsync</a></strong> para iniciar por resultados e obter dados de volta do app iniciado. Você pode usar <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync">Launcher.LaunchFileAsync</a></strong> para transmitir um arquivo para outro app para tratar.<br/><br/>Você pode usar um <strong>contrato de compartilhamento</strong> para compartilhar dados entre apps com facilidade.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/launch-default-app">Iniciar o aplicativo padrão para um URI</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Iniciar um aplicativo para obter resultados</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/launch-the-default-app-for-a-file">Iniciar o aplicativo padrão para um arquivo</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/share-data">Compartilhar dados</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Permitir que seu aplicativo seja invocado.</strong> <br><br>Permitir que seu app responda a uma solicitação de outro app.</td>
<td align="left">Os apps registram uma <strong>atividade de manipulação de intenção</strong> com um <strong>filtro de intenção</strong> para responder a uma intenção implícita de outro app.</td>
<td align="left">Empacotar uma <strong>extensão de app</strong> permite que os dados sejam compartilhados com outros apps. Os apps podem registrar um <strong>esquema de URL personalizado</strong> usando a chave <strong>CFBundleURLTypes</strong> em Info.plist.</td>
<td align="left">Você pode registrar seu app para ser o manipulador padrão de um <strong>nome de esquema de URI</strong> registrando um <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">protocolo</a></strong> no manifesto de pacote e atualizando o manipulador de evento <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong>, retornando os resultados opcionalmente. Da mesma forma, você pode registrar seu app para ser o manipulador padrão para certos tipos de arquivo, adicionando uma declaração no manifesto do pacote e manipulando o evento <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated">Application.OnFileActivated</a></strong>.<br/><br/>Você pode tratar solicitações de contrato de compartilhamento registrando seu app como um destino de compartilhamento no manifesto e manipulando o evento <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated">Application.OnShareTargetActivated</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Iniciar um aplicativo para obter resultados</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/handle-file-activation">Manipular a ativação do arquivo</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/receive-data">Receber dados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Copiar e colar.</strong> <br><br>Copiar e colar texto e outros tipos de conteúdo entre apps.</td>
<td align="left">A <strong>estrutura de área de transferência</strong> pode ser usada para implementar copiar e colar com as classes <strong>ClipboardManager</strong> e <strong>ClipData</strong>.</td>
<td align="left">Os <strong>UIPasteboard</strong>, <strong>UIMenuController</strong> e <strong>UIResponderStandardEditActions</strong> podem ser usados para implementar copiar e colar.</td>
<td align="left">Muitos controles XAML padrão já suportam copiar e colar. Você pode implementar copiar e colar por conta própria usando as classes <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong> em <strong><a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer">Windows.ApplicationModel.DataTransfer</a></strong>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/copy-and-paste">Copiar e colar</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Arrastar e soltar.</strong> <br><br>Arrastar e soltar conteúdo entre apps.</td>
<td align="left">Arrastar e soltar podem ser implementados em um único app usando a <strong>estrutura de arrastar e soltar do Android</strong>.</td>
<td align="left">Nenhuma API de alto nível de arrastar e soltar é fornecida pelo iOS.</td>
<td align="left">Você pode implementar arrastar e soltar em seu app para habilitar as capacidades app para app, área de trabalho para app e app para área de trabalho. Você implementa o suporte para arrastar e soltar na classe UIElement com as propriedades <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop">AllowDrop</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.candrag">CanDrag</a></strong>, e os eventos <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover">DragOver</a></strong> e <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop">Drop</a></strong> .<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop">Arrastar e soltar</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">Design de software</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Conceito geral</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP do Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Padrões de design de software.</strong> <br><br>Padrões recomendados ou utilizados com frequência para a plataforma.</td>
<td align="left">Nenhum padrão formal foi recomendado ou fornecido para desenvolvimento do Android, embora a estrutura de vinculação de dados beta possa habilitar um uso mais amplo do padrão <strong>Model-View-ViewModel (MVVM)</strong>. Vários artigos e estruturas de terceiros recomendam as abordagens <strong>Model-View-Presenter (MVP)</strong> e <strong>MVVM</strong>.</td>
<td align="left"><strong>Model-View-Controller (MVC)</strong> é um padrão comum usado com iOS e está integrado à plataforma.</td>
<td align="left">Você não fica limitado a um padrão específico ao compilar para UWP.<br/><br/>Você pode usar o padrão interno de <a href="https://docs.microsoft.com/windows/uwp/data-binding/index">vinculação de dados</a> para garantir a separação limpa de preocupações de dados e preocupações de interface do usuário, e evitar ter de codificar manipuladores de eventos de interface do usuário e, depois, atualizar valores de propriedade.<br/><br/>Você pode estender a vinculação de dados para seguir o padrão <strong>Model-View-ViewModel (MVVM)</strong>, fazendo uso de bibliotecas de MVVM de terceiros, como <a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light Toolkit</a>, ou rodando a sua própria e mantendo a lógica fora de código para trás.<br/><br/><a href="https://docs.microsoft.com/previous-versions/msp-n-p/hh848246(v=pandp.10)">The MVVM Pattern</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Modelos de projeto do Template 10 do Visual Studio</a></td>
</tr>
</tbody>
</table>
