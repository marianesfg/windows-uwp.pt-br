---
title: Testar o jogo em Unity no Visual Studio
description: Lista de verificação para um teste bem-sucedido do Unity se baseia no Visual Studio.
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589871"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>Testar sua compilação de jogo do Unity no Visual Studio

Para testar a funcionalidade do Xbox Live do seu jogo do Unity com dados reais, você deve criar o jogo conforme descrito na **compilar e testar o projeto** seção o [configurar Xbox Live no artigo do Unity](configure-xbox-live-in-unity.md). O artigo a seguir lhe fornecerá uma lista de verificação de itens para ajudar a garantir que os testes bem-sucedidos de jogo do Unity no Visual Studio.

1. **Verifique se você tiver um título configurado adequadamente associado com seu jogo do Unity.**
    Se você habilitou o Xbox Live no Assistente de associação do Xbox Live, você desejará se familiarizar com [Partner Center](https://partner.microsoft.com/dashboard). Partner Center permite que você defina as configurações do Xbox Live para seu título e deve ser configurado corretamente na ordem do título para se comunicar com o Xbox Live. O artigo [criar um novo título do programa de criadores do Xbox Live e publicar no ambiente de teste](create-and-test-a-new-creators-title.md) leva você através do processo de instalação do Partner Center. Se você já configurou seu jogo por meio de **Assistente de configuração do Xbox** no Unity, você pode pular para a seção [teste Xbox Live a configuração de serviço no seu jogo](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game). Enquanto no Partner Center, certifique-se de verificar que as informações em sua configuração do Xbox Live para o seu jogo do Unity correspondem à configuração do Partner Center para o seu jogo.

2. **Certifique-se de que seu título tem um Account(with gamertag) Microsoft autorizados que podem entrar no seu título.**
    Sem uma conta autorizada não será possível completa entrar ao testar seu título, nem será possível usar outros recursos do Xbox Live. Para verificar se você tiver um Account da Microsoft e o nome de jogador autorizados leia [autorizar Xbox Live contas para testes em seu ambiente](authorize-xbox-live-accounts.md).

3. **Certifique-se de que o título foi publicado para teste.**
    Quando você faz alterações em seu título essas alterações deve ser publicado para sua caixa de areia antes que possam ser usados. Verifique se você enviar por push o **teste** botão publicar suas alterações à sua configuração.

    ![Publicar para a imagem de teste](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    O **teste** botão é encontrado no [Partner Center](https://partner.microsoft.com/dashboard) configurações de serviço em Xbox Live seu título. Se você tiver feito alterações à sua configuração, como adicionar uma nova conta de testes autorizado, você vai querer enviar por push o **testar** botão Publicar novas definições de configuração para o serviço Xbox Live.

4. **Certifique-se de que seu computador está no modo de desenvolvedor e você estiver usando a apropriado Xbox Live área restrita.**
    Quando seu título for publicado para testá-lo é publicado em um ambiente específico, chamado de uma área restrita. Se seu computador de desenvolvimento não está configurado para usar essa área restrita não será capaz de testar os recursos do Xbox Live. Saiba como verificar e alterar a área restrita do seu PC com o [Xbox Live introdução de áreas restritas](xbox-live-sandboxes-creators.md).

5. **Certifique-se de que você compila seu projeto como um x64 direcionando o computador local para criar em seu computador de build**

    ![configurações de build](../images/unity/get-started-with-creators/vsBuildSettings.JPG)