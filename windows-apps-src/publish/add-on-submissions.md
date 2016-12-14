---
author: jnHs
Description: "Os complementos são publicados por meio do painel do Centro de Desenvolvimento do Windows."
title: Envios de complemento
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
translationtype: Human Translation
ms.sourcegitcommit: 0f2b48f646423f299999a61d78dd956a78a09a8e
ms.openlocfilehash: 1e7c02621da528c4635ab47bd7c2a898f2441da1

---

# <a name="add-on-submissions"></a>Envios de complemento

Complementos (também às vezes conhecidos como produtos no aplicativo) são itens suplementares para seu aplicativo que podem ser comprados pelos clientes. Um complemento pode ser um novo recurso suplementar divertido, um novo nível de jogo ou qualquer coisa que você ache que manterá os usuários envolvidos. Os complementos não são somente uma ótima forma de ganhar dinheiro, eles também ajudam a direcionar a interação e o compromisso do cliente.

Os complementos são publicados por meio do painel do Centro de Desenvolvimento do Windows. Você também precisará [habilitar os complementos](../monetize/in-app-purchases-and-trials.md) no código de seu aplicativo.

A primeira etapa no processo de envio do complemento é criar o complemento no painel, [definindo seu tipo e ID de produto](set-your-add-on-product-id.md). Depois disso, você pode criar um envio para que seu complemento possa ser adquirido através da Windows Store. Você pode enviar um complemento ao mesmo tempo em que [envia seu aplicativo](app-submissions.md) ou pode trabalhar nele de forma independente. E você pode fazer [atualizações](#updating-an-add-on-after-submission) nos complementos depois que o aplicativo estiver na Loja, sem precisar reenviar o aplicativo.

> **Observação**&nbsp;&nbsp;Esta seção da documentação descreve como enviar complementos no painel do Centro de Desenvolvimento. Opcionalmente, você poderá usar a [API de envio da Windows Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de complementos.

## <a name="checklist-for-submitting-an-add-on"></a>Lista de verificação para o envio de um complemento

Aqui está uma lista das informações que você fornece ao criar o envio do complemento. Os itens que você será solicitado a fornecer são indicados abaixo. Alguns são opcionais ou têm valores padrão que você possa alterar, conforme desejar.

### <a name="create-a-new-add-on-page"></a>Crie uma nova página de complemento
| Nome do campo                    | Observações                            |
|-------------------------------|----------------------------------|
| [**Tipo de produto**](set-your-add-on-product-id.md#product-type)      | Obrigatório. Se **Durável**, um **Ciclo de vida do produto** é obrigatório. |  
| [**ID do Produto**](set-your-add-on-product-id.md#product-id)          | Obrigatório |        

<span/>

### <a name="properties-page"></a>Página de propriedades
| Nome do campo                    | Observações                              |   
|-------------------------------|------------------------------------|
| [**Tempo de vida do produto**](enter-add-on-properties.md#product-lifetime)  | Obrigatório se o tipo de produto for **Durável**. Não aplicável a outros tipos de produto. |
| [**Quantidade**](enter-add-on-properties.md#quantity)  | Obrigatório se o tipo de produto for **Consumível gerenciado pela loja**. Não aplicável a outros tipos de produto.
| [**Tipo de conteúdo**](enter-add-on-properties.md#content-type)          | Obrigatório       |               
| [**Palavras-chave**](enter-add-on-properties.md#keywords)                  | Opcional (até 10 palavras-chave, com limite de 30 caracteres cada) |
| [**Dados de desenvolvedor personalizados**](enter-add-on-properties.md#custom-developer-data)                               | Opcional (limite de 3000 caracteres)             |

<span/>

### <a name="pricing-and-availability-page"></a>Página de preços e disponibilidade
| Nome do campo                    | Observações                                       |
|-------------------------------|---------------------------------------------|
| [**Preço base**](set-add-on-pricing-and-availability.md#base-price)                | Obrigatório                                    |
| [**Mercados e precificação personalizada**](set-add-on-pricing-and-availability.md#markets-and-custom-prices)  | Padrão: disponível em todos os mercados possíveis |
| [**Preço de venda**](put-apps-and-add-ons-on-sale.md)               | Opcional                             |
| [**Distribuição e visibilidade**](set-add-on-pricing-and-availability.md#distribution-and-visibility)   | Padrão: o complemento pode ser encontrado pelos clientes que navegam ou pesquisam na loja |
| [**Data de publicação**](set-add-on-pricing-and-availability.md#publish-date)                | Padrão: publicar assim que o complemento for aprovado na certificação |

<span/>

### <a name="store-listings"></a>Listagens da Loja
Uma listagem de Loja é necessária. É recomendável fornecer as listagens de Loja para cada [idioma](create-add-on-store-listings.md#languages) ao qual o seu aplicativo dá suporte.

| Nome do campo                    | Observações                                       |
|-------------------------------|---------------------------------------------|
| [**Título**](create-add-on-store-listings.md#title)                    | Obrigatório (limite de 100 caracteres)              |
| [**Descrição**](create-add-on-store-listings.md#description)       | Opcional (limite de 200 caracteres)              |
| [**Ícone**](create-add-on-store-listings.md#icon)                    | Opcional (.png, 300 x 300 pixels)             |

<span/>

Quando você terminar de inserir essas informações, clique em **Enviar para a loja**. Na maioria dos casos, o processo de certificação leva cerca de uma hora. Depois disso, seu complemento será publicado na Loja e estará pronto para os clientes comprarem.

**Observação**  O complemento também deve ser implementado no código do seu aplicativo. Para obter mais informações, consulte [Habilitar compras de produto no aplicativo](../monetize/enable-in-app-product-purchases.md).


## <a name="updating-an-add-on-after-publication"></a>Atualizando um complemento após a publicação

Você pode fazer alterações em um complemento publicado a qualquer momento. As alterações de complemento são enviadas e publicadas independentemente do seu aplicativo. Portanto, em geral, você não precisa atualizar o aplicativo inteiro para fazer alterações em um complemento, como atualizar o preço ou a descrição.

> **Importante**  Se o aplicativo estiver disponível para clientes no Windows 8.x, você deverá criar e publicar um novo envio de aplicativo para tornar as atualizações de complemento visíveis aos clientes. Da mesma forma, se você adicionar novos complementos a um aplicativo destinado ao Windows 8.x depois que o aplicativo for publicado, deverá atualizar o código do seu aplicativo para fazer referência a esses complementos e, em seguida, reenviar o aplicativo. Caso contrário, os novos complementos não serão visíveis aos clientes no Windows 8.x.

Para enviar atualizações, acesse a página do complemento no seu painel e clique em **Atualizar**. Isso criará um novo envio para o complemento, usando as informações do seu envio anterior como um ponto de partida. Altere as informações desejadas e clique em **Enviar para a loja**.

Se quiser remover um complemento oferecido anteriormente, você poderá fazer isso criando um novo envio e alterando a opção [Distribuição e visibilidade](set-add-on-pricing-and-availability.md) para **Não está mais disponível para compra. Não exibido na página de detalhes do seu aplicativo**. Atualize o código de seu aplicativo conforme necessário para também remover referências ao complemento.



<!--HONumber=Dec16_HO1-->


