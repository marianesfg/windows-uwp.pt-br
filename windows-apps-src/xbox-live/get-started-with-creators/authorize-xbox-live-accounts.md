---
title: Autorizar contas do Xbox Live no seu ambiente de teste
description: Aprenda a autorizar uma conta do Xbox Live pública para uso em testes no ambiente de desenvolvimento.
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, contas, contas de teste
ms.localizationpriority: medium
ms.openlocfilehash: 662f85f985baf58eef050060f8132f5a4b92444d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663851"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>Autorizar contas do Xbox Live para testes em seu ambiente

Este tópico percorrer o processo de configuração de uma conta do Xbox Live com o seu ambiente de teste de editores

## <a name="prerequisites"></a>Pré-requisitos

Você precisará do seguinte para autorizar uma conta de teste do Xbox Live:

* Um [conta do Xbox Live](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>Navegue até a página conta de teste do Xbox

Isso está localizado na seção configurações da conta do Partner Center

Você pode chegar a essa seção de uma das duas maneiras

1. Na [Partner Center](https://partner.microsoft.com/dashboard/windows/overview), clique o ⚙️ de engrenagem de configurações no canto superior direito do painel e selecione **configurações do desenvolvedor** na lista suspensa. Isso abrirá um menu de navegação no lado esquerdo da tela onde você irá selecionar a lista suspensa Xbox Live e, em seguida, o **contas de teste do Xbox** link.
2. Na página de configuração do Xbox Live criadores localize a seção de teste e clique no link intitulado **autorizar Xbox Live contas para seu ambiente de teste**

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>Autorizar uma conta do Xbox Live para seu ambiente de teste

* Uma vez dentro da página de contas de teste do Xbox, você deve ver uma lista de todas as contas autorizadas no momento. Para autorizar uma nova conta, clique no botão Adicionar conta

![Adicionando contas do Xbox Live](../images/creators_udc/add_test_account.png)

* Uma modal deve ser exibida na tela com uma caixa de texto onde é possível digitar o endereço de email da conta desejada

![Adicionando o Xbox Live contas Modal](../images/creators_udc/add_test_account_modal.png)

* Clique no botão Adicionar para validar que o endereço de email existe e tem uma conta do associada Xbox Live. Se as verificações forem aprovadas modal desaparecerá e você verá a nova conta na tabela que indica a ele agora está autorizada com êxito para o seu ambiente de teste

![Adicionando sucesso de contas ao vivo do Xbox](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>Solução de problemas

O e-mail inserido no modal é executado por meio de algumas verificações, incluindo uma pesquisa para garantir que não haja é uma conta do Xbox Live associada a ele. Se qualquer uma dessas verificações falhar, a conta não é adicionada à tabela e, portanto, não autorizada e você pode receber um erro "Desculpe, houve um problema ao adicionar seu endereço de email".

É uma verificação de boa se você estiver tendo problemas tentar entrar com a conta [Xbox.com](https://www.xbox.com/live/). Se você não pode entrar a conta não é uma conta do Xbox Live.
