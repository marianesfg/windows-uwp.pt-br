---
author: QuinnRadich
title: What's New in Windows Docs em maio de 2018 - como desenvolver aplicativos UWP
description: Foram adicionados novos recursos, vídeos e orientação do desenvolvedor para a documentação do desenvolvedor do Windows 10 para maio de 2018 e a conferência Microsoft Build.
keywords: What's new, atualização, recursos, orientação do desenvolvedor, Windows 10, maio, compilação
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2842747"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>What's New in os documentos do desenvolvedor de Windows no de 2018 maio

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As seguintes visões gerais do recurso, orientação do desenvolvedor, vídeos e amostras tenham sido disponibilizadas no mês de maio para coincidir com a conferência de desenvolvedores do [Microsoft 2018 de compilação](https://www.microsoft.com/build) .

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="motion-in-fluent-design"></a>Movimento no Design Fluent

O usuário de movimento no sistema de Design Fluent está evoluindo, criadas sobre os fundamentos do tempo, facilitando, direcionalidade e gravity. Aplicar esses conceitos básicos ajudará orientam o usuário por seu aplicativo e os conecta com sua experiência digital por refletindo o mundo natural. Saiba mais em artigos:

* [Visão geral do movimento](../design/motion/index.md) foi atualizado para refletir esses conceitos básicos.
* [Movimentação em prática](../design/motion/motion-in-practice.md) fornece exemplos de como aplicar essas fundamentos dentro de seu aplicativo.
* [Direcionalidade e gravity](../design/motion/directionality-and-gravity.md) solidifica modelo mental do usuário do seu aplicativo.
* [Temporização e atenuação](../design/motion/timing-and-easing.md) adiciona realismo o movimento no seu aplicativo.

![Movimentação em ação](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Atualizações de Design Fluent

Pequenas alterações e atualizações de visuais foram feitas as seguintes páginas do Design Fluent:

* [Alinhamento, preenchimento, margens](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [Noções básicas de comandos](../design/basics/commanding-basics.md)
* [Fluent Design para aplicativos do Windows](../design/fluent-design-system/index.md)
* [Introdução ao design do aplicativo](../design/basics/design-and-ui-intro.md)
* [Noções básicas de navegação](../design/basics/navigation-basics.md)
* [Técnicas de design dinâmicas](../design/layout/responsive-design.md)
* [Tamanhos de tela e pontos de interrupção](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Visão geral de estilo](../design/style/index.md)
* [Estilo de escrita](../design/style/writing-style.md)

Além disso, podemos ter reescrito as seguintes páginas com totalmente nova informações sobre suas áreas de conteúdo:

* [Ícones](../design/style/icons.md) agora fornece recomendações de práticas para usando ícones e torná-los clicáveis.
* [Tipografia](../design/style/typography.md) consolida informações de artigos semelhantes, colocando tudo em um único lugar com atualizada a orientação e as ilustrações.

![Imagem de paleta de cores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Arquivos do instalador do aplicativo no Visual Studio

Arquivos do instalador do aplicativo agora podem ser criados com o Visual Studio de 2017, 15.7 de atualização. [Saiba como usar o Visual Studio para criar um arquivo do instalador do aplicativo](../packaging/create-appinstallerfile-vs.md) e habilitar atualizações automáticas para seus aplicativos. Se você estiver executando em problemas, consulte [solução de problemas de instalação com o arquivo do instalador do aplicativo](../packaging/troubleshoot-appinstaller-issues.md) para exibir os problemas comuns e soluções.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Controle de exibição da Web para aplicativos do Windows Forms e WPF de borda

Mostra o conteúdo da web em seu aplicativo de área de trabalho usando o controle de exibição da Web, anteriormente disponível somente para aplicativos UWP. Este controle usa o Edge Microsoft mecanismo para inserir um modo de exibição que processa amplamente formatado HTML de processamento conteúdo de um servidor web remoto, código gerado dinamicamente ou arquivos de conteúdo. Encontre o controle de exibição da Web na versão mais recente do [o Kit de ferramentas do Windows Community.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Procure por outros controles, como a exibição da Web em versões futuras do Windows comunidade Toolkit. Para obter mais informações, consulte [UWP Host controla em aplicativos WPF e Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Entrada do olhar e interações

[Acompanhe o foco, a atenção e a presença do usuário com base na localização e na movimentação de seus olhos.](../design/input/gaze-interactions.md) Essa nova maneira poderosa usar e interagir com os seus aplicativos UWP é especialmente útil como a tecnologia assistida. Entrada do olhar também oferece convincentes oportunidades para jogos (incluindo a aquisição de destino e acompanhamento) e de outros cenários interativos onde tradicionais dispositivos de entrada (teclado, do mouse, toque) não estão disponíveis.

### <a name="msix-packaging-format"></a>Formato de empacotamento MSIX

Anunciado na conferência Microsoft compilação 2018, MSIX é um novo formato de pacote containerization que se aplica a todos os aplicativos do Windows, incluindo Win32, Windows Forms, WPF e UWP. Esse novo formato herda excelentes recursos do UWP:

* Instalação robusta e a atualização. 
* Gerenciados modelo de segurança com um sistema de capacidade flexível.
* Suporte para o Microsoft Store, gerenciamento corporativo e muitos modelos de distribuição personalizados.

Ferramentas para criar esses pacotes estarão disponíveis em uma versão futura do Visual Studio e o SDK do Windows.

O formato de empacotamento de MSIX é um formato de código aberto que torna mais fácil para os nossos parceiros suportar o ecossistema MSIX com suas ferramentas e soluções. Para saber mais sobre o formato de empacotamento MSIX, consulte o [SDK de MSIX](https://github.com/Microsoft/msix-packaging). 

![Imagem de empacotamento MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Pacotes opcionais com código executável

Pacotes opcionais no seu aplicativo agora podem conter o código c# executável. [Saiba como usar o Visual Studio para configurar os pacotes de complemento opcional para oferecer suporte a seu pacote de aplicativo principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transições de página

[Transições de página](../design/motion/page-transitions.md) navegue usuários entre páginas em um aplicativo. Elas ajudam os usuários a entender onde eles estão na hierarquia de navegação e forneça feedback sobre a relação entre as páginas.

### <a name="project-rome"></a>Project Rome

A equipe de projeto Roma tem revisadas suas iOS e Android SDKs, adicionando novos recursos, como as atividades do usuário e refatoração muito do seu código para fornecer uma experiência de programação consistente entre os SDKs diferentes. [Todos os nova referência de API e instruções docs](https://docs.microsoft.com/windows/project-rome/) irão ao vivo durante a conferência de desenvolvedores 2018 de compilação.

### <a name="sets"></a>Conjuntos

O recurso conjuntos está disponível no Insider Windows compilações de visualização. Ao usar o recurso conjuntos, você app é desenhada em uma janela que pode ser compartilhada com outros aplicativos, com cada aplicativo ter sua própria tabulação na barra de título. [Projetando para conjuntos de](../design/shell/design-for-sets.md) tem orientação sobre como otimizar o seu aplicativo para oferecer a melhor experiência na interface de usuário define.

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="get-started"></a>Introdução

Podemos ter revitalized nosso Get started conteúdo com novo acompanhamentos de aprendizado. Esses novos tópicos objetivam é fornecer aos desenvolvedores de Windows 10 novos informações sobre algumas tarefas comuns que desejam realizar. Eles não estiver tutoriais e não for fornecido um passo a passo de mão, mas em vez disso, destaque onde existe a documentação existente e como usá-lo. Confira o reformulada [Iniciar a codificação](../get-started/create-uwp-apps.md) de página ou explore cada trilha de aprendizado individual:

* [Construir um formulário](../get-started/construct-form-learning-track.md)
* [Exibir clientes em uma lista](../get-started/display-customers-in-list-learning-track.md)
* [Salvar e carregar configurações](../get-started/settings-learning-track.md)
* [Trabalhar com arquivos](../get-started/fileio-learning-track.md)

![Obter imagem de Introdução](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios

O [relatório de desempenho de anúncio](../publish/advertising-performance-report.md) no painel Dev Center agora oferece métricas de visualização. Adicionamos também o artigo de [otimizar a visualização da suas unidades da ad](../monetize/optimize-ad-unit-viewability.md) para oferecer recomendações para otimizar a visualização da suas ads.

### <a name="targeted-push-notifications"></a>Notificações por push direcionado

Agora, a página [notificações](../publish/send-push-notifications-to-your-apps-customers.md) no painel Dev Center fornece dados de análise adicionais para todas as notificações no gráfico e world as exibições do mapa.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C + + / WinRT é um novo modo de criação e consumindo Windows Runtime APIs. Ele tem implementado único nos arquivos de cabeçalho e projetado para fornecer acesso de primeira classe para recursos modernos app. [Assista ao vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) para saber como ele funciona, em seguida, [Leia os documentos do desenvolvedor](../cpp-and-winrt-apis/index.md) para obter mais informações.

### <a name="multi-instance-uwp-apps"></a>Aplicativos UWP de várias instâncias

Windows agora permite que você execute várias instâncias do seu aplicativo UWP, com cada um em seu próprio processo separado. [Assista ao vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para saber como criar um novo aplicativo que ofereça suporte a esse recurso, em seguida, [Leia os documentos do desenvolvedor](../launch-resume/multi-instance-uwp.md) para obter mais orientações sobre como e por que usar esse recurso.

## <a name="samples"></a>Exemplos

### <a name="customer-database-tutorial"></a>Tutorial do banco de dados do cliente

Este tutorial cria um aplicativo UWP básico de gerenciamento de uma lista de clientes e introduz os conceitos e práticas recomendadas útil no desenvolvimento empresarial. Ele o orienta através de implementação de elementos de interface do usuário e adicionando operações em relação a um banco de dados local SQLite e fornece orientação afastada para conexão com um banco de dados remoto do REST se quiser desce. [Confira o tutorial aqui](../enterprise/customer-database-tutorial.md)