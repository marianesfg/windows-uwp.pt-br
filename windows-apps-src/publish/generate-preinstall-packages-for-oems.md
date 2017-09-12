---
author: jnHs
Description: "Se sua conta de desenvolvedor tiver recebido as permissões adequadas, você poderá gerar e baixar pacotes de pré-instalação para que um OEM possa incluir o aplicativo na imagem do sistema operacional dele."
title: "Gerar pacotes de pré-instalação para OEMs"
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 076223a8407468dd34d8b24fec44288f3be67456
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2017
---
# <a name="generate-preinstall-packages-for-oems"></a>Gerar pacotes de pré-instalação para OEMs

Se sua conta de desenvolvedor tiver recebido as permissões adequadas, você poderá gerar e baixar pacotes de pré-instalação para que um OEM possa incluir o aplicativo na imagem do sistema operacional dele. Permissões de pré-instalação são habilitadas somente em contas de desenvolvedor patrocinadas pelos OEMs.


## <a name="important-preinstall-policy--limitations"></a>Política e limitações de pré-instalação importantes

Aplicativos de pré-instalação devem ser certificados por meio do Centro de Desenvolvimento do Windows para terem a licença mais recente da Loja, de forma a poderem se conectar à Loja e receber atualizações do aplicativo.

Qualquer aplicativo pré-instalado deve ser e permanecer gratuito em todos os mercados.


## <a name="generating-preinstall-packages"></a>Gerando pacotes de pré-instalação

Quando uma conta tiver sido habilitada com permissões de pré-instalação, conclua as seguintes etapas:

1.  Em seu painel, navegue até o aplicativo a ser pré-instalado.
2.  No menu de navegação esquerdo, expanda **Gerenciamento de aplicativo** e selecione **Pacotes atuais**.
3.  Na seção **Solicitar pacotes para pré-instalação do sistema operacional**, selecione **Habilitar pacotes baixáveis**.
4.  Na caixa de diálogo de confirmação, selecione **Habilitar**.
5.  Localize o pacote que você deseja baixar e selecione o link **Gerar pacote** apropriado.

    > [!NOTE]
    > O tempo de geração de pacotes de pré-instalação varia de acordo com o tamanho do pacote que você selecionou. Você pode deixar essa página e voltar mais tarde ou você pode deixar a página aberta enquanto o pacote é gerado.

6.  Depois que o pacote é gerado, um link para **Baixar pacote** aparecerá. Selecione esse link para baixar o arquivo .zip.

Em seguida, você pode fornecer o arquivo. zip para o OEM a fim de incluí-lo na imagem do sistema operacional.


## <a name="support"></a>Suporte

Se você ainda tiver dúvidas sobre a geração de pacotes de pré-instalação, envie um email para <partnerops@microsoft.com>.

 

 




