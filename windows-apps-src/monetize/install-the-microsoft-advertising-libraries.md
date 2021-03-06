---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Saiba mais sobre como instalar as bibliotecas do SDK do Microsoft Advertising.
title: Instalar o SDK do Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, instalação, SDK, biblioteca do publicidade
ms.localizationpriority: medium
ms.openlocfilehash: 109ddbd3551dbd4304b86e56ace40f39e1b71211
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209632"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Instalar o SDK do Microsoft Advertising

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Para exibir anúncio em aplicativos UWP para Windows 10, instale o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Esse SDK é uma extensão do Visual Studio 2015 e versões posteriores.

> [!NOTE]
> Se você estiver desenvolvendo um aplicativo UWP JavaScript/HTML e tiver instalado o Windows 10 SDK versão 10.0.14393 (atualização de aniversário) ou posterior, você também deverá instalar a biblioteca [WinJS](https://github.com/winjs/winjs) . Essa biblioteca costumava ser incluída em versões anteriores do SDK do Windows 10, mas a partir da versão 10.0.14393 do SDK do Windows 10 (Atualização de Aniversário), ela deve ser instalada separadamente.

<span id="install-msi" />

## <a name="install-via-msi"></a>Instalar por meio de MSI

Para instalar o SDK do Microsoft Advertising por meio do instalador MSI:

1.  Feche todas as instâncias do Visual Studio.

2. Se você já instalou qualquer versão anterior do SDK do Microsoft Advertising, do SDK do Universal AD Client, da extensão Ad Mediator ou do SDK do Microsoft Store Engagement and Monetization, desinstale essas versões do SDK agora. Opcionalmente, abra uma janela do **Prompt de Comando** e execute esses comandos para limpar quaisquer versões mais antigas do SDK do Advertising que possam ter sido instaladas com o Visual Studio, mas que podem não aparecer na lista de programas instalados em seu computador:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Baixe e instale o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). A instalação pode demorar alguns minutos. Aguarde até o processo terminar.

4.  Reinicie o Visual Studio.

5.  Se você tiver um projeto existente que faça referência a bibliotecas de publicidade de qualquer versão anterior do SDK do Microsoft Advertising, do SDK do Universal Ad Client ou do SDK do Microsoft Store Engagement and Monetization, nós recomendamos que você abra seu projeto no Visual Studio e limpe e recrie seu projeto (em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do seu projeto e escolha **Limpar** e, em seguida, clique com o botão direito do mouse novamente no nó do seu projeto e escolha **Recriar**).

  Caso contrário, se você estiver usando o SDK do Microsoft Advertising pela primeira vez em seu projeto, você estará pronto para [adicionar uma referência ao SDK do Microsoft Advertising](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Instalar por meio do NuGet

Para instalar o SDK do Microsoft Advertising em um projeto UWP específico por meio do NuGet:

1.  Feche todas as instâncias do Visual Studio.

2.  Se você já instalou qualquer versão anterior do SDK do Microsoft Advertising, do SDK do Universal AD Client, da extensão Ad Mediator ou do SDK do Microsoft Store Engagement and Monetization, desinstale essas versões do SDK agora. Opcionalmente, abra uma janela do **Prompt de Comando** e execute esses comandos para limpar quaisquer versões mais antigas do SDK do Advertising que possam ter sido instaladas com o Visual Studio, mas que podem não aparecer na lista de programas instalados em seu computador:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Inicie o Visual Studio e abra o projeto no qual você deseja usar o SDK do Microsoft Advertising.
    > [!NOTE]
    > Se seu projeto já inclui referências de biblioteca de uma instalação MSI anterior do SDK, remova essas referências do seu projeto. Essas referências terão ícones de aviso ao lado delas porque as bibliotecas a que fazem referência foram removidas nas etapas anteriores.

4. No Visual Studio, clique em **Projeto** e **Gerenciar Pacotes NuGet**.

5. Na caixa de pesquisa, digite **Microsoft.Advertising.XAML** (para um projeto XAML) ou **Microsoft.Advertising.JS** (para um projeto JavaScript/HTML) e instale o pacote correspondente. Quando terminar a instalação do pacote, salve sua solução.
    > [!NOTE]
    > Se a janela **Saída** relatar um erro *Pacote-Instalação* que indica que o caminho especificado é muito longo, talvez seja necessário configurar o NuGet para extrair pacotes em um local alternativo com um caminho mais curto do que o local padrão. Para fazer isso, adicione o valor `repositoryPath` a um arquivo nuget.config em seu computador e o atribua a um caminho de pasta curto onde os pacotes NuGet possam ser extraídos. Para obter mais informações, consulte [este artigo](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) na documentação do NuGet. Você também pode tentar mover seu projeto do Visual Studio para uma pasta alternativa com um caminho mais curto.

6. Feche sua solução e depois a reabra.

7.  Se seu projeto já faz referência a bibliotecas de uma versão anterior do SDK do Microsoft Advertising que foi instalado por meio do NuGet e você atualizou seu projeto para uma versão mais recente do SDK, nós recomendamos que você limpe e recrie seu projeto (em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do seu projeto e escolha **Limpar** e, em seguida, clique com o botão direito do mouse novamente no nó do seu projeto e escolha **Recriar**).

  Caso contrário, se você estiver usando o SDK pela primeira vez em seu projeto, você estará pronto para [adicionar uma referência ao SDK do Microsoft Advertising](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Adicionar uma referência ao SDK do Microsoft Advertising

Depois de instalar o SDK do Microsoft Advertising, siga estas instruções para fazer referência o SDK em seu projeto para que você possa usar as APIs de publicidade.

1. Abra o projeto no Visual Studio.
    > [!NOTE]
    > Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência ao SDK do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

2. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

3. No **Gerenciador de referência**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado do **SDK do Microsoft Advertising para XAML** (para aplicativos XAML) ou **SDK do Microsoft Advertising para JavaScript** (para aplicativos criados em JavaScript e HTML).

4.  No **Gerenciador de Referências**, clique em OK.

Para orientações passo a passo que mostram como começar a usar as APIs de publicidade, consulte os artigos a seguir:

* [Anúncios intersticiais](interstitial-ads.md)
* [Anúncios nativos](native-ads.md)
* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [AdControl em HTML 5 e JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Noções básicas sobre pacotes de estrutura no SDK do Microsoft Advertising

A biblioteca Microsoft.Advertising.dll no [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (para aplicativos UWP) está configurada como um *pacote de estrutura*. Essa biblioteca contém as APIs de publicidade nos namespaces [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) e [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui).

Como essa biblioteca é um pacote de estrutura, isso significa que, após um usuário instalar uma versão do seu aplicativo que usa essa biblioteca, ela é atualizada automaticamente em seus dispositivos por meio do Windows Update sempre que publicarmos uma nova versão da biblioteca com correções e melhorias de desempenho. Isso ajuda a garantir que seus clientes sempre terão a versão mais recente da biblioteca instalada nos dispositivos deles.

Se nós lançamos uma nova versão do SDK que apresenta novas APIs ou recursos dessa biblioteca, você precisará instalar a versão mais recente do SDK para usar esses recursos. Nesse cenário, você também precisa publicar seu aplicativo atualizado na Loja.
