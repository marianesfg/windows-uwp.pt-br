---
title: Adicionar uma tela inicial
description: Defina a imagem da tela inicial e a cor da tela de fundo usando o Microsoft Visual Studio.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 956e4050e3077ac827cf8107470698b42878a5e1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370870"
---
# <a name="add-a-splash-screen"></a>Adicionar uma tela inicial

Defina a imagem da tela inicial e a cor da tela de fundo usando o Microsoft Visual Studio.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Defina a imagem da tela inicial e a cor da tela de fundo no Visual Studio

Quando você usa um modelo do Visual Studio para criar o seu aplicativo, uma imagem padrão é adicionada ao seu projeto e definida como a imagem da tela inicial. A cor de tela de fundo da tela inicial muda para um padrão de cinza claro. Se você quiser mudar a imagem ou cor padrão da tela inicial de seu aplicativo, siga estas etapas:

1. Abra o seu projeto de aplicativa do Plataforma Universal do Windows (UWP) no Visual Studio.
2. Em **Solution Explorer**, abra o arquivo "Package.appxmanifest". Você também pode abrir esse arquivo na barra de menus, escolhendo **Project** &gt; **Store** &gt; **Editar manifesto do aplicativo**.
3. Abra a guia **Ativos Visuais** e selecione **Tela inicial** no painel **Todos os ativos de imagem** no lado esquerdo da janela "Package.appxmanifest". Se você estiver alterando sua tela inicial pela primeira vez, você verá o "ativos\\SplashScreen.png" caminho na **tela inicial** campo.

    A tela a seguir mostra a janela "Package.appxmanifest" no Visual Studio. Dependendo do tipo de projeto, você verá um conjunto de ativos visuais ligeiramente diferente.

    ![uma captura de tela da janela "package.appxmanifest" no visual studio 2017](images/appmanifest.png)

    Se você abrir "Package.appxmanifest" em um editor de texto, o [**elemento SplashScreen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) aparecerá como filho do [**elemento VisualElements**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements). O marcador da tela inicial padrão no arquivo de manifesto aparece assim eu editor de texto:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Para selecionar uma nova imagem de tela inicial para um aplicativo UWP, pressione o botão com reticências que aparece próximo ao rótulo **1240 x 600 px** abaixo de **Ativos dimensionados**. Escolha a imagem de 1240 x 600 pixels (.png, .jpg, or .jpeg) que gostaria de usar como imagem da sua tela inicial.

    **Importante**  a imagem de tela de abertura que você escolher deve ser pixels de 620 x 300 usando 1X fator de escala. Além disso, ao projetar sua tela inicial, observe que ela é menor do que a tela e centralizada. Ela não preenche a tela como uma tela inicial de um aplicativo da Store do Windows Phone faz.

5. Para selecionar uma nova imagem de tela inicial para um aplicativo da Store do Windows Phone, pressione o botão com reticências que aparece próximo ao rótulo **1152 x 1920 px** abaixo de **Ativos dimensionados**. Escolha a imagem de 1152 x 1920 pixels (.png, .jpg, or .jpeg) que gostaria de usar como imagem da sua tela inicial.

    **Importante**  a imagem de tela de abertura que você escolher deve ser 1920 x 1152 pixels que é o tamanho correto para um 2,4 x fator de escala. Se esse for o único ativo que você fornecer, então, ele será reduzido para os fatores de dimensionamento 1,4 x e 1x.

6. No campo **Cor da tela de fundo** na seção **Tela inicial**, defina a cor da tela de fundo exibida na imagem da sua tela inicial. Você pode inserir o nome de uma cor ou '\#' e o valor hexadecimal da cor. Para uma lista de nomes de cores disponíveis, consulte [**elemento SplashScreen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen). Definir uma cor da tela de fundo para sua tela de inicial é opcional. Se você não especificar uma cor para um aplicativo UWP, a cor do plano de fundo de tela inicial padrão é cinza-claro (valor hexadecimal \#464646). Essa é a mesma cor que a cor da tela de fundo do **Bloco** (consulte o campo **Cor da tela de fundo** da seção **Imagens e logotipos do bloco** na guia **Ativos visuais**). Se você não especificar uma cor para um aplicativo do Windows Phone, ou configurá-lo como "transparente", a cor de tela de fundo da tela inicial será transparente.

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Se o aplicativo levar algum tempo para ser carregado, considere a inclusão de uma tela inicial estendida. Para obter orientação passo a passo, veja [Create a customized splash screen](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Criar uma tela inicial personalizada](create-a-customized-splash-screen.md)
* [Referência de esquema de manifesto de pacote: Elemento SplashScreen](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [Classe Windows.ApplicationModel.Activation.SplashScreen](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)