---
title: O que há de novo para UWP no Xbox One
description: Destaca os novos recursos para aplicativos UWP em Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: aff65e5f1b4771cbb33bc8b8219224042b7bf7e2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660681"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Novidades para desenvolvedores na atualização mais recente da UWP em Xbox One

A atualização mais recente da UWP (Plataforma Universal do Windows) no Xbox One contém os seguintes novos recursos, atualizações para os recursos existentes e correções de bugs.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>Aplicativos e jogos x86 não têm mais suporte no Xbox  
Xbox não dá mais suporte ao desenvolvimento de aplicativos x86 ou envios de aplicativos x86 para a loja.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Agora, os aplicativos agora podem oferecer suporte à navegação para o aplicativo anterior 
Os aplicativos UWP no Xbox One agora oferecem suporte à navegação de volta para o aplicativo anterior. Para fazer isso, assine o evento [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) e defina a propriedade **Handled** como **false** no manipulador de eventos.

> [!NOTE]
> Por motivos de compatibilidade, essa funcionalidade está disponível somente para aplicativos criados com a versão mais recente do UWP no Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>A Home Page de Desenvolvimento agora é a experiência inicial padrão em consoles de desenvolvimento
Os consoles de desenvolvimento agora iniciam a Home Page de Desenvolvimento como a experiência inicial padrão. Isso permite que você trabalhe sem precisar clicar na Home Page de Desenvolvimento de varejo. A Home Page de Desenvolvimento agora inclui uma ação rápida para iniciar a tela inicial de varejo. Além disso, uma nova configuração permite que você transforme o menu inicial na experiência padrão. 

## <a name="new-dev-home-user-interface"></a>Nova interface do usuário da Home Page de Desenvolvimento
A interface do usuário da Home Page de Desenvolvimento agora inclui as seguintes melhorias de produtividade:
 - Dados importantes, como endereço IP e versão de recuperação agora são exibidos na parte superior da tela para proporcionar visibilidade. 
 - A Home Page de Desenvolvimento agora tem uma interface do usuário que agrupa ferramentas em conjuntos de lógicos, permitindo a navegação rápida.
 - Botões de ação rápida na primeira guia da Home Page de Desenvolvimento permitem o acesso rápido às ações mais utilizadas. 

## <a name="wdp-for-xbox-enhancements"></a>WDP para aprimoramentos do Xbox
O Windows Device Portal (WDP) agora inclui suporte adicional para as configurações do console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Agora, você pode alternar o tipo no título UWP entre "Aplicativo" e "Jogo"
Alternar o tipo do título UWP entre "Aplicativo" e "Jogo" permite que você teste cenários de jogos sem publicá-los na Loja. Na Home Page de Desenvolvimento, selecione o aplicativo no painel **Jogos e Aplicativos**, pressione o botão de Modo de exibição no controle, selecione **Detalhes do aplicativo** e, em seguida, altere o tipo para "Aplicativo" ou "Jogo".

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)
 - Já é possível fazer uma captura de tela do console. Para obter mais informações sobre fazer uma captura de tela, consulte o tópico de referência [/ext/screenshot](wdp-media-capture-api.md).
 - A ferramenta pode implantar uma compilação de arquivo flexível do aplicativo. Para obter mais informações compilações de arquivo flexíveis, consulte o tópico de referência [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Os arquivos de desenvolvedor no console podem ser acessados pelo Explorador de Arquivos no computador de desenvolvimento. Para obter mais informações sobre como acessar arquivos por meio do Explorador de Arquivos, consulte o tópico de referência [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)
