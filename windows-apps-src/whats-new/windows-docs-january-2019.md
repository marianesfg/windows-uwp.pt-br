---
title: Novidades dos documentos do Windows em janeiro de 2019 - desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 de janeiro de 2019
keywords: Novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, janeiro
ms.date: 1/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4f224663506cbb60f6c1476caccb5ecffefeaf7b
ms.sourcegitcommit: cfdc854fede8e586202523cdb59d3d0a2f5b4b36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2019
ms.locfileid: "9014386"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Novidades dos documentos de desenvolvedor do Windows em janeiro de 2019

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Visões gerais de recurso, diretrizes para desenvolvedores e vídeos a seguir foram disponibilizados no mês de janeiro.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="windows-development-on-microsoft-learn"></a>Desenvolvimento do Windows no Microsoft Learn

Microsoft Learn fornece novos aprendizado prático e oportunidades de treinamento para os desenvolvedores da Microsoft. Se você tiver interesse em aprender como desenvolver aplicativos do Windows, confira [nosso novo caminho de aprendizado](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) para obter uma introdução completa para a plataforma, as ferramentas e como escrever seus primeiros alguns aplicativos.

![Imagem do plano de aprendizado de desenvolvimento do Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Passagens de renderização do Direct3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) pode melhorar o desempenho do seu renderizador se ele for baseado em baseado em bloco renderização adiada (TBDR), entre outras técnicas. A técnica de ajuda para melhorar a eficiência GPU Habilitando seu aplicativo para identificar melhor renderização ordenação requisitos e dados dependências de recurso, e, portanto, reduzindo o tráfego de memória de/para memória-chip seu renderizador.

### <a name="msix-modification-packages"></a>Pacotes de modificação de MSIX

Windows 10 versão 1809 aprimorada suporte para [pacotes de modificação de MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Pacotes de modificação podem incluir plug-ins baseadas no registro e personalização associada e permitem que um aplicativo implantado por meio de MSIX para usar um registro virtual e executar conforme o esperado.

![Criação do pacote de modificação de MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Código-fonte aberto de WinUI, Windows Forms e WPF

As estruturas WinUI UX, Windows Forms e WPF agora estão disponíveis para contribuições de código-fonte aberto no GitHub. Para obter mais informações e links, consulte a [criação de blog de aplicativos do Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Aplicativos Web progressivos para Xbox

Com [Aplicativos Web progressivos para Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), você pode estender um aplicativo web e disponibilizá-lo como um aplicativo Xbox One por meio da Microsoft Store enquanto ainda continua a usar seu estruturas existentes, CDN e servidor back-end. Para a maior parte, é possível empacotar seu PWA para Xbox One da mesma maneira que você faria para Windows, no entanto, há várias diferenças importantes que este guia o orientará durante.

### <a name="windows-machine-learning"></a>Aprendizado de máquina do Windows

Nós reestruturar [a página inicial para APIs WinML](https://docs.microsoft.com/windows/ai/api-reference)e adicionado a nova documentação para operador personalizado WinML e APIs nativas.

[Treinar um modelo com PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) fornece diretrizes sobre como treinar um modelo usando a estrutura de PyTorch localmente ou na nuvem. Em seguida, você pode baixar esse modelo como um arquivo ONNX e usá-lo em seus aplicativos WinML.

![Gráfico de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="choose-your-platform"></a>Escolha a sua plataforma

Interessado em criar um novo aplicativo da área de trabalho? Confira nossa página [Escolher sua plataforma](https://docs.microsoft.com/windows/desktop/choose-your-technology) remodelada para obter descrições detalhadas e as comparações entre as plataformas UWP, WPF e Windows Forms e obter mais informações sobre a API Win32.

### <a name="faqs-on-win32-webview"></a>Perguntas frequentes sobre Win32 WebView

[Perguntas frequentes sobre](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) nossos fornece respostas para perguntas comuns ao usar o WebView de borda da Microsoft em aplicativos da área de trabalho, bem como links para exemplos e recursos adicionais.

### <a name="japanese-era-change"></a>Alteração de eras japonês

[Preparar seu aplicativo para o japonês era alterar](../design/globalizing/japanese-era-change.md) mostra como garantir que seu aplicativo está pronto para o japonês era alterar conjunto tirar do Windows coloque em 1º de maio de 2019. [Esta página é também está disponível em japonês](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicativos Web progressivos

Aplicativos Web progressivos são sites que funcionam como os aplicativos nativos em diferentes navegadores e uma ampla variedade de dispositivos Windows 10. [Assista ao vídeo](https://youtu.be/ugAewC3308Y) para saber mais e, em seguida, [Confira os documentos](http://aka.ms/Windows-PWA) para começar.

### <a name="vs-code-series"></a>Série de código do VS

Confira nossa [novos série de vídeos no Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obter informações sobre o que é o VSCode, como usá-lo e como ele foi criado.

### <a name="one-dev-question"></a>Uma pergunta sobre desenvolvimento

Série de vídeos a uma pergunta sobre desenvolvimento, os desenvolvedores da Microsoft há muito tempo abrangem uma série de perguntas sobre o desenvolvimento do Windows, cultura de equipe e histórico. Veja as perguntas mais recentes que podemos respondeu!

Raymond Chen:

* [Por que os arquivos de programas e arquivos de programas (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [Por que é COM tão complicado?](https://youtu.be/-gkXAV-StVA )
* [Qual foi a primeira entrevista como da Microsoft?](https://youtu.be/qRb6otsHG5c)
