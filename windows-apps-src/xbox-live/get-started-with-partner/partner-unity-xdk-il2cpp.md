---
title: Unity para XDK com back-end IL2CPP
description: Adicionar suporte a Xbox Live para o Unity para XDK com IL2CPP script back-end para ID@Xbox e gerenciados de parceiros
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608461"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Adicionar suporte a Xbox Live para o Unity para XDK com IL2CPP script back-end para ID@Xbox e gerenciados de parceiros

## <a name="overview"></a>Visão geral

Suporte de tempo de execução do Windows para IL2CPP no Unity

Com o lançamento do Unity 5.6f3 o mecanismo incluiu um novo recurso que permite aos desenvolvedores usar componentes de tempo de execução do Windows (WinRT) diretamente no script, incluindo-os diretamente no projeto do jogo. Até que os 5.6 desenvolvedores precisou de um plug-in ou dll para oferecer suporte a qualquer recurso de plataforma (incluindo Xbox Live SDK) do script do jogo. Essa nova camada de projeção elimina a necessidade de plug-in e introduz um fluxo de trabalho novo e simplificado compatível somente com jogos que escolher IL2CPP script back-end.

Para obter mais informações sobre como começar, consulte a documentação do Unity: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Etapas

**1) instalar o Unity**

Instalar o Unity 5.6 ou posterior e verifique se que você tem a extensão de editor Xbox One instalada.

**2) instalar ferramentas do Visual Studio para Unity versão 3.1 e acima para o IntelliSense dá suporte ao usar WinMDs** para o Visual Studio 2015, isso pode ser encontrado em https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Para o Visual Studio 2017, é possível adicionar o componente dentro do instalador do Visual Studio 2017.

**3) abra um projeto novo ou existente do Unity**

**4) alternar a plataforma para Xbox One no menu de configurações de Build do Unity**

**5) habilitar IL2CPP script back-end nas configurações do player do Unity e definir a compatibilidade de API para o .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**6) opção o Script compilador Roslyn**

**7) as bibliotecas de sistema apropriado Xbox One serão todos ser adicionadas automaticamente ao seu projeto e não há etapas adicionais são necessárias para incluir os binários de plataforma.**

**8) adicionar e anexar um novo C\# script a um objeto do Unity.**

Por exemplo, clique em um objeto do Unity como a câmera"Main" e clique em "Adicionar componente" \| "Novo Script de" \| C\# Script \| e nomeie-a como "XboxLiveScript". Qualquer objeto do jogo fará.

**9) abra o script no Visual Studio (com o VSTU 3.1 + instalado)**

Você observará dois projetos, abra o seu script jogo XboxLiveTest.cs no projeto "Player" gerado pelo VSTU

![](../images/unity/unity-il2cpp-2.png)

Isso é um projeto especial gerado para o XDK e inclua referências para os arquivos de winmd que você tenha colocado no seus ativos.
Ele também será definir o "#if ENABLE_WINMD_SUPPORT" definir para você, portanto, IntelliSense e realce de sintaxe funcionará corretamente.

**10) adicione o seguinte código do Xbox Live para o arquivo de origem XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**11) Verifique se você tem a capacidade de 'InternetClient' selecionada nas configurações de publicação encontradas nas configurações do player**

![](../images/unity/unity-il2cpp-3.png)

**12) compila o projeto no Unity.**
