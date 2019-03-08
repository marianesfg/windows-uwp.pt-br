---
Description: Se sua conta de desenvolvedor tiver recebido as permissões adequadas, você poderá gerar e baixar pacotes de pré-instalação para que um OEM possa incluir o aplicativo na imagem do sistema operacional dele.
title: Gerar pacotes de pré-instalação para OEMs
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1ab17adc80a643c04ac7793945486c3ff975fde5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643131"
---
# <a name="generate-preinstall-packages-for-oems"></a>Gerar pacotes de pré-instalação para OEMs

Se sua conta de desenvolvedor tiver recebido as permissões adequadas, você poderá gerar e baixar pacotes de pré-instalação para que um OEM possa incluir o aplicativo na imagem do sistema operacional dele. Permissões de pré-instalação são habilitadas somente em contas de desenvolvedor patrocinadas pelos OEMs.


## <a name="important-preinstall-policy--limitations"></a>Política e limitações de pré-instalação importantes

Pré-instalação aplicativos deverão ser certificados pelo [Partner Center](https://partner.microsoft.com/dashboard) ter a licença Store mais recente para que eles são capazes de se conectar a Store e receber atualizações de aplicativo.

Qualquer aplicativo pré-instalado deve ser e permanecer gratuito em todos os mercados.


## <a name="generating-preinstall-packages"></a>Gerando pacotes de pré-instalação

Quando uma conta tiver sido habilitada com permissões de pré-instalação, conclua as seguintes etapas:

1.  No Centro de parceiros, navegue até o aplicativo que deve ser pré-instalado.
2.  No menu de navegação esquerdo, expanda **Gerenciamento de aplicativo** e selecione **Pacotes atuais**.
3.  Na seção **Solicitar pacotes para pré-instalação do sistema operacional**, selecione **Habilitar pacotes baixáveis**.
4.  Na caixa de diálogo de confirmação, selecione **Habilitar**.
5.  Localize o pacote que você deseja baixar e selecione o link **Gerar pacote** apropriado.

    > [!NOTE]
    > O tempo de geração de pacotes de pré-instalação varia de acordo com o tamanho do pacote que você selecionou. Você pode deixar essa página e voltar mais tarde ou você pode deixar a página aberta enquanto o pacote é gerado.

6.  Depois que o pacote é gerado, um link para **Baixar pacote** aparecerá. Selecione esse link para baixar o arquivo .zip.

Em seguida, você pode fornecer o arquivo. zip para o OEM a fim de incluí-lo na imagem do sistema operacional.


## <a name="support"></a>Suporte

Se você tiver mais dúvidas sobre a geração de pré-instalação de pacotes, envie um email <partnerops@microsoft.com>.

 

 




