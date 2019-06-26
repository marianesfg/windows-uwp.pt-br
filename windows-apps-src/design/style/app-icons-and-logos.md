---
Description: Como criar ícones/logotipos para aplicativos que representem seu aplicativo no menu Iniciar, em blocos do aplicativo, na barra de tarefas, na Microsoft Store, entre outros.
title: Ícones e logotipos de aplicativos
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 31b90866a0f612fb8f488d11e7d989380f14da99
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63784880"
---
# <a name="app-icons-and-logos"></a>Ícones e logotipos de aplicativos 

Cada aplicativo tem um ícone/logotipo que o representa, e esse ícone é exibido em vários locais no shell do Windows: 

:::row:::
    :::column:::
        * A barra de título da janela do aplicativo
        * A lista de aplicativos no menu Iniciar
        * A barra de tarefas e o gerenciador de tarefas
        * Blocos do aplicativo
        * Tela inicial do aplicativo
        * Na Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Este artigo aborda os conceitos básicos para a criação de ícones de aplicativos, como usar o Visual Studio para gerenciá-los e como gerenciá-los manualmente, caso seja necessário.
 
Este artigo aborda especificamente ícones que representam o aplicativo; para obter orientações gerais sobre ícones, confira o artigo sobre [ícones](icons.md).

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de ícones, locais e fatores de dimensionamento

Por padrão, o Visual Studio armazena seus ativos de ícones em um subdiretório de ativos. Veja a seguir uma lista dos diferentes tipos de ícones, onde eles aparecem e como são chamados. 

| Nome do ícone | Aparece no/na | Nome do arquivo de ativo |
| ---      | ---        | --- |
| Bloco pequeno | Menu Iniciar |  SmallTile.png  |
| Bloco médio |Menu Iniciar, listagem da Microsoft Store\*  |  Square150x150Logo.png |
| Bloco largo  | Menu Iniciar   | Wide310x150Logo.png |
| Bloco grande   | Menu Iniciar, listagem da Microsoft Store\* |  LargeTile.png  |
| Ícone de aplicativo | Lista de aplicativos no menu Iniciar, na barra de tarefas, no gerenciador de tarefas | Square44x44Logo.png |
| Tela inicial | Tela inicial do aplicativo | SplashScreen.png  |
| Logotipo de notificação | Blocos do aplicativo | BadgeLogo.png  |
| Logotipo do pacote/Logotipo da Store | Instalador de aplicativo, Partner Center, a opção "Relatar um aplicativo" na Store, a opção "Escrever uma resenha" na Store | StoreLogo.png  |

\* Usado a menos que você escolha [exibir apenas imagens carregadas na Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para garantir que esses ícones fiquem nítidos em qualquer tela, você pode criar diversas versões do mesmo ícone para diferentes fatores de escala de exibição. 

O fator de escala determina o tamanho dos elementos da interface do usuário, como o texto. Os fatores de escala variam entre 100% e 400%. Valores maiores criam elementos de interface do usuário maiores, tornando a visualização desses elementos mais fácil nas exibições de alto DPI. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Como os ativos de ícone de aplicativos são bitmaps e estes não possuem um bom dimensionamento, recomendamos providenciar uma versão de cada ativo de ícone para cada fator de escala: 100%, 125%, 150%, 200% e 400%. São muitos ícones! Felizmente, o Visual Studio fornece uma ferramenta que facilita a criação e a atualização desses ícones. 

## <a name="microsoft-store-listing-image"></a>Imagem para os detalhes do aplicativo na Microsoft Store

"Como especificar imagens para os detalhes do meu aplicativo na Microsoft Store?"

Por padrão, usamos algumas imagens de seus pacotes na Store, conforme descrito na tabela na parte superior desta página (em conjunto com outras [imagens que você fornece durante o processo de envio](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). No entanto, você também pode impedir que a Store use as imagens do logotipo nos pacotes do aplicativo ao exibir os detalhes para os clientes no Windows 10 (inclusive no Xbox) e, em vez disso, permitir que a Store use apenas as imagens que você carregar. Isso oferece mais controle sobre a aparência do aplicativo em várias telas, em toda a Store. Observação: se o seu produto der suporte a versões anteriores do sistema operacional, os clientes poderão ver as imagens de seus pacotes, mesmo se você usar esta opção. Você pode fazer isso na seção **Logotipos da Store** na etapa de **Listagem da Store** do processo de envio.

![Especificando logotipos da Store durante o processo de envio do aplicativo](images/app-icons/storelogodisplay.png)

Ao marcar esta caixa, uma nova seção chamada **Imagens de exibição da Store** é exibida. Nela você poderá carregar três imagens de tamanhos diferentes que a Store usará no lugar das imagens do logotipo dos pacotes do seu aplicativo: 300 x 300, 150 x 150 e 71 x 71 pixels. Somente a imagem de tamanho 300 x 300 é obrigatória, embora seja recomendável fornecer as três imagens, nos três tamanhos.

Para saber mais, confira as informações sobre como [exibir apenas imagens do logotipo carregadas na Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Gerenciar ícones de aplicativo com o Designer de Manifesto do Visual Studio

O Visual Studio fornece uma ferramenta útil para gerenciar os ícones do aplicativo, chamada **Designer de Manifesto**. 

> Se você ainda não tem o Visual Studio 2017, saiba que há diversas versões disponíveis, incluindo uma versão gratuita (Visual Studio 2017 Community Edition); e as demais versões oferecem avaliações gratuitas. Você pode baixá-las aqui: [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Para iniciar o Designer de Manifesto:
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Use o Visual Studio para abrir o projeto UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. No **Gerenciador de Soluções**, clique duas vezes no arquivo Package.appmxanifest.
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
        3. Clique na guia **Ativos visuais**.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Gerar todos os ativos de uma vez

O primeiro item do menu na guia **Ativos Visuais**, **Todos os Ativos Visuais**, faz exatamente o que seu nome diz: gera todos os ativos visuais que seu aplicativo precisa com o toque de um botão.

![Gerar todos os ativos visuais no Visual Studio](images/app-icons/all-visual-assets.png)

Tudo o que você precisa fazer é fornecer uma única imagem, e o Visual Studio gera o bloco pequeno, o bloco médio, o bloco grande, o bloco largo, o ícone do aplicativo, a tela inicial e os ativos do logotipo do pacote em cada fator de escala.

Para gerar todos os ativos de uma vez:
1. Clique nas **...** ao lado do campo **Fonte** e selecione a imagem que você deseja usar. Se você estiver usando uma imagem bitmap, verifique se ela é pelo menos 400 por 400 pixels para que você obtenha resultados nítidos. As imagens vetoriais funcionam melhor; o Visual Studio permite que você use o AI (Adobe Illustrator) e arquivos PDF. 
2. Opcional. Na seção **Configurações de exibição**, configure estas opções:

    a.  **Nome Curto**:  especifique um nome curto para o seu aplicativo.

    b.  **Mostrar nome**: indique se você deseja exibir o nome curto em blocos médios, grandes ou largos. 

    c. **Tela de fundo do bloco**: especifique o valor hex. ou o nome de uma cor para a cor de tela de fundo do bloco. Por exemplo, `#464646`. O valor padrão é `transparent`.

    d. **Tela de fundo da tela inicial**: especifique o valor hex. ou o nome de uma cor para a tela de fundo da tela inicial. 

3. Clique em **Gerar**. 

O Visual Studio gera os arquivos da imagem e os adiciona ao projeto. Se você quiser alterar os ativos, basta repetir o processo. 

Os ativos de ícones dimensionados seguem a seguinte convenção de nomenclatura de arquivo:

*nome do arquivo*-escala-*fator de escala*.png

Por exemplo,

Quadrado150x150Logotipo-escala-100.png, Quadrado150x150Logotipo-escala-200.png, Quadrado150x150Logotipo-escala-400.png

Observe que o Visual Studio não gera um logotipo de notificação por padrão. Isso ocorre porque seu logotipo de notificação é exclusivo e provavelmente não deve coincidir com os outros ícones do aplicativo. Para obter mais informações, confira o [artigo sobre selos de notificação para aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Mais informações sobre ativos de ícone do aplicativo
O Visual Studio gera todos os ativos de ícone de aplicativo exigidos pelo projeto, mas se você quiser personalizá-los, é importante entender como eles são diferentes dos outros ativos de aplicativo. 

O ativo de ícone do aplicativo é exibido em muitos lugares: na barra de tarefas do Windows, na visão de tarefas, Alt+Tab, e no canto inferior direito do bloco da tela inicial. Como o ativo de ícones do aplicativo é exibido em muitos lugares, ele tem algumas opções adicionais de tamanho e fundo que os outros ativos não têm: ativos de "tamanho de destino" e "sem fundo". 

### <a name="target-size-app-icon-assets"></a>Ativos de ícone do aplicativo de tamanho de destino
Além dos tamanhos do fator da escala padrão ("Quadrado44x44Logotipo.escala-400.png"), também recomendamos a criação de ativos de "tamanho de destino". Esses ativos são chamados de “tamanho de destino” porque eles precisam ter tamanhos específicos, como 16 pixels, em vez de fatores de escala específicos, como 400. Os ativos de tamanho de destino são para superfícies que não usam o sistema de nível de ajuste pré-definido:

* Iniciar a lista de atalhos (desktop)
* Iniciar o canto inferior do bloco (desktop)
* Atalhos (desktop)
* Painel de Controle (desktop)

Veja a seguir a lista de ativos de tamanho de destino:


| Tamanho do ativo | Exemplo de nome de arquivo                  |
|------------|------------------------------------|
| 16x16\*    | Quadrado44x44Logotipo.tamanhodestino-16.png  |
| 24x24\*    | Quadrado44x44Logotipo.tamanhodestino-24.png  |
| 32x32\*    | Quadrado44x44Logotipo.tamanhodestino-32.png  |
| 48x48\*    | Quadrado44x44Logotipo.tamanhodestino-48.png  |
| 256x256\*  | Quadrado44x44Logotipo.tamanhodestino-256.png |
| 20x20      | Quadrado44x44Logotipo.tamanhodestino-20.png  |
| 30x30      | Quadrado44x44Logotipo.tamanhodestino-30.png  |
| 36x36      | Quadrado44x44Logotipo.tamanhodestino-36.png  |
| 40x40      | Quadrado44x44Logotipo.tamanhodestino-40.png  |
| 60x60      | Quadrado44x44Logotipo.tamanhodestino-60.png  |
| 64x64      | Quadrado44x44Logotipo.tamanhodestino-64.png  |
| 72x72      | Quadrado44x44Logotipo.tamanhodestino-72.png  |
| 80x80      | Quadrado44x44Logotipo.tamanhodestino-80.png  |
| 96x96      | Quadrado44x44Logotipo.tamanhodestino-96.png  |

\* Recomendamos que você forneça, pelo menos, esses tamanhos. 

Você não precisa adicionar preenchimento a esses ativos; o Windows adicionará o preenchimento, se necessário. Esses ativos devem levar em conta uma superfície mínima de 16 pixels. 

Aqui está um exemplo desses ativos conforme eles aparecem em ícones da barra de tarefas do Windows:

![ativos na barra de tarefas do Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ativos sem fundo
Por padrão, o Windows usa o ativo com tamanho do destino sobre um fundo colorido. Se você quiser, poderá fornecer um ativo com tamanho do destino sem fundo. "Sem fundo" significa que o ativo será exibido em um fundo transparente. Tenha em mente que esses ativos serão exibidos sobre uma grande variedade de cores de fundo. 

![ativos sem fundo e com fundo](images/assetguidance22.png)

Veja a seguir as superfícies que usam ativos de ícone sem fundo:
* Barra de tarefas e miniatura da barra de tarefas (área de trabalho)
* Lista de atalhos da barra de tarefas
* Visão de tarefas
* ALT+TAB

### <a name="unplated-assets-and-themes"></a>Ativos sem fundo e os temas

O tema selecionado pelo usuário determina a cor da barra de tarefas. Se o ativo sem fundo não tiver uma qualidade específica para o tema atual, o sistema verificará o contraste do ativo. Se ele tiver contraste suficiente em relação à barra de tarefas, o sistema irá usá-lo. Caso contrário, o sistema procura por uma versão do ativo com alto contraste. Se essa versão não for encontrada, o sistema desenha a forma com fundo do ativo. 


### <a name="target-and-unplated-sizing"></a>Tamanho de destino e sem fundo

Estas são as recomendações de tamanho para ativos baseados no destino, em uma escala de 100%:

![dimensionamento de ativos baseado em destino na escala de 100%](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Mais informações sobre os ativos de tela de inicial
Para obter mais informações sobre as telas iniciais, confira o [artigo sobre telas iniciais UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Mais informações sobre ativos de logotipo de notificação

Quando você usa o gerador de ativos para gerar todos os ativos que precisa, há um motivo por que ele não gera os logotipos de notificação por padrão: esses logotipos são muito diferentes dos outros ativos do aplicativo. O logotipo de notificação é uma imagem de status que aparece nas notificações e nos blocos do aplicativo. 

Para obter mais informações, confira o [artigo sobre selos de notificação para aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalizando o preenchimento de ativo

Por padrão, o gerador de ativos do Visual Studio aplica o preenchimento recomendado em todas as imagens. Se suas imagens já contiverem preenchimento ou se você desejar imagens completas com sangria, que se estendem até o fim do bloco, você poderá desativar esse recurso desmarcando a caixa de seleção **Aplicar preenchimento recomendado**. 

### <a name="tile-padding-recommendations"></a>Recomendações de preenchimento de blocos
Se desejar fornecer seu próprio preenchimento, veja a seguir nossas recomendações para os blocos. 

Há quatro tamanhos de blocos: pequeno (71 x 71), médio (150 x 150), largo (310 x 150) e grande (310 x 310). 

Cada ativo de bloco tem o mesmo tamanho do bloco no qual ele é colocado.

![Bloco apresentando a sangria completa](images/app-icons/tile-assets1.png)

Se não desejar que o ícone se estenda para a borda do bloco, você poderá usar pixels transparentes em seu ativo para criar o preenchimento. 

![bloco e fundo](images/assetguidance05.png)

Para blocos pequenos, limite a largura do ícone e a altura a 66% do tamanho do bloco:

![razões de dimensionamento de bloco pequeno](images/assetguidance09.png)

Para blocos médios, limite a largura do ícone a 66% e a altura a 50% do tamanho do bloco. Isso evita a sobreposição de elementos na barra de identidade visual:

![razões de dimensionamento de bloco médio](images/assetguidance10.png)

Para blocos largos, limite a largura do ícone a 66% e a altura a 50% do tamanho do bloco. Isso evita a sobreposição de elementos na barra de identidade visual:

![razões de dimensionamento de bloco largo](images/assetguidance11.png)

Para blocos grandes, limite a largura do ícone em 66% e a altura em 50% do tamanho do bloco:

![razões de tamanho de bloco grande](images/assetguidance12.png)

Alguns ícones foram projetados para serem orientados horizontal ou verticalmente, e outros têm formas mais complexas que os impedem de se ajustar de maneira centralizada dentro das dimensões desejadas. Ícones que aparentam estar centralizados podem estar pensos para um lado. Nesse caso, partes de um ícone podem travar fora da superfície recomendada, pois ele ocupa a mesma espessura visual de um ícone centralizado por igual:

![três ícones centralizados](images/assetguidance13.png)

Com ativos com sangramento completo, leve em conta elementos que interajam dentro das margens e das bordas dos blocos. Mantenha margens de pelo menos 16% da altura ou da largura do bloco. Essa porcentagem representa duas vezes a largura das margens nos tamanhos de bloco menores:

![bloco com sangramento completo e margens](images/assetguidance14.png)

Neste exemplo, as margens estão muito apertadas:

![bloco de sangramento completo com margens muito pequenas](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>Otimizar para temas, idiomas e outras condições específicas 

Este artigo descreveu como criar ativos para fatores de escala específicos, mas você também poderá criar ativos para uma ampla variedade de condições e combinações de condições. Por exemplo, você poderá criar ícones para telas de alto contraste, temas claros ou temas escuros. Você pode até mesmo criar ativos para idiomas específicos.

Para obter instruções, confira as informações sobre como [personalizar os recursos de acordo com o idioma, a escala, o alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)).













