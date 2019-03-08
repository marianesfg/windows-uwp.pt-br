---
Description: Como criar ícones/logotipos de aplicativos que representam seu aplicativo no menu Iniciar, blocos de aplicativos, a barra de tarefas, a Microsoft Store e muito mais.
title: Ícones e logotipos de apps
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622451"
---
# <a name="app-icons-and-logos"></a>Ícones e logotipos de apps 

Cada aplicativo tem um ícone/logotipo do que o representa, e esse ícone será exibido em vários locais no shell do Windows: 

:::row:::
    :::column:::
        * A barra de título da janela do aplicativo
        * A lista de aplicativos no menu Iniciar
        * O Gerenciador de tarefas e tarefa
        * Blocos do aplicativo
        * Tela inicial do seu aplicativo
        * Em que a Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Este artigo aborda os conceitos básicos da criação de ícones do aplicativo, como usar o Visual Studio para gerenciá-los e como gerenciá-los manualmente, caso seja necessário.
 
(Este artigo é especificamente para os ícones que representam o aplicativo em si; para obter diretrizes gerais de ícone, consulte o [ícones](icons.md) artigo.)

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de ícones, locais e fatores de dimensionamento

Por padrão, o Visual Studio armazena seus ativos de ícone em um subdiretório de ativos. Aqui está uma lista dos diferentes tipos de ícones, em que aparecem, e o que são chamados. 

| Nome do ícone | Aparece no | Nome do arquivo de ativo |
| ---      | ---        | --- |
| Bloco pequeno | Menu Iniciar |  SmallTile.png  |
| Bloco médio |Menu Iniciar, listagem da Microsoft Store\*  |  Square150x150Logo.png |
| Bloco largo  | Menu Iniciar   | Wide310x150Logo.png |
| Bloco grande   | Menu Iniciar, listagem da Microsoft Store\* |  LargeTile.png  |
| Ícone do aplicativo | Lista de aplicativos no menu Iniciar, a barra de tarefas, o Gerenciador de tarefas | Square44x44Logo.png |
| Tela inicial | Tela inicial do aplicativo | SplashScreen.png  |
| Logotipo de notificação | Blocos do aplicativo | BadgeLogo.png  |
| Logotipo do pacote logotipo/Store | Instalador de aplicativo, Partner Center, a opção "Um aplicativo de relatório" na Store, a opção "Escrever uma resenha" na Store | StoreLogo.png  |

\* Usado a menos que você [exibição de imagens no Store carregadas somente](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para garantir que esses ícones aparência perfeitos em todas as telas, você pode criar várias versões do mesmo ícone de fatores de escala de exibição diferentes. 

O fator de escala determina o tamanho dos elementos de interface do usuário, como texto. Intervalo de fatores de escala de 100% a 400%. Valores maiores criam elementos de interface do usuário maiores, tornando-os mais fácil de ver em com alto dpi. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Como ativos de ícone do aplicativo são bitmaps e bitmaps não escalam bem, recomendamos que você fornecer uma versão de cada ativo de ícone para cada fator de escala: 125%, 100%, 150%, 200% e 400 %). Há muitos dos ícones! Felizmente, o Visual Studio fornece uma ferramenta que torna mais fácil gerar e atualizar esses ícones. 

## <a name="microsoft-store-listing-image"></a>Imagem de listagem da Microsoft Store

"Como especificar imagens para listagem do meu aplicativo em que a Microsoft Store?"

Por padrão, podemos usar algumas das imagens de seus pacotes em Store, conforme descrito na tabela na parte superior desta página (juntamente com outros [imagens que você fornece durante o processo de envio](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). No entanto, você tem a opção de impedir que o Store usando as imagens de logotipo em pacotes do seu aplicativo ao exibir sua listagem para os clientes no Windows 10 (incluindo Xbox) e, em vez disso, ter o Store usar apenas as imagens que você carregar. Isso lhe dá mais controle sobre a aparência do aplicativo em várias telas em toda a Loja. (Observe que, se o seu produto dá suporte a versões anteriores do SO, os clientes que ainda podem ver imagens dos seus pacotes, mesmo se você usar essa opção.) Você pode fazer isso na **Store logotipos** seção o **listagem Store** etapa do processo de envio.

![Especificando os logotipos da Store durante o processo de envio de aplicativo](images/app-icons/storelogodisplay.png)

Quando você marcar esta caixa, uma nova seção chamada **exibir imagens de Store** é exibida. Aqui, você pode carregar 3 tamanhos de imagem que o Store usará no lugar de imagens de logotipo de pacotes do seu aplicativo: 71 x 71, 150 x 150 e 300 x 300 pixels. Somente o tamanho de 300 x 300 é necessário, embora seja recomendável fornecer todos os tamanhos de 3.

Para obter mais informações, consulte [vídeo carregado apenas imagens de logotipo no Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Gerenciando os ícones do aplicativo com o Designer do Visual Studio de manifesto

O Visual Studio fornece uma ferramenta muito útil para gerenciar seus ícones de aplicativo chamada a **Designer de manifesto**. 

> Se você ainda não tiver o Visual Studio 2017, há várias versões disponíveis, incluindo uma versão gratuita, (Visual Studio 2017 Community Edition), e as outras versões oferecem avaliações gratuitas. Você pode baixá-los aqui: [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Para iniciar o Designer de manifesto:
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Use o Visual Studio para abrir um projeto UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. No **Gerenciador de soluções**, duas vezes no arquivo Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Clique o **ativos visuais** guia.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Gerando todos os ativos ao mesmo tempo

O primeiro item de menu na **ativos visuais** guia **todos os ativos visuais**, faz exatamente o que seu nome diz: gera todos os ativos visuais que seu aplicativo precisa com o toque de um botão.

![Gerar todos os ativos visuais no Visual Studio](images/app-icons/all-visual-assets.png)

Tudo o que você precisa fazer é fornecer uma única imagem, e o Visual Studio gerará o pequeno bloco, bloco médio, grande bloco, bloco largo, bloco grande, ícone do aplicativo, tela inicial e ativos de logotipo para cada fator de escala do pacote.

Para gerar todos os ativos ao mesmo tempo:
1. Clique o **...**  ao lado de **origem** campo e selecione a imagem que você deseja usar. Se você estiver usando uma imagem de bitmap, verifique se é pelo menos 400 por 400 pixels para que você obtenha resultados nítidos. Imagens vetoriais funcionam melhor; Visual Studio permite que você use AI (Adobe Illustrator) e arquivos PDF. 
2. (Opcional). No **configurações de exibição** seção, configurar essas opções:

    a.  **Nome curto**:  Especifique um nome curto para seu aplicativo.

    b.  **Mostrar nome**: Indique se você deseja exibir o nome curto em blocos de médio, grande ou grandes. 

    c. **Plano de fundo lado a lado**: Especifique o valor hexadecimal ou um nome de cor para a cor de plano de fundo lado a lado. Por exemplo, `#464646`. O valor padrão é `transparent`.

    d. **Plano de fundo de tela de Spash**: Especifique o nome de cor ou o valor hexadecimal para a tela de fundo de tela de spash. 

3. Clique em **gerar**. 

Visual Studio gera os arquivos de imagem e os adiciona ao projeto. Se você quiser alterar seus ativos, basta repetir o processo. 

Ativos dimensionados ícone siga essa convenção de nomenclatura de arquivo:

*nome do arquivo*- scale -*fator de escala*. PNG

Por exemplo,

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Observe que o Visual Studio não gera um logotipo de notificação por padrão. Isso ocorre porque seu logotipo de notificação é exclusivo e provavelmente não deve coincidir com outros ícones do aplicativo. Para obter mais informações, consulte o [notificação notificações para o artigo de aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Mais informações sobre ativos de ícone do aplicativo
Visual Studio gerará todos os ativos de ícone de aplicativo exigidos pelo seu projeto, mas se você quiser personalizá-los, é importante para entender como eles são diferentes dos outros ativos do aplicativo. 

O ativo de ícone do aplicativo é exibido em muitos lugares: o canto inferior direito dos blocos de início, a exibição da tarefa, ALT + TAB e barra de tarefas do Windows. Como o ativo de ícone do aplicativo é exibido em tantos locais, ela tem alguns dimensionamento adicional e plating os outros ativos não precisam de opções: ativos de "tamanho de destino" e "unplated" ativos. 

### <a name="target-size-app-icon-assets"></a>Ativos de ícone do aplicativo de tamanho de destino
Além dos tamanhos do fator de escala padrão ("Square44x44Logo.scale-400.png"), também recomendamos a criação de ativos de "tamanho de destino". Chamamos esses ativos-tamanho de destino porque elas visam tamanhos específicos, como 16 pixels, em vez de fatores de dimensionamento específico, como 400. Ativos de tamanho de destino são para as superfícies que não usam o sistema de limite de dimensionamento:

* Iniciar a lista de atalhos (desktop)
* Iniciar o canto inferior do bloco (desktop)
* Atalhos (desktop)
* Painel de Controle (desktop)

Aqui está a lista de ativos de tamanho de destino:


| Tamanho do ativo | Exemplo de nome de arquivo                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* No mínimo, recomendamos que você fornecer esses tamanhos. 

Você não precisa adicionar preenchimento a esses ativos; o Windows adicionará o preenchimento, se necessário. Esses ativos devem levar em conta uma superfície mínima de 16 pixels. 

Aqui está um exemplo desses ativos conforme eles aparecem em ícones da barra de tarefas do Windows:

![ativos na barra de tarefas do Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Unplated ativos
Por padrão, o Windows usa um ativo de destino na parte superior de uma placa colorido por padrão. Se você quiser, você pode fornecer um ativo de unplated com base no destino. "Unplated" significa que o ativo será exibido em um plano de fundo transparente. Tenha em mente que esses ativos serão exibidos em uma grande variedade de cores de plano de fundo. 

![ativos sem fundo e com fundo](images/assetguidance22.png)

Aqui estão as superfícies que usam os ativos de ícone do aplicativo unplated:
* Barra de tarefas e miniatura da barra de tarefas (área de trabalho)
* Lista de atalhos da barra de tarefas
* Visão de tarefas
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Dimensionamento unplated e destino

Aqui estão as recomendações de tamanho para ativos com base no destino, em escala de 100%:

![dimensionamento de ativos baseado em destino na escala de 100%](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Mais informações sobre ativos de tela inicial
Para obter mais informações sobre as telas de abertura, consulte o [artigo de telas de abertura UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Mais informações sobre ativos de logotipo de notificação

Quando você usa o gerador de ativos para gerar todos os ativos que você precisa, há um motivo por que ele não gera logotipos do selo por padrão: eles são muito diferentes dos outros ativos do aplicativo. O logotipo de notificação é uma imagem de status que aparece nas notificações e em blocos do aplicativo. 

Para obter mais informações, consulte o [notificação notificações para o artigo de aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalizando o preenchimento de ativo

Por padrão, o gerador de ativos do Visual Studio aplica preenchimento recomendado para qualquer imagem. Se suas imagens já contêm o preenchimento ou se desejar que as imagens de sangramento completo que vão até o fim do bloco, você pode desativar esse recurso desmarcando os **aplicar recomendado preenchimento** caixa de seleção. 

### <a name="tile-padding-recommendations"></a>Recomendações de preenchimento lado a lado
Se você quiser fornecer sua própria preenchimento, aqui estão nossas recomendações para blocos. 

Há 4 tamanhos de bloco: pequeno (71 x 71), média (150 x 150), todo (310 x 150) e grande (310 x 310). 

Cada ativo de bloco tem o mesmo tamanho do bloco no qual ele é colocado.

![Lado a lado mostrando o total de sangramento](images/app-icons/tile-assets1.png)

Se você não quiser que seu ícone para estender para a borda do bloco, você pode usar os pixels transparentes em seu ativo para criar o preenchimento. 

![bloco e fundo](images/assetguidance05.png)

Para blocos pequenos, limite a largura do ícone e a altura a 66% do tamanho do bloco:

![razões de dimensionamento de bloco pequeno](images/assetguidance09.png)

Para blocos médios, limite a largura do ícone a 66% e a altura a 50% do tamanho do bloco. Isso evita a sobreposição de elementos na barra de identidade visual:

![razões de dimensionamento de bloco médio](images/assetguidance10.png)

Para blocos largos, limite a largura do ícone a 66% e a altura a 50% do tamanho do bloco. Isso evita a sobreposição de elementos na barra de identidade visual:

![razões de dimensionamento de bloco largo](images/assetguidance11.png)

Para blocos grandes, limite a largura do ícone a 66% e a altura a 50% do tamanho do bloco:

![razões de tamanho de bloco grande](images/assetguidance12.png)

Alguns ícones foram projetados para serem orientados horizontal ou verticalmente, e outros têm formas mais complexas que os impedem de se ajustar de maneira centralizada dentro das dimensões desejadas. Ícones que aparentam estar centralizados podem estar pensos para um lado. Nesse caso, partes de um ícone podem travar fora da superfície recomendada, pois ele ocupa a mesma espessura visual de um ícone centralizado por igual:

![três ícones centralizados](images/assetguidance13.png)

Com ativos com sangramento completo, leve em conta elementos que interajam dentro das margens e das bordas dos blocos. Mantenha margens de pelo menos 16% da altura ou da largura do bloco. Essa porcentagem representa duas vezes a largura das margens nos tamanhos de bloco menores:

![bloco com sangramento completo e margens](images/assetguidance14.png)

Neste exemplo, as margens estão muito apertadas:

![bloco de sangramento completo com margens muito pequenas](images/assetguidance15.png)









