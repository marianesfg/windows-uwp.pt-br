---
author: QuinnRadich
title: What's New in Windows Docs em julho de 2018 - como desenvolver aplicativos UWP
description: Orientação do desenvolvedor, vídeos, exemplos e novos recursos foram adicionados a documentação do desenvolvedor de Windows 10 de julho de 2018.
keywords: What's new, atualização, recursos, orientação do desenvolvedor, Windows 10, julho
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f41d25fd6757e5d3f80d00de341168de4f34e946
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2840994"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>What's New in os documentos do desenvolvedor de Windows em julho de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. As seguintes visões gerais do recurso, orientação do desenvolvedor, vídeos e amostras tenham sido disponibilizadas no mês de julho.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="progressive-web-apps-on-windows"></a>Progressivo Web Apps em Windows

[Progressivo Web Apps (PWAs)](https://developer.microsoft.com/windows/pwa) são simplesmente os aplicativos web que estão [progressivamente aprimorada](https://wikipedia.org/wiki/Progressive_enhancement) com recursos nativos de app semelhantes em dar suporte a plataformas e mecanismos de navegador, a instalação de lançamento do homescreen, suporte offline e push notificações. No Windows 10 com o mecanismo do Microsoft Edge (EdgeHTML), PWAs aproveitar a vantagem adicional de execução [independentemente da janela do navegador como apps UWP.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Uma imagem da PWAs em ação](images/progressive-web-apps.jpg)

Confira nossos guias PWA para:

* [Criar um aplicativo simples de web como um PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Aprimorar sua PWA com o tempo de execução do Windows](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [Publicar seu PWA para o armazenamento do Microsoft](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>Bloco de notas

Disponível no Windows 10 Insider Preview Build 17713, [o bloco de notas foi atualizado com vários novos recursos](http://aka.ms/ant-man). Suporte para Unix/Linux (LF) e Mac (CR) terminações da linha, em volta localizar/substituir e zoom agora estão disponíveis aos [Invasores internos do Windows](https://insider.windows.com/). 

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="design-landing-page"></a>Página de aterrissagem do design

Confira o [atualizado a página inicial de Design](https://developer.microsoft.com/windows/apps/design) para uma visão geral de um resumo geral de áreas de design UWP e informações sobre as últimas atualizações Design Fluent.

### <a name="design-toolkits"></a>Kits de ferramentas de design

Os kits de ferramentas XD Adobe e Adobe Illustrator foram atualizados com novos recursos. Esses kits de ferramentas de design fornecem controles e modelos de layout para projetar UWP aplicativos. [Check-out aqui.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

Adicionamos vários novos tópicos de [documentação WebVR](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [O que é WebVR?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) Explica o que é WebVR, por que você deve usá-lo e como começar a desenvolver para ele.

* [WebVR no progressiva Web Apps](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): Saiba como adicionar WebVR para um progressiva Web App (PWA).

* [WebVR na exibição da Web](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Saiba como adicionar WebVR a um controle de exibição da Web em um aplicativo do Windows 10.

* [Demonstrações de WebVR](https://docs.microsoft.com/microsoft-edge/webvr/demos): o Check-out de alguns demonstrações WebVR usando Microsoft Edge e um fone de ouvido imersivo realidade misto do Windows.

Além disso, que fizemos algumas atualizações às páginas existentes:

* O sumário agora é melhor organizado em quatro distintas classificações de nível superior: **conceitos básicos**, **desenvolvimento**, **demonstrações**e **recursos**.

* [Guia do desenvolvedor do WebVR (página inicial)](https://docs.microsoft.com/microsoft-edge/webvr/): atualizada aparência, com maiores imagens e ícones e demonstração nova.

* [Usando WebVR com borda Microsoft](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): atualizado para incluir informações sobre o Windows Update de 2018 10 de abril.

## <a name="videos"></a>Vídeos

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>Guia de Introdução para desenvolvedores: criar e personalizar um formulário no Windows 10

Nosso [docs Introdução](../get-started/index.md) para desenvolvedores do Windows agora fornecem uma experiência prática com tarefas de desenvolvimento de aplicativo básico. Este vídeo o orienta através de um desses tópicos e aborda as Noções básicas da criação de um formulário da interface do usuário em seu aplicativo. [Assista ao vídeo](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) para ver o código em ação, [check-out do tópico.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Aprimorar sua Bot com bate-papo de identificação do projeto

Bate-papo do projeto personalidade permite adicionar uma pessoa personalizável para seus bots de bate-papo. Integrando com o Microsoft Bot Framework SDK, você pode adicionar recursos de pequeno-talk para interagir com os clientes o modo mais conversação. [Assista ao vídeo](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) para aprender a implementar e [testar a demonstração interativa](http://aka.ms/PersonalityChat) para uma experiência prática.

### <a name="one-dev-question"></a>Uma pergunta de desenv.

A série de vídeos de uma pergunta Dev, há muito tempo desenvolvedores da Microsoft cobrem uma série de perguntas sobre o desenvolvimento, cultura de equipe e histórico do Windows. Eis as perguntas mais recentes que podemos atender!

Raymond Chen:

* [Por que você aplicou à Microsoft?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [Por que não podemos permitir que os desenvolvedores alterem o dispositivo de áudio padrão?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [Por que estão tantos assíncrono de funções UWP?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>Exemplos

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + / WinRT

O aplicativo de amostra do Photo Editor apresenta desenvolvimento com o [C + + / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) projeção de idioma. O aplicativo permite que você recupere fotos da biblioteca de **imagens** e, em seguida, editar uma imagem de eleito com efeitos de foto associada. [Clonar ou baixar o exemplo aqui.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![Um exemplo da amostra em ação](images/photo-editor-banner.png)
