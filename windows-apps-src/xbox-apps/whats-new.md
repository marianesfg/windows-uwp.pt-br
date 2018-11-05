---
author: v-angraf
title: O que há de novo para UWP no Xbox One
description: Destaca os novos recursos para aplicativos UWP em Xbox One.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cc2168014e714de0b43b6ffffe84126764f0a4a3
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6024285"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Novidades para desenvolvedores na atualização mais recente da UWP em Xbox One

A atualização mais recente da Universal Windows Platform (UWP) no Xbox One contém os seguintes novos recursos, as atualizações de recursos existentes e correções de bugs.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 não há suporte para aplicativos e jogos no Xbox  
Xbox não dá mais suporte ao desenvolvimento de aplicativos x86 ou envios de aplicativos x86 para a loja.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Aplicativos podem agora dão suporte à navegação de volta para o aplicativo anterior 
UWP no Xbox One aplicativos agora pode dar suporte a navegação de volta para o aplicativo anterior. Para fazer isso, assinar o evento [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) e defina a propriedade **Handled** como **false** em seu manipulador de eventos.

> [!NOTE]
> Por motivos de compatibilidade, essa funcionalidade está disponível somente para aplicativos que são criados com a versão mais recente da UWP no Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>A dev Home agora é a experiência inicial padrão em consoles de desenvolvimento
Consoles de desenvolvimento agora iniciem Dev Home como a experiência inicial padrão. Isso permite que você acertar trabalhar sem a necessidade de clicam na tela inicial do varejo. Dev Home agora inclui uma ação rápida para iniciar a tela inicial de varejo. Além disso, uma nova configuração permite que você faça varejo domésticos a experiência padrão. 

## <a name="new-dev-home-user-interface"></a>Nova interface do usuário Dev Home
A interface do usuário Dev Home agora inclui os seguintes aprimoramentos de produtividade:
 - Dados importantes, como endereço IP e recuperação versão agora é exibida na parte superior da tela para visibilidade. 
 - Dev Home agora tem uma IU com guias que agrupa ferramentas em conjuntos lógicos, permitindo que a navegação rápida.
 - Botões de ação rápida na primeira guia de início do desenvolvimento permitem acesso rápido às ações mais comumente usados. 

## <a name="wdp-for-xbox-enhancements"></a>WDP para aprimoramentos do Xbox
O Windows Device Portal (WDP) agora inclui suporte adicional para as configurações do console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Agora você pode alternar o tipo de seu título UWP entre "Aplicativo" e "Jogo"
Alternar o tipo de seu título UWP entre "Aplicativo" e "Jogo" permite que você teste cenários de jogos sem publicação na loja. Na Dev Home, selecione o aplicativo no painel de **jogos e aplicativos** , pressione o botão modo de exibição no controlador, selecionar os **detalhes do aplicativo** e, em seguida, altere o tipo para "Aplicativo" ou "Jogo".

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)
 - Já é possível fazer uma captura de tela do console. Para obter mais informações sobre fazer uma captura de tela, consulte o tópico de referência [/ext/screenshot](wdp-media-capture-api.md).
 - A ferramenta pode implantar uma compilação de arquivo flexível do aplicativo. Para obter mais informações compilações de arquivo flexíveis, consulte o tópico de referência [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Os arquivos de desenvolvedor no console podem ser acessados pelo Explorador de Arquivos no computador de desenvolvimento. Para obter mais informações sobre como acessar arquivos por meio do Explorador de Arquivos, consulte o tópico de referência [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)
