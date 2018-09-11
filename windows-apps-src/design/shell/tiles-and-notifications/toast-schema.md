---
author: andrewleader
Description: The following article describes all of the properties and elements within toast content.
title: Esquema de conteúdo de notificação do sistema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad1700d58ab3568aa3aefa46b5950d0a8bf3c320
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3850190"
---
# <a name="toast-content-schema"></a>Esquema de conteúdo de notificação do sistema

 

A seguir, são descritas todas as propriedades e os elementos no conteúdo da notificação do sistema.

Se você preferir usar XML bruto em vez da [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consulte [o esquema XML](toast-xml-schema.md).

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent é o objeto de nível superior que descreve o conteúdo da notificação, incluindo elementos visuais, ações e áudio.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Launch**| string | false | Uma cadeia de caracteres que é passada para o aplicativo quando ele é ativado pela Notificação do sistema. O formato e o conteúdo dessa cadeia de caracteres são definidos pelo aplicativo para seu uso próprio. Quando o usuário toca ou clica na Notificação do sistema para iniciar o aplicativo associado, a cadeia de caracteres de inicialização fornece o contexto ao aplicativo que o permite mostrar ao usuário uma exibição relevante para o conteúdo da Notificação do sistema em vez de inicializar de modo padrão. |
| **Visual** | [ToastVisual](#toastvisual) | true | Descreve a parte visual de notificação do sistema. |
| **Actions** | [IToastActions](#itoastactions) | false | Opcionalmente, crie ações personalizadas com botões e entradas. |
| **Audio** | [ToastAudio](#toastaudio) | false | Descreve a parte de áudio da notificação do sistema. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Especifica o tipo de ativação que será usada quando o usuário clica no corpo dessa notificação do sistema. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novidade na Atualização de criadores: opções adicionais referentes à ativação da notificação do sistema. |
| **Scenario** | [ToastScenario](#toastscenario) | false | Declara o cenário em que a notificação do sistema é usada, como um alarme ou lembrete. |
| **DisplayTimestamp** | DateTimeOffset? | false | Novidade na Atualização de criadores: substitui o carimbo de hora padrão por um carimbo de data e hora personalizado quando o conteúdo da notificação é disponibilizado em vez da hora em que a notificação foi recebida pela plataforma Windows. |
| **Header** | [ToastHeader](#toastheader) | false | Novidade na Atualização de criadores: adiciona um cabeçalho personalizado à notificação para agrupar várias notificações juntas na Central de Ações. |


### <a name="toastscenario"></a>ToastScenario
Especifica qual cenário representa a notificação do sistema.

| Valor | Significado |
|---|---|
| **Default** | O comportamento normal do sistema. |
| **Reminder** | Uma notificação de lembrete. Isso será exibido previamente expandido e permanece na tela do usuário até ser ignorado. |
| **Alarm** | Uma notificação de alarmes. Isso será exibido previamente expandido e permanece na tela do usuário até ser ignorado. O áudio fará um loop por padrão e usará o áudio de alarme. |
| **IncomingCall** | Uma notificação de chamada de entrada. Isso será exibido previamente expandido em um formato de chamada especial e permanece na tela do usuário até ser ignorado. O áudio fará um loop por padrão e usará o áudio de toque. |


## <a name="toastvisual"></a>ToastVisual
A parte visual das notificações do sistema contém as associações, a qual contém texto, imagens, conteúdo adaptável e muito mais.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | A vinculação de notificação do sistema genérica, que pode ser renderizada em todos os dispositivos. Essa vinculação é obrigatória e não pode ser nula. |
| **BaseUri** | Uri | false | Uma URL base padrão que é combinada às URLs relativas nos atributos de origem da imagem. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| string | false | A localidade de destino da carga visual ao usar recursos localizados, especificados como marcações de idioma BCP-47 como "en-US" ou "pt-BR". Esse local é substituído por qualquer localidade especificada na associação ou no texto. Quando não é fornecido, a localidade do sistema será usada em vez disso. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
A associação genérica é a associação padrão para notificações do sistema e é onde você pode especificar texto, imagens, conteúdo adaptável e muito mais.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | O conteúdo do corpo da Notificação do sistema, que pode incluir texto, imagens e grupos (adicionados na Atualização de aniversário). Os elementos de texto devem vir antes de quaisquer outros elementos e há suporte apenas para três elementos de texto. Se um elemento de texto é colocado após qualquer outro elemento, ele será puxado até a parte superior ou removido. Por fim, determinadas propriedades de texto como HintStyle não têm suporte nos elementos de texto filho de raiz e funcionam somente em um AdaptiveSubgroup. Se você usar AdaptiveGroup em dispositivos sem a Atualização de aniversário, o conteúdo do grupo é simplesmente ignorado. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | Um logotipo opcional para substituir o logotipo do aplicativo. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | Uma imagem opcional de "herói" em destaque que é exibida na notificação e na Central de Ações. |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | Texto de atribuição opcional que será exibido na parte inferior da notificação do sistema. |
| **BaseUri** | Uri | false | Uma URL base padrão que é combinada às URLs relativas nos atributos de origem da imagem. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Language**| string | false | A localidade de destino da carga visual ao usar recursos localizados, especificados como marcações de idioma BCP-47 como "en-US" ou "pt-BR". Esse local é substituído por qualquer localidade especificada na associação ou no texto. Quando não é fornecido, a localidade do sistema será usada em vez disso. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Marcador de interface para elementos filho de notificação do sistema que incluem texto, imagens, grupos e muito mais.

| Implementações |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Um elemento de texto adaptável. Se for colocado no ToastBindingGeneric.Children de nível superior, somente HintMaxLines será aplicado. Mas, se for colocado como um filho de um grupo/subgrupo, há suporte para o estilo de texto completo.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Texto** | sequência ou [BindableString](#bindablestring) | false | O texto para exibição. Suporte à vinculação de dados adicionado à Atualização de criadores, mas funciona apenas para elementos de texto de nível superior. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | O estilo controla o tamanho, a espessura e a opacidade da fonte do texto. Funciona somente para elementos de texto em um grupo/subgrupo. |
| **HintWrap** | bool? | false | Defina como true para habilitar o encapsulamento de texto. Os elementos de texto de nível superior ignoram essa propriedade e sempre são encapsulados (você pode usar HintMaxLines = 1 para desabilitar o encapsulamento para elementos de texto de nível superior). Elementos de texto em grupos/subgrupos padrão como false para encapsulamento. |
| **HintMaxLines** | int? | false | O número máximo de linhas que o elemento de texto tem permissão de exibir. |
| **HintMinLines** | int? | false | O número mínimo de linhas que que o elemento de texto deve exibir. Funciona somente para elementos de texto em um grupo/subgrupo. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | O alinhamento horizontal do texto. Funciona somente para elementos de texto em um grupo/subgrupo. |
| **Language** | string | false | A localidade de destino da carga XML, especificada como marcações de idioma BCP-47 como "en-US" ou "pt-BR". A localidade especificada aqui substitui qualquer outra localidade especificada, como na vinculação ou no visual. Se esse valor é uma cadeia de caracteres literal, esse atributo assume como padrão o idioma do usuário da interface do usuário. Se esse valor for uma referência de sequência, este atributo assumirá como padrão a localidade escolhida pelo Windows Runtime na resolução da sequência. |


### <a name="bindablestring"></a>BindableString
Um valor de associação para sequências.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **BindingName** | sequência | true | Obtém ou define o nome que mapeado para o valor de dados de associação. |


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
| **Origem** | string | true | A URL da imagem. Suporte para ms-appx, ms-appdata e http. Na the Fall Creators Update, as imagens da Web podem ter até 3 MB em conexões normais e 1 MB em conexões limitadas. Em dispositivos que ainda não executam Fall Creators Update, as imagens da web devem ser maiores do que 200 KB. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Novidade na Atualização de aniversário: controlar o recorte desejado da imagem. |
| **HintRemoveMargin** | bool? | false | Por padrão, as imagens em grupos/subgrupos têm uma margem de 8 px em volta. Você pode remover essa margem ao definir essa propriedade como true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | O alinhamento horizontal da imagem. Funciona somente para imagens em um grupo/subgrupo. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


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
Novidade na Atualização de aniversário: os grupos identificam semanticamente que o conteúdo do grupo deve ser exibido como um todo ou não exibido se não couber. Os Grupos também permitem a criação de várias colunas.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Filhos** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Subgrupos são exibidos como colunas verticais. Você deve usar subgrupos para fornecer qualquer conteúdo em um AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Novidade na Atualização de aniversário: subgrupos são colunas verticais que podem conter texto e imagens.

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


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Novidade na Atualização para Criadores: uma barra de progresso. Compatível somente com notificações do sistema na Área de trabalho, build 15063 ou mais recente.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Título** | sequência ou [BindableString](#bindablestring) | false | Obtém ou define uma sequência de título opcional. Suporte à associação de dados. |
| **Valor** | dobro ou [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) ou [BindableProgressBarValue](#bindableprogressbarvalue) | false | Obtém ou define o valor da barra de progresso. Suporte à associação de dados. Assume 0 como valor padrão. |
| **ValueStringOverride** | sequência ou [BindableString](#bindablestring) | false | Obtém ou define uma sequência para exibição em vez da sequência de percentual padrão. Caso não seja fornecida, algo como "70%" será exibido. |
| **Status** | sequência ou [BindableString](#bindablestring) | true | Obtém ou define uma sequência de status (obrigatória), que é exibida abaixo da barra de progresso à esquerda. Essa sequência deve refletir o status da operação, como "Baixando..." ou "Instalando..." |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Uma classe que represente o valor da barra de progresso.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Valor** | double | false | Obtém ou define o valor (0,0 – 1,0) que representa o percentual concluído. |
| **IsIndeterminate** | bool | false | Obtém ou define um valor que indica se a barra de progresso é indeterminada. Se isso acontecer, **Value** será ignorado. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Um valor de barra de progresso associável.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **BindingName** | sequência | true | Obtém ou define o nome que mapeado para o valor de dados de associação. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Um logotipo que será exibido em vez do logotipo do aplicativo.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Origem** | string | true | A URL da imagem. Suporte para ms-appx, ms-appdata e http. As imagens HTTP devem ter 200 KB ou menos. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | Especifica como você deseja que a imagem seja cortada. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Controla o recorte da imagem de logotipo do aplicativo.

| Valor | Significado |
|---|---|
| **Padrão** | O corte usa o comportamento padrão do renderizador. |
| **Nenhum** | A imagem não é cortada, exibida em um quadrado. |
| **Círculo** | A imagem é cortada em um círculo. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Uma imagem de "herói" em destaque que é exibida na notificação e na Central de Ações.

| Propriedade | Tipo | Obrigatório |Descrição |
|---|---|---|---|
| **Origem** | string | true | A URL da imagem. Suporte para ms-appx, ms-appdata e http. As imagens HTTP devem ter 200 KB ou menos. |
| **AlternateText** | string | false | Texto alternativo que descreve a imagem, usado para fins de acessibilidade. |
| **AddImageQuery** | bool? | false | Defina como "true" para permitir que o Windows acrescente uma cadeia de caracteres de consulta à URL de imagem fornecida na notificação do sistema. Use esse atributo se o servidor hospedar imagens e conseguir manipular cadeias de caracteres de consulta, ao recuperar uma variante da imagem com base nas cadeias de caracteres de consulta ou ao ignorar a cadeia de caracteres de consulta e retornar a imagem conforme especificado sem a cadeia de caracteres de consulta. Essa cadeia de caracteres de consulta especifica a escala, a configuração de contraste e o idioma; por exemplo, um valor de "www.website.com/images/hello.png" fornecido na notificação é transformado em "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Texto de atribuição exibido na parte inferior da notificação do sistema.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Texto** | string | true |  o texto para exibição. |
| **Language** | string | false | A localidade de destino da carga visual ao usar recursos localizados, especificados como marcações de idioma BCP-47 como "en-US" ou "pt-BR". Quando não é fornecido, a localidade do sistema será usada em vez disso. |


## <a name="itoastactions"></a>IToastActions
Interface de marcador para ações de notificação do sistema/entradas.

| Implementações |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Implementa [IToastActions](#itoastactions)*

Crie suas próprias ações personalizadas e entradas usando controles como botões, caixas de texto e entradas de seleção.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Entradas** | IList<[IToastInput](#itoastinput)> | false | Entradas como caixas de texto e entradas de seleção. Até cinco entradas são permitidas. |
| **Botões** | IList<[IToastButton](#itoastbutton)> | false | Os botões são exibidos após todas as entradas (ou adjacentes a uma entrada se o botão é usado como um botão de resposta rápida). Até cinco botões são permitidos (ou menos se também existirem itens de menu de contexto). |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Novidade na Atualização de aniversário: itens de menu de contexto personalizado, fornecendo ações adicionais se o usuário clicar com o botão direito do mouse na notificação. Você pode ter até cinco botões e itens de menu de contexto *combinados*. |


## <a name="itoastinput"></a>IToastInput
As interface do marcador para entradas de notificação do sistema.

| Implementações |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Implementa [IToastInput](#itoastinput)*

Um controle de caixa de texto na qual o usuário pode digitar texto.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Id** | string | true | A Id é obrigatória e é usada para mapear o texto inserido pelo usuário em um par chave-valor de id/valor que o aplicativo consome posteriormente. |
| **Título** | string | false | O texto do título que será exibido acima da caixa de texto. |
| **PlaceholderContent** | string | false | Texto do espaço reservado a ser exibido na caixa de texto quando o usuário ainda não tinha digitado qualquer texto. |
| **DefaultInput** | string | false | O texto inicial para colocar na caixa de texto. Deixe este nulo para uma caixa de texto em branco. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Implementa [IToastInput](#itoastinput)*

Um controle de caixa de seleção, que permite aos usuários selecionar a partir de uma lista suspensa de opções.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Id** | string | true | A Id é obrigatória. Se o usuário selecionou este item, essa Id será passada para o código do aplicativo, representando a seleção escolhida. |
| **Content** | string | true | O conteúdo é necessário e é uma cadeia de caracteres exibida no item da seleção. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Um item de caixa de seleção (um item que o usuário pode selecionar na lista suspensa).

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Id** | string | true | A Id é obrigatória e é usada para mapear o texto inserido pelo usuário em um par chave-valor de id/valor que o aplicativo consome posteriormente. |
| **Título** | string | false | O texto do título que será exibido acima da caixa de seleção. |
| **DefaultSelectionBoxItemId** | string | false | Isso controla qual item é selecionado por padrão e refere-se à propriedade da Id de [ToastSelectionBoxItem](#toastselectionboxitem). Se você não fornecer essas informações, a seleção padrão ficará vazia (o usuário vê nada). |
| **Items** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Os itens de seleção que o usuário pode selecionar na SelectionBox. Apenas cinco itens podem ser adicionados. |


## <a name="itoastbutton"></a>IToastButton
Interface de marcador para botões de notificação do sistema.

| Implementações |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Implementa [IToastButton](#itoastbutton)*

Um botão que o usuário pode clicar.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Content** | string | true | Obrigatório. O texto a ser exibido no botão. |
| **Arguments** | string | true | Obrigatório. Cadeia de caracteres de argumentos definida pelo aplicativo e ele receberá posteriormente se o usuário clicar neste botão. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Controla o tipo de ativação que o botão usará quando for clicado. O padrão é o Primeiro plano. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novidade na Atualização para Criadores: obtém ou define mais opções referentes à ativação do botão de notificação do sistema. |


### <a name="toastactivationtype"></a>ToastActivationType
Decide o tipo de ativação que será usada quando o usuário interage com uma ação específica.

| Valor | Significado |
|---|---|
| **Foreground** | Valor padrão. O aplicativo em primeiro plano é iniciado. |
| **Background** | A tarefa em segundo plano correspondente (supondo que você configure tudo) é acionada e você pode executar o código em segundo plano (como enviar uma mensagem de resposta rápida do usuário) sem interromper o usuário. |
| **Protocol** | Inicie um aplicativo diferente usando a ativação de protocolo. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Novidade na Atualização de criadores: opções adicionais relacionadas à ativação.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Novidade na Fall Creators Update: obtém ou define o comportamento que a notificação do sistema deve usar quando o usuário invoca essa ação. Isso funciona somente na Área de trabalho para [ToastButton](#toastbutton) e [ToastContextMenuItem](#toastcontextmenuitem). |
| **ProtocolActivationTargetApplicationPfn** | string | false | Se você estiver usando *ToastActivationType.Protocol*, como alternativa, você pode especificar o PFN de destino para que independentemente de vários aplicativos estarem registrados para manipular o mesmo uri de protocolo, o aplicativo desejado seja sempre iniciado. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Especifica o comportamento que a notificação do sistema deve usar quando o usuário executa ações referentes à notificação do sistema.

| Valor | Significado |
|---|---|
| **Padrão** | Comportamento padrão. A notificação do sistema será ignorada quando o usuário executar ações referentes à notificação do sistema. |
| **PendingUpdate** | Depois que o usuário clica em um botão da notificação do sistema, ela permanecerá presente, em um estado visual de "atualização pendente". Você deve atualizar imediatamente a notificação do sistema de uma tarefa em segundo plano para que o usuário não veja esse estado visual de "atualização pendente" por muito tempo. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Implementa [IToastButton](#itoastbutton)*

Um botão de adiamento manipulado pelo sistema que manipula automaticamente o adiamento da notificação.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **CustomContent** | string | false | Texto personalizado opcional exibido no botão que substitui o texto padrão localizado de "Tocar de novo". |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Implementa [IToastButton](#itoastbutton)*

Um botão de ignorar manipulado pelo sistema que descarta a notificação quando clicado.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **CustomContent** | string | false | Texto personalizado opcional exibido no botão que substitui o texto padrão localizado de "Ignorar". |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*Implementa [IToastActions](#itoastactions)

Constrói automaticamente uma caixa de seleção para adiar intervalos e botões tocar de novo/ignorar, localizados automaticamente, e a lógica de adiamento é manipulada automaticamente pelo sistema.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Novidade na Atualização de aniversário: itens de menu de contexto personalizado, fornecendo ações adicionais se o usuário clicar com o botão direito do mouse na notificação. Você pode ter até cinco itens. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Uma entrada de item de menu de contexto.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Content** | string | true | Obrigatório. O texto para exibição. |
| **Arguments** | string | true | Obrigatório. Cadeia de caracteres de argumentos definida pelo aplicativo que o aplicativo pode recuperar posteriormente depois que é ativada quando o usuário clica no item de menu. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Controla o tipo de ativação que o item de menu usará quando for clicado. O padrão é o Primeiro plano. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Novidade na Atualização de criadores: opções adicionais referentes à ativação do item de menu de contexto de notificação do sistema. |


## <a name="toastaudio"></a>ToastAudio
Especifique o áudio que será reproduzido quando a Notificação do sistema for recebida.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Src** | uri | false | O arquivo de mídia para reprodução em vez do som padrão. Suporte apenas para ms-appx e ms-appdata. |
| **Loop** | booliano | false | Defina como true se o som deve repetir enquanto a Notificação do sistema for exibida; false para reproduzir apenas uma vez (padrão). |
| **Silencioso** | booliano | false | True para desativar o som; false para permitir que o som da notificação do sistema seja reproduzido (padrão). |


## <a name="toastheader"></a>ToastHeader
Novidade na Atualização para Criadores: um cabeçalho personalizado que agrupa várias notificações na Central de Ações.

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Id** | string | true | Um identificador criado por desenvolvedores e que identifica exclusivamente este cabeçalho. Se duas notificações têm a mesma id de cabeçalho, elas serão exibidas sob o mesmo cabeçalho na Central de Ações. |
| **Título** | string | true | Um título para o cabeçalho. |
| **Argumentos**| string | true | Obtém ou define uma sequência de argumentos definida pelo desenvolvedor e que é retornada para o aplicativo quando o usuário clica nesse cabeçalho. Não pode ser nula. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Obtém ou define o tipo de ativação que o cabeçalho usará quando for clicado. O padrão é o Primeiro plano. Observe que apenas o Primeiro plano e o Protocolo são compatíveis. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Obtém ou define mais opções referentes à ativação do cabeçalho da notificação do sistema. |


## <a name="related-topics"></a>Tópicos relacionados

* [Guia de início rápido: enviar uma notificação do sistema local e manipular a ativação](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [Biblioteca de notificações no GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)