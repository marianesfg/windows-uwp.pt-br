---
author: jnHs
Description: "Ao enviar um complemento, as opções da página Propriedades ajudam a determinar o comportamento de seu complemento quando oferecido aos clientes."
title: Inserir propriedades de complemento
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 186088f249c2e6fe116c970bd1969fcb59863ba6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="enter-add-on-properties"></a>Inserir propriedades de complemento


Ao enviar um complemento, as opções da página **Propriedades** ajudam a determinar o comportamento de seu complemento quando oferecido aos clientes.

## <a name="product-type"></a>Tipo de produto

O tipo de produto é selecionado quando você [cria o complemento primeiro](set-your-add-on-product-id.md). O tipo de produto que você selecionou é exibido aqui, mas não é possível alterá-lo.

> **Observação**  Se você não tiver publicado o complemento, poderá excluir o envio e começar novamente se precisar escolher um tipo de produto diferente. 

Dependendo do tipo de produto selecionado, você pode ver um dos seguintes campos:

### <a name="product-lifetime"></a>Tempo de vida do produto
Se você selecionou **Durável** para seu tipo de produto, o **Ciclo de vida do produto** será mostrado aqui. O **Ciclo de vida do produto** padrão de um complemento durável é **Para sempre**, o que significa que o complemento nunca expira. Se preferir, você pode definir o **Ciclo de vida do produto** para que o complemento expire após uma duração definida (com opções de 1 a 365 dias). 

### <a name="quantity"></a>Quantidade
Se você selecionou **Consumível gerenciado pela Loja** para seu tipo de produto, a **Quantidade** será mostrada aqui. Você precisará inserir um número entre 1 e 1000000. Essa quantidade será concedida para o cliente quando ele adquirir o complemento, e a Loja detectará o equilíbrio conforme o aplicativo relatar o consumo do cliente do complemento.

## <a name="content-type"></a>Tipo de conteúdo

Independentemente do tipo de produto do complemento, você também precisará indicar o tipo de conteúdo que está oferecendo. Para a maioria dos complementos, o tipo de conteúdo deve ser **Download de software eletrônico**. Se outra opção da lista parecer descrever melhor seu complemento (por exemplo, se você estiver oferecendo um download de música ou um livro eletrônico), selecione essa opção. 

Estas são as opções de tipo de conteúdo possíveis do complemento:

-   Download de software eletrônico
-   Livros eletrônicos
-   Edição única de revista eletrônica
-   Edição única de jornal eletrônico
-   Download de música
-   Streaming de músicas
-   Serviços/armazenamento de dados online
-   Download de vídeo
-   Streaming de vídeos
-   Software como um serviço

## <a name="keywords"></a>Palavras-chave

Você tem a opção de fornecer até dez as palavras-chave de até 30 caracteres para cada complemento que você enviar. Em seguida, seu aplicativo pode consultar complementos que correspondam a essas palavras. Esse recurso permite que você crie telas em seu aplicativo que podem carregar complementos sem precisar especificar a ID do produto diretamente no código do seu aplicativo. Em seguida, você pode alterar as palavras-chave do complemento a qualquer momento, sem precisar fazer alterações no código em seu aplicativo ou enviar o aplicativo novamente.

> **Observação**  Palavras-chave não estão disponíveis para uso em pacotes para o Windows 8 e o Windows 8.1.

## <a name="custom-developer-data"></a>Dados de desenvolvedor personalizados

Você pode digitar até 3000 caracteres no campo **Dados de desenvolvedor personalizados** (chamado antigamente de **Marcação**) para fornecer contexto extra para seu produto no aplicativo. Geralmente, isso é na forma de uma cadeia de caracteres XML, mas você pode inserir qualquer coisa que você gostaria nesse campo.

Para consultar este campo, use a propriedade [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) no [namespace Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (Ou, se você estiver usando o [namespace Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), use a propriedade [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx).)

Por exemplo, digamos que você tenha um jogo, e está vendendo uma bolsa de moedas de ouro como um complemento. Usando o campo **Dados de desenvolvedor personalizados**, o aplicativo pode consultar essa bolsa de ouro. Você pode ajustar o valor a qualquer momento (neste caso, o número de moedas em sua bolsa) atualizando as informações do campo **Dados de desenvolvedor personalizados** do complemento, sem precisar fazer alterações no código em seu aplicativo ou enviar o aplicativo novamente.

> **Observação**  O campo **Dados de desenvolvedor personalizados** não está disponível para uso em pacotes para o Windows 8 e o Windows 8.1.

 

 

 




