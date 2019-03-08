---
title: Áreas restritas do Xbox Live
description: Introdução às áreas restritas do Xbox Live
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590221"
---
# <a name="xbox-live-sandboxes-introduction"></a>Introdução às áreas restritas do Xbox Live

No [configuração do serviço Xbox Live](xbox-live-service-configuration-creators.md) artigo, foi explicado que você deve configurar as informações sobre seu título na [Partner Center](https://partner.microsoft.com/dashboard). Essas informações incluem coisas como as estatísticas, placares de líderes, localização e muito mais. Alterações à sua configuração de serviço Xbox Live precisam ser publicados no Partner Center em sua área restrita de desenvolvimento antes que as alterações são detectadas pelo restante do Xbox Live e podem ser acessadas em seu título.

Uma área restrita de desenvolvimento permite que você trabalhe nas alterações ao seu título em um ambiente isolado. As áreas restritas oferecem várias vantagens:

1. Você pode iterar sobre as alterações a uma atualização para o título sem afetar a versão que está na produção.
2. Por motivos de segurança, algumas ferramentas só funcionam em uma área restrita de desenvolvimento.
3. Outros editores não podem ver o que você está trabalhando sem obter acesso a sua área restrita.

Por padrão, consoles Xbox One e PCs com Windows 10 estão na área restrita de varejo. Você precisará mudar seu PC e/ou Xbox One para a área restrita de desenvolvimento para acessar essa versão da configuração do serviço Xbox Live. É importante lembrar-se de alterar o dispositivo de volta para a área restrita de varejo, se você precisa testar algo no varejo ou deseja fazer um intervalo para reproduzir seu Xbox Live favorito jogo.

## <a name="finding-out-about-your-sandbox"></a>Para obter informações sobre sua área restrita

Uma área restrita é criada para você quando você cria um título. Você pode localizar sua ID de Sandbox abrindo seu produto em **Partner Center** e navegando até **Services** > **Xbox Live**. O **ID do Sandbox** será listado na parte superior da página.

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>Alternar a área restrita para desenvolvimento do seu PC
Você pode alternar seu PC em área restrita para o desenvolvimento usando o Unity, Windows Device Portal (WPD) ou por meio da linha de comando.

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>Pré-requisitos
As seguintes condições devem ser feitas antes que você pode alternar para dentro e fora de área restrita para desenvolvimento no Unity:

1. [Configurar o Xbox Live no Unity](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>Alternar áreas restritas
Interno no permite a janela Xbox Live configuração alternar facilmente entre o desenvolvimento e áreas restritas de varejo. Para começar, acesse **Xbox Live** > **configuração** no menu. Você pode ver a área restrita atual na **configuração do modo de desenvolvedor** seção.

1. Se **modo de desenvolvedor** diz **habilitado**, em seguida, você está atualmente na área restrita de desenvolvimento associada com seu jogo. Você pode clicar na **retornar ao modo de varejo** botão para alternar.
2. Se **modo de desenvolvedor** diz **desabilitada**, em seguida, você está atualmente na área restrita de varejo. Você pode clicar na **alternar para o modo de desenvolvedor** botão para alternar no.

![XBL habilitado](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>Pré-requisitos
As seguintes condições devem ser feito antes de alternar sua área restrita no Windows Device Portal (WPD):

1. [Portal do programa de instalação de dispositivo na área de trabalho do Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>Alternar áreas restritas

1. Abra **Portal de desenvolvimento do Windows** conectando-se a ele no seu navegador da web, conforme descrito na [Portal do programa de instalação de dispositivo na área de trabalho do Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) artigo.
2. Clique em **Xbox Live**.
3. Insira sua área restrita para desenvolvimento no campo de texto e clique em **alterar**.

![](../images/getting_started/wdp_switch_sandbox.png)

Para voltar para varejo, você pode inserir varejo aqui.

### <a name="command-line"></a>Linha de comando

#### <a name="prerequisites"></a>Pré-requisitos
As seguintes condições devem ser feitas antes que você pode alternar dentro e fora da área restrita para o desenvolvimento por meio da linha de comando:

1. Baixar o pacote de ferramentas do Xbox Live em [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) e descompacte.

#### <a name="switch-sandboxes"></a>Alternar áreas restritas
1. Execute o arquivo em lotes de SwitchSandbox.cmd **modo de administrador**.

Executá-lo no modo administrador para alternar sua área restrita. O primeiro argumento é a área restrita. Por exemplo, se você está tentando alternar para a área restrita MJJSQH.58, você usaria este comando:

```cmd
SwitchSandbox.cmd MJJSQH.58
```

Para voltar para varejo, você pode simplesmente fornecer que como o segundo argumento.

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Alternar a área de restrita de desenvolvimento do console Xbox One

### <a name="using-xbox-dev-portal"></a>Usando o Portal de desenvolvimento do Xbox

Você pode usar o Portal de desenvolvimento do Xbox para alterar a área restrita no seu console. Para fazer isso, vá para [Home Dev](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) no seu console e [habilitar Portal do dispositivo](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox). Depois de abrir o Portal do Xbox de desenvolvimento:

2. Clique em **Xbox Live**.
3. Insira sua área restrita para desenvolvimento no campo de texto e chick **alterar**.

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>Usando a interface do usuário do console Xbox One

Você pode usar [página inicial do desenvolvimento](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) para alterar a área restrita no seu console diretamente:

1. Clique em **área restrita de alteração**, localizado em **ações rápidas**.
2. Insira a ID de área restrita e, em seguida, clique em **salvar e reinicie o**.

### <a name="sign-in-with-the-xbox-app"></a>Entrar com o aplicativo do Xbox

Depois de alternar a seu computador para usar a área de segurança apropriada para o título de desenvolvimento, você desejará verificar que você está conectado com o Xbox Live com uma conta de teste qualificadas. Isso pode ser feito depois de entrar na [aplicativo do Xbox Live](https://www.xbox.com/en-US/xbox-app). Depois que seu ambiente de desenvolvimento é iniciado usando a área de segurança desejada App o Xbox serão usuários de entrar usando as mesmas restrições como qualquer outro serviço Xbox Live de serviço em execução na área restrita. Isso será útil verificar se está usando uma conta válida para a área restrita.
