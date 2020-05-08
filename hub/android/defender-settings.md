---
title: Melhorar a velocidade de desempenho atualizando as configurações do defender
description: Saiba como melhorar a velocidade de desempenho e os tempos de compilação atualizando as configurações do Windows Defender para excluir a verificação de tipos de arquivo especificados.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Windows Defender, configurações, configuração, exclusões,% USERPROFILE%, devenv. exe, desempenho, velocidade, compilação, gradle
ms.date: 04/28/2020
ms.openlocfilehash: d818c4f568698a121fb7051ec5e6e2d246bff924
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255130"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>Atualizar as configurações do Windows Defender para aprimorar o desempenho

Este guia aborda como configurar exclusões em suas configurações de segurança do Windows Defender para melhorar os tempos de compilação e a velocidade de desempenho geral do seu computador Windows.

## <a name="windows-defender-overview"></a>Visão geral do Windows Defender

No Windows 10, versão 1703 e posterior, o aplicativo [Windows Defender antivírus](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) faz parte da segurança do Windows. O Windows Defender visa manter seu computador seguro com proteção interna em tempo real contra vírus, ransomware, spyware e outras ameaças à segurança.

**No entanto**, a proteção em tempo real do Windows Defender também reduzirá drasticamente o acesso ao sistema de arquivos e a velocidade de compilação ao desenvolver aplicativos Android.

Durante o processo de compilação do Android, muitos arquivos são criados em seu computador. Com a verificação de antivírus em tempo real habilitada, o processo de compilação será interrompido toda vez que um novo arquivo for criado enquanto o antivírus verificar esse arquivo.

Felizmente, o Windows Defender tem a capacidade de excluir arquivos, diretórios de projeto ou tipos de arquivos que você sabe que são seguros do processo de verificação antivírus.

> [!WARNING]
> Para garantir que seu computador esteja protegido contra software mal-intencionado, você não deve desabilitar completamente a verificação em tempo real ou o software Windows Defender antivírus.

## <a name="add-exclusions-to-windows-defender"></a>Adicionar exclusões ao Windows Defender

Para melhorar a velocidade de Build do Android, Adicione exclusões na [central de segurança do Windows Defender](windowsdefender://) :

1. Selecione o botão **Iniciar** do menu do Windows
2. Insira a **segurança do Windows**
3. Selecionar **proteção contra vírus e ameaças**
4. Selecione **gerenciar configurações** em **vírus & ameaças proteção configurações**
5. Role até o cabeçalho **exclusões** e selecione **Adicionar ou remover exclusões**
6. Selecione **+ Adicionar uma exclusão**. Em seguida, será necessário escolher se a exclusão que você deseja adicionar é um **arquivo**, uma **pasta**, um **tipo de arquivo**ou um **processo**.

![Captura de tela Adicionar exclusão do Windows Defender](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>Exclusões recomendadas

A lista a seguir mostra o local padrão de cada diretório Android Studio recomendado para adicionar como uma exclusão da verificação em tempo real do Windows Defender:

- Cache gradle:`%USERPROFILE%\.gradle`
- Projetos de Android Studio:`%USERPROFILE%\AndroidStudioProjects`
- SDK do Android:`%USERPROFILE%\AppData\Local\Android\SDK`
- Android Studio arquivos do sistema:`%USERPROFILE%\.AndroidStudio<version>\system`

Esses locais de diretório podem não se aplicar ao seu projeto se você não tiver usado os locais padrão definidos por Android Studio ou se tiver baixado um projeto do GitHub (por exemplo). Considere adicionar uma exclusão ao diretório do seu projeto de desenvolvimento Android atual, onde quer que possa estar localizado.

As exclusões adicionais que talvez você queira considerar incluem:

- Processo do ambiente de desenvolvimento do Visual Studio:`devenv.exe`
- Processo de compilação do Visual Studio:`msbuild.exe`
- Diretório JetBrains:`%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

Para obter mais informações sobre como adicionar exclusões de verificação antivírus, incluindo como personalizar locais de diretório para ambientes Política de Grupo controlados, consulte a seção [impacto sobre o antivírus](https://developer.android.com/studio/intro/studio-config#antivirus-impact) da documentação do Android Studio.

> [!Note]
> Daniel knoodle configurou um repositório GitHub com scripts recomendados para adicionar [exclusões do Windows Defender para o Visual Studio 2017](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10).

## <a name="additional-resources"></a>Recursos adicionais

- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Adicionar exclusões do Windows Defender para melhorar o desempenho](./defender-settings.md)

- [Habilitar o suporte de virtualização para melhorar o desempenho do emulador](./emulator.md#enable-virtualization-support)
