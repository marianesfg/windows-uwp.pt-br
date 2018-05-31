---
author: jnHs
Description: The Store listings section of the app submission process is where you provide the text and images that customers will see when viewing your app's listing in the Microsoft Store.
title: Criar listagens da Store do app
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, listagem, descrição, página da store, notas de versão, título
ms.localizationpriority: high
ms.openlocfilehash: 871eb3cd8b8bdfd0cf12859dcb401df2158bf5b7
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816941"
---
# <a name="create-app-store-listings"></a>Criar listagens da Store do app


A seção **Listagens da Store** do [processo de envio de aplicativo](app-submissions.md) é onde você fornece o texto e as [imagens](app-screenshots-and-images.md) que os clientes verão na listagem do seu app na Microsoft Store.

Muitos dos campos em uma **listagem da Store** são opcionais, mas sugerimos que você forneça várias imagens e o máximo de informações possível para a listagem se destacar. O mínimo necessário para etapa de **listagens da Microsoft Store** ser considerada como concluída é uma descrição de texto e pelo menos uma [captura de tela](app-screenshots-and-images.md#screenshots). Para alguns envios, os campos [Política de privacidade](#privacy-policy) e [Informações de contato de suporte](#support-contact-info) também são necessários. 

> [!TIP]
> Opcionalmente, também é possível [importar e exportar listagens da Store](import-and-export-store-listings.md) se você prefere inserir as informações de listagem offline em um arquivo .csv em vez de fornecer essas informações e carregar arquivos diretamente no painel. O uso da opção de importar e exportar pode ser especialmente útil se você tiver listagens em vários idiomas, já que ele permite fazer várias atualizações ao mesmo tempo. 

Por padrão, usaremos a mesma listagem da Store (por idioma) para todos os seus sistemas operacionais de destino. Se você quiser usar uma listagem da Store personalizada para um sistema operacional específico com suporte para seu envio, poderá [criar listagens da Store específicas de plataforma](create-platform-specific-store-listings.md). A listagem padrão sempre será mostrada aos clientes no Windows 10.

## <a name="store-listing-languages"></a>Idiomas da listagem da Loja

Você deve preencher a página **listagem da Loja** para pelo menos um idioma. Convém fornecer uma listagem da Loja em cada idioma com suporte nos seus pacotes, mas você tem flexibilidade de remover idiomas para os quais não deseja fornecer uma listagem da Loja. Você também pode criar listagens da Loja em outros idiomas que não são compatíveis com seus pacotes.

> [!NOTE]
> Se o envio já incluir pacotes, mostraremos os [idiomas](supported-languages.md) com suporte nos seus pacotes na página de visão geral do envio (exceto ao remover qualquer um deles).

Para adicionar ou remover idiomas das listagens da Loja, clique em **Adicionar/remover idiomas** na página de visão geral do envio. Se você já tiver carregado pacotes, verá seus idiomas listados na seção **Idiomas com suporte em seus pacotes**. Para remover um ou mais desses idiomas, clique em **Remover**. Se você decidir mais tarde incluir um idioma que foi removido anteriormente desta seção, poderá clicar em **Adicionar**.

Na seção **Idiomas adicionais da listagem da Loja**, você pode clicar em **Gerenciar idiomas adicionais** para adicionar ou remover idiomas *não* incluídos nos seus pacotes. Marque as caixas de seleção dos idiomas que você deseja adicionar e depois clique em **Atualizar**. Os idiomas selecionados serão exibidos na seção **Idiomas adicionais da listagem da Loja**. Para remover um ou mais desses idiomas, clique em **Remover** (ou clique em **Gerenciar idiomas adicionais** e desmarque a caixa de idiomas que você deseja remover).

Quando terminar de fazer suas seleções, clique em **Salvar** para retornar à página de visão geral do envio.

> [!NOTE]
> Ao criar uma listagem da Loja em um idioma sem suporte nos seus pacotes, é necessário indicar quais dos nomes de aplicativo reservados devem ser exibidos na listagem da Loja, pois não há um pacote associado nesse idioma para obter o nome. O nome que você escolher aqui se aplicará somente à listagem da Loja desse idioma e não causará impacto no nome exibido quando um cliente instalar o aplicativo.

Para editar uma listagem da Loja, clique no nome do idioma na página de visão geral do envio.

Na parte superior da página **Listagem da Loja**, estão os campos associados à listagem da Loja padrão do idioma selecionado. Esses campos serão mostrados para todos os seus clientes, a menos que você tenha pacotes direcionados a versões anteriores do sistema operacional (Windows 8.x ou versões anteriores; Windows Phone 8.x ou versões anteriores) e crie listagens da Loja específicas da plataforma para incluir capturas de tela diferentes ou informações a serem exibidas aos clientes em versões específicas do sistema operacional. Para saber mais, consulte [Criar listagens da Loja específicas de plataforma](create-platform-specific-store-listings.md).

## <a name="description"></a>Descrição

O campo Descrição é onde você pode informar aos clientes o que seu aplicativo faz. Esse campo é obrigatório e aceita até 10.000 caracteres de texto sem formatação.

Para obter algumas dicas sobre como fazer a sua descrição se destacar, consulte [Escrever uma ótima descrição do aplicativo](write-a-great-app-description.md).

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>Novidades desta versão

Se esta for a primeira vez em que está enviando o seu app, deixe esse campo em branco. No caso da atualização de um aplicativo existente, é aqui que você pode informar ao cliente o que mudou na versão mais recente. Este campo tem um limite de 1500 caracteres. (Anteriormente, esse campo foi chamado de **Notas de versão**).

## <a name="app-features"></a>Recursos do aplicativo

Trata-se de resumos dos principais recursos do aplicativo. Eles são exibidos para o cliente como uma lista com marcadores na seção **Recursos** da listagem da Store do app, juntamente com a **Descrição**. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por recurso. Você pode incluir até 20 recursos.

> [!NOTE]
> Os recursos do aplicativo aparecerão com marcadores na listagem da Store. Portanto, não adicione marcadores próprios.

## <a name="screenshots"></a>Capturas de tela

Uma captura de tela é necessária para enviar seu aplicativo. Recomendamos que você forneça pelo menos quatro capturas de tela de cada tipo de dispositivos compatíveis com seu app para que todos vejam como será a aparência do app no tipo de dispositivo.

Para obter mais informações, consulte [Capturas de tela e imagens do app](app-screenshots-and-images.md#screenshots).


## <a name="store-logos"></a>Logotipos da Loja 

Os logotipos da Loja são imagens opcionais que você pode carregar para melhorar a forma como o aplicativo será exibido para os clientes. Você também pode especificar que somente as imagens carregadas aqui deverão ser usadas na listagem da Store do app para clientes no Windows 10 (incluindo o Xbox), em vez de permitir que a Store use as imagens de logotipo dos pacotes do app.

> [!IMPORTANT]
> Se o aplicativo oferecer suporte ao Xbox ou for compatível com Windows Phone 8.1 ou anterior, forneça determinadas imagens aqui para que a listagem apareça corretamente na Loja. 

Para obter mais informações, consulte [Logotipos da Loja](app-screenshots-and-images.md#store-logos).


## <a name="additional-art-assets"></a>Ativos de arte adicionais

Você pode enviar ativos adicionais do seu produto, incluindo trailers e imagens promocionais. Estas são todas opcionais, mas recomendamos que você carregue o máximo possível delas. Essas imagens podem dar aos clientes uma ideia melhor do que é seu produto e tornar a listagem mais atrativa.

Para obter mais informações, consulte [Ativos de arte adicionais](app-screenshots-and-images.md#additional-art-assets).


## <a name="supplemental-information"></a>Informações complementares

Os campos nesta seção são todos opcionais. Examine as informações abaixo para determinar se a apresentação dessas informações faz sentido para o seu envio. Em particular, a **Descrição curta** é recomendada para a maioria dos envios. Os outros campos podem ajudar a fornecer uma experiência ideal para seu produto em diferentes cenários.

### <a name="short-title"></a>Título curto

Uma versão mais curta do nome do seu produto. Se fornecido, esse nome mais curto pode aparecer em vários lugares no Xbox One (durante a instalação, em Conquistas etc.) no lugar do título completo do seu produto.

Este campo tem um limite de 50 caracteres.


### <a name="sort-title"></a>Título curto

Se seu produto puder ser colocado em ordem alfabética ou escrito de maneiras diferentes, você poderá inserir outra versão aqui. Isso permite que os clientes encontrem seu produto com mais rapidez se eles digitarem essa versão durante a pesquisa. 

Este campo tem um limite de 255 caracteres.


### <a name="voice-title"></a>Título de voz

Um nome alternativo para seu produto que, se fornecido, pode ser usado na experiência de áudio no Xbox One ao usar o Kinect ou um headset.

Este campo tem um limite de 255 caracteres.


### <a name="short-description"></a>Descrição breve

Uma descrição mais curta e interessante que pode ser usada na parte superior da listagem da Store do seu produto. Se não fornecida, o primeiro parágrafo (até 500 caracteres) da [descrição](#description) mais longa será usado. Como sua descrição também aparece abaixo desse texto, é recomendável fornecer uma breve descrição com texto diferente para que sua listagem da Store não seja repetitiva.

Para jogos, a descrição curta também pode aparecer na seção Informações do Hub de jogos no Xbox One.

Este campo tem um limite de 500 caracteres.


### <a name="additional-system-requirements"></a>Requisitos adicionais do sistema

Se necessário, você poderá descrever as configurações de hardware necessárias para que o aplicativo funcione corretamente (além das informações fornecidas na seção **Requisitos do sistema** em [Propriedades do aplicativo](enter-app-properties.md#system-requirements). Essas informações são especialmente importantes se o aplicativo exigir um hardware que possa não estar disponível em todo computador. Por exemplo, se o aplicativo funciona corretamente somente com o hardware USB externo, como uma impressora 3D ou microcontrolador, sugerimos que você os insira aqui. As informações que você insere aqui serão mostradas aos clientes que estão exibindo a listagem da Store do aplicativo no Windows 10, versão 1607 ou posterior (incluindo o Xbox), juntamente com os requisitos que você indicou na página de propriedades do produto. 

Você pode inserir até 11 itens tanto para **Hardware mínimo** quanto para **Hardware recomendado**. Eles são exibidos para o cliente na forma de uma lista com marcadores na listagem da Microsoft Store. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por item.

> [!NOTE]
> Os requisitos de sistema adicionais serão exibidos com marcadores na listagem da Store; portanto, não adicione marcadores próprios.


<span id="shared-fields" />

## <a name="additional-information"></a>Informações adicionais

Os itens descritos abaixo ajudam os clientes a descobrir e entender seu produto. As informações inseridas aqui se aplicarão a todas as listagens da Loja em um determinado idioma, independentemente do sistema operacional, mesmo se você [criar listagens da Loja específicas de plataforma](create-platform-specific-store-listings.md). (Esta seção anteriormente era chamada de **Campos compartilhados**).

### <a name="search-terms"></a>Termos de pesquisa

Os termos de pesquisa (antigamente denominados palavras-chave) são termos isolados ou pequenas frases que não são exibidos para os clientes, mas podem ajudar a tornar seu app detectável na Store quando os clientes pesquisarem usando esses termos. Você pode incluir até sete termos de pesquisa com no máximo 30 caracteres e usar até 21 palavras separadas em todos os termos de pesquisa.

Ao adicionar termos de pesquisa, pense nas palavras que os clientes podem usam ao procurar aplicativos como o seu, especialmente se elas não fizerem parte do nome do seu aplicativo. Não deixe de usar termos de pesquisa que não sejam relevantes para o aplicativo.

### <a name="copyright-and-trademark-info"></a>Informações sobre direitos autorais e marcas registradas

Se quiser fornecer informações adicionais sobre direitos autorais e/ou marca comercial, digite-as aqui. Este campo tem um limite de 200 caracteres.


### <a name="additional-license-terms"></a>Termos de licença adicionais

Deixe este campo em branco se quiser que seu aplicativo seja licenciado para os seus clientes sob os **Termos de Licença de Aplicativo Padrão** (associados ao [Contrato do Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058)).

Se os termos de licença forem diferentes dos **Termos de Licença de Aplicativo Padrão**, insira-os aqui.

Se você inserir uma única URL para este campo, ela será exibido para os clientes como um link em que eles podem clicar para ler os termos de licença adicionais. Isso será útil se os termos de licença adicionais forem muito longos ou se você quiser incluir links clicáveis ou formatação em seus termos de licença adicionais.

Você também pode inserir até 10.000 caracteres de texto neste campo. Se você fizer isso, os clientes verão esses termos de licença adicionais exibidos como texto sem formatação.


### <a name="developed-by"></a>Desenvolvido por

Digite o texto aqui se você quiser incluir um campo **Desenvolvido por** na listagem da loja do seu aplicativo. (O campo **Publicado por** listará o nome de exibição do fornecedor associado à conta, independentemente de você fornecer um valor para o campo **Desenvolvido por**.)

Este campo tem um limite de 255 caracteres.
 

<span id="privacy-policy" />

> [!NOTE]
> Os campos **Política de privacidade**, **Site** e **Informações de contato de suporte** agora estão localizados na página [Propriedades](enter-app-properties.md).

