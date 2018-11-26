---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empacotando apps
description: Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP).
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: 04736c9ac4de5adf162d32191ff30f7a981d6a6f
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7703050"
---
# <a name="packaging-apps"></a>Empacotando aplicativos


## <a name="purpose"></a>Finalidade

Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP).

| Tópico | Descrição |
|-------|-------------|
| [União de um aplicativo UWP com o Visual Studio](packaging-uwp-apps.md) | Para distribuir ou vender seu aplicativo UWP (Plataforma Universal do Windows), será necessário criar um pacote de apps para ele. |
| [Empacotamento manual de app](manual-packaging-root.md) | Se você quiser criar e assinar um pacote de app mas se não tiver usado o Visual Studio para desenvolver o app, precisará usar as ferramentas de empacotamento manual do app. |
| [Arquitetura do pacote de apps](device-architecture.md) | Saiba mais sobre quais arquiteturas de processador você deve usar ao criar seu pacote de aplicativos UWP. |
| [Instalação de streaming de aplicativo UWP](streaming-install.md) | A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano. |
| [Pacotes opcionais e conjunto de criação relacionado](optional-packages.md) | Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original. |
| [Pacotes opcionais com código executável](optional-packages-with-executable-code.md) | Saiba como usar o Visual Studio para criar um pacote opcional com código executável. |
| [Instalar aplicativos UWP com o Instalador de Aplicativo](appinstaller-root.md) | Instalador de Aplicativo permite que os aplicativos UWP sejam instalados clicando duas vezes no conjunto de aplicativo. |
| [Instalar aplicativos usando a ferramenta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode usar para implantar um aplicativo UWP em um computador com Windows 10 em qualquer dispositivo Windows 10 Mobile. Você pode usar essa ferramenta para implantar um pacote do aplicativo quando o dispositivo Windows 10 Mobile está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse aplicativo. Este artigo descreve como instalar aplicativos UWP usando essa ferramenta. |
| [Configurar compilações automáticas para seu aplicativo UWP](auto-build-package-uwp-apps.md) | Se você quiser empacotar seu aplicativo como parte de um processo de compilação automática, este tópico mostra como usar o Visual Studio Team Services (VSTS) para fazer isso. |
| [Declarações de funcionalidades do app](app-capability-declarations.md) | As funcionalidades devem ser declaradas no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211474) do aplicativo UWP para acessar determinada API ou recursos, como imagens, músicas ou dispositivos, como câmera ou microfone. |
| [Baixar e instalar atualizações de pacote a partir da Store](self-install-package-updates.md) | Seu aplicativo UWP pode verificar programaticamente se há atualizações de pacote e instalar as atualizações. Seu aplicativo também pode consultar os pacotes que foram marcados como obrigatórios no Partner Center e desabilitar a funcionalidade até que a atualização obrigatória seja instalada.  |
