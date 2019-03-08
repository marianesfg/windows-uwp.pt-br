---
Description: Declarações de produto ajudar a garantir que seu aplicativo é exibido corretamente na Microsoft Store e oferecido para o conjunto certo de clientes.
title: Declarações de produto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1e17fbd81c84ca4ce72d36dbabf9991fe8c6d75d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611781"
---
# <a name="product-declarations"></a>Declarações de produto

O **declarações de produto** seção o [propriedades](enter-app-properties.md) página da [processo de envio](app-submissions.md) ajuda a assegurar que seu aplicativo é exibido adequadamente e oferecido para o conjunto certo de clientes e ajuda a compreender como eles podem usar seu aplicativo.

As seções a seguir descrevem algumas das declarações e o que você precisa considerar ao determinar se cada declaração se aplica ao seu aplicativo. Observe que duas dessas declarações são verificadas por padrão (conforme descrito abaixo.) Dependendo da categoria do produto, você também pode ver declarações adicionais. Certifique-se de examinar todas as declarações e garantir que reflitam seu envio.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Esse aplicativo permite que os usuários fazer compras, mas não usa o sistema de comércio do Microsoft Store.

Para quase todos envio, você deve deixar essa caixa desmarcada, desde aplicativos que oferecem oportunidades para comprar os itens que são ou podem ser consumidos ou usados dentro de seu aplicativo devem usar a API as compras no aplicativo Microsoft Store para criar e enviar os complementos. Pela [contrato de desenvolvedor do aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), aplicativos que foram criados e enviados antes de 29 de junho de 2015, foi possível continuar a oferecer a funcionalidade de compra no aplicativo sem usar o mecanismo de comércio da Microsoft, como a funcionalidade de compra, desde está em conformidade com o [Microsoft Store políticas](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Se isso se aplica ao seu aplicativo, você deve marcar esta caixa. Caso contrário, deixe-a desmarcada.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Este aplicativo foi testado para atender às diretrizes de acessibilidade.

Marcar essa caixa torna seu aplicativo detectável para os clientes que estão procurando especificamente por aplicativos acessíveis na Loja.

Você só deve marcar essa caixa se tiver feito todas as seguintes tarefas:

-   Definido todas as informações relevantes de acessibilidade para elementos de IU, como nomes acessíveis.
-   Implementado a navegação e as operações de teclado, levando em conta a ordem da guia, a ativação do teclado, a navegação com setas de direção, atalhos.
-   Garanta uma experiência visual acessível incluindo itens como uma relação de contraste de texto de 4,5:1 e não dependa apenas da cor para passar informações ao usuário.
-   Usado ferramentas de comprovação de acessibilidade, como Inspect ou AccChecker, para verificar o aplicativo e resolver todos os erros de alta prioridade detectados por essas ferramentas.
-   Verificado os cenários principais do aplicativo de ponta a ponta usando recursos e ferramentas como Narrador, Lupa, Em Teclado de Tela, Alto Contraste e Alto DPI.

Ao declarar seu aplicativo como acessível, você concorda que o aplicativo é acessível para todos os clientes, inclusive pessoas com necessidades especiais. Por exemplo, isso significa que você testou o aplicativo com modo de alto contraste e com um leitor de tela. Você também verificou que a interface de usuário funciona corretamente com um teclado, a Lupa e outras ferramentas de acessibilidade.

Para obter mais informações, consulte [Acessibilidade](../design/accessibility/accessibility.md), [Testes de acessibilidade](../design/accessibility/accessibility-testing.md) e [Acessibilidade na Loja](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> Não lista seu aplicativo como acessíveis, a menos que especificamente projetado e testado para essa finalidade. Se seu aplicativo for declarado como acessível, mas não der suporte real à acessibilidade, você provavelmente receberá comentários negativos da comunidade.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Os clientes podem instalar esse aplicativo em unidades alternativas ou armazenamento removível.

Essa caixa é marcada por padrão, para permitir que os clientes instalem o seu aplicativo para o armazenamento externo ou removível unidade de mídia como um cartão SD, ou para um volume não são do sistema, como uma unidade externa.

Se você quiser impedir que seu aplicativo está sendo instalado em unidades alternativas ou de armazenamento removível e permitir somente a instalação para o disco rígido interno em seu dispositivo, desmarque essa caixa. (Observe que não há nenhuma opção para restringir a instalação para que um aplicativo possa *apenas* ser instalado para a mídia de armazenamento removível.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>O Windows pode incluir dados do aplicativo em backups automáticos no OneDrive.

Esta caixa é marcada por padrão, para permitir que os dados do seu aplicativo sejam incluídos quando um cliente optar por fazer com que o Windows faça backups automáticos no OneDrive.

Se você deseja impedir que os dados do aplicativo sejam incluídos em backups automáticos, desmarque essa caixa.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Esse aplicativo envia dados do Kinect a serviços externos. 

Se seu aplicativo usa dados do Kinect e o envia a qualquer serviço externo, marque essa caixa.



 

 

 




