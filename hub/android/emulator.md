---
title: Executando o dispositivo Android ou o emulador do Windows
description: Teste seu aplicativo em um dispositivo Android ou emulador do Windows e habilite a virtualização com o Hyper-v e a WHPX (plataforma de hipervisor do Windows).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, emulador, dispositivo virtual, configuração do dispositivo, habilitar dispositivo, desenvolvedor, configuração, virtualização, Visual Studio, Hyper-v, Intel, haxm, AMD, plataforma de hipervisor do Windows, WHPX
ms.date: 04/28/2020
ms.openlocfilehash: c651661d573695902368ffa595ce5d3014791a9a
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255150"
---
# <a name="test-on-an-android-device-or-emulator"></a>Testar em um dispositivo Android ou emulador

Há várias maneiras de testar e depurar seu aplicativo Android usando um dispositivo ou emulador real em seu computador Windows. Descrevemos algumas recomendações neste guia.

## <a name="run-on-a-real-android-device"></a>Executar em um dispositivo Android real

Para executar seu aplicativo em um dispositivo Android real, primeiro você precisará habilitar seu dispositivo Android para desenvolvimento. As opções de desenvolvedor no Android foram ocultadas por padrão, uma vez que a versão 4,2 e a habilitação podem variar com base na versão do Android.

### <a name="enable-your-device-for-development"></a>Habilitar seu dispositivo para desenvolvimento

Para um dispositivo que executa uma versão recente do Android 9.0 +:

1. Conecte seu dispositivo à sua máquina de desenvolvimento do Windows com um cabo USB. Você pode receber uma notificação para instalar um driver USB.
2. Abra a tela **configurações** em seu dispositivo Android.
3. Selecione **sobre o telefone**.
4. Role até a parte inferior e toque em **número da versão** sete vezes, até **que você agora seja um desenvolvedor!** está visível.
5. Retorne à tela anterior, selecione **sistema**.
6. Selecione **avançado**, role até a parte inferior e toque em **Opções do desenvolvedor**.
7. Na janela **Opções do desenvolvedor** , role para baixo até localizar e habilitar a **depuração de USB**.

Para um dispositivo que executa uma versão mais antiga do Android, consulte [Configurar o dispositivo para desenvolvimento](https://docs.microsoft.com/xamarin/android/get-started/installation/set-up-device-for-development).

### <a name="run-your-app-on-the-device"></a>Executar seu aplicativo no dispositivo

1. Na barra de ferramentas Android Studio, selecione seu aplicativo no menu suspenso **configurações de execução** .

    ![Android Studio menu de configuração de execução](../images/android-run-config-menu.png)

2. No menu suspenso **dispositivo de destino** , selecione o dispositivo no qual você deseja executar seu aplicativo.

    ![Android Studio menu do dispositivo de destino](../images/android-target-device-menu.png)

3. Selecione executar ▷. Isso iniciará o aplicativo em seu dispositivo conectado.

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>Executar seu aplicativo em um dispositivo Android virtual usando um emulador

A primeira coisa a saber sobre a execução de um emulador do Android no computador com Windows é que, independentemente do IDE (Android Studio, do Visual Studio etc.), o desempenho do emulador é amplamente melhorado ao habilitar o suporte à virtualização.

### <a name="enable-virtualization-support"></a>Habilitar o suporte à virtualização

Antes de criar um dispositivo virtual com o emulador do Android, é recomendável habilitar a virtualização ativando os recursos do Hyper-V e da WHPX (plataforma de hipervisor do Windows). Isso permitirá que o processador do computador melhore significativamente a velocidade de execução do emulador.

> Para executar o Hyper-V e a plataforma de hipervisor do Windows, o computador deve:
>
> * Ter 4 GB de memória disponível
> * Ter um processador Intel de 64 bits ou CPU AMD Ryzen com SLAT (conversão de endereços de segundo nível)
> * Executando o Windows 10 Build 1803 + ([Verifique sua compilação #](ms-settings:about))
> * Ter drivers gráficos atualizados (Device Manager > adaptadores de vídeo > driver de atualização)
>
> Se o computador não atender a esses critérios, você poderá executar o [Intel HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) ou o [processador AMD](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors). Para obter mais informações, consulte o artigo: [aceleração de hardware para desempenho de emulador](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) ou a [documentação do emulador de Android Studio](https://developer.android.com/studio/run/emulator).

1. Verifique se o hardware e o software do seu computador são compatíveis com o Hyper-V abrindo um prompt de comando e inserindo o comando:`systeminfo`

    ![Requisitos do Hyper-V do SystemInfo no prompt de comando](../images/systeminfo.png)

2. Na caixa de pesquisa do Windows (inferior esquerda), insira "recursos do Windows". Selecione **Ativar ou desativar recursos do Windows nos** resultados da pesquisa.

3. Depois que a lista de **recursos do Windows** for exibida, role para encontrar o **Hyper-V** (inclui as ferramentas de gerenciamento e a plataforma) e a **plataforma de hipervisor do Windows**, verifique se a caixa está marcada para habilitar ambos e selecione **OK**.

4. Reinicie seu computador quando solicitado.

### <a name="emulator-for-native-development-with-android-studio"></a>Emulador para desenvolvimento nativo com Android Studio

Ao criar e testar um aplicativo Android nativo, é recomendável [usar Android Studio](./native-android.md). Quando seu aplicativo estiver pronto para teste, você poderá compilar e executar seu aplicativo da:

1. Na barra de ferramentas Android Studio, selecione seu aplicativo no menu suspenso **configurações de execução** .

    ![Android Studio menu de configuração de execução](../images/android-run-config-menu.png)

2. No menu suspenso **dispositivo de destino** , selecione o dispositivo no qual você deseja executar seu aplicativo.

    ![Android Studio menu do dispositivo de destino](../images/android-target-device-menu.png)

3. Selecione executar ▷. Isso iniciará o [Android Emulator](https://developer.android.com/studio/run/emulator).

> [!TIP]
> Depois que seu aplicativo estiver instalado no dispositivo emulador, você poderá usar [aplicar alterações](https://developer.android.com/studio/run#apply-changes) para implantar determinadas alterações de código e de recursos sem criar um novo apk.

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>Emulador para desenvolvimento de plataforma cruzada com o Visual Studio

Há muitas [Opções de emulador do Android](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) disponíveis para computadores Windows. É recomendável usar o [Android Emulator](https://developer.android.com/studio/run/emulator)do Google, pois ele oferece acesso às mais recentes imagens do sistema operacional Android e serviços de Google Play.

### <a name="install-android-emulator-with-visual-studio"></a>Instalar o Android Emulator com o Visual Studio

1. Se você ainda não o tiver instalado, baixe o [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Use o Instalador do Visual Studio para [modificar suas cargas de trabalho](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) e garantir que você tenha o **desenvolvimento móvel com carga de trabalho do .net**.

2. Criar um novo projeto. Depois de [Configurar o Android Emulator](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/), você pode usar o [Android Device Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#requirements) para criar, duplicar, personalizar e iniciar uma variedade de dispositivos virtuais Android. Inicie o Android Device Manager no menu ferramentas com: **ferramentas** > **Android** > **Android Device Manager**.

3. Quando o Android Device Manager for aberto, selecione **+ novo** para criar um novo dispositivo.

4. Você precisará dar um nome ao dispositivo, escolher o tipo de dispositivo base em um menu suspenso, escolher um processador e uma versão do sistema operacional, juntamente com várias outras variáveis para o dispositivo virtual. Para obter mais informações, [Android Device Manager tela principal](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#main-screen).

5. Na barra de ferramentas do Visual Studio, escolha entre **depuração** (anexações ao processo do aplicativo em execução dentro do emulador depois que o aplicativo é iniciado) ou modo de **liberação** (desabilita o depurador). Em seguida, escolha um dispositivo virtual no menu suspenso dispositivo e selecione o botão **reproduzir** ▷ para executar o aplicativo no emulador.

    ![Android Emulator de inicialização do Visual Studio](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>Recursos adicionais

- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Adicionar exclusões do Windows Defender para melhorar o desempenho](defender-settings.md)
