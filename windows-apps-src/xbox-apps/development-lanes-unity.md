---
title: Trazendo o jogos Unity para UWP no Xbox
description: Desenvolvimento de UWP Unity no Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
ms.localizationpriority: medium
ms.openlocfilehash: 64a686aea24d23b5e999780eaa0eda661af3637f
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8331299"
---
# <a name="bringing-unity-games-to-uwp-on-xbox"></a>Trazendo o jogos Unity para UWP no Xbox


Neste tutorial passo a passo, presumimos que você já tenha um jogo no Unity, pronto para ser compilado e implantado.

Consulte também uma [versão em vídeo deste tutorial](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Procurando a versão do seu projeto Unity UWP? Consulte [Controle de versão do seu projeto UWP](development-lanes-unity-versioning.md).

## <a name="step-0-ensure-unity-is-installed-correctly"></a>Etapa 0: Verifique se o Unity está instalado corretamente

Ao instalar o Unity, estes componentes devem ser selecionados:

![Componentes de instalação do Unity](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>Etapa 1: Criando a solução UWP

No seu projeto de jogo Unity, abra as janelas **Configurações da Compilação** localizadas em **Arquivo -> Configurações da Compilação** e vá para o menu de opções da Microsoft Store mostrado abaixo.

![Janela Configurações da Compilação](images/build-settings.png)

Verifique se a configuração do **SDK** está definida como **Universal 10** e, em seguida, selecione o botão **Compilar**, que iniciará uma janela do Explorador de Arquivos solicitando uma pasta de destino. Crie uma pasta denominada **UWP** ao lado do diretório **Ativos** do seu projeto e escolha esta pasta como a pasta de destino da compilação.

![Pasta de destino da compilação](images/build-destination.png)

O Unity criou uma nova solução do Visual Studio que usaremos para implantar seu jogo UWP.

![Solução do VS da UWP](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>Etapa 2: Implementando seu jogo

Abra a solução recentemente gerada na pasta **UWP** e, em seguida, altere a plataforma de destino para **x64**.

![Plataforma da Compilação x64](images/x64-build-platform.png)

Agora que você tem uma solução do Visual Studio da UWP para seu jogo, [seguir estas etapas](getting-started.md) permitirá que você implante com êxito seu jogo em seu Xbox One de varejo!

## <a name="step-3-modify-and-rebuild"></a>Etapa 3: Modificar e recriar

Se forem feitas alterações em algo que não seja um script, para que essas alterações sejam mostradas na versão UWP do jogo, o projeto deve ser recriado no Editor (conforme descrito na __Etapa 1__).

## <a name="versioning-your-uwp-project"></a>Controle de versão do seu projeto UWP

Há algumas situações comuns onde adicionar partes desse diretório UWP recentemente gerado ao controle de versão se torna necessário. Por exemplo, se você estiver adicionando uma nova dependência ao projeto UWP (por exemplo, SDK do Xbox Live).  Vamos examinar este exemplo em detalhes em [Controle de versão do seu projeto UWP](development-lanes-unity-versioning.md).

## <a name="see-also"></a>Consulte também
- [Trazendo jogos existentes para o Xbox](development-lanes-landing.md)
- [UWP no Xbox One](index.md)
