---
author: jnHs
Description: "Ao enviar um complemento, as opções da página Propriedades ajudam a determinar o comportamento de seu complemento quando oferecido aos clientes."
title: Inserir propriedades de complemento
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 253e008d3622094dcfe765531d71e5f37b7777b0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2017
---
# <a name="enter-add-on-properties"></a>Inserir propriedades de complemento


Ao enviar um complemento, as opções da página **Propriedades** ajudam a determinar o comportamento de seu complemento quando oferecido aos clientes.

## <a name="product-type"></a>Tipo de produto

O tipo de produto é selecionado quando você [cria o complemento primeiro](set-your-add-on-product-id.md). O tipo de produto que você selecionou é exibido aqui, mas não é possível alterá-lo.

> [!TIP]
> Se você ainda não tiver publicado o complemento, é possível excluir o envio e começar novamente se desejar escolher um tipo de produto diferente.

Os campos disponíveis nesta página variam de acordo com o tipo de produto do complemento.

## <a name="product-lifetime"></a>Tempo de vida do produto


Se você selecionou **Durável** como o tipo de produto, o **Ciclo de vida do produto** será mostrado aqui. O **Ciclo de vida do produto** padrão de um complemento durável é **Para sempre**, o que significa que o complemento nunca expira. Se preferir, você pode definir o **Ciclo de vida do produto** para que o complemento expire após uma duração definida (com opções de 1 a 365 dias).

## <a name="quantity"></a>Quantidade


Se você selecionou **Consumível gerenciado pela Loja** como o tipo de produto, a **Quantidade** será mostrada aqui. Você precisará inserir um número entre 1 e 1000000. Essa quantidade será concedida para o cliente quando ele adquirir o complemento, e a Loja detectará o equilíbrio conforme o aplicativo relatar o consumo do cliente do complemento.


## <a name="subscription-period"></a>Período de assinatura

Se você selecionou **Assinatura** como o tipo de produto, o **Período de assinatura** será mostrado aqui. Você precisará escolher uma das opções disponíveis (**Mensal**, **Três meses**, **Seis meses**, **Anual** ou **24 meses**) para indicar a frequência de cobrança da assinatura de um cliente. Observe que depois que o complemento é publicado, você não pode alterar a seleção **Período de assinatura**.

> [!NOTE]
> No momento, a capacidade de criar complementos por assinatura está disponível somente para um conjunto de contas de desenvolvedor que participam de um programa de adoção antecipada. Disponibilizaremos complementos de assinatura para todas as contas de desenvolvedor no futuro, e a documentação preliminar será disponibilizada agora para fornecer aos desenvolvedores uma prévia desse recurso. Para obter mais informações, consulte [Habilitar complementos de assinatura para o aplicativo](../monetize/enable-subscription-add-ons-for-your-app.md).


## <a name="free-trial"></a>Avaliação gratuita

Para complementos de assinatura, a **Avaliação gratuita** também é mostrada aqui. Você deve optar por permitir que os clientes usem o complemento gratuitamente por um período específico (**1 semana** ou **1 mês**), ou se deseja oferecer **Sem avaliação gratuita**. Observe que depois que o complemento é publicado, você não pode alterar a seleção da **Avaliação gratuita**.


## <a name="content-type"></a>Tipo de conteúdo

Independentemente do tipo de produto do complemento, é necessário indicar o tipo de conteúdo oferecido. Para a maioria dos complementos, o tipo de conteúdo deve ser **Download de software eletrônico**. Se outra opção da lista descreve melhor seu complemento (por exemplo, se você estiver oferecendo um download de música ou um livro eletrônico), selecione essa opção.

Estas são as opções de tipo de conteúdo possíveis do complemento:

-   Download de software eletrônico
-   Livros eletrônicos
-   Edição única de revista eletrônica
-   Edição única de jornal eletrônico
-   Download de música
-   Streaming de músicas
-   Serviços/armazenamento de dados online
-   Software como um serviço
-   Download de vídeo
-   Streaming de vídeos


## <a name="additional-properties"></a>Propriedades adicionais

Esses campos são opcionais para todos os tipos de complementos.

<span id="keywords" />
### <a name="keywords"></a>Palavras-chave

Você tem a opção de fornecer até dez as palavras-chave de até 30 caracteres para cada complemento que você enviar. Em seguida, seu aplicativo pode consultar complementos que correspondam a essas palavras. Esse recurso permite que você crie telas em seu aplicativo que podem carregar complementos sem precisar especificar a ID do produto diretamente no código do seu aplicativo. Em seguida, você pode alterar as palavras-chave do complemento a qualquer momento, sem precisar fazer alterações no código em seu aplicativo ou enviar o aplicativo novamente.

Para consultar este campo, use a propriedade [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Keywords) no [namespace Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (ou, se você estiver usando o [namespace Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), use a propriedade [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting#Windows_ApplicationModel_Store_ProductListing_Keywords).)

> [!NOTE]
> As palavras-chave não estão disponíveis para uso em pacotes do Windows 8 e Windows 8.1.

<span id="custom-developer-data" />
### <a name="custom-developer-data"></a>Dados de desenvolvedor personalizados

Você pode digitar até 3000 caracteres no campo **Dados de desenvolvedor personalizados** (chamado antigamente de **Marcação**) para fornecer contexto extra para seu produto no aplicativo. Geralmente, isso é na forma de uma cadeia de caracteres XML, mas você pode inserir qualquer coisa que você gostaria nesse campo. O aplicativo pode consultar esse campo para ler o conteúdo (embora o aplicativo não possa editar os dados e passar as alterações novamente.)

Por exemplo, digamos que você tenha um jogo e esteja vendendo um complemento que permite ao cliente acessar mais níveis. Com o campo **Dados de desenvolvedor personalizados**, o aplicativo pode consultar quais níveis estão disponíveis quando um cliente adquire o complemento. Você pode ajustar o valor a qualquer momento (nesse caso, os níveis que estão incluídos), sem precisar fazer alterações no código do aplicativo ou enviá-lo novamente, ao atualizar as informações no campo **Dados de desenvolvedor personalizados** do complemento e, em seguida, publicar um envio atualizado do complemento.

Para consultar este campo, use a propriedade [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) no [namespace Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (Ou, se você estiver usando o [namespace Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), use a propriedade [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx).)

> [!NOTE]
> O campo **Dados de desenvolvedor personalizados** não está disponível para uso em pacotes do Windows 8 e Windows 8.1.

 

 

 
