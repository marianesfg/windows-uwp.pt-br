---
author: JordanEllis6809
title: Trazendo o jogos Unity para UWP no Xbox
description: Desenvolvimento de UWP Unity no Xbox.
translationtype: Human Translation
ms.sourcegitcommit: ea3bea2e5d6de0e55615de701a69e90d81f0f553
ms.openlocfilehash: 73f701a2608c6ce8d10cab817683ada4e9eecc08

---

# Trazendo o jogos Unity para UWP no Xbox


Neste tutorial passo a passo, presumimos que você já tenha um jogo no Unity, pronto para ser compilado e implantado.

Consulte também uma [versão em vídeo deste tutorial](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Procurando a versão do seu projeto Unity UWP? Consulte [Controle de versão do seu projeto UWP](development-lanes-unity-versioning.md).

## Etapa 0: Verifique se o Unity está instalado corretamente

Ao instalar o Unity, estes componentes devem ser selecionados:

![Componentes de instalação do Unity](images/unity-install-components.png)

## Etapa 1: Criando a solução UWP

No seu projeto de jogo Unity, abra as janelas **Configurações da Compilação** localizadas em **Arquivo -> Configurações da Compilação** e vá para o menu de opções da Windows Store mostrado abaixo.

![Janela Configurações da Compilação](images/build-settings.png)

Verifique se a configuração do **SDK** está definida como **Universal 10** e, em seguida, selecione o botão **Compilar**, que iniciará uma janela do Explorador de Arquivos solicitando uma pasta de destino. Crie uma pasta denominada **UWP** ao lado do diretório **Ativos** do seu projeto e escolha esta pasta como a pasta de destino da compilação.

![Pasta de destino da compilação](images/build-destination.png)

O Unity criou uma nova solução do Visual Studio que usaremos para implantar seu jogo UWP.

![Solução do VS da UWP](images/uwp-vs-solution.png)

## Etapa 2: Implementando seu jogo

Abra a solução recentemente gerada na pasta **UWP** e, em seguida, altere a plataforma de destino para **x64**.

![Plataforma da Compilação x64](images/x64-build-platform.png)

Agora que você tem uma solução do Visual Studio da UWP para seu jogo, [seguir estas etapas](getting-started.md) permitirá que você implante com êxito seu jogo em seu Xbox One de varejo!

## Etapa 3: Modificar e recriar

Se forem feitas alterações em algo que não seja um script, para que essas alterações sejam mostradas na versão UWP do jogo, o projeto deve ser recriado no Editor (conforme descrito na __Etapa 1__).

## Controle de versão do seu projeto UWP

Há algumas situações comuns onde adicionar partes desse diretório UWP recentemente gerado ao controle de versão se torna necessário. Por exemplo, se você estiver adicionando uma nova dependência ao projeto UWP (por exemplo, SDK do Xbox Live).  Vamos examinar este exemplo em detalhes em [Controle de versão do seu projeto UWP](development-lanes-unity-versioning.md).

## Consulte também
- [Trazendo jogos existentes para o Xbox](development-lanes-landing.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Aug16_HO4-->


