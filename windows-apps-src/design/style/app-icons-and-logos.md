---
author: mijacobs
Description: How to create app icons/logos that represent your app in the Start menu, app tiles, the taskbar, the Microsoft Store, and more.
title: Logotipos e ícones do aplicativo
template: detail.hbs
ms.author: mijacobs
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 136a52cedd7d4b0599adaff08fd0860260da4ce3
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3824430"
---
# <a name="app-icons-and-logos"></a>Logotipos e ícones do aplicativo 

Cada aplicativo tem um logotipo/ícone que o representa, e que aparece em vários locais no shell do Windows: 

:::row:::
    :::column:::
        * Na barra de título da janela do aplicativo * a lista de aplicativos no menu Iniciar * o Gerenciador de tarefas e tarefas * lado a lado do aplicativo * tela do aplicativo * no armazenamento Microsoft :::column-end:::
    :::column:::
        ![início e blocos do windows 10](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Este artigo aborda as Noções básicas da criação de ícones do aplicativo, como usar Visual Studio para gerenciá-los e como gerenciá-los manualmente, caso seja necessário.
 
(Este artigo é especificamente para os ícones que representam o próprio aplicativo; para obter orientações gerais do ícone, consulte o artigo de [ícones](icons.md) .)

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de ícones, locais e os fatores de escala

Por padrão, o Visual Studio armazena seus ativos de ícone em um subdiretório de ativos. Aqui está uma lista dos diferentes tipos de ícones, em que elas aparecem, e o que são chamados. 

| Nome do ícone | Aparece em | Nome do arquivo ativo |
| ---      | ---        | --- |
| Lado a lado pequena | Menu Iniciar |  SmallTile.png  |
| Lado a lado média |Menu Iniciar, Microsoft Store listing\ *  |  Square150x150Logo.PNG |
| Lado a lado grande  | Menu Iniciar   | Wide310x150Logo.PNG |
| Lado a lado grande   | Menu Iniciar, Microsoft Store listing\ * |  LargeTile.png  |
| Ícone de aplicativo | Lista de aplicativos no menu Iniciar, a barra de tarefas, Gerenciador de tarefas | Square44x44Logo.PNG |
| Tela inicial | Tela do aplicativo | SplashScreen.png  |
| Logotipo do emblema | Lado a lado do aplicativo | BadgeLogo.png  |
| Logotipo do logotipo/armazenamento de pacote | Instalador do aplicativo, o Centro de desenvolvimento, a opção "Relatar um aplicativo" da loja, a opção "Escrever uma revisão" no armazenamento | StoreLogo.png  |

\ * Usado a menos que escolha [vídeo carregado somente imagens no armazenamento](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para garantir que esses ícones fiquem mais nítidas em cada tela, você pode criar várias versões do mesmo ícone de exibição diferentes fatores de escala. 

O fator de escala determina o tamanho dos elementos da interface do usuário, como texto. Fatores de escala variam de 100% a 400%. Valores maiores criam mais elementos de interface do usuário, tornando-as mais fáceis de ver em exibições de alta DPI. 

:::row:::
    :::column:::
        O Windows automaticamente define o fator de escala para cada vídeo com base em seu DPI (pontos por polegada) e a distância de exibição do dispositivo. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Como os ativos de ícone de aplicativo são bitmaps e bitmaps não são satisfatoriamente dimensionáveis, recomendamos fornecendo uma versão de cada ativo de ícone para cada fator de escala: 100%, 125%, 150%, 200% e 400%. Há muitos ícones! Fortunatly, Visual Studio fornece uma ferramenta que facilita a gerar e atualizar esses ícones. 

## <a name="microsoft-store-listing-image"></a>Imagem de listagem Microsoft Store

"Como especificar imagens para listagem do aplicativo no Microsoft Store?"

Por padrão, podemos usar algumas imagens de seus pacotes no armazenamento, conforme descrito na tabela na parte superior desta página (juntamente com outras [imagens fornecidas durante o processo de envio](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). No entanto, você tem a opção de impedir que a loja usando as imagens de logotipo em pacotes do aplicativo ao exibir sua listagem para clientes Windows 10 (incluindo Xbox) e em vez disso, ter a loja usar apenas as imagens que você carregar. Isso lhe dá mais controle sobre a aparência do seu aplicativo em várias exibições em todo o armazenamento. (Observe que, se o produto oferece suporte a versões anteriores do sistema operacional, os clientes ainda poderão ver imagens de pacotes, mesmo se você usar essa opção.) Você pode fazer isso na seção **logotipos de loja** da etapa do processo de envio de **listagem de armazenamento** .

![Especificar logotipos de armazenamento durante o processo de envio do aplicativo](images/app-icons/storelogodisplay.png)

Quando você marcar esta caixa, aparece uma nova seção denominada **armazenamento de exibir imagens** . Aqui, você pode carregar 3 tamanhos de imagem que o armazenamento será usado no lugar das imagens de logotipo de pacotes do aplicativo: 300 x 300, 150 x 150 e 71 x 71 pixels. Apenas o tamanho de 300 x 300 é necessário, embora recomendamos o fornecimento de todos os 3 portes.

Para obter mais informações, consulte [exibição carregados somente imagens de logotipo na loja](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Gerenciar ícones de aplicativo com o Designer do Visual Studio de manifesto

Visual Studio fornece uma ferramenta muito útil para gerenciar os ícones de aplicativo chamados o **Manifesto do Designer**. 

> Se você não tiver o Visual Studio 2017, várias versões estão disponíveis, incluindo uma versão gratuita, (Visual Studio 2017 Community Edition), e as outras versões oferecem avaliações gratuitas. Você pode baixá-los aqui:[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Para abrir o Designer de manifesto:
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. use Visual Studio para abrir um projeto UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. no **Solution Explorer**, clique duas vezes no arquivo package.appmanifest.
    :::column-end:::
    :::column:::
        ![O Visual Studio Designer de manifesto de 2017](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            O Visual Studio exibe o criador do manifesto.
    :::column-end:::
    :::column:::
            ![Na guia ativos visuais](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Clique na guia **Visual ativos** . :::column-end:::
    :::column:::
        ![Na guia ativos visuais](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Gerando todos os ativos de uma vez

O primeiro item de menu na guia **Visual ativos** , **Todos os ativos Visual**, exatamente o que seu nome sugere: gera todas as necessidades de seu aplicativo com o simples pressionar de um botão de recursos visuais.

![Gerar todos os ativos de visual Visaul Studio](images/app-icons/all-visual-assets.png)

Tudo o que você precisa fazer é a fonte de uma única imagem e Visaul Studio gerará pequena lado a lado, lado a lado média, ladrilho grande, grande lado a lado, ladrilho grande, ícone de aplicativo, tela de abertura e ativos de logotipo para cada fator de escala do pacote.

Para gerar todos os ativos de uma só vez:
1. Clique o **...** ao lado do campo de **origem** e selecione a imagem que deseja usar. Se você estiver usando uma imagem de bitmap, verifique se é pelo menos 400 x 400 pixels para que você obtenha resultados nítidos. Imagens baseadas em vetor funcionam melhor; Visual Studio permite que você use AI (Adobe Illustrator) e arquivos PDF. 
2. (Opcional). Na seção **Configurações de vídeo** , configure estas opções:

    a.  **Nome curto**: Especifique um nome curto para o seu aplicativo.

    b.  **Mostrar nome**: indique se deseja exibir o nome curto em peças de médio, grande ou grande. 

    c. **Plano de fundo lado a lado**: especifique o valor hexadecimal ou um nome de cor para a cor de plano de fundo lado a lado. Por exemplo, `#464646`. O valor padrão é `transparent`.

    d. **Plano de fundo de tela Spash**: especifique o nome de cor ou valor hexadecimal para o plano de fundo de tela de spash. 

3. Clique em **Gerar**. 

O Visual Studio gera os arquivos de imagem e os adiciona ao projeto. Se você quiser alterar os ativos, bastaria repetir o processo. 

Ícone em escala ativos siga essa convenção de nomenclatura de arquivo:

o *nome do arquivo*- escala -*fator de escala*PNG

Por exemplo,

Square150x150Logo de escala de 100.png, Square150x150Logo de escala de 200.png, Square150x150Logo de escala de 400.png

Observe que o Visual Studio não gera um logotipo do emblema por padrão. Isso acontece porque o logotipo do emblema é exclusivo e provavelmente não deve coincidir com os outros ícones do aplicativo. Para obter mais informações, consulte as [notificações de crachá do artigo de aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Mais informações sobre recursos de ícone de aplicativo
O Visual Studio irá gerar todos os ativos de ícone de aplicativo necessários para seu projeto, mas se desejar personalizá-los, é útil para entender como eles são diferentes dos outros ativos de aplicativo. 

O recurso de ícone do aplicativo aparece em vários locais: a barra de tarefas do Windows, o modo de exibição de tarefa, ALT + TAB e o canto inferior direito de ladrilhos do início. Como o ativo de ícone do aplicativo aparece em tantos locais, ele tem alguns dimensionamento adicional e plating opções não tem outros ativos: ativos de "tamanho de destino" e "unplated" ativos. 

### <a name="target-size-app-icon-assets"></a>Ativos de ícone de aplicativo de destino de tamanho
Além dos tamanhos de fator de escala padrão ("Square44x44Logo.scale-400.png"), também recomendamos criar ativos "tamanho de destino". Chamamos o tamanho de destino desses ativos, porque se destinam a tamanhos específicos, como 16 pixels, em vez de fatores de escala específica, como 400. Tamanho de destino ativos são para superfícies que não usam o sistema de estabilização de dimensionamento:

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

\ *, No mínimo, é recomendável fornecer esses tamanhos. 

Você não precisa adicionar preenchimento a esses ativos; o Windows adicionará o preenchimento, se necessário. Esses ativos devem levar em conta uma superfície mínima de 16 pixels. 

Aqui está um exemplo desses ativos conforme eles aparecem em ícones da barra de tarefas do Windows:

![ativos na barra de tarefas do Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ativos de unplated
Por padrão, o Windows usa um ativo com base no destino sobre uma placa traseira colorido por padrão. Se desejar, você pode fornecer um ativo unplated com base no destino. "Unplated" significa que o ativo será exibido em um plano de fundo transparente. Tenha em mente que esses ativos serão exibidas em uma variedade de cores de plano de fundo. 

![ativos sem fundo e com fundo](images/assetguidance22.png)

Aqui estão as superfícies que usam recursos de ícone de aplicativo unplated:
* Barra de tarefas e miniatura da barra de tarefas (área de trabalho)
* Lista de atalhos da barra de tarefas
* Visão de tarefas
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Destino e dimensionamento unplated

Aqui estão as recomendações de tamanho de ativos baseados no destino, em 100% da escala:

![dimensionamento de ativos baseado em destino na escala de 100%](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Mais informações sobre ativos da tela de abertura
Para obter mais informações sobre as telas de splash, consulte o [artigo de telas de splash UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Mais sobre ativos de logotipo de emblema

Quando você usar o gerador de ativo para gerar todos os ativos necessários, há um motivo por que ele não gerar crachá logotipos por padrão: eles são muito diferentes dos outros ativos de aplicativo. O logotipo do emblema é uma imagem de status será exibido nas notificações e em peças do aplicativo. 

Para obter mais informações, consulte as [notificações de crachá do artigo de aplicativos UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalizando o enchimento de ativo

Por padrão, o gerador de ativo do Visual Studio aplica preenchimento recomendado para qualquer imagem. Se as imagens já contenham preenchimento ou quiser imagens de sangramento total que se estendem até o final da peça, você pode desativar esse recurso desmarcando a caixa de seleção **Aplicar recomendado enchimento** . 

### <a name="tile-padding-recommendations"></a>Recomendações de preenchimento lado a lado
Se você quiser fornecer sua própria enchimento, aqui estão nossas recomendações para lado a lado. 

Há 4 tamanhos: pequeno (71 x 71), médio (150 x 150), wide (310 x 150) e grande (310 x 310). 

Cada ativo de bloco tem o mesmo tamanho do bloco no qual ele é colocado.

![Lado a lado mostrando completo de sangramento](images/app-icons/tile-assets1.png)

Se não quiser que o ícone para estender até a borda do lado, você pode usar os pixels transparentes em seu ativo para criar o preenchimento. 

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









