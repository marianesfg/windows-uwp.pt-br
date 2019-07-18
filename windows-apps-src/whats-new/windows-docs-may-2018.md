---
title: Novidades nos documentos do Windows em maio de 2018 – Desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor Windows 10 referente a maio de 2018 e à conferência Microsoft Build.
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, maio, build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9adf5a80595e00a30098044536d1ecfe4fd62279
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820492"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Novidades nos documentos de Desenvolvedor Windows em maior de 2018

A Documentação do Desenvolvedor Windows recebe atualizações constantes com informações sobre novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As visões gerais de recursos, as diretrizes para desenvolvedores, os vídeos e as amostras a seguir foram disponibilizados em maio para coincidir com a conferência de desenvolvedores [Microsoft Build 2018](https://www.microsoft.com/build/).

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="motion-in-fluent-design"></a>Movimento no Design Fluente

O uso de movimento no Sistema do Design Fluente está evoluindo. Ele foi criado com conceitos básicos de tempo, redução, direcionalidade e gravidade. Aplicar esses conceitos básicos ajudará a orientar o usuário por meio de seu aplicativo e os conectará com sua experiência digital ao refletir o mundo natural. Saiba mais nestes artigos:

* [A visão geral de movimento](../design/motion/index.md) foi atualizada para refletir esses conceitos básicos.
* O artigo [Movimento na prática](../design/motion/motion-in-practice.md) fornece exemplos de como aplicar esses conceitos básicos ao seu aplicativo.
* [Direcionalidade e gravidade](../design/motion/directionality-and-gravity.md) solidifica o modelo mental do usuário do seu aplicativo.
* [Tempo e facilitação](../design/motion/timing-and-easing.md) adiciona realismo ao movimento em seu aplicativo.

![Movimento em ação](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Atualizações do Design Fluente

Atualizações de visual e pequenas alterações foram feitas nas seguintes páginas do Design Fluente:

* [Alinhamento, preenchimento, margens](../design/layout/alignment-margin-padding.md)
* [Cor](../design/style/color.md)
* [Noções básicas de comandos](../design/basics/commanding-basics.md)
* [Design Fluente para aplicativos do Windows](../design/fluent-design-system/index.md)
* [Introdução ao design de aplicativos](../design/basics/design-and-ui-intro.md)
* [Noções básicas de navegação](../design/basics/navigation-basics.md)
* [Técnicas de design dinâmico](../design/layout/responsive-design.md)
* [Tamanhos de tela e pontos de interrupção](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Visão geral de estilo](../design/style/index.md)
* [Estilo de escrita](../design/style/writing-style.md)

Além disso, reescrevemos as páginas a seguir com informações novas sobre suas áreas de conteúdo:

* A página [Ícones](../design/style/icons.md) agora fornece recomendações de práticas para usar ícones e torná-los clicáveis.
* [Tipografia](../design/style/typography.md) consolida as informações de artigos semelhantes, colocando tudo em um único lugar com diretrizes atualizadas e ilustrações.

![Imagem da paleta de cores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Arquivos do Instalador de Aplicativo no Visual Studio

Arquivos do Instalador de Aplicativo agora podem ser criados com o Visual Studio 2017, atualização 15.7 e com versões posteriores. [Saiba como usar o Visual Studio para criar um arquivo do Instalador de Aplicativo](../packaging/create-appinstallerfile-vs.md) e habilitar as atualizações automáticas em seus aplicativos. Se estiver com problemas, consulte [solução de problemas de instalação com o arquivo do Instalador de Aplicativo](../packaging/troubleshoot-appinstaller-issues.md) para ver os problemas e soluções comuns.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Controle Edge WebView para aplicativos Windows Forms e WPF

Mostre conteúdo da Web em seu aplicativo da área de trabalho usando o controle WebView, anteriormente disponível apenas para aplicativos UWP. Este controle usa o mecanismo de renderização do Microsoft Edge para inserir uma exibição que renderiza conteúdo HTML ricamente formatado de um servidor Web remoto, código gerado dinamicamente ou arquivos de conteúdo. Você pode encontrar o WebView na versão mais recente do [Kit de Ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).

Procure por outros controles como o WebView em versões futuras do Kit de Ferramentas da Comunidade do Windows. Para obter mais informações, veja [Hospedar controles UWP em aplicativos WPF e Windows Forms](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

### <a name="gaze-input-and-interactions"></a>Interações e entrada por foco

[Acompanhe o foco, a atenção e a presença do usuário com base na localização e na movimentação de seus olhos.](../design/input/gaze-interactions.md) Essa nova maneira avançada de usar e interagir com seus aplicativos UWP é especialmente útil como uma tecnologia adaptativa. A entrada por foco também fornece oportunidades interessantes para jogos (incluindo aquisição de alvo e acompanhamento) e outros cenários interativos em que dispositivos de entrada tradicionais (teclado, mouse, toque) não estão disponíveis.

### <a name="msix-packaging-format"></a>Formato de empacotamento MSIX

Anunciado na conferência Microsoft Build 2018, o MSIX é um novo formato de pacote de contêineres que se aplica a todos os aplicativos Windows, incluindo Win32, Windows Forms, WPF e UWP. Esse novo formato herda excelentes recursos da UWP:

* Instalação e atualização robustas. 
* Modelo de segurança gerenciado com sistema de capacidade flexível.
* Suporte para Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizada.

Ferramentas para criar esses pacotes estarão disponíveis em uma versão futura do SDK do Visual Studio e do Windows.

O formato de empacotamento MSIX é um formato de software livre que torna mais fácil para nossos parceiros dar suporte ao ecossistema MSIX com suas ferramentas e soluções. Para saber mais sobre o formato de empacotamento MSIX, consulte [SDK do MSIX](https://github.com/Microsoft/msix-packaging). 

![Imagem do empacotamento MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Pacotes opcionais com código executável

Pacotes opcionais em seu aplicativo agora podem conter código executável C#. [Saiba como usar o Visual Studio para configurar pacotes de complemento opcionais para dar suporte a seu pacote do aplicativo principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transições de página

As [Transições de página](../design/motion/page-transitions.md) possibilitam que os usuários naveguem entre páginas em um aplicativo. Elas ajudam os usuários a entender onde eles estão na hierarquia de navegação e fornecem comentários sobre a relação entre as páginas.

### <a name="project-rome"></a>Project Rome

A equipe do Project Rome aperfeiçoou seus SDKs para iOS e Android, adicionando novos recursos como Atividades do Usuário e refatorando grande parte de seu código para fornecer uma experiência de programação consistente entre os diferentes SDKs. [A referência de API e documentos de instruções totalmente novos](https://docs.microsoft.com/windows/project-rome/) entrarão em vigor durante a conferência de desenvolvedores Build 2018.

### <a name="sets"></a>Conjuntos

O recurso de Conjuntos está disponível nas versões prévias do Windows Insider. Ao usar o recurso de Conjuntos, o aplicativo é desenhado em uma janela que pode ser compartilhada com outros aplicativos, com cada aplicativo tendo sua própria guia na barra de título. 

## <a name="developer-guidance"></a>Diretrizes do desenvolvedor

### <a name="get-started"></a>Introdução

Renovamos nossos conteúdos de Introdução com novos roteiros de aprendizagem. Esses novos tópicos têm como objetivo fornecer a novos desenvolvedores do Windows 10 informações sobre algumas tarefas comuns que eles podem querer realizar. Eles não são tutoriais e não fornecerem um passo a passo, mas destacam onde está a documentação existente e como usá-la. Confira a página [Começar a codificar](../get-started/create-uwp-apps.md) remodelada ou explore cada roteiro de aprendizado individual:

* [Construir um formulário](../get-started/construct-form-learning-track.md)
* [Exibir clientes em uma lista](../get-started/display-customers-in-list-learning-track.md)
* [Salvar e carregar configurações](../get-started/settings-learning-track.md)
* [Trabalhar com arquivos](../get-started/fileio-learning-track.md)

![Imagem de Introdução](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios

O [Relatório de desempenho de publicidade](../publish/advertising-performance-report.md) no Partner Center agora oferece métricas de visualização. Também adicionamos o artigo [Otimizar a visualização das unidades de anúncio](../monetize/optimize-ad-unit-viewability.md) para fornecer recomendações para otimizar a visualização de anúncios.

### <a name="targeted-push-notifications"></a>Notificações por push direcionadas

A página [Notificações](../publish/send-push-notifications-to-your-apps-customers.md) no Partner Center agora fornece dados de análise adicionais para todas as notificações nos modos de exibição de gráfico e mapa mundi.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

O C++/WinRT é uma nova maneira de criar e consumir APIs do Windows Runtime. Ele é implementado exclusivamente em arquivos de cabeçalho e foi projetado para fornecer acesso de primeira classe aos recursos de aplicativo modernos. [Assista ao vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) para saber como ele funciona, em seguida, [leia os documentos do desenvolvedor](../cpp-and-winrt-apis/index.md) para obter mais informações.

### <a name="multi-instance-uwp-apps"></a>Aplicativos UWP de várias instâncias

O Windows agora permite que você execute várias instâncias do seu aplicativo UWP, com cada um em seu próprio processo separado. [Assista ao vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para aprender a criar um aplicativo compatível com esse recurso, em seguida, [leia os documentos do desenvolvedor](../launch-resume/multi-instance-uwp.md) para obter mais informações sobre como e por que usar esse recurso.

## <a name="samples"></a>Exemplos

### <a name="customer-database-tutorial"></a>Tutorial de banco de dados de clientes

Este tutorial cria um aplicativo UWP básico para gerenciar uma lista de clientes e apresenta conceitos e práticas úteis no desenvolvimento corporativo. Ele orienta você durante a implementação de elementos de interface do usuário e adição de operações em relação a um banco de dados SQLite local, bem como fornece orientação flexível sobre conexão a um banco de dados REST remoto se você quer ir além. [Confira o tutorial aqui](../enterprise/customer-database-tutorial.md)