---
author: QuinnRadich
title: Novidades do Windows Docs em maio de 2018 - desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 de maio de 2018 e conferência Microsoft Build.
keywords: Novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, maio, compilação
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4947571"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Novidades dos documentos de desenvolvedor do Windows em maio de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As seguintes visões gerais de recurso, diretrizes para desenvolvedores, vídeos e amostras foram disponibilizadas no mês de maio para coincidir com a conferência de desenvolvedores [Microsoft Build 2018](https://www.microsoft.com/build) .

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="motion-in-fluent-design"></a>Movimento em Design fluente

O usuário de movimento no sistema de Design Fluent está evoluindo, construído sobre os conceitos básicos do tempo, suavização, direção e gravidade. Aplicar esses conceitos básicos ajuda a orientar o usuário por meio do seu aplicativo e os conecta com sua experiência digital refletindo mundo natural. Saiba mais neste artigo:

* [Visão geral do movimento](../design/motion/index.md) foi atualizada para refletir esses conceitos básicos.
* [Movimento na prática](../design/motion/motion-in-practice.md) fornece exemplos de como aplicar esses conceitos básicos de dentro de seu aplicativo.
* [Direção e gravidade](../design/motion/directionality-and-gravity.md) solidifica o modelo mental do usuário do seu aplicativo.
* [Tempo e suavização](../design/motion/timing-and-easing.md) adiciona realismo para o movimento em seu aplicativo.

![Movimento em ação](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Atualizações de Design fluente

As atualizações visuais e pequenas alterações foram feitas as seguintes páginas de Design fluente:

* [Alinhamento, preenchimento, margens](../design/layout/alignment-margin-padding.md)
* [Cor](../design/style/color.md)
* [Noções básicas de comandos](../design/basics/commanding-basics.md)
* [Design fluente para aplicativos do Windows](../design/fluent-design-system/index.md)
* [Introdução ao design de aplicativo](../design/basics/design-and-ui-intro.md)
* [Noções básicas de navegação](../design/basics/navigation-basics.md)
* [Técnicas de design dinâmicas](../design/layout/responsive-design.md)
* [Tamanhos de tela e pontos de interrupção](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Visão geral de estilo](../design/style/index.md)
* [Estilo de escrita](../design/style/writing-style.md)

Além disso, estamos tiver reescrito páginas com todas as novas informações sobre suas áreas de conteúdo a seguir:

* [Ícones](../design/style/icons.md) agora fornece recomendações práticas para utilizar ícones e torná-los clicáveis.
* [Tipografia](../design/style/typography.md) consolida informações de artigos semelhantes, colocar tudo em um único lugar com diretrizes atualizadas e ilustrações.

![Imagem de paleta de cores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Arquivos do instalador de aplicativo no Visual Studio

Arquivos do instalador de aplicativo agora podem ser criados com o Visual Studio 2017, atualização 15.7. [Saiba como usar o Visual Studio para criar um arquivo do instalador de aplicativo](../packaging/create-appinstallerfile-vs.md) e ativar atualizações automáticas para seus aplicativos. Se você estiver encontrando problemas, consulte a [solução de problemas de instalação com o arquivo do instalador de aplicativo](../packaging/troubleshoot-appinstaller-issues.md) para exibir problemas e soluções comuns.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Controle WebView para aplicativos Windows Forms e WPF de borda

Mostre o conteúdo da web em seu aplicativo da área de trabalho usando o controle WebView, anteriormente disponível somente para aplicativos UWP. Esse controle usa o Microsoft Edge conteúdo de um servidor web remoto, código gerado dinamicamente ou arquivos de conteúdo do mecanismo para incorporar um modo de exibição que renderiza sofisticadamente formatado HTML de processamento. Localize o controle WebView na versão mais recente do [o Kit de ferramentas do Windows da comunidade.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Procure por outros controles como WebView em versões futuras do Kit de ferramentas da comunidade Windows. Para obter mais informações, consulte [UWP Host controles em aplicativos WPF e Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Entrada e interações de foco

[Acompanhe o foco, a atenção e a presença do usuário com base na localização e na movimentação de seus olhos.](../design/input/gaze-interactions.md) Essa nova maneira eficiente de usar e interagir com seus aplicativos UWP é útil como uma tecnologia assistencial. Entrada com foco também oferece oportunidades atraentes para jogos (incluindo a aquisição do alvo e acompanhamento) e outros cenários interativos onde os dispositivos de entrada tradicionais (teclado, mouse, toque) não estão disponíveis.

### <a name="msix-packaging-format"></a>Formato de empacotamento MSIX

MSIX anunciado na conferência Microsoft Build 2018, é um novo formato de pacote enormemente que se aplica a todos os aplicativos do Windows, incluindo Win32, Windows Forms, WPF e UWP. Esse novo formato herda excelentes recursos do UWP:

* Instalação robusta e a atualização. 
* Gerenciado modelo de segurança com um sistema de funcionalidade flexível.
* Suporte para a Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizado.

Ferramentas para criar esses pacotes estarão disponíveis em uma versão futura do Visual Studio e SDK do Windows.

O formato de empacotamento MSIX é um formato de código-fonte aberto que torna mais fácil para os nossos parceiros dar suporte ao ecossistema MSIX com suas soluções e ferramentas. Para saber mais sobre o formato de empacotamento MSIX, consulte [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Imagem de empacotamento MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Pacotes opcionais com código executável

Pacotes opcionais em seu aplicativo agora podem conter código executável em c#. [Saiba como usar o Visual Studio para configurar pacotes de complemento opcional para dar suporte ao pacote do aplicativo principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transições de página

[Transições de página](../design/motion/page-transitions.md) navegam entre as páginas em um aplicativo. Elas ajudam os usuários a compreender onde eles estão na hierarquia de navegação e fornecer comentários sobre a relação entre as páginas.

### <a name="project-rome"></a>Project Rome

A equipe de projeto Roma reformulado seus iOS e Android SDKs, adicionando novos recursos como atividades do usuário e refatoração grande parte do seu código para fornecer uma experiência de programação consistente entre os SDKs diferentes. [Todos os novos referência de API e documentos instruções](https://docs.microsoft.com/windows/project-rome/) passará ao vivo durante a conferência de desenvolvedores de Build 2018.

### <a name="sets"></a>Conjuntos

O recurso de conjuntos está disponível no Windows compilações do Insider preview. Ao usar o recurso de conjuntos, seu aplicativo é desenhado em uma janela que pode ser compartilhada com outros aplicativos, com cada aplicativo ter sua própria guia na barra de título. [Projetando para conjuntos](../design/shell/design-for-sets.md) tem orientações sobre como otimizar seu aplicativo para fornecer a melhor experiência na interface do usuário define.

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="get-started"></a>Introdução

Nós já revitalized nosso Get iniciado conteúdo com novos acompanhamentos de aprendizagem. Esses novos tópicos objetivam é fornecer novos desenvolvedores do Windows 10 com informações sobre algumas tarefas comuns que eles podem querer fazer. Eles não estiver tutoriais e não fornecerem um passo a passo portátil, mas em vez disso, destacar onde existe documentação existente e como usá-lo. Confira o remodelado [começar a codificar](../get-started/create-uwp-apps.md) página ou explore a cada faixa de aprendizado de individuais:

* [Construir um formulário](../get-started/construct-form-learning-track.md)
* [Exibir clientes em uma lista](../get-started/display-customers-in-list-learning-track.md)
* [Salvar e carregar configurações](../get-started/settings-learning-track.md)
* [Trabalhar com arquivos](../get-started/fileio-learning-track.md)

![Obter imagem iniciada](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios

O [relatório de desempenho de anúncios](../publish/advertising-performance-report.md) no painel do Centro de desenvolvimento agora fornece métricas de visualização. Também adicionamos o artigo de [otimizar a visualização de suas unidades de anúncio](../monetize/optimize-ad-unit-viewability.md) para fornecer recomendações para otimizar a visualização de seus anúncios.

### <a name="targeted-push-notifications"></a>Notificações por push direcionadas

A página de [notificações](../publish/send-push-notifications-to-your-apps-customers.md) no painel do Centro de desenvolvimento agora fornece dados de análise adicionais para todas as suas notificações nos modos de exibição de mapa de gráfico e o mundo.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C++ c++ /WinRT é uma nova maneira de criação e consumo de APIs do Windows Runtime. Ele tem implementado única em arquivos de cabeçalho e projetada para fornecer acesso de primeira classe aos recursos de aplicativos modernos. [Assista ao vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) para saber como ele funciona, em seguida, [Leia os documentos de desenvolvedor](../cpp-and-winrt-apis/index.md) para obter mais informações.

### <a name="multi-instance-uwp-apps"></a>Aplicativos UWP de várias instâncias

Windows agora permite que você execute várias instâncias do aplicativo UWP, com cada em seu próprio processo separado. [Assista ao vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para saber como criar um novo aplicativo que dá suporte a esse recurso, em seguida, [Leia os documentos de desenvolvedor](../launch-resume/multi-instance-uwp.md) para obter mais orientações sobre como e por que usar esse recurso.

## <a name="samples"></a>Exemplos

### <a name="customer-database-tutorial"></a>Tutorial de banco de dados do cliente

Este tutorial cria um aplicativo UWP básico para gerenciar uma lista de clientes e apresenta conceitos e práticas útil no desenvolvimento empresarial. Ele orienta você em meio à implementação de elementos de interface do usuário e adicionando operações em relação a um banco de dados local do SQLite e fornece diretrizes flexível para se conectar a um banco de dados remoto do restante se você deseja ir além. [Confira aqui o tutorial](../enterprise/customer-database-tutorial.md)