---
Description: As declarações de produto ajudam a garantir que seu aplicativo seja exibido adequadamente no Microsoft Store e oferecido ao conjunto certo de clientes.
title: Declarações de produto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47011a22353f26361a392690d857bde1fc180c03
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79211002"
---
# <a name="product-declarations"></a>Declarações de produto

A seção **declarações de produto** da página [Propriedades](enter-app-properties.md) do processo de [envio](app-submissions.md) ajuda a garantir que seu aplicativo seja exibido adequadamente e oferecido ao conjunto correto de clientes e os ajuda a entender como eles podem usar seu aplicativo.

As seções a seguir descrevem algumas das declarações e o que você precisa considerar ao determinar se cada declaração se aplica ao seu aplicativo. Observe que duas dessas declarações são verificadas por padrão (conforme descrito abaixo). Dependendo da categoria do produto, você também poderá ver declarações adicionais. Certifique-se de examinar todas as declarações e garantir que elas reflitam com precisão seu envio.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Esse aplicativo permite que os usuários façam compras, mas não usa o sistema de comércio Microsoft Store.

Para quase todos os envios, você deve deixar essa caixa desmarcada, já que os aplicativos que oferecem oportunidades para comprar itens que são ou podem ser consumidos ou usados em seu aplicativo devem usar a API de compra do Microsoft Store no aplicativo para criar e enviar os complementos. De acordo com o [contrato de desenvolvedor do aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), os aplicativos que foram criados e enviados antes de 29 de junho de 2015 podem continuar a oferecer a funcionalidade de compra no aplicativo sem usar o mecanismo de comércio da Microsoft, desde que a funcionalidade de compra esteja em conformidade com as políticas de [Microsoft Store](store-policies.md#108-financial-transactions). Se isso se aplica ao seu aplicativo, você deve marcar esta caixa. Caso contrário, deixe-a desmarcada.

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
> não liste seu aplicativo como acessível, a menos que você tenha desenvolvido e testado especificamente para essa finalidade. Se seu aplicativo for declarado como acessível, mas não der suporte real à acessibilidade, você provavelmente receberá comentários negativos da comunidade.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Os clientes podem instalar esse aplicativo em unidades alternativas ou armazenamento removível.

Essa caixa é marcada por padrão, para permitir que os clientes instalem seu aplicativo em mídia de armazenamento externa ou removível, como um cartão SD, ou em uma unidade de volume que não seja do sistema, como uma unidade externa.

Se você quiser impedir que seu aplicativo seja instalado em unidades alternativas ou armazenamento removível e permitir a instalação somente no disco rígido interno em seu dispositivo, desmarque essa caixa. (Observe que não há nenhuma opção para restringir a instalação para que um aplicativo *só* possa ser instalado na mídia de armazenamento removível.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>O Windows pode incluir dados do aplicativo em backups automáticos no OneDrive.

Esta caixa é marcada por padrão, para permitir que os dados do seu aplicativo sejam incluídos quando um cliente optar por fazer com que o Windows faça backups automáticos no OneDrive.

Se você deseja impedir que os dados do aplicativo sejam incluídos em backups automáticos, desmarque essa caixa.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Esse aplicativo envia dados do Kinect a serviços externos. 

Se seu aplicativo usa dados do Kinect e o envia a qualquer serviço externo, marque essa caixa.



 

 

 




