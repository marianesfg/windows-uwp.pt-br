---
author: andrewleader
Description: The following article describes all of the properties and elements within tile content.
title: Esquema de conteúdo do bloco
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 07/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, bloco, notificação de bloco, conteúdo de bloco, esquema, carga de bloco
ms.localizationpriority: medium
ms.openlocfilehash: d2baa2e2d7b8d68505159eb480ea3be78750f507
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4620818"
---
# <a name="tile-content-schema"></a>Esquema de conteúdo do bloco

 

A seguir, são descritas todas as propriedades e elementos do conteúdo do bloco.

Se você preferir usar XML bruto em vez da [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consulte [o esquema XML](../tiles-and-notifications/adaptive-tiles-schema.md).

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#TileBindingContentAdaptive)
    * [TileBindingContentIconic](#TileBindingContentIconic)
    * [TileBindingContentContact](#TileBindingContentContact)
    * [TileBindingContentPeople](#TileBindingContentPeople)
    * [TileBindingContentPhotos](#TileBindingContentPhotos)


## <a name="tilecontent"></a>TileContent
TileContent é o objeto de nível superior que descreve o conteúdo da notificação de bloco, incluindo os elementos visuais.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Visual** | [ToastVisual](#tilevisual) | true | Descreve a parte visual da notificação de bloco. |


## <a name="tilevisual"></a>TileVisual
A parte visual dos blocos contém as especificações visuais de todos os tamanhos de bloco e mais propriedades relacionadas aos elementos visuais.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | Forneça uma associação pequena opcional para especificar o conteúdo do tamanho de bloco pequeno. |
| **TileMedium** | [TileBinding](#tilebinding) | false | Forneça uma associação média opcional para especificar o conteúdo do tamanho de bloco médio. |
| **TileWide** | [TileBinding](#tilebinding) | false | Forneça uma associação larga opcional para especificar o conteúdo do tamanho de bloco largo. |
| **TileLarge** | [TileBinding](#tilebinding) | false | Forneça uma associação grande opcional para especificar o conteúdo do tamanho de bloco grande. |
| **Branding** | [TileBranding](#tilebranding) | false | O formato que o bloco deve usar para exibir a marca do app. Por padrão, herda a identidade visual do bloco padrão. |
| **DisplayName** | string | false | Uma cadeia de caracteres opcional que substitui o nome de exibição do bloco durante a exibição dessa notificação. |
| **Arguments** | string | false | Novidades na Atualização de Aniversário: dados definidos pelo app que são retornados ao app por meio da propriedade TileActivatedInfo em LaunchActivatedEventArgs quando o usuário inicia o app no Bloco Dinâmico. Informa a você quais notificações de bloco o usuário viu quando tocou no Bloco Dinâmico. Nos dispositivos sem a Atualização de Aniversário, isso simplesmente será ignorado. |
| **LockDetailedStatus1** | string | false | Se você especificar isso, também deverá fornecer uma associação TileWide. Esse será a primeira linha de texto a ser exibida na tela de bloqueio se o usuário tiver selecionado o bloco como app de status detalhado. |
| **LockDetailedStatus2** | string | false | Se você especificar isso, também deverá fornecer uma associação TileWide. Esse será a segunda linha de texto a ser exibida na tela de bloqueio se o usuário tiver selecionado o bloco como app de status detalhado. |
| **LockDetailedStatus3** | string | false | Se você especificar isso, também deverá fornecer uma associação TileWide. Esse será a terceira linha de texto a ser exibida na tela de bloqueio se o usuário tiver selecionado o bloco como app de status detalhado. |
| **BaseUri** | Uri | false | Uma URL base padrão que é combinada às URLs relativas nos atributos de origem da imagem. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| string | false | A localidade de destino da carga visual ao usar recursos localizados, especificados como marcações de idioma BCP-47 como "en-US" ou "pt-BR". Esse local é substituído por qualquer localidade especificada na associação ou no texto. Se não for fornecido, a localidade do sistema será usada em vez disso. |


## <a name="tilebinding"></a>TileBinding
O objeto de associação possui o conteúdo visual para um tamanho de bloco específico.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Content** | [ITileBindingContent](#itilebindingcontent) | false | O conteúdo visual a ser exibido no bloco. Uma das [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#TileBindingContentIconic), [TileBindingContentContact](#TileBindingContentContact), [TileBindingContentPeople](#TileBindingContentPeople) ou [TileBindingContentPhotos](#TileBindingContentPhotos). |
| **Branding** | [TileBranding](#tilebranding) | false | O formato que o bloco deve usar para exibir a marca do app. Por padrão, herda a identidade visual do bloco padrão. |
| **DisplayName** | string | false | Uma cadeia de caracteres opcional que substitui o nome de exibição do bloco neste tamanho de bloco. |
| **Arguments** | string | false | Novidades na Atualização de Aniversário: dados definidos pelo app que são retornados ao app por meio da propriedade TileActivatedInfo em LaunchActivatedEventArgs quando o usuário inicia o app no Bloco Dinâmico. Informa a você quais notificações de bloco o usuário viu quando tocou no Bloco Dinâmico. Nos dispositivos sem a Atualização de Aniversário, isso simplesmente será ignorado. |
| **BaseUri** | Uri | false | Uma URL base padrão que é combinada às URLs relativas nos atributos de origem da imagem. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| string | false | A localidade de destino da carga visual ao usar recursos localizados, especificados como marcações de idioma BCP-47 como "en-US" ou "pt-BR". Esse local é substituído por qualquer localidade especificada na associação ou no texto. Se não for fornecido, a localidade do sistema será usada em vez disso. |


## <a name="itilebindingcontent"></a>ITileBindingContent
Interface de marcador para conteúdo de associação do bloco. Permite que você escolha o que deseja especificar nos elementos visuais do bloco - Adaptável ou um dos modelos especiais.

| Implementações |
| --- |
| [TileBindingContentAdaptive](#TileBindingContentAdaptive) |
| [TileBindingContentIconic](#TileBindingContentIconic) |
| [TileBindingContentContact](#TileBindingContentContact) |
| [TileBindingContentPeople](#TileBindingContentPeople) |
| [TileBindingContentPhotos](#TileBindingContentPhotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
Compatível com todos os tamanhos. Esta é a maneira recomendada de especificar o conteúdo do bloco. Modelos de bloco adaptáveis novos no Windows 10; você pode criar uma ampla variedade de blocos personalizados por meio de modelos adaptáveis.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Children** | IList<[ITileBindingContentAdaptiveChild](#ITileBindingContentAdaptiveChild)> | false | Os elementos visuais embutidos. Os objetos [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage) e [AdaptiveGroup](#adaptivegroup) podem ser adicionados. Os filhos são exibidos em um StackPanel vertical. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | Uma imagem de plano de fundo opcional exibida atrás do conteúdo do bloco, sangramento total. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | Uma imagem opcional animada que surge da parte superior do bloco. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | Controla o empilhamento de texto (alinhamento vertical) do conteúdo filho como um todo. |


## <a name="adaptivetext"></a>AdaptiveText
Um elemento de texto adaptável.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Texto** | string | false | O texto a ser exibido. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | O estilo controla o tamanho, a espessura e a opacidade da fonte do texto. |
| **HintWrap** | bool? | false | Defina como true para habilitar a quebra automática de texto. Assume false como valor padrão. |
| **HintMaxLines** | int? | false | O número máximo de linhas que o elemento de texto tem permissão de exibir. |
| **HintMinLines** | int? | false | O número mínimo de linhas que o elemento de texto deve exibir. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | O alinhamento horizontal do texto. |
| **Language** | string | false | A localidade de destino da carga XML, especificada como marcações de idioma BCP-47 como "en-US" ou "pt-BR". A localidade especificada aqui substitui qualquer outra localidade especificada, como na vinculação ou no visual. Se esse valor é uma cadeia de caracteres literal, esse atributo assume como padrão o idioma do usuário da interface do usuário. Se esse valor for uma referência de cadeia de caracteres, esse atributo assume como padrão a localidade escolhida pelo Windows Runtime na resolução da cadeia de caracteres. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
O estilo de texto controla o tamanho, a espessura e opacidade da fonte. A opacidade sutil é 60% opaca.

| Valor | Significado |
|---|---|
| **Padrão** | Valor padrão. O estilo é determinado pelo renderizador. |
| **Caption** | Tamanho menor do que a fonte de parágrafo. |
| **CaptionSubtle** | Mesmo que Caption, mas com opacidade sutil. |
| **Body** | Tamanho da fonte de parágrafo. |
| **BodySubtle** | Mesmo que Body, mas com opacidade sutil. |
| **Base** | Tamanho da fonte de parágrafo, a espessura é negrito. Essencialmente a versão em negrito do corpo. |
| **BaseSubtle** | Mesmo que Base, mas com opacidade sutil. |
| **Subtítulo** | Tamanho da fonte H4. |
| **SubtitleSubtle** | Mesmo que Subtitle, mas com opacidade sutil. |
| **Title** | Tamanho da fonte H3. |
| **TitleSubtle** | Mesmo que Title, mas com opacidade sutil. |
| **TitleNumeral** | Igual a Title, mas com preenchimento superior ou inferior removido. |
| **Subcabeçalho** | Tamanho da fonte H2. |
| **SubheaderSubtle** | Mesmo que Subheader, mas com opacidade sutil. |
| **SubheaderNumeral** | Igual a Subheader, mas com preenchimento superior ou inferior removido. |
| **Header** | Tamanho da fonte H1. |
| **HeaderSubtle** | Mesmo que Header, mas com opacidade sutil. |
| **HeaderNumeral** | Igual a Header, mas com preenchimento superior ou inferior removido. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Controla o alinhamento horizontal de texto.

| Valor | Significado |
|---|---|
| **Padrão** | Valor padrão. O alinhamento é determinado automaticamente pelo renderizador. |
| **Automático** | O alinhamento é determinado por idioma e cultura atual. |
| **Esquerda** | Alinha o texto horizontalmente à esquerda. |
| **Centralizar** | Alinha o texto horizontalmente no centro. |
| **Direita** | Alinha o texto horizontalmente à direita. |


## <a name="adaptiveimage"></a>AdaptiveImage
Uma imagem embutida.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Origem** | string | true | A URL da imagem. Suporte para ms-appx, ms-appdata e http. Na Fall Creators Update, as imagens da Web podem ter até 3 MB em conexões normais e 1 MB em conexões limitadas. Em dispositivos que ainda não executam a Fall Creators Update, as imagens da Web devem ser maiores do que 200 KB. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Controla o recorte desejado da imagem. |
| **HintRemoveMargin** | bool? | false | Por padrão, as imagens em grupos/subgrupos têm uma margem de 8 px em volta. Você pode remover essa margem ao definir essa propriedade como true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | O alinhamento horizontal da imagem. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do bloco. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Especifica o recorte desejado da imagem.

| Valor | Significado |
|---|---|
| **Padrão** | Valor padrão. Comportamento de corte determinado pelo renderizador. |
| **Nenhum** | A imagem não é cortada. |
| **Círculo** | A imagem é cortada na forma de círculo. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Especifica o alinhamento horizontal de uma imagem.

| Valor | Significado |
|---|---|
| **Padrão** | Valor padrão. Comportamento de alinhamento determinado pelo renderizador. |
| **Ampliar** | A imagem é esticada para preencher a largura disponível (e a altura possivelmente disponível também, dependendo de onde a imagem é colocada). |
| **Esquerda** | Alinhe a imagem à esquerda, exibindo a imagem na resolução nativa. |
| **Centralizar** | Alinhe a imagem horizontalmente no centro, exibindo a imagem na resolução nativa. |
| **Direita** | Alinhe a imagem à direita, exibindo a imagem na resolução nativa. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Os grupos identificam semanticamente que o conteúdo do grupo deve ser exibido como um todo ou não exibido se não couber. Os Grupos também permitem a criação de várias colunas.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Filhos** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Subgrupos são exibidos como colunas verticais. Você deve usar subgrupos para fornecer qualquer conteúdo em um AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Os subgrupos são colunas verticais que podem conter texto e imagens.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Filhos** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) e [AdaptiveImage](#adaptiveimage) são filhos válidos de subgrupos. |
| **HintWeight** | int? | false | Controle a largura da coluna do subgrupo ao especificar a espessura em relação aos outros subgrupos. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | Controle o alinhamento vertical do conteúdo deste subgrupo. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Interface de marcador para filhos de subgrupo.

| Implementações |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking especifica o alinhamento vertical do conteúdo.

| Valor | Significado |
|---|---|
| **Padrão** | Valor padrão. O renderizador selecionará automaticamente o alinhamento vertical padrão. |
| **Superior** | Alinhamento vertical com a parte superior. |
| **Centralizar** | Alinhamento vertical com o centro. |
| **Menor** | Alinhamento vertical com a parte inferior. |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
Uma imagem de plano de fundo exibida com sangramento total no bloco.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Origem** | string | true | A URL da imagem. ms-appx, ms-appdata e http(s) são compatíveis. As imagens HTTP devem ter 200 KB ou menos. |
| **HintOverlay** | int? | false | Uma sobreposição preta em uma imagem de plano de fundo. Este valor controla a opacidade da sobreposição preta, sendo 0 equivalente a nenhuma sobreposição e 100 equivalente a completamente preto. Assume 20 como valor padrão. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | Novo na versão 1511: especifica como a imagem deve ser cortada. Nas versões anteriores a 1511, isso será ignorado e a imagem de plano de fundo será exibida sem cortes. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do bloco. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
Controla o recorte da imagem de plano de fundo.

| Valor | Significado |
|---|---|
| **Padrão** | O corte usa o comportamento padrão do renderizador. |
| **Nenhum** | A imagem não é cortada, exibida em um quadrado. |
| **Círculo** | A imagem é cortada em círculo. |


## <a name="tilepeekimage"></a>TilePeekImage
Uma imagem animada que surge da parte superior do bloco.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Origem** | string | true | A URL da imagem. ms-appx, ms-appdata e http(s) são compatíveis. As imagens HTTP devem ter 200 KB ou menos. |
| **HintOverlay** | int? | false | Novo na versão 1511: uma sobreposição preta na imagem que surge. Este valor controla a opacidade da sobreposição preta, sendo 0 equivalente a nenhuma sobreposição e 100 equivalente a completamente preto. Assume 20 como valor padrão. Nas versões anteriores, este valor será ignorado e a imagem que surge será exibida com sobreposição 0. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | Novo na versão 1511: especifica como a imagem deve ser cortada. Nas versões anteriores a 1511, isso será ignorado e a imagem que surge será exibida sem cortes. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do bloco. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
Controla o recorte da imagem que surge.

| Valor | Significado |
|---|---|
| **Padrão** | O corte usa o comportamento padrão do renderizador. |
| **Nenhum** | A imagem não é cortada, exibida em um quadrado. |
| **Círculo** | A imagem é cortada em círculo. |


### <a name="tiletextstacking"></a>TileTextStacking
O empilhamento de texto especifica o alinhamento vertical do conteúdo.

| Valor | Significado |
|---|---|
| **Padrão** | Valor padrão. O renderizador selecionará automaticamente o alinhamento vertical padrão. |
| **Superior** | Alinhamento vertical com a parte superior. |
| **Centralizar** | Alinhamento vertical com o centro. |
| **Menor** | Alinhamento vertical com a parte inferior. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Compatível com blocos pequenos e médios. Habilita um modelo de bloco icônico, em que você pode ter um ícone e um selo exibidos um ao lado do outro no bloco, no estilo clássico verdadeiro do Windows Phone. O número ao lado do ícone é obtido por meio de uma notificação de selo separada.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Icon** | [TileBasicImage](#tilebasicimage) | true | No mínimo, para dar suporte a blocos pequenos e médios na Área de Trabalho e no Celular forneça uma imagem de taxa de proporção quadrada com uma resolução de 200 x 200, formato PNG, com transparência e nenhuma outra cor além do branco. Para obter mais informações, consulte [Modelos de bloco especiais](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
Somente celular. Compatível com blocos pequenos, médios e largos.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Image** | [TileBasicImage](#tilebasicimage) | true | A imagem a ser exibida. |
| **Text** | [TileBasicText](#tilebasictext) | false | Uma linha de texto exibida. Não é exibida em blocos pequenos. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
Novo na versão 1511: compatível com blocos médios, largos e grandes (Área de Trabalho e Celular). Antigamente, isso aplicava-se somente a celulares e a blocos médios e largos.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | Imagens que percorrerão o entorno como círculos. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
Oferece animação por meio de uma apresentação de slides de fotos. Compatível com todos os tamanhos.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | Até 12 imagens podem ser fornecidas (o celular exibirá até nove imagens), que serão usadas na apresentação de slides. A adição de mais de 12 imagens gerará uma exceção. |


### <a name="tilebasicimage"></a>TileBasicImage
Uma imagem usada em diversos modelos especiais.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Origem** | string | true | A URL da imagem. ms-appx, ms-appdata e http(s) são compatíveis. As imagens HTTP devem ter 200 KB ou menos. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do bloco. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="tilebasictext"></a>TileBasicText
Um elemento de texto básico usado em diversos modelos especiais.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Texto** | string | false | O texto a ser exibido. |
| **Language** | string | false | A localidade de destino da carga XML, especificada como marcações de idioma BCP-47 como "en-US" ou "pt-BR". A localidade especificada aqui substitui qualquer outra localidade especificada, como na vinculação ou no visual. Se esse valor é uma cadeia de caracteres literal, esse atributo assume como padrão o idioma do usuário da interface do usuário. Se esse valor for uma referência de cadeia de caracteres, este atributo assumirá como padrão a localidade escolhida pelo Windows Runtime na resolução da cadeia de caracteres. |


## <a name="related-topics"></a>Tópicos relacionados

* [Início rápido: enviar uma notificação de bloco local](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Biblioteca de notificações no GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)