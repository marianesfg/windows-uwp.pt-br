---
title: O que há de novo nos documentos do Windows em maio de 2018 – desenvolva aplicativos UWP
description: Diretrizes para desenvolvedores, vídeos e novos recursos foram adicionados para a documentação do desenvolvedor do Windows 10 de maio de 2018 e a conferência Microsoft Build.
keywords: o que há de novo, atualizar, recursos, diretrizes para desenvolvedores, o Windows 10, maio, build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598781"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>O que há de novo nos documentos de desenvolvedor do Windows em maio de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As seguintes visões gerais de recursos, diretrizes para desenvolvedores, vídeos e exemplos estavam disponíveis no mês de maio para coincidir com o [Microsoft Build 2018](https://www.microsoft.com/build) conferência para desenvolvedores.

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="motion-in-fluent-design"></a>Movimento no Design Fluent

O usuário do movimento no sistema de Design Fluent está evoluindo, criado sobre os conceitos básicos de medição de tempo, facilitando a, direcionalidade e gravidade. Aplicar esses conceitos básicos ajudarão a orientar o usuário por meio de seu aplicativo e os conecta com sua experiência digital ao refletir o mundo natural. Saiba mais neste artigo:

* [Visão geral do movimento](../design/motion/index.md) foi atualizado para refletir esses conceitos básicos.
* [Movimento na prática](../design/motion/motion-in-practice.md) fornece exemplos de como aplicar esses conceitos básicos dentro de seu aplicativo.
* [Direcionalidade e gravidade](../design/motion/directionality-and-gravity.md) solidifica modelo mental do usuário do seu aplicativo.
* [Atingir o tempo e atenuação](../design/motion/timing-and-easing.md) adiciona o realismo e a animação em seu aplicativo.

![Movimento em ação](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Atualizações de Design Fluent

Atualizações do Visual e pequenas alterações foram feitas as seguintes páginas de Design Fluent:

* [Alinhamento de preenchimento, margens](../design/layout/alignment-margin-padding.md)
* [Cor](../design/style/color.md)
* [Noções básicas de comandos](../design/basics/commanding-basics.md)
* [Design Fluent para aplicativos do Windows](../design/fluent-design-system/index.md)
* [Introdução ao design de aplicativo](../design/basics/design-and-ui-intro.md)
* [Noções básicas de navegação](../design/basics/navigation-basics.md)
* [Técnicas de design dinâmico](../design/layout/responsive-design.md)
* [Tamanhos de tela e pontos de interrupção](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Visão geral de estilo](../design/style/index.md)
* [Estilo de escrita](../design/style/writing-style.md)

Além disso, podemos ter reescrito as páginas a seguir com todas as novas informações sobre suas áreas de conteúdo:

* [Ícones](../design/style/icons.md) agora fornece recomendações de práticas para usando ícones e torná-los clicável.
* [Tipografia](../design/style/typography.md) consolida as informações de artigos semelhantes, colocando tudo em um único lugar com as diretrizes atualizadas e ilustrações.

![Imagem da paleta de cores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Arquivos do instalador de aplicativo no Visual Studio

Arquivos do instalador de aplicativos agora podem ser criados com o Visual Studio 2017 15.7 de atualização. [Saiba como usar o Visual Studio para criar um arquivo do instalador do aplicativo](../packaging/create-appinstallerfile-vs.md) e habilitar as atualizações automáticas em seus aplicativos. Se você estiver com problemas, consulte [solução de problemas de instalação com o arquivo do instalador do aplicativo](../packaging/troubleshoot-appinstaller-issues.md) para exibir os problemas e soluções comuns.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Borda do controle WebView para aplicativos Windows Forms e WPF

Mostra conteúdo da web em seu aplicativo da área de trabalho, usando o controle WebView, anteriormente disponível apenas para aplicativos UWP. Este controle usa o mecanismo para inserir uma exibição que renderiza ricamente formatado HTML de processamento conteúdo de um servidor web remoto, o código gerado dinamicamente ou arquivos de conteúdo do Microsoft Edge. Localizar o controle WebView na versão mais recente do [Kit de ferramentas de comunidade do Windows.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Procure outros controles, como o WebView em versões futuras do Kit de ferramentas de comunidade do Windows. Para obter mais informações, consulte [controles UWP do Host em aplicativos WPF e Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Olhar entrada e as interações

[Controle olhar um usuário, atenção e presença com base na localização e movimentação de seus olhos.](../design/input/gaze-interactions.md) Essa maneira eficiente de usar e interagir com seus aplicativos UWP é especialmente útil como uma tecnologia assistencial. Olhar entrada também fornece oportunidades interessantes para jogos (incluindo aquisição de destino e de controle) e outros cenários interativos em que dispositivos de entrada tradicionais (teclado, mouse, toque) não estão disponíveis.

### <a name="msix-packaging-format"></a>Formato de empacotamento MSIX

Anunciado na conferência Microsoft Build 2018, MSIX é um novo formato de pacote do uso de contêineres que se aplica a todos os aplicativos do Windows, incluindo Win32, Windows Forms, WPF e UWP. Esse novo formato herda os excelentes recursos de UWP:

* Robusto de instalação e atualização. 
* Gerenciado modelo de segurança com um sistema de capacidade flexível.
* Suporte para a Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizada.

Ferramentas para criar esses pacotes estarão disponíveis em uma versão futura do Visual Studio e SDK do Windows.

O formato de empacotamento MSIX é um formato de código-fonte aberto que torna mais fácil para nossos parceiros dar suporte o ecossistema MSIX com suas ferramentas e soluções. Para saber mais sobre o formato de empacotamento MSIX, consulte [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Imagem de empacotamento MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Pacotes opcionais com código executável

Pacotes opcionais em seu aplicativo agora podem conter o executável C# código. [Saiba como usar o Visual Studio para configurar pacotes de complemento opcional para dar suporte a seu pacote do aplicativo principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transições de página

[Transições de página](../design/motion/page-transitions.md) navegue usuários entre páginas em um aplicativo. Eles ajudam os usuários a entender onde eles estão na hierarquia de navegação e fornecer comentários sobre a relação entre as páginas.

### <a name="project-rome"></a>Project Rome

A equipe de projeto Roma aperfeiçoou seus iOS e Android SDKs, adição de novos recursos, como as atividades do usuário e a refatoração grande parte do seu código para fornecer uma experiência de programação consistente entre os diferentes SDKs. [Todos os novos referência e instruções sobre os documentos da API](https://docs.microsoft.com/windows/project-rome/) entrarão em vigor durante a conferência para desenvolvedores Build 2018.

### <a name="sets"></a>Conjuntos de

O recurso de conjuntos está disponível no Insider Windows compilações do preview. Ao usar o recurso de conjuntos, o aplicativo é desenhado em uma janela que pode ser compartilhada com outros aplicativos, com cada aplicativo ter sua própria guia na barra de título. 

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="get-started"></a>Introdução

Nós já revitalized nossas conteúdos introdutórios com roteiros de aprendizagem. Ter como objetivo fornecer novos desenvolvedores do Windows 10 com informações sobre algumas tarefas comuns que talvez queiram realizar esses novos tópicos. Eles não estiver tutoriais e não fornecerem um passo a passo de bolso, mas em vez disso, destacar onde existe documentação existente e como usá-lo. Fazer check-out a remodelada [começar a codificar](../get-started/create-uwp-apps.md) página ou explore cada faixa de aprendizado individual:

* [Construir um formulário](../get-started/construct-form-learning-track.md)
* [Clientes de exibição em uma lista](../get-started/display-customers-in-list-learning-track.md)
* [Salvar e carregar as configurações](../get-started/settings-learning-track.md)
* [Trabalhar com arquivos](../get-started/fileio-learning-track.md)

![Obter imagem iniciada](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios

O [relatório de desempenho de publicidade](../publish/advertising-performance-report.md) no Partner Center agora oferece métricas de visualização. Também adicionamos a [otimizar a visualização das suas unidades ad](../monetize/optimize-ad-unit-viewability.md) artigo para fornecer recomendações para otimizar a visualização de anúncios.

### <a name="targeted-push-notifications"></a>Notificações por push direcionadas

O [notificações](../publish/send-push-notifications-to-your-apps-customers.md) página no Partner Center agora fornece dados de análise adicionais para todas as notificações em modos de exibição de mapa de gráfico e o mundo.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C + + c++ /CLI WinRT é uma nova maneira de criar e consumir APIs do Windows Runtime. Ele tem implementado sole nos arquivos de cabeçalho e projetado para fornecer acesso aos recursos de aplicativos modernos de primeira classe. [Assista ao vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) para saber como ele funciona, em seguida, [leia os documentos de desenvolvedor](../cpp-and-winrt-apis/index.md) para obter mais informações.

### <a name="multi-instance-uwp-apps"></a>Aplicativos UWP de várias instâncias

Windows agora permite que você execute várias instâncias do seu aplicativo da UWP, com cada um em seu próprio processo separado. [Assista ao vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para aprender a criar um novo aplicativo que dá suporte a esse recurso, em seguida, [leia os documentos de desenvolvedor](../launch-resume/multi-instance-uwp.md) para obter mais informações sobre como e por que usar esse recurso.

## <a name="samples"></a>Exemplos

### <a name="customer-database-tutorial"></a>Tutorial de banco de dados do cliente

Este tutorial cria um aplicativo UWP básico para gerenciar uma lista de clientes e apresenta os conceitos e práticas recomendadas úteis no desenvolvimento empresarial. Ele orienta você durante a implementação de elementos de interface do usuário e adicionar operações em relação a um banco de dados SQLite local e fornece orientação flexível para se conectar a um banco de dados remoto do REST se você quiser ir além. [Confira o tutorial aqui](../enterprise/customer-database-tutorial.md)