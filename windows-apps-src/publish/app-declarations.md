---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: Declarações de produto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 959e056d5edf5e1fe7a1c51a2f855c9e11512cb0
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4393287"
---
# <a name="product-declarations"></a>Declarações de produto

A seção de **declarações de produto** da página [Propriedades](enter-app-properties.md) do [processo de envio](app-submissions.md) de ajuda certificar-se de que seu aplicativo é exibido adequadamente e oferecido ao conjunto certo de clientes e ajudando-os a entender como eles podem usar seu aplicativo.

As seções a seguir descrevem algumas das declarações e o que você precisa considerar ao determinar se cada declaração se aplica ao seu aplicativo. Observe que dois dessas declarações são verificadas por padrão (conforme descrito abaixo.) Dependendo da categoria do produto, você também pode ver declarações adicionais. Certifique-se de revisar todas as declarações e garantir que eles refletem com precisão seu envio.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Este aplicativo permite que os usuários façam compras, mas não usa o sistema de comércio da Microsoft Store.

Para quase todas as envio, você deve deixar essa caixa desmarcada, desde aplicativos que oferecem oportunidades para comprar itens que são ou podem ser consumidos ou usados dentro de seu aplicativo devem usar a API de compra no aplicativo Microsoft Store para criar e enviar os complementos. Pelo [Contrato de desenvolvedor](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), aplicativos que foram criados e enviados antes de 29 de junho de 2015, podem continuar oferecendo a funcionalidade de compra no aplicativo sem usar o mecanismo de comércio da Microsoft, desde que a funcionalidade de compra esteja em conformidade com o [ Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Se isso se aplica ao seu aplicativo, você deve marcar esta caixa. Caso contrário, deixe-a desmarcada.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Este aplicativo foi testado para atender às diretrizes de acessibilidade.

Marcar essa caixa torna seu aplicativo detectável para os clientes que estão procurando especificamente por aplicativos acessíveis na Loja.

Você só deve marcar essa caixa se tiver feito todas as seguintes tarefas:

-   Definido todas as informações relevantes de acessibilidade para elementos de IU, como nomes acessíveis.
-   Implementado a navegação e as operações de teclado, levando em conta a ordem da guia, a ativação do teclado, a navegação com setas de direção, atalhos.
-   Garanta uma experiência visual acessível incluindo itens como uma relação de contraste de texto de 4,5:1 e não dependa apenas da cor para passar informações ao usuário.
-   Usado ferramentas de comprovação de acessibilidade, como Inspect ou AccChecker, para verificar o aplicativo e resolver todos os erros de alta prioridade detectados por essas ferramentas.
-   Verificado os cenários principais do aplicativo de ponta a ponta usando recursos e ferramentas como Narrador, Lupa, Em Teclado de Tela, Alto Contraste e Alto DPI.

Ao declarar seu aplicativo como acessível, você concorda que o aplicativo é acessível para todos os clientes, inclusive pessoas com necessidades especiais. Por exemplo, isso significa que você testou o aplicativo com modo de alto contraste e com um leitor de tela. Você também verificou que a interface de usuário funciona corretamente com um teclado, a Lupa e outras ferramentas de acessibilidade.

Para obter mais informações, consulte [acessibilidade](../design/accessibility/accessibility.md), [testes de acessibilidade](../design/accessibility/accessibility-testing.md)e a [acessibilidade na loja](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> Não lista seu aplicativo como acessível, a menos que você tenha especificamente a engenharia e para essa finalidade. Se seu aplicativo for declarado como acessível, mas não der suporte real à acessibilidade, você provavelmente receberá comentários negativos da comunidade.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Os clientes podem instalar esse aplicativo em unidades alternativas ou armazenamento removível.

Essa caixa é marcada por padrão, para permitir que os clientes instalem seu aplicativo para o armazenamento removível ou externo unidade de mídia, como um cartão SD, ou para um volume fora do sistema, como uma unidade externa. (Para o Windows Phone 8.1, isso foi anteriormente indicado por meio do Storemanifest.)

Se você deseja impedir que seu aplicativo seja instalado em unidades alternativas ou armazenamento removível e permitem apenas a instalação na unidade de disco rígido interno em seus dispositivos, desmarque essa caixa.

Observe que não há nenhuma opção para restringir a instalação para que um aplicativo *só* pode ser instalado em mídia de armazenamento removível.


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>O Windows pode incluir dados do aplicativo em backups automáticos no OneDrive.

Esta caixa é marcada por padrão, para permitir que os dados do seu aplicativo sejam incluídos quando um cliente optar por fazer com que o Windows faça backups automáticos no OneDrive. (Para o Windows Phone 8.1, isso foi anteriormente indicado por meio do Storemanifest.)

Se você deseja impedir que os dados do aplicativo sejam incluídos em backups automáticos, desmarque essa caixa.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Este aplicativo envia os dados do Kinect a serviços externos. 

Se seu aplicativo usa dados do Kinect e envia para qualquer serviço externo, você deve marcar essa caixa.



 

 

 




