---
title: Reagir nativo ao desenvolvimento do Android no Windows
description: Comece a desenvolver aplicativos Android usando o Xamarin nativo no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, reagir nativo, emulador, exposição, metro embarcar, terminal
ms.date: 04/28/2020
ms.openlocfilehash: db49e3ed12fee8e7ced7680e305a84afb89f32a2
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255230"
---
# <a name="get-started-developing-for-android-using-react-native"></a>Introdução ao desenvolvimento para Android usando reagir nativo

Este guia ajudará você a começar a usar o reaja nativo no Windows para criar um aplicativo de plataforma cruzada que funcionará em dispositivos Android.

## <a name="overview"></a>Visão geral

Reagir nativo é uma estrutura de aplicativo móvel [de](https://github.com/facebook/react-native) software livre criada pelo Facebook. Ele é usado para desenvolver aplicativos para Android, iOS, Web e UWP (Windows) que fornecem controles de interface do usuário nativos e acesso completo à plataforma nativa. Trabalhar com reagir nativo requer uma compreensão dos conceitos básicos do JavaScript.

## <a name="get-started-with-react-native-by-installing-required-tools"></a>Introdução ao reagir nativo instalando as ferramentas necessárias

1. [Instale Visual Studio Code](https://code.visualstudio.com) (ou o editor de código de sua escolha).

2. [Instale o Android Studio para Windows](https://developer.android.com/studio). O Android Studio instala a SDK do Android mais recente por padrão. Reagir nativo exige o SDK do Android 6,0 (marshmallow) ou superior. É recomendável usar o SDK mais recente.

3. Crie caminhos de variável de ambiente para o SDK do Java e SDK do Android:
    - No menu pesquisa do Windows, digite: "editar as variáveis de ambiente do sistema". isso abrirá a janela **Propriedades do sistema** .
    - Escolha **variáveis de ambiente...** e, em seguida, escolha **novo...** em **variáveis de usuário**.
    - Insira o nome e o valor da variável (caminho). Os caminhos padrão para os SDKs de Java e Android são os seguintes. Se você tiver escolhido um local específico para instalar os SDKs de Java e Android, atualize os caminhos de variável adequadamente.
        - JAVA_HOME: C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME: C:\Users\username\AppData\Local\Android\Sdk

    ![Captura de tela da adição de caminho de variável de ambiente](../images/add-environmental-variable-path.png)

4. [Instalar o NodeJS para Windows](https://nodejs.org/en/) Talvez você queira considerar o uso do [node versão Manager (NVM) para Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) se você estiver trabalhando com vários projetos e a versão do NodeJS. É recomendável instalar a versão mais recente do LTS para novos projetos.

> [!NOTE]
> Talvez você também queira instalar e usar o terminal do [Windows](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) para trabalhar com a CLI (interface de linha de comando) preferida, bem como o [git para controle de versão](https://git-scm.com/downloads). O [JDK do Java](https://www.oracle.com/java/technologies/javase-downloads.html) vem empacotado com o Android Studio v 2.2 +, mas se você precisar atualizar o JDK separadamente do Android Studio, use o [Windows x64 Installer](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html).

## <a name="create-a-new-project-with-react-native"></a>Criar um novo projeto com reagir nativo

1. Use NPM para instalar o utilitário de linha de comando da [CLI do exposição](https://docs.expo.io/versions/latest/) no prompt de comando do Windows, PowerShell, [terminal do Windows](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)ou terminal integrado no vs Code (Exibir > terminal integrado).

    ```powershell
    npm install -g expo-cli
    ```

2. Use exposição para criar um aplicativo nativo reagir que é executado no iOS, no Android e na Web. Você precisará escolher entre modelos de projeto, que incluem o TypeScript ( **em**branco **)**, **guias** (exemplos de telas usando reagir-navegação), **mínimo**ou **mínimo (typescript)**.

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > Se você estiver acostumado a usar `npx create-react-native-app`o, isso ainda funcionará, mas a inicialização exposição-CLI tem [alguns benefícios adicionais](https://github.com/react-native-community/discussions-and-proposals/issues/23).

3. Abra seu novo diretório "meu novo aplicativo":

    ```powershell
    cd my-new-app
    ```

4. Para executar o projeto, insira o comando a seguir. Isso abrirá uma janela do localhost no navegador da Internet padrão exibindo o nó Metro embarcador. Ele também exibirá um código QR na linha de comando e na janela do navegador do pacote metro. * Você também pode usar o comando `npm start` : `npm run android` ou.

     ```powershell
    expo start
    ```

    ![Captura de tela do metro embarcador no navegador](../images/metro-bundler.png)

5. Para exibir o projeto em execução em um dispositivo Android, você precisará primeiro [instalar o aplicativo cliente exposição com o Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) em seu dispositivo Android. Depois que o aplicativo cliente do exposição estiver instalado, abra-o em seu dispositivo e selecione **digitalizar código QR**. Depois que o código QR for registrado, você poderá ver a compilação do pacote em seu dispositivo e na janela do conjunto do metro em execução no localhost em seu navegador.

6. Para exibir o projeto em execução em um emulador do Android, primeiro você precisará abrir Android Studio e, em seguida, criar e iniciar um dispositivo virtual. **Ferramentas** > **AVD Manager** > **[+ criar dispositivo virtual...](https://developer.android.com/studio/run/managing-avds#createavd)**. Depois que o dispositivo virtual for criado, selecione o botão Iniciar ▷ na coluna **ações** do Device Manager virtual do Android para iniciar a emulação do dispositivo. Depois que o dispositivo virtual estiver aberto, retorne para a janela do conjunto do metro em execução na janela do navegador da Internet e selecione "executar no dispositivo/emulador do Android" na coluna à esquerda. Você deve ver um pop-up, permitindo que você saiba que o conjunto metro está "tentando abrir um simulador..." em seguida, veja o aplicativo cliente do exposição aberto em seu dispositivo Android emulado e, depois de concluir o download do pacote do JavaScript, você verá seu aplicativo de reagir nativo exibido. (Se você tiver problemas, [Verifique os documentos do exposição Android Emulator](https://docs.expo.io/workflow/android-studio-emulator/).)

7. Abra o seu projeto nativo reagir para começar a trabalhar em seu aplicativo. Você deve ver as alterações atualizadas automaticamente no aplicativo em execução por meio do cliente exposição em seu dispositivo ou em seu Android Emulator.

8. Tente alterar o texto de exibição da página de aterrissagem para dizer: "Olá, Mundo!". Você pode fazer isso no IDE de sua escolha. (Recomendamos VS Code ou Android Studio.) O arquivo da página de aterrissagem será diferente dependendo do modelo escolhido. Pode ser `App.js`, `App.tsx`ou. `HomeScreen.js`

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. Tente adicionar uma imagem. Primeiro, você precisará criar uma pasta no mesmo nível das pastas "Android" e "Ios" em seu aplicativo, vamos chamá-la de "images". Coloque uma imagem nessa pasta, `your-image.png` por exemplo. Use o formato abaixo para fazer referência à sua imagem e estilizar com uma altura e largura.

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> Se você quiser adicionar suporte para seu aplicativo reajam nativo para que ele seja executado como um aplicativo do Windows 10, consulte a [introdução ao reagir nativo para Windows](https://microsoft.github.io/react-native-windows/docs/getting-started) docs.

## <a name="additional-resources"></a>Recursos adicionais

- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Adicionar exclusões do Windows Defender para melhorar o desempenho](defender-settings.md)

- [Habilitar o suporte de virtualização para melhorar o desempenho do emulador](emulator.md#enable-virtualization-support)
