---
title: Trazer seu jogo Unity para o Xbox One
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# Trazer seu jogo Unity para o Xbox One

Neste tutorial passo a passo, presumimos que você já tenha um jogo no Unity, pronto para ser compilado e implantado.

[Versão em vídeo deste tutorial.](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## Etapa 0: Verifique se o Unity está instalado corretamente

Ao instalar o Unity, estes componentes devem ser selecionados:

![Componentes de instalação do Unity](images/unity-install-components.png)

## Etapa 1: Criando a solução UWP

No seu projeto de jogo Unity, abra as janelas Configurações da Compilação localizadas em `File -> Build Settings...`e vá para o menu de opções da Windows Store mostrado abaixo.

![Janela Configurações da Compilação](images/build-settings.png)

Verifique se a configuração `SDK` está definida como `Universal 10`. Em seguida, pressione o botão Compilação na parte inferior do menu. Isso iniciará uma janela do explorer solicitando uma pasta de destino. Crie uma pasta denominada `UWP` no diretório `Assets` do seu projeto e escolha esta pasta como a pasta de destino da compilação.

![Pasta de destino da compilação](images/build-destination.png)

O Unity criou uma nova solução do Visual Studio que usaremos para implantar seu jogo UWP na próxima etapa.

![Solução do VS da UWP](images/uwp-vs-solution.png)

## Etapa 2: Implantando seu jogo

Abra a solução recentemente gerada na pasta `Assets/UWP`.  Depois de abrir, altere a plataforma de destino para X64.

![Plataforma da Compilação x64](images/x64-build-platform.png)

Agora que você tem uma solução do Visual Studio da UWP para seu jogo, [seguir estas etapas](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started) permitirá que você implante com êxito seu jogo em seu Xbox One de varejo!

## Notas para desenvolvedor

- É recomendável que você ignore a pasta UWP no seu controle de versão. Se você estiver tentando adicionar outros elementos XAML ao seu projeto e precisar criar uma versão de alguns ativos na pasta UWP, consulte [estes exemplos que fazem exatamente isso](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

- Se você fizer alterações no Unity para qualquer conteúdo (exceto scripts) que estiver incluído na compilação do seu jogo, precisará recriar sua solução UWP para ver essas alterações na próxima vez que implantar. Isso ocorre porque durante a etapa de compilação do Unity, todos os ativos do projeto são compilados em um arquivo de recurso. A solução UWP faz referência a esse arquivo de recurso gerado ao implantar o jogo.




<!--HONumber=Jun16_HO5-->


