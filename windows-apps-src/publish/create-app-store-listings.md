---
author: jnHs
Description: "A seção de listagens da Loja do processo de envio de app é onde você fornece o texto e as imagens que os clientes verão na página de detalhes do seu app na Loja."
title: Criar listagens de apps da Loja
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dcce4d53dd095c634f893d40f87eaf69cf546f1d
ms.lasthandoff: 02/07/2017

---

# <a name="create-app-store-listings"></a>Criar listagens de apps da Loja


A seção de **listagens da Loja** do [processo de envio de app](app-submissions.md) é onde você fornece o texto e as [imagens](app-screenshots-and-images.md) que os clientes verão na listagem da Loja do seu app.

Muitos dos campos da seção **listagem da Loja** são opcionais, mas sugerimos o fornecimento de várias imagens e tantas informações quanto possível para fazer a sua listagem se destacar. O mínimo obrigatório para a etapa **listagens da Loja** ser considerada completa é uma descrição de texto e pelo menos uma [captura de tela](app-screenshots-and-images.md).

Por padrão, usaremos a mesma listagem da Loja (por idioma) para todos os seus sistemas operacionais de destino. Se quiser usar uma listagem da Loja personalizada para um sistema operacional específico, você poderá [criar listagens da Loja específicas da plataforma](create-platform-specific-store-listings.md).

## <a name="store-listing-languages"></a>Idiomas de listagem da Loja

Você deve preencher a página **listagem da Loja** para pelo menos um idioma. Convém fornecer uma listagem da Loja em cada idioma com suporte nos seus pacotes, mas você tem flexibilidade de remover idiomas para os quais não deseja fornecer uma listagem da Loja. Você também pode criar listagens da Loja em outros idiomas que não são compatíveis com seus pacotes.

> **Observação** Se o seu envio já incluir pacotes, mostraremos os [idiomas](supported-languages.md) com suporte nos seus pacotes na página de visão geral do envio (a menos que você remova qualquer um deles).

Para adicionar ou remover idiomas para suas listagens da Loja, clique em **Gerenciar idiomas de listagem da Loja** na página de visão geral de envio. Se você já tiver carregado pacotes, verá seus idiomas listados na seção **Idiomas com suporte em seus pacotes**. Para remover um ou mais desses idiomas, clique em **Remover**. Se você decidir mais tarde incluir um idioma que foi removido anteriormente desta seção, poderá clicar em **Adicionar**.

Na seção **Idiomas adicionais da listagem da Loja**, você pode clicar em **Gerenciar idiomas adicionais** para adicionar ou remover idiomas *não* incluídos nos seus pacotes. Marque as caixas de seleção dos idiomas que você deseja adicionar e depois clique em **Atualizar**. Os idiomas selecionados serão exibidos na seção **Idiomas adicionais da listagem da Loja**. Para remover um ou mais desses idiomas, clique em **Remover** (ou clique em **Gerenciar idiomas adicionais** e desmarque a caixa de idiomas que você deseja remover).

Quando terminar de fazer suas seleções, clique em **Salvar** para retornar à página de visão geral do envio.

> **Observação** Ao criar uma listagem da Loja em um idioma que não tenha suporte em seus pacotes, você precisará indicar quais dos seus nomes de app reservados devem ser exibidos nessa listagem da Loja, pois não há um pacote associado nesse idioma no qual obter o nome. O nome que você escolher aqui se aplicará somente à listagem da Loja desse idioma e não causará impacto no nome exibido quando um cliente instalar o app.

Para editar uma listagem da Loja, clique no nome do idioma na visão geral do envio. As seções da página **listagem da Loja** estão descritas abaixo.

## <a name="default-store-listing-fields"></a>Campos de listagem da Loja padrão

Na parte superior da página **listagem da Loja** são os campos associados à sua listagem da Loja padrão para o idioma selecionado. Esses campos serão mostrados para todos os seus clientes, a menos que você tenha pacotes direcionados a versões anteriores do sistema operacional (Windows 8.x ou versões anteriores; Windows Phone 8.x ou versões anteriores) e crie listagens da Loja específicas da plataforma para incluir capturas de tela diferentes ou informações a serem exibidas aos clientes em versões específicas do sistema operacional. Para saber mais, consulte [Criar listagens da Loja específicas de plataforma](create-platform-specific-store-listings.md).

### <a name="description"></a>Descrição

O campo Descrição é onde você pode informar aos clientes o que seu app faz. Esse campo é obrigatório e aceita até 10.000 caracteres de texto sem formatação.

Para obter algumas dicas sobre como fazer a sua descrição se destacar, consulte [Escrever uma ótima descrição do app](write-a-great-app-description.md).

### <a name="release-notes"></a>Notas de versão

Se esta for a primeira vez em que está enviando o seu app, provavelmente você vai querer deixar este campo em branco. No caso da atualização de um app existente, é aqui que você pode informar ao cliente o que mudou na versão mais recente. Este campo tem um limite de 1500 caracteres.

### <a name="screenshots"></a>Capturas de tela

Na maioria dos casos, você verá vários campos para fornecer capturas de tela de diferentes tipos de dispositivos. Você não tem obrigação de fornecer capturas de tela separadas de cada tipo de dispositivo; apenas uma captura de tela é necessária para o seu envio (mas você pode fornecer até nove por tipo de dispositivo). Na maior parte dos casos, sugerimos que você forneça capturas de tela de todos os tipos de dispositivos compatíveis com o seu app, para que os clientes vejam imagens parecidas com a aparência que o app terá no dispositivo deles.

Para saber mais, consulte [Capturas de tela e imagens do app](app-screenshots-and-images.md).

### <a name="app-tile-icon"></a>Ícone do bloco do app

O ícone do bloco de apps é usado ao exibir a listagem da Loja do seu app para clientes no Windows Phone 8.1 e versões anteriores (e em alguns layouts da Loja para clientes no Windows 10). Deve ser um arquivo .png medindo 300 x 300 pixels.

Para saber mais, consulte [Ícone do bloco de app](app-screenshots-and-images.md#app-tile-icon).

### <a name="app-features"></a>Recursos do app

Trata-se de resumos dos principais recursos do app. Eles são exibidos para o cliente na forma de uma lista com marcadores na listagem da Loja do app, junto com a Descrição. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por recurso. Você pode incluir até 20 recursos.

**Observação**  Eles serão exibidos com marcadores na sua listagem da Loja. Assim, não adicione marcadores próprios.

### <a name="additional-system-requirements"></a>Requisitos adicionais do sistema

Se necessário, você poderá descrever as configurações de hardware necessárias para que o app funcione corretamente (além das informações fornecidas na seção **Requisitos do sistema** em [Propriedades do app](enter-app-properties.md#system-requirements). Essas informações são especialmente importantes se o app exigir um hardware que possa não estar disponível em todo computador.

 Você pode inserir até 11 itens tanto para **Hardware mínimo** quanto para **Hardware recomendado**.  Elas são exibidas para o cliente na forma de uma lista com marcadores nos detalhes do seu app. Deixe-os resumidos, com apenas algumas palavras (e não mais que 200 caracteres) por item. As informações que você insere aqui serão mostradas aos clientes que estão exibindo a listagem da Loja do app no Windows 10, versão 1607 ou posterior, juntamente com os requisitos que você indicou na página de propriedades do produto.

**Observação**  Eles serão exibidos com marcadores na lista. Assim, não adicione marcadores próprios.

## <a name="shared-fields"></a>Campos compartilhados

Os itens descritos abaixo são todos os campos compartilhados e se aplicarão às listagens da Loja em um determinado idioma, independentemente do sistema operacional, mesmo se você [criar listagens da Loja específicas de plataforma](create-platform-specific-store-listings.md).

### <a name="keywords"></a>Palavras-chave

As palavras-chave são termos isolados ou pequenas frases que não são exibidos aos clientes, mas podem ajudar seu app a aparecer nos resultados de pesquisa relacionados à palavra-chave. Você pode incluir até 7 palavras-chave com um máximo de 30 caracteres.

Se você quiser adicionar palavras-chave, pense nas palavras que os clientes podem usam ao pesquisar apps como o seu, especialmente se elas não fazem parte do nome do seu app. Não deixe de usar todas as palavras-chave realmente importantes para o app.

### <a name="copyright-and-trademark-info"></a>Informações sobre direitos autorais e marcas registradas

Se quiser fornecer informações adicionais sobre direitos autorais e/ou marca comercial, digite-as aqui. Este campo tem um limite de 200 caracteres.

### <a name="additional-license-terms"></a>Termos de licença adicionais

Deixe este campo em branco se quiser que seu app seja licenciado para os seus clientes sob os **Termos de Licença de Aplicativo Padrão** (associados ao [Contrato do Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058)).

Se os termos de licença forem diferentes dos **Termos de Licença de Aplicativo Padrão**, insira-os aqui.

Se você inserir uma única URL para este campo, ela será exibido para os clientes como um link em que eles podem clicar para ler os termos de licença adicionais. Isso será útil se os termos de licença adicionais forem muito longos ou se você quiser incluir links clicáveis ou formatação em seus termos de licença adicionais.

Você também pode adicionar até 10.000 caracteres de texto nesse campo. Se você fizer isso, os clientes verão esses termos de licença adicionais exibidos como texto sem formatação.

### <a name="website"></a>Site

Insira a URL da página da Web do seu app. A URL deve apontar para uma página em seu próprio site, não para os detalhes do seu app na Loja.

### <a name="support-contact-info"></a>Informações de contato de suporte

Insira a URL da página da Web em que seus clientes podem buscar suporte relacionado ao seu app ou o endereço de email que os seus clientes podem contatar para obter suporte).

**Importante**  A Microsoft não fornece suporte para seu app aos seus clientes.

### <a name="privacy-policy"></a>Política de privacidade

Se você tiver uma política de privacidade para o seu app, insira sua URL aqui. Você é responsável por garantir que seu app esteja em conformidade com as leis e as normas privacidade e por fornecer uma política de privacidade, se obrigatório.

**Importante**  A Microsoft não fornece uma política de privacidade padrão para o app. Da mesma forma, o app não é coberto por nenhuma política de privacidade da Microsoft. Para determinar se o seu app requer uma política de privacidade, consulte o [Contrato de Desenvolvedor de Aplicativo](https://msdn.microsoft.com/library/windows/apps/hh694058) e as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1).

