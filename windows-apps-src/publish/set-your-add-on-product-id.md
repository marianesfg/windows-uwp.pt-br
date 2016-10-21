---
author: jnHs
Description: "Quando você cria um novo complemento no painel do Centro de Desenvolvimento do Windows, precisa especificar um tipo de produto e atribuir uma ID do produto (product ID)."
title: Definir seu tipo de produto e ID do produto (product ID) do complemento
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
translationtype: Human Translation
ms.sourcegitcommit: e59324aca65cf8baacb085da22a20d952fdb8c9a
ms.openlocfilehash: 2a469506c8b440e1aa8555ac57b88f2026ae4d8e

---

# Definir seu tipo de produto e ID do produto (product ID) do complemento

Um complemento deve estar associado a um aplicativo que você já tenha criado no painel (mesmo se você ainda não o enviou). Você pode encontrar o botão **Criar um novo complemento** na página **Visão geral** ou **Complementos** de seu aplicativo.

Depois de clicar no botão, você verá a página **Criar um novo complemento**. Aqui, você precisará especificar um tipo de produto e atribuir a ele uma ID do produto (product ID).

## Tipo de produto

Primeiro, você precisará indicar qual tipo de complemento está oferecendo. Esta seleção se refere a como o cliente pode usar seu complemento.

> **Observação** Não será possível alterar o tipo de produto depois de salvar essa página para criar o complemento. Se você escolheu o tipo de produto errado, pode excluir seu envio de complemento em andamento e começar pela criação de um novo complemento.

Se o produto puder ser comprado, usado (consumido) e depois recomprado, você desejará selecionar um dos tipos de produto **consumíveis**. Complementos consumíveis geralmente são usados para coisas como moedas em jogos (ouro, moedas etc.) que podem ser adquiridas em valores definidos e, em seguida, usadas pelo cliente. Para obter mais informações sobre como incluir complementos consumíveis no aplicativo, consulte [Habilitar compras de complementos consumíveis](../monetize/enable-consumable-add-on-purchases.md).

Existem dois tipos de complementos consumíveis que podem ser selecionados:

- **Consumível gerenciado pelo desenvolvedor**: compatível em todas as versões de sistema operacional. O saldo e o cumprimento devem ser gerenciados dentro do aplicativo. 
- **Consumível gerenciado pela Loja:** O saldo será acompanhado pela Microsoft em todos os dispositivos do cliente nos quais o Windows 10, versão 1607, ou posterior esteja em execução; não compatível em versões anteriores do sistema operacional. Para usar essa opção, o produto pai deve ser compilado usando-se o SDK do Windows 10 versão 14393 ou posterior. Você não poderá enviar um complemento consumível gerenciado pela Loja para a Loja até o produto pai ter sido publicado (embora possa criar o envio no painel e começar a trabalhar nele a qualquer momento). Você precisará inserir a quantidade para o complemento consumível gerenciado pela Loja na página **Propriedades**.

Você deverá selecionar **Durável** se o produto puder ser comprado apenas uma vez. Complementos duráveis geralmente são usados para desbloquear funcionalidade adicional em um aplicativo. Complementos duráveis não são consumidos, mas você pode definir o **Ciclo de vida do produto** para que eles expirem após uma duração definida (com opções de 1 a 365 dias). O **Ciclo de vida do produto** padrão de um complemento durável é **Para sempre**, o que significa que o complemento nunca expira. Você pode alterar isso para uma duração diferente na etapa [Propriedades do complemento](enter-add-on-properties.md) do processo de envio de complemento.

## ID do Produto (Product ID)

Insira uma ID do produto (product ID) exclusiva para seu complemento. Este é o mesmo identificador a que você precisa fazer referência no [código do seu aplicativo para chamar o complemento](https://msdn.microsoft.com/library/windows/apps/mt219684).

Aqui estão algumas coisas para se ter em mente ao escolher uma ID do produto (product ID):

-   Os clientes não verão essa ID de produto. (Mais tarde, você pode inserir um [título e descrição](create-add-on-descriptions.md) a serem exibidos para os clientes).
-   Você não pode alterar ou excluir a ID do produto (product ID) do complemento depois que ele tiver sido publicado.
-   Uma ID do produto (product ID) não pode ter mais de 100 caracteres.
-   Um ID do produto (product ID) não pode incluir nenhum dos seguintes caracteres: **&lt; &gt; \* % & : \\ ? + ,**
-   Para oferecer o complemento em todos os dispositivos, você deve usar apenas caracteres alfanuméricos, pontos e/ou sublinhados. Se você usar outros tipos de caracteres, o complemento não estará disponível para compra para clientes que executam o Windows Phone 8.1 ou anterior.
-   Uma ID de produto não precisa ser exclusiva dentro da Windows Store, mas ela deve ser exclusiva para sua conta de desenvolvedor.
 







<!--HONumber=Aug16_HO5-->


