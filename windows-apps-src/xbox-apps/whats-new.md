---
author: v-angraf
title: O que há de novo para UWP no Xbox One
description: Destaca os novos recursos para aplicativos UWP em Xbox One.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cbabe9d31b5b9762320df8e4a92d19ae4e33497d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "301452"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Novidades para desenvolvedores na atualização mais recente da UWP em Xbox One

A atualização mais recente do Universal Windows plataforma (UWP) em um Xbox contém os seguintes novos recursos, as atualizações recursos existentes e correções de erros.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 jogos e aplicativos não são mais suportados no Xbox  
Xbox não dá mais suporte ao desenvolvimento de aplicativos x86 ou envios de aplicativos x86 para a loja.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Aplicativos agora podem suportar naveguem de volta para o aplicativo anterior 
UWP nos aplicativos um Xbox agora pode suportar naveguem de volta para o aplicativo anterior. Para fazer isso, assinar o evento [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) e definir a propriedade **Handled** como **Falso** no manipulador de eventos.

> [!NOTE]
> Por motivos de compatibilidade, essa funcionalidade está disponível somente para aplicativos que são compilados com a versão mais recente do UWP no Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Home dev agora é a experiência de residencial padrão em consoles de desenvolvimento
Consoles de desenvolvimento agora início Dev Home como a experiência de página inicial padrão. Isso permite que você obtenha direita para funcionar sem a necessidade de clicar na tela inicial varejo. Home dev agora inclui uma ação rápida para iniciar a tela inicial de varejo. Além disso, uma nova configuração permite que você verifique varejo residencial a experiência padrão. 

## <a name="new-dev-home-user-interface"></a>Nova interface do usuário Dev Home
A interface do usuário Dev Home agora inclui os seguintes aprimoramentos de produtividade:
 - Dados importantes, como endereço IP e a versão de recuperação será agora exibido na parte superior da tela para visibilidade. 
 - Home dev agora tem uma UI com guias que agrupa ferramentas em conjuntos de lógicos, permitindo a navegação rápida.
 - Botões de ação rápida na primeira guia da Home Dev permitem acesso rápido às ações mais comumente usadas. 

## <a name="wdp-for-xbox-enhancements"></a>WDP para aprimoramentos do Xbox
O Portal de dispositivo do Windows (WDP) agora inclui suporte adicional para as configurações do console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Agora você pode alternar o tipo de seu título UWP entre "Aplicativo" e "Jogo"
Alternar o tipo de seu título UWP entre "Aplicativo" e "Jogo" permite que você teste os cenários jogos sem publicação para o repositório. Em casa Dev, selecione o aplicativo no painel **jogos e aplicativos** , pressione o botão Exibir no controlador, selecione **detalhes do aplicativo** e, em seguida, altere o tipo para "Aplicativo" ou "Jogo".

## <a name="see-also"></a>Veja também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)
 - Já é possível fazer uma captura de tela do console. Para obter mais informações sobre fazer uma captura de tela, consulte o tópico de referência [/ext/screenshot](wdp-media-capture-api.md).
 - A ferramenta pode implantar uma compilação de arquivo flexível do aplicativo. Para obter mais informações compilações de arquivo flexíveis, consulte o tópico de referência [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Os arquivos de desenvolvedor no console podem ser acessados pelo Explorador de Arquivos no computador de desenvolvimento. Para obter mais informações sobre como acessar arquivos por meio do Explorador de Arquivos, consulte o tópico de referência [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)
