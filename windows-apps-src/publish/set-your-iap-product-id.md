---
author: jnHs
Description: "Quando você cria um novo IAP (produto no aplicativo) no painel do Centro de Desenvolvimento do Windows, precisa especificar um tipo de produto e atribuir uma ID de produto."
title: Defina seu tipo de produto IAP e ID de produto
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.sourcegitcommit: ae4727974af632a275c102a6328734597cee3e9b
ms.openlocfilehash: 9faee009cd907cd8ccdeded019e23713cbc058f0

---

# Defina seu tipo de produto IAP e ID de produto

Um IAP deve estar associado a um aplicativo que você já tenha criado no painel (mesmo se você ainda não o enviou). Você pode encontrar o botão **Criar um novo IAP** na página **Visão geral** ou **IAPs** de seu aplicativo.

Depois de clicar no botão, você verá a página **Criar um novo IAP**. Aqui, você precisará especificar um tipo de produto e atribuir a ele uma ID de produto.

## Tipo de produto

Primeiro, você precisará indicar qual tipo de IAP está oferecendo. Esta seleção se refere a como o cliente pode usar seu IAP.

> 
            **Observação** Não será possível alterar o tipo de produto depois de salvar essa página para criar o IAP. Se você escolheu o tipo de produto errado, pode excluir seu envio de IAP em andamento e começar pela criação de um novo IAP.

Selecione o tipo de produto que seja adequado para seu IAP:

- Consumível: um produto que pode ser comprado, usado (consumido) e depois comprado novamente. IAPs consumíveis geralmente são usados para coisas como moedas em jogos (ouro, moedas etc.) que podem ser adquiridas em valores definidos e, em seguida, usadas pelo cliente.
- Durável: um produto comprado e possuído pelo comprador por um período de tempo especificado. IAPs duráveis geralmente são usados para desbloquear funcionalidade adicional em um aplicativo. IAPs duráveis não são consumidos, mas você pode definir o **Ciclo de vida do produto** para que eles expirem após uma duração definida (com opções de 1 a 365 dias). O **Ciclo de vida do produto** padrão de um IAP durável é **Para sempre**, o que significa que o IAP nunca expira. Você pode alterar isso para uma duração diferente na etapa [Propriedades de IAP](enter-iap-properties.md) do processo de envio de IAP.

## ID do produto

Insira uma ID do produto exclusiva para seu IAP. Este é o mesmo identificador a que você precisa fazer referência no [código do seu aplicativo para chamar o IAP](https://msdn.microsoft.com/library/windows/apps/mt219684).

Aqui estão algumas coisas para se ter em mente ao escolher uma ID de produto:

-   Os clientes não verão essa ID de produto. (Mais tarde, você pode inserir um [título e descrição](create-iap-descriptions.md) a serem exibidos para os clientes).
-   Você não pode alterar ou excluir a ID de produto do IAP depois que ele tiver sido publicado.
-   Uma ID de produto não pode ter mais de 100 caracteres.
-   Um ID de produto não pode incluir nenhum dos seguintes caracteres: **&lt;&gt; \* % & : \\ ? + ,**
-   Para oferecer o IAP em todos os dispositivos, você deve usar apenas caracteres alfanuméricos, pontos e/ou sublinhados. Se você usar outros tipos de caracteres, o IAP não estará disponível para compra para clientes que executam o Windows Phone 8.1 ou anterior.
-   Uma ID de produto não precisa ser exclusiva dentro da Windows Store, mas ela deve ser exclusiva para sua conta de desenvolvedor.
 







<!--HONumber=Jun16_HO5-->


