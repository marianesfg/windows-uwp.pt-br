---
author: jnHs
Description: "Ao criar um novo complemento no painel do Centro de Desenvolvimento do Windows, é necessário especificar um tipo de produto e atribuir uma ID de produto (product ID)."
title: Defina seu tipo de produto e a ID do produto (product ID) do complemento
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: abd6c27367476e5f1da11cde14b7d7f08105ad3e
ms.lasthandoff: 02/07/2017

---

# <a name="set-your-add-on-product-type-and-product-id"></a>Defina seu tipo de produto e a ID do produto (product ID) do complemento

Um complemento deve estar associado a um aplicativo que você já tenha criado no painel (mesmo se você ainda não o enviou). Você pode encontrar o botão **Criar um novo complemento** na página **Visão geral** ou **Complementos** de seu aplicativo.

Depois de clicar no botão, você verá a página **Criar um novo complemento**. Aqui, você precisará especificar um tipo de produto e atribuir a ele uma ID do produto (product ID).

## <a name="product-type"></a>Tipo de produto

Primeiro, você precisará indicar qual tipo de complemento está oferecendo. Esta seleção se refere a como o cliente pode usar seu complemento.

> **Observação** Não será possível alterar o tipo de produto depois de salvar essa página para criar o complemento. Se você escolheu o tipo de produto errado, pode excluir seu envio de complemento em andamento e começar pela criação de um novo complemento.

Se o produto puder ser comprado, usado (consumido) e depois recomprado, você desejará selecionar um dos tipos de produto **consumíveis**. Complementos consumíveis geralmente são usados para coisas como moedas em jogos (ouro, moedas etc.) que podem ser adquiridas em valores definidos e, em seguida, usadas pelo cliente. Para obter mais informações sobre como incluir complementos consumíveis no aplicativo, consulte [Habilitar compras de complementos consumíveis](../monetize/enable-consumable-add-on-purchases.md).

Existem dois tipos de complementos consumíveis que podem ser selecionados:

- **Consumível gerenciado pelo desenvolvedor**: compatível em todas as versões de sistema operacional. O saldo e o cumprimento devem ser gerenciados dentro do aplicativo. 
- **Consumível gerenciado pela Loja:** O saldo será acompanhado pela Microsoft em todos os dispositivos do cliente nos quais o Windows 10, versão 1607, ou posterior esteja em execução; não compatível em versões anteriores do sistema operacional. Para usar essa opção, o produto pai deve ser compilado usando-se o SDK do Windows 10 versão 14393 ou posterior. Você não poderá enviar um complemento consumível gerenciado pela Loja para a Loja até o produto pai ter sido publicado (embora possa criar o envio no painel e começar a trabalhar nele a qualquer momento). Você precisará inserir a quantidade para o complemento consumível gerenciado pela Loja na página **Propriedades**.

Você deverá selecionar **Durável** se o produto puder ser comprado apenas uma vez. Complementos duráveis geralmente são usados para desbloquear funcionalidade adicional em um aplicativo. Complementos duráveis não são consumidos, mas você pode definir o **Ciclo de vida do produto** para que eles expirem após uma duração definida (com opções de 1 a 365 dias). O **Ciclo de vida do produto** padrão de um complemento durável é **Para sempre**, o que significa que o complemento nunca expira. Você pode alterar isso para uma duração diferente na etapa [Propriedades do complemento](enter-add-on-properties.md) do processo de envio de complemento.

## <a name="product-id"></a>ID do Produto (Product ID)

Insira uma ID do produto (product ID) exclusiva para seu complemento. Este é o mesmo identificador a que você precisa fazer referência no [código do seu aplicativo para chamar o complemento](https://msdn.microsoft.com/library/windows/apps/mt219684).

Aqui estão algumas coisas para se ter em mente ao escolher uma ID do produto (product ID):

-   Os clientes não verão essa ID de produto. (Mais tarde, você pode inserir um [título e descrição](create-add-on-descriptions.md) a serem exibidos para os clientes).
-   Você não pode alterar ou excluir a ID do produto (product ID) do complemento depois que ele tiver sido publicado.
-   Uma ID do produto (product ID) não pode ter mais de 100 caracteres.
-   Um ID do produto (product ID) não pode incluir nenhum dos seguintes caracteres: **&lt; &gt; \* % & : \\ ? + ,**
-   Para oferecer o complemento em todos os dispositivos, você deve usar apenas caracteres alfanuméricos, pontos e/ou sublinhados. Se você usar outros tipos de caracteres, o complemento não estará disponível para compra para clientes que executam o Windows Phone 8.1 ou anterior.
-   Uma ID de produto não precisa ser exclusiva dentro da Windows Store, mas ela deve ser exclusiva para sua conta de desenvolvedor.
 





