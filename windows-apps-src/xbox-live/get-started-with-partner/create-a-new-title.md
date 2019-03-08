---
title: Criar um novo título
description: Saiba como criar um novo título do Xbox Live, usando o Partner Center.
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1aa2447a2044bec9b2013b30c05e45342b763fc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656561"
---
# <a name="create-a-new-title-for-xbox-live"></a>Criar um novo título para o Xbox Live

## <a name="introduction"></a>Introdução

Antes de escrever qualquer código, você deve configurar um novo título no portal de configuração do serviço.  Você pode aprender mais sobre a configuração de serviço na [configuração do serviço Xbox Live](../xbox-live-service-configuration.md)

Este artigo orientará você por esse processo com as seguintes suposições

1. Você está desenvolvendo um título de plataforma Universal do Windows (UWP).  Títulos UWP são executados no Xbox One, PCs desktop do Windows 10 e mobile
2. Você está configurando seu título na [Partner Center](https://partner.microsoft.com/dashboard).
3. Você está usando o Visual Studio com um mecanismo de jogos personalizado ou Unity.
4. Seu computador de desenvolvimento executando o Windows 10.

Desde que os itens acima forem VERDADEIRO, o restante deste artigo explica tudo que é necessário para obter um título configurado no Partner Center, um novo projeto criado e Xbox Live entrar no código gravado e testado.

> [!NOTE]
> Se você for parte do programa de criadores do Xbox Live, as suposições acima se aplicam a você e você deve acompanhar este artigo.

## <a name="partner-center-setup"></a>Instalação do Partner Center

Você precisa de um título do Xbox Live habilitado criado na [Partner Center](https://partner.microsoft.com/dashboard) como um pré-requisito para qualquer trabalho de funcionalidade do Xbox Live.

### <a name="create-a-microsoft-account"></a>Criar uma conta da Microsoft
Se você não tiver uma Account da Microsoft (também conhecido como uma MSA), você precisará primeiro criar um de cada [ https://go.microsoft.com/fwlink/p/?LinkID=254486 ](https://go.microsoft.com/fwlink/p/?LinkID=254486).  Se você tiver uma conta do Office 365, use Outlook.com ou tiver uma conta do Xbox Live - você provavelmente já terá uma MSA.

### <a name="register-as-an-app-developer"></a>Registre-se como desenvolvedor de aplicativos
Você precisará registrar-se como desenvolvedor de aplicativos antes de você tem permissão para criar um novo título no Partner Center.

Para registrar vá para https://developer.microsoft.com/en-us/store/register e siga o processo de inscrição.

### <a name="create-a-new-uwp-title"></a>Criar um novo título UWP
Em seguida, você precisa de um título UWP definido no Partner Center.  Você pode fazer isso primeiro indo até o painel

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

Em seguida, crie um novo título.  Você precisará reservar um nome.

![](../images/getting_started/first_xbltitle_newapp.png)

Em seguida, será levado para o *visão geral do aplicativo* página do seu aplicativo.  A página principal no qual você vai configurar Xbox Live está sob os serviços -> menu Xbox Live, mostrado abaixo.

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>Criar uma conta do Xbox Live
Você precisará de uma conta do Xbox Live para entrar no Xbox Live.  Se você já tiver um que você usa para entrar no seu console Xbox One, ou em App o Xbox no Windows 10, em seguida, você poderá usá-lo.

Caso contrário, você deve abrir o App Xbox no seu computador e entrar com sua Account da Microsoft.  Em seguida, ele será ativado para uso com o Xbox Live.

Você pode encontrar o App Xbox indo para o *Menu Iniciar* e digitando "Xbox" conforme mostrado abaixo.

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>Próximas etapas
Agora que você tem um novo título criado, agora você pode configurar um título do Xbox Live habilitado no seu mecanismo de jogos, o Visual Studio ou o ambiente de compilação de escolha.

Consulte [guia passo a passo a passo para integrar o Xbox Live](partners-step-by-step-guide.md)
