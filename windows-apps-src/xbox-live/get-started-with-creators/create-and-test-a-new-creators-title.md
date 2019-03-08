---
title: Criar e testar um novo título para criadores
description: Saiba como criar um novo título do programa de criadores do Xbox Live e publicar no ambiente de teste.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, para criadores e teste
ms.localizationpriority: medium
ms.openlocfilehash: 5b39a2ed67d2fe77fa8904408b81a329125d73c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594591"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>Criar um novo título do programa de criadores do Xbox Live e publicar no ambiente de teste

## <a name="introduction"></a>Introdução

Antes de escrever qualquer código do Xbox Live, você deve configurar um novo título no portal de configuração do serviço.  Você pode aprender mais sobre a configuração de serviço na [configuração do serviço Xbox Live](../xbox-live-service-configuration.md).

Este artigo irá percorrer todo o necessário para obter um título configurado no [Partner Center](https://partner.microsoft.com/dashboard), um novo projeto criado e preparando o Xbox Live para teste. Este artigo pressupõe o seguinte:

1. Você está usando o programa de criadores do Xbox Live.
2. Você está desenvolvendo um título de plataforma Universal do Windows (UWP).  Títulos UWP podem executar no Xbox One, PCs desktop do Windows 10 e mobile
3. Você está configurando seu título na [Partner Center](https://partner.microsoft.com/dashboard).
4. Seu computador de desenvolvimento executando o Windows 10.

> [!NOTE]
> Se você for parte do programa de criadores do Xbox Live, as suposições acima se aplicam a você e você deve acompanhar este artigo

## <a name="partner-center-setup"></a>Instalação do Partner Center

Você precisa de um título do Xbox Live habilitado criado na [Partner Center](https://partner.microsoft.com/dashboard) como um pré-requisito para usar qualquer funcionalidade do Xbox Live.

### <a name="create-a-microsoft-account"></a>Criar uma conta da Microsoft
Se você não tiver uma Account da Microsoft (também conhecido como uma MSA), você precisará primeiro criar um de cada [Account da Microsoft - Sign In](https://go.microsoft.com/fwlink/p/?LinkID=254486). Se você tiver uma conta do Office 365, use Outlook.com ou tiver uma conta do Xbox Live - você provavelmente já terá uma MSA.

### <a name="register-as-an-app-developer"></a>Registre-se como desenvolvedor de aplicativos
Você precisará registrar-se como desenvolvedor de aplicativos antes de criar um novo título no Partner Center.

Para registrar, vá para [Registre-se como desenvolvedor de aplicativos](https://developer.microsoft.com/store/register) e siga o processo de inscrição.

### <a name="create-a-new-uwp-title"></a>Criar um novo título UWP
Você precisará de um título UWP definido no Partner Center. Para isso, primeiro vamos [Partner Center](https://partner.microsoft.com/dashboard).

Em seguida, crie um novo título. Você precisará reservar um nome.

![](../images/getting_started/first_xbltitle_newapp.png)

A captura de tela mostra a página principal onde você vai Configurando Xbox Live, localizado sob a **Services** > **Xbox Live** menu.

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>Habilitar serviços do Xbox Live
Quando você clica o **Xbox Live** vincular sob **Services** pela primeira vez para um produto, você será levado à página habilitar Xbox Live programa de criadores.  Em seguida, clique o **habilitar** botão para abrir a caixa de diálogo Instalação do Xbox Live.

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

Na caixa de diálogo de instalação, selecione as plataformas que você deseja habilitar os serviços do Xbox Live para (Xbox One e computador com Windows 10 são selecionadas por padrão).  Clique o **confirmar** botão para habilitar o programa de criadores do Xbox Live para seu jogo.

> [!IMPORTANT]
> Somente há suporte para o Xbox Live para jogos. Para publicar com êxito seu jogo com o Xbox Live, você deve definir seu tipo de produto para "Game" na seção de propriedades do envio.

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>Testar a configuração do serviço Xbox Live em seu jogo
Quando você fizer alterações à configuração do Xbox Live para o seu jogo, você precisa publicar as alterações em um ambiente específico antes que eles são captados pelo restante do Xbox Live e podem ser vistos por seu jogo.

Quando você ainda está trabalhando em seu jogo, você publicar sua própria área restrita para desenvolvimento.  A área restrita de desenvolvimento permite que você trabalhe nas alterações para seu jogo em um ambiente isolado.

Quando o jogo é liberado para o público, a configuração do Xbox Live será publicada automaticamente para a área restrita de varejo.

Por padrão, os Consoles um Xbox e PCs com Windows 10 são na área restrita de varejo.

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>Publicar o Xbox Live configuração ao ambiente de teste

Sempre que você habilitar os serviços do Xbox Live e fazer alterações à configuração de serviço Xbox Live, para que as alterações entrem em vigor, você precisa publicar essas alterações em sua área restrita de desenvolvimento.

Na página de configuração do Xbox Live, clique no **teste** botão para publicar a configuração atual do Xbox Live em sua área restrita de desenvolvimento.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>Autorizar usuários para a área restrita de desenvolvimento e dispositivos

Apenas dispositivos autorizados e os usuários podem acessar a configuração para o jogo em sua área restrita para desenvolvimento de Xbox Live.

Por padrão, todos os consoles de desenvolvimento Xbox One que você adicionou à sua conta no Partner Center têm acesso à sua área restrita de desenvolvimento.  Para adicionar um console do Xbox One, vá para [consoles gerenciar Xbox One](https://partner.microsoft.com/xboxconfig/devices). Se você já estiver em sua conta no Partner Center, você pode ir para **configurações de conta** > **configurações de conta** > **Dev dispositivos**  >  **Consoles do Xbox One desenvolvimento**.

Você também pode autorizar contas normais do Xbox Live para ter acesso à sua área restrita de desenvolvimento.  Para autorizar o acesso de contas Xbox Live para sua área restrita para desenvolvimento, vá para [gerenciar contas](https://developer.microsoft.com/xboxtestaccounts/configurecreators).

## <a name="next-steps"></a>Próximas etapas
Agora que você tem um novo título criado, agora você pode configurar um título do Xbox Live habilitado no seu mecanismo de jogos, o Visual Studio ou o ambiente de compilação de escolha.

Consulte [guia passo a passo a passo para integrar o Xbox Live](creators-step-by-step-guide.md)
