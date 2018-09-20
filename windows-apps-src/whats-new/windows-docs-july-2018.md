---
author: QuinnRadich
title: Novidades do Windows Docs em julho de 2018 - desenvolver aplicativos UWP
description: Novos recursos, vídeos, amostras e diretrizes para desenvolvedores têm foram adicionados à documentação do desenvolvedor do Windows 10 de julho de 2018.
keywords: Novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, julho
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f41d25fd6757e5d3f80d00de341168de4f34e946
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4083069"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>Novidades dos documentos de desenvolvedor do Windows em julho de 2018

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. O seguinte visões gerais de recursos, diretrizes para desenvolvedores, vídeos e amostras foram disponibilizadas no mês de julho.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/create-uwp-apps.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="progressive-web-apps-on-windows"></a>Aplicativos Web progressivos no Windows

[Aplicativos Web progressivos (PWAs)](https://developer.microsoft.com/windows/pwa) são simplesmente os aplicativos web que são [progressivamente avançado](https://wikipedia.org/wiki/Progressive_enhancement) com recursos nativos do aplicativo semelhantes em dar suporte a plataformas e mecanismos de navegador, como instalação de inicialização de homescreen, suporte offline e por push notificações. No Windows 10 com o mecanismo Microsoft Edge (EdgeHTML), PWAs aproveitam a vantagem adicional de execução [independentemente da janela do navegador, como aplicativos UWP.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Uma imagem de PWAs em ação](images/progressive-web-apps.jpg)

Confira nossos guias PWA para:

* [Criar um aplicativo web simples como um PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Aprimorar seu PWA com o tempo de execução do Windows](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [Publicar seu PWA na Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>Bloco de notas

Está disponível no Windows 10 Insider Preview Build 17713, [o bloco de notas foi atualizado com muitos recursos novos](http://aka.ms/ant-man). Zoom, delimitador localizar/substituir e suporte para terminações de linha Unix/Linux (AL) e o Mac (CR) agora estão disponíveis para [Os participantes do Windows Insider](https://insider.windows.com/). 

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="design-landing-page"></a>Página de aterrissagem de design

Confira o [atualizado página inicial de Design](https://developer.microsoft.com/windows/apps/design) para uma visão geral de uma visão geral de áreas de design da UWP e informações sobre as últimas atualizações ao Design Fluent.

### <a name="design-toolkits"></a>Kits de ferramentas de design

Kits de ferramentas do Adobe XD e Adobe Illustrator foram atualizados com os novos recursos. Esses kits de ferramentas de design fornecem controles e modelos de layout para criar aplicativos UWP. [Check-out aqui.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

Adicionamos vários novos tópicos a [WebVR documentação](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [O que é WebVR?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) Explica o que é WebVR, por que você deve usá-lo e como começar a desenvolver para ele.

* [WebVR em aplicativos Web progressivos](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): Saiba como adicionar WebVR para um aplicativo de Web progressivos (PWA).

* [WebVR no WebView](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Saiba como adicionar WebVR a um controle de exibição da Web em um aplicativo do Windows 10.

* [Demonstrações de WebVR](https://docs.microsoft.com/microsoft-edge/webvr/demos): Confira algumas demonstrações WebVR usando o Microsoft Edge e um headset imersivo do Windows Mixed Reality.

Além disso, fizemos algumas atualizações para páginas existentes:

* Sumário agora é melhor organizado em quatro buckets distintos de nível superior: **conceitos básicos**, **desenvolvimento**, **recursos**e **demonstrações**.

* [Guia do desenvolvedor WebVR (página inicial)](https://docs.microsoft.com/microsoft-edge/webvr/): atualizada aparência, com maiores imagens e ícones e nova demonstração.

* [Usando a WebVR com o Microsoft Edge](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): atualizado para incluir informações sobre o Windows Update de 10 de abril de 2018.

## <a name="videos"></a>Vídeos

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>Introdução para desenvolvedores: criar e personalizar um formulário no Windows 10

[Introdução documentos](../get-started/index.md) para desenvolvedores do Windows agora fornecem experiência prática com as tarefas de desenvolvimento de aplicativo básico. Este vídeo orienta você por meio de um desses tópicos e aborda as Noções básicas de criação de um formulário da interface do usuário em seu aplicativo. [Assista ao vídeo](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) para ver o código em ação, em seguida, [Confira o tópico por conta própria.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Aprimorar seu Bot com bate-papo de personalidade do projeto

Bate-papo do projeto personalidade permite que você adicionar uma pessoa personalizável para a robôs bate-papo. Por meio da integração com o SDK do Microsoft Bot estrutura, você pode adicionar recursos de pequeno falar uma maneira mais conversa interagir com os clientes. [Assista ao vídeo](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) para saber como implementar e [experimentar a demonstração interativa](http://aka.ms/PersonalityChat) para uma experiência prática.

### <a name="one-dev-question"></a>Uma pergunta sobre desenvolvimento

Série de vídeos a uma pergunta sobre desenvolvimento, há muito tempo desenvolvedores da Microsoft abrangem uma série de perguntas sobre o desenvolvimento do Windows, cultura de equipe e histórico. Veja as perguntas mais recentes que podemos respondeu!

Raymond Chen:

* [Por que você aplicou para a Microsoft?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [Por que não permitimos que os desenvolvedores alterar o dispositivo de áudio padrão?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [Por que são tantas UWP funções async?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>Exemplos

### <a name="photo-editor-cwinrt"></a>Editor de fotos C + c++ WinRT

O aplicativo de exemplo do Editor de fotos mostra o desenvolvimento com o [C++ c++ WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) projeção de linguagem. O aplicativo permite que você recupere fotos a partir da biblioteca **imagens** e, em seguida, edite uma imagem lecionada com efeitos de fotografia associado. [Clonar ou baixar o exemplo aqui.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![Um exemplo de exemplo em ação](images/photo-editor-banner.png)
