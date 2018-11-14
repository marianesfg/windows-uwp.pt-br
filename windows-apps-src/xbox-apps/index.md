---
author: Mtoepke
title: UWP no Xbox One
description: Como criar apps para a UWP (Plataforma Universal do Windows) no Xbox One.
ms.author: mstahl
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: e7c14c51c0b5b09b96859a8aba97172485a14c85
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6669207"
---
# <a name="uwp-on-xbox-one"></a>UWP no Xbox One

Comece a criar aplicativos para a UWP (Plataforma Universal de Aplicativos) no Xbox One.

A UWP no Xbox One dá suporte ao desenvolvimento de aplicativos e jogos. Você não precisa fazer parte de um programa para desenvolvedores para experimentar, criar e testar jogos ou apps no Xbox. Tudo o que você precisa é uma [conta de desenvolvedor](https://developer.microsoft.com/en-us/store/register) no [Partner Center](https://partner.microsoft.com/dashboard). Quando você estiver pronto para publicar e vender jogos no Xbox One ou tirar proveito do Xbox Live no Windows 10, precisará ingressar no [Programa de Criadores do Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) ou ser um desenvolvedor do [ID@Xbox](http://www.xbox.com/Developers/id). Se você planeja ser um desenvolvedor do ID@Xbox, recomendamos sua inscrição no programa primeiro antes de criar uma conta de desenvolvedor. Para saber mais, veja a [Visão geral do programa para desenvolvedores](../xbox-live/developer-program-overview.md).

Esta seção inclui as etapas de instalação, um guia do processo de autenticação, informações sobre como instalar as versões necessárias das ferramentas do Visual Studio e do Windows 10 e as etapas para criar, executar e depurar seu primeiro aplicativo simples. 

| Tópico      | Descrição |
|------------|-------------|
|[Introdução](getting-started.md)| Guia de introdução ao desenvolvimento da UWP no Xbox One. |
|[Novidades](whats-new.md)| Destaca os novos recursos na UWP no Xbox One. |
|[Ativação do Modo de Desenvolvedor do Xbox One](devkit-activation.md)| Explica como habilitar o modo de desenvolvedor no Xbox One. |
|[Desativando o modo de desenvolvedor no Xbox One](devkit-deactivation.md)| Explica como desabilitar o modo de desenvolvedor no Xbox One. |
|[Configurar seu ambiente de desenvolvimento da UWP no Xbox](development-environment-setup.md)| Descreve as etapas para configurar e testar seu ambiente de desenvolvimento do Xbox One. |
|[Exemplos](samples.md)| Ponteiro para o local do GitHub – TVHelpers - onde você encontrará amostras de XAML e JavaScript úteis para ajudar você a começar a desenvolver para o Xbox. As amostras incluem um modelo de aplicativo de mídia XAML completo, bem como navegação de controlador automático, reprodução de mídia avançada e pesquisa de tecnologias baseadas na Web. |
|[Problemas conhecidos](known-issues.md)| Problemas conhecidos com UWP no Xbox One. |
|[Perguntas frequentes](frequently-asked-questions.md)| Perguntas frequentes relacionadas à UWP no Xbox One. |
|[Ferramentas](introduction-to-xbox-tools.md)| Descreve a ferramenta _Dev Home_ específica do Xbox One, como usar o Windows Device Portal e como configurar o Visual Studio para desenvolvimento. Esta seção também orienta um novo desenvolvedor por meio de seu primeiro aplicativo UWP do Xbox e explica como usar a ferramenta Fiddler para exibir o tráfego de rede. |
| [Evento de desenvolvimento de aplicativo no Xbox](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | O evento de Desenvolvimento de aplicativo no Xbox é um ótimo ponto de partida para desenvolvedores que ainda não criaram aplicativos no Xbox. Assista as sessões gravadas e leia as postagens de blog do evento. |
|[Projetar para Xbox e TV](../design/devices/designing-for-tv.md)| Descreve as práticas recomendadas para criar um aplicativo que será exibido em uma TV e usará um controlador de entrada. |
|[Práticas recomendadas para o Xbox](tailoring-for-xbox.md)| Como desativar o modo do mouse, desenhar as bordas da tela e desabilitar o dimensionamento. |
|[Usar a fala para invocar os elementos da interface do usuário](ves-on-xbox.md)| Descreve as práticas recomendadas para oferecer ao Shell de voz habilitada em aplicativos UWP no Xbox. |
|[Recursos do sistema para aplicativos UWP e jogos no Xbox One](system-resource-allocation.md)| Descreve os recursos disponíveis para seu aplicativo quando ele está sendo executado no Xbox One. |
|[Introdução aos aplicativos multiusuário](multi-user-applications.md)| Descreve os MUAs (aplicativos para multiusuário) no Xbox One. |
| [Automatizar tarefas de desenvolvimento do Xbox One](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | O projeto WindowsDevicePortalWrapper no GitHub fornece uma biblioteca que permite a automação de tarefas de desenvolvimento comuns, como a implantação ou o lançamento de um aplicativo. O projeto inclui um exemplo, XboxWdpDriver.exe, que demonstra como usar as APIs para tarefas comuns. |
|[Trazendo jogos existentes para o Xbox](development-lanes-landing.md)|Com base na tecnologia em que seu jogo for criado, podemos direcioná-lo para instruções passo a passo que podem agilizar o processo de trazer seu jogo para o Xbox usando a UWP.|
|[Recursos da UWP que ainda não são suportados no Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  Descreve as áreas de recursos da UWP que não estão totalmente funcionais no Xbox One.|

## <a name="videos"></a>Vídeos

A seguir, no Channel 9, uma excelente fonte de informações para a compilação de aplicativos incríveis em Xbox:

* [Compilação de excelentes aplicativos da Plataforma Universal do Windows (UWP) para Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
* [Adaptar o aplicativo para Xbox One e TV](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [Desenvolvimento de UWP 1: compilação de uma interface do usuário adaptável](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Aplicativos Web além do navegador: plataforma cruzada encontra entre dispositivos](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>Veja também

- [Automatizar o lançamento de aplicativos UWP do Windows 10](automate-launching-uwp-apps.md)
- [CPUSets para desenvolvimento de jogos](cpusets-games.md)