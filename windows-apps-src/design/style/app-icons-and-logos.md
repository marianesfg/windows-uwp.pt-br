---
Description: How to create app icons/logos that represent your app in the Start menu, app tiles, the taskbar, the Microsoft Store, and more.
title: Ícones e logotipos de apps
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7083152efb4cf871f8abebf6d2970d2da4ba06e9
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485826"
---
# <a name="app-icons-and-logos"></a>Ícones e logotipos de apps 

Todo aplicativo tem um logotipo/ícone que representa a ele, e esse ícone é exibido em vários locais no shell do Windows: 

:::row:::
    :::column:::
        * Barra de título da janela do aplicativo
        * A lista de aplicativo no menu Iniciar
        * O Gerenciador de barra de tarefas e tarefas
        * Blocos do seu aplicativo
        * Tela inicial do seu aplicativo
        * Na Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Este artigo aborda as Noções básicas de criação de ícones de aplicativos, como usar o Visual Studio para gerenciá-los e como gerenciá-las manualmente, caso seja necessário.
 
(Este artigo é especificamente para ícones que representam o próprio aplicativo; para obter orientações gerais do ícone, consulte o artigo de [ícones](icons.md) ).

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de ícone, locais e fatores de escala

Por padrão, o Visual Studio armazena os ativos de ícone em um subdiretório de ativos. Aqui está uma lista dos diferentes tipos de ícones, onde eles são exibidos, e o que são chamados. 

| Nome do ícone | Aparece em | Nome do arquivo de ativo |
| ---      | ---        | --- |
| Bloco pequeno | Menu Iniciar |  SmallTile.png  |
| Bloco médio |Menu Iniciar, Microsoft Store listing\ *  |  Square150x150Logo.PNG |
| Bloco largo  | Menu Iniciar   | Wide310x150Logo.PNG |
| Bloco grande   | Menu Iniciar, Microsoft Store listing\ * |  LargeTile.png  |
| Ícone de aplicativo | Lista de aplicativos no menu Iniciar, a barra de tarefas, Gerenciador de tarefas | Square44x44Logo.PNG |
| Tela inicial | Tela inicial do aplicativo | SplashScreen  |
| Logotipo do selo | Blocos do seu aplicativo | BadgeLogo.png  |
| Logotipo do logotipo/armazenamento de pacote | Instalador de aplicativo, Partner Center, a opção "Relatar um aplicativo" na loja, a opção "Escrever uma análise" na loja | StoreLogo.png  |

\ * Usado, a menos que você escolha a [exibição carregados somente imagens na loja](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para garantir que esses ícones nítidos em cada tela, você pode criar várias versões do mesmo ícone de fatores de escala de exibição diferentes. 

O fator de escala determina o tamanho dos elementos de interface do usuário, como texto. Intervalo de fatores de escala de 100% a 400%. Valores maiores criam maiores elementos de interface do usuário, tornando-o mais fácil de ver em telas de DPI alto. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Como ativos de ícone do aplicativo são bitmaps e bitmaps corrigidos sem escala bem, é recomendável fornecer uma versão de cada ativo de ícone para cada fator de escala: 100%, 125%, 150%, 200% e 400%. Isso é muito ícones! Fortunatly, o Visual Studio fornece uma ferramenta que facilita a gerar e atualizar esses ícones. 

## <a name="microsoft-store-listing-image"></a>Imagem de listagem da Microsoft Store

"Como especificar imagens de listagem do meu aplicativo na Microsoft Store?"

Por padrão, usamos algumas das imagens de seus pacotes na loja, conforme descrito na tabela na parte superior desta página (juntamente com outras [imagens que você forneça durante o processo de envio](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). No entanto, você tem a opção de impedir que a loja usando as imagens de logotipo nos pacotes do aplicativo ao exibir a listagem para clientes no Windows 10 (incluindo o Xbox) e, em vez disso, tenha a Store use apenas as imagens enviadas. Isso oferece mais controle sobre a aparência do aplicativo em várias telas em toda a loja. (Observe que, se seu produto oferece suporte a versões anteriores do sistema operacional, os clientes ainda poderão ver imagens dos seus pacotes, mesmo se você usa essa opção). Você pode fazer isso na seção **logotipos da loja** da **listagem da Store** etapa do processo de envio.

![Especificando os logotipos da loja durante o processo de envio de aplicativo](images/app-icons/storelogodisplay.png)

Quando você marcar essa caixa, aparece uma nova seção chamada **loja exibir imagens** . Aqui, você pode carregar 3 tamanhos de imagem que usará a loja no lugar de imagens de logotipo dos pacotes do aplicativo: 300 x 300, 150 x 150 e 71 x 71 pixels. Somente o tamanho de 300 x 300 é necessário, embora seja recomendável fornecer os tamanhos de 3.

Para obter mais informações, consulte a [exibição carregados apenas as imagens de logotipo na loja](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Gerenciamento de ícones de aplicativos com o Designer do Visual Studio de manifesto

Visual Studio fornece uma ferramenta muito útil para gerenciar seus ícones de aplicativo chamados o **Designer de manifesto**. 

> Se você ainda não tiver o Visual Studio 2017, existem várias versões disponíveis, incluindo uma versão gratuita, (Visual Studio 2017 Community Edition), e as outras versões oferecem avaliações gratuitas. Você pode baixá-los aqui:[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


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
        2. No **Gerenciador de soluções**, clique duas vezes no arquivo Package.appmxanifest.
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
        3. Clique na guia **Ativos visuais** .
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Geração de todos os ativos ao mesmo tempo

O primeiro item de menu na guia **Ativos visuais** , **Todos os ativos visuais**, faz exatamente o que seu nome sugere: gera cada ativo visual que seu aplicativo precisa com o pressionamento de um botão.

![Gerar todos os ativos visuais no Visual Studio](images/app-icons/all-visual-assets.png)

Tudo que você precisa fazer é fornecer uma única imagem, e o Visual Studio irá gerar o bloco pequeno, bloco médio, grande bloco, bloco largo, bloco grande, ícone do aplicativo, tela inicial e ativos de logotipo para cada fator de escala do pacote.

Para gerar todos os ativos ao mesmo tempo:
1. Clique o Stores **** ao lado do campo de **origem** e selecione a imagem que você deseja usar. Se você estiver usando uma imagem de bitmap, verifique se que ele é pelo menos 400 por 400 pixels para que você obterá resultados afiados. Imagens com base em vetor funcionam melhor; O Visual Studio permite que você use AI (Adobe Illustrator) e arquivos PDF. 
2. (Opcional). Na seção **Configurações de exibição** , configure essas opções:

    a.  **Nome curto**: Especifique um nome curto para o seu aplicativo.

    b.  **Mostrar o nome**: indicar se você deseja exibir o nome curto em blocos de médio, largo ou grande. 

    c. **Plano de fundo do bloco**: especificar o valor hex ou um nome de cor para a cor de fundo do bloco. Por exemplo, `#464646`. O valor padrão é `transparent`.

    d. **Plano de fundo de tela Spash**: especificar o nome de cor ou valor hexadecimal para o plano de fundo de tela spash. 

3. Clique em **Gerar**. 

O Visual Studio gera os arquivos de imagem e as adiciona ao projeto. Se você quiser alterar os ativos, basta repetir o processo. 

Ativos de ícone dimensionadas seguem Esta convenção de nomenclatura de arquivo:

*nome do arquivo*- scale -*fator de escala*PNG

Por exemplo,

Square150x150Logo de escala de 100.png, Square150x150Logo de escala de 200.png, Square150x150Logo de escala de 400.png

Observe que o Visual Studio não gera um logotipo de selo por padrão. Isso ocorre porque o logotipo de selo é exclusivo e provavelmente não deve coincidir com seus outros ícones do aplicativo. Para obter mais informações, consulte as [notificações de selo para artigo de aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Mais informações sobre os ativos de ícone de aplicativo
Visual Studio gerará todos os ativos de ícone de aplicativo exigidos pelo seu projeto, mas se você quiser personalizá-las, ele ajuda a entender como eles são diferentes dos outros ativos de aplicativo. 

O ativo de ícone do aplicativo aparece em muitos lugares: na barra de tarefas do Windows, o modo de exibição de tarefa, ALT + TAB e o canto inferior direito dos blocos em Iniciar. Como o ativo de ícone do aplicativo aparece em tantos locais, ele tem alguns dimensionamento adicional e plating opções os outros ativos não têm: ativos "tamanho" e "sem fundo" ativos. 

### <a name="target-size-app-icon-assets"></a>Ativos de ícone do aplicativo de tamanho
Os tamanhos de fator de escala padrão ("square44x44logo. Scale-400.png"), além de também recomendamos criar ativos "tamanho de destino". Chamamos tamanho esses ativos porque eles direcionar tamanhos específicos, como 16 pixels, em vez de fatores de escala específica, como 400. Ativos de tamanho de destino são para superfícies que não usam o sistema de 1X dimensionamento:

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

\ * No mínimo, é recomendável fornecer esses tamanhos. 

Você não precisa adicionar preenchimento a esses ativos; o Windows adicionará o preenchimento, se necessário. Esses ativos devem levar em conta uma superfície mínima de 16 pixels. 

Aqui está um exemplo desses ativos conforme eles aparecem em ícones da barra de tarefas do Windows:

![ativos na barra de tarefas do Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ativos sem fundo
Por padrão, o Windows usa um ativo baseados no destino sobre um fundo colorido por padrão. Se você quiser, você pode fornecer um ativo sem fundo baseados no destino. "Sem fundo" significa que o ativo será exibido em um plano de fundo transparente. Tenha em mente que esses ativos serão exibidos ao longo de uma variedade de cores de fundo. 

![ativos sem fundo e com fundo](images/assetguidance22.png)

Aqui estão as superfícies que usar ativos de ícone do aplicativo sem fundo:
* Barra de tarefas e miniatura da barra de tarefas (área de trabalho)
* Lista de atalhos da barra de tarefas
* Visão de tarefas
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Destino e dimensionamento sem fundo

Aqui estão as recomendações de tamanho para ativos baseados no destino, em escala de 100%:

![dimensionamento de ativos baseado em destino na escala de 100%](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Mais sobre os ativos de tela inicial
Para obter mais informações sobre telas iniciais, consulte o [artigo de telas UWP inicial](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Mais sobre os ativos de logotipo do selo

Quando você usa o gerador de ativo para gerar todos os ativos que você precisa, há um motivo por que ela não gerar o selo logotipos por padrão: eles são muito diferentes dos outros ativos de aplicativo. O logotipo de selo é uma imagem de status que aparece em notificações e blocos do aplicativo. 

Para obter mais informações, consulte as [notificações de selo para artigo de aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalizando o preenchimento de ativo

Por padrão, o gerador de ativo Visual Studio aplica preenchimento recomendado para qualquer imagem. Se suas imagens já contêm preenchimento ou se quiser imagens sangramento completo que se estender até o final do bloco, você pode desativar esse recurso desmarcando a caixa de seleção **Aplicar recomendado preenchimento** . 

### <a name="tile-padding-recommendations"></a>Recomendações de preenchimento de bloco
Se você quiser fornecer sua própria preenchimento, aqui estão nossas recomendações de blocos. 

Há 4 tamanhos de bloco: pequeno (71 x 71), médio (150 x 150), todo o (310 x 150) e grandes (310 x 310). 

Cada ativo de bloco tem o mesmo tamanho do bloco no qual ele é colocado.

![Sangramento completo do bloco mostrando](images/app-icons/tile-assets1.png)

Se você não quiser seu ícone para se estender até a borda do bloco, você pode usar pixels transparentes em seu ativo para criar o preenchimento. 

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









