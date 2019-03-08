---
title: O que há de novo nos documentos do Windows em janeiro de 2019 - desenvolver aplicativos UWP
description: Diretrizes para desenvolvedores, vídeos e novos recursos foram adicionados para a documentação do desenvolvedor do Windows 10 de janeiro de 2019
keywords: o que há de novo, update, recursos, diretrizes para desenvolvedores, Windows 10, janeiro
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636571"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>O que há de novo nos documentos de desenvolvedor do Windows em janeiro de 2019

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Visões gerais de recursos, diretrizes para desenvolvedores e vídeos a seguir estavam disponíveis no mês de janeiro.

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="windows-development-on-microsoft-learn"></a>Desenvolvimento do Windows no Microsoft Learn

Microsoft Learn fornece novos aprendizado prático e oportunidades de treinamento para desenvolvedores da Microsoft. Se você estiver interessado em aprender a desenvolver aplicativos do Windows, fazer check-out [nosso novo roteiro de aprendizagem](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) para uma introdução abrangente à plataforma, as ferramentas e como escrever seus primeiros aplicativos alguns.

![Imagem do plano de aprendizado de desenvolvimento do Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Direct3D 12 renderizar passes](/windows/desktop/direct3d12/direct3d-12-render-passes) pode melhorar o desempenho de seu renderizador se baseia-se no processamento de adiado baseado em bloco (TBDR), entre outras técnicas. A técnica o ajuda a seu renderizador para melhorar a eficiência GPU, permitindo que seu aplicativo identificar melhor renderização ordenação dados e requisitos de dependências de recursos e reduzindo o tráfego de memória da memória-chip.

### <a name="msix-modification-packages"></a>Pacotes de modificação de MSIX

Windows 10 versão 1809 suporte aprimorado para [pacotes de modificação de MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Pacotes de modificação podem incluir baseada no registro de plug-ins e a personalização associada e permitirá que um aplicativo implantado por meio de MSIX para usar um registro virtual e executados conforme o esperado.

![Criação de pacote de modificação de MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Código-fonte aberto do WPF, Windows Forms e WinUI

As estruturas do WPF, Windows Forms e UX WinUI agora estão disponíveis para contribuições de código-fonte aberto no GitHub. Para obter mais informações e links, consulte o [Compilando o blog de aplicativos do Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Aplicativos Web progressivo para Xbox

Com o [progressivo aplicativos Web para o Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), você pode estender um aplicativo web e disponibilizá-lo como um aplicativo do Xbox One por meio da Microsoft Store enquanto ainda continua a usar as estruturas existentes, o CDN e o servidor back-end. Geralmente, você pode empacotar seu PWA para Xbox One da mesma maneira que faria para Windows, no entanto, há várias diferenças importantes que este guia orientará você.

### <a name="windows-machine-learning"></a>Aprendizado de máquina do Windows

Podemos ter reestruturado [a página de aterrissagem para APIs WinML](https://docs.microsoft.com/windows/ai/api-reference)e adicionada a nova documentação para operador personalizado de WinML e APIs nativas.

[Treinar um modelo com o PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) fornece orientação sobre como treinar um modelo usando a estrutura de PyTorch localmente ou na nuvem. Em seguida, você pode baixar esse modelo como um arquivo ONNX e usá-lo em seus aplicativos WinML.

![Gráfico de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="choose-your-platform"></a>Escolha sua plataforma

Interessado em criar um novo aplicativo de área de trabalho? Confira nosso remodelada [escolha sua plataforma](https://docs.microsoft.com/windows/desktop/choose-your-technology) página para obter descrições detalhadas e comparações de plataformas Windows Forms, WPF e UWP e obter mais informações sobre a API do Win32.

### <a name="faqs-on-win32-webview"></a>Perguntas frequentes sobre o WebView do Win32

Nossos [perguntas frequentes](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) fornece respostas para perguntas comuns ao usar a exibição da Web do Microsoft Edge em aplicativos da área de trabalho, bem como links para exemplos e recursos adicionais.

### <a name="japanese-era-change"></a>Alteração era japonesa

[Preparar seu aplicativo para que a alteração era japonesa](../design/globalizing/japanese-era-change.md) mostra como garantir que seu aplicativo está pronto para a era japonês alterar conjunto entrem de Windows colocar em 1 de maio de 2019. [Essa página também está disponível em japonês](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicativos Web progressivos

Aplicativos de Web progressivo são sites da web que funcionam como aplicativos nativos em navegadores diferentes e uma ampla variedade de dispositivos Windows 10. [Assista ao vídeo](https://youtu.be/ugAewC3308Y) para saber mais e então [fazer check-out de documentos do](https://aka.ms/Windows-PWA) para começar a usar.

### <a name="vs-code-series"></a>Série de código do VS

Confira nossos [série de vídeos de novo no Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obter informações sobre o que é o VSCode, como usá-lo e como ele foi criado.

### <a name="one-dev-question"></a>Uma pergunta de desenvolvimento

A série de vídeos de uma pergunta de desenvolvimento, os desenvolvedores da Microsoft há muito tempo abrangem uma série de perguntas sobre desenvolvimento Windows, a cultura da equipe e o histórico. Eis aqui as perguntas mais recente que podemos ter respondido!

Raymond Chen:

* [Por que ter arquivos de programas e arquivos de programas (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [Por que o COM é tão complicado?](https://youtu.be/-gkXAV-StVA )
* [Qual foi a primeira entrevista, como para a Microsoft?](https://youtu.be/qRb6otsHG5c)
