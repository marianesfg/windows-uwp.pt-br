---
author: jnHs
Description: "Os IAPs são publicados por meio do painel do Centro de Desenvolvimento do Windows."
title: Envios de IAP
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.sourcegitcommit: 97f4aee47cab9064ac053e7a6e16441d6960d41f
ms.openlocfilehash: 4a1764dfb8f94409aba973a28ba2999854179196

---

# Envios de IAP


IAP (produto no aplicativo) é um item suplementar do seu aplicativo que pode ser comprado pelos clientes. Um IAP pode ser um divertido e novo recurso suplementar, um novo nível de jogo ou qualquer coisa que você ache que manterá os usuários envolvidos. Os IAPs não são somente uma ótima forma de ganhar dinheiro, eles também ajudam a direcionar a interação e o compromisso do cliente.

Os IAPs são publicados por meio do painel do Centro de Desenvolvimento do Windows. Você também precisará [habilitar as IAPs](../monetize/enable-in-app-product-purchases.md) no código de seu aplicativo.

A primeira etapa no processo de envio do IAP é criar o IAP no painel, [definindo seu tipo e ID de produto](set-your-iap-product-id.md). Depois disso, você pode criar um envio para que seu IAP possa ser adquirido através da Windows Store. Você pode enviar um IAP ao mesmo tempo em que [envia seu aplicativo](app-submissions.md), ou pode trabalhar nele de forma independente. E você pode fazer [atualizações](#updating-an-iap-after-submission) nos IAPs depois que o aplicativo estiver na Loja, sem precisar reenviar o aplicativo.

## Lista de verificação para o envio de um IAP

Aqui está uma lista das informações que você fornece ao criar o envio da IAP. Os itens que você será solicitado a fornecer são indicados abaixo. Alguns são opcionais ou têm valores padrão que você possa alterar, conforme desejar.

### Criar uma nova página IAP
| Nome do campo                    | Observações                            | 
|-------------------------------|----------------------------------|
| [**Tipo de produto**](set-your-iap-product-id.md#product-type)      | Obrigatório. Se **Durável**, um **Ciclo de vida do produto** é obrigatório. |  
| [**ID do Produto**](set-your-iap-product-id.md#product-id)          | Obrigatório |        

### Página de propriedades
| Nome do campo                    | Observações                              |   
|-------------------------------|------------------------------------|
| [**Tempo de vida do produto**](enter-iap-properties.md#product-lifetime)  | Obrigatório se o tipo de produto for **Durável**. Não aplicável se o tipo de produto for **Consumível**. | 
| [**Tipo de conteúdo**](enter-iap-properties.md#content-type)          | Obrigatório       |               
| [**Palavras-chave**](enter-iap-properties.md#keywords)                  | Opcional (até 10 palavras-chave, com limite de 30 caracteres cada) | 
| [**Marca**](enter-iap-properties.md#tag)                               | Opcional (limite de 3000 caracteres)             | 

### Página de preços e disponibilidade 
| Nome do campo                    | Observações                                       | 
|-------------------------------|---------------------------------------------|
| [**Preço base**](set-iap-pricing-and-availability.md#base-price)                | Obrigatório                                    | 
| [**Mercados e precificação personalizada**](set-iap-pricing-and-availability.md#markets-and-custom-prices)  | Padrão: disponível em todos os mercados possíveis | 
| [**Preço de venda**](put-apps-and-iaps-on-sale.md)               | Opcional                             |
| [**Distribuição e visibilidade**](set-iap-pricing-and-availability.md#distribution-and-visibility)   | Padrão: o IAP pode ser encontrado pelos clientes que navegam ou pesquisam na loja | 
| [**Data de publicação**](set-iap-pricing-and-availability.md#publish-date)                | Padrão: publicar assim que o IAP for aprovado na certificação |

### Página de descrições
Uma descrição necessária. É recomendável fornecer descrições para cada [idioma](create-iap-descriptions.md#languages) ao qual o seu aplicativo dá suporte.

| Nome do campo                    | Observações                                       | 
|-------------------------------|---------------------------------------------|
| [**Título**](create-iap-descriptions.md#title)                    | Obrigatório (limite de 100 caracteres)              |
| [**Descrição**](create-iap-descriptions.md#description)       | Opcional (limite de 200 caracteres)              |
| [**Ícone**](create-iap-descriptions.md#icon)                    | Opcional (.png, 300 x 300 pixels)             | 

Quando você terminar de inserir essas informações, clique em **Enviar para a loja**. Na maioria dos casos, o processo de certificação leva cerca de uma hora. Depois disso, seu IAP será publicado na Loja e estará pronto para os clientes comprarem.

**Observação**  O IAP também deve ser implementado no código do seu aplicativo. Para obter mais informações, consulte [Habilitar compras de produto no aplicativo](../monetize/enable-in-app-product-purchases.md).


## Atualizando um IAP após a publicação

Você pode fazer alterações em um IAP publicado a qualquer momento. As alterações de IAP são enviadas e publicadas independentemente do seu aplicativo. Portanto, em geral, você não precisa atualizar o aplicativo inteiro para fazer alterações em um IAP, como atualizar o preço ou a descrição.

> **Importante**  Se o aplicativo estiver disponível para clientes no Windows 8.x, você deverá criar e publicar um novo envio de aplicativo para tornar as atualizações de IAP visíveis aos clientes. Da mesma forma, se você adicionar novos IAPs a um aplicativo destinado ao Windows 8.x depois que o aplicativo for publicado, deverá atualizar o código do seu aplicativo para fazer referência a esses IAPs e, em seguida, reenviar o aplicativo. Caso contrário, os novos IAPs não serão visíveis aos clientes no Windows 8.x.

Para enviar atualizações, acesse a página do IAP no seu painel e clique em **Atualizar**. Isso criará um novo envio para o IAP, usando as informações do seu envio anterior como um ponto de partida. Altere as informações desejadas e clique em **Enviar para a loja**.

Se quiser remover um IAP oferecido anteriormente, você poderá fazer isso criando um novo envio e alterando a opção [Distribuição e visibilidade](set-iap-pricing-and-availability.md) para **Não está mais disponível para compra. Não exibido na página de detalhes do seu aplicativo**. Atualize o código de seu aplicativo conforme necessário para também remover referências ao IAP.




<!--HONumber=Jun16_HO4-->


