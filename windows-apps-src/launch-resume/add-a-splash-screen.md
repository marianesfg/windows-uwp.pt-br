---
title: Adicionar uma tela inicial
description: Defina a imagem da tela inicial e a cor da tela de fundo usando o Microsoft Visual Studio.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 882ee548754b9fa498697a8d75a12a23f86fc9de
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7841466"
---
# <a name="add-a-splash-screen"></a>Adicionar uma tela inicial

Defina a imagem da tela inicial e a cor da tela de fundo usando o Microsoft Visual Studio.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Defina a imagem da tela inicial e a cor da tela de fundo no Visual Studio

Quando você usa um modelo do Visual Studio para criar o seu aplicativo, uma imagem padrão é adicionada ao seu projeto e definida como a imagem da tela inicial. A cor de tela de fundo da tela inicial muda para um padrão de cinza claro. Se você quiser mudar a imagem ou cor padrão da tela inicial de seu aplicativo, siga estas etapas:

1. Abra o seu projeto de aplicativa do Plataforma Universal do Windows (UWP) no Visual Studio.
2. Em **Solution Explorer**, abra o arquivo "Package.appxmanifest". Você também pode abrir esse arquivo pela barra de menus escolhendo **Projeto** &gt; **Store** &gt; **Editar Manifesto do Aplicativo**.
3. Abra a guia **Ativos Visuais** e selecione **Tela inicial** no painel **Todos os ativos de imagem** no lado esquerdo da janela "Package.appxmanifest". Se você estiver alterando sua tela inicial pela primeira vez, verá o caminho "Assets\\SplashScreen.png"no campo **Tela inicial**.

    A tela a seguir mostra a janela "Package.appxmanifest" no Visual Studio. Dependendo do tipo de projeto, você verá um conjunto de ativos visuais ligeiramente diferente.

    ![uma captura de tela da janela "package.appxmanifest" no visual studio 2017](images/appmanifest.png)

    Se você abrir "Package.appxmanifest" em um editor de texto, o [**elemento SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467) aparecerá como filho do [**elemento VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471). O marcador da tela inicial padrão no arquivo de manifesto aparece assim eu editor de texto:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Para selecionar uma nova imagem de tela inicial para um aplicativo UWP, pressione o botão com reticências que aparece próximo ao rótulo **1240 x 600 px** abaixo de **Ativos dimensionados**. Escolha a imagem de 1240 x 600 pixels (.png, .jpg, or .jpeg) que gostaria de usar como imagem da sua tela inicial.

    **Importante**imagem da tela inicial que você escolher deve ser 620 x 300 pixels usando um 1 x fator de escala. Além disso, ao projetar sua tela inicial, observe que ela é menor do que a tela e centralizada. Ela não preenche a tela como uma tela inicial de um aplicativo da Store do Windows Phone faz.

5. Para selecionar uma nova imagem de tela inicial para um aplicativo da Store do Windows Phone, pressione o botão com reticências que aparece próximo ao rótulo **1152 x 1920 px** abaixo de **Ativos dimensionados**. Escolha a imagem de 1152 x 1920 pixels (.png, .jpg, or .jpeg) que gostaria de usar como imagem da sua tela inicial.

    **Importante**imagem da tela inicial que você escolher deve ter 1152 x 1920 pixels que é o tamanho correto para um 2,4 x fator de escala. Se esse for o único ativo que você fornecer, então, ele será reduzido para os fatores de dimensionamento 1,4 x e 1x.

6. No campo **Cor da tela de fundo** na seção **Tela inicial**, defina a cor da tela de fundo exibida na imagem da sua tela inicial. Você pode inserir o nome de uma cor ou '\#' and e o valor hex. de uma cor. Para uma lista de nomes de cores disponíveis, consulte [**elemento SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467). Definir uma cor da tela de fundo para sua tela de inicial é opcional. Se você não especificar uma cor para um aplicativo UWP, a cor da tela de fundo da tela inicial fica padroniza-se em cinza claro (valor hex. \#464646). Essa é a mesma cor que a cor da tela de fundo do **Bloco** (consulte o campo **Cor da tela de fundo** da seção **Imagens e logotipos do bloco** na guia **Ativos visuais**). Se você não especificar uma cor para um aplicativo do Windows Phone, ou configurá-lo como "transparente", a cor de tela de fundo da tela inicial será transparente.

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Se o aplicativo levar algum tempo para ser carregado, considere a inclusão de uma tela inicial estendida. Para obter orientação passo a passo, veja [Create a customized splash screen](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Criar uma tela inicial personalizada](create-a-customized-splash-screen.md)
* [Referência do esquema do manifesto do pacote: elemento SplashScreen](https://msdn.microsoft.com/library/windows/apps/br211467)
* [Classe Windows.ApplicationModel.Activation.SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763)