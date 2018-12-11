---
Description: The Store listings section of the app submission process is where you provide the text and images that customers will see when viewing your app's listing in the Microsoft Store.
title: Criar listagens da Store do app
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, listagem, descrição, página da store, notas de versão, título
ms.localizationpriority: medium
ms.openlocfilehash: a913c522450a8d28c03066c922df2e3e2972f92f
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8877495"
---
# <a name="create-app-store-listings"></a>Criar listagens da Store do app


A seção **Listagens da Store** do [processo de envio de aplicativo](app-submissions.md) é onde você fornece o texto e as [imagens](app-screenshots-and-images.md) que os clientes verão na listagem do seu app na Microsoft Store.

Muitos dos campos em uma **listagem da Store** são opcionais, mas sugerimos que você forneça várias imagens e o máximo de informações possível para a listagem se destacar. O mínimo necessário para etapa de **listagens da Microsoft Store** ser considerada como concluída é uma descrição de texto e pelo menos uma [captura de tela](app-screenshots-and-images.md#screenshots).

> [!TIP]
> Você pode, opcionalmente, [Importar e exportar listagens da loja](import-and-export-store-listings.md) se você prefere inserir as informações de listagem offline em um arquivo. csv, em vez de fornecer informações e carregando arquivos diretamente no Partner Center. O uso da opção de importar e exportar pode ser especialmente útil se você tiver listagens em vários idiomas, já que ele permite fazer várias atualizações ao mesmo tempo. 

Se seu aplicativo publicado anteriormente é compatível com o Windows 8. x e/ou Windows Phone 8. x ou anterior, você pode [criar listagens da loja específicas de plataforma](create-platform-specific-store-listings.md) para mostrar aos clientes. 

## <a name="store-listing-languages"></a>Idiomas da listagem da Loja

Você deve preencher a página **listagem da Loja** para pelo menos um idioma. Convém fornecer uma listagem da Loja em cada idioma com suporte nos seus pacotes, mas você tem flexibilidade de remover idiomas para os quais não deseja fornecer uma listagem da Loja. Você também pode criar listagens da Loja em outros idiomas que não são compatíveis com seus pacotes.

> [!NOTE]
> Se o envio já incluir pacotes, mostraremos os [idiomas](supported-languages.md) com suporte nos seus pacotes na página de visão geral do envio (exceto ao remover qualquer um deles).

Para adicionar ou remover idiomas das listagens da Loja, clique em **Adicionar/remover idiomas** na página de visão geral do envio. Se você já tiver carregado pacotes, verá seus idiomas listados na seção **Idiomas com suporte em seus pacotes**. Para remover um ou mais desses idiomas, clique em **Remover**. Se você decidir mais tarde incluir um idioma que foi removido anteriormente desta seção, poderá clicar em **Adicionar**.

Na seção **Idiomas adicionais da listagem da Loja**, você pode clicar em **Gerenciar idiomas adicionais** para adicionar ou remover idiomas *não* incluídos nos seus pacotes. Marque as caixas de seleção dos idiomas que você deseja adicionar e depois clique em **Atualizar**. Os idiomas selecionados serão exibidos na seção **Idiomas adicionais da listagem da Loja**. Para remover um ou mais desses idiomas, clique em **Remover** (ou clique em **Gerenciar idiomas adicionais** e desmarque a caixa de idiomas que você deseja remover).

Quando terminar de fazer suas seleções, clique em **Salvar** para retornar à página de visão geral do envio. 

## <a name="add-and-edit-store-listing-info"></a>Adicionar e editar informações de listagem da loja

Para editar uma listagem da loja, selecione o nome do idioma na página de visão geral do envio. Você deve editar cada idioma separadamente, a menos que você escolha exportar suas listagens da loja e trabalhar offline e, em seguida, importar todos os dados de listagem ao mesmo tempo. Para obter mais informações sobre como isso funciona, consulte [Importar e exportar listagens da loja](import-and-export-store-listings.md).

Os campos disponíveis são descritos abaixo.

## <a name="product-name"></a>Nome do produto

Essa caixa de lista suspensa permite que você especifique qual nome deve ser usado na listagem da loja (se você tiver reservado mais de um nome para o aplicativo).

Se você tiver carregado pacotes no mesmo idioma como o armazenamento de listagem que você está trabalhando no, o nome usado nesses pacotes será selecionado. Se você precisar renomear [o aplicativo](manage-app-names.md#rename-an-app-that-has-already-been-published) depois que ele já tiver sido publicado, você pode selecionar um nome diferente reservado aqui quando você cria um novo envio, depois que você tiver carregado pacotes que usam o novo nome.

Se você ainda não tiver carregado pacotes para o idioma que você está trabalhando e reservados mais de um nome, você precisará selecionar um dos nomes de aplicativo reservados, pois não há um pacote associado nesse idioma no qual obter o nome.

> [!NOTE]
> O **nome do produto** que você selecionar somente se aplica a listagem da loja no idioma estiver trabalhando em. Ela não afeta o nome exibido quando um usuário instala o aplicativo; Esse nome vem de manifesto do pacote que é instalado. Para evitar confusão, recomendamos que os pacotes e listagem da loja de cada idioma usem o mesmo nome.

## <a name="description"></a>Descrição

O campo Descrição é onde você pode informar aos clientes o que seu aplicativo faz. Esse campo é obrigatório e aceita até 10.000 caracteres de texto sem formatação.

Para obter algumas dicas sobre como fazer a sua descrição se destacar, consulte [Escrever uma ótima descrição do aplicativo](write-a-great-app-description.md).

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>Novidades desta versão

Se esta for a primeira vez em que está enviando o seu app, deixe esse campo em branco. Para obter uma atualização para um aplicativo existente, isso é onde você pode permitir que os clientes saibam o que mudou na versão mais recente. Este campo tem um limite de 1500 caracteres. (Anteriormente, esse campo foi chamado de **Notas de versão**).

## <a name="product-features"></a>Recursos do produto

Trata-se de resumos dos principais recursos do aplicativo. Eles são exibidos para o cliente como uma lista com marcadores na seção **Recursos** da listagem da Store do app, juntamente com a **Descrição**. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por recurso. Você pode incluir até 20 recursos.

> [!NOTE]
> Esses recursos serão exibidos com marcadores na listagem da Store, portanto, não adicione marcadores próprios.

## <a name="screenshots"></a>Capturas de tela

Uma captura de tela é necessária para enviar seu aplicativo. Recomendamos que você forneça pelo menos quatro capturas de tela de cada tipo de dispositivos compatíveis com seu app para que todos vejam como será a aparência do app no tipo de dispositivo.

Para obter mais informações, consulte [Capturas de tela e imagens do app](app-screenshots-and-images.md#screenshots).


## <a name="store-logos"></a>Logotipos da Loja 

Os logotipos da Loja são imagens opcionais que você pode carregar para melhorar a forma como o aplicativo será exibido para os clientes. Você também pode especificar que somente as imagens carregadas aqui deverão ser usadas na listagem da Store do app para clientes no Windows 10 (incluindo o Xbox), em vez de permitir que a Store use as imagens de logotipo dos pacotes do app.

> [!IMPORTANT]
> Se o aplicativo oferecer suporte ao Xbox ou for compatível com Windows Phone 8.1 ou anterior, forneça determinadas imagens aqui para que a listagem apareça corretamente na Loja. 

Para obter mais informações, consulte [Logotipos da Loja](app-screenshots-and-images.md#store-logos).


## <a name="trailers-and-additional-assets"></a>Trailers e outros ativos

Você pode enviar ativos adicionais para seu produto, incluindo trailers de vídeo e imagens promocionais. Estas são todas opcionais, mas recomendamos que você carregue o máximo possível delas. Essas imagens podem dar aos clientes uma ideia melhor do que é seu produto e tornar a listagem mais atrativa.

Para obter mais informações, consulte [Trailers e outros ativos](app-screenshots-and-images.md#trailers-and-additional-assets).

<a id="supplemental-information" />

## <a name="supplemental-fields"></a>Campos complementares

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

Para obter melhores resultados, manter sua descrição curta em 270 caracteres. O campo tem um limite de 500 caracteres, mas em alguns modos de exibição, apenas os primeiro 270 caracteres serão mostrados (com um link disponível para exibir o restante da descrição breve).


### <a name="additional-system-requirements"></a>Requisitos adicionais do sistema

Se necessário, você poderá descrever as configurações de hardware necessárias para que o aplicativo funcione corretamente (além das informações fornecidas na seção **Requisitos do sistema** em [Propriedades do aplicativo](enter-app-properties.md#system-requirements). Essas informações são especialmente importantes se o aplicativo exigir um hardware que possa não estar disponível em todo computador. Por exemplo, se o aplicativo funciona corretamente somente com o hardware USB externo, como uma impressora 3D ou microcontrolador, sugerimos que você os insira aqui. As informações que você insere aqui serão mostradas aos clientes que estão exibindo a listagem da Store do aplicativo no Windows 10, versão 1607 ou posterior (incluindo o Xbox), juntamente com os requisitos que você indicou na página de propriedades do produto. 

Você pode inserir até 11 itens tanto para **Hardware mínimo** quanto para **Hardware recomendado**. Eles são exibidos para o cliente na forma de uma lista com marcadores na listagem da Microsoft Store. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por item.

> [!NOTE]
> Os requisitos de sistema adicionais serão exibidos com marcadores na listagem da Store; portanto, não adicione marcadores próprios.


<span id="shared-fields" />

## <a name="additional-information"></a>Informações adicionais

Os itens descritos abaixo ajudam os clientes a descobrir e entender seu produto. (Esta seção anteriormente era chamada de **Campos compartilhados**).

### <a name="search-terms"></a>Termos de pesquisa

Os termos de pesquisa (antigamente denominados palavras-chave) são termos isolados ou pequenas frases que não são exibidos para os clientes, mas podem ajudar a tornar seu app detectável na Store quando os clientes pesquisarem usando esses termos. Você pode incluir até sete termos de pesquisa com no máximo 30 caracteres e usar até 21 palavras separadas em todos os termos de pesquisa.

Ao adicionar termos de pesquisa, pense nas palavras que os clientes podem usam ao procurar aplicativos como o seu, especialmente se elas não fizerem parte do nome do seu aplicativo. Não deixe de usar termos de pesquisa que não sejam relevantes para o aplicativo.

### <a name="copyright-and-trademark-info"></a>Informações sobre direitos autorais e marcas registradas

Se quiser fornecer informações adicionais sobre direitos autorais e/ou marca comercial, digite-as aqui. Este campo tem um limite de 200 caracteres.


### <a name="additional-license-terms"></a>Termos de licença adicionais

Deixe este campo em branco se quiser que seu aplicativo seja licenciado para os seus clientes sob os **Termos de Licença de Aplicativo Padrão** (associados ao [Contrato do Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)).

Se os termos de licença forem diferentes dos **Termos de Licença de Aplicativo Padrão**, insira-os aqui.

Se você inserir uma única URL para este campo, ela será exibido para os clientes como um link em que eles podem clicar para ler os termos de licença adicionais. Isso será útil se os termos de licença adicionais forem muito longos ou se você quiser incluir links clicáveis ou formatação em seus termos de licença adicionais.

Você também pode inserir até 10.000 caracteres de texto neste campo. Se você fizer isso, os clientes verão esses termos de licença adicionais exibidos como texto sem formatação.


### <a name="developed-by"></a>Desenvolvido por

Digite o texto aqui se você quiser incluir um campo **Desenvolvido por** na listagem da loja do seu aplicativo. (O campo **Publicado por** listará o nome de exibição do fornecedor associado à conta, independentemente de você fornecer um valor para o campo **Desenvolvido por**.)

Este campo tem um limite de 255 caracteres.
 

<span id="privacy-policy" />

> [!NOTE]
> Os campos **Política de privacidade**, **Site** e **Informações de contato de suporte** agora estão localizados na página [Propriedades](enter-app-properties.md).

