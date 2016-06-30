---
author: jnHs
Description: "A página Propriedades do aplicativo do processo de envio de aplicativo permite definir a categoria do seu aplicativo e indicar as preferências de hardware ou outras declarações."
title: Insira as propriedades do aplicativo
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8861c13478adbe2010a164126c56f555375e0472

---

# Insira as propriedades do aplicativo

A página **Propriedades do aplicativo** do [processo de envio de aplicativo](app-submissions.md) permite definir a categoria do seu aplicativo e indicar as preferências de hardware ou outras declarações. Aqui, nós o guiaremos pelas opções desta página e o que você deve considerar ao inserir essas informações.

> **Observação**  Classificações etárias agora são uma página separada do processo de envio. Para saber mais, consulte [Classificações etárias](age-ratings.md).

## Categoria e subcategoria

Nesta seção, você indica a categoria (e subcategoria, se aplicável) que a Loja deve usar para categorizar seu aplicativo. Especificar uma categoria é necessário para enviar seu aplicativo.

Para saber mais, consulte [Tabela de categoria e subcategoria](category-and-subcategory-table.md).

## Preferências de hardware


Nesta seção, você tem a opção de indicar se determinados recursos de hardware são necessários para executar e interagir com seu aplicativo corretamente.

Nesse caso, a Windows Store tentará detectar se o dispositivo de um cliente dá suporte ao recurso de hardware selecionado. Se detectarmos que não, aparecerá um aviso ao cliente informando que está tentando baixar um aplicativo que declarou ter uma preferência por aquele hardware. Os clientes em dispositivos Windows 10 também verão os recursos selecionados listados na seção **Requisitos de hardware** dos detalhes da Loja do seu aplicativo.

Isso não impede que as pessoas baixem seu aplicativo em dispositivos que não tenham o hardware apropriado, mas elas não poderão classificar ou analisar o aplicativo nesses dispositivos.

> **Importante**  Com exceção da **Tela de toque**, esses avisos só são exibidos aos clientes em dispositivos Windows 10 que não tenham o(s) recurso(s) selecionado(s).

Além de fazer uma seleção aqui, recomendamos adicionar verificações de tempo de execução para o hardware especificado em seu aplicativo, uma vez que a Loja nem sempre pode detectar que o recurso selecionado está ausente no dispositivo do cliente e ele ainda poderá baixar seu aplicativo, mesmo se um aviso for exibido.

> **Dica**  Se quiser impedir que seu aplicativo UWP seja baixado em um dispositivo que não atende aos requisitos mínimos de memória ou nível do DirectX, você pode designar os requisitos mínimos em um arquivo XML StoreManifest. Para saber mais, consulte [Esquema StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).

## Declarações de aplicativo


Você pode marcar caixas nesta seção para indicar se qualquer uma das declarações se aplicam ao seu aplicativo. Isso pode afetar a forma em que seu aplicativo é exibido, se ele é oferecido para determinados clientes, ou como os clientes podem usá-lo.

Para saber mais, consulte [Declarações de aplicativo](app-declarations.md).



<!--HONumber=Jun16_HO4-->


