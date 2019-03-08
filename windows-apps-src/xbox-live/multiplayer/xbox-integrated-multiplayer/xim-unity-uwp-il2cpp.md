---
title: Usar o XIM (Unity com IL2CPP)
description: Usando com vários participantes do Xbox integrado com o Unity para UWP com back-end de script IL2CPP
ms.date: 04/03/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, Unity, com vários participantes integrado do Xbox
ms.localizationpriority: medium
ms.openlocfilehash: a600fd253efae1daca34241b105a69514561e01d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646441"
---
# <a name="use-xim-unity-with-il2cpp"></a>Usar o XIM (Unity com IL2CPP)

## <a name="overview"></a>Visão geral

Suporte de tempo de execução do Windows para IL2CPP no Unity

Com o lançamento do Unity 5.6f3 o mecanismo incluiu um novo recurso que permite aos desenvolvedores usar componentes de tempo de execução do Windows (WinRT) diretamente no script, incluindo-os diretamente no projeto do jogo. Até que os 5.6 desenvolvedores precisou de um plug-in ou dll para oferecer suporte a qualquer recurso de plataforma (incluindo com vários participantes do Xbox integrado) do script de jogo em UWP. Essa nova camada de projeção elimina a necessidade de plug-in e introduz um fluxo de trabalho novo e simplificado compatível somente com jogos que escolher IL2CPP script back-end.

- Para obter mais informações sobre como começar com o uso de WinRT e o Unity, consulte o [documentação do Unity](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html).
- Para obter mais informações sobre como adicionar suporte do Xbox Live para Unity usando IL2CPP, consulte o [documentação do Xbox Live](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp) sobre o assunto.

## <a name="using-the-xim-unity-asset-package"></a>Usando o pacote do Unity XIM ativo

### <a name="1-install-unity"></a>1. Instalar o Unity

Instalar o Unity 5.6 ou posterior e verifique se você tem o **back-end o script do Windows Store Il2CPP** selecionado durante a instalação

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. Instalar ferramentas do Visual Studio para Unity versão 3.1 e acima para o IntelliSense dá suporte ao usar WinMDs

Para Visual Studio 2015, isso pode ser encontrado [no Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity). Para o Visual Studio 2017, é possível adicionar o componente dentro do instalador do Visual Studio 2017.

### <a name="3-open-a-new-or-existing-unity-project"></a>3. Abra um projeto novo ou existente do Unity

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. Alternar a plataforma para a plataforma Universal do Windows no menu de configurações de Build do Unity

![Menu de configurações de build do Unity com a plataforma Universal do Windows compilar a configuração selecionada](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. Habilitar o IL2CPP script back-end nas configurações do player do Unity e definir a compatibilidade de API para o .NET 4.6

![A seção de configuração do menu Configurações de Player do Unity com a configuração de "Compatibilidade de Api" definido como ".NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. Importar a versão mais recente do pacote de ativos do Xbox integrado com vários participantes WinRT Unity

Isso pode ser encontrado em https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. Agora você pode usar XIM em seus scripts

Para obter mais informações sobre como usar XIM com C#, consulte [XIM usar (C#)](using-xim-cs.md).

O trecho a seguir mostra como XIM pode ser integrado ao seu código:

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

Para obter mais informações sobre o `ENABLE_WINMD_SUPPORT` #define diretiva, consulte a documentação do Unity no [suporte de tempo de execução do Windows](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)

### <a name="8-required-capability-content"></a>8. Conteúdo de recurso necessário

Um aplicativo usando XIM inerentemente requer conexão e aceitando conexões de recursos de rede, tanto pela Internet e a rede local. Ele também requer acesso a dispositivos de microfone para dar suporte ao bate-papo. Como resultado, o aplicativo deve declarar os recursos de "InternetClientServer" e "PrivateNetworkClientServer" e a funcionalidade do dispositivo "Microfone" nas configurações de publicação encontrada nas configurações do player.

![Menu de recursos do Unity com "InternetClientServer", "PrivateNetworkClientServer" e o recurso de "Microfone" selecionado](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. Compile o projeto no Unity.

1. Ir para arquivo \| configurações de compilação, clique em **plataforma Universal do Windows** e verifique se você clicar em **alternar plataforma**

2. Clique em "Adicionar aberta nos bastidores" para adicionar a cena atual para a compilação

3. Na caixa de combinação do SDK, escolha "Universal 10"

4. Na caixa de combinação tipo de compilação da UWP, escolha "D3D", mas "XAML" também funcionará se você preferir.

5. Clique em "Build" para o Unity gerar o projeto do Visual Studio de UWP que encapsula o jogo Unity em um aplicativo UWP.

    Quando for solicitada uma localização, crie uma nova pasta para evitar confusão, pois muitos dos novos arquivos serão criados. É recomendável chamar a pasta "Build" e, em seguida, selecione a pasta.

6. Adicionar manifesto de rede do XIM ao seu projeto

    Adicione o arquivo networkmanifest.xml:

    ![Propriedades de networkmanifest.xml do Visual Studio](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    Ver [configuração do projeto XIM](xim-manifest.md) para obter mais informações sobre o manifesto de rede e seu conteúdo.

7. Compilar e executar o aplicativo UWP do Visual Studio

Isso iniciará o aplicativo como um aplicativo UWP normal e permitir chamadas Xbox Live operar, pois elas exigem um contêiner de aplicativo UWP à função.

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. Recompile se você fizer alterações a qualquer coisa no Unity

Se você alterar qualquer coisa no Unity, em seguida, você deve recompilar o projeto UWP
