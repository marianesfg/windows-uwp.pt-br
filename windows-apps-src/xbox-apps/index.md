---
title: UWP no Xbox One
description: Como criar aplicativos para a UWP (Plataforma Universal do Windows) no Xbox One.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: aecb0a6e6a7d9dfbe05a43b760b044d032fff4e1
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685029"
---
# <a name="uwp-on-xbox-one"></a>UWP no Xbox One

Comece a criar aplicativos para a UWP (Plataforma Universal do Windows) no Xbox One.

A UWP no Xbox One dá suporte ao desenvolvimento de aplicativos e jogos. Você não precisa fazer parte de um programa para desenvolvedores para experimentar, criar e testar jogos ou aplicativos no Xbox. Tudo o que você precisa é uma [conta de desenvolvedor](https://developer.microsoft.com/store/register) no [Partner Center](https://partner.microsoft.com/dashboard). Quando você estiver pronto para publicar e vender jogos no Xbox One ou tirar proveito do Xbox Live no Windows 10, precisará ingressar no [Programa de Criadores do Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) ou ser um desenvolvedor do [ID@Xbox](https://www.xbox.com/Developers/id). Se você planeja ser um desenvolvedor do ID@Xbox, recomendamos sua inscrição no programa primeiro antes de criar uma conta de desenvolvedor. Para saber mais, veja a [Visão geral do programa para desenvolvedores](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview).

Esta seção inclui as etapas de instalação, um guia do processo de autenticação, informações sobre como instalar as versões necessárias das ferramentas do Visual Studio e do Windows 10 e as etapas para criar, executar e depurar seu primeiro aplicativo simples. 

| Tópico      | Descrição |
|------------|-------------|
|[Introdução](getting-started.md)| Guia de introdução ao desenvolvimento para a UWP no Xbox One. |
|[Quais são as novidades?](whats-new.md)| Destaca os novos recursos da UWP no Xbox One. |
|[Ativação do Modo de Desenvolvedor do Xbox One](devkit-activation.md)| Explica como habilitar o modo de desenvolvedor no Xbox One. |
|[Desabilitando o modo de desenvolvedor no Xbox One](devkit-deactivation.md)| Explica como desabilitar o modo de desenvolvedor no Xbox One. |
|[Configurar seu ambiente de desenvolvimento da UWP no Xbox](development-environment-setup.md)| Descreve as etapas para configurar e testar seu ambiente de desenvolvimento do Xbox One. |
|[Amostras](samples.md)| Ponteiro para o local do GitHub TVHelpers – em que você encontrará amostras de XAML e JavaScript úteis para ajudar você a começar a desenvolver para o Xbox. As amostras incluem um modelo de aplicativo de mídia XAML completo, bem como navegação com controlador automático, reprodução de mídia avançada e pesquisa de tecnologias baseadas na Web. |
|[Problemas conhecidos](known-issues.md)| Problemas conhecidos com a UWP no Xbox One. |
|[Perguntas frequentes](frequently-asked-questions.md)| Perguntas frequentes relacionadas à UWP no Xbox One. |
|[Ferramentas](introduction-to-xbox-tools.md)| Descreve a ferramenta _Dev Home_ específica do Xbox One, como usar o Portal de Dispositivos do Windows e como configurar o Visual Studio para desenvolvimento. Esta seção também orienta um novo desenvolvedor em seu primeiro aplicativo do Xbox para a UWP e explica como usar a ferramenta Fiddler para exibir o tráfego de rede. |
| [Evento de desenvolvimento de aplicativo no Xbox](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | O evento de Desenvolvimento de Aplicativo no Xbox é um ótimo ponto de partida para desenvolvedores que não têm experiência na criação de aplicativos no Xbox. Assista às sessões gravadas e leia as postagens de blog do evento. |
|[Projetando para TV e Xbox](../design/devices/designing-for-tv.md)| Descreve as práticas recomendadas para criar um aplicativo que será exibido em uma TV e usará um controlador para entrada. |
|[Práticas recomendadas para o Xbox](tailoring-for-xbox.md)| Como desativar o modo do mouse, desenhar as bordas da tela e desabilitar o dimensionamento. |
|[Usar a fala para invocar os elementos da interface do usuário](ves-on-xbox.md)| Descreve as práticas recomendadas para oferecer ao Shell de voz habilitada em aplicativos UWP no Xbox. |
|[Recursos do sistema para jogos e aplicativos UWP no Xbox One](system-resource-allocation.md)| Descreve os recursos disponíveis para seu aplicativo quando ele está sendo executado no Xbox One. |
|[Introdução aos aplicativos multiusuário](multi-user-applications.md)| Descreve os MUAs (aplicativos para multiusuário) no Xbox One. |
| [Automatizar tarefas de desenvolvimento do Xbox One](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | O projeto WindowsDevicePortalWrapper no GitHub fornece uma biblioteca que permite a automação de tarefas de desenvolvimento comuns, como a implantação ou o lançamento de um aplicativo. O projeto inclui um exemplo, XboxWdpDriver.exe, que demonstra como usar as APIs para tarefas comuns. |
|[Trazendo jogos existentes para o Xbox](development-lanes-landing.md)|Com base na tecnologia em que seu jogo foi criado, podemos direcioná-lo para instruções passo a passo que podem agilizar o processo de trazer seu jogo para o Xbox usando a UWP.|
|[Recursos da UWP que ainda não são compatíveis com o Xbox One](https://docs.microsoft.com/uwp/extension-sdks/uwp-limitations-on-xbox?redirectedfrom=MSDN)|  Descreve as áreas de recursos da UWP que não estão totalmente funcionais no Xbox One.|

## <a name="videos"></a>Vídeos

A seguir, no Canal 9, uma excelente fonte de informações para a criação de aplicativos incríveis em Xbox:

* [Criação de excelentes aplicativos da UWP (Plataforma Universal do Windows) para Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
* [Adaptar o aplicativo para Xbox One e TV](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [Desenvolvimento de UWP 1: Criando uma interface do usuário adaptável](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Além de aplicativos Web do navegador: Multiplataforma encontra multidispositivo](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>Veja também

- [Automatizar o lançamento de aplicativos UWP do Windows 10](automate-launching-uwp-apps.md)
- [CPUSets para desenvolvimento de jogos](cpusets-games.md)
- [Aplicativos Web progressivos para Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)