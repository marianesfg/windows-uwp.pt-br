---
title: Abordagem do PWA ao desenvolvimento para Android no Windows
description: Comece a desenvolver aplicativos Android usando a abordagem do PWA no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android no Windows, PWA, Android, Cordova, Ionic, PhoneGap, aplicativo Web híbrido
ms.date: 04/28/2020
ms.openlocfilehash: c0ff9acf1d8e93e82f1db424d7a356c974988683
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255203"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>Introdução ao desenvolvimento de um aplicativo Web do PWA ou híbrido para Android

Este guia ajudará você a começar a criar um aplicativo Web híbrido ou um aplicativo Web progressivo (PWA) no Windows usando uma única base de código HTML/CSS/JavaScript que pode ser usada na Web e em plataformas de dispositivos (Android, iOS, Windows).

Usando as estruturas e componentes certos, os aplicativos baseados na Web podem trabalhar em um dispositivo Android de uma forma que pareça aos usuários muito semelhantes a um aplicativo nativo.

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>Recursos de um aplicativo Web do PWA ou híbrido

Há dois tipos principais de aplicativos Web que podem ser instalados em dispositivos Android. A principal diferença é se o código do aplicativo está inserido em um pacote de aplicativo (híbrido) ou hospedado em um servidor Web (PWA).

- **Aplicativos Web híbridos**: o código (HTML, js, CSS) é empacotado em um apk e pode ser distribuído por meio do Google Play Store. O mecanismo de exibição é isolado do navegador da Internet dos usuários, sem compartilhamento de sessão ou cache.

- **PWAs (aplicativos Web progressivos)**: o código (HTML, js, CSS) reside na Web e não precisa ser empacotado como um apk. Os recursos são baixados e atualizados conforme necessário usando um trabalhador de serviço. O navegador Chrome renderizará e exibirá seu aplicativo, mas parecerá nativo e não incluirá a barra de endereços normal do navegador, etc. Você pode compartilhar armazenamento, cache e sessões com o navegador. Isso é basicamente como a instalação de um atalho para o navegador Chrome em um modo especial. PWAs também pode ser listado no Google Play Store usando atividade da Web confiável.

Os aplicativos Web PWAs e híbridos são muito semelhantes a um aplicativo Android nativo, pois:

- Pode ser instalado por meio da loja de aplicativos (Google Play Store e/ou Microsoft Store)
- Ter acesso a recursos de dispositivo nativo como câmera, GPS, Bluetooth, notificações e lista de contatos
- Trabalhar offline (sem conexão com a Internet)

O PWAs também tem alguns recursos exclusivos:

- Pode ser instalado na tela inicial do Android diretamente da Web (sem uma loja de aplicativos)
- Também pode ser instalado por meio do Google Play Store [usando uma atividade da Web confiável](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/)
- Pode ser descoberto por meio da pesquisa na Web ou compartilhado por meio de um link de URL
- Conte com um [operador de serviço](https://developers.google.com/web/fundamentals/primers/service-workers) para evitar a necessidade de empacotar código nativo

Você não precisa de uma estrutura para criar um aplicativo híbrido ou o PWA, mas há algumas estruturas populares que serão abordadas neste guia, incluindo PhoneGap (com Cordova) e Ionic (com Cordova ou capacitor usando angular ou reagir).

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/) é uma estrutura de código-fonte aberto que pode simplificar a comunicação entre o código JavaScript em uma exibição nativa do [WebView](https://developer.android.com/reference/android/webkit/WebView) e a plataforma Android nativa usando [plugins](https://cordova.apache.org/plugins/?platforms=cordova-android). Esses plug-ins expõem pontos de extremidade JavaScript que podem ser chamados do seu código e usados para chamar APIs nativas do dispositivo Android. Alguns plug-ins de exemplo do Cordova incluem acesso a serviços de dispositivos como status da bateria, acesso a arquivos, vibração/tons de toque, etc. Esses recursos normalmente não estão disponíveis para aplicativos Web ou navegadores.

Há duas distribuições populares do Cordova:

- [PhoneGap](https://phonegap.com/)

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

[PhoneGap](https://phonegap.com/): uma estrutura com suporte da Adobe que dá suporte ao Cordova com ferramentas adicionais, como uma [linha de comando](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/), um aplicativo de [área de trabalho](https://phonegap.com/products#desktop-app-section)e uma [compilação PhoneGap](https://build.phonegap.com/), um serviço que permite que você carregue seu código em um servidor Adobe que criará aplicativos nativos para você sem a necessidade de instalar SDKs nativos em seu computador local. Isso permite que você faça coisas como criar um aplicativo iOS usando seu computador Windows.

### <a name="install-phonegap"></a>Instalar o PhoneGap

Para começar a criar um aplicativo Web do PWA ou híbrido com o PhoneGap, você deve primeiro instalar as seguintes ferramentas:

- Node. js para interagir com o ecossistema Ionic. [Baixe o NodeJS para Windows](https://nodejs.org/en/) ou siga o [Guia de instalação do NodeJS](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2) usando o subsistema do Windows para Linux (WSL). Talvez você queira considerar o uso do [node versão Manager (NVM)](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) se estiver trabalhando com vários projetos e a versão do NodeJS.

Instale o PhoneGap inserindo o seguinte na linha de comando:

```bash
npm install -g phonegap
```

Para criar um novo projeto PhoneGap, siga suas etapas para [começar](https://phonegap.com/getstarted/). Visite a seção de [recursos do PWA](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/) do PhoneGap docs para saber como mover seu aplicativo de ser híbrido para um PWA.  

## <a name="ionic"></a>Ionic

[Ionic](https://ionicframework.com/) é uma estrutura que ajusta a interface do usuário do seu aplicativo para corresponder à linguagem de design de cada plataforma (Android, Ios, Windows). O Ionic permite que você use [angular](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro) ou [reagir](https://ionicframework.com/react).

> [!NOTE]
> Há uma nova versão do Ionic que usa uma alternativa ao Cordova, chamada de [capacitor](https://capacitor.ionicframework.com/). Essa alternativa usa contêineres para tornar seu aplicativo [mais amigável](https://ionicframework.com/blog/announcing-capacitor-1-0/)para a Web.

### <a name="get-started-with-ionic-by-installing-required-tools"></a>Introdução ao Ionic instalando as ferramentas necessárias

Para começar a criar um aplicativo Web do PWA ou híbrido com o Ionic, você deve primeiro instalar as seguintes ferramentas:

- Node. js para interagir com o ecossistema Ionic. [Baixe o NodeJS para Windows](https://nodejs.org/en/) ou siga o [Guia de instalação do NodeJS](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2) usando o subsistema do Windows para Linux (WSL). Talvez você queira considerar o uso do [node versão Manager (NVM)](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) se estiver trabalhando com vários projetos e a versão do NodeJS.

- VS Code para escrever seu código. [Baixe vs Code para Windows](https://code.visualstudio.com/). Talvez você também queira instalar a [extensão remota WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) se preferir criar seu aplicativo com uma linha de comando do Linux.

- Terminal do Windows para trabalhar com sua CLI (interface de linha de comando) preferida. [Instale o terminal do Windows do Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

- Git para controle de versão. [Baixe o Git](https://git-scm.com/downloads).

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>Criar um novo projeto com o Ionic Cordova e o angular

Instale o Ionic e o Cordova digitando o seguinte na linha de comando:

```bash
npm install -g @ionic/cli cordova
```

Crie um aplicativo Ionic angular usando o modelo de aplicativo "tabs" digitando o comando:

```bash
ionic start photo-gallery tabs
```

Altere para a pasta do aplicativo:

```bash
cd photo-gallery
```

Execute o aplicativo em seu navegador da Web:

```bash
ionic serve
```

Para obter mais informações, consulte os [documentos angulares do Ionic Cordova](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro). Visite a seção [criando seu aplicativo angular em um PWA](https://ionicframework.com/docs/angular/pwa) do Ionic docs para saber como mover seu aplicativo de uma forma híbrida para um PWA.

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>Criar um novo projeto com capacitor Ionic e angular

Instale o Ionic e o Cordova-res digitando o seguinte na linha de comando:

```bash
npm install -g @ionic/cli native-run cordova-res
```

Crie um aplicativo angular Ionic usando o modelo de aplicativo "tabs" e adicione o capacitor, inserindo o comando:

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

Altere para a pasta do aplicativo:

```bash
cd photo-gallery
```

Adicionar componentes para tornar o aplicativo um PWA:

```bash
npm install @ionic/pwa-elements
```

Importe @ionic/pwa-elements por adicionar o seguinte ao seu `src/main.ts` arquivo:

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

Execute o aplicativo em seu navegador da Web:

```bash
ionic serve
```

Para obter mais informações, consulte os [documentos angulares do capacitor Ionic](https://ionicframework.com/docs/angular/your-first-app). Visite a seção [criando seu aplicativo angular em um PWA](https://ionicframework.com/docs/angular/pwa) do Ionic docs para saber como mover seu aplicativo de uma forma híbrida para um PWA.  

## <a name="create-a-new-project-with-ionic-and-react"></a>Criar um novo projeto com Ionic e reagir

Instale a CLI do Ionic digitando o seguinte na linha de comando:

```bash
npm install -g @ionic/cli
```

Crie um novo projeto com reagir digitando o comando:

```bash
ionic start myApp blank --type=react
```

Altere para a pasta do aplicativo:

```bash
cd myApp
```

Execute o aplicativo em seu navegador da Web:

```bash
ionic serve
```

Para obter mais informações, consulte os [docs Ionic reajam](https://ionicframework.com/docs/react/quickstart). Visite a seção [fazendo seu aplicativo reagir a um PWA](https://ionicframework.com/docs/react/pwa) do Ionic docs para saber como mover seu aplicativo de forma a ser híbrido para um PWA.

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>Testar seu aplicativo Ionic em um dispositivo ou emulador

Para testar seu aplicativo Ionic em um dispositivo Android, conecte seu dispositivo (verifique[se ele está habilitado primeiro para desenvolvimento](emulator.md#enable-your-device-for-development)) e, em seguida, na linha de comando, digite:

```bash
ionic cordova run android
```

Para testar seu aplicativo Ionic em um emulador de dispositivo Android, você deve:

1. [Instale os componentes necessários--Java Development Kit (JDK), gradle e o SDK do Android](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements).

2. [Crie um dispositivo virtual Android (AVD)](https://developer.android.com/studio/run/managing-avds.html).

3. Insira o comando para Ionic para compilar e implantar seu aplicativo no emulador: `ionic cordova emulate [<platform>] [options]`. Nesse caso, o comando deve ser:

```bash
ionic cordova emulate android --list
```

Consulte o [emulador do Cordova](https://ionicframework.com/docs/cli/commands/cordova-emulate) no Ionic docs para obter mais informações.

## <a name="additional-resources"></a>Recursos adicionais

- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Adicionar exclusões do Windows Defender para melhorar o desempenho](defender-settings.md)

- [Habilitar o suporte de virtualização para melhorar o desempenho do emulador](emulator.md#enable-virtualization-support)
