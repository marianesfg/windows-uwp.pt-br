---
author: jnHs
Description: "A seção de listagens da Loja do processo de envio de aplicativo é onde você fornece o texto e as imagens que os clientes verão na página de detalhes do seu aplicativo na Loja."
title: Criar listagens de apps da Loja
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 004169178c906ac892865569fd2ed483bd2471fa
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
# <a name="create-app-store-listings"></a>Criar listagens de apps da Loja


A seção de **listagens da Loja** do [processo de envio de aplicativo](app-submissions.md) é onde você fornece o texto e as [imagens](app-screenshots-and-images.md) que os clientes verão na listagem da Loja do seu aplicativo.

> [!NOTE]
> Recentemente, atualizamos as opções desta página. Se você tinha um envio em andamento antes das opções mais recentes estarem disponíveis, esse envio ainda mostrará as opções mais antigas. Você pode excluir esse envio e, em seguida, criar um novo se quiser usar as novas opções desse aplicativo. Caso contrário, as opções mais recentes serão disponibilizadas com a próxima atualização depois que você publicar o envio em andamento.

Muitos dos campos da seção **listagem da Loja** são opcionais, mas sugerimos o fornecimento de várias imagens e tantas informações quanto possível para fazer a sua listagem se destacar. O mínimo necessário para a etapa **listagens da Loja** ser considerada completa é uma descrição de texto e pelo menos uma [captura de tela](app-screenshots-and-images.md#screenshots).

> [!TIP]
> Também é possível [importar e exportar listagens da Loja](import-and-export-store-listings.md) se você deseja inserir as informações de listagem offline em um arquivo .csv em vez de fornecer essas informações diretamente no painel. Isso pode ser útil com listagens em vários idiomas.

Por padrão, usaremos a mesma listagem da Loja (por idioma) para todos os seus sistemas operacionais de destino. Se você quiser usar uma listagem da Loja personalizada para um sistema operacional específico, poderá [criar listagens da Loja específicas de plataforma](create-platform-specific-store-listings.md). A listagem padrão sempre será mostrada aos clientes no Windows 10.

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

## <a name="release-notes"></a>Notas de versão

Se esta for a primeira vez em que está enviando o seu aplicativo, provavelmente você vai querer deixar este campo em branco. No caso da atualização de um aplicativo existente, é aqui que você pode informar ao cliente o que mudou na versão mais recente. Este campo tem um limite de 1500 caracteres.

## <a name="screenshots"></a>Capturas de tela

Uma captura de tela é necessária para enviar seu aplicativo. É recomendável fornecer pelo menos uma captura de tela para cada tipo de dispositivo compatível com seu aplicativo.

Para obter mais informações, consulte [Capturas de tela e imagens do aplicativo](app-screenshots-and-images.md#screenshots).

## <a name="store-logos"></a>Logotipos da Loja 

Os logotipos da Loja são imagens opcionais que você pode carregar para melhorar a forma como o aplicativo será exibido para os clientes. Você também pode especificar que somente as imagens carregadas aqui deverão ser usadas na listagem da Loja do aplicativo para clientes do Windows 10, em vez de permitir que a Loja use as imagens de logotipo dos pacotes do aplicativo.

> [!IMPORTANT]
> Se o aplicativo oferecer suporte ao Xbox ou for compatível com Windows Phone 8.1 ou anterior, forneça determinadas imagens aqui para que a listagem apareça corretamente na Loja. 

Para obter mais informações, consulte [Logotipos da Loja](app-screenshots-and-images.md#store-logos).

## <a name="additional-art-assets"></a>Ativos de arte adicionais

Você pode enviar ativos adicionais do seu produto, incluindo trailers e imagens promocionais. Estas são todas opcionais, mas recomendamos que você carregue o máximo possível delas. Essas imagens podem dar aos clientes uma ideia melhor do que é seu produto e tornar a listagem mais atrativa.

Para obter mais informações, consulte [Ativos de arte adicionais](app-screenshots-and-images.md#additional-art-assets).

## <a name="additional-information"></a>Informações adicionais

Os campos desta seção são todos opcionais, mas podem ser usados para ajudar os clientes a entender mais sobre o que seu aplicativo faz e o que é necessário para ter a melhor experiência. Sugerimos a análise das opções descritas abaixo e o fornecimento de qualquer informação que os clientes talvez precisem saber sobre seu aplicativo ou que possa atraí-los para o download.

### <a name="app-features"></a>Recursos do aplicativo

Trata-se de resumos dos principais recursos do aplicativo. Eles são exibidos para o cliente como uma lista com marcadores na listagem da Loja do aplicativo, juntamente com a Descrição. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por recurso. Você pode incluir até 20 recursos.

> [!NOTE]
> Os recursos do aplicativo aparecerão com marcadores na listagem da Loja. Portanto, não adicione marcadores próprios.

### <a name="additional-system-requirements"></a>Requisitos adicionais do sistema

Se necessário, você poderá descrever as configurações de hardware necessárias para que o aplicativo funcione corretamente (além das informações fornecidas na seção **Requisitos do sistema** em [Propriedades do aplicativo](enter-app-properties.md#system-requirements). Essas informações são especialmente importantes se o aplicativo exigir um hardware que possa não estar disponível em todo computador.

Você pode inserir até 11 itens tanto para **Hardware mínimo** quanto para **Hardware recomendado**.  Elas são exibidas para o cliente na forma de uma lista com marcadores nos detalhes do seu aplicativo. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por item.

As informações que você insere aqui serão mostradas aos clientes que estão exibindo a listagem da Loja do aplicativo no Windows 10, versão 1607 ou posterior, juntamente com os requisitos que você indicou na página de propriedades do produto.

> [!NOTE]
> Os requisitos de sistema adicionais serão exibidos com marcadores na listagem da loja; portanto, não adicione marcadores próprios.

### <a name="developed-by"></a>Desenvolvido por

Digite o texto aqui se você quiser incluir um campo **Desenvolvido por** na listagem da loja do seu aplicativo. (O campo **Publicado por** listará o nome de exibição do fornecedor associado à conta, independentemente de você fornecer um valor para o campo **Desenvolvido por**.)

Este campo tem um limite de 255 caracteres.


## <a name="shared-fields"></a>Campos compartilhados

Os itens descritos abaixo ajudam os clientes a descobrir e entender seu produto. As informações inseridas aqui se aplicarão a todas as listagens da Loja em um determinado idioma, independentemente do sistema operacional, mesmo se você [criar listagens da Loja específicas de plataforma](create-platform-specific-store-listings.md).

### <a name="search-terms"></a>Termos de pesquisa

Os termos de pesquisa (antigamente denominados palavras-chave) são termos isolados ou pequenas frases que não são exibidos para os clientes, mas podem ajudar seu aplicativo a aparecer nos resultados de pesquisa relacionados ao termo. Você pode incluir até sete termos de pesquisa com no máximo 30 caracteres e usar até 21 palavras separadas em todos os termos de pesquisa.

Ao adicionar termos de pesquisa, pense nas palavras que os clientes podem usam ao procurar aplicativos como o seu, especialmente se elas não fizerem parte do nome do seu aplicativo. Não deixe de usar termos de pesquisa que não sejam relevantes para o aplicativo.


### <a name="privacy-policy"></a>Política de privacidade

Se seu aplicativo tiver uma política de privacidade, insira a URL aqui. Você é responsável por garantir que seu aplicativo esteja em conformidade com as leis e as normas privacidade e por fornecer uma política de privacidade, se necessário.

> [!IMPORTANT]
> A Microsoft não fornece uma política de privacidade padrão para o aplicativo. Da mesma forma, o aplicativo não é coberto por nenhuma política de privacidade da Microsoft. Para determinar se o aplicativo requer uma política de privacidade, consulte o [Contrato de Desenvolvedor de Aplicativo](https://msdn.microsoft.com/library/windows/apps/hh694058) e as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1).

### <a name="copyright-and-trademark-info"></a>Informações sobre direitos autorais e marcas registradas

Se quiser fornecer informações adicionais sobre direitos autorais e/ou marca comercial, digite-as aqui. Este campo tem um limite de 200 caracteres.

### <a name="additional-license-terms"></a>Termos de licença adicionais

Deixe este campo em branco se quiser que seu aplicativo seja licenciado para os seus clientes sob os **Termos de Licença de Aplicativo Padrão** (associados ao [Contrato do Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058)).

Se os termos de licença forem diferentes dos **Termos de Licença de Aplicativo Padrão**, insira-os aqui.

Se você inserir uma única URL para este campo, ela será exibido para os clientes como um link em que eles podem clicar para ler os termos de licença adicionais. Isso será útil se os termos de licença adicionais forem muito longos ou se você quiser incluir links clicáveis ou formatação em seus termos de licença adicionais.

Você também pode inserir até 10.000 caracteres de texto neste campo. Se você fizer isso, os clientes verão esses termos de licença adicionais exibidos como texto sem formatação.

### <a name="website"></a>Site

Insira a URL da página da Web do seu aplicativo. A URL deve apontar para uma página em seu próprio site, não para os detalhes do seu aplicativo na Loja.

### <a name="support-contact-info"></a>Informações de contato de suporte

Insira a URL da página da Web em que seus clientes podem buscar suporte relacionado ao seu aplicativo ou o endereço de email que os seus clientes podem contatar para obter suporte).

> [!IMPORTANT]
> A Microsoft não fornece suporte para seu aplicativo aos seus clientes.

