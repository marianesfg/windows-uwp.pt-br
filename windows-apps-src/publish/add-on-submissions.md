---
Description: Os complementos (ou produtos no aplicativo) são publicados por meio do Partner Center.
title: Envios de complemento
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, cra, compra realizada em aplicativo, produto no aplicativo, envio de cra
ms.localizationpriority: medium
ms.openlocfilehash: 87e5298f677a078eb1d6f89b72af3ea15d44b75f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259055"
---
# <a name="add-on-submissions"></a>Envios de complemento

Complementos (também às vezes conhecidos como produtos no aplicativo) são itens suplementares para seu aplicativo que podem ser comprados pelos clientes. Um complemento pode ser um novo recurso divertido, um novo nível de jogo ou qualquer outra coisa que você ache manterá os usuários envolvidos. Os complementos não são somente uma ótima forma de ganhar dinheiro, eles também ajudam a direcionar a interação e o compromisso do cliente.

Os complementos são publicados por meio do [Partner Center](https://partner.microsoft.com/dashboard)e exigem que você tenha uma [conta de desenvolvedor](https://developer.microsoft.com/store/register)ativa. Você também precisará [habilitar os complementos](../monetize/in-app-purchases-and-trials.md) no código de seu aplicativo.

A primeira etapa no processo de envio do complemento é criar o complemento no Partner Center [definindo seu tipo de produto e a ID do produto](set-your-add-on-product-id.md). Depois disso, você criará um envio para que seu complemento possa ser adquirido por meio do Microsoft Store. Você pode enviar um complemento ao mesmo tempo em que [envia seu aplicativo](app-submissions.md) ou pode trabalhar nele de forma independente. E você pode fazer [atualizações](#updating-an-add-on-after-publication) nos complementos depois que o aplicativo estiver na Loja, sem precisar reenviar o aplicativo.

> [!NOTE]
> Esta seção da documentação descreve como enviar Complementos no Partner Center. Opcionalmente, você poderá usar a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de complementos.


## <a name="checklist-for-submitting-an-add-on"></a>Lista de verificação para o envio de um complemento

Aqui está uma lista das informações que você fornece ao criar o envio do complemento. Os itens que você será solicitado a fornecer são indicados abaixo. Alguns são opcionais ou têm valores padrão que você possa alterar, conforme desejar.


### <a name="create-a-new-add-on-page"></a>Crie uma nova página de complemento

| Nome do campo                    | Observações                            |
|-------------------------------|----------------------------------|
| [**Tipo de produto**](set-your-add-on-product-id.md#product-type)      | Obrigatório |  
| [**ID do produto**](set-your-add-on-product-id.md#product-id)          | Obrigatório |        


### <a name="properties-page"></a>Página de propriedades

| Nome do campo                    | Observações                              |   
|-------------------------------|------------------------------------|
| [**Tempo de vida do produto**](enter-add-on-properties.md#product-lifetime)  | Obrigatório se o tipo de produto for **Durável**. Não aplicável a outros tipos de produto. |
| [**Quantidade**](enter-add-on-properties.md#quantity)  | Obrigatório se o tipo de produto for **Consumível gerenciado pela loja**. Não aplicável a outros tipos de produto. |
| [**Período de assinatura**](enter-add-on-properties.md#subscription-period)          | Obrigatório se o tipo de produto for **Assinatura**. Não aplicável a outros tipos de produto.       |  
| [**Avaliação gratuita**](enter-add-on-properties.md#free-trial)          | Obrigatório se o tipo de produto for **Assinatura**. Não aplicável a outros tipos de produto.       |
| [**Tipo de conteúdo**](enter-add-on-properties.md#content-type)          | Obrigatório    |               
| [**Palavras-chave**](enter-add-on-properties.md#keywords)                  | Opcional (até 10 palavras-chave, com limite de 30 caracteres cada) |
| [**Dados de desenvolvedor personalizados**](enter-add-on-properties.md#custom-developer-data)   | Opcional (limite de 3000 caracteres)            |


### <a name="pricing-and-availability-page"></a>Página de preços e disponibilidade

| Nome do campo                    | Observações                                       |
|-------------------------------|---------------------------------------------|
| [**Mercados**](set-add-on-pricing-and-availability.md#markets)  | Padrão: todos os mercados possíveis |
| [**Visualizar**](set-add-on-pricing-and-availability.md#visibility)   | Padrão: disponível para compra. Pode ser exibido nos detalhes do aplicativo |
| [**Agendamento**](set-add-on-pricing-and-availability.md#schedule)    | Padrão: Liberar o mais rápido possível
| [**Refere**](set-add-on-pricing-and-availability.md#pricing)                | Obrigatório                                    |
| [**Preços de venda**](put-apps-and-add-ons-on-sale.md)               | Opcional                    |


### <a name="store-listings"></a>Listagens da Store

Uma listagem de Store é necessária. É recomendável fornecer as listagens de Store para cada [idioma](create-add-on-store-listings.md#store-listing-languages) ao qual o seu aplicativo dá suporte.

| Nome do campo                    | Observações                                       |
|-------------------------------|---------------------------------------------|
| [**Título**](create-add-on-store-listings.md#title)                    | Obrigatório (limite de 100 caracteres)           |
| [**Ndescrição**](create-add-on-store-listings.md#description)       | Opcional (limite de 200 caracteres)            |
| [**Cone**](create-add-on-store-listings.md#icon)                    | Opcional (.png, 300 x 300 pixels)            |


Quando você terminar de inserir essas informações, clique em **Enviar para a loja**. Na maioria dos casos, o processo de certificação leva cerca de uma hora. Depois disso, seu complemento será publicado na Store e estará pronto para os clientes comprarem.

> [!NOTE]
> O complemento também deve ser implementado no código do aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Atualizando um complemento após a publicação

Você pode fazer alterações em um complemento publicado a qualquer momento. As alterações de complemento são enviadas e publicadas independentemente do seu aplicativo, de modo que geralmente não é necessário atualizar o aplicativo inteiro para fazer alterações em um complemento, como atualizar seu preço ou descrição.

Para enviar atualizações, vá para a página do complemento no Partner Center e clique em **Atualizar**. Isso criará um novo envio para o complemento, usando as informações do envio anterior como um ponto de partida. Faça as alterações desejadas e, em seguida, clique em **Enviar para a loja**.

Se quiser remover um complemento oferecido anteriormente, você poderá fazer isso criando um novo envio e alterando a opção [Distribuição e visibilidade](set-add-on-pricing-and-availability.md) para **Oculto na Microsoft Store** com a opção **Interromper aquisição**. Certifique-se de atualizar o código do aplicativo conforme necessário para também remover referências ao complemento (especialmente se o aplicativo publicado tiver suporte para Windows 8.1 anteriormente; essa configuração de visibilidade não se aplicará a esses clientes).

> [!IMPORTANT]
> Se o aplicativo publicado anteriormente estiver disponível para clientes no Windows 8. x, você precisará criar e publicar um novo envio de aplicativo para tornar as atualizações do complemento visíveis para esses clientes. Da mesma forma, se você adicionar novos complementos a um aplicativo destinado ao Windows 8.x depois que o aplicativo for publicado, deverá atualizar o código do seu aplicativo para fazer referência a esses complementos e, em seguida, reenviar o aplicativo. Caso contrário, os novos complementos não serão visíveis aos clientes no Windows 8.x.
