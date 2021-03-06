---
Description: Ao criar um novo complemento no Partner Center, você precisa especificar um tipo de produto e atribuir a ele uma ID de produto.
title: Definir seu tipo de produto e ID do produto (product ID) do complemento
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complementos, cra, durável, consumível, assinatura, tipo de produto, id do produto, compra realizada em aplicativo, produto no aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: a6ef1ca71ffcd7b2d445292bfb38a6a8d29e7a74
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210502"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Definir seu tipo de produto e ID do produto (product ID) do complemento

Um complemento deve ser associado a um aplicativo que você criou no [Partner Center](https://partner.microsoft.com/dashboard) (mesmo que ainda não tenha enviado). Você pode encontrar o botão **Criar um novo complemento** na página **Visão geral** ou em **Complementos** do aplicativo.

Depois de selecionar **Criar um novo complemento**, você será solicitado a especificar um tipo de produto e atribuir uma ID de produto ao complemento.

## <a name="product-type"></a>Tipo de produto

Primeiro, você precisará indicar qual tipo de complemento está oferecendo. Esta seleção se refere a como o cliente pode usar seu complemento.

> [!NOTE]
> Não será possível alterar o tipo de produto depois de salvar essa página para criar o complemento. Se você escolheu o tipo de produto errado, é possível excluir o envio de complemento em andamento e começar a criação de um novo complemento.

<span id="durable" />

### <a name="durable"></a>Durável

Selecione **Durável** como o tipo de produto se o complemento for comprado apenas uma vez em geral. Esses complementos duráveis geralmente são usados para desbloquear a funcionalidade adicional em um aplicativo.

O **Ciclo de vida do produto** padrão de um complemento durável é **Para sempre**, o que significa que o complemento nunca expira. Você tem a opção de definir o **Tempo de vida do produto** como uma duração diferente na etapa [Propriedades](enter-add-on-properties.md) do processo de envio de complemento. Se você fizer isso, o complemento expira após o período especificado (com opções de 1 a 365 dias), caso no qual um cliente pode comprá-lo novamente após a expiração.

### <a name="consumable"></a>Consumível

Se o complemento puder ser comprado, usado (consumido) e depois recomprado, você desejará selecionar um dos tipos de produto **consumíveis**. Complementos consumíveis geralmente são usados para coisas como moedas em jogos (ouro, moedas etc.) que podem ser adquiridas em valores definidos e, em seguida, usadas pelo cliente. Para obter mais informações, consulte [Habilitar compras de complementos consumíveis](../monetize/enable-consumable-add-on-purchases.md).

Há dois tipos de complementos para consumo:
- **Consumível gerenciado pelo desenvolvedor**: o saldo e o cumprimento devem ser gerenciados no aplicativo. Compatível com todas as versões do sistema operacional.
- **Consumível gerenciado pela Loja:** O saldo será acompanhado pela Microsoft em todos os dispositivos do cliente nos quais o Windows 10, versão 1607, ou posterior esteja em execução; não compatível em versões anteriores do sistema operacional. Para usar essa opção, o produto pai deve ser compilado usando-se o SDK do Windows 10 versão 14393 ou posterior. Observe também que você não pode enviar um complemento de consumo gerenciado por loja para a loja até que o produto pai tenha sido publicado (embora você possa criar o envio no Partner Center e começar a trabalhar nele a qualquer momento). Você deverá inserir a quantidade para o complemento consumível gerenciado pela Loja na etapa **Propriedades** do envio.

### <a name="subscription"></a>Assinatura

Se você deseja cobrar os clientes pelo complemento com frequência, escolha **Assinatura**.

Depois que um complemento de assinatura inicialmente é adquirido por um cliente, eles continuarão a ser cobrados em intervalos recorrentes para continuar usando o complemento. O cliente pode cancelar a assinatura a qualquer momento para evitar cobranças futuras. É necessário especificar o período de assinatura e se deseja ou não oferecer uma avaliação gratuita na etapa **Propriedades** do envio.

Há suporte para complementos de assinatura somente para os clientes que executam o Windows 10, versão 1607 ou posterior. O aplicativo pai deve ser compilado utilizando o Windows 10 SDK, versão 14393 ou posterior, e deve usar a API de compras no aplicativo no namespace **Windows.Services.Store**, em vez do namespace **ApplicationModel**. Para obter mais informações, consulte [Habilitar complementos de assinatura para o aplicativo](../monetize/enable-subscription-add-ons-for-your-app.md).

Você deve enviar o produto pai antes de publicar os complementos da assinatura no repositório (embora possa criar o envio no Partner Center e começar a trabalhar nele a qualquer momento).

## <a name="product-id"></a>ID do Produto

Independentemente do tipo de produto escolhido, é necessário inserir um ID de produto exclusiva para o complemento. Esse nome será usado para identificar o complemento no Partner Center e você poderá usar esse identificador para [fazer referência ao complemento em seu código](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

Aqui estão algumas coisas para se ter em mente ao escolher uma ID do produto (product ID):

-   Uma ID de produto deve ser exclusiva dentro do produto pai.
-   Você não pode alterar ou excluir a ID do produto (product ID) do complemento depois que ele tiver sido publicado.
-   Uma ID do produto (product ID) não pode ter mais de 100 caracteres.
-   Uma ID de produto não pode incluir nenhum dos seguintes caracteres: **&lt; &gt; \*% &: \\? +,**
-   Os clientes não verão a ID do produto. (Mais tarde, você pode inserir um [título e descrição](create-add-on-descriptions.md) a serem exibidos para os clientes).
-   Se o aplicativo publicado anteriormente der suporte a Windows Phone 8,1 ou anterior, você deverá usar apenas caracteres alfanuméricos, pontos e/ou sublinhados em sua ID de produto. Se você usar outros tipos de caracteres, o complemento não estará disponível para compra para clientes que executam o Windows Phone 8.1 ou anterior.

 
