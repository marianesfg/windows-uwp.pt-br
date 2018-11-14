---
author: Xansky
description: Saiba como atualizar o aplicativo para usar as bibliotecas do Microsoft Advertising compatíveis mais recentes e verifique se o aplicativo continua recebendo anúncios em faixa.
title: Atualizar seu aplicativo para as bibliotecas mais recentes de anúncios em faixa
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, AdControl, AdMediatorControl, migração
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: 48fc818c1ae7a9a46912ff52c2b00b07f19c366e
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6448612"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>Atualizar seu aplicativo para as bibliotecas mais recentes de anúncios em faixa

A partir de 1 de abril de 2017, não veicularemos mais os anúncios em faixa para aplicativos que usam uma versão do SDK de publicidade sem suporte. Se você usar o **AdControl** para exibir anúncios em faixa em aplicativo UWP (Plataforma Universal do Windows), use as informações neste artigo para determinar se você está usando um SDK de publicidade sem suporte e migrar o aplicativo para um SDK com suporte.

## <a name="overview"></a>Visão geral

Os aplicativos UWP que mostram anúncios em faixa devem usar o **AdControl** de bibliotecas de publicidade distribuídas no [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp). Este SDK oferece suporte a um conjunto mínimo de recursos, incluindo a capacidade de servir mídia avançada em HTML5 por meio da [especificação de Mobile Rich-media Ad Interface Definitions (MRAID) 1.0](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) do Interactive Advertising Bureau (IAB). Muitos dos anunciantes procuram esses recursos, e exigimos que os desenvolvedores de aplicativos usem uma dessas versões do SDK para ajudar a tornar o ecossistema de aplicativos mais atraente para anunciantes e, por fim, gerar mais receita para você.

Antes desse SDK ser lançado, nós fornecemos anteriormente a classe **AdControl** em várias versões antigas do SDK de publicidade. Essas versões mais antigas do SDK de publicidade não têm suporte, pois os recursos de publicidade mínimo descritos acima não têm suporte. A partir de 1 de abril de 2017, não veicularemos mais os anúncios em faixa para aplicativos que usam uma versão do SDK de publicidade sem suporte. Se você tiver um aplicativo que ainda usa uma versão do SDK de publicidade sem suporte, você observará o comportamento a seguir:

* Os anúncios em faixa não serão mais veiculados para qualquer **AdControl** no aplicativo, e você deixará de receber receita de publicidade desses controles.

* Quando o **AdControl** no aplicativo solicitar um novo anúncio, o evento **ErrorOccurred** do controle será acionado e a propriedade **ErrorCode** dos argumentos de evento terá o valor **NoAdAvailable**.

* Quaisquer unidades de publicidade associadas ao aplicativo serão desativadas. Você não pode remover essas unidades desativadas da sua conta do Centro de DePartnerv. Se você atualizar o aplicativo para usar um [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp), ignore essas unidades de anúncio e crie novas.

* Os anúncios em faixa também não serão mais disponibilizados para qualquer unidade de anúncio usada em mais de um aplicativo. Certifique-se de que suas unidades de anúncio serão usadas em um único aplicativo.

Se tiver um aplicativo (já na Microsoft Store ou ainda em desenvolvimento) com anúncios em faixa usando **AdControl** e você não tiver certeza sobre qual SDK de publicidade está sendo utilizado, siga as instruções neste artigo para determinar se é necessário atualizar o aplicativo para um SDK compatível. Se você tiver algum problema ou precisar de ajuda, [contate o suporte](http://go.microsoft.com/fwlink/?LinkId=393643).

> [!NOTE]
> Se o aplicativo já usa o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) (para aplicativos UWP), você não precisa fazer outras alterações nele.

## <a name="prerequisites"></a>Pré-requisitos

* O código-fonte completo e os arquivos de projeto do Visual Studio do aplicativo que usam **AdControl**.
* O pacote .appx do aplicativo.

> [!NOTE]
> Se não tiver mais o pacote .appx do aplicativo, mas ainda tiver um computador de desenvolvimento com a versão do Visual Studio e o SDK de publicidade usado para criar o aplicativo, você poderá gerar novamente o pacote .appx no Visual Studio.

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>Parte 1: determine se você precisa atualizar o aplicativo UWP

Siga as instruções nas seções a seguir para determinar se você precisa atualizar o aplicativo.

1. Crie uma cópia do pacote .appx do aplicativo de maneira que você não afete o original, renomeie a cópia de maneira que ela tenha uma extensão .zip e extraia o conteúdo do arquivo.

2. Verifique o conteúdo extraído do pacote do aplicativo:
  * Se vir um arquivo Microsoft.Advertising.dll, o aplicativo usará um SDK antigo e você deverá atualizar o projeto seguindo as instruções nas seções abaixo. Vá até [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).
  * Se não vir um arquivo Microsoft.Advertising.dll, o aplicativo UWP já usará o SDK de publicidade disponível mais recente e você não precisará fazer alterações no projeto.


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>Parte 2: Instale o SDK mais recente

Se o aplicativo usar uma versão do SDK anterior, siga estas instruções para se certificar de que você tenha o SDK mais recente no computador de desenvolvimento.

1. Verifique se o computador de desenvolvimento tem o Visual Studio 2015 ou uma versão mais recente instalada.
    > [!NOTE]
    > Se o Visual Studio estiver aberto no computador de desenvolvimento, feche-o antes de realizar as etapas a seguir.

1.  Desinstale todas as versões anteriores do SDK do Microsoft Advertising e do SDK do Ad Mediator do computador de desenvolvimento.

2.  Abra uma janela do **Prompt de Comando** e execute estes comandos para limpar todas as versões do SDK anteriores que possam ter sido instaladas com o Visual Studio, mas que talvez não sejam exibidas na lista de programas instalados no computador:
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Instale o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp).

## <a name="part-3-update-your-project"></a>Parte 3: Atualize o projeto

Remova todas as referências existentes às bibliotecas do Microsoft Advertising do projeto e siga [essas instruções](install-the-microsoft-advertising-libraries.md#reference) para adicionar as referências necessárias. Isso garantirá que o projeto use as bibliotecas corretas. É possível preservar a marcação e o código existentes.

## <a name="part-4-test-and-republish-your-app"></a>Parte 4: Teste e republique o aplicativo

Teste o aplicativo para garantir que ele exiba anúncios em faixa conforme o esperado.

Se a versão anterior do seu aplicativo já está disponível na loja, [Crie um novo envio](../publish/app-submissions.md) para o aplicativo atualizado no Partner Center republicar o aplicativo.
