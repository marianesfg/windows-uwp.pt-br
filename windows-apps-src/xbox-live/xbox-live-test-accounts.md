---
title: Contas de teste do Xbox Live
description: Saiba como criar contas de teste para testes do Xbox Live habilitado jogo durante o desenvolvimento.
ms.assetid: e8076233-c93c-4961-86ac-27ec74917ebc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, conta de teste
ms.localizationpriority: medium
ms.openlocfilehash: 14313b6121cabf82762b5e3e862c73a9d3ec05cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594621"
---
# <a name="xbox-live-test-accounts"></a>Contas de teste do Xbox Live

Ao testar a funcionalidade em seu título durante o desenvolvimento, ele pode ser útil criar contas adicionais do Xbox Live.  Por exemplo, convém uma nova conta com nenhuma conquista.  Ou você talvez queira fazer várias contas e torná-los amigos uns dos outros para testar cenários de redes sociais.

Ele pode ser demorado para criar várias contas MSA (Microsoft), de modo que é fornecida uma maneira fácil de criar várias contas de teste ao mesmo tempo.

Contas de teste têm alguns outros benefícios também.  Eles podem entrar no seu *área restrita para desenvolvimento*, enquanto uma MSA regular não é possível devido a restrições de segurança.  Se você não souber o que um *área restrita para desenvolvimento* , em seguida, for, consulte [áreas de segurança do Xbox Live](xbox-live-sandboxes.md)

## <a name="types-of-test-accounts"></a>Tipos de contas de teste

Há duas opções de contas de teste.  MSAs regulares que são provisionadas para trabalhar em sua área restrita de desenvolvimento, ou as contas de teste que só funcionam em uma área restrita de desenvolvimento.

Se você estiver desenvolvendo um título com o programa de criadores, você só pode usar MSAs regulares que são provisionados para sua área restrita de desenvolvimento.

A seguir, vamos discutir como criar ambos os tipos.

## <a name="provisioning-regular-msas"></a>MSAs Regular de provisionamento

Se você tiver um Xbox Live conta já existente, um bom ponto de partida seria provisioná-la para uso com sua área restrita de desenvolvimento.

Se você não tiver uma conta do existente Xbox Live ou exigirem MSAs adicionais, você pode criar algumas [ https://account.microsoft.com/account ](https://account.microsoft.com/account).

## <a name="creating-test-accounts"></a>Criação de contas de teste

Se você for um ID@Xbox desenvolvedor, você também pode criar contas de teste exclusivamente para uso em suas áreas de segurança de desenvolvimento.  Você também pode criar várias contas de teste ao mesmo tempo.

Para ir para a página de gerenciamento de conta de teste no Partner Center.
1. Vá para [Partner Center](https://partner.microsoft.com/dashboard)
2. Clique no ícone de engrenagem na parte superior direita para ir para configurações de conta
3. Clique em "Contas de teste".

Veja a seguir uma captura de tela mostrando onde encontrá-lo

![](images/getting_started/devcenter_testaccount_nav.png)

Depois de clicar em "Contas de teste", você verá um resumo de todos os existentes contas de teste, se você tiver uma.  Você também tem a opção para criar novas contas de teste.

![](images/getting_started/devcenter_testaccount_summary.png)

Você pode clicar em "Nova conta de teste" e você verá um formulário que você pode usar para criar contas de teste.

![](images/getting_started/devcenter_testaccount_new.png)

As contas de teste que você cria serão prefixadas com o nome da sua área restrita para desenvolvimento e terão automaticamente acesso a sua área restrita de desenvolvimento.
