---
title: O que há de novo nos documentos do Windows em julho de 2018 – desenvolva aplicativos UWP
description: Foram adicionados novos recursos, vídeos, exemplos e orientações para desenvolvedores para a documentação do desenvolvedor do Windows 10 de julho de 2018.
keywords: o que há de novo, update, recursos, diretrizes para desenvolvedores, Windows 10 de julho
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22a3a9614a4488791a36f81a3d4dedac572111b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596831"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>O que há de novo nos documentos de desenvolvedor do Windows em julho de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Os seguintes visões gerais de recursos, diretrizes para desenvolvedores, vídeos e exemplos estavam disponíveis no mês de julho.

[Instale as ferramentas e o SDK](https://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="progressive-web-apps-on-windows"></a>Aplicativos Web progressivo no Windows

[Aplicativos da Web progressivo (PWAs)](https://developer.microsoft.com/windows/pwa) são simplesmente os aplicativos web que estejam [aprimorada progressivamente](https://wikipedia.org/wiki/Progressive_enhancement) com recursos nativos do aplicativo semelhante no suporte a plataformas e mecanismos de navegador, como instalação de inicialização de homescreen off-line suporte e enviar notificações por push. No Windows 10 com o mecanismo do Microsoft Edge (EdgeHTML), PWAs aproveitam a vantagem adicional de execução [independentemente da janela do navegador como aplicativos da UWP.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Uma imagem de PWAs em ação](images/progressive-web-apps.jpg)

Confira nossos guias de PWA para:

* [Criar um aplicativo web simples como um PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Aprimore seu PWA com o tempo de execução do Windows](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [Publicar seu PWA para a Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>Bloco de notas

Disponível no Windows 10 Insider Preview Build 17713, [bloco de notas foi atualizado com muitos recursos novos](https://aka.ms/ant-man). Aumentar o zoom, embrulhado localizar/substituir e suporte para terminações de linha (LF) Unix/Linux e Mac (CR) agora estão disponíveis para [Windows Insiders](https://insider.windows.com/). 

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="design-landing-page"></a>Página de aterrissagem do design

Fazer check-out a [atualizada da página de aterrissagem do Design](https://developer.microsoft.com/windows/apps/design) para uma visão geral de uma visão geral da UWP design áreas e obter informações sobre as últimas adições ao Design Fluent.

### <a name="design-toolkits"></a>Kits de ferramentas de design

Os kits de ferramentas do Adobe Illustrator e de XD Adobe foram atualizados com novos recursos. Esses kits de ferramentas de design fornecem modelos de layout e controles para projetar aplicativos UWP. [Check-out aqui.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

Adicionamos vários tópicos novos para o [WebVR documentação](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [O que é WebVR?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) Explica o que é WebVR, por que você deve usá-lo e como começar a desenvolver para ele.

* [WebVR em aplicativos Web progressivo](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): Saiba como adicionar WebVR para um aplicativo de Web progressivo (PWA).

* [WebVR no WebView](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Saiba como adicionar WebVR a um controle WebView em um aplicativo do Windows 10.

* [Demonstrações de WebVR](https://docs.microsoft.com/microsoft-edge/webvr/demos): Confira algumas demonstrações WebVR usando um fone de ouvido imersivo Windows Mixed Reality e Microsoft Edge.

Além disso, fizemos algumas atualizações às páginas existentes:

* O sumário agora é melhor organizado em quatro segmentos distintos de nível superior: **Conceitos básicos**, **desenvolvimento**, **recursos**, e **demonstrações**.

* [Guia do desenvolvedor WebVR (página de aterrissagem)](https://docs.microsoft.com/microsoft-edge/webvr/): Atualizada a aparência e, com maiores imagens, ícones e demonstração.

* [Usando WebVR com o Microsoft Edge](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): Atualizado para incluir informações sobre o Windows Update 10 de abril de 2018.

## <a name="videos"></a>Vídeos

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>Introdução para desenvolvedores: Criar e personalizar um formulário no Windows 10

Nossos [Introdução ao docs](../get-started/index.md) para Windows os desenvolvedores agora fornecem uma experiência prática com a tarefa de desenvolvimento de aplicativo básico. Este vídeo o orienta por meio de um desses tópicos e aborda os conceitos básicos da criação de um formulário da interface do usuário em seu aplicativo. [Assista ao vídeo](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) para ver o código em ação, em seguida, [Confira o tópico por conta própria.](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Aprimore seu Bot com os bate-papo de personalidade do projeto

Bate-papo de personalidade de projeto permite que você adicione uma persona personalizável para seus bots de bate-papo. Integrando com o SDK do Microsoft Bot Framework, você pode adicionar recursos de palestra pequeno para uma maneira mais conversação interagir com os clientes. [Assista ao vídeo](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) para aprender a implementá-lo, em seguida, [Experimente nossa demonstração interativa](https://aka.ms/PersonalityChat) para uma experiência prática.

### <a name="one-dev-question"></a>Uma pergunta de desenvolvimento

A série de vídeos de uma pergunta de desenvolvimento, os desenvolvedores da Microsoft há muito tempo abrangem uma série de perguntas sobre desenvolvimento Windows, a cultura da equipe e o histórico. Eis aqui as perguntas mais recente que podemos ter respondido!

Raymond Chen:

* [Por que você aplicou à Microsoft?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [Por que não podemos permitir que os desenvolvedores alterem o dispositivo de áudio padrão?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [Por que são tantos async de funções UWP?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>Exemplos

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + / WinRT

O aplicativo de exemplo de Photo Editor apresenta o desenvolvimento com o [C + + c++ /CLI WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) projeção de linguagem. O aplicativo permite que você recupere fotos do **imagens** biblioteca e, em seguida, editar uma imagem escolhida com efeitos de foto associada. [Clonar ou baixar o exemplo aqui.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![Um exemplo de como o exemplo em ação](images/photo-editor-banner.png)
