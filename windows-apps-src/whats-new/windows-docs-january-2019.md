---
title: Novidades nos Documentos do Windows em janeiro de 2019 – Desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 de janeiro de 2019
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, janeiro
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63800583"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Novidades nos Documentos de Desenvolvedor do Windows em janeiro de 2019

A Documentação do Desenvolvedor Windows recebe atualizações constantes com informações sobre novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As visões gerais de recursos, as diretrizes para desenvolvedores e os vídeos a seguir foram disponibilizados em janeiro.

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="windows-development-on-microsoft-learn"></a>Desenvolvimento do Windows no Microsoft Learn

O Microsoft Learn fornece novas oportunidades de treinamento e aprendizado prático para desenvolvedores Microsoft. Se estiver interessado em aprender a desenvolver aplicativos do Windows, confira [nosso novo roteiro de aprendizagem](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) para ver uma introdução abrangente à plataforma, às ferramentas e a como escrever seus primeiros aplicativos.

![Imagem do caminho de aprendizado de desenvolvimento do Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Os passes de renderização do Direct3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) podem melhorar o desempenho de seu renderizador se ele é baseado em TBDR (renderização diferida baseada em bloco), entre outras técnicas. A técnica ajuda seu renderizador a melhorar a eficiência de GPU, permitindo que seu aplicativo identifique melhor as dependências de dados e a ordenação de requisitos da renderização de recursos, reduzindo assim o tráfego de memória da memória sem chip e para ela.

### <a name="msix-modification-packages"></a>Pacotes de modificação MSIX

O Windows 10 versão 1809 aumentou o suporte para [pacotes de modificação MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Pacotes de modificação podem incluir plug-ins baseados em Registro e a personalização associada, além de permitirem que um aplicativo implantado por meio de MSIX use um Registro virtual e seja executado conforme o esperado.

![Criação de pacotes de modificação MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Open-source para WPF, Windows Forms e WinUI

As estruturas do WPF, Windows Forms e WinUI UX agora estão disponíveis para contribuições open-source no GitHub. Para obter mais informações e links, confira o [blog Criando aplicativos do Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Aplicativos Web progressivos para Xbox

Com [Aplicativos Web progressivos para Xbox](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), você pode estender um aplicativo Web e disponibilizá-lo como um aplicativo do Xbox One por meio da Microsoft Store enquanto continua usando as estruturas existentes, o CDN e o servidor back-end. De modo geral, você pode empacotar seu PWA para Xbox One da mesma maneira que faria para o Windows, no entanto, há várias diferenças importantes que este guia mostrará.

### <a name="windows-machine-learning"></a>Windows Machine Learning

Reestruturamos [a página de aterrissagem para APIs WinML](https://docs.microsoft.com/windows/ai/api-reference) e adicionamos nova documentação para APIs nativas e personalizadas do operador de WinML.

[Treinar um modelo com PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) fornece orientações sobre como treinar um modelo usando a estrutura PyTorch localmente ou na nuvem. Você pode baixar esse modelo como um arquivo ONNX e usá-lo em seus aplicativos WinML.

![Gráfico de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Diretrizes do desenvolvedor

### <a name="choose-your-platform"></a>Escolha sua plataforma

Interessado em criar um aplicativo da área de trabalho? Confira nossa página remodelada [Escolha sua plataforma](https://docs.microsoft.com/windows/desktop/choose-your-technology) para obter descrições detalhadas e comparações entre as plataformas Windows Forms, WPF e UWP e obter mais informações sobre a API Win32.

### <a name="faqs-on-win32-webview"></a>Perguntas frequentes sobre o WebView do Win32

Nossas [perguntas frequentes](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) fornecem respostas para perguntas comuns ao usar o Microsoft Edge WebView em aplicativos da área de trabalho, bem como links para exemplos e recursos adicionais.

### <a name="japanese-era-change"></a>Mudança da era japonesa

[Prepare seu aplicativo para a mudança da era japonesa](../design/globalizing/japanese-era-change.md) mostra como garantir que seu aplicativo do Windows esteja pronto para a mudança da era japonesa, prevista para entrar em vigor em 1º de maio de 2019. [Esta página também está disponível em japonês](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicativos Web progressivos

Aplicativos Web progressivos são sites da Web que funcionam como aplicativos nativos em diferentes navegadores e uma ampla variedade de dispositivos do Windows 10. [Assista ao vídeo](https://youtu.be/ugAewC3308Y) para saber mais e [confira os documentos](https://aka.ms/Windows-PWA) para começar a usar.

### <a name="vs-code-series"></a>Série do VS Code

Confira nossa [nova série de vídeos sobre o Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obter informações sobre o que é o VSCode, como usá-lo e como ele foi criado.

### <a name="one-dev-question"></a>Uma pergunta sobre desenvolvimento

Na série de vídeos Uma pergunta sobre desenvolvimento, os desenvolvedores experientes da Microsoft respondem a uma série de perguntas sobre desenvolvimento, cultura de equipe e história do Windows. Aqui estão as perguntas mais recentes que podemos ter respondido!

Raymond Chen:

* [Por que ter Arquivos de Programas e Arquivos de Programas (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [Por que COM é tão complicado?](https://youtu.be/-gkXAV-StVA )
* [Como foi sua primeira entrevista para a Microsoft?](https://youtu.be/qRb6otsHG5c)
